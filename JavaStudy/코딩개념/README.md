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
