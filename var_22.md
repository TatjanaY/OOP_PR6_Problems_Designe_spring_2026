# **Вариант № 22**

## **Генеративная художественная композиция "Здание с наружной лестницей" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей городской архитектуры, который хочет получить интерактивную генеративную инсталляцию "Здание с наружной лестницей". Инсталляция должна создавать уникальные композиции с зданиями и наружными лестницами, где архитектурные элементы оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и лестницы — это "живые" объекты, способные расти, менять цвет от серого к синему и реагировать на взаимодействие, создавая динамичные композиции в стиле городской застройки с функциональными элементами.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
stairs_count = 2  # количество лестниц
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к синему. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и наружных лестниц, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в городском стиле
- **`Stairs`** — наружная лестница (ступени с перилами)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в городском стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить места для крепления лестницы
        pass

class Stairs(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать ступени (прямоугольники со смещением)
        # TODO: добавить перила (линии вдоль лестницы)
        # TODO: добавить площадку наверху
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
- метод `add_stairs_to_building(building)` создаёт лестницу у здания
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
    
    def add_stairs_to_building(self, building):
        # TODO: создать лестницу у здания
        # TODO: позиция: building.x + building.width, building.y
        # TODO: добавить лестницу в elements
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
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'stairs', 'railing')
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

Создайте композицию из 3 зданий с 2 лестницами у них, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Stairs у зданий

# Пример создания лестницы:
# stairs = Stairs(building.x + building.width, building.y, 
#                 60, 150, "Stairs1")
# canvas.add_element(stairs)

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
        # TODO: создать Rectangle для фасада в городском стиле
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
        # TODO: добавить места для крепления лестницы
        for i in range(3):
            anchor_y = self.y + i * (self.height / 3)
            anchor = Circle((self.x + self.width, anchor_y), 5, 
                           facecolor='#404040', edgecolor='none')
            ax.add_patch(anchor)

class Stairs(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 200)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать ступени (прямоугольники со смещением)
        num_steps = 8
        step_height = self.height / num_steps
        step_width = self.width / num_steps
        for i in range(num_steps):
            step_x = self.x + i * step_width
            step_y = self.y + i * step_height
            step = Rectangle((step_x, step_y), step_width, step_height, 
                           facecolor=color, edgecolor='#404040', linewidth=1)
            ax.add_patch(step)
        # TODO: добавить перила (линии вдоль лестницы)
        ax.plot([self.x, self.x + self.width], 
               [self.y, self.y + self.height], 
               color='#404040', linewidth=3)
        ax.plot([self.x + 10, self.x + self.width + 10], 
               [self.y, self.y + self.height], 
               color='#404040', linewidth=3)
        # TODO: добавить площадку наверху
        platform = Rectangle((self.x + self.width - 20, self.y + self.height), 
                            40, 15, facecolor=color, edgecolor='#404040')
        ax.add_patch(platform)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и лестницы должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" городской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать лестницу с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к синему в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления ступеней к лестнице, где ступени появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" здания с наружной лестницей.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Stairs: высота должна быть достаточной для ступеней (не менее 80px)

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
        self.stairs_level = 0  # уровень лестницы
    
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

class Stairs(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.num_steps = 8  # количество ступеней
        self.has_railing = True  # есть ли перила
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 80:  # минимальная высота для ступеней
            value = 80
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_stairs_decor()` — добавляет декор для лестницы
- `get_color()` — возвращает цвет в градиенте от серого к синему

**Для Stairs**:
- `add_stairs(n)` — добавляет n ступеней к лестнице
- `add_railing()` — добавляет перила к лестнице
- `get_color()` — возвращает цвет в градиенте от серого к синему

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_stairs_decor(self):
        # TODO: увеличить stairs_level
        self.stairs_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к синему
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если stairs_level > 0: добавить декор для лестницы
        pass

class Stairs(ArchElement):
    def add_stairs(self, n):
        # TODO: увеличить количество ступеней на n
        self.num_steps += n
        # TODO: увеличить высоту пропорционально
        self.height += 15 * n
        # TODO: добавить запись в history
        self.history.append(self._height)
        # TODO: вернуть self
        return self
    
    def add_railing(self):
        # TODO: установить has_railing = True
        self.has_railing = True
        return self
    
    def get_color(self):
        # TODO: градиент от серого к синему по высоте
        ratio = min(1.0, self._height / 200)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать лестницу с учётом num_steps и has_railing
        # TODO: отрисовать ступени, перила и площадку
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление ступеней к лестнице

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
                'steps': getattr(elem, 'num_steps', 0),
                'railing': getattr(elem, 'has_railing', False)
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
        ax.set_title(f"Шаг {step}: Здание с наружной лестницей")
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

Создайте композицию из 2 зданий и 2 лестниц. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые ступени лестницы
- Шаги 4-5: лестницы растут, добавляются ступени
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

stairs = [
    Stairs(150, 100, 60, 100, "S1", 800, 600),
    Stairs(400, 100, 60, 105, "S2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for s in stairs:
    canvas.add_element(s)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Лестницы добавляют ступени
        for elem in stairs:
            elem.add_stairs(1)
            elem.add_railing()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    s = getattr(elem, 'num_steps', 0)
    print(f"{elem.name}: высота={h}, ступени={s}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и лестницы серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится синеватым
- Шаг 3: лестницы получают первые ступени (с 8 до 9)
- Шаг 4-5: лестницы продолжают расти, добавляются новые ступени (до 10-11)
- Шаг 6: финальная композиция с градиентом от серого к насыщенному синему

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→синий
- Появлению новых ступеней на лестницах
- Добавлению перил на лестницах
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как здание с наружной лестницей "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление ступеней и трансформацию цветовой схемы от серого к синему. Важно показать разнообразие: здания и лестницы должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Stairs3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для лестницы дополнительно хранить `self.step_path` (история количества ступеней)
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


class Stairs3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.num_steps = 8
        self.num_steps = 8
        # TODO: создать self.step_path = [8]
        self.step_path = [8]
        self.has_railing = True
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 200)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с городскими элементами
- `create_stairs_mesh(x, y, width, depth, height, num_steps, has_railing, color)` — лестница (ступени + перила)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с городскими элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_stairs_mesh(x, y, width, depth, height, num_steps, has_railing, color):
    """Создает лестницу: ступени + перила"""
    traces = []
    
    step_height = height / num_steps
    step_width = width / num_steps
    
    # Ступени (прямоугольные блоки)
    for i in range(num_steps):
        step_x = x + i * step_width
        step_y = y
        step_z = i * step_height
        
        traces.append(create_building_mesh(
            step_x, step_y, step_width, depth, step_height, color
        ))
    
    # Перила (линии вдоль лестницы)
    if has_railing:
        traces.append(go.Scatter3d(
            x=[x, x + width], y=[y + depth, y + depth], z=[0, height],
            mode='lines',
            line=dict(color='#404040', width=4)
        ))
        traces.append(go.Scatter3d(
            x=[x + 10, x + width + 10], y=[y + depth, y + depth], z=[0, height],
            mode='lines',
            line=dict(color='#404040', width=4)
        ))
    
    # Площадка наверху
    platform_z = height
    traces.append(create_building_mesh(
        x + width - 20, y, 40, depth, 15, color
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Stairs3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: лестницы добавляют ступени

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Stairs3D(1.8, 1.0, 0.6, 100, "S1"),
    Stairs3D(3.9, 1.0, 0.6, 105, "S2")
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
        elif isinstance(elem, Stairs3D) and step >= 4:
            # Лестницы добавляют ступени на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.num_steps += 1
            elem.step_path.append(elem.num_steps)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (серый цвет)
- Все здания с текущей высотой из `path[step]`
- Все лестницы с текущим количеством ступеней из `step_path[step]`
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
        elif isinstance(elem, Stairs3D):
            color = elem.get_color()
            s = elem.step_path[step] if step < len(elem.step_path) else elem.step_path[-1]
            traces.extend(create_stairs_mesh(
                elem.x, elem.y, elem.width, 0.4, h, s, elem.has_railing, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Здание с наружной лестницей".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора городской застройки
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
    title="🪜 Анимация: Здание с наружной лестницей (градиент от серого к синему)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами и лестницы с 8 ступенями
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Лестницы начинают добавлять ступени (шаги 4-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному синему (`#4169E1`)
  - Количество ступеней увеличивается с 8 до 11-12
  - Перила появляются на лестницах
  - Площадка наверху лестницы видна
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" городской застройки с функциональными элементами
