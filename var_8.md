# **Вариант № 8**

## **Генеративная художественная композиция "Маяк с вращающимся светом" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей морской истории, который хочет получить интерактивную генеративную инсталляцию "Маяк с вращающимся светом". Инсталляция должна создавать уникальные композиции с маяками, где здания и световые лучи оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и маяки — это "живые" объекты, способные расти, менять цвет от белого к красному и реагировать на взаимодействие, создавая динамичные композиции с вращающимся световым лучом.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
lighthouse_count = 2  # количество маяков
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к красному. Это позволяет плавно менять настроение инсталляции (день/вечер/ночь) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и маяков, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в морском стиле
- **`Lighthouse`** — маяк с цилиндрическим корпусом и вращающимся светом

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в морском стиле
        # TODO: добавить окна (2 ряда по 3 окна)
        # TODO: добавить крышу (треугольник)
        pass

class Lighthouse(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать цилиндрический корпус маяка
        # TODO: отрисовать световой луч (линия от вершины)
        # TODO: добавить фонарную комнату на вершине
        # TODO: использовать градиент от белого к красному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_lighthouse_near(building)` создаёт маяк рядом со зданием
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
    
    def add_lighthouse_near(self, building):
        # TODO: создать маяк рядом со зданием
        # TODO: позиция: building.x + building.width + 50, building.y
        # TODO: добавить маяк в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('day', 'evening', 'night') с градиентом от белого к красному
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'lighthouse', 'light')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #FFFFFF к #DC143C (ratio от 0 до 1)

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
                'sea': '#4682B4',
                'base_white': '#FFFFFF',
                'base_red': '#DC143C'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для day/evening/night
        # Градиент от белого к красному усиливается к night
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от белого к красному
        # ratio: 0.0 = белый, 1.0 = красный
        from matplotlib.colors import to_rgb, to_hex
        white = to_rgb('#FFFFFF')
        red = to_rgb('#DC143C')
        interpolated = tuple(w + (r - w) * ratio for w, r in zip(white, red))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 2 маяками рядом, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('evening')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Lighthouse рядом с зданиями

# Пример создания маяка:
# lighthouse = Lighthouse(building.x + building.width + 50, building.y, 
#                         50, 200, "Lighthouse1")
# canvas.add_element(lighthouse)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Polygon, Circle, Wedge
import numpy as np

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в морском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#4682B4', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить крышу (треугольник)
        roof = Polygon([
            (self.x - 10, self.y + self.height),
            (self.x + self.width + 10, self.y + self.height),
            (self.x + self.width/2, self.y + self.height + 50)
        ], facecolor='#4682B4', edgecolor='#4682B4', linewidth=2)
        ax.add_patch(roof)
        # TODO: добавить окна (2 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#4682B4')
                ax.add_patch(window)

class Lighthouse(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 300)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать цилиндрический корпус маяка (трапеция для перспективы)
        body = Polygon([
            (self.x + self.width * 0.2, self.y),
            (self.x + self.width * 0.8, self.y),
            (self.x + self.width * 0.7, self.y + self.height * 0.85),
            (self.x + self.width * 0.3, self.y + self.height * 0.85)
        ], facecolor=color, edgecolor='#DC143C', linewidth=2)
        ax.add_patch(body)
        # TODO: добавить фонарную комнату на вершине
        lantern = Rectangle((self.x + self.width * 0.25, 
                            self.y + self.height * 0.85),
                           self.width * 0.5, self.height * 0.15,
                           facecolor='#FFD700', edgecolor='#DC143C', linewidth=2)
        ax.add_patch(lantern)
        # TODO: отрисовать световой луч (конус света)
        center_x = self.x + self.width / 2
        center_y = self.y + self.height * 0.92
        light_beam = Wedge((center_x, center_y), 150, -30, 30, 
                          facecolor='#FFFF00', edgecolor='none', alpha=0.5)
        ax.add_patch(light_beam)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и маяки должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" морской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать маяк с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к красному в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию вращения светового луча маяка, где луч поворачивается постепенно, а цветовая динамика подчёркивает процесс "оживления" маяка.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Lighthouse: высота должна быть достаточной для света (не менее 150px)

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
        self.light_level = 0  # уровень освещения
    
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

class Lighthouse(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.light_angle = 0  # угол вращения света
        self.light_intensity = 0  # интенсивность света
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 150:  # минимальная высота для света
            value = 150
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_light_decor()` — добавляет декор в морском стиле
- `get_color()` — возвращает цвет в градиенте от белого к красному

**Для Lighthouse**:
- `add_light()` — увеличивает интенсивность света
- `rotate_light(angle)` — вращает световой луч на угол
- `get_color()` — возвращает цвет в градиенте от белого к красному

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_light_decor(self):
        # TODO: увеличить light_level
        self.light_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к красному
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если light_level > 0: добавить морской декор
        pass

class Lighthouse(ArchElement):
    def add_light(self):
        # TODO: увеличить интенсивность света
        self.light_intensity += 1
        # TODO: увеличить высоту пропорционально
        self.height += 15
        # TODO: добавить запись в history
        self.history.append(self._height)
        # TODO: вернуть self
        return self
    
    def rotate_light(self, angle):
        # TODO: увеличить угол вращения
        self.light_angle += angle
        # TODO: нормализовать угол (0-360)
        self.light_angle = self.light_angle % 360
        return self
    
    def get_color(self):
        # TODO: градиент от белого к красному по высоте
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать корпус маяка
        # TODO: отрисовать световой луч с учётом угла вращения
        # TODO: использовать rotate_light для анимации вращения
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное вращение светового луча маяка

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
                'light_angle': getattr(elem, 'light_angle', 0),
                'intensity': getattr(elem, 'light_intensity', 0)
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
        
        # TODO: добавить море
        ax.add_patch(Rectangle((0, 0), self.width, 100, 
                              facecolor='#4682B4', edgecolor='none'))
        
        # TODO: для каждого элемента вызвать draw()
        for elem in self.elements:
            elem.draw(ax)
        
        # TODO: добавить заголовок с номером шага
        ax.set_title(f"Шаг {step}: Маяк с вращающимся светом")
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

Создайте композицию из 2 зданий и 2 маяков. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: маяки включают свет
- Шаги 4-5: маяки продолжают расти, свет вращается
- Шаг 6: финальная композиция с полным градиентом цвета

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('evening')

# создание анимированного холста
canvas = AnimatedCity(800, 600)

# TODO: создать элементы:
buildings = [
    Building(50, 150, 100, 80, "B1", 800, 600),
    Building(300, 150, 100, 75, "B2", 800, 600)
]

lighthouses = [
    Lighthouse(180, 100, 50, 180, "L1", 800, 600),
    Lighthouse(430, 100, 50, 190, "L2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for l in lighthouses:
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
        # Маяки включают свет и вращают луч
        for elem in lighthouses:
            elem.add_light()
            elem.rotate_light(60)  # вращение на 60 градусов
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    angle = getattr(elem, 'light_angle', 0)
    print(f"{elem.name}: высота={h}, угол света={angle}°")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и маяки белого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится красноватым
- Шаг 3: маяки включают свет (появляется световой луч)
- Шаг 4-5: маяки продолжают расти, свет вращается на 60 градусов каждый шаг
- Шаг 6: финальная композиция с градиентом от белого к насыщенному красному

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→красный
- Появлению светового луча на маяках
- Вращению светового луча (изменение угла)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как морской пейзаж с маяками "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, включение света и трансформацию цветовой схемы от белого к красному. Важно показать разнообразие: обычные здания и маяки должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Lighthouse3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для маяка дополнительно хранить `self.light_path` (история интенсивности света) и `self.rotation_angle` (угол вращения)
- Метод `get_color()` возвращает цвет в градиенте от белого к красному

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
        # TODO: вернуть цвет в градиенте от белого к красному
        from matplotlib.colors import to_hex, to_rgb
        white = to_rgb('#FFFFFF')
        red = to_rgb('#DC143C')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (r - w) * ratio for w, r in zip(white, red))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Lighthouse3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.light_intensity = 0
        self.light_intensity = 0
        # TODO: создать self.light_path = [0]
        self.light_path = [0]
        # TODO: создать self.rotation_angle = 0
        self.rotation_angle = 0
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с крышей
- `create_lighthouse_mesh(x, y, width, depth, height, light_angle, light_intensity, color)` — маяк (цилиндр + световой луч)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с морскими элементами"""
    # 8 вершин параллелепипеда
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    # 12 треугольников (6 граней по 2 треугольника)
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_lighthouse_mesh(x, y, width, depth, height, light_angle, light_intensity, color):
    """Создает маяк: цилиндр + фонарная комната + световой луч"""
    traces = []
    
    # Корпус маяка (цилиндр через аппроксимацию)
    n_segments = 16
    base_x, base_y = [], []
    top_x, top_y = [], []
    
    center_x, center_y = x + width/2, y + depth/2
    base_radius = width/2
    top_radius = width/3
    body_height = height * 0.85
    
    for angle in np.linspace(0, 2*np.pi, n_segments):
        base_x.append(center_x + base_radius * np.cos(angle))
        base_y.append(center_y + base_radius * np.sin(angle))
        top_x.append(center_x + top_radius * np.cos(angle))
        top_y.append(center_y + top_radius * np.sin(angle))
    
    # Создаём корпус через Mesh3d
    body_z = [0]*n_segments + [body_height]*n_segments
    body_mesh = go.Mesh3d(
        x=base_x + top_x,
        y=base_y + top_y,
        z=body_z,
        color=color, opacity=0.95,
        alphahull=0
    )
    traces.append(body_mesh)
    
    # Фонарная комната
    lantern_height = height * 0.15
    lantern_mesh = go.Mesh3d(
        x=[center_x - width*0.15, center_x + width*0.15, 
           center_x + width*0.15, center_x - width*0.15,
           center_x - width*0.15, center_x + width*0.15,
           center_x + width*0.15, center_x - width*0.15],
        y=[center_y - width*0.15, center_y - width*0.15,
           center_y + width*0.15, center_y + width*0.15,
           center_y - width*0.15, center_y - width*0.15,
           center_y + width*0.15, center_y + width*0.15],
        z=[body_height]*4 + [body_height + lantern_height]*4,
        i=[0,0,1,1,2,2,3,3,0,0,1,1],
        j=[1,1,2,2,3,3,0,0,4,4,5,5],
        k=[4,5,5,6,6,7,7,4,5,4,6,5],
        color='#FFD700', opacity=0.9
    )
    traces.append(lantern_mesh)
    
    # Световой луч (конус)
    if light_intensity > 0:
        beam_length = 100 * light_intensity
        beam_angle = np.radians(light_angle)
        
        # Создаём конус света через Scatter3d
        beam_x = [center_x, center_x + beam_length * np.cos(beam_angle)]
        beam_y = [center_y, center_y + beam_length * np.sin(beam_angle)]
        beam_z = [body_height + lantern_height/2, body_height + lantern_height/2]
        
        traces.append(go.Scatter3d(
            x=beam_x, y=beam_y, z=beam_z,
            mode='lines',
            line=dict(color='#FFFF00', width=8, opacity=0.7)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Lighthouse3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: маяки включают свет и вращают луч

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Lighthouse3D(2.0, 1.0, 0.5, 180, "L1"),
    Lighthouse3D(4.0, 1.0, 0.6, 190, "L2")
]

# Коэффициенты роста
growth_rates = [15, 20, 18, 18]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Lighthouse3D) and step >= 4:
            # Маяки включают свет и вращают луч на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.light_intensity += 1
            elem.light_path.append(elem.light_intensity)
            elem.rotation_angle += 60  # вращение на 60 градусов
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность моря (синий цвет)
- Все здания с текущей высотой из `path[step]`
- Все маяки с текущей интенсивностью света из `light_path[step]` и углом вращения
- Цветовую динамику от белого к красному

### **Шаблон кода**

```python
frames = []
max_steps = max(len(elem.path) for elem in elements)

# Координаты моря
x_sea = np.arange(0, 6, 0.3)
y_sea = np.arange(0, 3, 0.3)
X, Y = np.meshgrid(x_sea, y_sea)

for step in range(max_steps):
    traces = []
    
    # Поверхность моря
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale='Blues', showscale=False, opacity=0.5
    ))
    
    for elem in elements:
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Lighthouse3D):
            color = elem.get_color()
            intensity = elem.light_path[step] if step < len(elem.light_path) else elem.light_path[-1]
            angle = elem.rotation_angle if step >= 4 else 0
            traces.extend(create_lighthouse_mesh(
                elem.x, elem.y, elem.width, 0.4, h, angle, intensity, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Маяк с вращающимся светом".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора морского пейзажа
- Добавлены подписи и заголовок с темой "От белого к красному"

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
    title="🗼 Анимация: Маяк с вращающимся светом (градиент от белого к красному)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами и маяки без света
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Маяки включают свет (шаги 4-7)
  - Цвет всех элементов плавно переходит от белого (`#FFFFFF`) к насыщенному красному (`#DC143C`)
  - Световой луч маяка вращается на 60 градусов каждый шаг
  - Фонарная комната маяка светится золотым цветом
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" морского архитектурного ансамбля
