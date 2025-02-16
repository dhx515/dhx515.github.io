# 01. git branch 전략

작성일: 241228  
작성자: 송민수  
참여자: 김동현, 이경훈, 우창석

## 00. Git이란?

Git은 분산 버전 관리 시스템(Distributed Version Control System)으로, 소스 코드나 파일의 변경 이력을 기록하고 관리할 수 있는 도구를 의미.

이를 통해 개빌자들은 프로젝트의 파일과 코드를 효율적으로 관리하고 협업할 수 있음.

**주요 특징**

- 버전 관리
  - 파일의 변경 이력을 저장하고, 특정 시점으로 되돌리거나 비교할 수 있음
  - 각 변경 사항은 고유한 커밋 ID로 식별됨
- 브랜치와 병합
  - 브랜치를 사용하여 독립적인 작업 공간을 만들고, 나중에 병합(Merge)할 수 있음
  - 이로 인해 여러 사람이 동시에 작업하기 용이함

<br/>

## 01. Git Branch 전략

Git의 버전 관리 및 브랜치와 병합 특징을 이용하여, 여러 개발자가 하나의 저장소에 작업을 할 때, 협업을 효과적으로 하기 위해 git branch에 대한 규칙을 정하고 저장소를 잘 활용하기 위한 workflow를 정의하는 것을 **git branch 전략**이라고 한다.

**git branch 전략 사용 시 이점**

- 개발 중인 기능이나 수정사항이 개발자 독립적으로 진행됨
- 각각의 브랜치가 특정 기능, 이슈에 대응하여 특정 작업을 수행하고, 필요한 경우 작업 단위의 Rollback이 가능하여 프로젝트 관리의 유연성을 향상시켜줌
- 각각의 태그는 Release를 원하는 버전 단위로 관리할 수 있도록 하여, 배포 안정성을 향상시켜줌

**대표적인 git branch**

- Git Flow
- GitHub Flow

<br/>

## 02. Git Flow


![image.png](../_images/01%20git%20branch%20전략/image.png)

**Branch 종류**

- `master/main` : 제품 출시 버전을 관리하는 메인 브랜치(태그 사용)
- `develop/development` : 다음 출시 버전을 위해 개발하는 브랜치
- `feature` : 새로운 기능을 개발하는 브랜치
- `release` : 다음 출시 버전을 준비하는 브랜치
- `hotfix` : 출시된 제품의 버그를 고치기 위한 브랜치

**Branch 활용 시나리오**

1. 신규 기능을 개발하기 위해서 개발자는 `develop/development` 브랜치에서 한 `feature` 브랜치를 따서 작업을 진행
2. 기능 개발이 완료되면, `feature` 브랜치를 `develop/development` 브랜치로 병합(Merge)

   이때, 작업 내용을 Review 하는 Pull Request(PR) 작업이 완료된 후 병합됨.

3. 다음 출시 버전을 위해 개발 중인 `develop/development` 브랜치에서 `release` 브랜치를 따서 배포를 위한 준비 진행

   이때 발견되는 버그들(ex. 기능 추가 후 관통 테스트 오류, 배포 단계에서의 오류 등)은 `release` 브랜치에서 바로 반영

4. 충분한 테스트 후, 일정한 주기로(혹은 배포하고자 하는 버전 단위로) `master/main` 브랜치로 Merge 하여 제품을 출시
5. 상용 배포 이후, `release` 브랜치에서 미처 발견되지 못한 새로운 버그들은 `hotfix` 브랜치에 바로 반영

<br/>

## 03. GitHub Flow


![image.png](../_images/01%20git%20branch%20전략/image%201.png)

GitHub Flow는 브랜치를 하나의 `base 브랜치(master/main)` + `base에 기능을 추가하기 위한 브랜치(feature)` 두 개만으로 운영하여 간단하지만 빠르게 수정 배포할 수 있는 전략

Branch 활용 시나리오

1. `master/main` 브랜치는 배포를 위한 소스코드를 관리하는 브랜치. 새로운 기능 개발이 필요할 때, `feature` 브랜치를 따서 작업 진행.
2. 기능 개발이 완료되면, PR(Pull Request)를 통해 Review 수행.
3. Review가 완료되고, 피드백이 모두 반영되면 해당 `feature` 브랜치를 `master/main` 브랜치로 병합.

GitHub Flow는 단순하며 지속적인 배포를 강조하며, **master 브랜치에서 배포를 수행**함.

<br/>

## 04. 상황에 따른 Git Flow와 GitHub Flow 사용법


### Git Flow

더 많은 제어와 복잡성을 가지고 있어 특정 기능이나 수정을 빠르게 배포해야 할 경우에 유연성이 다소 떨어짐

그러나, 그만큼 배포 안정성과 버전 관리 및 롤백 등 체계적인 운영이 가능함

### GitHub Flow

테스트와 검증 절차를 거치지 않고 바로 `master/main` 브랜치로 병합되므로 위험성을 가지고 있음

하지만 그만큼 단순하고 빠르게 기능을 테스트하고 Agile하게 배포할 수 있기 때문에 주로 각 환경의 구분이 명확하지 않고 작은 규모의 프로젝트에 적합함

### Git Flow vs GitHub Flow

|           | Git Flow                                                  | GitHub Flow                                                           |
| --------- | --------------------------------------------------------- | --------------------------------------------------------------------- |
| 브랜치 수 | 다양한 종류의 브랜치 사용                                 | 단일 브랜치(`master/main`) 사용                                       |
| 배포 방식 | `release`, `hotfix` 브랜치를 통해 명확한 배포 절차를 가짐 | 단순하고 지속적인 배포를 강조하며, `master/main` 브랜치에서 배포 수행 |
| 복잡성    | 복잡한 프로젝트나 대규모 팀에서 사용하기 좋음             | 단순하며 빠른 개발 및 배포를 위해 사용                                |

<br/>

## 05. 회사(제조업)에 맞는 Git Branch 전략?

개발을 하는 인원들 중 대부분이 Git을 사용하지 못했을 가능성이 높으므로,

**GitHub Flow로 시작하여 Git을 활용합 협업에 익숙해지는 것이 중요.**

이후 Git사용이 익숙해 진다면, **Git Flow를 사용하여 체계적으로 브랜치 및 서비스의 버전관리를 진행하는 것이 좋아 보인다.**

다만, MSA로 전환되는 과정에서 Git Flow를 설정하는 것은 서비스를 운영하기 위한 환경설정에서 많은 시간을 소비할 수 있으므로,

main, development, feature, staging(release) 브랜치만을 사용하여 빠르게 개발 및 배포를 진행,

이후 버그 발생 시 hotfix 브랜치를 따서 수정하는 방법을 진행하는 것이 바람직해 보임.

## 참고자료

[Github flow 사용법 & 시나리오](https://velog.io/@taeate/Github-flow-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4)

[[Git] 깃 합치다 충돌났을 때 / 컨플릭트 (conflict)](https://devyihyun.tistory.com/39)
