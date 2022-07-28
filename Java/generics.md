### 제네릭스를 사용하는 이유는?
다양한 타입의 객체들을 다루는 메서드나 컬렉션을 만들고 싶다고해보자.

다음과 같이 만들 수 있다.

```java
public class List{
	Object objects = new Object[10];
	int pos = 0;
	public void add(Object object){
		objects[pos++] = object;
	}

	public Object get(int pos){
		return objects[pos];
	}
}
```

하지만 이 클래스는 다음과 같이 여러 오브젝트가 들어가는 상황을 막지 못한다.

```java
List list = new List();
list.add("hello");
list.add(100);
```

위와 같이 서로 다른 타입의 객체를 넣는 경우는 개발자가 의도한 경우도 아니고 추후에 잘못 캐스팅하는 등 런타임에 문제가 생길 수 있다.

하지만 Generics를 쓸 경우 위와 같이 못하도록 컴파일 타임에 막아줄 수 있다. 

즉 Generics를 쓰는 가장 중요한 이유는 컴파일 시점에 타입체크를 해준다는 것이다.