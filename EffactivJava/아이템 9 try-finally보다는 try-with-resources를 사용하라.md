# 아이템 9 : try-finally보다는 try-with-resources를 사용하라

- 자바 라이브러리에는 close 메서드를 호출해 직접 닫아줘야 하는 자원이 많다.
- InputStream, OutputStream, java.sql.Connection 등이 좋은 예다.

### try-finally - 더 이상 자원을 회수하는 최선의 방책이 아니다!

```java
static String firstLineOfFile(String path) throws IOException {

	BufferedReader br = new BufferedReader( new FileReader(path));
	try {
		return br.readLine();
	} finally {
		br.close();
	}

}

```

- 자원이 둘 이상이면 try-finally 방식은 너무 지저분하다!

이러한 문제들은 try-with-resources 덕에 모두해결되었다.

해당 자원이 AutoCloseable 인터페이스를 구현해야한다.

### try-with-resources- 자원을 회수하는 최선책!

```java
static String firstLineOfFile(String path) throws IOException {
	try (BufferedReader br = new BufferedReader(
				new FileReader(path))) {
			return br.readLine();
	 }
}

static void copy(String src, String dst) thorws IOException {
	try (InputStream in = new FileInputStream(src);
			OutputStream out = new FileOutputStream(dst)) {
		byte[] buf = new byte[BUFFER_SIZE];
		int n;
		while ((n = in.read(buf)) >= 0)
			out.write(buf, 0, n);
	}
}
```

- try-with-resources 버전이 짧고 읽기 수월할 뿐 아니라 문제를 진단하기도 훨씬 좋다.

### try-with-resources를 catch절과 함께 쓰는 모습

```java
static String firstLineOfFile(String path, String defaultVal) {
	try (BufferedReader br = new BufferedReader(
				new FileReader(path))) {
			return br.readLine();

	

	} catch (IOException e) {
		return defaultVal;
	}
}
```

꼭 회수해야 하는 자원을 다룰때는 try-fianl 말고 try-with-resources를 사용하자. 예외는 없다.

코드는 더짧고 분명해지고, 만들어지는 예외 정보도 훨씬 유용하다.