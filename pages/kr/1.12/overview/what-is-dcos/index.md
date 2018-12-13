---
layout: layout.pug
navigationTitle:  What is DC/OS
title: What is DC/OS?
menuWeight: 1
excerpt: Understanding DC/OS
enterprise: false
---

DC/OS는 자체 분산 시스템, 클러스터 관리, 컨테이너 플랫폼, 운영체제로서의 역할 수행 합니다.

## 분산 시스템

분산 시스템 으로써, DC/OS는 마스터 노드 그룹이 제어하는 Agent 노드 그룹을 가집니다. 다른 분산 시스템 처럼, 마스터 노드에서 돌아가는 몇몇 컴포넌트들은 피어와 함께 [leader election](https://en.wikipedia.org/wiki/Distributed_computing#Coordinator-election)를 수행 합니다.

## 클러스터 관리자

클러스터 관리자로써 DC/OS는 Agent 노드의 리소스와 작업을 관리합니다. Agent 노드는 클러스터에 리소스를 제공 합니다. 이 리소스들은 리소스 제안 (resource offer) 으로 묶여 스케쥴러들이 사용할수 있도록 설정 됩니다. 스케쥴러들은 이 제안을 수락하여 특정 작업에 리소스를 할당하고 특정 Agent 노드에 간접적인 방법으로 작업을 배치 합니다. 각 Agent 노드는 실행자 (executor) 를 불러내어 작업을 관리하도록 하고, 실행자들은 할당된 작업을 실행하고 관리합니다. 외부 클러스터 프로비저너와 다르게, DC/OS는 클러스터 내부에서 실행되고 실행시킨 작업의 생명주기를 관리합니다. 이러한 클러스터 관리기능은 [Apache Mesos](/kr/1.12/overview/concepts/#apache-mesos)로 부터 제공 됩니다.

## 컨테이너 플랫폼

컨테이너 플랫폼으로써 DC/OS는 두가지 작업 스케쥴러 (Marathon and DC/OS Jobs (Metronome))와 두가지 컨테이너 환경 (Docker and Mesos)를 제공합니다. 이러한 조합된 기능은 흔히 컨테이너 조율(container orchestration) 이라고 불립니다. DC/OS는 더 복잡한 어플리케이션에 특화된 스케쥴링 로직을 위한 사용자 지정 스케쥴러를 지원합니다. 데이터 베이스와 메세지 큐와 같은 상태를 가진 서비스들은 사용자 지정 스케쥴러를 통해 설정, 해체, 백업, 복원, 마이그레이션, 동기화, 재조정같은 작업을 수행가능합니다.

DC/OS 위의 모든 작업은 컨테이너화 됩니다. 컨테이너틑 [Docker Hub](https://hub.docker.com/)과 같은 컨테이너 저장소에서 내려받은 이미지로 시작되거나, 실행시 컨테이너화되는 네이티브 실행파일이 될수 있습니다. 지금은 모든 노드에 도커가 요구 되지만 이는 미래에 선택사항이 될것 입니다. 이는 업 컴포넌트, 패키지가 Mesos Universal Container Runtime 을 사용하기 위함입니다.

## 운영체제

운영체제로써 DC/OS는 클러스터의 하드웨어와 소프트웨어 리소스를 추상화 하여 어필리케이션에 일관된 서비스를 제공합니다. 패키지 관리, 네트워킹, 로깅, 저장장소, 계정 관리가 클러스터 관리와 컨테이너 조율기능에 걸쳐서 제공됩니다.

리눅스와 비슷하게, DC/OS는 시스템 공간과 유저 공간을 나누어 가집니다. 시스템 공간은 유저가 접근할수 없는 보호된 공간이며 리소스 할당, 보안, 프로세스 격리와 같은 저레벨 운영에 관여 합니다. 사용자 공간은 어플리케이션, 작업, 서비스 등이 있는 공간입니다. 기본 포함된 패키지 관리자를 사용하여 유저 공간에 서비스르 설치 할 수 있습니다.

리눅스와 다르게, DC/OS [host operating system](/kr/1.12/overview/concepts/#host-operating-system)이 아닙니다. DC/OS는 여러 장비에 걸치지만 각 장비가 가진 운영체제와 커널에 의존 합니다.
