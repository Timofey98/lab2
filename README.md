# Лабораторная работа 2

## Задание
1. Сделать проект с помощью maven. Добавить в проект зависимость и продемонстрировать её работу
2. Добавить jar библиотеку в локальный репозиторий maven. Подключить ее как зависимость и продемонстрировать работу.
3. Сделать родительский проект и два дочерних с помощью maven. Один дочерний зависим от другого.

## Разработка

### Задание 1

В проект добавили библиотеку commons-lang3, предоставляющую статические методы для выполнения стандартных задач (например расширение массива и добавление в него дополнительного элемента)

[``Pom``](task1/pom.xml)
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.company</groupId>
    <artifactId>task1</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.9</version>
        </dependency>
    </dependencies>

</project>
```

Класс [``Main``](task1/src/main/java/com/company/Main.java), который выводит "Hello world!", при помощи методов библиотеки commons-lang3: 

```java
public class Main {
    public static void main(String[] argv) {
        String[] phrase = {"Hello"};
        phrase = add(phrase, " ");
        phrase = add(phrase, "world!");
        for (String word : phrase) {
            System.out.print(word);
        }
        System.out.println();
    }
}
```

### Задание 2

В локальном репозитории создали jar библиотеку с классом [``LocalStringClass``](task2/LocalString/src/main/java/com/company/LocalStringClass.java)

```java
public class LocalStringClass {
	public static String str = "Привет из локального репозитория";
}
```
Создали отдельный проект с классом [``ProjectMaven``](task2/ProjectMaven/src/main/java/com/company/ProjectMaven.java), где подключили зависимость из локального репозитория. в результате чего программа выводит строку "Привет из локального репозитория".

```java
import com.company.LocalStringClass;

public class ProjectMaven {
    public static void main(String[] args) {
        System.out.println(LocalStringClass.str);
    }
}
```

[``pom``](task2/ProjectMaven/pom.xml) для проекта ProjectMaven
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.company</groupId>
    <artifactId>ProjectMaven</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>LocalString</artifactId>
            <version>${project.version}</version>
        </dependency>
    </dependencies>

</project>
```

### Задание 3
Сделать родительский проект и два дочерних с помощью maven. Один дочерний зависим от другого.

Сделали родительский проект, в котором подключили зависимость commons-lang3.

[``Pom``](task3/Parent/pom.xml) родительского проекта
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.company</groupId>
    <artifactId>Parent</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>

    <modules>
        <module>FirstChild</module>
    </modules>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.9</version>
        </dependency>
    </dependencies>

</project>
```

[``Pom``](task3/Parent/FirstChild/pom.xml) первого дочернего проекта
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>Parent</artifactId>
        <groupId>com.company</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>
    <packaging>pom</packaging>

    <modules>
        <module>SecondChild</module>
        <module>SecondChild</module>
    </modules>

    <groupId>com.company</groupId>
    <artifactId>FirstChild</artifactId>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

</project>
```

[``Pom``](task3/Parent/FirstChild/SecondChild/pom.xml) второго дочернего проекта
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>FirstChild</artifactId>
        <groupId>com.company</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>SecondChild</artifactId>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

</project>
```

Во втором дочернем проекте создали класс [``SecondChild``](task3/Parent/FirstChild/SecondChild/src/main/java/com/company/SecondChild.java), использующий библеотеку подключенную в родительском проекта
```java
import static org.apache.commons.lang3.ArrayUtils.*;

public class SecondChild {

    public static void main(String[] argv) {
        String[] phrase = {"Hello"};
        phrase = add(phrase, " ");
        phrase = add(phrase, "World!");
        for (String word : phrase) {
            System.out.print(word);
        }
        System.out.println();
    }

}
```

### Вывод 
В результате выполнения лабораторной работы получили практические навыки работы с maven. 
Научились: 
* создавать с помощью maven новые проекты;
* подключать внешние и локальные зависимости;
* создавать зависимости между родительскими и дочерними проектами.

## Работа выполнена

[Ащеулов Михаил ПИМ-21](https://github.com/VergiliusAW)

[Кравцов Тимофей ПИМ-21](https://github.com/Timofey98)
