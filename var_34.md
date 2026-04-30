# **Вариант № 34**

## **Генеративная художественная композиция "Больница с красным крестом" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей медицинской истории, который хочет получить интерактивную генеративную инсталляцию "Больница с красным крестом". Инсталляция должна создавать уникальные композиции с больничными зданиями, где здания и медицинские символы оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и кресты — это "живые" объекты, способные расти, менять цвет от белого к синему и реагировать на взаимодействие, создавая динамичные композиции в стиле медицинской архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
cross_count = 4  # количество крестов на здании
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к синему. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и медицинских крестов, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание больницы с окнами
- **`Cross`** — медицинский крест (красный крест на белом фоне)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания больницы
        # TODO: добавить окна (3 ряда по 4 окна)
        # TODO: добавить вход с навесом
        pass

class Cross(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать красный крест
        # TODO: добавить белый фон для креста
        # TODO: использовать градиент от белого к синему
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_cross_to_building(building, count)` добавляет кресты к зданию больницы
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
    
    def add_cross_to_building(self, building, count):
        # TODO: создать кресты на фасаде здания
        # TODO: равномерно распределить кресты по ширине здания
        # TODO: добавить кресты в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от белого к синему
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'cross', 'medical')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #FFFFFF к #4169E1 (ratio от 0 до 1)

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
                'base_blue': '#4169E1',
                'cross_red': '#DC143C'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от белого к синему усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от белого к синему
        # ratio: 0.0 = белый, 1.0 = синий
        from matplotlib.colors import to_rgb, to_hex
        white = to_rgb('#FFFFFF')
        blue = to_rgb('#4169E1')
        interpolated = tuple(w + (b - w) * ratio for w, b in zip(white, blue))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий больницы с 4 крестами на каждом фасаде, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (100, 400), (300, 380), (500, 420)
# - По 4 Cross на каждом здании

# Пример создания креста:
# cross = Cross(building.x + offset, building.y + building.height - 50, 
#               30, 30, "Cross1")
# canvas.add_element(cross)

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
        # TODO: создать Rectangle для фасада больницы
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 4)
        window_w, window_h = self.width * 0.12, self.height * 0.12
        for row in range(3):
            for col in range(4):
                wx = self.x + self.width * 0.08 + col * (self.width * 0.22)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#4169E1')
                ax.add_patch(window)
        # TODO: добавить вход с навесом
        entrance = Rectangle((self.x + self.width/2 - 25, self.y), 
                            50, self.height * 0.3,
                            facecolor='#F0F8FF', edgecolor='#4169E1', linewidth=2)
        ax.add_patch(entrance)
        # TODO: добавить навес над входом
        awning = Polygon([
            (self.x + self.width/2 - 30, self.y + self.height * 0.3),
            (self.x + self.width/2 + 30, self.y + self.height * 0.3),
            (self.x + self.width/2, self.y + self.height * 0.25)
        ], facecolor='#4169E1', edgecolor='#4169E1')
        ax.add_patch(awning)

class Cross(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 60)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать белый фон для креста
        bg = Rectangle((self.x, self.y), self.width, self.height, 
                      facecolor='#FFFFFF', edgecolor='#4169E1', linewidth=2)
        ax.add_patch(bg)
        # TODO: отрисовать красный крест (вертикальная полоса)
        vert_w = self.width * 0.2
        vert = Rectangle((self.x + self.width/2 - vert_w/2, self.y + 5), 
                        vert_w, self.height - 10,
                        facecolor='#DC143C', edgecolor='none')
        ax.add_patch(vert)
        # TODO: отрисовать красный крест (горизонтальная полоса)
        horz_h = self.height * 0.2
        horz = Rectangle((self.x + 5, self.y + self.height/2 - horz_h/2), 
                        self.width - 10, horz_h,
                        facecolor='#DC143C', edgecolor='none')
        ax.add_patch(horz)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и кресты должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" медицинской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать крест с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к синему в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления крестов к больнице, где кресты появляются постепенно и начинают светиться, а цветовая динамика подчёркивает процесс "оживления" медицинского учреждения.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Cross: высота должна быть достаточной для креста (не менее 20px)

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
        self.cross_level = 0  # уровень крестов
    
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

class Cross(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.is_glowing = False  # светится ли крест
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 20:  # минимальная высота для креста
            value = 20
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_cross()` — добавляет крест к зданию
- `get_color()` — возвращает цвет в градиенте от белого к синему

**Для Cross**:
- `add_cross()` — увеличивает размер креста
- `glow()` — включает свечение креста
- `get_color()` — возвращает цвет в градиенте от белого к синему

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_cross(self):
        # TODO: увеличить cross_level
        self.cross_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к синему
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если cross_level > 0: добавить кресты
        pass

class Cross(ArchElement):
    def add_cross(self):
        # TODO: увеличить размер креста
        self.height += 10
        self.width += 10
        self.history.append(self._height)
        return self
    
    def glow(self):
        # TODO: установить is_glowing = True
        self.is_glowing = True
        return self
    
    def get_color(self):
        # TODO: градиент от белого к синему по высоте
        ratio = min(1.0, self._height / 60)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать крест с учётом is_glowing
        # TODO: отрисовать фон и красный крест
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление крестов к больнице

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
                'crosses': getattr(elem, 'cross_level', 0),
                'glowing': getattr(elem, 'is_glowing', False)
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
        ax.set_title(f"Шаг {step}: Больница с красным крестом")
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

Создайте композицию из 2 зданий и 4 крестов. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые кресты
- Шаги 4-5: кресты растут, начинают светиться
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

crosses = [
    Cross(120, 200, 30, 30, "C1", 800, 600),
    Cross(180, 200, 30, 30, "C2", 800, 600),
    Cross(420, 195, 30, 30, "C3", 800, 600),
    Cross(480, 195, 30, 30, "C4", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for c in crosses:
    canvas.add_element(c)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Кресты растут и светятся
        for elem in crosses:
            elem.add_cross()
            if step >= 4:
                elem.glow()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, Cross):
        g = getattr(elem, 'is_glowing', False)
        print(f"{elem.name}: высота={h}, свечение={g}")
    else:
        c = getattr(elem, 'cross_level', 0)
        print(f"{elem.name}: высота={h}, кресты={c}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и кресты белого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится синеватым
- Шаг 3: появляются первые кресты на фасадах больницы
- Шаг 4-5: кресты растут, начинают светиться (красный цвет становится ярче)
- Шаг 6: финальная композиция с градиентом от белого к насыщенному синему

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→синий
- Появлению крестов на фасадах зданий
- Свечению крестов (яркость красного цвета)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как медицинский комплекс с крестами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление крестов и трансформацию цветовой схемы от белого к синему. Важно показать разнообразие: здания и кресты должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Cross3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для креста дополнительно хранить `self.glow_path` (история свечения)
- Метод `get_color()` возвращает цвет в градиенте от белого к синему

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
        # TODO: вернуть цвет в градиенте от белого к синему
        from matplotlib.colors import to_hex, to_rgb
        white = to_rgb('#FFFFFF')
        blue = to_rgb('#4169E1')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (b - w) * ratio for w, b in zip(white, blue))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Cross3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.is_glowing = False
        self.is_glowing = False
        # TODO: создать self.glow_path = [False]
        self.glow_path = [False]
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 60)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с медицинскими элементами
- `create_cross_mesh(x, y, width, depth, height, is_glowing, color)` — крест (3D крест на белом фоне)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с медицинскими элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_cross_mesh(x, y, width, depth, height, is_glowing, color):
    """Создает крест: 3D крест на белом фоне"""
    traces = []
    
    # Белый фон для креста
    bg_x = [x, x+width, x+width, x]
    bg_y = [y, y, y+depth, y+depth]
    bg_z = [height/2] * 4
    
    bg_mesh = go.Mesh3d(
        x=bg_x, y=bg_y, z=bg_z,
        color='#FFFFFF', opacity=0.9,
        alphahull=0
    )
    traces.append(bg_mesh)
    
    # Вертикальная полоса креста
    vert_w = width * 0.2
    vert_x = [x + width/2 - vert_w/2, x + width/2 + vert_w/2, 
              x + width/2 + vert_w/2, x + width/2 - vert_w/2]
    vert_y = [y, y, y+depth, y+depth]
    vert_z = [5, 5, 5, 5]
    
    vert_mesh = go.Mesh3d(
        x=vert_x, y=vert_y, z=vert_z,
        color='#DC143C', opacity=0.9 if is_glowing else 0.7,
        alphahull=0
    )
    traces.append(vert_mesh)
    
    # Горизонтальная полоса креста
    horz_h = height * 0.2
    horz_x = [x, x+width, x+width, x]
    horz_y = [y + height/2 - horz_h/2, y + height/2 - horz_h/2,
              y + height/2 + horz_h/2, y + height/2 + horz_h/2]
    horz_z = [6, 6, 6, 6]
    
    horz_mesh = go.Mesh3d(
        x=horz_x, y=horz_y, z=horz_z,
        color='#DC143C', opacity=0.9 if is_glowing else 0.7,
        alphahull=0
    )
    traces.append(horz_mesh)
    
    # Свечение креста (если включено)
    if is_glowing:
        traces.append(go.Scatter3d(
            x=[x + width/2], y=[y + height/2], z=[10],
            mode='markers',
            marker=dict(size=15, color='#FF0000', opacity=0.5)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 6 элементов (2 Building3D + 4 Cross3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: кресты растут и начинают светиться

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 1.0, 60, "B1"),
    Building3D(4.0, 1.0, 1.0, 70, "B2"),
    Cross3D(1.2, 1.0, 0.3, 30, "C1"),
    Cross3D(1.5, 1.0, 0.3, 30, "C2"),
    Cross3D(4.2, 1.0, 0.3, 30, "C3"),
    Cross3D(4.5, 1.0, 0.3, 30, "C4")
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
        elif isinstance(elem, Cross3D) and step >= 4:
            # Кресты растут и светятся на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            if step >= 5:
                elem.is_glowing = True
            elem.glow_path.append(elem.is_glowing)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (серый цвет)
- Все здания с текущей высотой из `path[step]`
- Все кресты с текущим состоянием свечения из `glow_path[step]`
- Цветовую динамику от белого к синему

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
    
    # Поверхность земли (серый цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale=[[0, '#D3D3D3'], [1, '#A9A9A9']], 
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
        elif isinstance(elem, Cross3D):
            color = elem.get_color()
            glow = elem.glow_path[step] if step < len(elem.glow_path) else elem.glow_path[-1]
            traces.extend(create_cross_mesh(
                elem.x, elem.y, elem.width, 0.4, h, glow, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Больница с красным крестом".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора медицинской архитектуры
- Добавлены подписи и заголовок с темой "От белого к синему"

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
    title="🏥 Анимация: Больница с красным крестом (градиент от белого к синему)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами и кресты без свечения
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Кресты начинают расти и светиться (шаги 4-7)
  - Цвет всех элементов плавно переходит от белого (`#FFFFFF`) к насыщенному синему (`#4169E1`)
  - Кресты начинают светиться (красный цвет становится ярче)
  - Белый фон крестов виден на фасаде здания
  - Навес над входом в больницу виден в синем цвете
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" медицинского архитектурного ансамбля
