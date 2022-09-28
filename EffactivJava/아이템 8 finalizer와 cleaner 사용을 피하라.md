# 아이템 8 : finalizer와 cleaner 사용을 피하라

- finalizer는 예측할수 없고, 상황에 따라 위험할수 있어 일반적으로 불필요 하다.
- cleaner는 finalizer보다는 덜 위험하지만, 여전히 예측할 수 없고, 느리고, 일반적으로 불필요하다.
- finalizer와 cleaner는 즉시 수행된다는 보장이없다.
    - 즉, finalizer나 cleaner로는 제때 실행되어야 하는 작업은 절대 할 수 없다.
- finalizer와 cleaner 는 심각한 성능 문제도 동반한다.

### 그렇다면 파일이나 스레드 등 종료해야 할 자원을 담고 있는 객체의 클래스에서 finalizer나 cleaner를 대신해 줄 묘안은 무엇일까?

- AutoCloseable을 구현해주고 , 클라이언트에서 다쓰고 나면 close 메서드를 호출한다.

- cleaner나 finalizer 즉시 호출되리라는 보장은 없지만, 클라이언트가 하지 않은 자원 회수를 늦게라도 해주는 것이 아예 안하는 것보다 좋다.

### Cleaner와 Finalizer 의 적절한 활용

1. 자원의 소유자가 close 메서드를 호출하지 않는 것에 대비한 안전망 역할
    1. 클라이언트가 하지 않은 자원 회수를 늦게라도 해주는 것이 아에안하는 것보다는 낮다
    2. FileInputStream,FileOutputStram,ThreadPoolExecutor가 대표적이다
2. 네이티브 피어와 연결된 객체에서사용
    1. 네이티브 피어는 자바 객체가 아니니 가비지 컬렉터는 그 존재를 알지못한다. 그결과 네이티브 객체를 회수하지못한다.
    

### Cleaner의 사용

```java
public class Room impleaments AutoCloseable {
	private static final Cleaner cleaner = Cleaner.create();
	
	//청소가 필요한 자원, 절대 Room을 참조해서는 안된다!
	private static class State implements Runnable {
		int numJunkPiles; // 방(Room) 안의 쓰레기 수
		
		State(int numJunkPiles) {
			this.numJunkPiles = numJunkPiles;
		}

		// close 메서드나 cleaner가 호출한다.
		
		@Override public void run() {
				System.out.println("방 청소");
				numJunkPiles = 0;
		}
		
		//방의 상태, cleanable 과 공유 한다.
		private final State state;

		// cleanable 객체, 수거 대상이 되면 방을 청소한다.
		private final Cleaner.Cleanable cleanable;
			

		public Room(int numJunkPiles) {
				state = new State(numJunkPiles);
				cleanable = cleaner.register(this, state);
		}

		@Override public void close() {
			cleanable.clean();
		}

	}
}
```

- Room을 호출할때 close를 호출하지 않는다면, cleaner가 State의 run 메서드를 호출해줄 것이다.