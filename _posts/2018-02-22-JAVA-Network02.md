---
title:  "JAVA-Network-TCP"
tags: Java
---
# [TCP client]
~~~java
import java.io.DataInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.Socket;

//client

//server IP: 192.168.0.24
//port: 9999
public class Ex02_TCP_Client {
	public static void main(String[] args) throws Exception, IOException {
		Socket socket = new Socket("192.168.0.36", 9999);
		System.out.println("서버와 연결 되었습니다");

		//서버에서 보낸 메시지 받기
		InputStream in = socket.getInputStream();
		DataInputStream dis = new DataInputStream(in);

		String servermsg = dis.readUTF();
		System.out.println("서버에서 보낸 메시지: " + servermsg);

		dis.close();
		in.close();
		socket.close();
	}
}
~~~

# [TCP server]

~~~java
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

//TCP 서버
//server(process) - client(process)

//서버: 192.168.0.24
//포트: port: 9999

public class Ex02_TCP_Server {
	public static void main(String[] args) throws IOException {
		ServerSocket serversocket = new ServerSocket(9999);
		System.out.println("접속 대기중...");
		Socket socket = serversocket.accept(); //클라이언트가 접속을 하면
		System.out.println("연결완료");

		//연결이 되면
		//서버: 클라이언트 (read, write)
		//socket과 socket
		//socket(input, output Stream)

		OutputStream out = socket.getOutputStream();
		DataOutputStream dos = new DataOutputStream(out); //8가지 기본 타입 + @
		dos.writeUTF("문자데이터 Byte 통신 가능: 아뇨, 전 뚱인데요");

		System.out.println("서버 종료");

		dos.close();
		out.close();
		socket.close();
		serversocket.close();
	}
}
~~~