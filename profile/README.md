# About Our Project

Quant Trading 은 주식이나 선물 등의 상품 거래를 수학과 통계를 기반으로 접근하여 전략을 수립하고 투자하는 일련의 방법론을 의미합니다. 그러나 데이터를 받아오고 거래하는 인프라 구축, 분석을 위한 인공지능 지식 등은 퀀트 입문에 크나큰 문턱이며 늘 한계로 지적되어 왔습니다. Goboolean 은 이 문제를 엔지니어링을 통해 해결하여 사용자에게 최상의 퀀트 분석 경험을 제공하고자 합니다. 이 서비스를 통해 사용자는 많은 종류의 알고리즘을 선택하고 이를 튜닝하며, 주식 종목 등의 상품을 적용하여 분석하는 과정을 장난감을 조립하듯 할 수 있습니다.



# We Currently Use..

**Language**

| Go |
| -- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/go.svg" alt="Go" width="30px"> |


**Database**

| PostgreSQL | MongoDB | etcd | MiniO |
| -----------| ------- | ---- | ----- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/postgresql.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/mongodb.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/etcd.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/minio.svg" width="30px"> | 


**Data Stream**

| Kafka | gRPC | 
| ----- | ---- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/apachekafka.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/grpc.svg" width="30px"> |


**Cloud Engineering**

| Docker | Kubernetes | Helm |
| ------ | ---------- | ---  |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/docker.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/kubernetes.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/helm.svg" width="30px"> |


**Ops**

| GCP | Github Actions | Traefik |
| --- | -------------- | ------- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/googlecloud.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/githubactions.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/traefikproxy.svg" width="30px"> |


**Machine Learning**

| Pytorch | Kubeflow |
| ------- | -------- |
| <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/pytorch.svg" width="30px"> | <img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/icon/kubeflow.svg" width="30px"> |



# Our Architecture

Goboolean Server 은 **fetch-system.worker** **fetch-system.master** **core-system.command** **core-system.worker** **core-system.join** **manager-server** **identity-server** 이렇게 7개의 어플리케이션 컴포넌트와 **Kafka** **MongoDB** **PostgreSQL** 등의 인프라 이루어진 MSA 아키텍처를 이루고 있습니다.

아키텍처에 대한 자세한 설명은 [링크](https://github.com/Goboolean/.github/tree/main/docs/architecture/full-system) 를 참고해주세요


<img src="https://raw.githubusercontent.com/Goboolean/.github/main/asset/diagram/full-system.png" alt="full-diagram" > 


# Our Repositories

아래는 Goboolean 에서 현재 개발중인 리포지토리입니다.

| Repository Name   | Link | Level |
| ------------------- | --- | ----- |
| fetch-system.worker | <a href="https://github.com/Goboolean/fetch-system.worker"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" height="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
| fetch-system.master | <a href="https://github.com/Goboolean/fetch-system.master"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
| fetch-system.infrastructure | <a href="https://github.com/Goboolean/fetch-system.infrastructure"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
| fetch-server.v1 | <a href="https://github.com/Goboolean/fetch-server.v1"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/5875/5875988.png" alt="" width="20"/> |
| core-system.command | <a href="https://github.com/Goboolean/core-system.command"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
| core-system.worker  | <a href="https://github.com/Goboolean/core-system.worker"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
| core-system.join    | <a href="https://github.com/Goboolean/core-system.worker"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
| manager-server  | <a href="https://github.com/Goboolean/manager-server"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/5875/5875988.png" alt="" width="20"/> |
| identity-server | <a href="https://github.com/Goboolean/identity-server"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/1951/1951379.png" alt="" width="20"/> |
| client | <a href="https://github.com/Goboolean/client"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/1951/1951379.png" alt="" width="20"/> |
| common | <a href="https://github.com/Goboolean/common"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/5875/5875988.png" alt="" width="20"/> |
| ops    | <a href="https://github.com/Goboolean/ops"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
| MLRefactorized | <a href="https://github.com/Goboolean/MLRefactorized"> <img src="https://cdn-icons-png.flaticon.com/512/733/733609.png" alt="깃헙 링크" width="20"/> </a> | <img src="https://cdn-icons-png.flaticon.com/512/4321/4321473.png" alt="" width="20"/> |
