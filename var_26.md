# **Вариант № 26**

## **Генеративная художественная композиция "Эко-здание с солнечными панелями" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей современной эко-архитектуры, который хочет получить интерактивную генеративную инсталляцию "Эко-здание с солнечными панелями". Инсталляция должна создавать уникальные композиции с эко-зданиями, где здания и солнечные панели оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и солнечные панели — это "живые" объекты, способные расти, менять цвет от серого к синему и реагировать на взаимодействие, создавая динамичные композиции в стиле современной эко-архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
solar_panel_count = 6  # количество солнечных панелей
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к синему. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и солнечных панелей, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в эко-стиле
- **`SolarPanel`** — солнечная панель (прямоугольная панель на крыше)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в эко-стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить места для солнечных панелей
        pass

class SolarPanel(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать панель (прямоугольник с сеткой)
        # TODO: добавить крепления к крыше
        # TODO: добавить индикатор энергии
        # TODO: использовать градиент от серого к синему
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_solar_to_building(building, count)` добавляет солнечные панели к зданию
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
    
    def add_solar_to_building(self, building, count):
        # TODO: создать солнечные панели на крыше здания
        # TODO: позиция: building.x + offset, building.y + building.height
        # TODO: добавить панели в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от серого к синему
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'solar', 'energy')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #808080 к #4169E1 (ratio от 0 до 1)

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
                'base_blue': '#4169E1'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от серого к синему усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от серого к синему
        # ratio: 0.0 = серый, 1.0 = синий
        from matplotlib.colors import to_rgb, to_hex
        gray = to_rgb('#808080')
        blue = to_rgb('#4169E1')
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(gray, blue))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 6 солнечными панелями на крышах, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - По 2 SolarPanel на каждом здании

# Пример создания солнечной панели:
# panel = SolarPanel(building.x + offset, building.y + building.height, 
#                    40, 20, "Panel1")
# canvas.add_element(panel)

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
        # TODO: создать Rectangle для фасада в эко-стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#4169E1')
                ax.add_patch(window)
        # TODO: добавить зелёные элементы (растения на балконах)
        for i in range(3):
            plant_x = self.x + self.width * 0.2 + i * (self.width * 0.3)
            plant = Circle((plant_x, self.y + self.height * 0.5), 8, 
                          facecolor='#228B22', edgecolor='none')
            ax.add_patch(plant)

class SolarPanel(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 50)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать панель (прямоугольник с сеткой)
        panel = Rectangle((self.x, self.y), self.width, self.height, 
                         facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(panel)
        # TODO: добавить сетку панелей (ячейки)
        cell_w = self.width / 4
        cell_h = self.height / 2
        for row in range(2):
            for col in range(4):
                cx = self.x + col * cell_w
                cy = self.y + row * cell_h
                cell = Rectangle((cx, cy), cell_w, cell_h, 
                                facecolor='none', edgecolor='#4169E1', linewidth=1)
                ax.add_patch(cell)
        # TODO: добавить индикатор энергии (светящаяся точка)
        indicator = Circle((self.x + self.width/2, self.y + self.height/2), 
                          5, facecolor='#00FF00', edgecolor='none')
        ax.add_patch(indicator)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и солнечные панели должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" эко-архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать панель с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к синему в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления солнечных панелей к зданию, где панели появляются постепенно и начинают генерировать энергию, а цветовая динамика подчёркивает процесс "оживления" эко-здания.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для SolarPanel: высота должна быть достаточной для ячеек (не менее 15px)

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
        self.solar_level = 0  # уровень солнечных панелей
    
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

class SolarPanel(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.energy_level = 0  # уровень генерации энергии
        self.cell_count = 8  # количество ячеек
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 15:  # минимальная высота для ячеек
            value = 15
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_solar()` — добавляет солнечную панель к зданию
- `get_color()` — возвращает цвет в градиенте от серого к синему

**Для SolarPanel**:
- `add_solar()` — увеличивает размер панели
- `generate_energy()` — увеличивает уровень генерации энергии
- `get_color()` — возвращает цвет в градиенте от серого к синему

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_solar(self):
        # TODO: увеличить solar_level
        self.solar_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к синему
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если solar_level > 0: добавить солнечные панели
        pass

class SolarPanel(ArchElement):
    def add_solar(self):
        # TODO: увеличить размер панели
        self.height += 5
        self.width += 5
        self.history.append(self._height)
        return self
    
    def generate_energy(self):
        # TODO: увеличить уровень генерации энергии
        self.energy_level += 1
        return self
    
    def get_color(self):
        # TODO: градиент от серого к синему по высоте и энергии
        ratio = min(1.0, (self._height / 50 + self.energy_level / 5) / 2)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать панель с учётом energy_level
        # TODO: отрисовать ячейки и индикатор
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление солнечных панелей к зданию

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
                'solar': getattr(elem, 'solar_level', 0),
                'energy': getattr(elem, 'energy_level', 0)
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
        ax.set_title(f"Шаг {step}: Эко-здание с солнечными панелями")
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

Создайте композицию из 2 зданий и 4 солнечных панелей. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые солнечные панели
- Шаги 4-5: панели растут, генерируют энергию
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

panels = [
    SolarPanel(70, 230, 40, 20, "P1", 800, 600),
    SolarPanel(130, 230, 40, 20, "P2", 800, 600),
    SolarPanel(320, 225, 40, 20, "P3", 800, 600),
    SolarPanel(380, 225, 40, 20, "P4", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for p in panels:
    canvas.add_element(p)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Панели растут и генерируют энергию
        for elem in panels:
            elem.add_solar()
            elem.generate_energy()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, SolarPanel):
        e = getattr(elem, 'energy_level', 0)
        print(f"{elem.name}: высота={h}, энергия={e}")
    else:
        s = getattr(elem, 'solar_level', 0)
        print(f"{elem.name}: высота={h}, панели={s}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и панели серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится синеватым
- Шаг 3: появляются первые солнечные панели на крышах
- Шаг 4-5: панели растут, начинают генерировать энергию (индикаторы светятся)
- Шаг 6: финальная композиция с градиентом от серого к насыщенному синему

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→синий
- Появлению солнечных панелей на крышах
- Свечению индикаторов энергии
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как эко-здание с солнечными панелями "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление панелей и трансформацию цветовой схемы от серого к синему. Важно показать разнообразие: здания и панели должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `SolarPanel3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для панели дополнительно хранить `self.energy_path` (история уровня энергии)
- Метод `get_color()` возвращает цвет в градиенте от серого к синему

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
        # TODO: вернуть цвет в градиенте от серого к синему
        from matplotlib.colors import to_hex, to_rgb
        gray = to_rgb('#808080')
        blue = to_rgb('#4169E1')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(gray, blue))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class SolarPanel3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.energy_level = 0
        self.energy_level = 0
        # TODO: создать self.energy_path = [0]
        self.energy_path = [0]
        self.cell_count = 8
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты и энергии
        self.color_ratio = min(1.0, (self.path[-1] / 50 + self.energy_level / 5) / 2)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с эко-элементами
- `create_solar_mesh(x, y, width, depth, height, energy_level, color)` — солнечная панель (плоскость с ячейками)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с эко-элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_solar_mesh(x, y, width, depth, height, energy_level, color):
    """Создает солнечную панель: плоскость с ячейками"""
    traces = []
    
    # Основная панель (плоскость)
    panel_x = [x, x+width, x+width, x]
    panel_y = [y, y, y+depth, y+depth]
    panel_z = [height, height, height, height]
    
    panel_mesh = go.Mesh3d(
        x=panel_x, y=panel_y, z=panel_z,
        color=color, opacity=0.9,
        alphahull=0
    )
    traces.append(panel_mesh)
    
    # Ячейки панели (сетка)
    cell_w = width / 4
    cell_d = depth / 2
    for row in range(2):
        for col in range(4):
            cx = x + col * cell_w
            cy = y + row * cell_d
            
            # Границы ячейки
            traces.append(go.Scatter3d(
                x=[cx, cx+cell_w, cx+cell_w, cx, cx],
                y=[cy, cy, cy+cell_d, cy+cell_d, cy],
                z=[height]*5,
                mode='lines',
                line=dict(color='#4169E1', width=2)
            ))
    
    # Индикатор энергии (светящаяся точка)
    if energy_level > 0:
        traces.append(go.Scatter3d(
            x=[x + width/2], y=[y + depth/2], z=[height + 5],
            mode='markers',
            marker=dict(size=8, color='#00FF00', opacity=0.8)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 6 элементов (2 Building3D + 4 SolarPanel3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: панели растут и генерируют энергию

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    SolarPanel3D(1.2, 1.0, 0.4, 20, "P1"),
    SolarPanel3D(1.6, 1.0, 0.4, 20, "P2"),
    SolarPanel3D(3.2, 1.0, 0.4, 20, "P3"),
    SolarPanel3D(3.6, 1.0, 0.4, 20, "P4")
]

# Коэффициенты роста
growth_rates = [15, 20, 5, 5, 5, 5]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, SolarPanel3D) and step >= 4:
            # Панели растут и генерируют энергию на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.energy_level += 1
            elem.energy_path.append(elem.energy_level)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (зелёный цвет)
- Все здания с текущей высотой из `path[step]`
- Все панели с текущим уровнем энергии из `energy_path[step]`
- Цветовую динамику от серого к синему

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
        elif isinstance(elem, SolarPanel3D):
            color = elem.get_color()
            e = elem.energy_path[step] if step < len(elem.energy_path) else elem.energy_path[-1]
            traces.extend(create_solar_mesh(
                elem.x, elem.y, elem.width, 0.4, h, e, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Эко-здание с солнечными панелями".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора эко-архитектуры
- Добавлены подписи и заголовок с темой "От серого к синему"

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
    title="☀️ Анимация: Эко-здание с солнечными панелями (градиент от серого к синему)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами и панели без энергии
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Солнечные панели начинают расти и генерировать энергию (шаги 4-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному синему (`#4169E1`)
  - Уровень энергии увеличивается с 0 до 4-5
  - Индикаторы энергии светятся зелёным цветом
  - Ячейки панелей становятся более заметными
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" современной эко-архитектуры
