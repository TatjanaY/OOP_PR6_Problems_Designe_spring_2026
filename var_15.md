# **Вариант № 15**

## **Генеративная художественная композиция "Здание с часами (Big Ben)" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей британской истории, который хочет получить интерактивную генеративную инсталляцию "Здание с часами (Big Ben)". Инсталляция должна создавать уникальные композиции с часовой башней, где здания, шпили и циферблаты оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания, шпили и часы — это "живые" объекты, способные расти, менять цвет от серого к коричневому и реагировать на взаимодействие, создавая динамичные композиции в стиле викторианской архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
clock_tower_count = 2  # количество часовых башен
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к коричневому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий, шпилей и часов, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в викторианском стиле
- **`Spire`** — шпиль (высокая заострённая башня)
- **`Clock`** — циферблат часов с римскими цифрами

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в викторианском стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить декоративные элементы
        pass

class Spire(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать шпиль (сужающийся кверху)
        # TODO: добавить декоративные элементы на шпиле
        # TODO: использовать градиент от серого к коричневому
        pass

class Clock(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать круглый циферблат
        # TODO: добавить римские цифры (XII, III, VI, IX)
        # TODO: добавить стрелки часов (часовая и минутная)
        # TODO: использовать градиент от серого к коричневому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_clock_tower(building)` создаёт часовую башню рядом со зданием
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
    
    def add_clock_tower(self, building):
        # TODO: создать часовую башню рядом со зданием
        # TODO: позиция: building.x + building.width + 30, building.y
        # TODO: добавить шпиль и часы в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от серого к коричневому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'spire', 'clock')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #808080 к #8B4513 (ratio от 0 до 1)

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
                'base_brown': '#8B4513'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от серого к коричневому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от серого к коричневому
        # ratio: 0.0 = серый, 1.0 = коричневый
        from matplotlib.colors import to_rgb, to_hex
        gray = to_rgb('#808080')
        brown = to_rgb('#8B4513')
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(gray, brown))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 2 часовыми башнями, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Clock Tower рядом с зданиями

# Пример создания часовой башни:
# spire = Spire(building.x + building.width + 30, building.y, 
#               40, 200, "Spire1")
# clock = Clock(building.x + building.width + 40, building.y + 150, 
#               60, 60, "Clock1")
# canvas.add_element(spire)
# canvas.add_element(clock)

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
        # TODO: создать Rectangle для фасада в викторианском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#654321', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#654321')
                ax.add_patch(window)
        # TODO: добавить декоративные элементы (карнизы)
        for i in range(4):
            cornice_y = self.y + i * (self.height / 4)
            ax.plot([self.x - 5, self.x + self.width + 5], 
                   [cornice_y, cornice_y], color='#654321', linewidth=2)

class Spire(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 300)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать шпиль (сужающийся кверху)
        base_w = self.width
        top_w = self.width * 0.3
        spire_body = Polygon([
            (self.x, self.y),
            (self.x + base_w, self.y),
            (self.x + base_w/2 + top_w/2, self.y + self.height),
            (self.x + base_w/2 - top_w/2, self.y + self.height)
        ], facecolor=color, edgecolor='#654321', linewidth=2)
        ax.add_patch(spire_body)
        # TODO: добавить декоративные элементы на шпиле
        for i in range(5):
            deco_y = self.y + i * (self.height / 5)
            deco_w = base_w - (base_w - top_w) * i / 5
            ax.plot([self.x + (base_w - deco_w)/2, 
                    self.x + (base_w + deco_w)/2], 
                   [deco_y, deco_y], color='#654321', linewidth=1)

class Clock(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 100)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать круглый циферблат
        center_x = self.x + self.width / 2
        center_y = self.y + self.height / 2
        radius = self.width / 2
        clock_face = Circle((center_x, center_y), radius, 
                           facecolor='#F5F5DC', edgecolor='#654321', linewidth=3)
        ax.add_patch(clock_face)
        # TODO: добавить римские цифры (XII, III, VI, IX)
        roman_nums = ['XII', 'III', 'VI', 'IX']
        angles = [90, 0, -90, 180]  # в градусах
        for i, num in enumerate(roman_nums):
            angle_rad = np.radians(angles[i])
            num_x = center_x + radius * 0.7 * np.cos(angle_rad)
            num_y = center_y + radius * 0.7 * np.sin(angle_rad)
            ax.text(num_x, num_y, num, ha='center', va='center', 
                   fontsize=10, color='#654321', fontweight='bold')
        # TODO: добавить стрелки часов
        # Часовая стрелка (короткая)
        hour_angle = np.radians(30)  # 1 час
        hour_x = center_x + radius * 0.4 * np.cos(hour_angle)
        hour_y = center_y + radius * 0.4 * np.sin(hour_angle)
        ax.plot([center_x, hour_x], [center_y, hour_y], 
               color='#654321', linewidth=3)
        # Минутная стрелка (длинная)
        minute_angle = np.radians(120)  # 20 минут
        minute_x = center_x + radius * 0.6 * np.cos(minute_angle)
        minute_y = center_y + radius * 0.6 * np.sin(minute_angle)
        ax.plot([center_x, minute_x], [center_y, minute_y], 
               color='#654321', linewidth=2)
        # Центр стрелок
        center_dot = Circle((center_x, center_y), 5, 
                           facecolor='#654321', edgecolor='none')
        ax.add_patch(center_dot)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания, шпили и часы должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" викторианской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать часы с отрицательным диаметром или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к коричневому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления часов к башне, где циферблат появляется постепенно, а стрелки начинают двигаться, подчёркивая процесс "оживления" часовой башни.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Clock: диаметр должен быть достаточным для цифр (не менее 40px)

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
        self.clock_level = 0  # уровень часов
    
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

class Clock(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.hour_hand = 12  # часовая стрелка
        self.minute_hand = 0  # минутная стрелка
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 40:  # минимальный диаметр для цифр
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
- `add_clock_decor()` — добавляет декор часов
- `get_color()` — возвращает цвет в градиенте от серого к коричневому

**Для Spire**:
- `add_spire(height)` — увеличивает высоту шпиля
- `get_color()` — возвращает цвет в градиенте от серого к коричневому

**Для Clock**:
- `add_clock()` — добавляет циферблат
- `tick()` — двигает стрелки на 1 минуту
- `get_color()` — возвращает цвет в градиенте от серого к коричневому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_clock_decor(self):
        # TODO: увеличить clock_level
        self.clock_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к коричневому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если clock_level > 0: добавить декор часов
        pass

class Spire(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.history = [height]
    
    def add_spire(self, height):
        # TODO: увеличить высоту шпиля
        self.height += height
        self.history.append(self._height)
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)

class Clock(ArchElement):
    def add_clock(self):
        # TODO: увеличить размер циферблата
        self.height += 10
        self.width += 10
        self.history.append(self._height)
        return self
    
    def tick(self):
        # TODO: двигать минутную стрелку на 6 градусов (1 минута)
        self.minute_hand += 6
        if self.minute_hand >= 360:
            self.minute_hand = 0
            # TODO: двигать часовую стрелку на 0.5 градуса
            self.hour_hand += 0.5
            if self.hour_hand >= 360:
                self.hour_hand = 0
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 100)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление часов к башне

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
                'hour_hand': getattr(elem, 'hour_hand', 0),
                'minute_hand': getattr(elem, 'minute_hand', 0)
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
        ax.set_title(f"Шаг {step}: Здание с часами (Big Ben)")
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

Создайте композицию из 2 зданий и 2 часовых башен. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются шпили
- Шаги 4-5: появляются часы, стрелки двигаются
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

spires = [
    Spire(180, 100, 40, 180, "S1", 800, 600),
    Spire(430, 100, 40, 190, "S2", 800, 600)
]

clocks = [
    Clock(185, 200, 50, 50, "C1", 800, 600),
    Clock(435, 200, 50, 50, "C2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for s in spires:
    canvas.add_element(s)
for c in clocks:
    canvas.add_element(c)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step == 3:
        # Появление шпилей
        for elem in spires:
            elem.add_spire(20)
    elif step <= 5:
        # Часы появляются и тикают
        for elem in clocks:
            elem.add_clock()
            elem.tick()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, Clock):
        print(f"{elem.name}: высота={h}, время={elem.hour_hand}:{elem.minute_hand}")
    else:
        print(f"{elem.name}: высота={h}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания, шпили и часы серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится коричневым
- Шаг 3: шпили увеличиваются в высоту
- Шаг 4-5: часы появляются, стрелки начинают двигаться (тикать)
- Шаг 6: финальная композиция с градиентом от серого к насыщенному коричневому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→коричневый
- Появлению циферблатов часов
- Движению стрелок часов (тикание)
- Отображению времени в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как викторианская часовая башня "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление шпилей и часов и трансформацию цветовой схемы от серого к коричневому. Важно показать разнообразие: здания, шпили и часы должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D`, `Spire3D` и `Clock3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для часов дополнительно хранить `self.hand_path` (история положения стрелок)
- Метод `get_color()` возвращает цвет в градиенте от серого к коричневому

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
        # TODO: вернуть цвет в градиенте от серого к коричневому
        from matplotlib.colors import to_hex, to_rgb
        gray = to_rgb('#808080')
        brown = to_rgb('#8B4513')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(gray, brown))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Spire3D(ArchElement3D):
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Clock3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.hour_hand = 12
        self.hour_hand = 12
        # TODO: создать self.minute_hand = 0
        self.minute_hand = 0
        # TODO: создать self.hand_path = [(12, 0)]
        self.hand_path = [(12, 0)]
    
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 100)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с викторианскими элементами
- `create_spire_mesh(x, y, width, depth, height, color)` — шпиль (сужающаяся пирамида)
- `create_clock_mesh(x, y, width, depth, height, hour_hand, minute_hand, color)` — часы (циферблат + стрелки)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с викторианскими элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_spire_mesh(x, y, width, depth, height, color):
    """Создает шпиль: сужающаяся пирамида"""
    traces = []
    
    # Усечённая пирамида (шпиль)
    base_w, base_d = width, depth
    top_w, top_d = width * 0.3, depth * 0.3
    
    # 8 вершин усечённой пирамиды
    vertices_x = [x, x+base_w, x+base_w, x, 
                  x + (base_w-top_w)/2, x+base_w - (base_w-top_w)/2, 
                  x+base_w - (base_w-top_w)/2, x + (base_w-top_w)/2]
    vertices_y = [y, y, y+base_d, y+base_d,
                  y + (base_d-top_d)/2, y+base_d - (base_d-top_d)/2,
                  y+base_d - (base_d-top_d)/2, y + (base_d-top_d)/2]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,1,2,3,4,5,6,7,0,1,2,3]
    j = [1,2,3,0,5,6,7,4,4,5,6,7]
    k = [4,5,6,7,4,5,6,7,1,2,3,0]
    
    spire = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                     i=i, j=j, k=k, color=color, opacity=0.95)
    traces.append(spire)
    
    return traces


def create_clock_mesh(x, y, width, depth, height, hour_hand, minute_hand, color):
    """Создает часы: циферблат + стрелки"""
    traces = []
    
    # Циферблат (цилиндр)
    n_segments = 32
    center_x, center_y = x + width/2, y + depth/2
    radius = width/2
    
    clock_x, clock_y, clock_z = [], [], []
    for angle in np.linspace(0, 2*np.pi, n_segments):
        clock_x.append(center_x + radius * np.cos(angle))
        clock_y.append(center_y + radius * np.sin(angle))
        clock_z.append(height/2)
    
    clock_mesh = go.Mesh3d(
        x=clock_x, y=clock_y, z=clock_z,
        color='#F5F5DC', opacity=0.9,
        alphahull=0
    )
    traces.append(clock_mesh)
    
    # Часовая стрелка
    hour_angle = np.radians(hour_hand * 30 - 90)
    hour_x = [center_x, center_x + radius * 0.4 * np.cos(hour_angle)]
    hour_y = [center_y, center_y + radius * 0.4 * np.sin(hour_angle)]
    hour_z = [height/2 + 1, height/2 + 1]
    
    traces.append(go.Scatter3d(
        x=hour_x, y=hour_y, z=hour_z,
        mode='lines',
        line=dict(color='#654321', width=6)
    ))
    
    # Минутная стрелка
    minute_angle = np.radians(minute_hand * 6 - 90)
    minute_x = [center_x, center_x + radius * 0.6 * np.cos(minute_angle)]
    minute_y = [center_y, center_y + radius * 0.6 * np.sin(minute_angle)]
    minute_z = [height/2 + 2, height/2 + 2]
    
    traces.append(go.Scatter3d(
        x=minute_x, y=minute_y, z=minute_z,
        mode='lines',
        line=dict(color='#654321', width=4)
    ))
    
    # Римские цифры (маркеры)
    roman_positions = [(90, 'XII'), (0, 'III'), (-90, 'VI'), (180, 'IX')]
    for angle, num in roman_positions:
        rad = np.radians(angle)
        num_x = center_x + radius * 0.7 * np.cos(rad)
        num_y = center_y + radius * 0.7 * np.sin(rad)
        
        traces.append(go.Scatter3d(
            x=[num_x], y=[num_y], z=[height/2 + 3],
            mode='text',
            text=[num],
            textfont=dict(size=10, color='#654321')
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 5 элементов (2 Building3D + 2 Spire3D + 1 Clock3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-5**: шпили увеличиваются
- **Шаги 6-7**: часы появляются, стрелки двигаются

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Spire3D(2.0, 1.0, 0.4, 180, "S1"),
    Spire3D(4.0, 1.0, 0.5, 190, "S2"),
    Clock3D(2.5, 1.0, 0.5, 50, "C1")
]

# Коэффициенты роста
growth_rates = [15, 20, 18, 18, 10]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Spire3D) and step >= 4 and step <= 5:
            # Шпили растут на шагах 4-5
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Clock3D) and step >= 6:
            # Часы появляются и тикают на шагах 6-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.minute_hand += 15  # двигать на 15 минут
            if elem.minute_hand >= 60:
                elem.minute_hand = 0
                elem.hour_hand += 1
            elem.hand_path.append((elem.hour_hand, elem.minute_hand))
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все шпили с текущей высотой из `path[step]`
- Часы с текущим положением стрелок из `hand_path[step]`
- Цветовую динамику от серого к коричневому

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
        elif isinstance(elem, Spire3D):
            color = elem.get_color()
            traces.extend(create_spire_mesh(
                elem.x, elem.y, elem.width, 0.4, h, color
            ))
        elif isinstance(elem, Clock3D):
            color = elem.get_color()
            hands = elem.hand_path[step] if step < len(elem.hand_path) else elem.hand_path[-1]
            traces.extend(create_clock_mesh(
                elem.x, elem.y, elem.width, 0.4, h, hands[0], hands[1], color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Здание с часами (Big Ben)".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора викторианской архитектуры
- Добавлены подписи и заголовок с темой "От серого к коричневому"

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
    title="🕐 Анимация: Здание с часами Big Ben (градиент от серого к коричневому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами, шпили и часы
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Шпили увеличиваются (шаги 4-5)
  - Часы появляются и стрелки начинают двигаться (шаги 6-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному коричневому (`#8B4513`)
  - Стрелки часов тикают (минутная двигается на 15 минут каждый шаг)
  - Римские цифры на циферблате видны (XII, III, VI, IX)
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" викторианского архитектурного ансамбля
