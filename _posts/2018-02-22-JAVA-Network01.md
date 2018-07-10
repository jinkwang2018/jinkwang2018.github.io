---
layout: post
title:  "JAVA-Network-URLConnection"
date:   2018-02-22 12:00:00
author: 강진광
categories: [JAVA]
comments: true
tags:
  - Java
  - Network
  - URLConnection
---
# [URLConnection]

~~~java
import java.io.BufferedInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.net.URL;
import java.net.URLConnection;

//네트워크 작업을 할 수 있는 자원
//JAVA API 제공
//클래스를 통해서
//URL 클래스 (인터넷 상에 주소를 다루는 클래스)
//URLConnection 클래스(연결된 URL 주소로 부터 다양한 정보와 작업)
//http://image3.kangcom.com/2018/02/b_pic/201802028179.jpg
public class Ex01_URLConnection {
	public static void main(String[] args) throws IOException {
		String urlstr = "http://www.kangcom.com/images/banner/chocolate_150.jpg";
		URL url = new URL(urlstr);
		BufferedInputStream bis = new BufferedInputStream(url.openStream());

		FileOutputStream fos = new FileOutputStream("copy1.jpg"); //파일 생성 (프로젝트 폴더)

		byte[] buffer = new byte[2048];
		int n = 0;
		int count = 0;

		URLConnection uc = url.openConnection();
		int filesize = uc.getContentLength();

		System.out.println("파일크기: " + filesize);
		System.out.println("파일형식: " + uc.getContentType());

		while((n = bis.read(buffer)) != -1) {
			fos.write(buffer, 0, buffer.length);
			fos.flush();
			System.out.println("n: " + n);
			count += 1;
		}
		System.out.println("count: " + count);
		fos.close();
		bis.close();
	}
}
~~~
