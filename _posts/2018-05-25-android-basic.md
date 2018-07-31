---
layout: post
title:  "app"
date:   2018-05-25 09:05:00
author: 강진광
categories: [ANDROID]
comments: true
tags:
  - Android
---
## 중요한 것
1.activity에 대한 구조 layout종류들
2.intant 
3.intant간에 데이터 주고 받는 방식들
ex)start activity result함수 사용해서
4.activity의 life cycle
5.observer
6.event 거는 것들
7.adapter

메인쓰레드는 UI이다.
어플리케이션의 비동기는 쓰레드이다. > A Sync Task이다.비동기 자원을 처리할 수 있는 쓰레드이다.
다중 쓰레드를 지원한다.
보조 쓰레드로는 메인 쓰레드의 UI를 직접 변경 하면 안된다.***
보조 쓰레드는 메인쓰레드의 변수를 참조하거나 변경을 할 수 있지만
보조 쓰레드로 UI를 변경하기 위해선 별도의 추가 도구가 필요하다

AsyncTask는 백그라운드 스레드 와 UI 스레드를 같이 쓰기 쉽게 설계했으며, 
일반 스레드와 달리, 간단한 작업에 적합하게 만들었다

AsyncTask객체는 abstract로 작성되었다. 따라서, 익명클래스로 사용허던가 위와 같이 상속을 통해서 사용해야 한다.

~~~java
public class MommooAsyncTask extends AsyncTask<String,Void,String>{
~~~


제네릭 인자3개를 정해야한다. 



첫번째 인자는 doInBackground 메서드에 선언하는 가변인수 매개변수의 타입을 정한다.



두번째 인자는 onProgressUpdate 메서드에 선언하는 가변인수 매개변수의 타입을 정한다.



세번째 인자는 onPostExecute 메서드에 선언하는 매개변수의 타입을 정한다.

~~~java
@Override
    protected void onPreExecute() {
        super.onPreExecute();
    }
~~~


첫번째 메서드다. 해당 메서드는 이름에서 볼 수 있드시,  background스레드를 실행하기전 준비 단계이다.



변수의 초기화나, 네트워크 통신전 셋팅해야할 것들을 위의 메서드 공간에 작성한다. 




~~~java
 @Override
    protected String doInBackground(String... params) {
        return result;
    }
~~~


두번째 메서드다. 해당 메서드가 background 스레드로 일처리를 해주는 곳이다.



보통 네트워크, 병행 일처리등을 위 메서드 공간에 작성한다.



중요한건 마찬가지로 스레드 이므로 UI스레드가 어떤 일을 하고 있는지 상관없이



별개의 일을 진행한다는 점이다. 따라서 AysncTask는 비동기적으로 작동한다.




~~~java
@Override
    protected void onProgressUpdate(Void... values) {
        super.onProgressUpdate(values);
    }
~~~


세번째 메서드는 doInBackground 메서드에서 중간중간에 UI스레드 에게 일처리를 맡겨야 하는 상황일때



쓴다. 매개변수로 Void를 받으므로, doInBackground안에 실제인자가 없이,



 publishProgress( ) 메서드를 호출하면 BackgroundThread 중간에 mainThread에게 일을 시킬 수 있다.




~~~java
@Override
    protected void onPostExecute(String s) {
        super.onPostExecute(s);
    }
~~~


마지막 메서드다. background Thread가 일을 끝마치고 리턴값으로 result를 넘겨준다.



그 값을 지금 보고 있는 해당 메서드가 매개변수로 받은후 받은 데이터를 토데로



UI스레드에 일처리를 시킬때 쓰는 메서드이다.