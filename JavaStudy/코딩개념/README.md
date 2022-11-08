# Collection

- List, Set, Queue로 크게 3가지 상위 인터페이스로 분류
- Map의 경우 Collection 인터페이슬르 상속받지 않지만 Collection으로 분륜된다.

| 인터페이스 |          구현클래스           | 특징                                                                                                                                         |
| :--------: | :---------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------- |
|    Set     |       HashSet, TreeSet        | 순서를 유지하지 않는 데이터의 집합으로 데이터의 중복을 허용하지 않는다.                                                                      |
|    List    | LinkedList, Vector, ArrayList | 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.                                                                                      |
|   Queue    |   LinkedList, PriorityQueue   | List와 유사                                                                                                                                  |
|    Map     |  Hashtable, HashMap, TreeMap  | 키(Key), 값(Value)의 쌍으로 이루어진 데이터으 집합으로, 순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다. |

- Set 인터페이스

  - 순서 X, 중복 X
  - HashSet
    - 가장빠른 임의 접근 속도
    - 순서를 예측할 수 없음
  - TreeSet
    - 정렬방법을 지정할 수 있음
    - HashSet에 비해 느림

- List 인터페이스

  - 순서 O, 중복 O
  - LinkedList
    - 양방향 포인터 구조
    - 스택, 큐
  - Vector
    - 과거에는 대용량 처리를 위해 사용했으며, 내부에서 자동 동기화처리가 일어나고 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않음.
  - ArrayList
    - 단방향 포인터 구조. 인덱스를 가지고 있어 조회 기능에 성능이 뛰어남

- Map 인터페이스
  - 키, 값의 쌍으로 이루어진 데이터 집합
  - 순서 X, 키 중복 X, 값 중복 O
  - HashTable
    - HashMap보다 느리지만 동기화 지원
    - null 불가
  - HashMap
    - 중복과 순서가 허용되지 않지만 null값이 올 수 있음.
  - TreeMap
    - 정렬된 순서대로 키와 값을 저장하여 검색이 빠름

---

# Set

- 순서가 없고 중복되지 않는 집합
- HashSet과 TreeSet이 있으며, 일반적으로 HashSet을 사용한다. 시간이 더 빠름.

## HashSet

### 1. 개념 및 구조

- hash를 사용
- 저장하는 값을 중복되지 않는 숫자코드로 변환하여 해당 코드의 메모리 위치에 값을 저장
- 저장 : O(1)
- 탐색 : O(1)

### 2. 실사용

- Collection 중 set의 파생 클래스
  - 중복원소를 허용하지 않는다
  - 순서 개념이 없다. 즉, collection.sort() 메소드를 사용할 수 없다. 정렬을 원한다면 리스트로 변환 후 정렬해야 한다.
- 메소드

  - 선언

    ```
        HashSet<Integer> set1 = new HashSet<Integer>();//HashSet생성
        HashSet<Integer> set2 = new HashSet<>();//new에서 타입 파라미터 생략가능
        HashSet<Integer> set3 = new HashSet<Integer>(set1);//set1의 모든 값을 가진 HashSet생성
        HashSet<Integer> set4 = new HashSet<Integer>(10);//초기 용량(capacity)지정
        HashSet<Integer> set5 = new HashSet<Integer>(10, 0.7f);//초기 capacity,load factor지정
        HashSet<Integer> set6 = new HashSet<Integer>(Arrays.asList(1,2,3));//초기값 지정
    ```

    - 기본으로 생성 했을 때, initial capacity(16), load factor(0.75)의 값을 가진 HashSet객체가 생성
    - 저장공간보다 값이 추가로 들어오면 저장용량을 두배로 늘림
    - 초기 데이터 갯수를 알고 있다면 초기용량을 정해주는 것이 좋음

  - 값 추가

    ```
        HashSet<Integer> set = new HashSet<Integer>();
        set.add(1);
        set.add(2);
        set.add(3);
    ```

    - add(value)
      - 내부에 값이 존재하지 않으면, 추가하고 true를 반환
      - 내부에 값이 존재하면, false를 반환

  - 값 삭제

    ```
        HashSet<Integer> set = new HashSet<Integer>(Arrays.asList(1,2,3));//HashSet생성
        set.remove(1);//값 1 제거
        set.clear();//모든 값 제거
    ```

    - remove(value)
      - 내부에 값이 존재하면, 삭제하고 true를 반환
      - 내부에 값이 존재하지 않으면, false를 반환
    - clear()
      - 모든 값을 제거

  - 크기 구하기

    - size()

  - 값 출력

    ```
        HashSet<Integer> set = new HashSet<Integer>(Arrays.asList(1,2,3));//HashSet생성

        System.out.println(set); //전체출력 [1,2,3]

        Iterator iter = set.iterator();	// Iterator 사용
        while(iter.hasNext()) {//값이 있으면 true 없으면 false
            System.out.println(iter.next());
        }
    ```

    - 그냥 print하면 []로 묶어서 Set 전체값이 출력됨.
    - 전체 객체를 대상으로 한번씩 반복해서 가져오는 반복자(iterator)를 제공
    - iterator에서 하나의 객체를 가져올 때는 next() 메소드를 사용
    - 사용하기 전에 hasNext() 메소드를 사용하여 있는지 확인작업 필요
      - 객체가 있으면 true, 없으면 false 리턴

  - 값 검색
    - contain(value)
      - 값을 가지고 있다면 true, 없다면 false

## TreeSet

- 이진트리 기반의 레드-블랙 트리
- 한 노드를 기준으로 적은 값은 왼쪽, 큰 값은 오른쪽에 저장
- 저장 : O(log n)
- 탐색 : O(log n)

---

# Scanner, InputStream, BufferedReader

## Java의 인코딩

- 자바는 내부적으로 (메모리 상에서) 문자열이 UTF-16 으로 인코딩되어 처리된다.
- 문자열 송/수신을 위해 직렬화가 필요할 때에는 변형된 UTF-8 을 사용한다.
- 문자열을 입출력 할 때는 운영체제 기본 인코딩값, 또는 사용자가 지정한 인코딩 값으로 문자열을 인코딩한다. (내부 메모리 상에서 처리되는 - 것과는 다르다.)
- 1 ~ 127 까지는 Ascii 코드 값과 유니코드(UTF-8, UTF-16 등..), MS계열 코드(CP949, MS949 등..) 의 값이 같다.

## UTF-16 vs UTF-8

- Byte 구성 방식에서 차이가 있음
- UTF-8 의 경우 문자의 영역에 따라 Byte 사용 개수가 다른데, 영어의 경우 1Byte, 한글의 경우 3Byte 를 사용한다.
- UTF-16 은 거의 모든 문자가 2Byte 로 구성된다.
- 인코딩 할때 NULL 문자가 나타나지 않기 위해 Java에서는 UTF-16를 사용한다.

## Java 메모리에 올라갈 때의 과정

- 이클립스의 File encoding 이 UTF-8 이라면
  - 입력(UTF-8) -> 송수신(modified UTF-8) -> 자바 메모리 (UTF-16) -> 송수신(modified UTF-8) -> 출력(UTF-8)
- 운영체제 혹은 시스템에 설정되어 있는 인코딩 형식으로 입력받으면 UTF-16 의 인코딩 규칙에 의해 인코딩되어 메모리에 올라감
- 출력하게 될 경우 메모리에 UTF-16 인코딩 규칙에 의해 저장되있는 값을 다시 운영체제 혹은 시스템에서 설정한 인코딩 형식으로 대응되는 문자를 출력함

## 스트림이란?

- 출발지와 도착지를 이어주는 빨대
- [입력장치] >>(입력 스트림)>> [프로그램] >>(입력 스트림)>> [출력 장치]
- 한 곳에서 다른 곳으로의 데이터 흐름
- 단방향이기에 입력과 출력이 동시에 발생할 수 없다.
- 자바에서 가장 기본이 되는 입력 스트림은 InputStream

## 1. System.in 그리고 InputStream

- System.in 이 InputStream 타입의 필드
- System 클래스의 in 이라는 필드는 InputStream 의 정적 필드

```
import java.io.IOException;
import java.io.InputStream;

public class Input_Test {

	public static void main(String[] args) throws IOException {

		InputStream inputstream = System.in;
		int a = inputstream.read();
		System.out.println(a);

	}
}
```

- 입력 : 9, 출력 : 57
- 입력한 값과 다른 값이 나옴

### InputStream.read()

- 입력받은 데이터는 int 형으로 저장되는데 이는 해당 문자의 시스템 또는 운영체제의 인코딩 형식의(필자의 경우 UTF-8) 10진수로 변수에 저장
- 1 byte 만 읽는다
  - InputStream 은 바이트 단위로 데이터를 보내며 이 InputStream 의 입력 메소드인 read()는 1 바이트 단위로 읽어 들인다.
  - 입력받은 문자가 2byte 이상으로 구성되어있는 인코딩을 사용할 경우 1byte 값만 읽어들이고 나머지는 읽지 않고 스트림에만 남아있기 때문에 출력할 때는 해당 데이터의 1 Byte 에 대한 인코딩 값을 10진수로 변환한 값이 출력
- 한글을 제대로 인식하지 못하는 가장 큰 문제가 있음
- 정리
  - UTF-8 로 입력을 받는다
  - read() 메소드는 1 byte 만 읽기 때문에 나머지 byte 는 스트림에 잔존하게 된다.
  - 읽어들인 byte 값은 메모리에 UTF-16 에 대응되는 문자의 인코딩방식으로 2진수 값이 저장
  - 출력시 메모리에 저장되어있던 2진수에 대응되는 문자가 UTF-8 로 변환되어 출력

## 2. Scanner(System.in) 그리고 InputStreamReader(System.in)

- Scanner(System.in) 은 입력 바이트 스트림인 InputStream 을 통해 표준 입력받으려고 함
- 왜 Scanner() 에 왜 InputStream 이 들어가는 걸까?
  - Scanner 클래스의 파일에는 Scanner 라는 생성자가 오버로딩(Overloading) 되어있음
  - 흔하게 쓰는 Scanner(System.in) 은 System.in 이 InputStream 타입이고 System.in 하나만 넣었으니 어디로 가는지 볼 수 있음
  - public Scanner(InputStream source) 로 보내짐
    ```
    public Scanner(InputStream source) {
      this(new InputStreamReader(source), WHITESPACE_PATTERN);
    }
    ```
    - 여러 Scanner() 생성자들은 private Scanner(Readable source, Pattern pattern) 으로 넘겨짐
    - new InputStreamReader(source)을 왜 보냄?
      - 여기서 InputStreamReader이 나옴

## InputStreamReader

- InputStream 은 우리가 InputStream.read() 를 통해 입력을 받으려고 해도 1Byte 만 인식하니 한글은 입력해봤자 읽지도 못하고 엉뚱한 문자만 나옴.
- 문자를 온전하게 읽어들이기 위해 확장시킨 것이 InputStreamReader.
- InputStream 의 바이트 단위로 읽어 들이는 형식을 문자단위(character)로 데이터로 변환시키는 중개자 역할
- InputStreamReader을 문자스트림이라 함
- 특징
  - 바이트 단위 데이터를 문자(character) 단위 데이터로 처리할 수 있도록 변환
  - char 배열로 데이터를 받을 수 있음

## Scanner

- next(), nextInt(), nextDouble(), nextFloat() 등 입력을 통한 메소드는 다음과 같이 구성
- nextInt()
  - Scanner.nextInt() 를 쓰면 nextInt() 메소드에서 오버로딩된 아래의 nextInt(int radix) 메소드로 보내짐
  - try - catch 문의 String s = next(integerPattern());
    - buildIntegerPatternString()
      - 여기서 입력받은 문자를 해당 메소드로 보내서 정규식들을 검사하고 검사 된 문자열을 반환함.
      - 중요! Scanner가 느린 이유임. 정규식을 다 검사하기 때문에.
  - 많은 정규식을 통과하여 return 된 정규식의 문자열을 patternCache.forName()으로 보냄.
  - 이 메소드를 통해 Pattern 이라는 타입으로 compile 을 호출하여 String 정규식 문자열을 Pattern 이라는 객체로 반환
    - Pattern 은 java.util.regex 패키지에 있는 클래스이며 정규식의 컴파일된 표현
  - Pattern 이라는 객체가 반환되면 Pattern integerPattern 에 저장되고 이를 반환
  - 처음의 nextInt() 로 다시 돌아감
  - 즉, Pattern 객체를 String 타입으로 변환시키고 최종적으로 Integer.parseInt(s, radix)로 인해 int 형으로 리턴된다.
- 정리
  - InputStream (바이트스트림) 을 통해 입력 받음
  - 문자로 온전하게 받기 위해 중개자 역할을 하는 InputStreamReader(문자스트림) 을 통해 char 타입으로 데이터를 처리
  - 입력받은 문자는 입력 메소드( next(), nextInt() 등등.. ) 의 타입에 맞게 정규식을 검사
  - 정규식 문자열을 Pattern.compile() 이라는 메소드를 통해 Pattern 타입으로 변환
  - 반환된 Pattern 타입을 String으로 변환
  - String 은 입력 메소드의 타입에 맞게 반환

## BufferedReader 그리고 InputStreamReader(System.in)

```
InputStream inputstream = System.in;
InputStreamReader sr = new InputStreamReader(inputstream);
BufferedReader br = new BufferedReader(sr);
```

```
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
```

- 기본적으로 바이트 스트림인 InputStream 을 통해 바이트 단위로 데이터를 입력을 받음
- 입력 데이터를 char 형태로 처리하기 위해 중개자 역할인 문자스트림 InputStreamReader 로 감싸줌

## BufferedReader는 왜 필요할까?

- Scanner 에서 InputStreamReader 을 설명할 때 '문자'를 처리한다고 했다. '문자열'이 아니다.
  - 그래서 Scanner 에서도 내부에서 임시 배열을 두어 문자열처럼 사용하고 있음
- 만약 문자열을 입력하고 싶다면 매번 배열을 선언해야 한다는 단점은 그대로 남아있다. 심지어 입력받을 문자열의 길이가 가변적이라면 더욱 불편하다.
  - 그렇기에 Buffer(버퍼)를 통해 입력받은 문자를 쌓아둔 뒤 한 번에 문자열처럼 보내는 것.
- BufferedReader 를 쓸 때 우리는 입력 메소드로 readLine() 을 많이 쓴다. 이 메소드는 한 줄 전체를(공백 포함) 읽기 때문에 char 배열을 하나하나 생성할 필요 없이 String 으로 리턴하여 바로 받을 수 있다
- 정리
  - Byte Type = InputStream
  - Char Type = InputStreamReader
  - Char Type 의 직렬화(String) = BufferedReader
- 특징
  - 버퍼가 있는 스트림
  - 별다른 정규식 검사를 하지 않는다.
- 문자 하나하나를 보내는 것이 아니라, 한번에 모아 보내고 정규식 검사를 하지 않으니 속도가 Scanner에 비해 매우 빠르다.
- 즉, 바이트 단위 [InputStream]로 문자를 입력받아 문자(character) [InputStreamReader]로 처리한 뒤 버퍼(buffer) [BufferedReader]에 담아두었다가 일정 조건이 되면 버퍼를 비우면서 데이터를 보내는 것
