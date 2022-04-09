# Задача "Понимание JVM"

```Java
public class JvmComprehension {

    public static void main(String[] args) {
    	int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```

## Этапы работы JVM:

  ### 1. Загрузка классов (ClassLoaders)
  Осуществляются загрузчиками классов
  Существуют три основных встроенных загрузчика:
  * Application class loader. Загружает классы приложений
  * Platform class loader. Загружает выбранные модули Java SE и JDK
  * Bootstrap class loader. Загружает основные модули Java SE и JDK

  Application class loader будет загружен класс JvmComprehension
  Bootstrap class loader будут загруженны классы Object, Integer, System

  ### 2. Связывание (Linking)
  При связывании происходит:
  * Верификация. Проверяет корректность кода
  * Подготовка. Подготавливает статические переменные класса и инициализирует память под значения по умолчанию
  * Разрешение ссылок. Связывает ссылки на другие классы

  ### 3. Инициализация
  Инициализирует переменные класса их правильными начальными значениями

  ### 4. Области памяти
  * область памяти Stack помещаются
  * область памяти Heap помещаются
  * область памяти Metaspace
  
  Класс JvmComprehension и системные классы загружаются в область памяти **Metaspace**

  * В **Stack** создается фрейм метода main()
      * переменная *int i = 1*
      * ссылка *o* на объект Object
      * ссылка *ii* на объект Integer
  * В **Heap** создается объект Object и связывается с переменной *o* метода main()
  * В **Heap** создается объект Integer и связывается с переменной *ii* метода main()
  * В **Stack** создается фрейм метода printAll()
      * ссылка *o* на объект Object
      * переменная *int i = 1*
      * ссылка *ii* на объект Integer
      * ссылка *uselessVar* на объект Integer
  * В **Heap** объект Object связывается с переменной *o* метода printAll()
  * В **Heap** объект Integer связывается с переменной *ii* метода printAll()
  * В **Heap** создается объект Integer и связывается с переменной *uselessVar* метода printAll()
  * В **Stack** создается фрейм метода println()
      * ссылка *o* на объект Object
      * переменная *int i = 1*
      * ссылка *ii* на объект Integer
  * В **Heap** объект Object связывается с переменной *o* метода println()
  * В **Heap** объект Integer связывается с переменной *ii* метода println()
  * В **Heap** создается строка String и связывается с переменной *o* метода println()
  * В **Stack** создается фрейм метода println() 
  * В **Heap** создается строка String и связывается с фреймом метода println()

  ### 5. Выполнение (Execution Engine)
  Выполняется байт-код.
  * Interpreter интерпретирует байт-код в файле строка за строкой
  * JIT Compiler используется для повышения эффективности интерпретатора
  * Garbage Collector Периодически собирает объекты из памяти (Heap), которые больше не используются
    * В Heap существуют следующие зоны:
      - Eden. Зона создания объектов
      - Survivor1, Survivor2. Зоны удаления не используемых ссылок
      - Tenured. Зона регулярно используемых объектов, сдесь хранятся объекты прошедшие сортировку на удаление
    * Для сборки мусора происходит приостановка программы
