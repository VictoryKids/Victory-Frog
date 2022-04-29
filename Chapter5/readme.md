# 객체지향 설계 5원칙 - SOLID
- 객체 지향이 1960년 발표되고 수많은 시행착오와 베스트 프랙티스 속에서 객체 지향 설계(Object Oriented Design)의 정수라 할 수 있는 5원칙이 집대성 됐는데, 바로 `SOLID`다
    - SRP(Single Responsibilitiy Principle): 단일 책임 원칙
    - OCP(Open Closed Principle): 개방 폐쇄 원칙
    - LSP(Liskov Substitution Principle): 인터페이스 분리 원칙
    - DIP(Dependency Inversion Principle): 의존 역전 원칙

- `응집도는 높이고(High Cohesion), 결합도는 낮추라(Loose Coupling)`는 고전 원칙을 객체 지향의 관점에서 재정립한 것이라고 할 수 있다.
    - 결합도는 모듈(클래스) 간 상호 의존 정도.
        - 결합도가 낮으면 의존성이 줄어 객체의 재사용 및 유지보수가 용이
        - 결합도 수준: 데이터, 스탬프, 컨트롤, 외부, 공유, 내용 결합도
    - 응집도는 하나의 모듈 내부에 존재하는 구성 요소들의 기능적 관련성
        - 응집도가 높은 모듈은 하나의 책임에 집중하고 독립성이 높아져 재사용 및 유지보수가 용이
        - 기능, 순차, 통신, 절차, 시간, 논리, 우연 응집도
