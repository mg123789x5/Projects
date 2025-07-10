#projects 

---

| J.Nr. | Teemad:                                                       |
| ----- | ------------------------------------------------------------- |
| 1     | [[_start DevOps \| DevOps]]                                   |
| 2     | [[_start EclipseIDE \| Eclipse IDE]]                          |
| 3     | [[_start Java NetProg\| Java EE, Web]]                        |
| 4     | [[_start Java Learning \| Java Learning]]                     |
| 5     | [[_start JUnit 5 Testing \| JUnit 5 Testing]]                 |
| 6     | [[_start NetWork \| NetWork]]                                 |
| 7     | [[_start PKI with OpenSSL \| PKI with OpenSSL]]               |
| 8     | [[_start Java EE-WebDev \| Java NetProg TCP/IP]]              |
| 9     | [[_start Java MyPhotos \| Java EE - MyPhotos]]                |
| 10    | [[_start Docker и Docker Compose \| Docker и Docker Compose]] |
| 11    | [[_start Self Hosting Guide \| Self Hosting Guide]]           |
|       |                                                               |


---
[AlgoMaster](https://algomaster.io)
>Платформа по тренировке навыков решения задач с LeetCode. Сервис AlgoMaster подходит как новичкам, так и спецам. У каждой задачи есть решение с GitHub или YouTube.Все задачи чётко разделены по темам, да и абсолютно бесплатно.

---
[Learn to Code](https://www.w3schools.com)

---
[EJBCA](https://www.ejbca.org/about/)
>flexibility and robustness. Unlike other open-source certificate authority and PKI solutions, EJBCA is platform-independent and can be scaled up and down to match your needs.

---



10 полезных функций, которые часто пригождаются продвинутым Java-разработчикам


1. Safe Cast с Optional


public static <T> Optional<T> safeCast(Object obj, Class<T> clazz) {
    return clazz.isInstance(obj) ? Optional.of(clazz.cast(obj)) : Optional.empty();
}



2. Таймер производительности (Stopwatch)


public static <T> T measureTime(String label, Supplier<T> supplier) {
    long start = System.nanoTime();
    T result = supplier.get();
    long duration = System.nanoTime() - start;
    System.out.printf("[%s] Duration: %d ms%n", label, duration / 1_000_000);
    return result;
}



3. Deep Copy через сериализацию


@SuppressWarnings("unchecked")
public static <T extends Serializable> T deepCopy(T object) {
    try (
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(baos)
    ) {
        oos.writeObject(object);
        try (
            ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());
            ObjectInputStream ois = new ObjectInputStream(bais)
        ) {
            return (T) ois.readObject();
        }
    } catch (IOException | ClassNotFoundException e) {
        throw new RuntimeException("Deep copy failed", e);
    }
}



4. Рекурсивный поиск файлов с фильтром


public static List<Path> findFiles(Path root, Predicate<Path> filter) throws IOException {
    try (Stream<Path> stream = Files.walk(root)) {
        return stream.filter(Files::isRegularFile).filter(filter).collect(Collectors.toList());
    }
}



5. Универсальный Retry Wrapper


public static <T> T retry(int attempts, Duration delay, Supplier<T> task) {
    for (int i = 1; i <= attempts; i++) {
        try {
            return task.get();
        } catch (Exception e) {
            if (i == attempts) throw e;
            try { Thread.sleep(delay.toMillis()); } catch (InterruptedException ie) { Thread.currentThread().interrupt(); }
        }
    }
    throw new RuntimeException("Retry failed");
}



6. Сериализация Map в query string


public static String toQueryString(Map<String, String> params) {
    return params.entrySet().stream()
        .map(e -> URLEncoder.encode(e.getKey(), StandardCharsets.UTF_8) + "=" +
                  URLEncoder.encode(e.getValue(), StandardCharsets.UTF_8))
        .collect(Collectors.joining("&"));
}



7. Группировка по произвольному ключу


public static <T, K> Map<K, List<T>> groupBy(Collection<T> items, Function<T, K> classifier) {
    return items.stream().collect(Collectors.groupingBy(classifier));
}



8. Метод ожидания условия с таймаутом


public static boolean waitFor(Predicate<Void> condition, Duration timeout, Duration interval) {
    long deadline = System.currentTimeMillis() + timeout.toMillis();
    while (System.currentTimeMillis() < deadline) {
        if (condition.test(null)) return true;
        try { Thread.sleep(interval.toMillis()); } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
    }
    return false;
}



9. Поиск первого ненулевого значения


@SafeVarargs
public static <T> Optional<T> firstNonNull(Supplier<T>... suppliers) {
    for (Supplier<T> supplier : suppliers) {
        T value = supplier.get();
        if (value != null) return Optional.of(value);
    }
    return Optional.empty();
}



10. Конвертация Enum в Map


public static <E extends Enum<E>> Map<String, E> enumMap(Class<E> enumClass) {
    return Arrays.stream(enumClass.getEnumConstants())
                 .collect(Collectors.toMap(Enum::name, Function.identity()));
}




----

 `var` в Java — типы без шума

Java 10 принес var — возможность писать меньше кода без потери типов. Он не делает Java динамической, но помогает писать чище и быстрее.

⚡️ До и после var

📌 Было:
HashMap<String, List<Integer>> map = new HashMap<String, List<Integer>>();

📌 Стало:
var map = new HashMap<String, List<Integer>>();
👉 Код стал вдвое короче, но компилятор по-прежнему знает точный тип.


🔁 Циклы:
var list = List.of("A", "B", "C");

for (var item : list) {
    System.out.println(item);
}
🔍 item — автоматически String. Работает идеально с Map:
var map = Map.of("one", 1, "two", 2);

for (var entry : map.entrySet()) {
    System.out.println(entry.getKey() + " = " + entry.getValue());
}

🔄 Stream API
var names = List.of("john", "jane", "max");

var upper = names.stream()
    .map((var name) -> name.toUpperCase())
    .filter(n -> n.startsWith("J"))
    .toList();
👉 Лаконично, читаемо, с сохранением типовой строгости.

🧪 Использование с методами
var user = getUser(); // Что возвращает getUser?

⚠️ Неочевидно. Лучше явно:
User user = getUser();
или хотя бы:
var user = getUser(); // getUser() → User
✍️ Пиши комментарий, если тип важен для читаемости.

💡 Когда var — 🔥

 Тип очевиден из правой части:
  var url = new URL("https://example.com");
  

Тип слишком громоздкий:
  var map = new HashMap<String, Map<Integer, Set<List<String>>>>();
  

В локальных временных переменных:
  var tmp = Files.readAllLines(Path.of("file.txt"));
  

❌ Когда лучше не использовать
var result = doStuff();
⚠️ Тип неочевиден — хуже читается. Особенно в API, сигнатурах и публичных методах:
// Так нельзя
public var getUser() { ... }

📛 var работает только локально, и только с инициализацией:
var x;           // ❌ ошибка
var list = null; // ❌ ошибка


🗣️ Запомни: var — не фишка ради фишки. Он помогает избавиться от шума там, где типы очевидны. Не злоупотребляй, но используй — особенно в стримах, циклах, дженериках и временных переменных.





