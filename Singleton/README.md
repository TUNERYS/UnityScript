# UNITY Singleton
## 1. 싱글톤이란?
- 오직 한 개의 클래스 인스턴스만을 갖도록 보장하고 전역적인 접근접을 제공하는 클래스 패턴

- 게임 상태나 사운드 등의 자원을 관리하기 위해 딱 하나만 필요한 매니저 계열의 클래스에 적합
<br/><br/>

## 2. Singleton With MonoBehaviour
유니티에서 MonoBehaviour을 상속받아 사용하는 싱글톤 패턴
```c#
public class Singleton : MonoBehaviour
{
	private void static Singleton instance;
	public void static Singleton Instance
	{
		get
		{	
			// 싱글턴의 Instance를 사용하고자 할 때
			// Singleton의 인스턴스가 없으면 실행
			if(null == instance)
			{
				GameObject gameObject = new GameObject("Object Name");
				instance = gameObject.AddComponent<Singleton>();	
			}
		}
		return instance;
	}

	private void Awake()
	{
		// 컴포넌트 초기화
		if( null == instance )
		{
			instance = this;
			//Scene 변경 시 현재 게임오브젝트가 사라지지 않도록
			DontDestroyOnLoad(gameObject);
		}
		else if( this != instance)
		{
			//인스턴스가 존재하면 현재 컴포넌트 즉시 삭제
			DestroyImmediate(gameObject);
		}
	}
}
```
<br/><br/>

## 3. Singleton Without MonoBehaviour
MonoBehaviour을 상속받지 않는 일반 싱글턴 클래스
### - 기본적인 싱글턴 패턴
```c#
public class Singleton
{
	private static Singleton instance;
	public static Singleton Instance => instance ?? (instance = new Singleton());

	private Singleton(){}
}
```
### - 선언과 동시에 인스턴스 생성
```c#
public class Singleton
{
	private static readonly Singleton instance = new Singleton();
	public static Singleton Instance => instance;

	private Singleton(){}
}
```
- 선언과 동시에 new를 통해 메모리에 올라감
- 앞과 비교하여 미리 만들어두기 때문에 속도가 조금 더 빠르다
<br/><br/>


## 4. 싱글톤의 문제점
#### | 커플링 문제

- 싱글톤을 정적 멤버 변수를 사용하게 되는데, 정적 변수는 모든 클래스에서 접근이 가능하므로

	 서로 상관 없는 클래스 간의 커플링이 발생할 수도 있다.

- 커플링이란 두 클래스 사이에 결합이 발생하여 한 클래스가 변경되면 다른 클래스도 변경되어야 하는 상황을 말한다.

####  | Lazy initialization

- Lazy initialization은 초기화 시점을 인스턴스를 처음 사용하는 시점으로 미루어 cpu와 메모리의 부담을 줄이지만,

	게임에서는 게임 도중 초기화 작업이 발생한다면 프레임이 떨어지는 등의 문제가 발생할 수 있다.
