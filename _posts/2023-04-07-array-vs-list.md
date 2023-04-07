---
layout: post
title: Array와 List의 차이
subtitle: 각 구조의 차이와 왜 다르게 사용하는지, 어떤 자료구조가 좋을까?
date: 2023-04-06 23:13:00 GMT+0900
tags: [data_structure]
---
# 들어가기 전에
일단 둘 다 변수를 저장할 수 있는 저장 공간이라는 생각이 든다. 그럼 차이가 뭘까? 그냥 같은거 아닌가?라는 생각을 했다.

# 자료구조
## 1. 자료 구조에서의 리스트
- 원소를 한줄로 나열한 1차원 자료구조
- 각 요소(element)는 그와 대응되는 번호(index)를 가지고 있다

## 2. 자료 구조에서의 배열
- 고정된 크기에 저장하는 자료 구조
- 메모리의 연속된 기억공간에 저장
	- 논리적 순서와 물리적 저장 순서가 일치하게 된다
	- 인덱스를 통해 random access가 가능하다

## 3. 공통점과 차이점
아무리봐도 둘 다 같은 얘기인것 같은데 왜 다르게 부르는지 모르겠어서 예전 필기 내용을 찾아봤다. 예전 필기 내용을 바탕으로 정리한거라 다시 찾아볼 필요가 있을것 같다.

### 차이점
결론부터 말하면 list가 더 상위 개념이다. list는 **유한**한 **요소들의 연속**이며, array는 **인덱스** 기반으로 list를 구현한 것이다. 흔히 array의 비교 대상인 Linked List는 이와 다르게 pointer 기반으로 list를 구현한 것이다. _그래서 array와 list를 비교하는 건가?_

또한 프로그래밍 언어 상에서 array는 같은 데이터 타입을 저장하는 자료구조를 통칭한다.
예를 들어 javascript에서는 타입이 달라도 같은 list에 저장할 수 있지만, C++에서는 불가능하다.
``` javascript
list in javascript : -
let list1 = ['scaler',234,78,true]
``` 

``` c++
array in c++ : -
    
int arr = [23,45,78,32,65,78]
```

### 공통점
공통점이라고 하기 애매해다. 결국 array가 list의 하위 개념이므로 list의 특징이 공통점이다.

주된 특징은 다음과 같다
1. 1차원 자료구조
2. 하나 이상의 요소를 한줄로 나열하기 위한 자료구조
3. 각 요소(element)는 그와 대응되는 번호(index)를 가지고 있다 -> 쌍을 이룬다

# In Java
java에서 array와 list를 알아보려고 한다.

일단 List는 interface, Arrays는 class로 구현 방식부터가 다르다. 그래서 `List`는 타입으로 사용할 수 없다. `new List()`로 객체를 생성하려고 하면 각 메서드를 ovrride해서 구현해야 한다.

일반적으로 배열을 사용할 때 선언하는 기본 방식에 대해 알아보려고 한다.

## 3. 리스트와 배열
``` java
// 1.
int[] intArray = new int[]{10, 20, 30, 40, 50};

// 2.
List<Integer> integerList1 = new List<Integer>();	// 에러

// 3.
List<Integer> integerList2 = new ArrayList<>();
ArrayList<Integer> integerArrayList = new ArrayList<Integer>();

// 4.
LinkedList<Integer> integerLinkedList = new LinkedList<Integer>();

// 5.
Arrays arrays = new Arrays();	// 에러
Arrays.sort(intArray); // 이런 식으로 사용한다.
```

1. `T[]` 형식

	일반적인 배열로 선언하는 방식이다. 

2. `List<T>` 형식

	그냥 List는 인터페이스이므로 new 연산자를 통해 할당할 수 없다. 대신 3번처럼 구체적인 타입으로 선언해야 한다.

3. `ArrayList`

	> Resizable-array implementation of the List interface.

	List를 구현한 구현체. 일반적으로 사용하는 배열 객체이다.

	``` java
	public class ArrayList<E> extends AbstractList<E>
	        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
	{
		transient Object[] elementData;
	}
	```

	그래서 내용을 보면 이렇게 Object 타입의 배열을 기본적으로 가지고 있다.


4. `LinkedList`
	> Doubly-linked list implementation of the List and Deque interfaces.

	ArrayList와 마찬가지로 List를 구현한 구현체이지만 `Deque`도 같이 구현해서 조금 다르다. 우리가 일반적으로 말하는 LinkedList이다.

	``` java
	public class LinkedList<E>
		extends AbstractSequentialList<E>
		implements List<E>, Deque<E>, Cloneable, java.io.Serializable
	{
		transient int size = 0;

		/**
		 * Pointer to first node.
		 */
		transient Node<E> first;

		/**
		 * Pointer to last node.
		 */
		transient Node<E> last;

	}
	```

5. `Arrays`

	array(배열)를 조작하기 위한 메소드를 가지고 있는 객체이다.

	내용을 살펴보면 기본 생성자가 private으로 선언되어 있어서 위의 예제처럼 선언하면 에러가 발생한다. 그 외 메서드들도 static으로 선언되어 있어서 일종의 유틸성 객체이다.

	``` java
	public class Arrays {

		// Suppresses default constructor, ensuring non-instantiability.
		private Arrays() {}

	}
	``` 

## 4. 연산
그럼 데이터가 많을 때 ArrayList를 사용하는게 좋을까? Linked List를 사용하는게 좋을까?

일단 두 방식의 특징을 다시 한 번 확인해보면,

먼저 ArrayList는 연속된 배열이다. 또한 그 길이가 정해져있다.

반면 LinkedList는 포인터 기반의 배열이다. 다음 요소의 위치를 가르키기만 하면 된다. 즉 비연속적이다.

### 요소 추가하기
- ArrayList의 경우

먼저 ArrayList의 경우 `add()` 메서드를 이용해서 요소를 추가한다.

``` java
// 요소 추가
ArrayList<Integer> integerArrayList = new ArrayList<Integer>();
integerArrayList.add(29);

// public class ArrayList<E>
public boolean add(E e) {
	modCount++;
	add(e, elementData, size);
	return true;
}

private void add(E e, Object[] elementData, int s) {
    if (s == elementData.length)
        elementData = grow();
    elementData[s] = e;
    size = s + 1;
}

```

구현 내용을 확인해보면 파라미터가 새롭게 추가할 요소(`e`), 기존 배열(`elementData`), 배열의 사이즈(`s`)이다. 그리고 만약 현재 배열에 있는 요소의 개수(`elementData.length`)가 배열 사이즈와 같다면 더이상 요소를 추가할 공간이 없기 때문에 `grow()` 메서드를 통해 배열의 크기를 키워준다.

그러면 `grow()`메서드는 어떻게 동작할까?

``` java
private Object[] grow(int minCapacity) {
	int oldCapacity = elementData.length;
	if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
		int newCapacity = ArraysSupport.newLength(oldCapacity,
			minCapacity - oldCapacity, /* minimum growth */
			oldCapacity >> 1           /* preferred growth */);
	return elementData = Arrays.copyOf(elementData, newCapacity);
		} else {
	return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
	}
}

private Object[] grow() {
	return grow(size + 1);
}
```

코드를 살펴보면, 두번째에 있는 `grow()` 메서드가 위에서 호출한 메서드이다. 매개변수로 현재 배열 사이즈 + 1을 넘겨준다. 그러면 내부적으로 새로운 크기롤 계산해서 더 큰 배열을 만들어서 기존 배열을 복사해서 넣는다.(`return elementData = Arrays.copyOf(elementData, newCapacity);`)




- LinkedList의 경우

반면 LinkedList도 `add()` 메서드롤 요소를 추가한다. 그러나 내부 구현 내용은 다르다.

``` java
LinkedList<Integer> integerLinkedList = new LinkedList<Integer>();
integerLinkedList.add(39);

// public class LinkedList<E>
public boolean add(E e) {
	linkLast(e);
	return true;
}

/**
 * Links e as last element.
 */
void linkLast(E e) {
	final Node<E> l = last;
	final Node<E> newNode = new Node<>(l, e, null);
	last = newNode;
	if (l == null)
		first = newNode;
	else
		l.next = newNode;
	size++;
	modCount++;
}

```

LinkedList의 경우 마지막 요소를 새로운 요소로 바꿔주고(`last = newNode;`) size만 증가시켜주면 된다(`size++;`).

내부 로직을 확인했을때 요소를 추가할 때는 LinkedList가 더 유리하다.

### 조회하기
- ArrayList의 경우

arrayList는 `get()` 메서드에 원하는 인덱스 번호를 매개변수로 하여 원하는 요소를 찾는다.

``` java
integerArrayList.get(1);

// public class ArrayList<E>
public E get(int index) {
	Objects.checkIndex(index, size);
	return elementData(index);
}

E elementData(int index) {
	return (E) elementData[index];
}
```

ArrayList 객체는 내부적으로 배열을 가지고 있다. 배열은 연속적인 데이터 집합이므로 인덱스를 통해 random access가 가능하다. 그러므로 바로 원하는 위치의 값을 가져올 수 있으며 이 경우 시간 복잡도는 O(1)이다.




- LinkedList의 경우

LinkedList 역시 `get()` 메서드를 사용한다.

``` java
integerLinkedList.get(1);


// public class LinkedList<E>
public E get(int index) {
	checkElementIndex(index);
	return node(index).item;
}

Node<E> node(int index) {
	// assert isElementIndex(index);

	if (index < (size >> 1)) {
		Node<E> x = first;
		for (int i = 0; i < index; i++)
			x = x.next;
		return x;
	} else {
		Node<E> x = last;
		for (int i = size - 1; i > index; i--)
			x = x.prev;
		return x;
	}
}
```

반면 LinkedList는 내부에 배열이 없다. 대신 처음과 끝을 나타내는 Node만 있다. 각 Node들은 자신의 값과 자신의 다음 Node, 이전 Node만 알고 있다. 이러한 특징 때문에 비연속적으로 데이터가 저장된다.

그렇기 때문에 원하는 순서(index)의 값을 찾기 위해서는 일일히 반복문을 통해 모든 요소에 접근해야 한다. 따라서 시간복잡도는 O(n)이다.

따라서 이런 경우 조회 속도는 당연히 ArrayList가 빠를 것이다.

# 정리
- 앞으로 array는 배열로, list는 linked list로 이해해야겠다.
- array는 연속적으로 데이터를 저장하며
- list는 비연속적으로 데이터를 저장한다.
- 이러한 특징 때문에 각 연산의 시간복잡도가 달라지게 된다.

