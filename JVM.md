# Код для исследования:
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

## Разбор кода
### Загрузка классов

Первостепенно файл .java скомпилируется в байткод (файл получит расширение .class) и затем jvm строчка за строчкой начнет выполнять 
этот код на любой операционной системе (благодаря кроссплатформенности)

Первым делом в работу вступит подсистема загрузки классов.
1. Bootstrap classLoader проверит не является ли представленный класс платформенным (java core)
2. Platform classLoader (.util классы)
3. Application classLoader (прикладные классы)
4. Так же могут быть пользовательские classLoader

Затем наступит очередь этапа связывания
1. verify (верификация)
2. prepare (Подготовка. Выделение памяти)
3. resolve (Разрешение символьных ссылок)

И наконец - инициальзация

### Области памяти

1. В metaSpace запишутся данные о классах - JvmComprehension class и другие системные классы (например String и Object)
2. В heap запишутся объекты класса Object и Integer
3. В стеке создастся фрейм Main(), printAll() и фрейм печати (System.out.println(o.toString() + i + ii)) и (System.out.println("finished")). 
    В Main попадут "int i = 1", переменная "о" с ссылкой на объект Object, переменная "ii" с сылкой на объект Integer.
    В printAll попадет переменная "uselessVar" с ссылкой на объект Integer

### Сборка мусора

Во время сборки мусора приостанавливается работа всей программы.
Для сборки используются 2 основных метода:
    - Подсчет ссылок
    - Обход графа достижимых объектов
Очищается только heap, остальные области памяти не затрагиваются
В данной программе сборка мусора не целесообразна так как программа очень короткая и памяти занимает очень мало.