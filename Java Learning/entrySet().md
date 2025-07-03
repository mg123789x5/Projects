#LeetCode

**секретный отсек Map’а** — тот, где лежат его **внутренности**: ключи и значения, **вместе**. 

---
## 💡 Что такое `entrySet()`?

Когда ты создаёшь `Map<Character, Integer>` —  
ты как будто говоришь:
> «У меня есть ящик, где каждый **ключ** связан с каким-то **значением**.»

Но как пройтись по этим парам _вместе_?  
Вот тут приходит на помощь:

```java
for (Map.Entry<Character, Integer> entry : map.entrySet()) { ... }
```

---

### 🧱 `entrySet()` возвращает **набор всех пар** `ключ → значение`:

```java
Map<Character, Integer> map = new HashMap<>();
map.put('a', 3);
map.put('b', 2);
map.put('c', 1);
```

```java
Set<Map.Entry<Character, Integer>> set = map.entrySet();
```

Теперь `set` содержит:
- `('a', 3)`    
- `('b', 2)`    
- `('c', 1)`    

---

## 🤔 Зачем `List<Map.Entry<...>>`?

Ты не можешь **отсортировать `Set` напрямую**.  
Но ты можешь превратить его в список:

```java
List<Map.Entry<Character, Integer>> list = new ArrayList<>(map.entrySet());
```

Теперь `list` содержит все пары — и ты можешь:

✅ отсортировать:

```java
list.sort((a, b) -> b.getValue() - a.getValue());
```

✅ достать по индексу:

```java
list.get(0) → самый частый  
list.get(1) → второй по частоте
```

---

## 🧠 А что такое `Map.Entry<...>`?

Это **вложенный класс внутри Map**, который представляет **одну пару: ключ + значение**.

Внутри него есть:

- `getKey()` → получить ключ
    
- `getValue()` → получить значение
    

---

## 🔧 Пример:

```java
Map<Character, Integer> freq = new HashMap<>();
// Заполнили карту...

List<Map.Entry<Character, Integer>> list = new ArrayList<>(freq.entrySet());
list.sort((a, b) -> b.getValue() - a.getValue());  // по убыванию

if (list.size() >= 2) {
    char second = list.get(1).getKey();
    return second;
}
```

---




