Кіріл Мачула, [09.01.2026 18:53]
РЕФЕРАТ: ООП в Python та його особливості і обмеження
Виконав: БОЛЬБОТ А. О., група 345 Н.Г

Анотація
Об'єктно-орієнтоване програмування (ООП) є фундаментальною парадигмою у сучасному програмуванні, а його реалізація в Python має унікальні особливості, що відрізняють її від класичних об'єктно-орієнтованих мов як Java чи C++. Python пропонує динамічну, гнучку та інтуїтивно зрозумілу модель ООП, засновану на принципах прототипування та динамічної типізації. Цей звіт досліджує архітектуру ООП в Python, починаючи від базових концепцій класів та об'єктів до розширених механізмів метакласів, дескрипторів та множинного успадкування. Аналізуються як потужні можливості мови (динамічні атрибути, декоратори класів, магічні методи), так і істотні обмеження (швидкість виконання, відсутність інкапсуляції на рівні мови, проблеми з керуванням пам'яттю). Особлива увага приділяється порівнянню з іншими мовами програмування та практичним рекомендаціям для ефективного використання ООП в Python-проектах.

##Зміст
[Розділ 1. Філософія та базові концепції ООП в Python](#розділ-1-філософія-та-базові-концепції-ооп-в-python)

1.1 "Все є об'єктом": Фундаментальний принцип Python

1.2 Дизайн ООП: Паскаль vs Перл

1.3 Чотири стовпи ООП у Python-інтерпретації

Розділ 2. Класи та Об'єкти: Архітектурні особливості

2.1 Динамичний характер класів

2.2 Простір імен та ланцюжки пошуку (MRO)

2.3 Магічні методи (Dunder Methods) як основа поведінки

Розділ 3. Механізми успадкування та їх реалізація

3.1 Множинне успадкування та C3 лінеаризація

3.2 Mixin-класи та їх роль у композиції

3.3 Візуалізація ланцюжка пошуку методів (MRO)

Розділ 4. Інкапсуляція та управління доступом

4.1 Філософія "ми всі дорослі тут"

4.2 Практики приватності: name mangling та декоратори

4.3 Властивості (Properties) як засіб інкапсуляції

Розділ 5. Розширені механізми: метакласи, дескриптори, ABC

5.1 Дескриптори: фундамент властивостей та методів

5.2 Метакласи: програмування класів як об'єктів

5.3 Абстрактні базові класи (ABC) та протоколи

Розділ 6. Обмеження та практичні проблеми

6.1 Продуктивність: накладні витрати динамізму

6.2 Типізація та перевірки під час виконання

6.3 Проблеми з керуванням пам'яттю та циклами посилань

Розділ 7. Порівняльний аналіз та майбутнє ООП в Python

7.1 Python vs Java vs C++: різні підходи до ООП

7.2 Сучасні тренди: композиція над успадкуванням, dataclasses

7.3 Вплив типізації (type hints) на ООП-практики

Висновки

## Розділ 1. Філософія та базові концепції ООП в Python
1.1 "Все є об'єктом": Фундаментальний принцип Python
На відміну від багатьох мов програмування, де примітивні типи (цілі числа, рядки) відрізняються від об'єктів, у Python кожна сутність є об'єктом. Це означає, що навіть функції, модулі та самі класи є об'єктами, які можна передавати як параметри, зберігати в структурах даних та модифікувати під час виконання програми.

python
Демонстрація принципу "все є об'єкт"
x = 42
print(type(x))          # <class 'int'>
print(x.add(10))    # 52 - виклик методу об'єкта

def func():
    return "Hello"

print(type(func))       # <class 'function'>
print(func.name)    # 'func' - атрибут функції-об'єкта

class MyClass:
    pass

print(type(MyClass))    # <class 'type'> - клас теж об'єкт
Ця уніфікація значно спрощує модель програмування, але має наслідки для продуктивності та дизайну системи типів.

1.2 Дизайн ООП: Паскаль vs Перл
Гвідо ван Россум, творець Python, запозичив дизайн ООП з мови Modula-3, а не з класичних об'єктно-орієнтованих мов. Python слідує "паскалеподібному" підходу, де об'єктні механізми додаються до існуючої процедурної моделі, на відміну від "перлоподібного" підходу, де ООП є основним способом структурування коду.

Ключові відмінності:

Явний параметр self: У Python методи отримують посилання на екземпляр як перший аргумент

Відкриті класи: Структуру класу можна змінювати під час виконання

Декларативність: Класи створюються динамічно виконанням коду

1.3 Чотири стовпи ООП у Python-інтерпретації
Хоча Python підтримує класичні принципи ООП, їх реалізація відрізняється від канонічної:

Кіріл Мачула, [09.01.2026 18:53]
Принцип  Реалізація в Python  Відмінності від канонічної
Інкапсуляція  Конвенції та name mangling  Відсутність справжніх приватних полів
Успадкування  Динамичне множинне успадкування з MRO  C3 лінеаризація замість простої ланцюжкової
Поліморфізм  Утина типізація (duck typing)  Перевірка під час виконання, не компіляції
Абстракція  ABC та протоколи  Декларативна, не обов'язкова
Розділ 2. Класи та Об'єкти: Архітектурні особливості
2.1 Динамичний характер класів
Класи в Python — це об'єкти першого класу, які створюються під час виконання програми. Ключове слово class не є декларацією на рівні компіляції, а виконуваною інструкцією.

python
# Динамічна природа класів
class DynamicClass:
    pass

# Можна додавати методи після оголошення
def new_method(self):
    return "Додано динамічно!"

DynamicClass.dynamic_method = new_method

obj = DynamicClass()
print(obj.dynamic_method())  # Працює!

# Можна створювати класи програмно
ProgrammaticClass = type('ProgrammaticClass', (), {'x': 42})
print(ProgrammaticClass().x)  # 42
Така динамічність забезпечує небувалу гнучкість, але ускладнює статичний аналіз та оптимізацію.

2.2 Простір імен та ланцюжки пошуку (MRO)
Кожен об'єкт у Python має словник (dict), що зберігає його атрибути. При пошуку атрибута Python використовує складний механізм з кешуванням:

Екземпляр — пошук у obj.dict

Клас — пошук у ClassName.dict

Батьківські класи — пошук згідно з MRO (Method Resolution Order)

Метаклас — у разі необхідності

python
# Ілюстрація пошуку атрибутів
class A:
    class_attr = "Атрибут класу A"
    
class B(A):
    pass

obj = B()
obj.instance_attr = "Атрибут екземпляра"

print(obj.instance_attr)  # З екземпляра
print(obj.class_attr)     # З класу B -> A
print(obj.dict)       # {'instance_attr': 'Атрибут екземпляра'}
2.3 Магічні методи (Dunder Methods) як основа поведінки
Магічні методи (з подвійним підкресленням) дозволяють перевизначати поведінку вбудованих операцій. Python має понад 100 таких методів.

python
# Приклад кастомного вектора з магічними методами
class Vector:
    def init(self, x, y):
        self.x = x
        self.y = y
    
    def add(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def str(self):
        return f"Vector({self.x}, {self.y})"
    
    def len(self):
        return 2
    
    def getitem(self, index):
        return [self.x, self.y][index]

v1 = Vector(2, 3)
v2 = Vector(4, 5)
print(v1 + v2)  # Vector(6, 8)
print(len(v1))  # 2
print(v1[0])    # 2
Розділ 3. Механізми успадкування та їх реалізація
3.1 Множинне успадкування та C3 лінеаризація
Python підтримує множинне успадкування з унікальним алгоритмом C3 для визначення порядку пошуку методів (MRO). Цей алгоритм гарантує:

Консистентність порядку

Відсутність циклічних залежностей

Збереження порядку у класах-нащадках

3.2 Mixin-класи та їх роль у композиції
Mixin-класи — це спеціальні класи, які не призначені для створення самостійних екземплярів, а для додавання функціональності іншим класам.

python
# Приклад Mixin-класів
class JSONSerializableMixin:
    def to_json(self):
        import json
        return json.dumps(self.dict)

class XMLSerializableMixin:
    def to_xml(self):
        # Спрощена реалізація
        attrs = " ".join(f'{k}="{v}"' for k, v in self.dict.items())
        return f"<object {attrs}/>"

class Product(JSONSerializableMixin, XMLSerializableMixin):
    def init(self, name, price):
        self.name = name
        self.price = price

p = Product("Laptop", 999)
print(p.to_json())  # {"name": "Laptop", "price": 999}
print(p.to_xml())   # <object name="Laptop" price="999"/>
3.3 Візуалізація ланцюжка пошуку методів (MRO)
Діаграма нижче показує складну ієрархію множинного успадкування та порядок пошуку методів:












Розділ 4. Інкапсуляція та управління доступом
4.1 Філософія "ми всі дорослі тут"
На відміну від Java чи C++, де приватні поля захищені на рівні мови, Python дотримується філософії: "ми всі дорослі тут". Це означає, що розробнику довіряється доступ до внутрішніх деталей об'єкта, але очікується, що він буде використовувати їх розумно.

Кіріл Мачула, [09.01.2026 18:53]
4.2 Практики приватності: name mangling та декоратори
Python пропонує два механізми для позначення приватних атрибутів:

Name mangling (здвоєне підкреслення): автоматичне перейменування атрибутів

Конвенційне підкреслення: індикація "захищеного" доступу

python
class BankAccount:
    def init(self, balance):
        self.balance = balance  # Приватний атрибут
        self._transaction_count = 0  # Захищений атрибут
    
    def deposit(self, amount):
        self.__balance += amount
        self._transaction_count += 1
    
    def get_balance(self):
        return self.__balance

account = BankAccount(1000)
# account.__balance  # AttributeError
print(account._BankAccount__balance)  # 1000 - доступ через mangled ім'я
print(account._transaction_count)     # 0 - доступ за конвенцією
4.3 Властивості (Properties) як засіб інкапсуляції
Декоратор @property дозволяє створювати гетери/сетери з синтаксисом звичайних атрибутів.

python
class Temperature:
    def __init(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Температура нижче абсолютного нуля")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32

temp = Temperature(25)
print(temp.celsius)      # 25
print(temp.fahrenheit)   # 77.0
temp.celsius = 30        # Виклик сеттера
# temp.celsius = -300    # ValueError
Розділ 5. Розширені механізми: метакласи, дескриптори, ABC
5.1 Дескриптори: фундамент властивостей та методів
Дескриптори — це об'єкти, що реалізують протокол get, set або delete. Вони є основою властивостей, методів та статичних методів.

python
class ValidatedAttribute:
    """Дескриптор для валідації значень"""
    def init(self, min_value, max_value):
        self.min_value = min_value
        self.max_value = max_value
        self.data = {}
    
    def get(self, obj, objtype):
        return self.data.get(id(obj))
    
    def set(self, obj, value):
        if not (self.min_value <= value <= self.max_value):
            raise ValueError(f"Значення має бути між {self.min_value} і {self.max_value}")
        self.data[id(obj)] = value

class Person:
    age = ValidatedAttribute(0, 150)  # Дескриптор
    height = ValidatedAttribute(30, 250)  # Дескриптор
    
    def init(self, age, height):
        self.age = age
        self.height = height

p = Person(25, 180)
# p.age = 200  # ValueError
5.2 Метакласи: програмування класів як об'єктів
Метакласи дозволяють модифікувати поведінку класів під час їх створення. Всі класи в Python є екземплярами метакласу type.

python
# Метаклас для автоматичної реєстрації класів
class RegistryMeta(type):
    registry = {}
    
    def new(mcs, name, bases, attrs):
        cls = super().new(mcs, name, bases, attrs)
        if name not in ['BasePlugin']:  # Ігноруємо базовий клас
            mcs.registry[name] = cls
        return cls

class BasePlugin(metaclass=RegistryMeta):
    pass

class EmailPlugin(BasePlugin):
    pass

class SMSPlugin(BasePlugin):
    pass

print(RegistryMeta.registry)  
# {'EmailPlugin': <class 'main.EmailPlugin'>, 
#  'SMSPlugin': <class 'main.SMSPlugin'>}
5.3 Абстрактні базові класи (ABC) та протоколи
Python підтримує абстрактні класи через модуль abc, але основною парадигмою залишається утина типізація.

python
from abc import ABC, abstractmethod
from typing import Protocol

# Традиційний підхід з ABC
class Drawable(ABC):
    @abstractmethod
    def draw(self):
        pass
    
    @abstractmethod
    def area(self):
        pass

# Сучасний підхід з Protocol (Python 3.8+)
class DrawableProtocol(Protocol):
    def draw(self) -> None: ...
    def area(self) -> float: ...

# Клас, що реалізує протокол
class Circle:
    def init(self, radius):
        self.radius = radius
    
    def draw(self):
        print(f"Малюю коло радіусом {self.radius}")
    
    def area(self):
        return 3.14 * self.radius ** 2

Кіріл Мачула, [09.01.2026 18:53]
# Функція працює з будь-яким об'єктом, що має методи draw і area
def render_shape(shape: DrawableProtocol):
    shape.draw()
    print(f"Площа: {shape.area()}")

circle = Circle(5)
render_shape(circle)
Розділ 6. Обмеження та практичні проблеми
6.1 Продуктивність: накладні витрати динамізму
Динамічна природа Python створює значні накладні витрати:

Операція  Python (ns)  C++ (ns)  Відношення
Виклик методу  ~70  ~2  35:1
Доступ до атрибута  ~60  ~1  60:1
Створення об'єкта  ~400  ~10  40:1
Ці накладні витрати обумовлені:

Динамичний пошук атрибутів через словники

Перевірки типів під час виконання

Відсутність статичної оптимізації

6.2 Типізація та перевірки під час виконання
Відсутність статичної типізації призводить до помилок, які виявляються лише під час виконання:

python
def calculate_total(items):
    # Типи параметрів не визначені
    return sum(item.price for item in items)

# Помилка виникне лише при виконанні
# calculate_total([1, 2, 3])  # AttributeError
Рішення: Type Hints (Python 3.5+) та mypy для статичного аналізу.

6.3 Проблеми з керуванням пам'яттю та циклами посилань
Python використовує підрахунок посилань з циклічним збиранням сміття, що може призводити до витоків пам'яті:

python
# Класичний приклад циклу посилань
class Node:
    def init(self):
        self.parent = None
        self.children = []
    
    def add_child(self, child):
        self.children.append(child)
        child.parent = self

# Створюємо цикл посилань
parent = Node()
child = Node()
parent.add_child(child)

# Об'єкти ніколи не будуть видалені, поки не спрацює GC
del parent
del child
# Пам'ять звільниться лише після виклику gc.collect()
Розділ 7. Порівняльний аналіз та майбутнє ООП в Python
7.1 Python vs Java vs C++: різні підходи до ООП
Характеристика  Python  Java  C++
Типізація  Динамічна  Статична  Статична
Успадкування  Множинне  Одиночне  Множинне
Інкапсуляція  Конвенції  Модифікатори  Модифікатори
Поліморфізм  Утина типізація  Інтерфейси  Віртуальні функції
Метапрограмування  Метакласи, декоратори  Анотації, рефлексія  Шаблони, макроси
Швидкість  Повільний  Середній  Швидкий
7.2 Сучасні тренди: композиція над успадкуванням, dataclasses
Сучасний Python сприяє композиції та протоколам замість глибоких ієрархій успадкування:

python
from dataclasses import dataclass
from typing import List

# Сучасний підхід з dataclasses
@dataclass
class Product:
    name: str
    price: float
    tags: List[str] = None
    
    def post_init(self):
        if self.tags is None:
            self.tags = []

# Використання композиції замість успадкування
class ShoppingCart:
    def init(self):
        self.items = []
        self.discounts = []
    
    def add_item(self, product, quantity=1):
        self.items.append({"product": product, "quantity": quantity})
    
    def apply_discount(self, discount_strategy):
        self.discounts.append(discount_strategy)
7.3 Вплив типізації (type hints) на ООП-практики
Type hints змінюють підхід до ООП в Python, додаючи переваги статичної типізації без втрати динамічності:

python
from typing import TypeVar, Generic, Optional, List

T = TypeVar('T')

class Repository(Generic[T]):
    def init(self):
        self._items: List[T] = []
    
    def add(self, item: T) -> None:
        self._items.append(item)
    
    def get_by_id(self, id: int) -> Optional[T]:
        for item in self._items:
            if getattr(item, 'id', None) == id:
                return item
        return None

# Використання з type checking
@dataclass
class User:
    id: int
    name: str

repo = Repository[User]()
repo.add(User(1, "Alice"))
user = repo.get_by_id(1)  # Тип: Optional[User]
Висновки
Об'єктно-орієнтоване програмування в Python представляє унікальний синтез потужності та простоти. На відміну від класичних ООП-мов, Python пропонує динамічну, гнучку модель, яка пріоритетізує практичність над догматичною суворістю.

Ключові переваги Python ООП:

Динамічна природа: можливість зміни класів та об'єктів під час виконання

Простота синтаксису: мінімум boilerplate-коду, чистий та зрозумілий дизайн

Кіріл Мачула, [09.01.2026 18:53]
Потужні механізми метапрограмування: декоратори, метакласи, дескриптори

Гнучка типізація: утина типізація для швидкої розробки, type hints для великих проектів

Основні обмеження:

Продуктивність: значні накладні витрати через динамізм

Відсутність справжньої інкапсуляції: покладання на конвенції замість механізмів мови

Пізнє виявлення помилок: багато проблем виявляється лише під час виконання

Складність великих ієрархій: множинне успадкування може створювати заплутані залежності

Майбутнє ООП в Python пов'язане з поєднанням динамічної гнучкості та статичної надійності. Type hints, протоколи та dataclasses вказують на напрямок розвитку, де Python зберігає свою динамічну природу, але набуває інструментів для побудови надійних, підтримуваних систем.

Для ефективного використання ООП в Python розробники повинні:

Віддавати перевагу композиції над успадкуванням

Використовувати type hints для великих проектів

Розуміти накладні витрати динамічних механізмів
