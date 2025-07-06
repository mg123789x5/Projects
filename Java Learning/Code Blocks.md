


---
## Какие шаги нужно выполнить для прочтения текстового файла?


Для прочтения текстового файла в Java нужно выполнить несколько шагов. 
#### 1) Выбор метода чтения файла
В Java есть несколько способов читать текстовые файлы:
1. Использование класса FileReader.
2. Использование BufferedReader для построчного чтения.
3. Использование java.nio.file.Files для чтения всех строк файла.
4. Применение Scanner для гибкого чтения данных.

#### 2) Открытие файла
Нужно указать путь к файлу (абсолютный или относительный) и передать его в выбранный класс. Например:
```java
FileReader reader = new FileReader("file.txt");
```
#### 3) Чтение данных
Выберите подходящий метод для обработки содержимого файла:
- Построчное чтение.
- Чтение символов.
- Чтение всех строк сразу (например, через `Files.readAllLines`).

#### 4) Закрытие ресурса
Для освобождения системных ресурсов после работы с файлом нужно закрыть поток. Это делается вручную или с помощью конструкции try-with-resources.


🚩Примеры реализации:

Построчное чтение с **BufferedReader**
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class FileReadExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line); // Вывод строки
            }
        } catch (IOException e) {
            System.out.println("Ошибка: " + e.getMessage());
        }
    }
}
```

Чтение файла с помощью **Files**
```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.io.IOException;
import java.util.List;

public class FileReadExample {
    public static void main(String[] args) {
        try {
            List<String> lines = Files.readAllLines(Paths.get("file.txt"));
            for (String line : lines) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Ошибка: " + e.getMessage());
        }
    }
}
```

Чтение через **Scanner**
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class FileReadExample {
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(new File("file.txt"))) {
            while (scanner.hasNextLine()) {
                System.out.println(scanner.nextLine()); // Построчное чтение
            }
        } catch (FileNotFoundException e) {
            System.out.println("Файл не найден: " + e.getMessage());
        }
    }
}
```
---




