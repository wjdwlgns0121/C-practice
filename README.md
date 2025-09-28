가장 기본적인 상속

	#include <iostream>
	using namespace std;
	
	class Vehicle
	{
	private:
		int person = 0;    // 탑승인원
		int baggage;       // 화물 무게
	public:
		void ride()  // 탑승
		{
			person++;
		}
		void load(int weight) { baggage = weight; }   // 짐 싣기
		void getOff(int person) { person--; }   // 하차
		int getPerson() { return person; } // 탑승인원 확인
		void showPerson() { cout << person << endl; }
		void showBaggage() { cout << baggage << endl; }
	};
	
	class Cruise :public Vehicle
	{
	private:
		int room;
	public:
		void setRoom(int room);    // 크루즈의 방 수 설정
		void countPerson() { cout << Vehicle::getPerson() << endl; }  // 부모클래스 호출
	};
	
	class AirPlane :public Vehicle
	{
	private:
		int seat;
	public:
		void setSeat(int seat);    // 자리 수 설정
		void countPerson()    // 탑승인원 확인
		{
			cout << Vehicle::getPerson() << endl;     // 부모클래스 호출
		}
	};
	
	int main(int argc, char const* argv[])
	{
		Cruise dolphin;
		dolphin.ride();    // 부모클래스 멤버함수  접근
		dolphin.load(10);  // 부모클래스 멤버함수  접근
		dolphin.countPerson();     // 자식클래스 멤버함수 호출
		dolphin.showPerson();
		dolphin.showBaggage();
	
	
		AirPlane cppAir;
		cppAir.ride();    // 부모클래스의 멤버함수 접근
		cppAir.load(20);  // 부모클래스 멤버함수  접근 
		cppAir.countPerson();     // 자식클래스 멤버함수 호출
		cppAir.showPerson();
		cppAir.showBaggage();
	}

접근 지정자자(protected)

	#include <iostream>
	using namespace std;

	class Base {
	private:
		int pri = 1;
	public:
		int pub = 2;
	protected:
		int pro = 3;
	};
	
	class Drived : private Base{
	public:
		void show() {
			//cout << pri << endl;
			cout << pub << endl;
			cout << pro << endl;
		}
	};
	
	class Drived2 : protected Base{
	public:
		void show() {
			//cout << pri << endl;
			cout << pub << endl;
			cout << pro << endl;
		}
	};
	
	class Drived3 : public Base {
	public:
		void show() {
			//cout << pri << endl;
			cout << pub << endl;
			cout << pro << endl;
		}
	};
	
	int main() {
		Drived priv;
		Drived2 prot;
		Drived3 publ;
	
		priv.show();
		prot.show();
		publ.show();
	
		return 0;
	}

단일 상속

	#include <iostream>
	#include <string>
	using namespace std;
	
	// 부모 클래스 (기반 클래스)
	class Person {
	private:
	
	protected:
		string name;
		int age;
	
	public:
		Person(string n, int a) : name(n), age(a) {}
	
		void introduce() {
			cout << "이름: " << name << ", 나이: " << age << endl;
		}
		void intro() { introduce(); } 
	};
	
	// 자식 클래스 (파생 클래스)
	class Student : public Person {
	private:
		string major;
	
	protected:
		void study() {
			cout << name << " 학생이 " << major << " 전공 공부 중입니다." << endl;
		}
	
	public:
		Student(string n, int a, string m) : Person(n, a), major(m) {}
		//void introCall() { introduce(); }
	};
	
	int main() {
		Student s("홍길동", 21, "컴퓨터공학");
		//s.introduce();   // 부모 클래스 함수 사용(상속할 때 접근 지정자가 protected/private일 떄 접근 불가)
		//s.intro();	// 부모 클래스의 introduce의 접근 지정자가 protected/private여도 접근 가능
					// 자식 클래스가 상속할 때 접근 지정자가 protected/privat일 때 접근 불가
		//s.study();		// 자식 클래스 함수 사용(접근 지정자가 protected/private일 때 접근 불가)
		//s.introCall(); //부모 클래스의 introduce의 접근 지정자에 상관없이 접근 가능
	
		return 0;
	}




다중 상속

	#include <iostream>
	using namespace std;
	
	class Teacher {
	public:
		void teach() {
			cout << "강의를 하고 있습니다." << endl;
		}
	
		void test() {
			cout << "다중 상속을 테스트하기 위한 첫 번째 코드입니다." << endl;
		}
	};
	
	class Researcher {
	public:
		void research() {
			cout << "연구를 하고 있습니다." << endl;
		}
	
		void test() {
			cout << "다중 상속을 테스트하기 위한 두 번째 코드입니다." << endl;
		}
	};
	
	
	class Professor : public Teacher, public Researcher {
	public:
		void introduce() {
			cout << "저는 교수입니다." << endl;
		}
	
		void test() {
			cout << "다중 상속을 테스트하기 위한 세 번째 코드입니다." << endl;
		}
	
	};
	
	int main() {
		Professor p;
		p.introduce();
		p.teach();
		p.research();
		
		p.test(); //자식 클래스의 코드가 호출됨(자식 클래스에 없다면 오류가 발생함-모호성이 생김)
		p.Teacher::test(); //부모 클래스 Teacher의 코드가 호출됨
		p.Researcher::test(); //부모 클래스 Researcher의 코드가 호출됨
		p.Professor::test(); //자식 클래스의 코드가 호출됨
	
	
		return 0;
	}




오버라이딩, 시스템상 각 class의 speak의 저장공간이 다름

	#include <iostream>
	using namespace std;
	
	class Animal {
	public:
		//가상 함수: 함수 선언에서 앞에 virtual 키워드를 붙이면 가상 함수로 바뀌며 오버라이딩이 가능해진다.
		//virtual 키워드로 '가상'으로 바꾸지 않으면 '오버로딩'으로 인식함
		//override 키워드는 컴파일러에게 부모의 가상 함수를 재정의함을 알려주는 키워드, 없어도 오버라이딩으로 작동함
		//└키워드를 사용하면 부모 클래스에 문제가 있을 때(함수가 없거나 시그니처가 다르거나) 컴파일 에러로 알려줌
		//함수의 시그니처는 프로토타입과 비슷하다. 시그니처에 포함된 것: 함수 이름, 매개변수의 타입과 순서) ★반환 자료형은 포함되지 않음
		//오버라이딩은 시그니처와 반환 자료형이 같아야 가능함(오버로딩은 반환 자료형은 고려하지 않음)
		virtual void speak() {
			cout << "동물이 소리를 냅니다." << endl;
		}
	};
	
	class Dog : public Animal {
	public:
		void speak() override {
			cout << "멍멍!" << endl;
		}
	};
	
	class Cat : public Animal {
	public:
		void speak() override {
			cout << "야옹!" << endl;
		}
	};
	
	int main() {
	
		Animal a1;
		Dog a2;
		Cat a3;
	
		a1.speak();
		a2.speak();
		a3.speak();
	
		a1 = a2;
		a1.speak(); //복사 생성자를 호출하더라도 Animal 객체의 speak가 호출됨
		a2.speak();
	
		return 0;
	}

