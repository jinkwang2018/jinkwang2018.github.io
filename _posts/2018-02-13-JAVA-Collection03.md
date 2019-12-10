---
title:  "JAVA-Collection-Generic"
tags: Java
key: post-java-11
---
# [Generic]
#### JDK1.5 부터 지원
#### C#, Java 필수기능

### 사용목적
##### 1.Object 타입 저항 -> Object 타입 탈피
##### 2.타입 안정성 (타입 강제성)
##### 3.형변환 (casting) 문제 해결: (Car)obj;

##### Generic 클래스 설계 >> 타입을 강제해서 객체를 생성할 수 있다
~~~java
MyGen<String> m = new MyGen<String>();
class MyGen<T> { //Type Parameter
	T obj;
	void add(T obj) {
		this.obj = obj;
	}
	T get() {
		return this.obj;
	}
}
/*
[heap메모리에 올라 갈 때]
MyGen<Car> c = new MyGen<Car>(); 해주면

class MyGen<Car> { //Type Parameter
	Car obj;
	void add(Car obj) {
		this.obj = obj;
	}
	Car get() {
		return this.obj;
	}
}
 */

public static void main(String[] args) {
MyGen<String> my = new MyGen<String>();
my.add("Hello");
String result = my.get();
System.out.println(result);

//Person 타입을 강제하는
//MyGen 타입의 객체 생성
MyGen<Person> myobj = new MyGen<Person>();
myobj.add(new Person());
Person p = myobj.get();
System.out.println(p.age);

//Quiz (Generic x)
ArrayList list = new ArrayList();
list.add(500);
list.add(new Person());
list.add("홍길동");

//개선된 for문을 통해 출력 >> 500, 100, "홍길동"
for(Object obj : list) {
	if(obj instanceof Person) {
		Person person = (Person)obj;
		System.out.println(person.age);
	}else {
		System.out.println(obj);
	}
}

//위 처럼 사용하지 말자 -> generic 사용 배경
ArrayList<Person> alist = new ArrayList<>();
alist.add(new Person());
System.out.println(alist.get(0).age);
		
	}
}
~~~
# EX1)
~~~java

class Tv extends Product {
	@Override
	public String toString() {
		return "Tv";
	}
}

class Audio extends Product {
	@Override
	public String toString() {
		return "Audio";
	}
}

class NoteBook extends Product {
	@Override
	public String toString() {
		return "NoteBook";
	}
}

public class Ex07_Generic_Quiz {
	public static void main(String[] args) {
		//1. Array(배열)를 사용해서 Cart를 만들고 Cart에 제품(Tv, Audio, Notebook) 담으세요
		//Product[] cart = {new Tv(), new Audio(), new Notebook()}; //내가 짠 코드
		Product[] cart = new Product[3]; //다형성을 사용한 배열
		cart[0] = new Tv();
		cart[1] = new Audio();
		cart[2] = new NoteBook();

		//2. ArrayList(Collection를 사용해서 Cart를 만들고 Cart에 제품(Tv, Audio, Notebook) 담으세요
		//ArrayList<Product> pcart = new ArrayList<>();
		List<Product> pcart = new ArrayList<>();
		pcart.add(new Tv());
		pcart.add(new Tv());
		pcart.add(new Tv());
		pcart.add(new Audio());
		pcart.add(new NoteBook());
		pcart.add(new NoteBook());

		for(Product product : pcart) {
			System.out.println(product.toString());
		}
	}
}

~~~

# EX2)
~~~java
public class Ex08_Generic_Quiz {
	public static void main(String[] args) {
		//1. Emp 클래스를 사용해서 사원 3명을 생성하세요
		//단 ArrayList<T> 제너릭을 사용하세요
		ArrayList<Emp> earr = new ArrayList<>();
		
		earr.add(new Emp(1, "김", "군인"));
		earr.add(new Emp(2, "이", "IT"));
		earr.add(new Emp(3, "박", "영업"));
		
		
		//2. 사원의 정보 (사번, 이름, 직종)을 개선된 for문을 사용해서 출력하세요
		//단 toString() 사용 금지
		for(Emp e : earr) {
			System.out.println(e.getEmpno() + "/" + e.getEname() + "/" + e.getJob());
		}
		
		System.out.println();
		
		//3. 사원의 정보 (사번, 이름, 직종)을 일반 for문을 사용해서 출력하세요
		//단 toString() 사용 금지
		for(int i = 0; i < earr.size(); i++) {
			System.out.println(earr.get(i).getEmpno() + "/" + earr.get(i).getEname() + "/" + earr.get(i).getJob());
		}
		
		System.out.println();
		
		//4. CopyEmp 라는 클래스를 만드세요(Emp와 같은데)
		//job member field 대신에>>
		//private int sal; 로 바꾸어 만드세요(getter, setter 구현)
		//kr.or.bit.CopyEmp 로 하세요
		//ArrayList<> 제너릭 사용해서 사원 3명 만드세요
		//아래 데이터 사용
		//100, "김", 1000
		//200, "이", 2000
		//300, "박", 3000
		ArrayList<CopyEmp> carr = new ArrayList<>();
		carr.add(new CopyEmp(100, "김", 1000));
		carr.add(new CopyEmp(200, "이", 2000));
		carr.add(new CopyEmp(300, "박", 3000));
		
		//5. 200번 사원의 급여를 5000으로 수정해서 출력하세요 (일반 for문 안에서...)
		for(int i = 0; i < carr.size(); i++) {
			if(carr.get(i).getEmpno() == 200) {
				carr.get(i).setSal(5000);
				System.out.println(carr.get(i).toString());
			}
		}
		
		System.out.println();
		
		//6. 300번 사원의 이름을 "궁금해"로 수정해서 출력하세요 (일반 for문 안에서...)
		for(CopyEmp c : carr) {
			if(c.getEmpno() == 300) {
				c.setEname("궁금해");
				System.out.println(c.toString());
			}
		}
		
		System.out.println();
		
		//7.사원정보를 Iterator 인터페이스를 사용해서 출력하세요
		/*
		ArrayList<Integer> intlist = new ArrayList<>();
		intlist.add(44);
		intlist.add(55);
		intlist.add(66);
		
		Iterator<Integer> list2 = intlist.iterator();
		while(list2.hasNext()) {
			System.out.println("[" + list2.next() + "");
		}
		*/
		Iterator<CopyEmp> it = carr.iterator();
		while(it.hasNext()) {
			//next() 이동해서 값을 return
			//System.out.println(it.next().getEmpno());
			//System.out.println(it.next().getEname());
			//System.out.println(it.next().getSal());
			CopyEmp e = it.next();
			System.out.println(e.getEmpno());
			System.out.println(e.getEname());
			System.out.println(e.getSal());
		}
		
		
	}
}
~~~