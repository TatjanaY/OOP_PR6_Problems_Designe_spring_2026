# **Вариант № 33**

## **Генеративная художественная композиция "Библиотека с книгой" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей книжной культуры, который хочет получить интерактивную генеративную инсталляцию "Библиотека с книгой". Инсталляция должна создавать уникальные композиции с библиотеками, где здания и книги оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и книги — это "живые" объекты, способные расти, менять цвет от коричневого к золотому и реагировать на взаимодействие, создавая динамичные композиции в стиле библиотечной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
book_count = 6  # количество книг на полках
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от коричневого к золотому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и книг, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

---

## **1. Абстрактный базовый класс архитектурного элемента**

### **Задание**

Создайте абстрактный класс `ArchElement`, который определяет общий интерфейс для всех архитектурных элементов.

Класс должен содержать:
- конструктор для хранения координат (x, y), размера (width, height) и названия элемента
- абстрактный метод `draw(ax)` — отрисовка элемента на переданной оси matplotlib
- метод `transform(scale_x, scale_y)` — масштабирование элемента (возвращает self для цепочечных вызовов)

### **Шаблон кода**

```python
from abc import ABC, abstractmethod

class ArchElement(ABC):
    def __init__(self, x, y, width, height, name):
        # TODO: сохранить координаты, размеры и имя
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.name = name
    
    @abstractmethod
    def draw(self, ax):
        pass
    
    def transform(self, scale_x, scale_y):
        # TODO: изменить width и height с учетом масштаба
        self.width *= scale_x
        self.height *= scale_y
        # TODO: вернуть self
        return self
```

---

## **2. Классы-наследники архитектурных элементов**

### **Задание**

Создайте два класса, наследующихся от `ArchElement`:

- **`Building`** — прямоугольное здание библиотеки с окнами
- **`Book`** — книга (раскрытая или закрытая)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания библиотеки
        # TODO: добавить окна (3 ряда по 4 окна)
        # TODO: добавить книжные полки на фасаде
        pass

class Book(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать книгу (прямоугольник с корешком)
        # TODO: добавить страницы (линии)
        # TODO: добавить название на обложке
        # TODO: использовать градиент от коричневого к золотому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_books_to_building(building, count)` добавляет книги к зданию библиотеки
- метод `render()` отображает всю композицию
- класс должен быть итерируемым (реализовать `__iter__`)

### **Шаблон кода**

```python
class CityCanvas:
    def __init__(self, width, height):
        # TODO: сохранить размеры
        self.width = width
        self.height = height
        # TODO: создать пустой список elements
        self.elements = []
    
    def add_element(self, element):
        # TODO: добавить элемент в список
        self.elements.append(element)
    
    def add_books_to_building(self, building, count):
        # TODO: создать книги на полках здания
        # TODO: равномерно распределить книги по ширине здания
        # TODO: добавить книги в elements
        pass
    
    def render(self):
        # TODO: создать фигуру matplotlib
        # TODO: установить пределы осей (0, width, 0, height)
        # TODO: для каждого элемента вызвать draw()
        # TODO: показать результат
        pass
    
    def __iter__(self):
        # TODO: вернуть итератор по self.elements
        return iter(self.elements)
```

---

## **4. Паттерн Borg для управления цветом**

### **Задание**

Реализуйте класс `ColorTheme`, использующий **паттерн Borg** для хранения цветовой схемы композиции.

**Требования**:
- все экземпляры класса имеют общий словарь `colors`
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от коричневого к золотому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'book', 'shelf')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #8B4513 к #FFD700 (ratio от 0 до 1)

### **Шаблон кода**

```python
class ColorTheme:
    _shared_state = {}
    
    def __init__(self):
        # TODO: установить self.__dict__ = self._shared_state
        self.__dict__ = self._shared_state
        # TODO: если словарь пуст, инициализировать базовые цвета
        if not self._shared_state:
            self.colors = {
                'sky': '#87CEEB',
                'ground': '#8B7355',
                'base_brown': '#8B4513',
                'base_gold': '#FFD700'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от коричневого к золотому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от коричневого к золотому
        # ratio: 0.0 = коричневый, 1.0 = золотой
        from matplotlib.colors import to_rgb, to_hex
        brown = to_rgb('#8B4513')
        gold = to_rgb('#FFD700')
        interpolated = tuple(b + (g - b) * ratio for b, g in zip(brown, gold))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий библиотеки с 6 книгами на полках, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (100, 400), (300, 380), (500, 420)
# - По 2 Book на каждом здании

# Пример создания книги:
# book = Book(building.x + offset, building.y + offset, 
#             30, 40, "Book1")
# canvas.add_element(book)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Circle, Polygon

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада библиотеки
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#654321', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 4)
        window_w, window_h = self.width * 0.12, self.height * 0.12
        for row in range(3):
            for col in range(4):
                wx = self.x + self.width * 0.08 + col * (self.width * 0.22)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#654321')
                ax.add_patch(window)
        # TODO: добавить книжные полки на фасаде
        for i in range(3):
            shelf_y = self.y + self.height * 0.3 + i * (self.height * 0.2)
            ax.plot([self.x + 10, self.x + self.width - 10], 
                   [shelf_y, shelf_y], color='#654321', linewidth=2)

class Book(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 80)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать книгу (прямоугольник с корешком)
        book = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#654321', linewidth=2)
        ax.add_patch(book)
        # TODO: добавить корешок книги
        spine = Rectangle((self.x, self.y), self.width * 0.15, self.height, 
                         facecolor='#654321', edgecolor='#654321')
        ax.add_patch(spine)
        # TODO: добавить страницы (линии)
        for i in range(5):
            page_x = self.x + self.width * 0.2 + i * (self.width * 0.15)
            ax.plot([page_x, page_x], [self.y + 5, self.y + self.height - 5], 
                   color='#F5F5DC', linewidth=1)
        # TODO: добавить название на обложке
        ax.text(self.x + self.width * 0.5, self.y + self.height/2, 
               'BOOK', ha='center', va='center', fontsize=8, color='#654321')
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и книги должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" библиотечной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать книгу с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от коричневого к золотому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления книг к библиотеке, где книги появляются постепенно и открываются, а цветовая динамика подчёркивает процесс "оживления" библиотечного здания.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Book: высота должна быть достаточной для страниц (не менее 25px)

### **Шаблон кода**

```python
class Building(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        # Сохраняем границы холста
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        
        # Приватные атрибуты
        self._x = x
        self._y = y
        self._width = width
        self._height = height
        self.name = name
        self.history = [height]  # история изменений высоты
        self.floors = height // 20  # количество этажей
        self.book_level = 0  # уровень книг
    
    @property
    def x(self):
        # TODO: вернуть _x
        return self._x
    
    @x.setter
    def x(self, value):
        # TODO: проверить тип (int или float)
        # TODO: проверить границы 0..canvas_width
        # TODO: установить _x
        pass
    
    @property
    def height(self):
        # TODO: вернуть _height
        return self._height
    
    @height.setter
    def height(self, value):
        # TODO: проверить тип
        # TODO: проверить value > 0
        # TODO: сохранить предыдущее значение в history
        # TODO: установить _height
        # TODO: обновить self.floors = _height // 20
        pass

class Book(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.is_open = False  # открыта ли книга
        self.page_count = 10  # количество страниц
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 25:  # минимальная высота для страниц
            value = 25
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_books()` — добавляет книги к зданию
- `get_color()` — возвращает цвет в градиенте от коричневого к золотому

**Для Book**:
- `add_book()` — увеличивает размер книги
- `open_book()` — открывает книгу
- `get_color()` — возвращает цвет в градиенте от коричневого к золотому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_books(self):
        # TODO: увеличить book_level
        self.book_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от коричневого к золотому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если book_level > 0: добавить книги
        pass

class Book(ArchElement):
    def add_book(self):
        # TODO: увеличить размер книги
        self.height += 10
        self.width += 5
        self.history.append(self._height)
        return self
    
    def open_book(self):
        # TODO: установить is_open = True
        self.is_open = True
        return self
    
    def get_color(self):
        # TODO: градиент от коричневого к золотому по высоте
        ratio = min(1.0, self._height / 80)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать книгу с учётом is_open
        # TODO: отрисовать обложку, корешок и страницы
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление книг к библиотеке

### **Шаблон кода**

```python
class AnimatedCity:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.elements = []      # список элементов
        self.frames = []         # список кадров (параметры на каждом шаге)
    
    def add_element(self, element):
        # TODO: добавить элемент в список
        self.elements.append(element)
    
    def record_frame(self):
        # TODO: сохранить текущие параметры всех элементов в self.frames
        frame = {
            elem.name: {
                'height': getattr(elem, '_height', elem.height),
                'books': getattr(elem, 'book_level', 0),
                'open': getattr(elem, 'is_open', False)
            }
            for elem in self.elements
        }
        self.frames.append(frame)
    
    def show_frame(self, step, frame_data):
        # TODO: создать фигуру matplotlib
        fig, ax = plt.subplots(figsize=(10, 6))
        ax.set_xlim(0, self.width)
        ax.set_ylim(0, self.height)
        ax.set_facecolor('#87CEEB')  # небо
        
        # TODO: добавить землю
        ax.add_patch(Rectangle((0, 0), self.width, 50, 
                              facecolor='#8B7355', edgecolor='none'))
        
        # TODO: для каждого элемента вызвать draw()
        for elem in self.elements:
            elem.draw(ax)
        
        # TODO: добавить заголовок с номером шага
        ax.set_title(f"Шаг {step}: Библиотека с книгой")
        ax.axis('off')
        
        # TODO: показать фигуру и пауза
        plt.tight_layout()
        plt.pause(0.5)
        plt.close(fig)
    
    def animate(self, interval=1.0):
        # TODO: для каждого кадра в self.frames вызвать show_frame()
        for i, frame in enumerate(self.frames):
            self.show_frame(i, frame)
```

---

## **4. Основной цикл анимации**

### **Задание**

Создайте композицию из 2 зданий и 4 книг. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые книги
- Шаги 4-5: книги растут, открываются
- Шаг 6: финальная композиция с полным градиентом цвета

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание анимированного холста
canvas = AnimatedCity(800, 600)

# TODO: создать элементы:
buildings = [
    Building(100, 150, 120, 80, "B1", 800, 600),
    Building(400, 150, 120, 75, "B2", 800, 600)
]

books = [
    Book(120, 200, 30, 40, "K1", 800, 600),
    Book(170, 200, 30, 40, "K2", 800, 600),
    Book(420, 195, 30, 40, "K3", 800, 600),
    Book(470, 195, 30, 40, "K4", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for k in books:
    canvas.add_element(k)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Книги растут и открываются
        for elem in books:
            elem.add_book()
            if step >= 4:
                elem.open_book()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, Book):
        o = getattr(elem, 'is_open', False)
        print(f"{elem.name}: высота={h}, открыта={o}")
    else:
        b = getattr(elem, 'book_level', 0)
        print(f"{elem.name}: высота={h}, книги={b}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и книги коричневого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится золотистым
- Шаг 3: появляются первые книги на полках библиотеки
- Шаг 4-5: книги растут, открываются (видны страницы)
- Шаг 6: финальная композиция с градиентом от коричневого к насыщенному золотому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту коричневый→золотой
- Появлению книг на полках зданий
- Открытию книг (видны страницы)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как библиотечный комплекс с книгами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление книг и трансформацию цветовой схемы от коричневого к золотому. Важно показать разнообразие: здания и книги должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Book3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для книги дополнительно хранить `self.open_path` (история открытия)
- Метод `get_color()` возвращает цвет в градиенте от коричневого к золотому

### **Шаблон кода**

```python
class ArchElement3D:
    def __init__(self, x, y, width, height, name):
        # TODO: сохранить координаты, размеры, имя
        self.x = x
        self.y = y
        self.width = width
        self._height = height
        self.name = name
        # TODO: создать self.path = [height]
        self.path = [height]
        self.color_ratio = 0  # для градиента
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от коричневого к золотому
        from matplotlib.colors import to_hex, to_rgb
        brown = to_rgb('#8B4513')
        gold = to_rgb('#FFD700')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(b + (g - b) * ratio for b, g in zip(brown, gold))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Book3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.is_open = False
        self.is_open = False
        # TODO: создать self.open_path = [False]
        self.open_path = [False]
        self.page_count = 10
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 80)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с библиотечными элементами
- `create_book_mesh(x, y, width, depth, height, is_open, color)` — книга (обложка + страницы)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с библиотечными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_book_mesh(x, y, width, depth, height, is_open, color):
    """Создает книгу: обложка + страницы"""
    traces = []
    
    # Обложка книги
    if is_open:
        # Открытая книга (две половинки)
        left_cover_x = [x, x + width/2, x + width/2, x]
        left_cover_y = [y, y, y + depth, y + depth]
        left_cover_z = [0, 0, 0, 0]
        
        right_cover_x = [x + width/2, x + width, x + width, x + width/2]
        right_cover_y = [y, y, y + depth, y + depth]
        right_cover_z = [0, 0, 0, 0]
        
        traces.append(go.Mesh3d(
            x=left_cover_x, y=left_cover_y, z=left_cover_z,
            color=color, opacity=0.9,
            alphahull=0
        ))
        traces.append(go.Mesh3d(
            x=right_cover_x, y=right_cover_y, z=right_cover_z,
            color=color, opacity=0.9,
            alphahull=0
        ))
    else:
        # Закрытая книга
        traces.append(create_building_mesh(x, y, width, depth, height, color))
    
    # Страницы (линии)
    for i in range(5):
        page_x = x + width * 0.2 + i * (width * 0.15)
        traces.append(go.Scatter3d(
            x=[page_x, page_x], y=[y + depth/2, y + depth/2], 
            z=[0, height],
            mode='lines',
            line=dict(color='#F5F5DC', width=2)
        ))
    
    # Корешок книги
    traces.append(go.Scatter3d(
        x=[x, x], y=[y, y + depth], z=[height/2, height/2],
        mode='lines',
        line=dict(color='#654321', width=4)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 6 элементов (2 Building3D + 4 Book3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: книги растут и открываются

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 1.0, 60, "B1"),
    Building3D(4.0, 1.0, 1.0, 70, "B2"),
    Book3D(1.2, 1.0, 0.3, 40, "K1"),
    Book3D(1.5, 1.0, 0.3, 40, "K2"),
    Book3D(4.2, 1.0, 0.3, 40, "K3"),
    Book3D(4.5, 1.0, 0.3, 40, "K4")
]

# Коэффициенты роста
growth_rates = [15, 20, 10, 10, 10, 10]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Book3D) and step >= 4:
            # Книги растут и открываются на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            if step >= 5:
                elem.is_open = True
            elem.open_path.append(elem.is_open)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все книги с текущим состоянием открытия из `open_path[step]`
- Цветовую динамику от коричневого к золотому

### **Шаблон кода**

```python
frames = []
max_steps = max(len(elem.path) for elem in elements)

# Координаты земли
x_ground = np.arange(0, 6, 0.3)
y_ground = np.arange(0, 3, 0.3)
X, Y = np.meshgrid(x_ground, y_ground)

for step in range(max_steps):
    traces = []
    
    # Поверхность земли (коричневый цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale=[[0, '#8B7355'], [1, '#654321']], 
        showscale=False, opacity=0.5
    ))
    
    for elem in elements:
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Book3D):
            color = elem.get_color()
            open_state = elem.open_path[step] if step < len(elem.open_path) else elem.open_path[-1]
            traces.extend(create_book_mesh(
                elem.x, elem.y, elem.width, 0.4, h, open_state, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Библиотека с книгой".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора библиотечной архитектуры
- Добавлены подписи и заголовок с темой "От коричневого к золотому"

### **Шаблон кода**

```python
# Создание фигуры (начальный кадр)
fig = go.Figure(data=frames[0].data, frames=frames)

# Кнопка Play
fig.update_layout(
    updatemenus=[dict(
        type='buttons',
        buttons=[dict(
            label='▶ Запустить анимацию',
            method='animate',
            args=[None, {"frame": {"duration": 600, "redraw": True},
                         "fromcurrent": True, "transition": {"duration": 300}}]
        )]
    )],
    scene=dict(
        xaxis_title='Пространство X',
        yaxis_title='Пространство Y', 
        zaxis_title='Высота',
        camera=dict(eye=dict(x=2.5, y=2.5, z=2.0)),
        aspectmode='manual',
        aspectratio=dict(x=1.5, y=0.8, z=1.2),
        bgcolor='#87CEEB'
    ),
    title="📚 Анимация: Библиотека с книгой (градиент от коричневого к золотому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания коричневого цвета с минимальными высотами и закрытые книги
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Книги начинают расти и открываться (шаги 4-7)
  - Цвет всех элементов плавно переходит от коричневого (`#8B4513`) к насыщенному золотому (`#FFD700`)
  - Книги открываются (видны страницы)
  - Корешки книг видны в тёмно-коричневом цвете
  - Страницы книг светятся кремовым цветом
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" библиотечного архитектурного ансамбля
