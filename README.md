# Explore-MQTT

## MQTT (Message Queuing Telemetry Transport) 프로토콜:

1. 정의:
   MQTT는 경량의 발행-구독(publish-subscribe) 기반 메시징 프로토콜입니다. IoT(사물인터넷) 환경에서 널리 사용되며, 제한된 네트워크 대역폭과 리소스에서도 효율적으로 동작합니다.

2. 주요 특징:
   - 경량화: 헤더가 작아 네트워크 부하가 적습니다.
   - 양방향 통신: 클라이언트와 서버 간 실시간 메시지 교환이 가능합니다.
   - QoS (Quality of Service): 메시지 전달 보장 수준을 3단계로 제공합니다.
   - 지속 세션: 클라이언트 연결이 끊어져도 메시지를 저장하고 재연결 시 전달합니다.

3. 주요 구성 요소:
   - 브로커(Broker): 메시지를 중계하는 서버 역할을 합니다.
   - 발행자(Publisher): 특정 주제(topic)로 메시지를 보내는 클라이언트입니다.
   - 구독자(Subscriber): 특정 주제의 메시지를 받는 클라이언트입니다.

4. 작동 원리:
   - 발행자가 특정 주제로 메시지를 발행합니다.
   - 브로커가 해당 메시지를 받아 저장합니다.
   - 구독자가 관심 있는 주제를 구독합니다.
   - 브로커는 구독자에게 해당 주제의 메시지를 전달합니다.

공유 자전거 플랫폼에서의 MQTT 활용:

1. 자전거 상태 보고:
   - 각 자전거가 발행자 역할을 합니다.
   - 주기적으로 위치, 배터리 상태, 잠금 상태 등을 발행합니다.
   - 주제 예: "bikes/{bike_id}/status"

2. 서버의 자전거 모니터링:
   - 서버가 모든 자전거의 상태 주제를 구독합니다.
   - 실시간으로 자전거 fleet의 상태를 파악할 수 있습니다.

3. 자전거 제어:
   - 서버가 특정 자전거에 명령을 발행합니다.
   - 주제 예: "bikes/{bike_id}/command"
   - 명령 예: 잠금/해제, 경보 울리기 등

4. 시스템 알림:
   - 서버가 전체 시스템 상태나 긴급 알림을 발행합니다.
   - 주제 예: "system/alerts"

5. 유지보수 알림:
   - 자전거가 문제 상황을 감지하면 유지보수 주제로 발행합니다.
   - 주제 예: "maintenance/{bike_id}"

7. 사용 통계:
   - 자전거가 주행 시작/종료 시 통계 데이터를 발행합니다.
   - 주제 예: "usage/stats/{bike_id}"

``` mermaid
graph TD
    B1[Bike 1] -->|Publish Status| MB{MQTT Broker}
    B2[Bike 2] -->|Publish Status| MB
    B3[Bike 3] -->|Publish Status| MB
    MB -->|Subscribe| S[Server]
    S -->|Publish Commands| MB
    MB -->|Commands| B1
    MB -->|Commands| B2
    MB -->|Commands| B3
    
    S <-->|REST API| MA[Mobile App]
    
    subgraph Bikes
        B1
        B2
        B3
    end
    
    subgraph Broker
        MB
    end
    
    subgraph Backend
        S
    end
    
    subgraph Client
        MA
    end
    
    classDef bike fill:#a0d0ff,stroke:#333,stroke-width:2px;
    classDef broker fill:#f0f0f0,stroke:#333,stroke-width:2px;
    classDef server fill:#ffd700,stroke:#333,stroke-width:2px;
    classDef app fill:#90ee90,stroke:#333,stroke-width:2px;
    
    class B1,B2,B3 bike;
    class MB broker;
    class S server;
    class MA app;

    %% REST API Connection
    linkStyle 8 stroke:#ff6347,stroke-width:2px,stroke-dasharray: 5 5;
    %% Command flows
    linkStyle 4,5,6,7 stroke:#32CD32,stroke-width:2px;
```

이 구조의 장점:
1. 실시간성: MQTT의 발행-구독 모델을 통해 모든 구성 요소가 실시간으로 정보를 주고받을 수 있습니다.
2. 확장성: 새로운 자전거나 클라이언트를 쉽게 추가할 수 있습니다.
3. 효율성: MQTT의 경량 특성으로 네트워크 리소스를 효율적으로 사용합니다.
4. 신뢰성: MQTT의 QoS 기능을 통해 중요한 메시지의 전달을 보장할 수 있습니다.
