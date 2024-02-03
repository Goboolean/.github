# About Our Project

Quant Trading 은 주식이나 선물 등의 상품 거래를 수학과 통계를 기반으로 접근하여 전략을 수립하고 투자하는 일련의 방법론을 의미합니다. 그러나 데이터를 받아오고 거래하는 인프라 구축, 분석을 위한 인공지능 지식 등은 퀀트 입문에 크나큰 문턱이며 늘 한계로 지적되어 왔습니다. Goboolean 은 이 문제를 엔지니어링을 통해 해결하여 사용자에게 최상의 퀀트 분석 경험을 제공하고자 합니다. 이 서비스를 통해 사용자는 많은 종류의 알고리즘을 선택하고 이를 튜닝하며, 주식 종목 등의 상품을 적용하여 분석하는 과정을 장난감을 조립하듯 할 수 있습니다.


# Full System

### Architecture Diagram
<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/full-system.png" alt="full system"> 
Goboolean 의 전체 시스템 다이어그램은 다음과 같습니다.
fetch system 은 구현이 완료되어 kubernetes 환경에 배포되었으며, 나머지 시스템도 박차를 가하고 있습니다.



# Fetch System

### Architecture Diagram
<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/fetch-system.png" alt="fetch system"> 
fetch system 의 아키텍처 다이어그램은 위와 같습니다.
외부 HTS로부터 다량의 거래 데이터를 받아와 Goboolean 의 다른 시스템에서 사용할 수 있도록 데이터를 적절하게 처리 및 저장 & 전송하는 기능을 담당하고 있습니다.
평균 초당 3000건, 최대 15000건의 데이터를 처리하고 있으며, 부하에 유연하게 확장될 수 있는 구조를 가지고 있습니다.


### Skill

**language**
| Go | Java |
| -- | ---- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/go.svg" alt="Go" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/java.svg" alt="Java" width="30px">

**database**
| MongoDB | etcd | InfluxDB | PostgreSQL |
| ------- | ---- | -------- | ---------- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/mongodb.svg" alt="mongodb" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/etcd.svg" alt="etcd" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/influxdb.svg" alt="influxdb" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/postgresql.svg" alt="postgresql" width="30px"> | 

**data streaming**
| kafka | kafka streams | kafka connect |
| ----- | ------------- | ------------- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/apachekafka.svg" alt="kafka" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/apachekafka.svg" alt="kafka" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/apachekafka.svg" alt="kafka" width="30px"> |


### Kafka Ecosystem
<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/kafka-ecosystem.png" alt="kafka ecosystem">
또한 **kafka ecosystem (kafka broker | kafka streams | kafka connect)** 를 통해 전체 파이프라인을 stateful 하게 관리하여 장애 및 부하 상황에도 유연하게 대처할 수 있도록 하고 있습니다.


# Core System

### Architecture Diagram
<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/core-system.png" alt="core system">
core system 의 아키텍처 다이어그램은 다음과 같습니다.
goboolean 시스템에서 사용자의 핵심 유즈케이스를 담당합니다.

### Skill

**language**
| Go | Java |
| -- | ---- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/go.svg" alt="Go" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/java.svg" alt="Java" width="30px">

**database**
| MongoDB | PostgreSQL |
| -- | ---- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/mongodb.svg" alt="MongoDB" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/postgresql.svg" alt="PostgreSQL" width="30px">

**communication**
| kafka | gRPC |
| ----- | ---- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/kafka.svg" alt="Go" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/grpc.svg" alt="Java" width="30px">


### MSA Pattern
core system 에는 Event Driven Architecture 이 적용되어 core system 내부의 컴포넌트간 통신은 물론 외부 시스템과의 통신을 이벤트 기반으로 하여 유연함을 확보하였습니다.
또한 도메인의 분해 과정에서 CQRS 를 적용하여 각자의 역할에 맞는 도메인 로직 및 모델을 설정하였습니다.


### Sequence Diagram
<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/core-system-sequence-diagram.png" alt="core system use case sequence diagram">
core system 의 core usecase 에 대한 sequence diagram 은 위와 같습니다.
하나의 유즈케이스는 core system 의 여러 컴포넌트의 상호작용을 통해 서비스되고 있습니다.


# ML System

### Architecture Diagram
<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/ml-system.png" alt="ml system">
ml system 의 다이어그램은 위와 같습니다.
kubeflow 환경에서 머신러닝과 서비스의 통합 솔루션을 제공합니다.
worker은 한 번의 실험에서 최대 90만개의 event 를 발행합니다.

### Sequence Diagram
<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/ml-system-sequence-diagram.png" alt="core system use case sequence diagram">