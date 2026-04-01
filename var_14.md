# **Вариант № 14**

## **Генеративная художественная композиция "Круглая ротонда" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей классической архитектуры, который хочет получить интерактивную генеративную инсталляцию "Круглая ротонда". Инсталляция должна создавать уникальные композиции с ротондами, где здания и круглые структуры оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и ротонды — это "живые" объекты, способные расти, менять цвет от белого к мраморному и реагировать на взаимодействие, создавая динамичные композиции в стиле классической архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
rotunda_count = 2  # количество ротонд
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к мраморному. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и ротонд, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в классическом стиле
- **`Rotunda`** — круглая ротонда с колоннами по периметру

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в классическом стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить декоративные элементы
        pass

class Rotunda(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать круглое основание ротонды
        # TODO: добавить колонны по периметру
        # TODO: отрисовать купол на вершине
        # TODO: использовать градиент от белого к мраморному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_rotunda_near(building)` создаёт ротонду рядом со зданием
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
    
    def add_rotunda_near(self, building):
        # TODO: создать ротонду рядом со зданием
        # TODO: позиция: building.x + building.width + 50, building.y
        # TODO: добавить ротонду в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от белого к мраморному
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'rotunda', 'marble')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #FFFFFF к #F5F5DC (ratio от 0 до 1)

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
                'ground': '#D2B48C',
                'base_white': '#FFFFFF',
                'base_marble': '#F5F5DC'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от белого к мраморному усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от белого к мраморному
        # ratio: 0.0 = белый, 1.0 = мраморный
        from matplotlib.colors import to_rgb, to_hex
        white = to_rgb('#FFFFFF')
        marble = to_rgb('#F5F5DC')
        interpolated = tuple(w + (m - w) * ratio for w, m in zip(white, marble))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 2 ротондами рядом, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Rotunda рядом с зданиями

# Пример создания ротонды:
# rotunda = Rotunda(building.x + building.width + 50, building.y, 
#                   100, 150, "Rotunda1")
# canvas.add_element(rotunda)

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
        # TODO: создать Rectangle для фасада в классическом стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)

class Rotunda(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 250)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать круглое основание ротонды
        center_x = self.x + self.width / 2
        center_y = self.y + self.height * 0.5
        base = Circle((center_x, center_y), self.width / 2, 
                     facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(base)
        # TODO: добавить колонны по периметру (8 колонн)
        num_columns = 8
        radius = self.width / 2 - 10
        for i in range(num_columns):
            angle = 2 * 3.14159 * i / num_columns
            col_x = center_x + radius * 3.14159 / 180 * 0
            col_y = self.y + radius * 3.14159 / 180 * 0
            col = Rectangle((center_x + radius * 0.8 - 5, self.y), 
                           10, self.height * 0.7,
                           facecolor=color, edgecolor='#8B4513')
            ax.add_patch(col)
        # TODO: отрисовать купол на вершине
        dome = Circle((center_x, self.y + self.height * 0.8), 
                     self.width / 3, 
                     facecolor='#F5F5DC', edgecolor='#8B4513', linewidth=2)
        ax.add_patch(dome)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и ротонды должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" классической архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать ротонду с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к мраморному в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления колонн к ротонде, где колонны появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" ротонды.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Rotunda: высота должна быть достаточной для колонн (не менее 100px)

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
        self.column_level = 0  # уровень колонн
    
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

class Rotunda(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.num_columns = 8  # количество колонн
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 100:  # минимальная высота для колонн
            value = 100
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_columns()` — добавляет колонны к зданию
- `get_color()` — возвращает цвет в градиенте от белого к мраморному

**Для Rotunda**:
- `add_columns(n)` — добавляет n колонн к ротонде
- `get_color()` — возвращает цвет в градиенте от белого к мраморному

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_columns(self):
        # TODO: увеличить column_level
        self.column_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к мраморному
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если column_level > 0: добавить колонны
        pass

class Rotunda(ArchElement):
    def add_columns(self, n):
        # TODO: увеличить количество колонн на n
        self.num_columns += n
        # TODO: увеличить высоту пропорционально
        self.height += 15 * n
        # TODO: добавить запись в history
        self.history.append(self._height)
        # TODO: вернуть self
        return self
    
    def get_color(self):
        # TODO: градиент от белого к мраморному по высоте
        ratio = min(1.0, self._height / 250)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать ротонду с учётом num_columns
        # TODO: отрисовать купол на вершине
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление колонн к ротонде

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
                'columns': getattr(elem, 'num_columns', 0),
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
                              facecolor='#D2B48C', edgecolor='none'))
        
        # TODO: для каждого элемента вызвать draw()
        for elem in self.elements:
            elem.draw(ax)
        
        # TODO: добавить заголовок с номером шага
        ax.set_title(f"Шаг {step}: Круглая ротонда")
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

Создайте композицию из 2 зданий и 2 ротонд. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: ротонды получают первые колонны
- Шаги 4-5: ротонды продолжают расти, добавляются колонны
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
    Building(50, 150, 100, 80, "B1", 800, 600),
    Building(300, 150, 100, 75, "B2", 800, 600)
]

rotundas = [
    Rotunda(180, 100, 100, 120, "R1", 800, 600),
    Rotunda(430, 100, 100, 130, "R2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for r in rotundas:
    canvas.add_element(r)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Ротонды добавляют колонны
        for elem in rotundas:
            elem.add_columns(2)
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    c = getattr(elem, 'num_columns', 0)
    print(f"{elem.name}: высота={h}, колонны={c}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и ротонды белого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится мраморным
- Шаг 3: ротонды получают первые колонны (с 8 до 10)
- Шаг 4-5: ротонды продолжают расти, добавляются новые колонны (до 12-14)
- Шаг 6: финальная композиция с градиентом от белого к насыщенному мраморному

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→мраморный
- Появлению новых колонн на ротондах
- Увеличению высоты ротонд
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как классический комплекс с ротондами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление колонн и трансформацию цветовой схемы от белого к мраморному. Важно показать разнообразие: обычные здания и ротонды должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Rotunda3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для ротонды дополнительно хранить `self.column_path` (история количества колонн)
- Метод `get_color()` возвращает цвет в градиенте от белого к мраморному

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
        # TODO: вернуть цвет в градиенте от белого к мраморному
        from matplotlib.colors import to_hex, to_rgb
        white = to_rgb('#FFFFFF')
        marble = to_rgb('#F5F5DC')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (m - w) * ratio for w, m in zip(white, marble))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Rotunda3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.num_columns = 8
        self.num_columns = 8
        # TODO: создать self.column_path = [8]
        self.column_path = [8]
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 250)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с классическими элементами
- `create_rotunda_mesh(x, y, width, depth, height, num_columns, color)` — ротонда (цилиндр + колонны + купол)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с классическими элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_rotunda_mesh(x, y, width, depth, height, num_columns, color):
    """Создает ротонду: цилиндр + колонны + купол"""
    traces = []
    
    # Основной цилиндр ротонды
    n_segments = 32
    base_x, base_y = [], []
    top_x, top_y = [], []
    
    center_x, center_y = x + width/2, y + depth/2
    radius = width/2
    
    for angle in np.linspace(0, 2*np.pi, n_segments):
        base_x.append(center_x + radius * np.cos(angle))
        base_y.append(center_y + radius * np.sin(angle))
        top_x.append(center_x + radius * np.cos(angle))
        top_y.append(center_y + radius * np.sin(angle))
    
    body_z = [0]*n_segments + [height*0.8]*n_segments
    body_mesh = go.Mesh3d(
        x=base_x + top_x,
        y=base_y + top_y,
        z=body_z,
        color=color, opacity=0.95,
        alphahull=0
    )
    traces.append(body_mesh)
    
    # Колонны по периметру
    column_radius = width / (num_columns * 3)
    column_height = height * 0.7
    
    for i in range(num_columns):
        angle = 2 * np.pi * i / num_columns
        col_x = center_x + radius * 0.8 * np.cos(angle)
        col_y = center_y + radius * 0.8 * np.sin(angle)
        
        # Колонна как цилиндр
        col_base_x = [col_x + column_radius * np.cos(a) for a in np.linspace(0, 2*np.pi, 16)]
        col_base_y = [col_y + column_radius * np.sin(a) for a in np.linspace(0, 2*np.pi, 16)]
        col_top_x = col_base_x.copy()
        col_top_y = col_base_y.copy()
        col_z = [0]*16 + [column_height]*16
        
        col_mesh = go.Mesh3d(
            x=col_base_x + col_top_x,
            y=col_base_y + col_top_y,
            z=col_z,
            color=color, opacity=0.9,
            alphahull=0
        )
        traces.append(col_mesh)
    
    # Купол на вершине (полусфера)
    dome_radius = radius * 0.6
    dome_center_z = height * 0.8
    
    theta = np.linspace(0, 2*np.pi, 20)
    phi = np.linspace(0, np.pi/2, 10)
    
    dome_x, dome_y, dome_z = [], [], []
    for p in phi:
        for t in theta:
            dome_x.append(center_x + dome_radius * np.sin(p) * np.cos(t))
            dome_y.append(center_y + dome_radius * np.sin(p) * np.sin(t))
            dome_z.append(dome_center_z + dome_radius * np.cos(p))
    
    dome_mesh = go.Mesh3d(
        x=dome_x, y=dome_y, z=dome_z,
        color='#F5F5DC', opacity=0.9,
        alphahull=0
    )
    traces.append(dome_mesh)
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Rotunda3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: ротонды добавляют колонны и растут

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Rotunda3D(2.0, 1.0, 0.8, 120, "R1"),
    Rotunda3D(4.0, 1.0, 0.9, 130, "R2")
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
        elif isinstance(elem, Rotunda3D) and step >= 4:
            # Ротонды добавляют колонны и растут на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.num_columns += 2
            elem.column_path.append(elem.num_columns)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (мраморный цвет)
- Все здания с текущей высотой из `path[step]`
- Все ротонды с текущим количеством колонн из `column_path[step]`
- Цветовую динамику от белого к мраморному

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
    
    # Поверхность земли (мраморный цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale=[[0, '#F5F5DC'], [1, '#E8E4C9']], 
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
        elif isinstance(elem, Rotunda3D):
            color = elem.get_color()
            c = elem.column_path[step] if step < len(elem.column_path) else elem.column_path[-1]
            traces.extend(create_rotunda_mesh(
                elem.x, elem.y, elem.width, 0.4, h, c, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Круглая ротонда".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора классической архитектуры
- Добавлены подписи и заголовок с темой "От белого к мраморному"

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
    title="🏛️ Анимация: Круглая ротонда (градиент от белого к мраморному)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами и ротонды с 8 колоннами
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Ротонды начинают добавлять колонны (шаги 4-7)
  - Цвет всех элементов плавно переходит от белого (`#FFFFFF`) к насыщенному мраморному (`#F5F5DC`)
  - Количество колонн увеличивается с 8 до 14-16
  - Купол на вершине ротонды светится мраморным цветом
  - Колонны расположены по периметру ротонды
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" классического архитектурного ансамбля
