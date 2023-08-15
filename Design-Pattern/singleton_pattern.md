### 싱글톤 패턴 (Singleton Pattern)

싱클톤은 클래스에 인스턴스가 하나만 있도록 하면서 이 인스턴스에 대한 전역 접근(엑세스) 지점을 제공하는 생성 디자인 패턴.

- 장점
    - 클래스가 하나의 인스턴스만 가짐
    - 해당 인스턴스에 대한 전역 접근 지점을 얻음
    - 싱글톤 객체는 처음 요청될 때만 초기화됨
- 단점
    - 단일 책임 원칙 위반. 한번에 두 가지의 문제를 동시에 해결함.
    - 다중 스레드* 환경에서 여러 스레드가 싱글턴 객체를 여러번 생성하지 않도록 특별한 처리가 필요
    (다중 스레드 -  프로그램, 특히 프로세스 내에서 실행되는 흐름의 단위를 스레드라 말함. 일반적으로 하나의 스레드를 가지고 있지만, 경우에 따라 둘 이상의 스레드를 동시에 실행할 수 있다.) 
    (스레드 - 프로세스 내에서 실제로 작업을 수행하는 주체)
- 의사코드
    
    ```python
    // 데이터베이스 클래스는 클라이언트들이 프로그램 전체에서 데이터베이스 연결의 같은
    // 인스턴스에 접근할 수 있도록 해주는 `getInstance`(인스턴스 가져오기) 메서드를
    // 정의합니다.
    class Database is
        // 싱글턴 인스턴스를 저장하기 위한 필드는 정적으로 선언되어야 합니다.
        private static field instance: Database
    
        // 싱글턴의 생성자는 `new` 연산자를 사용한 직접 생성 호출들을 방지하기 위해
        // 항상 비공개여야 합니다.
        private constructor Database() is
            // 데이터베이스 서버에 대한 실제 연결과 같은 일부 초기화 코드.
    
        // 싱글턴 인스턴스로의 접근을 제어하는 정적 메서드.
        public static method getInstance() is
            if (Database.instance == null) then
                acquireThreadLock() and then
                    // 이 스레드가 잠금 해제를 기다리는 동안 인스턴스가 다른
                    // 스레드에 의해 초기화되지 않았는지 확인하세요.
                    if (Database.instance == null) then
                        Database.instance = new Database()
            return Database.instance
    
        // 마지막으로 모든 싱글턴은 해당 로직의 인스턴스에서 실행할 수 있는 비즈니스
        // 로직을 정의해야 합니다.
        public method query(sql) is
            // 예를 들어 앱의 모든 데이터베이스 쿼리들은 이 메서드를 거칩니다. 따라서
            // 여기에 스로틀링 또는 캐싱 논리를 배치할 수 있습니다.
            // …
    
    class Application is
        method main() is
            Database foo = Database.getInstance()
            foo.query("SELECT ...")
            // …
            Database bar = Database.getInstance()
            bar.query("SELECT ...")
            // 변수 `bar`는 변수 `foo`와 같은 객체를 포함할 것입니다.
    ```
    

---

[리팩터링과 디자인 패턴](https://refactoring.guru/ko)

디자인 패턴 설명글