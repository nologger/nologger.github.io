---
layout: post
title: "NoLogger-admin"
---

# NoLogger-admin

* * *

1. 문제정의
2. 요구사항
3. 구현 계획 수립
4. 소프트웨어 아키텍처 설계
5. 상세 설계
6. 코드 작성 및 디버깅
7. 단위 테스트
8. 통합 테스트
9. 통합
10. 시스템 테스트
11. 유지보수

* * *

## 1. 문제정의

> 문제정의란 해결책에 대해서는 언급하지 않고 단지 문제가 무엇인지만을 정의한다.

> 문제정의는 반드시 사용자 언어로 작성해야 한다.

- `nologger.github.io` 페이지의 메뉴항목을 수정하기 위해서는 프로젝트의 설정파일을 수정해야 한다. 이는 보기쉽게 관리하기 어렵다는 문제점이 있다.
- `nologger.github.io` 프로젝트 개발환경을 갖추지 않은 상태에서는 게시글을 올리기가 어렵다.

## 2. 요구사항

> 요구사항은 명시적으로 나타내어야 한다.

> 명시적인 요구사항은 개발자 대신 사용자가 시스템의 기능을 주도하게 하는 데 도움이 된다.

> 견고한 요구사항은 아키텍처부터 설계, 코드 작성, 테스트까지 순서에 따라 계획대로 진행할 수 있다.

> 요구사항은 프로젝트를 구현하는 동안에도 변경 될 수 있다.

- `admin.nologger.com`에서 `nologger.github.io` 페이지의 메뉴를 수정할 수 있어야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 변경내역을 확인 할 수 있어야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 게시글을 올릴 수 있어야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 게시글을 수정할 수 있어야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 게시글을 수정했을 때 게시글 내에 수정일을 남겨야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 게시글을 삭제할 수 있어야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 게시글 미리보기 기능을 지원해야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 세부 레이아웃(jekyll-layouts)를 수정할 수 있어야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 변경사항을 발생시켰을 때 `git`도 함께 동작해야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 변경사항을 발생시켰을 때 `git log`를 저장해야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 변경사항을 발생시켰을 때 `git status`를 나타낼 수 있어야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 변경사항을 발생시켰을 때 `jekyll build`를 실행해야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 변경사항을 발생시켰을 때 `jekyll log`를 저장해야 한다.
- `admin.nologger.com`에서 `nologger.github.io` 변경사항을 발생시켰을 때 `jekyll status`를 나타낼 수 있어야 한다.
- `admin.nologger.com`에 등록된 ip만이 접근할 수 있어야 한다.
- `admin.nologger.com`에 등록된 계정만이 접근할 수 있어야 한다.
- `admin.nologger.com`에 등록된 계정의 접속기록을 저장해야 한다.
- `admin.nologger.com`의 디자인(레이아웃)과 `nologger.github.io`의 디자인은 같아야 한다.

## 3. 구현 계획 수립

> 언어에 의한 프로그램을 작성하는 개발자는 자신의 사고를 프로그래밍 언어가 지원하는 기능으로 제한한다. 언어 도구가 원시적이면 개발자의 생각도 원시적일 것이다.

> 언어를 활용한 프로그램을 개발하는 개발자는 표현하고자 하는 생각이 무엇인지를 결정하고 나서 특정 언어가 제공하는 툴을 사용해 그러한 생각을 어덯게 표현할 것인지 결정한다.

- 설계 및 구현 진행절차 : 설계(100%) -> 구현(100%) : 프로젝트 계획부터 배포까지 절차를 학습을 목표에 두고 진행하는 프로젝트이기 때문에, 모든 설계를 마친 후 구현을 진행하도록 한다. 다만, 프로젝트 진행 중 설계가 변경될 여지는 존재한다.
- 코드 작성 규약 : 프로젝트 초안 구현 후 이펙티브 자바를 통해 리팩토링
- 오류 처리 방법 : RunTimeException을 확장한 공통 Exception을 이용하여 처리하도록 한다.
- 코드 성능 : 코드의 가독성을 해치지 않는 선에서 성능적인 부분에 노력해야 한다.
- 보안 : 프로젝트 초안 구현 후 스프링 시큐리티 인 액션 및 보안관련 서적을 통해 리팩토링 진행
- VCS : Git(GitHub)

## 4. 소프트웨어 아키텍처 설계

> "설계"란 단순히 상세 부분을 작성하기 전에 의사코드(pseudo code)로 클래스 인터페이스를 작성하는 것일 수도 있고, 코드로 구현하기 전에 몇몇 클래스 사이의 관계를 다이어그램으로 그리는 것일 수도 있고, 코드로 구현하기 전에 몇몇 클래스 사이의 관계를 다이어그램으로 그리는 것일 수도 있고, 다른 개발자에게 어떤 디자인 패턴이 더 좋은지 물어보는 것일 수도 있다.

> 설꼐는 요구사항을 코드 작성과 디버깅에 연결하는 작업이다. 훌륭한 상위 수준 설계는 여러 개의 하위 수준 설계를 무리 없이 담을 수 잇는 구조를 제공한다. 훌륭한 설계는 규모가 큰 프로젝트에서는 꼭 필요한 작업이며 작은 프로젝트에서도 유용하다.

> 복잡성 관리는 소프트웨어 개발에서 가장 중요한 기술적 주제다. 개인적으로 소프트웨어의 주요 기술적 의무는 복잡성을 관리하는 것이라고 생각한다. 에츠허르 데이크스트라(Edsger Dijksttra)는 컴퓨팅은 그저 1비트에서 수백 메가 비트 차이에 온통 집중해야 하는 작업이라고 지적했다. 이 비율은 정말 어마어마한 값이다.

> 의미적 층위 측면에서 보면 보통의 수학 공식은 거의 층위가 없다. 심오한 개념적인 계층 구조에 대한 필요로 인해 컴퓨터는 인류에게 유례 없는 새로운 본질적인 문제들을 제시했다.

> 소프트웨어에서 복잡성을 관리하는 것이 다른 기술적인 목표를 달성하는 것보다 중요하다.

### 4-1. 다음은 바람직한 설계의 특징을 나열한다.
- 복잡성 최소화
- 유지보수의 편리함
- 느슨한 결합
- 확장성
- 재사용성
- 높은 팬인(fan-in)
- 낮은 팬아웃(fan-out)
- 이식성
- 간결성
- 계층과
- 표준기법


### 4-2. 다음은 단계적 설계를 위한 단계를 나타낸다.
1. 소프트웨어 시스템 : 전체 시스템을 기준으로 시스템의 목적 및 방향성을 설정한다.
2. 서브시스템/패키지 분할 : 전체 시스템을 서브 시스템으로 나눈다.
3. 패키지내 클래스 분할 : 패키지내 상세 클래스 설정
4. 루틴 분할 : 클래스 내 간략적인 루틴 작성
5. 내부 루틴 설계 : 상세 코드 작성

## 5. 상세 설계

```
─ NoLogger-Admin ───┬ Admin ┬ MenuManagement ───┬ Create Menu ──┬ set menu id(auto)
                    │       │                   │               ├ set menu link
                    │       │                   │               ├ set menu name
                    │       │                   │               └ set menu order
                    │       │                   ├ Modify Menu ──┬ set menu id(auto)
                    │       │                   │               ├ set menu link
                    │       │                   │               ├ set menu name
                    │       │                   │               └ set menu order
                    │       │                   └ Delete Menu
                    │       │
                    │       ├ AdminManagement ──┬ Create Member ┬ set member id(auto)
                    │       │                   │               ├ set member account
                    │       │                   │               ├ set member password
                    │       │                   │               ├ set member name
                    │       │                   │               ├ set member email
                    │       │                   │               ├ set member phoneNumber
                    │       │                   │               ├ set member ip address1
                    │       │                   │               ├ set member ip address2
                    │       │                   │               └ set member ip address3
                    │       │                   ├ Modify Member ┬ update member id(auto)
                    │       │                   │               ├ update member account
                    │       │                   │               ├ update member password
                    │       │                   │               ├ update member name
                    │       │                   │               ├ update member email
                    │       │                   │               ├ update member phoneNumber
                    │       │                   │               ├ update member ip address1
                    │       │                   │               ├ update member ip address2
                    │       │                   │               └ update member ip address3
                    │       │                   └ Delete Member
                    │       │
                    │       └ Statistics
                    │
                    └ NoLogger ─┬ MenuManagement ───┬ Create Menu ──┬ set menu id(auto)
                                │                   │               ├ set menu link    
                                │                   │               ├ set menu name
                                │                   │               ├ set menu order
                                │                   │               ├ set parent id
                                │                   ├ Modify Menu ──┬ set menu id(auto)
                                │                   │               ├ set menu link
                                │                   │               ├ set menu name
                                │                   │               ├ set menu order
                                │                   │               └ set parent id
                                │                   └ Delete Menu
                                │
                                └ PostManagement ───┬ Create Post ──┬ set post id(auto)
                                                    │               ├ set post title
                                                    │               ├ set post content
                                                    │               ├ set post author
                                                    │               ├ set post isVisible
                                                    │               ├ set post Synchronize
                                                    │               ├ set post createdAt
                                                    │               ├ set post modifiedAt
                                                    │               ├ set post category(menu) id
                                                    └ View Post ────┬ update post id(auto)
                                                                    ├ update post title
                                                                    ├ update post content
                                                                    ├ update post author
                                                                    ├ update post isVisible
                                                                    ├ update post Synchronize
                                                                    ├ update post createdAt
                                                                    ├ update post modifiedAt
                                                                    ├ update post category(menu) id
                                                                    └ delete post

```

## 6. 코드 작성 및 디버깅

null

## 7. 단위 테스트

null

## 8. 통합 테스트

null

## 9. 통합

null

## 10. 시스템 테스트

- 전체 코드에 대한 단위 테스트 작성
- 통합 테스트 진행

## 11. 유지보수

- 리팩터링(마틴파울러) 참고 후 리팩터링 계획 수립
- 이펙티브 자바(조슈아 블로크) 참고 후 [3.구현계획수립]내 코드 작성 규약 재정리 및 리팩터링
- 스프링 시큐리티 인 액션(로렌티우 스필카) 참고 후 [3.구현계획수립]내 보안 재정리 및 리팩터링
- (구체적 선정 바람) 보안관련 서적 참고 후 [3.구현계획수립]내 보안 재정리 및 리팩터링
- (구체적 선정 바람) 테스트 관련 서적 참고 후 [3.구현계획수립]내 테스트 재정리 및 리팩터링
- 이펙티브 디버깅(디오미디스 스피넬리스) 참고 후 디버깅 [3.구현계획수립]내 디버깅 재정리 및 리팩터링