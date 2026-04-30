# **Вариант № 35**

## **Генеративная художественная композиция "Школа с колоколом" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей образовательной истории, который хочет получить интерактивную генеративную инсталляцию "Школа с колоколом". Инсталляция должна создавать уникальные композиции со школьными зданиями, где здания и колокола оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и колокола — это "живые" объекты, способные расти, менять цвет от красного к белому и реагировать на взаимодействие, создавая динамичные композиции в стиле образовательной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
bell_count = 2  # количество колоколов
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от красного к белому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и колоколов, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание школы с окнами
- **`Bell`** — школьный колокол (на башне или отдельной конструкции)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания школы
        # TODO: добавить окна (3 ряда по 4 окна)
        # TODO: добавить вход с крыльцом
        pass

class Bell(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать колокол (трапеция + язык)
        # TODO: добавить башню для колокола
        # TODO: использовать градиент от красного к белому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_bell_to_building(building)` добавляет колокол к зданию школы
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
    
    def add_bell_to_building(self, building):
        # TODO: создать колокол на здании школы
        # TODO: позиция: building.x + building.width/2 - 20, building.y + building.height
        # TODO: добавить колокол в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от красного к белому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'bell', 'tower')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #DC143C к #FFFFFF (ratio от 0 до 1)

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
                'base_red': '#DC143C',
                'base_white': '#FFFFFF'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от красного к белому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от красного к белому
        # ratio: 0.0 = красный, 1.0 = белый
        from matplotlib.colors import to_rgb, to_hex
        red = to_rgb('#DC143C')
        white = to_rgb('#FFFFFF')
        interpolated = tuple(r + (w - r) * ratio for r, w in zip(red, white))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий школы с 2 колоколами на башнях, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (100, 400), (300, 380), (500, 420)
# - 2 Bell на зданиях

# Пример создания колокола:
# bell = Bell(building.x + building.width/2 - 20, building.y + building.height, 
#             40, 50, "Bell1")
# canvas.add_element(bell)

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
        # TODO: создать Rectangle для фасада школы
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 4)
        window_w, window_h = self.width * 0.12, self.height * 0.12
        for row in range(3):
            for col in range(4):
                wx = self.x + self.width * 0.08 + col * (self.width * 0.22)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)
        # TODO: добавить вход с крыльцом
        entrance = Rectangle((self.x + self.width/2 - 25, self.y), 
                            50, self.height * 0.3,
                            facecolor='#F5F5DC', edgecolor='#8B4513', linewidth=2)
        ax.add_patch(entrance)
        # TODO: добавить крыльцо
        porch = Rectangle((self.x + self.width/2 - 35, self.y), 
                         70, 15, facecolor='#8B4513', edgecolor='#8B4513')
        ax.add_patch(porch)

class Bell(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 100)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать башню для колокола
        tower = Rectangle((self.x + 5, self.y), self.width - 10, self.height * 0.6, 
                         facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(tower)
        # TODO: отрисовать колокол (трапеция)
        bell = Polygon([
            (self.x + 10, self.y + self.height * 0.6),
            (self.x + self.width - 10, self.y + self.height * 0.6),
            (self.x + self.width - 5, self.y + self.height),
            (self.x + 5, self.y + self.height)
        ], facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(bell)
        # TODO: добавить язык колокола
        clapper = Circle((self.x + self.width/2, self.y + self.height * 0.85), 
                        8, facecolor='#8B4513', edgecolor='none')
        ax.add_patch(clapper)
        # TODO: добавить верёвку для звонка
        ax.plot([self.x + self.width/2, self.x + self.width/2], 
               [self.y + self.height * 0.85, self.y + self.height + 20], 
               color='#8B4513', linewidth=2)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и колокола должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" образовательной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать колокол с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от красного к белому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления колоколов к школе, где колокола появляются постепенно и начинают звонить, а цветовая динамика подчёркивает процесс "оживления" школьного здания.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Bell: высота должна быть достаточной для колокола (не менее 40px)

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
        self.bell_level = 0  # уровень колокола
    
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

class Bell(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.is_ringing = False  # звонит ли колокол
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 40:  # минимальная высота для колокола
            value = 40
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_bell()` — добавляет колокол к зданию
- `get_color()` — возвращает цвет в градиенте от красного к белому

**Для Bell**:
- `add_bell()` — увеличивает размер колокола
- `ring()` — заставляет колокол звонить
- `get_color()` — возвращает цвет в градиенте от красного к белому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_bell(self):
        # TODO: увеличить bell_level
        self.bell_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от красного к белому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если bell_level > 0: добавить колокол
        pass

class Bell(ArchElement):
    def add_bell(self):
        # TODO: увеличить размер колокола
        self.height += 15
        self.width += 10
        self.history.append(self._height)
        return self
    
    def ring(self):
        # TODO: установить is_ringing = True
        self.is_ringing = True
        return self
    
    def get_color(self):
        # TODO: градиент от красного к белому по высоте
        ratio = min(1.0, self._height / 100)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать колокол с учётом is_ringing
        # TODO: отрисовать башню, колокол и язык
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление колоколов к школе

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
                'bells': getattr(elem, 'bell_level', 0),
                'ringing': getattr(elem, 'is_ringing', False)
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
        ax.set_title(f"Шаг {step}: Школа с колоколом")
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

Создайте композицию из 2 зданий и 2 колоколов. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые колокола
- Шаги 4-5: колокола растут, начинают звонить
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

bells = [
    Bell(140, 200, 40, 50, "L1", 800, 600),
    Bell(440, 195, 40, 50, "L2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for l in bells:
    canvas.add_element(l)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Колокола растут и звонят
        for elem in bells:
            elem.add_bell()
            if step >= 4:
                elem.ring()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, Bell):
        r = getattr(elem, 'is_ringing', False)
        print(f"{elem.name}: высота={h}, звон={r}")
    else:
        b = getattr(elem, 'bell_level', 0)
        print(f"{elem.name}: высота={h}, колокола={b}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и колокола красного цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится белее
- Шаг 3: появляются первые колокола на башнях школы
- Шаг 4-5: колокола растут, начинают звонить (язык двигается)
- Шаг 6: финальная композиция с градиентом от красного к насыщенному белому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту красный→белый
- Появлению колоколов на башнях зданий
- Звону колоколов (движение языка)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как школьный комплекс с колоколами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление колоколов и трансформацию цветовой схемы от красного к белому. Важно показать разнообразие: здания и колокола должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Bell3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для колокола дополнительно хранить `self.ring_path` (история звона)
- Метод `get_color()` возвращает цвет в градиенте от красного к белому

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
        # TODO: вернуть цвет в градиенте от красного к белому
        from matplotlib.colors import to_hex, to_rgb
        red = to_rgb('#DC143C')
        white = to_rgb('#FFFFFF')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(r + (w - r) * ratio for r, w in zip(red, white))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Bell3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.is_ringing = False
        self.is_ringing = False
        # TODO: создать self.ring_path = [False]
        self.ring_path = [False]
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 100)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с школьными элементами
- `create_bell_mesh(x, y, width, depth, height, is_ringing, color)` — колокол (башня + колокол + язык)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с школьными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_bell_mesh(x, y, width, depth, height, is_ringing, color):
    """Создает колокол: башня + колокол + язык"""
    traces = []
    
    # Башня для колокола
    tower_h = height * 0.6
    traces.append(create_building_mesh(x + 5, y, width - 10, depth, tower_h, color))
    
    # Колокол (усечённая пирамида)
    bell_base_x = [x + 10, x + width - 10, x + width - 10, x + 10]
    bell_base_y = [y, y, y + depth, y + depth]
    bell_base_z = [tower_h] * 4
    
    bell_top_x = [x + 5, x + width - 5, x + width - 5, x + 5]
    bell_top_y = [y, y, y + depth, y + depth]
    bell_top_z = [height] * 4
    
    bell_mesh = go.Mesh3d(
        x=bell_base_x + bell_top_x,
        y=bell_base_y + bell_top_y,
        z=bell_base_z + bell_top_z,
        color=color, opacity=0.9,
        alphahull=0
    )
    traces.append(bell_mesh)
    
    # Язык колокола (сфера)
    clapper_z = tower_h + (height - tower_h) * 0.4
    traces.append(go.Scatter3d(
        x=[x + width/2], y=[y + depth/2], z=[clapper_z],
        mode='markers',
        marker=dict(size=8, color='#8B4513', 
                   opacity=1.0 if is_ringing else 0.7)
    ))
    
    # Верёвка для звонка
    traces.append(go.Scatter3d(
        x=[x + width/2, x + width/2], 
        y=[y + depth/2, y + depth/2], 
        z=[clapper_z, height + 20],
        mode='lines',
        line=dict(color='#8B4513', width=3)
    ))
    
    # Эффект звона (если звонит)
    if is_ringing:
        for i in range(3):
            ring_x = x + width/2 + np.random.randint(-15, 15)
            ring_y = y + depth/2 + np.random.randint(-15, 15)
            ring_z = clapper_z + np.random.randint(-10, 10)
            
            traces.append(go.Scatter3d(
                x=[ring_x], y=[ring_y], z=[ring_z],
                mode='markers',
                marker=dict(size=5, color='#FFD700', opacity=0.5)
            ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Bell3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: колокола растут и начинают звонить

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 1.0, 60, "B1"),
    Building3D(4.0, 1.0, 1.0, 70, "B2"),
    Bell3D(1.4, 1.0, 0.4, 50, "L1"),
    Bell3D(4.4, 1.0, 0.4, 50, "L2")
]

# Коэффициенты роста
growth_rates = [15, 20, 15, 15]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Bell3D) and step >= 4:
            # Колокола растут и звонят на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            if step >= 5:
                elem.is_ringing = True
            elem.ring_path.append(elem.is_ringing)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все колокола с текущим состоянием звона из `ring_path[step]`
- Цветовую динамику от красного к белому

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
        elif isinstance(elem, Bell3D):
            color = elem.get_color()
            ring = elem.ring_path[step] if step < len(elem.ring_path) else elem.ring_path[-1]
            traces.extend(create_bell_mesh(
                elem.x, elem.y, elem.width, 0.4, h, ring, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Школа с колоколом".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора образовательной архитектуры
- Добавлены подписи и заголовок с темой "От красного к белому"

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
    title="🔔 Анимация: Школа с колоколом (градиент от красного к белому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания красного цвета с минимальными высотами и колокола без звона
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Колокола начинают расти и звонить (шаги 4-7)
  - Цвет всех элементов плавно переходит от красного (`#DC143C`) к насыщенному белому (`#FFFFFF`)
  - Колокола начинают звонить (язык двигается, появляются золотые эффекты)
  - Башни для колоколов становятся выше
  - Верёвка для звонка видна
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" образовательного архитектурного ансамбля
