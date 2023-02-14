---
layout: post
title: "NoLogger-admin"
---

{% raw %}
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

null

## 4. 소프트웨어 아키텍처 설계

null

## 5. 상세 설계

null

## 6. 코드 작성 및 디버깅

null

## 7. 단위 테스트

null

## 8. 통합 테스트

null

## 9. 통합

null

## 10. 시스템 테스트

null

## 11. 유지보수

null

{% endraw %}