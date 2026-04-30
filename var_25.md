# **Вариант № 25**

## **Генеративная художественная композиция "Современная ветроэлектростанция" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей современной энергетики, который хочет получить интерактивную генеративную инсталляцию "Современная ветроэлектростанция". Инсталляция должна создавать уникальные композиции с ветряными турбинами, где здания и турбины оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и ветряные турбины — это "живые" объекты, способные расти, менять цвет от белого к серому и реагировать на взаимодействие, создавая динамичные композиции в стиле современной эко-архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
turbine_count = 4  # количество ветряных турбин
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к серому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и ветряных турбин, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в современном стиле
- **`WindTurbine`** — ветряная турбина с башней и лопастями

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в современном стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить солнечные панели на крыше
        pass

class WindTurbine(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать башню турбины (вертикальная линия/прямоугольник)
        # TODO: отрисовать 3 лопасти (линии от центра)
        # TODO: добавить центр вращения (круг)
        # TODO: использовать градиент от белого к серому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_turbine_near(building)` создаёт турбину рядом со зданием
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
    
    def add_turbine_near(self, building):
        # TODO: создать турбину рядом со зданием
        # TODO: позиция: building.x + building.width + 50, building.y
        # TODO: добавить турбину в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от белого к серому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'turbine', 'blade')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #FFFFFF к #808080 (ratio от 0 до 1)

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
                'base_white': '#FFFFFF',
                'base_gray': '#808080'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от белого к серому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от белого к серому
        # ratio: 0.0 = белый, 1.0 = серый
        from matplotlib.colors import to_rgb, to_hex
        white = to_rgb('#FFFFFF')
        gray = to_rgb('#808080')
        interpolated = tuple(w + (g - w) * ratio for w, g in zip(white, gray))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 4 ветряными турбинами, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 4 WindTurbine рядом с зданиями

# Пример создания турбины:
# turbine = WindTurbine(building.x + building.width + 50, building.y, 
#                       20, 200, "Turbine1")
# canvas.add_element(turbine)

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
import numpy as np

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в современном стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#808080', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#808080')
                ax.add_patch(window)
        # TODO: добавить солнечные панели на крыше
        panel_w = self.width * 0.3
        panel = Rectangle((self.x + self.width * 0.35, self.y + self.height), 
                         panel_w, 15, facecolor='#4169E1', edgecolor='#808080')
        ax.add_patch(panel)

class WindTurbine(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 300)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать башню турбины
        tower = Rectangle((self.x + self.width/2 - 5, self.y), 
                         10, self.height * 0.7, 
                         facecolor=color, edgecolor='#808080', linewidth=2)
        ax.add_patch(tower)
        # TODO: отрисовать 3 лопасти (линии от центра)
        center_x = self.x + self.width/2
        center_y = self.y + self.height * 0.7
        blade_length = self.height * 0.4
        for i in range(3):
            angle = 2 * np.pi * i / 3
            end_x = center_x + blade_length * np.cos(angle)
            end_y = center_y + blade_length * np.sin(angle)
            ax.plot([center_x, end_x], [center_y, end_y], 
                   color=color, linewidth=4)
        # TODO: добавить центр вращения (круг)
        hub = Circle((center_x, center_y), 8, 
                    facecolor=color, edgecolor='#808080')
        ax.add_patch(hub)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и турбины должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" современной энергетики)
- Иметь защиту от некорректных параметров (нельзя сделать турбину с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к серому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления лопастей к турбине, где лопасти начинают вращаться, а цветовая динамика подчёркивает процесс "оживления" ветроэлектростанции.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для WindTurbine: высота должна быть достаточной для лопастей (не менее 150px)

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
        self.turbine_level = 0  # уровень турбины
    
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

class WindTurbine(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.blade_angle = 0  # угол вращения лопастей
        self.blade_count = 3  # количество лопастей
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 150:  # минимальная высота для лопастей
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
- `add_turbine_decor()` — добавляет декор современной энергетики
- `get_color()` — возвращает цвет в градиенте от белого к серому

**Для WindTurbine**:
- `add_turbine()` — увеличивает высоту турбины
- `rotate_blades(angle)` — вращает лопасти на угол
- `get_color()` — возвращает цвет в градиенте от белого к серому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_turbine_decor(self):
        # TODO: увеличить turbine_level
        self.turbine_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к серому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если turbine_level > 0: добавить декор энергетики
        pass

class WindTurbine(ArchElement):
    def add_turbine(self):
        # TODO: увеличить высоту турбины
        self.height += 20
        self.history.append(self._height)
        return self
    
    def rotate_blades(self, angle):
        # TODO: увеличить угол вращения
        self.blade_angle += angle
        # TODO: нормализовать угол (0-360)
        self.blade_angle = self.blade_angle % 360
        return self
    
    def get_color(self):
        # TODO: градиент от белого к серому по высоте
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать турбину с учётом blade_angle
        # TODO: отрисовать башню, лопасти и центр
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное вращение лопастей турбины

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
                'blade_angle': getattr(elem, 'blade_angle', 0),
                'turbines': getattr(elem, 'turbine_level', 0)
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
        ax.set_title(f"Шаг {step}: Ветроэлектростанция")
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

Создайте композицию из 2 зданий и 4 турбин. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые турбины
- Шаги 4-5: турбины растут, лопасти вращаются
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

turbines = [
    WindTurbine(180, 100, 20, 180, "T1", 800, 600),
    WindTurbine(250, 100, 20, 190, "T2", 800, 600),
    WindTurbine(430, 100, 20, 185, "T3", 800, 600),
    WindTurbine(500, 100, 20, 195, "T4", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for t in turbines:
    canvas.add_element(t)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Турбины растут и вращаются
        for elem in turbines:
            elem.add_turbine()
            elem.rotate_blades(30)  # вращение на 30 градусов
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, WindTurbine):
        angle = getattr(elem, 'blade_angle', 0)
        print(f"{elem.name}: высота={h}, угол лопастей={angle}°")
    else:
        print(f"{elem.name}: высота={h}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и турбины белого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится серым
- Шаг 3: турбины начинают работать (появляются лопасти)
- Шаг 4-5: турбины растут, лопасти вращаются на 30° каждый шаг
- Шаг 6: финальная композиция с градиентом от белого к насыщенному серому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→серый
- Вращению лопастей турбин (изменение угла)
- Увеличению высоты турбин
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как ветроэлектростанция "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление турбин и трансформацию цветовой схемы от белого к серому. Важно показать разнообразие: здания и турбины должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `WindTurbine3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для турбины дополнительно хранить `self.blade_path` (история угла вращения)
- Метод `get_color()` возвращает цвет в градиенте от белого к серому

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
        # TODO: вернуть цвет в градиенте от белого к серому
        from matplotlib.colors import to_hex, to_rgb
        white = to_rgb('#FFFFFF')
        gray = to_rgb('#808080')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (g - w) * ratio for w, g in zip(white, gray))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class WindTurbine3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.blade_angle = 0
        self.blade_angle = 0
        # TODO: создать self.blade_path = [0]
        self.blade_path = [0]
        self.blade_count = 3
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с современными элементами
- `create_turbine_mesh(x, y, width, depth, height, blade_angle, color)` — турбина (башня + лопасти)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с современными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_turbine_mesh(x, y, width, depth, height, blade_angle, color):
    """Создает ветряную турбину: башня + лопасти"""
    traces = []
    
    # Башня турбины (цилиндр)
    tower_height = height * 0.7
    traces.append(create_building_mesh(
        x + width/2 - 5, y, 10, depth, tower_height, color
    ))
    
    # Лопасти (линии от центра)
    center_x = x + width/2
    center_y = y + depth/2
    center_z = tower_height
    blade_length = height * 0.4
    
    for i in range(3):
        angle = 2 * np.pi * i / 3 + np.radians(blade_angle)
        end_x = center_x + blade_length * np.cos(angle)
        end_y = center_y + blade_length * np.sin(angle)
        
        traces.append(go.Scatter3d(
            x=[center_x, end_x], y=[center_y, end_y], z=[center_z, center_z],
            mode='lines',
            line=dict(color=color, width=6)
        ))
    
    # Центр вращения
    traces.append(go.Scatter3d(
        x=[center_x], y=[center_y], z=[center_z],
        mode='markers',
        marker=dict(size=8, color=color)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 6 элементов (2 Building3D + 4 WindTurbine3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: турбины растут и вращаются

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    WindTurbine3D(2.0, 1.0, 0.2, 180, "T1"),
    WindTurbine3D(2.5, 1.0, 0.2, 190, "T2"),
    WindTurbine3D(4.0, 1.0, 0.2, 185, "T3"),
    WindTurbine3D(4.5, 1.0, 0.2, 195, "T4")
]

# Коэффициенты роста
growth_rates = [15, 20, 18, 18, 18, 18]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, WindTurbine3D) and step >= 4:
            # Турбины растут и вращаются на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.blade_angle += 30
            elem.blade_path.append(elem.blade_angle)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (зелёный цвет)
- Все здания с текущей высотой из `path[step]`
- Все турбины с текущим углом вращения из `blade_path[step]`
- Цветовую динамику от белого к серому

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
    
    # Поверхность земли (зелёный цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale='Greens', showscale=False, opacity=0.4
    ))
    
    for elem in elements:
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, WindTurbine3D):
            color = elem.get_color()
            angle = elem.blade_path[step] if step < len(elem.blade_path) else elem.blade_path[-1]
            traces.extend(create_turbine_mesh(
                elem.x, elem.y, elem.width, 0.4, h, angle, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Ветроэлектростанция".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора современной энергетики
- Добавлены подписи и заголовок с темой "От белого к серому"

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
    title="💨 Анимация: Ветроэлектростанция (градиент от белого к серому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами и турбины с неподвижными лопастями
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Турбины начинают расти и вращаться (шаги 4-7)
  - Цвет всех элементов плавно переходит от белого (`#FFFFFF`) к насыщенному серому (`#808080`)
  - Лопасти турбин вращаются на 30° каждый шаг
  - Башни турбин становятся выше
  - 3 лопасти на каждой турбине видны в 3D
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" современной ветроэлектростанции
