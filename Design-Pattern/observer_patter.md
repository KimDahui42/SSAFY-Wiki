# 옵서버 패턴

태그: 디자인패턴
CS 주제: [3주차] 디자인패턴 (https://www.notion.so/3-d4384722e3ef47a3bfbb7de851960b49?pvs=21)
날짜: 2023년 8월 12일
블로그 게시: No
완료: No
작성자: 박주헌
참고자료: https://refactoring.guru/ko/design-patterns/observer
출석: 230812 출석 (https://www.notion.so/230812-ac15e50bb8cb413c97ea7b5708bd5477?pvs=21)

# 옵저버 패턴(Observer)

---

### What?

- **옵저버 패턴이란**
    
    **옵저버** 패턴은 당신이 여러 객체에 자신이 관찰 중인 객체에 발생하는 모든 이벤트에 대하여 알리는 구독 메커니즘을 정의할 수 있도록 하는 행동 디자인 패턴.
    

---

### Why?

- **옵저버 패턴을 쓰는 이유**
    - 다른 객체의 상태 변화를 별도의 함수 호출 없이 즉각적으로 알 수 있다.
    - 때문에, 이벤트에 대한 처리가 잦다면 효율적인 프로그램을 작성할 수 있다.

---

### How?

- **옵저버 패턴의 사용방법**
    
    ```python
    // 기초 출판사 클래스에는 구독 관리 코드 및 알림 메서드들이 포함됩니다.
    class EventManager is
        private field listeners: hash map of event types and listeners
    
        method subscribe(eventType, listener) is
            listeners.add(eventType, listener)
    
        method unsubscribe(eventType, listener) is
            listeners.remove(eventType, listener)
    
        method notify(eventType, data) is
            foreach (listener in listeners.of(eventType)) do
                listener.update(data)
    
    // 구상 출판사는 일부 구독자에게 흥미로운 실제 비즈니스 논리를 포함합니다. 우리는
    // 이 클래스를 기초 출판사로부터 파생시킬 수 있습니다. 그러나 이는 현실에서 항상
    // 가능하지 않습니다. 왜냐하면 구상 클래스가 이미 자식 클래스일 수 있기
    // 때문입니다. 이 경우 여기에서 했던 것처럼 합성 관계 속으로 구독 논리를 덧붙여
    // 넣을 수 있습니다.
    class Editor is
        public field events: EventManager
        private field file: File
    
        constructor Editor() is
            events = new EventManager()
    
        // 비즈니스 로직의 메서드들은 구독자들에게 변경 사항에 대해 알릴 수 있습니다.
        method openFile(path) is
            this.file = new File(path)
            events.notify("open", file.name)
    
        method saveFile() is
            file.write()
            events.notify("save", file.name)
    
        // …
    
    // 여기 구독자 인터페이스가 있습니다. 사용 중인 프로그래밍 언어가 함수형 타입을
    // 지원하는 경우 전체 구독자 계층구조를 함수들의 집합으로 바꿀 수 있습니다.
    interface EventListener is
        method update(filename)
    
    // 구상 구독자들은 자신이 연결된 출판사가 발행한 업데이트에 반응합니다.
    class LoggingListener implements EventListener is
        private field log: File
        private field message: string
    
        constructor LoggingListener(log_filename, message) is
            this.log = new File(log_filename)
            this.message = message
    
        method update(filename) is
            log.write(replace('%s',filename,message))
    
    class EmailAlertsListener implements EventListener is
        private field email: string
        private field message: string
    
        constructor EmailAlertsListener(email, message) is
            this.email = email
            this.message = message
    
        method update(filename) is
            system.email(email, replace('%s',filename,message))
    
    // 앱은 런타임에 출판사들과 구독자들을 설정할 수 있습니다.
    class Application is
        method config() is
            editor = new Editor()
    
            logger = new LoggingListener(
                "/path/to/log.txt",
                "Someone has opened the file: %s")
            editor.events.subscribe("open", logger)
    
            emailAlerts = new EmailAlertsListener(
                "admin@example.com",
                "Someone has changed the file: %s")
            editor.events.subscribe("save", emailAlerts)
    ```
    

---