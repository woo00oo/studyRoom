## try-finally보다는 try-with-resources를 사용하라

자바 라이브러리에는 close 메서드를 호출해 직접 닫아줘야 하는 자원이 많다.

ex) InputStream, OutputStream, java.sql.Connection

꼭 회수해야 하는 자원을 다룰 때는 try-finally 말고, try-with-resources를 사용하자.

예외는 없다. 코드는 더 짧고 분명해지고, 만들어지는 예외 정보도 훨씬 유용하다. try-with-finally로 작성하면 실용적이지 
못할 만큼 코드가 지저분해지는 경우라도, try-with-resources로는 정확하고 쉽게 자원을 회수할 수 있다.