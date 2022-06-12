### 정의는?
    
    DataSource는 커넥션을 획득하는 방법을 추상화하는 인터페이스이다.
    
    ---
    
    김영한, 스프링 DB 1편, 인프런, DataSource 이해
    
### 사용해야 하는 이유는?
    1. 커넥션을 얻고 사용하는 방법은 다양한다. DriverManager로 직접 얻을 수도 있고 HikariCP나 DBCP2와 같은 커넥션 풀을 사용할 수도 있다.
        
        이 때 방식을 변경할 때마다 애플리케이션 코드에 변경 없이 원하는 방식을 사용할 수 있으려면 Datasource를 사용해야 한다.
        
    2. DriverManager를 사용해서 커넥션을 얻으려면 매번 설정을 얻을 때마다 URL, USERNAME, PASSWORD를 넘겨야 한다.
        
        하지만 DataSource는 처음에만 설정해주면 그 후에는 아무런 인자 없이 getConnection()을 통해서 커넥션을 얻을 수 있다.
        
        즉 설정과 사용을 분리시켜준다.
        
        구체적인 URL과 같은 정보를 사용하는 쪽에서 모르게 만듬으로써 결합도를 낮춰줄 수 있다.
        
    
    ---
    
    김영한, 스프링 DB 1편, 인프런, DataSource 이해
    
### 어떤 방식으로 동작할까?
    
    public interface DataSource{
    	Connection getConnection() throws SQLException;
    }
    
    과 같은 방식으로 추상화가 되어 있고 Hikaricp와 같은 구현체들은 이 메소드를 알아서 구현하여 Connection을 넘기게 된다.
    
    ---
    
    김영한, 스프링 DB 1편, 인프런, DataSource 이해
    
### 잘 사용하려면 어떻게 해야할까?
    
    사실 DriverManager는 DataSource 인터페이스를 구현하고 있지 않기 때문에 DirverManager를 사용하다가 다른 방식으로 커넥션을 얻으려면 코드를 고쳐야 한다.
    
    하지만 스프링에서 DriverManagerDataSource라는 DataSource를 구현하면서도 DriverManager의 역할을 할 수 있는 클래스를 제공하므로 이것을 사용하는게 좋다.