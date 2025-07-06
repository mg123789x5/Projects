#LeetCode

# Java Data Structures

## 🔹 int[] — массив целых чисел
```java
int[] nums = new int[]{1, 2, 3};
int len = nums.length;
Arrays.sort(nums); // сортировка
```

## 🔹`ArrayList<T>` — динамический список

```java
List<Integer> list = new ArrayList<>();
list.add(42);
list.get(0);
list.size();
list.remove(1);
Collections.sort(list);
```
##### Преобразования:
```java
list.toArray();
Arrays.asList(arr); // из массива
```

## 🔹 `LinkedList<T>` — очередь, стек
```java
LinkedList<Integer> queue = new LinkedList<>();
queue.add(1);      // в конец
queue.addFirst(2); // в начало
queue.remove();
queue.getFirst();
```

## 🔹 `Stack<T>` — стек (LIFO)
```java
Stack<Character> stack = new Stack<>();
stack.push('(');
stack.pop();
stack.peek();
stack.isEmpty();
```

## 🔹 `Queue<T>` через LinkedList
```java
Queue<Integer> q = new LinkedList<>();
q.offer(1);
q.poll();
q.peek();
```

## 🔹 `Deque<T>` — двухсторонняя очередь
```java
Deque<Integer> deque = new ArrayDeque<>();
deque.addFirst(1);
deque.addLast(2);
deque.removeFirst();
deque.removeLast();
```

## 🔹 `HashMap<K, V>` — словарь
```java
Map<Character, Integer> map = new HashMap<>();
map.put('a', 1);
map.get('a');
map.containsKey('a');
map.remove('a');
map.keySet();
map.values();
```
##### Счётчик:
```java
map.put(ch, map.getOrDefault(ch, 0) + 1);
```

## 🔹 `HashSet<T>` — множество уникальных элементов
```java
Set<Integer> set = new HashSet<>();
set.add(42);
set.contains(42);
set.remove(42);
```

## 🔹 `StringBuilder` — быстрая сборка строки
```java
StringBuilder sb = new StringBuilder();
sb.append("abc");
sb.reverse();
sb.toString();
sb.deleteCharAt(2);
```

## 🔹 `char[]` — массив символов
```java
char[] chars = s.toCharArray();
```

## 🔹 `Arrays` и `Collections`
```java
Arrays.sort(arr);
Collections.sort(list);
Collections.reverse(list);
Collections.frequency(list, 3);
```

## 🔹 `Комбинации`
-  `Map<Character, Integer>` — счётчик букв
- `Set<Integer>` — уникальные индексы
- `Deque<Integer>` — скользящие окна
- `ArrayList<int[]>` — пары и интервалы
- `List<List<Integer>>` — матрицы и результаты

---

## Какие методы самые важные и используются чаще всего?

В Java есть ключевые методы, которые активно применяются в программировании. Их можно разделить на общие методы Object, методы строк, коллекций и потоков.  

🟠Важные методы **Object** (базовые для всех классов)  

Каждый класс в Java наследует Object, поэтому эти методы важны для любой программы. 
```java
class Person {
    String name;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return name.equals(person.name);
    }

    @Override
    public int hashCode() {
        return name.hashCode();
    }
}
```

🟠Важные методы **String** (работа со строками)  
Строки в Java неизменяемы, поэтому методы создают новые объекты.  
```java
String text = "  Hello, Java!  ";
System.out.println(text.trim().toUpperCase()); // "HELLO, JAVA!"
```

Важные методы **List**, **Set**, **Map** (коллекции)  
```java
List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
System.out.println(names.get(0)); // Alice
```
```java
Set<String> uniqueNames = new HashSet<>();
uniqueNames.add("Alice");
uniqueNames.add("Alice");
System.out.println(uniqueNames.size()); // 1 (дубликат не добавился)
```

🚩Методы **Map** (**HashMap**, **TreeMap**)
```java
Map<String, Integer> ages = new HashMap<>();
ages.put("Alice", 25);
ages.put("Bob", 30);
System.out.println(ages.get("Alice")); // 25
```

**Stream API** позволяет работать с данными декларативно.  
```java
List<String> names = List.of("Alice", "Bob", "Charlie");

names.stream()
     .filter(name -> name.startsWith("A"))
     .map(String::toUpperCase)
     .forEach(System.out::println);
```

---







