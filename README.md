# 42-CPP-Piscines
### CPP_00-ex00
Можно юзать системную штуку (char)toupper

```
#include <iostream>

int main()
{
	std::cout << "Hello World!" << std::endl;
	return 0;
}
```
Чтобы читать или писать в стандартный поток ввода/вывода необходимо включить <iostream>
Любая переменная или объект, существующий в стандартной библиотеке C++, включается в стандартное пространство имен std.
Чтобы использовать стандартный объект вывода, вам нужно написать std::cout, чтобы присоединить [пространство имен](https://docs.microsoft.com/ru-ru/cpp/cpp/namespaces-cpp?view=msvc-170)
Пространство имен означает принадлежность определенному объекту или функции. Когда объект объявлен в a() и b(), это означает, что он может использоваться только с другим префиксом.\
abc a::abc b::abc
```
int doSomething(int x, int y)
{
	return x + y;
}
```
```
int doSomething(int x, int y)
{
	return x - y;
}
```
```
#include <iostream>
int main()
{
	std::cout << doSomething(4, 3);
	return 0;
}
Если эти два параметра включены в одну и ту же программу, что и в примере, возникает конфликт имен, поскольку функция с тем же именем и параметром находится в той же области.
```
```
namespace Foo
{
	int doSomething(int x, int y)
	{
		return x + y;
	}
}
```
```
namespace Goo
{
	int doSomething(int x, int y)
	{
		return x - y;
	}
}
```
```
#include <iostream>

int main()
{
	std::cout << Foo::doSomething(4, 3); << '\n'
	std::cout << Goo::doSomething(4, 3); << endl
	return 0;
}
Если над проектом работают два разработчика и они оба решили назвать свою функцию doSomething, то при сборке будет больно. Поэтому можно использовать пространство имён. Для этого используем namespace Foo и namespace Goo. Таким образом обе функции doSomething не будут ничего знать друг о друге и всё скомпилируется успешно.
Оператор разрешения области действия (::) необходим для поиска идентификаторов в каждом пространстве имен.
Чтобы использовать этот оператор, добавьте к имени идентификатора префикс пространства имен, которое будет использоваться.
```
