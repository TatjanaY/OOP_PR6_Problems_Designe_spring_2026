# **Вариант № 28**

## **Генеративная художественная композиция "Водонапорная башня" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей коммунальной архитектуры, который хочет получить интерактивную генеративную инсталляцию "Водонапорная башня". Инсталляция должна создавать уникальные композиции с водонапорными башнями, где здания и резервуары оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и водонапорные башни — это "живые" объекты, способные расти, менять цвет от серого к белому и реагировать на взаимодействие, создавая динамичные композиции в стиле индустриальной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
tank_count = 2  # количество водонапорных башен
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к белому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и водонапорных башен, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в индустриальном стиле
- **`WaterTank`** — водонапорная башня (резервуар на опорах)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в индустриальном стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить трубы/вентиляцию
        pass

class WaterTank(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать резервуар (цилиндр/прямоугольник)
        # TODO: добавить опоры (вертикальные линии)
        # TODO: добавить трубы подключения
        # TODO: использовать градиент от серого к белому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_tank_near(building)` создаёт водонапорную башню рядом со зданием
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
    
    def add_tank_near(self, building):
        # TODO: создать водонапорную башню рядом со зданием
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от серого к белому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'tank', 'water')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #808080 к #FFFFFF (ratio от 0 до 1)

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
                'base_white': '#FFFFFF'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от серого к белому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от серого к белому
        # ratio: 0.0 = серый, 1.0 = белый
        from matplotlib.colors import to_rgb, to_hex
        gray = to_rgb('#808080')
        white = to_rgb('#FFFFFF')
        interpolated = tuple(g + (w - g) * ratio for g, w in zip(gray, white))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 2 водонапорными башнями, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 WaterTank рядом с зданиями

# Пример создания водонапорной башни:
# tank = WaterTank(building.x + building.width + 50, building.y, 
#                  60, 180, "Tank1")
# canvas.add_element(tank)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Circle, Ellipse

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в индустриальном стиле
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
        # TODO: добавить трубы/вентиляцию
        vent = Circle((self.x + self.width/2, self.y + self.height), 15, 
                     facecolor='#404040', edgecolor='none')
        ax.add_patch(vent)

class WaterTank(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 250)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать резервуар (цилиндр/эллипс)
        tank = Ellipse((self.x + self.width/2, self.y + self.height * 0.8), 
                      self.width, self.height * 0.4, 
                      facecolor=color, edgecolor='#404040', linewidth=2)
        ax.add_patch(tank)
        # TODO: добавить опоры (вертикальные линии)
        for i in range(4):
            leg_x = self.x + self.width * (i + 1) / 5
            ax.plot([leg_x, leg_x], [self.y, self.y + self.height * 0.8], 
                   color='#404040', linewidth=3)
        # TODO: добавить трубы подключения
        pipe_x = self.x + self.width/2
        ax.plot([pipe_x, pipe_x], [self.y + self.height * 0.8, self.y + self.height], 
               color='#404040', linewidth=4)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и водонапорные башни должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" коммунальной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать башню с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к белому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию заполнения резервуара водой, где уровень воды поднимается постепенно, а цветовая динамика подчёркивает процесс "оживления" водонапорной системы.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для WaterTank: высота должна быть достаточной для резервуара (не менее 120px)

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
        self.tank_level = 0  # уровень башни
    
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

class WaterTank(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.water_level = 0  # уровень воды в резервуаре
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 120:  # минимальная высота для резервуара
            value = 120
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_tank_decor()` — добавляет индустриальный декор
- `get_color()` — возвращает цвет в градиенте от серого к белому

**Для WaterTank**:
- `add_tank()` — увеличивает высоту башни
- `fill_water()` — заполняет резервуар водой
- `get_color()` — возвращает цвет в градиенте от серого к белому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_tank_decor(self):
        # TODO: увеличить tank_level
        self.tank_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к белому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если tank_level > 0: добавить индустриальный декор
        pass

class WaterTank(ArchElement):
    def add_tank(self):
        # TODO: увеличить высоту башни
        self.height += 15
        self.history.append(self._height)
        return self
    
    def fill_water(self):
        # TODO: увеличить уровень воды
        self.water_level += 1
        return self
    
    def get_color(self):
        # TODO: градиент от серого к белому по высоте
        ratio = min(1.0, self._height / 250)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать башню с учётом water_level
        # TODO: отрисовать резервуар, опоры и уровень воды
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное заполнение резервуара водой

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
                'water': getattr(elem, 'water_level', 0),
                'tanks': getattr(elem, 'tank_level', 0)
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
        ax.set_title(f"Шаг {step}: Водонапорная башня")
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

Создайте композицию из 2 зданий и 2 водонапорных башен. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые башни
- Шаги 4-5: башни растут, резервуары заполняются водой
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

tanks = [
    WaterTank(180, 100, 60, 150, "T1", 800, 600),
    WaterTank(430, 100, 60, 160, "T2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for t in tanks:
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
        # Башни растут и заполняются водой
        for elem in tanks:
            elem.add_tank()
            elem.fill_water()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, WaterTank):
        w = getattr(elem, 'water_level', 0)
        print(f"{elem.name}: высота={h}, вода={w}")
    else:
        t = getattr(elem, 'tank_level', 0)
        print(f"{elem.name}: высота={h}, башни={t}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и башни серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится белее
- Шаг 3: появляются первые водонапорные башни
- Шаг 4-5: башни растут, резервуары заполняются водой
- Шаг 6: финальная композиция с градиентом от серого к насыщенному белому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→белый
- Появлению водонапорных башен
- Заполнению резервуаров водой
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как коммунальная система с водонапорными башнями "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление башен и трансформацию цветовой схемы от серого к белому. Важно показать разнообразие: здания и башни должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `WaterTank3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для башни дополнительно хранить `self.water_path` (история уровня воды)
- Метод `get_color()` возвращает цвет в градиенте от серого к белому

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
        # TODO: вернуть цвет в градиенте от серого к белому
        from matplotlib.colors import to_hex, to_rgb
        gray = to_rgb('#808080')
        white = to_rgb('#FFFFFF')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (w - g) * ratio for g, w in zip(gray, white))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class WaterTank3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.water_level = 0
        self.water_level = 0
        # TODO: создать self.water_path = [0]
        self.water_path = [0]
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 250)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с индустриальными элементами
- `create_tank_mesh(x, y, width, depth, height, water_level, color)` — водонапорная башня (резервуар + опоры)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с индустриальными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_tank_mesh(x, y, width, depth, height, water_level, color):
    """Создает водонапорную башню: резервуар + опоры"""
    traces = []
    
    # Резервуар (цилиндр)
    n_segments = 32
    center_x, center_y = x + width/2, y + depth/2
    radius = width/2
    tank_height = height * 0.4
    tank_z = height * 0.6
    
    tank_x, tank_y, tank_z_list = [], [], []
    for angle in np.linspace(0, 2*np.pi, n_segments):
        tank_x.append(center_x + radius * np.cos(angle))
        tank_y.append(center_y + radius * np.sin(angle))
        tank_z_list.append(tank_z)
    
    tank_top_z = [tank_z + tank_height] * n_segments
    
    tank_mesh = go.Mesh3d(
        x=tank_x + tank_x,
        y=tank_y + tank_y,
        z=tank_z_list + tank_top_z,
        color=color, opacity=0.9,
        alphahull=0
    )
    traces.append(tank_mesh)
    
    # Опоры (линии)
    for i in range(4):
        angle = 2 * np.pi * i / 4
        leg_x = center_x + radius * 0.8 * np.cos(angle)
        leg_y = center_y + radius * 0.8 * np.sin(angle)
        
        traces.append(go.Scatter3d(
            x=[leg_x, leg_x], y=[leg_y, leg_y], z=[0, tank_z],
            mode='lines',
            line=dict(color='#404040', width=4)
        ))
    
    # Вода в резервуаре (синяя)
    if water_level > 0:
        water_z = tank_z + (water_level / 5) * tank_height
        traces.append(go.Scatter3d(
            x=[center_x], y=[center_y], z=[water_z],
            mode='markers',
            marker=dict(size=15, color='#4682B4', opacity=0.7)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 WaterTank3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: башни растут и заполняются водой

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    WaterTank3D(2.0, 1.0, 0.6, 150, "T1"),
    WaterTank3D(4.0, 1.0, 0.6, 160, "T2")
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
        elif isinstance(elem, WaterTank3D) and step >= 4:
            # Башни растут и заполняются на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.water_level += 1
            elem.water_path.append(elem.water_level)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (серый цвет)
- Все здания с текущей высотой из `path[step]`
- Все башни с текущим уровнем воды из `water_path[step]`
- Цветовую динамику от серого к белому

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
        elif isinstance(elem, WaterTank3D):
            color = elem.get_color()
            w = elem.water_path[step] if step < len(elem.water_path) else elem.water_path[-1]
            traces.extend(create_tank_mesh(
                elem.x, elem.y, elem.width, 0.4, h, w, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Водонапорная башня".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора индустриальной архитектуры
- Добавлены подписи и заголовок с темой "От серого к белому"

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
    title="🏭 Анимация: Водонапорная башня (градиент от серого к белому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами и башни с пустыми резервуарами
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Водонапорные башни начинают расти и заполняться водой (шаги 4-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному белому (`#FFFFFF`)
  - Уровень воды в резервуарах увеличивается с 0 до 4-5
  - Опоры башен видны в тёмно-сером цвете
  - Вода в резервуарах светится синим цветом
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" коммунальной инфраструктуры
