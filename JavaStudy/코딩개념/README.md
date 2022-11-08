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
