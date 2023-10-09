

# Introduction

Goboolean 서비스의 구조는 많은 일반적인 웹서비스와는 다소 다른 양상을 띄고 있습니다. 일반적인 서비스에서 유즈케이스는 사용자가 서버에 요청을 보내면 데이터베이스에 쿼리하여 적절한 결과를 응답하여 마치는 형태를 띕니다. Goboolean 에서는 서비스의 특성상 하나의 유즈케이스 호흡이 상당히 길고, 데이터 및 이벤트의 파이프라인이 여러 인스턴스에 걸쳐서 존재합니다. 설계를 통해 높은 서비스 복잡도를 어떻게 해소하였는지, 그리고 어떤 방식으로 서비스를 개선 하였는지를 소개합니다!


# Use Case

Goboolean 서비스의 메인 유즈케이스는 아래와 같습니다.

core system 의 주요 서비스는 이렇게 두 가지입니다.
1. 과거 상품 분석
2. 실시간 상품 거래

사용자가 선택한 상품, 모델, 사용자 전략, 기타 파라미터의 조합을 입력하면, 과거 테스트를 진행합니다.
이후 사용자는 검증이 된 설정의 조합을 저장하여, 이를 실시간 거래에 사용할 수 있습니다.


fetch system 의 주요 서비스는 이렇게 두 가지입니다.
1. 상품 거래 데이터 구독
2. 저장된 상품을 관리

여러 플랫폼에서 데이터를 수집하고 처리하여 다른 서비스에 실시간으로 데이터를 제공합니다.
이후 데이터는 데이터베이스에 저장되어 관리자 시스템에서 관리하고, 다른 서비스에 과거의 데이터를 제공합니다.


identity server 은 계정, 인증 등의 정보를 다룹니다.


결과적으로 각각의 시스템이 모두 유기적으로 상호작용하며 전체 서비스를 제공하게 됩니다.


# Full Diagram

아래는 전체 플로우를 간략하게 나타낸 다이어그램입니다. 계속해서 아키텍처의 구조를 수정하고 개선하여 최종적으로 현재의 상태에 이르게 되었습니다.


<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/full-system.png" alt="full-diagram" > 




아래는 각각 fetch system 과 core system 의 동작을 아키텍처 수준에서 조금 더 구체적으로 표현한 다이어그램입니다.

<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/fetch-system.png" alt="fetch-system" > 

<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/core-system.png" alt="core-system" > 