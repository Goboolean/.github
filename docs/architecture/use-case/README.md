


# Service Policy



### Introduction

Goboolean 은 가능한 많은 옵션들을 사용자가 레고를 조립하는 것처럼 쉽게 선택하여 분석을 진행할 수 있는 서비스를 제공하는 것을 목표로 합니다. 반면에 실제 퀀트는 다양한 종류의 함수들과 머신러닝 모델이 사용되는 전문가의 영역입니다. 이 사이의 간극을 엔지니어링을 통해 해결한다는 것은 말처럼 쉽지는 않았습니다. 사용자에게 일관된 형태의 옵션을 제공해야하나, 실제 모델이 동작하는 형태는 정말 다양합니다. 프로젝트 메니저가 여기까지 전부 이해하고 서비스를 정리하기에는 범위가 너무 많습니다. Goboolean 전체의 아키텍처와 머신러닝 엔지니어의 개발 방식 그리고 도메인 지식이 서로 잘 어우러지는 정책과 시스템 구조를 결정하는 문제였습니다.

저희는 분할 정복으로 이 문제를 접근하였습니다. 사용자 앤드포인트 요구사항과, 실제 머신러닝 모델 사이를 여러 계층으로 구분하고, 전체 문제를 각 계층에서 해결해야 하는 문제로 쪼개었습니다. 전체 서비스는 여러 계층으로 나눠지고, 상위 계층으로 갈수록 추상적인 수준의, 하위 계층으로 갈수록 구체적인 수준의 관심을 갖도록 하였습니다.

1. f' : (product_id, model_id, …strategy_parameter) ⇒ yield rate
2. f : (product_id, model_id, …hyper_parameter, …strategy_parameter) ⇒ yield rate
3. g : (current data, conditions) ⇒ (timestamp, price, trade)
4. h : (current data, conditions) ⇒ model specific result


f' 계층은 Client 의 계층의 관심을 나타냅니다. 사용자는 상품의 종류, 사용할 모델, 사용할 차트의 주기 및 전략들을 입력하면 해당 구성의 효율을 알 수 있습니다.

f 계층은 f' 계층과 유사하지만, 하나의 분석이 실행되는 라이프사이클의 관심을 나타냅니다. 아키텍처에서는 core-system.worker 이 해당 역할을 담당합니다. worker 은 사용자가 입력한 strategy parameter 외에도 hyper parameter 을 요구합니다. worker 의 라이프사이클은 위 함수의 라이프사이클과 같습니다. 어떤 worker 이 띄워질 때 작업을 할당받아 실행하고, 작업이 끝나면 worker 은 종료됩니다. 다만 실제 yield rate 를 계산하는 역할은 worker 의 소관이 아닙니다. 또한 strategy_parameter 의 종류는 모델에 의존적이나, f 수준에서는 파라미터의 내용에 관심을 갖지 않아도 됩니다.

g 계층은 분석 과정에서 발생하는 거래 이벤트의 관심을 나타냅니다. worker 은 매번 갱신된 데이터를 읽고 조건을 만족하는 경우 거래 이벤트를 생성합니다. g 함수의 결과를 모두 모으면 f 의 관심사인 yield rate 를 얻을 수 있습니다. core-system.command, core-system.join 등 다른 core system 에서 필요로 하는 것이 이 이벤트입니다.

h 계층은 거래 이벤트 발생에 필요한 기초 자료를 나타냅니다. g 수준과는 다르게 output 과 output 을 처리하는 method 가 model 에 의존적입니다. 즉슨 h 수준의 output 은 정수 하나에 해당하는 기술 지표인 경우도 있고, 미래의 예측값, 예측확률분포 등 다양한 형태로 존재합니다. worker 은 model 을 나타내는 소스코드를 실행하여 결과를 얻는데, 이를 g 수준으로 끌어올리기 위해서 머신러닝 엔지니어라 model 을 개발함과 함께 method 도 개발하여야 합니다.



이렇게 계층을 분리하면 각자의 역할이 명확해지며 협업이 쉬워집니다.

1. product manager 은 f 수준에만 관심을 가질 수 있습니다. 모델의 간단한 동작 원리와 사용자가 조절할 수 있는 값을 머신러닝 개발자에게 제안하고, 검증을 받을 수 있습니다.
2. machine learning engineer 은 g 계층의 관심을 표현하는 모델 소스코드를 작성하는게 기본적인 역할입니다. 또한 동시에 이를 h 수준으로 끌어올리는 메서드를 작성하는 데에도 책임이 있습니다.
3. server engineer 은 모델의 종류와 무관하게 g 함수 추상화 수준에서의 이벤트에만 집중하고 개발할 수 있습니다.



# Use Case


### Data Management Use Case

데이터는 아래와 같은 단계들을 거쳐 서비스에서 활용됩니다.

data prepare
1. manager server 에서 Goboolean 에서 서비스할 상품을 정의하고 데이터베이스에 저장합니다.
2. fetch-system.master 은 최초로 실행될때, 그리고 주기적으로 데이터베이스를 읽습니다.

data fetch
1. fetch-system.worker 은 kis, polygon 등 적절한 외부 서버로부터 데이터를 구독하여 raw data 를 kafka broker 로 전송합니다.
2. fetch-system 의 kafka streams 에서 kafka broker 에서 raw data 를 처리하여 batch data 를 생성하여 kafka broker 로 전송합니다. 이 과정을 중첩하여 진행하여 1s 5s 1m 5m 종류의 데이터를 얻습니다. 
3. fetch-system 의 kafka connect 는 kafka broker 의 데이터를 읽어 mongodb 에 저장합니다. 
4. 외부 시스템은 fetch-system 의 kafka 에서 실시간 데이터를, mongodb 에서 과거 데이터를 얻을 수 있습니다.

data store
1. manager server 에서 저징된 데이터를 관리합니다.
2. 주기적으로 데이터를 백업하여 dump 를 google cloud service 에 저장합니다.
3. fetch system 이외의 경로로 한꺼번에 필요한 데이터를 받을 수 있는 경우 이를 관리합니다.



### Past Analysis Use Case

과거 분석 유즈케이스는 pre simulation phase, hyper parameter tuning phase, simulation phase, post simulation phase 로 4개의 phase 로 구성되어 있습니다. phase 는 작업의 단계를 의미하여 각 phase 는 순차적으로 진행됩니다.

1. pre simulation phase
	1. client 는 core-system.command 에 실험 요청을 보내면, command 는 task 생성 이벤트를 발행합니다.
	2. client 가 join 으로부터 계속해서 결과를 구독할 수 있도록 task id 를 사용자에게 제공하고 데이터베이스에도 저장합니다.
	3. 실험 요청은 스케줄링됩니다. 서버에 여유 core-system.worker 이 없다면 요청은 대기되고, 새롭게 worker 이 생성되어 task 를 가져가면 요청은 처리됩니다.
	4. worker 이 정상적으로 task 를 할당받아 성공적으로 작업을 시작하면, 작업 시작 이벤트를 발행하여 command 에 알립니다.
	5. command 는 client 에게 실시간으로 순번의 변화를 알리며, 작업이 시작될경우 연결을 종료합니다.

2. hyper parameter tuning phase
	1. command 는 사용자가 요청한 설정을 확인하고, worker 이 필요로 하는 hyper parameter 이 있는지를 확인합니다. 만약 없다면 ml-system 에 요청합니다.
	2. ml-system.optimizer 은 hyper parameter tuning 을 거쳐 최적의 hyper parameter 을 얻습니다. 계산을 완료하여 hyper-parameter tuning 을 통해 파라미터를 얻으면 이에 대한 이벤트를 발행합니다.
	3. command 는 해당 이벤트를 구독하고 데이터베이스에 저장합니다.
	4. 전체 과정이 끝나면 worker scheduling 을 진행합니다.
	5. 하이퍼 파라미터 튜닝 과정은 어떤 configuration 에 대해 한 번만 계산해두면 이후에는 이 값을 미리 사용할 수 있습니다.

3. simulation phase
	1. worker 은 분석을 진행하며 실시간으로 결과 이벤트를 발행합니다.
	2. join 서버는 해당 이벤트를 구독하여, fetch-system 으로부터 과거 거래 데이터와 조인하여 client 에 제공합니다.
	3. 일반적으로 분석 결과 이벤트 발행이 훨씬 느리기 때문에, 이에 싱크를 맞추어 필요한 만큼만 client 에 제공합니다.
	4. client 는 재접속 하더라도 join 서버로부터 그동안 진행된 실험 내용을 다시 받을 수 있습니다.

4. post simulation phase
	1. worker 은 실험을 마친 이후 실험 종료 이벤트를 발행합니다.
	2. command 은 지금까지 발행한 이벤트를 모두 종합하여 수익률을 계산하고 최종 결과를 포함하여 메타데이터를 정리합니다.
	3. 이후에 사용자가 동일한 실험을 요청하는 경우, 모든 단계를 건너뛰고 join 서버에서 과거 거래 데이터와 실험 결과를 데이터베이스에서 불러와 조인하여 빠르게 사용자에게 제공합니다.



### Realtime Trade Use Case

실시간 거래 유즈케이스는 execution flow, process flow, trade flow 로 3개의 flow 로 구성되어 있습니다. flow 는 이벤트의 흐름에서 어떤 과정을 의미하며 논리적 순서를 나타냅니다.

1. execution flow
	1. client 과거 분석이 완료된 configuration set 으로 core-system.command 에 실험 요청을 보내면, command 는 task 생성 이벤트를 발행합니다.
	2. 실시간 분석의 경우에는 과거 분석과 달리 worker 에 부하가 크지 않기 때문에 항상 worker 을 할당받을 수 있도록 관리합니다. 따라서 스케줄링을 하지 않습니다.
	3. 실시간 거래 유즈케이스의 실행은 과거 분석이 완료된 경우에만 할 수 있기 때문에 hyper parameter tuning phase 가 필요하지 않습니다.
	4. worker 이 정상적으로 task 를 할당받아 성공적으로 작업을 시작하면, 작업 시작 이벤트를 발행하여 command 에 알립니다.

2. process flow
	1. fetch-system 은 디폴트로 Goboolean 이 서비스하는 모든 거래 데이터를 구독하고 있습니다. core-system.worker 은 fetch-system 으로부터 필요한 상품 데이터를 구독합니다.
	2. worker 은 model 로부터 model-specific result 를 받고, 이를 해석하여 거래 형태의 타입으로 변환하여 이벤트를 발행합니다.
	3. core-system.join 은 worker 이 발행하는 이벤트를 실시간 거래 데이터와 조인하여 사용자에게 제공합니다.
	4. core-system.command 는 worker 이 발행하는 이벤트를 구독하여 거래 관련 작업을 처리합니다.
	5. 해당 과정은 사용자가 종료를 원할 때까지 계속해서 진행됩니다.

3. trade flow
	1. 편의를 위해 사용자가 자동으로 거래가 진행되는 것을 원할 제공할 경우 관련 서비스를 제공합니다. command 에서 kis, polygon, buycycle 등 사용자가 사용하는 플랫폼에서 제공하는 api 를 호출합니다.
	2. 보안 문제를 이유로 사용자가 서버에서 거래가 진행되는 것을 원치 않을 경우 관련 서비스를 제공합니다. command 에서 api 서버를 열고, 자동 거래 시스템을 구현한 사용자가 이를 호출할 수 있도록 합니다.
	3. 이벤트에 대한 알림만 받고자 하는 경우 관련 서비스를 제공합니다. command 에서 FCM 서비스를 호출합니다.
