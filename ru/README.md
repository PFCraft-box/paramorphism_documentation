# Введение

Paramorphism — это обфускатор для виртуальной Java машины (JVM). В настоящее время он поддерживает такие языки программирования как Java и Kotlin, но теоретически может поддерживать любой JVM язык, который не слишком сильно зависит от рефлексии или [динамической отправки по имени](https://en.wikipedia.org/wiki/Dynamic_dispatch).

## Начало работы

Создав приложение Java в виде файла JAR, вы можете передать его в Paramorphism'у, для обфускации.

Paramorphism работает от переданного ему в качестве аргумента конфиг-файла.

```sh
java -jar paramorphism.jar путь/к/config
```

Paramorphism поддерживает два формата конфиг-файлов, это: YAML и JSON. В данной документации мы будем использовать файлы формата YML в качестве примеров (если у вас имеются какие-то проблемы с данным форматом, мы всегда можете проверить его правильность [здесь](https://yaml-online-parser.appspot.com/)).

Пример конфигурации для базового приложения 'Hello, World!':

```yml
input: path/to/my/helloworld.jar
output: path/to/my/obfuscated-helloworld.jar

# Мы исключаем основной класс HelloWorld от ремаппинга,
# чтобы атрибут 'Main-Class' в MANIFEST.MF не указывал
# на недопустимое имя класса.
remapper:
  mask:
    exclude:
      - com/example/HelloWorld
```

Как видите, мы используем `input`, чтобы указать путь к JAR файлу, который хотим обфусцировать, а `output` используется для указания пути, куда будет сохранен обфусцированный файл. Если параментр `output` не установлен, его значением берется из `input`, а также переименовывает его разширение из `.jar` в `.obf.jar`.

Дополнительные параметры конфигурации указаны в разделе 'Конфигурация' настоящей документации.

## Ограничения

В общем случае, обфускация Java может препятствовать правильному функционированию некоторых методов в программе.

Например, смена имен может нарушать вызовы методов, выполненных при помощи рефлексии, имена элементов которых жестко закодированы.

Например, `Class.forName("com.example.HelloWorldPrinterFactory")` может вернуть исключение `ClassNotFoundException` после обфускации, поскольку `HelloWorldPrinterFactory` будет иметь имя вроде `a.a.c`.

Старайтесь не забывать об этом во время разработки собственного приложения, которое в конечном итоге будет обфусцированно и после, потребует тщательного тестирования вашего приложения.

## Как сообщить о возможных ошибках

Если вы обнаружили какую-либо проблему с обфускатором, сообщите об этом в соответствующий [GitHub репозиторий](https://github.com/SerenityEnterprises/paramorphism-issues/).