---
title:  "hikaricp 와 타임아웃"
date:   2023-06-14 13:00:00 +0900
categories: 
  - java
tags: 
  - HikariCP
  - cheet_sheet
---

### 참조
https://github.com/brettwooldridge/HikariCP

# 결론
HikariCP 에서 관리되는 타임아웃은 백엔드와 connection pool 간의 타임아웃 그리고 connection 이 pool 안에 있을 때의 타임아웃만을 관리하며, 대체로 connection 을 제공한 이후는 관여하지 않는다.

# ⏳ 관련옵션
Timeout 옵션과 관련된 옵션은 DB 와의 통신 중의 연결 시간으로 오해할 수 있지만, connection 을 로직에 제공한 이후의 처리에 관해서는 HikariCP 관여하지 않고, 그것은 timeout 도 마찬가지이다.

- ⏳connectionTimeout
    - default: 30000 (30sec)
    - `HikariDataSource#getConnection` 함수의 응답시간에 대한 타임아웃
- ⏳idleTimeout
    - default: 600000 (10min)
    - pool 에 보관중인 conneciton 이 `minimumIdle` 에 설정된 옵션보다 클 때, pool 에서 여분의 connection 을 보관하는 시간
- ⏳keepaliveTime
    - default: 0 (disabled)
    - pool 에 여분의 connection 에 keepalive 패킷 혹은 쿼리를 요청하는 주기
    - 설정시 기본적으로 jdbc 의 isValid 메소드를 이용하여 aliveness 확인
    - `🔤connectionTestQuery` 옵션을 설정했을 경우 해당 옵션에 지정한 쿼리를 이용하여 aliveness 를 판단함
- ⏳maxLifetime
    - default: 1800000 (30min)
    - pool 의 connection 은 해당 옵션에서 설정한 시간이 지나면 다른 새로운 connection 으로 대체됨
- ⏳initializationFailTimeout (`TODO`)
    - default: 1
    - pool 에서 connection 을 반환할 때 유효성을 확인한 후 반환하고, 유효성 확인에 실패하면 connection 을 획득하는 시점에 예외가 발생함.
    - default 에서 `connectTimeout(30000) + initializationFailTimeout(1) ms` 만큼 초기 연결을 시도한다
    - 해당 값이 0일 경우 유효성 확인이 실패했을 때 HikariCP 가 시작되지 않음
- ⏳validationTimeout
    - default: 5000 (5sec)
    - idle connection 의 aliveness 확인 요청의 timeout
- ⏳leakDetectionThreshold (`TODO`)
    - default: 0 (disabled)
    - Connection Leak 감지 시간
        - Connection Leak 이란 pool 에 커넥션의 반환을 하지 않을 때  
        - 해당 timeout 을 초과하면 connection 을 pool 에서 out 시킴
