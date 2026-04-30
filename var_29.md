# **Вариант № 29**

## **Генеративная художественная композиция "Телевизионная башня" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей телекоммуникаций, который хочет получить интерактивную генеративную инсталляцию "Телевизионная башня". Инсталляция должна создавать уникальные композиции с телебашнями, где здания и антенны оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и антенны — это "живые" объекты, способные расти, менять цвет от серого к красному и реагировать на взаимодействие, создавая динамичные композиции в стиле современной телекоммуникационной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
antenna_count = 2  # количество антенн
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к красному. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и телевизионных антенн, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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
- **`RadioTower`** — телевизионная башня с антенной

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в современном стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить технические элементы
        pass

class RadioTower(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать мачту башни (вертикальная линия)
        # TODO: добавить антенну на вершине
        # TODO: добавить сигнальные огни
        # TODO: использовать градиент от серого к красному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_tower_near(building)` создаёт башню рядом со зданием
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
    
    def add_tower_near(self, building):
        # TODO: создать башню рядом со зданием
        # TODO: позиция: building.x + building.width + 50, building.y
        # TODO: добавить башню в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от серого к красному
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'tower', 'signal')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #808080 к #DC143C (ratio от 0 до 1)

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
                'base_gray': '#808080',
                'base_red': '#DC143C'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от серого к красному усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от серого к красному
        # ratio: 0.0 = серый, 1.0 = красный
        from matplotlib.colors import to_rgb, to_hex
        gray = to_rgb('#808080')
        red = to_rgb('#DC143C')
        interpolated = tuple(g + (r - g) * ratio for g, r in zip(gray, red))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 2 телевизионными башнями, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('evening')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 RadioTower рядом с зданиями

# Пример создания башни:
# tower = RadioTower(building.x + building.width + 50, building.y, 
#                    30, 250, "Tower1")
# canvas.add_element(tower)

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
        # TODO: создать Rectangle для фасада в современном стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#404040', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#404040')
                ax.add_patch(window)
        # TODO: добавить технические элементы (антенны на крыше)
        for i in range(3):
            ant_x = self.x + self.width * 0.2 + i * (self.width * 0.3)
            ax.plot([ant_x, ant_x], [self.y + self.height, self.y + self.height + 15], 
                   color='#404040', linewidth=2)

class RadioTower(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 350)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать мачту башни (вертикальная линия)
        mast_x = self.x + self.width / 2
        ax.plot([mast_x, mast_x], [self.y, self.y + self.height * 0.9], 
               color=color, linewidth=4)
        # TODO: добавить антенну на вершине
        antenna = Polygon([
            (mast_x - 15, self.y + self.height * 0.9),
            (mast_x + 15, self.y + self.height * 0.9),
            (mast_x, self.y + self.height)
        ], facecolor=color, edgecolor='#404040', linewidth=2)
        ax.add_patch(antenna)
        # TODO: добавить сигнальные огни (красные точки)
        for i in range(5):
            light_y = self.y + i * (self.height * 0.2)
            light = Circle((mast_x, light_y), 5, 
                          facecolor='#FF0000', edgecolor='none')
            ax.add_patch(light)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и башни должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" телекоммуникационной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать башню с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к красному в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления антенн к башне, где антенны появляются постепенно и начинают передавать сигнал, а цветовая динамика подчёркивает процесс "оживления" телекоммуникационной системы.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для RadioTower: высота должна быть достаточной для антенны (не менее 180px)

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
        self.tower_level = 0  # уровень башни
    
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

class RadioTower(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.antenna_count = 1  # количество антенн
        self.signal_strength = 0  # сила сигнала
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 180:  # минимальная высота для антенны
            value = 180
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_tower_decor()` — добавляет телекоммуникационный декор
- `get_color()` — возвращает цвет в градиенте от серого к красному

**Для RadioTower**:
- `add_antenna()` — добавляет антенну к башне
- `transmit_signal()` — увеличивает силу сигнала
- `get_color()` — возвращает цвет в градиенте от серого к красному

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_tower_decor(self):
        # TODO: увеличить tower_level
        self.tower_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к красному
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если tower_level > 0: добавить телекоммуникационный декор
        pass

class RadioTower(ArchElement):
    def add_antenna(self):
        # TODO: увеличить количество антенн
        self.antenna_count += 1
        # TODO: увеличить высоту пропорционально
        self.height += 20
        # TODO: добавить запись в history
        self.history.append(self._height)
        # TODO: вернуть self
        return self
    
    def transmit_signal(self):
        # TODO: увеличить силу сигнала
        self.signal_strength += 1
        return self
    
    def get_color(self):
        # TODO: градиент от серого к красному по высоте
        ratio = min(1.0, self._height / 350)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать башню с учётом antenna_count и signal_strength
        # TODO: отрисовать мачту, антенны и сигнальные огни
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление антенн к башне

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
                'antennas': getattr(elem, 'antenna_count', 0),
                'signal': getattr(elem, 'signal_strength', 0)
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
        ax.set_title(f"Шаг {step}: Телевизионная башня")
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

Создайте композицию из 2 зданий и 2 башен. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые антенны
- Шаги 4-5: башни растут, сигнал усиливается
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

towers = [
    RadioTower(180, 100, 30, 200, "T1", 800, 600),
    RadioTower(430, 100, 30, 210, "T2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for t in towers:
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
        # Башни добавляют антенны и передают сигнал
        for elem in towers:
            elem.add_antenna()
            elem.transmit_signal()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, RadioTower):
        a = getattr(elem, 'antenna_count', 0)
        s = getattr(elem, 'signal_strength', 0)
        print(f"{elem.name}: высота={h}, антенны={a}, сигнал={s}")
    else:
        t = getattr(elem, 'tower_level', 0)
        print(f"{elem.name}: высота={h}, башни={t}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и башни серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится красноватым
- Шаг 3: появляются первые антенны на башнях
- Шаг 4-5: башни растут, сигнал усиливается (сигнальные огни мигают)
- Шаг 6: финальная композиция с градиентом от серого к насыщенному красному

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→красный
- Появлению новых антенн на башнях
- Усилению сигнала (мигание огней)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как телекоммуникационная система с башнями "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление антенн и трансформацию цветовой схемы от серого к красному. Важно показать разнообразие: здания и башни должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `RadioTower3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для башни дополнительно хранить `self.antenna_path` (история количества антенн)
- Метод `get_color()` возвращает цвет в градиенте от серого к красному

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
        # TODO: вернуть цвет в градиенте от серого к красному
        from matplotlib.colors import to_hex, to_rgb
        gray = to_rgb('#808080')
        red = to_rgb('#DC143C')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (r - g) * ratio for g, r in zip(gray, red))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class RadioTower3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.antenna_count = 1
        self.antenna_count = 1
        # TODO: создать self.antenna_path = [1]
        self.antenna_path = [1]
        self.signal_strength = 0
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 350)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с современными элементами
- `create_tower_mesh(x, y, width, depth, height, antenna_count, signal_strength, color)` — телебашня (мачта + антенны)

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


def create_tower_mesh(x, y, width, depth, height, antenna_count, signal_strength, color):
    """Создает телебашню: мачта + антенны"""
    traces = []
    
    # Мачта башни (линия)
    mast_x = x + width/2
    mast_y = y + depth/2
    
    traces.append(go.Scatter3d(
        x=[mast_x, mast_x], y=[mast_y, mast_y], z=[0, height * 0.9],
        mode='lines',
        line=dict(color=color, width=6)
    ))
    
    # Антенны на вершине
    for i in range(antenna_count):
        angle = 2 * np.pi * i / antenna_count
        ant_x = mast_x + 20 * np.cos(angle)
        ant_y = mast_y + 20 * np.sin(angle)
        
        traces.append(go.Scatter3d(
            x=[mast_x, ant_x], y=[mast_y, ant_y], z=[height * 0.9, height],
            mode='lines',
            line=dict(color=color, width=4)
        ))
    
    # Сигнальные огни
    for i in range(5):
        light_z = i * (height * 0.2)
        opacity = 0.5 + (signal_strength / 5) * 0.5
        
        traces.append(go.Scatter3d(
            x=[mast_x], y=[mast_y], z=[light_z],
            mode='markers',
            marker=dict(size=8, color='#FF0000', opacity=opacity)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 RadioTower3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: башни добавляют антенны и передают сигнал

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    RadioTower3D(2.0, 1.0, 0.3, 200, "T1"),
    RadioTower3D(4.0, 1.0, 0.3, 210, "T2")
]

# Коэффициенты роста
growth_rates = [15, 20, 25, 25]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, RadioTower3D) and step >= 4:
            # Башни добавляют антенны на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.antenna_count += 1
            elem.antenna_path.append(elem.antenna_count)
            elem.signal_strength += 1
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (серый цвет)
- Все здания с текущей высотой из `path[step]`
- Все башни с текущим количеством антенн из `antenna_path[step]`
- Цветовую динамику от серого к красному

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
        colorscale=[[0, '#808080'], [1, '#404040']], 
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
        elif isinstance(elem, RadioTower3D):
            color = elem.get_color()
            a = elem.antenna_path[step] if step < len(elem.antenna_path) else elem.antenna_path[-1]
            traces.extend(create_tower_mesh(
                elem.x, elem.y, elem.width, 0.4, h, a, elem.signal_strength, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Телевизионная башня".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора телекоммуникационной архитектуры
- Добавлены подписи и заголовок с темой "От серого к красному"

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
    title="📡 Анимация: Телевизионная башня (градиент от серого к красному)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами и башни с 1 антенной
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Телевизионные башни начинают добавлять антенны (шаги 4-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному красному (`#DC143C`)
  - Количество антенн увеличивается с 1 до 5-6
  - Сигнальные огни становятся ярче с увеличением силы сигнала
  - Мачта башни становится выше
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" телекоммуникационной инфраструктуры
