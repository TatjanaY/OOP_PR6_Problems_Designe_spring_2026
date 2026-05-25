**Вариант № 36 **

## **Генеративная художественная композиция "Университетский кампус" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей высшего образования, который хочет получить интерактивную генеративную инсталляцию "Университетский кампус". Инсталляция должна создавать уникальные композиции с университетскими зданиями, где корпуса, библиотеки и студенческие пространства оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания кампуса — это "живые" объекты, способные расти, менять цвет от зелёного к синему и реагировать на взаимодействие, создавая динамичные композиции в стиле образовательной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 4
campus_zones = 3  # количество зон кампуса
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от зелёного к синему. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий кампуса, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

Создайте три класса, наследующихся от `ArchElement`:

- **`LectureHall`** — лекционный зал с большими окнами
- **`Library`** — библиотека с книжными полками
- **`StudentCenter`** — студенческий центр с зоной отдыха

### **Шаблон кода**

```python
class LectureHall(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник лекционного зала
        # TODO: добавить большие окна (2 ряда по 5 окон)
        # TODO: добавить вход с навесом
        pass

class Library(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать здание библиотеки
        # TODO: добавить окна-витражи
        # TODO: добавить книжные стеллажи на фасаде
        # TODO: использовать градиент от зелёного к синему
        pass

class StudentCenter(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать студенческий центр
        # TODO: добавить зону отдыха (скамейки)
        # TODO: добавить кафе на первом этаже
        # TODO: использовать градиент от зелёного к синему
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_path_between(elem1, elem2)` создаёт дорожку между элементами
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
    
    def add_path_between(self, elem1, elem2):
        # TODO: создать дорожку между elem1 и elem2
        # TODO: добавить дорожку в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от зелёного к синему
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'path', 'green')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #228B22 к #4169E1 (ratio от 0 до 1)

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
                'base_green': '#228B22',
                'base_blue': '#4169E1'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от зелёного к синему усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от зелёного к синему
        # ratio: 0.0 = зелёный, 1.0 = синий
        from matplotlib.colors import to_rgb, to_hex
        green = to_rgb('#228B22')
        blue = to_rgb('#4169E1')
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(green, blue))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 4 зданий кампуса (лекционный зал, библиотека, студенческий центр), используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 2 LectureHall на координатах (50, 400), (250, 380)
# - 1 Library на координате (450, 420)
# - 1 StudentCenter на координате (600, 400)

# Пример создания здания:
# lecture = LectureHall(50, 400, 100, 80, "Lecture1")
# canvas.add_element(lecture)

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

class LectureHall(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада лекционного зала
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#228B22', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить большие окна (2 ряда по 5)
        window_w, window_h = self.width * 0.12, self.height * 0.15
        for row in range(2):
            for col in range(5):
                wx = self.x + self.width * 0.05 + col * (self.width * 0.18)
                wy = self.y + self.height * 0.3 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#228B22')
                ax.add_patch(window)
        # TODO: добавить вход с навесом
        entrance = Rectangle((self.x + self.width/2 - 20, self.y), 
                            40, self.height * 0.3,
                            facecolor='#F5F5DC', edgecolor='#228B22')
        ax.add_patch(entrance)

class Library(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 200)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать здание библиотеки
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна-витражи
        for i in range(4):
            vitrage_x = self.x + self.width * 0.15 + i * (self.width * 0.2)
            vitrage = Rectangle((vitrage_x, self.y + self.height * 0.3), 
                               self.width * 0.15, self.height * 0.5,
                               facecolor='#FFD700', edgecolor='#4169E1', alpha=0.5)
            ax.add_patch(vitrage)
        # TODO: добавить книжные стеллажи на фасаде
        for i in range(3):
            shelf_y = self.y + self.height * 0.2 + i * (self.height * 0.25)
            ax.plot([self.x + 10, self.x + self.width - 10], 
                   [shelf_y, shelf_y], color='#4169E1', linewidth=2)

class StudentCenter(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 180)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать студенческий центр
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#228B22', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить зону отдыха (скамейки)
        for i in range(3):
            bench_x = self.x + self.width * 0.2 + i * (self.width * 0.25)
            bench = Rectangle((bench_x, self.y - 20), 30, 15, 
                             facecolor='#8B4513', edgecolor='#654321')
            ax.add_patch(bench)
        # TODO: добавить кафе на первом этаже
        cafe = Rectangle((self.x + 10, self.y + 10), 
                        self.width - 20, self.height * 0.25,
                        facecolor='#FFA07A', edgecolor='#228B22')
        ax.add_patch(cafe)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания кампуса должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" университетской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать здание с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от зелёного к синему в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления зданий к кампусу, где корпуса появляются постепенно и соединяются дорожками, а цветовая динамика подчёркивает процесс "оживления" университетского комплекса.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Library: высота должна быть достаточной для витражей (не менее 100px)

### **Шаблон кода**

```python
class LectureHall(ArchElement):
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
        self.path_level = 0  # уровень дорожек
    
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

class Library(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.vitrage_count = 4  # количество витражей
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 100:  # минимальная высота для витражей
            value = 100
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для LectureHall**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_path()` — добавляет дорожку к зданию
- `get_color()` — возвращает цвет в градиенте от зелёного к синему

**Для Library**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_vitrage()` — добавляет витраж
- `get_color()` — возвращает цвет в градиенте от зелёного к синему

**Для StudentCenter**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_zone()` — добавляет зону отдыха
- `get_color()` — возвращает цвет в градиенте от зелёного к синему

### **Шаблон кода**

```python
class LectureHall(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_path(self):
        # TODO: увеличить path_level
        self.path_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от зелёного к синему
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если path_level > 0: добавить дорожки
        pass

class Library(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        return self
    
    def add_vitrage(self):
        # TODO: увеличить количество витражей
        self.vitrage_count += 1
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 200)
        return theme.get_gradient(ratio)

class StudentCenter(ArchElement):
    def add_floors(self, n):
        self.height += n * 30
        return self
    
    def add_zone(self):
        # TODO: добавить зону отдыха
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 180)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление зданий к кампусу

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
                'paths': getattr(elem, 'path_level', 0),
                'floors': getattr(elem, 'floors', 0)
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
        ax.set_title(f"Шаг {step}: Университетский кампус")
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

Создайте композицию из 4 зданий кампуса. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: лекционные залы добавляют этажи
- Шаг 3: появляется библиотека
- Шаги 4-5: здания соединяются дорожками, появляется студенческий центр
- Шаг 6: финальная композиция с полным градиентом цвета

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание анимированного холста
canvas = AnimatedCity(800, 600)

# TODO: создать элементы:
lecture_halls = [
    LectureHall(50, 150, 100, 80, "L1", 800, 600),
    LectureHall(250, 150, 100, 75, "L2", 800, 600)
]

library = Library(450, 140, 120, 100, "Lib", 800, 600)

student_center = StudentCenter(600, 150, 110, 85, "SC", 800, 600)

for lh in lecture_halls:
    canvas.add_element(lh)
canvas.add_element(library)
canvas.add_element(student_center)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Лекционные залы добавляют этажи
        for elem in lecture_halls:
            elem.add_floors(1)
    elif step == 3:
        # Появление библиотеки
        library.add_floors(1)
        library.add_vitrage()
    elif step <= 5:
        # Здания соединяются дорожками
        for elem in lecture_halls:
            elem.add_path()
        student_center.add_floors(1)
        student_center.add_zone()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, LectureHall):
        p = getattr(elem, 'path_level', 0)
        print(f"{elem.name}: высота={h}, дорожки={p}")
    elif isinstance(elem, Library):
        v = getattr(elem, 'vitrage_count', 0)
        print(f"{elem.name}: высота={h}, витражи={v}")
    else:
        print(f"{elem.name}: высота={h}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания кампуса зелёного цвета с минимальными параметрами
- Шаг 1-2: лекционные залы растут, цвет постепенно становится синеватым
- Шаг 3: появляется библиотека с витражами
- Шаг 4-5: здания соединяются дорожками, появляется студенческий центр
- Шаг 6: финальная композиция с градиентом от зелёного к насыщенному синему

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту зелёный→синий
- Появлению новых зданий кампуса
- Соединению зданий дорожками
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как университетский кампус "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление зданий и трансформацию цветовой схемы от зелёного к синему. Важно показать разнообразие: лекционные залы, библиотека и студенческий центр должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `LectureHall3D`, `Library3D` и `StudentCenter3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для библиотеки дополнительно хранить `self.vitrage_path` (история витражей)
- Метод `get_color()` возвращает цвет в градиенте от зелёного к синему

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
        # TODO: вернуть цвет в градиенте от зелёного к синему
        from matplotlib.colors import to_hex, to_rgb
        green = to_rgb('#228B22')
        blue = to_rgb('#4169E1')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(green, blue))
        return to_hex(interpolated)


class LectureHall3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Library3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.vitrage_count = 4
        self.vitrage_count = 4
        # TODO: создать self.vitrage_path = [4]
        self.vitrage_path = [4]
    
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 200)
        return super().get_color()


class StudentCenter3D(ArchElement3D):
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 180)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_lecture_mesh(x, y, width, depth, height, color)` — лекционный зал с окнами
- `create_library_mesh(x, y, width, depth, height, vitrage_count, color)` — библиотека с витражами
- `create_student_mesh(x, y, width, depth, height, color)` — студенческий центр

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_lecture_mesh(x, y, width, depth, height, color):
    """Создает лекционный зал для plotly"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_library_mesh(x, y, width, depth, height, vitrage_count, color):
    """Создает библиотеку с витражами"""
    traces = []
    
    # Основное здание
    traces.append(create_lecture_mesh(x, y, width, depth, height, color))
    
    # Витражи (светящиеся панели)
    for i in range(vitrage_count):
        vitrage_x = x + width * 0.15 + i * (width * 0.2)
        vitrage_y = y + depth/2
        vitrage_z = height * 0.3
        
        traces.append(go.Scatter3d(
            x=[vitrage_x, vitrage_x], y=[vitrage_y, vitrage_y],
            z=[vitrage_z, vitrage_z + height * 0.5],
            mode='lines',
            line=dict(color='#FFD700', width=8, opacity=0.7)
        ))
    
    return traces


def create_student_mesh(x, y, width, depth, height, color):
    """Создает студенческий центр"""
    traces = []
    
    # Основное здание
    traces.append(create_lecture_mesh(x, y, width, depth, height, color))
    
    # Зона отдыха (скамейки)
    for i in range(3):
        bench_x = x + width * 0.2 + i * (width * 0.25)
        bench_y = y - 10
        bench_z = 0
        
        traces.append(go.Scatter3d(
            x=[bench_x, bench_x + 20], y=[bench_y, bench_y], z=[bench_z, bench_z],
            mode='lines',
            line=dict(color='#8B4513', width=6)
        ))
    
    # Кафе (светящееся окно)
    traces.append(go.Scatter3d(
        x=[x + 10, x + width - 10], y=[y + depth, y + depth],
        z=[height * 0.1, height * 0.1],
        mode='lines',
        line=dict(color='#FFA07A', width=10, opacity=0.8)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 LectureHall3D + 1 Library3D + 1 StudentCenter3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: лекционные залы растут
- **Шаги 4-5**: библиотека добавляет витражи
- **Шаги 6-7**: студенческий центр растёт, добавляются дорожки

### **Шаблон кода**

```python
# Создание элементов
elements = [
    LectureHall3D(1.0, 1.0, 0.8, 60, "L1"),
    LectureHall3D(2.5, 1.0, 0.8, 70, "L2"),
    Library3D(4.0, 1.0, 1.0, 100, "Lib"),
    StudentCenter3D(5.5, 1.0, 0.9, 85, "SC")
]

# Коэффициенты роста
growth_rates = [15, 20, 18, 18]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, LectureHall3D) and step <= 3:
            # Лекционные залы растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Library3D) and step >= 4 and step <= 5:
            # Библиотека добавляет витражи на шагах 4-5
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.vitrage_count += 1
            elem.vitrage_path.append(elem.vitrage_count)
        elif isinstance(elem, StudentCenter3D) and step >= 6:
            # Студенческий центр растёт на шагах 6-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (зелёный цвет)
- Все здания с текущей высотой из `path[step]`
- Библиотека с текущим количеством витражей из `vitrage_path[step]`
- Цветовую динамику от зелёного к синему

### **Шаблон кода**

```python
frames = []
max_steps = max(len(elem.path) for elem in elements)

# Координаты земли
x_ground = np.arange(0, 7, 0.3)
y_ground = np.arange(0, 3, 0.3)
X, Y = np.meshgrid(x_ground, y_ground)

for step in range(max_steps):
    traces = []
    
    # Поверхность земли (зелёный цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale='Greens', showscale=False, opacity=0.4
    ))
    
    for elem in elements:
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, LectureHall3D):
            color = elem.get_color()
            traces.append(create_lecture_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Library3D):
            color = elem.get_color()
            v = elem.vitrage_path[step] if step < len(elem.vitrage_path) else elem.vitrage_path[-1]
            traces.extend(create_library_mesh(
                elem.x, elem.y, elem.width, 0.4, h, v, color
            ))
        elif isinstance(elem, StudentCenter3D):
            color = elem.get_color()
            traces.extend(create_student_mesh(
                elem.x, elem.y, elem.width, 0.4, h, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Университетский кампус".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора образовательной архитектуры
- Добавлены подписи и заголовок с темой "От зелёного к синему"

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
        camera=dict(eye=dict(x=3.0, y=3.0, z=2.5)),
        aspectmode='manual',
        aspectratio=dict(x=1.8, y=0.8, z=1.2),
        bgcolor='#87CEEB'
    ),
    title="🎓 Анимация: Университетский кампус (градиент от зелёного к синему)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания зелёного цвета с минимальными высотами
- При нажатии Play:
  - Лекционные залы начинают расти (шаги 1-3)
  - Библиотека добавляет витражи (шаги 4-5)
  - Студенческий центр растёт (шаги 6-7)
  - Цвет всех элементов плавно переходит от зелёного (`#228B22`) к насыщенному синему (`#4169E1`)
  - Витражи библиотеки светятся золотым цветом
  - Зона отдыха и кафе видны в студенческом центре
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" университетского кампуса
