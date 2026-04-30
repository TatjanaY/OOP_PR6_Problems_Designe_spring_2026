# **Вариант № 32**

## **Генеративная художественная композиция "Театр с масками" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей театрального искусства, который хочет получить интерактивную генеративную инсталляцию "Театр с масками". Инсталляция должна создавать уникальные композиции с театральными зданиями, где здания и декоративные маски оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и маски — это "живые" объекты, способные расти, менять цвет от красного к золотому и реагировать на взаимодействие, создавая динамичные композиции в стиле театральной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
mask_count = 4  # количество масок на фасаде
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от красного к золотому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и театральных масок, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание театра с окнами и входом
- **`Mask`** — театральная маска (комедия/трагедия)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания театра
        # TODO: добавить арочные окна (3 ряда по 3 окна)
        # TODO: добавить главный вход с занавесом
        pass

class Mask(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать маску (овал с глазами и ртом)
        # TODO: добавить выражение (улыбка/грусть)
        # TODO: использовать градиент от красного к золотому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_masks_to_building(building, count)` добавляет маски к зданию театра
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
    
    def add_masks_to_building(self, building, count):
        # TODO: создать маски на фасаде здания
        # TODO: равномерно распределить маски по ширине здания
        # TODO: добавить маски в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от красного к золотому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'mask', 'curtain')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #DC143C к #FFD700 (ratio от 0 до 1)

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
                'base_gold': '#FFD700'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от красного к золотому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от красного к золотому
        # ratio: 0.0 = красный, 1.0 = золотой
        from matplotlib.colors import to_rgb, to_hex
        red = to_rgb('#DC143C')
        gold = to_rgb('#FFD700')
        interpolated = tuple(r + (g - r) * ratio for r, g in zip(red, gold))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий театра с 4 масками на каждом фасаде, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('evening')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (100, 400), (300, 380), (500, 420)
# - По 4 Mask на каждом здании

# Пример создания маски:
# mask = Mask(building.x + offset, building.y + building.height - 50, 
#             40, 40, "Mask1")
# canvas.add_element(mask)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Circle, Ellipse, Polygon

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада театра
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить арочные окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)
                # Арочное завершение
                arc = Polygon([
                    (wx, wy + window_h),
                    (wx + window_w, wy + window_h),
                    (wx + window_w/2, wy + window_h * 1.3)
                ], facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(arc)
        # TODO: добавить главный вход с занавесом
        curtain = Rectangle((self.x + self.width/2 - 30, self.y), 
                           60, self.height * 0.4,
                           facecolor='#DC143C', edgecolor='#8B4513', linewidth=2)
        ax.add_patch(curtain)

class Mask(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 80)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать маску (овал)
        center_x = self.x + self.width / 2
        center_y = self.y + self.height / 2
        face = Ellipse((center_x, center_y), self.width, self.height, 
                      facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(face)
        # TODO: добавить глаза
        eye_offset = self.width * 0.2
        left_eye = Circle((center_x - eye_offset, center_y + self.height * 0.1), 
                         self.width * 0.1, facecolor='#FFFFFF', edgecolor='#8B4513')
        right_eye = Circle((center_x + eye_offset, center_y + self.height * 0.1), 
                          self.width * 0.1, facecolor='#FFFFFF', edgecolor='#8B4513')
        ax.add_patch(left_eye)
        ax.add_patch(right_eye)
        # TODO: добавить рот (улыбка для комедии)
        mouth = Polygon([
            (center_x - self.width * 0.2, center_y - self.height * 0.1),
            (center_x, center_y - self.height * 0.2),
            (center_x + self.width * 0.2, center_y - self.height * 0.1)
        ], facecolor='#8B4513', edgecolor='#8B4513')
        ax.add_patch(mouth)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и маски должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" театральной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать маску с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от красного к золотому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления масок к театру, где маски появляются постепенно и меняют выражение, а цветовая динамика подчёркивает процесс "оживления" театрального здания.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Mask: высота должна быть достаточной для черт лица (не менее 30px)

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
        self.mask_level = 0  # уровень масок
    
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

class Mask(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.expression = 'comedy'  # comedy или tragedy
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 30:  # минимальная высота для черт лица
            value = 30
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_masks()` — добавляет маски к зданию
- `get_color()` — возвращает цвет в градиенте от красного к золотому

**Для Mask**:
- `add_mask()` — увеличивает размер маски
- `change_expression()` — меняет выражение маски (комедия/трагедия)
- `get_color()` — возвращает цвет в градиенте от красного к золотому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_masks(self):
        # TODO: увеличить mask_level
        self.mask_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от красного к золотому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если mask_level > 0: добавить маски
        pass

class Mask(ArchElement):
    def add_mask(self):
        # TODO: увеличить размер маски
        self.height += 10
        self.width += 10
        self.history.append(self._height)
        return self
    
    def change_expression(self):
        # TODO: изменить выражение маски
        self.expression = 'tragedy' if self.expression == 'comedy' else 'comedy'
        return self
    
    def get_color(self):
        # TODO: градиент от красного к золотому по высоте
        ratio = min(1.0, self._height / 80)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать маску с учётом expression
        # TODO: отрисовать лицо, глаза и рот
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление масок к театру

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
                'masks': getattr(elem, 'mask_level', 0),
                'expression': getattr(elem, 'expression', 'comedy')
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
        ax.set_title(f"Шаг {step}: Театр с масками")
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

Создайте композицию из 2 зданий и 4 масок. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые маски
- Шаги 4-5: маски растут, меняют выражение
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
    Building(100, 150, 120, 80, "B1", 800, 600),
    Building(400, 150, 120, 75, "B2", 800, 600)
]

masks = [
    Mask(120, 200, 40, 40, "M1", 800, 600),
    Mask(180, 200, 40, 40, "M2", 800, 600),
    Mask(420, 195, 40, 40, "M3", 800, 600),
    Mask(480, 195, 40, 40, "M4", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for m in masks:
    canvas.add_element(m)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Маски растут и меняют выражение
        for elem in masks:
            elem.add_mask()
            if step % 2 == 0:
                elem.change_expression()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, Mask):
        e = getattr(elem, 'expression', 'comedy')
        print(f"{elem.name}: высота={h}, выражение={e}")
    else:
        m = getattr(elem, 'mask_level', 0)
        print(f"{elem.name}: высота={h}, маски={m}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и маски красного цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится золотистым
- Шаг 3: появляются первые маски на фасадах театра
- Шаг 4-5: маски растут, меняют выражение (комедия/трагедия)
- Шаг 6: финальная композиция с градиентом от красного к насыщенному золотому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту красный→золотой
- Появлению масок на фасадах зданий
- Изменению выражения масок (улыбка/грусть)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как театральный комплекс с масками "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление масок и трансформацию цветовой схемы от красного к золотому. Важно показать разнообразие: здания и маски должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Mask3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для маски дополнительно хранить `self.expression_path` (история выражения)
- Метод `get_color()` возвращает цвет в градиенте от красного к золотому

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
        # TODO: вернуть цвет в градиенте от красного к золотому
        from matplotlib.colors import to_hex, to_rgb
        red = to_rgb('#DC143C')
        gold = to_rgb('#FFD700')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(r + (g - r) * ratio for r, g in zip(red, gold))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Mask3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.expression = 'comedy'
        self.expression = 'comedy'
        # TODO: создать self.expression_path = ['comedy']
        self.expression_path = ['comedy']
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 80)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с театральными элементами
- `create_mask_mesh(x, y, width, depth, height, expression, color)` — маска (овал + глаза + рот)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с театральными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_mask_mesh(x, y, width, depth, height, expression, color):
    """Создает маску: овал + глаза + рот"""
    traces = []
    
    # Лицо маски (эллипсоид)
    n_theta, n_phi = 20, 15
    theta = np.linspace(0, 2*np.pi, n_theta)
    phi = np.linspace(0, np.pi, n_phi)
    
    center_x, center_y = x + width/2, y + depth/2
    center_z = height/2
    
    face_x, face_y, face_z = [], [], []
    for p in phi:
        for t in theta:
            face_x.append(center_x + width * 0.4 * np.sin(p) * np.cos(t))
            face_y.append(center_y + depth * 0.4 * np.sin(p) * np.sin(t))
            face_z.append(center_z + height * 0.3 * np.cos(p))
    
    face_mesh = go.Mesh3d(
        x=face_x, y=face_y, z=face_z,
        color=color, opacity=0.9,
        alphahull=0
    )
    traces.append(face_mesh)
    
    # Глаза
    eye_offset = width * 0.15
    traces.append(go.Scatter3d(
        x=[center_x - eye_offset, center_x + eye_offset],
        y=[center_y, center_y],
        z=[center_z + height * 0.1, center_z + height * 0.1],
        mode='markers',
        marker=dict(size=8, color='#FFFFFF')
    ))
    
    # Рот (зависит от выражения)
    if expression == 'comedy':
        # Улыбка (дуга вверх)
        mouth_x = [center_x - width * 0.2, center_x, center_x + width * 0.2]
        mouth_z = [center_z - height * 0.1, center_z - height * 0.2, center_z - height * 0.1]
    else:
        # Грусть (дуга вниз)
        mouth_x = [center_x - width * 0.2, center_x, center_x + width * 0.2]
        mouth_z = [center_z - height * 0.2, center_z - height * 0.1, center_z - height * 0.2]
    
    traces.append(go.Scatter3d(
        x=mouth_x, y=[center_y] * 3, z=mouth_z,
        mode='lines',
        line=dict(color='#8B4513', width=4)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 6 элементов (2 Building3D + 4 Mask3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: маски растут и меняют выражение

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 1.0, 60, "B1"),
    Building3D(4.0, 1.0, 1.0, 70, "B2"),
    Mask3D(1.2, 1.0, 0.4, 40, "M1"),
    Mask3D(1.6, 1.0, 0.4, 40, "M2"),
    Mask3D(4.2, 1.0, 0.4, 40, "M3"),
    Mask3D(4.6, 1.0, 0.4, 40, "M4")
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
        elif isinstance(elem, Mask3D) and step >= 4:
            # Маски растут и меняют выражение на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            if step % 2 == 0:
                elem.expression = 'tragedy' if elem.expression == 'comedy' else 'comedy'
            elem.expression_path.append(elem.expression)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все маски с текущим выражением из `expression_path[step]`
- Цветовую динамику от красного к золотому

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
        elif isinstance(elem, Mask3D):
            color = elem.get_color()
            expr = elem.expression_path[step] if step < len(elem.expression_path) else elem.expression_path[-1]
            traces.extend(create_mask_mesh(
                elem.x, elem.y, elem.width, 0.4, h, expr, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Театр с масками".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора театральной архитектуры
- Добавлены подписи и заголовок с темой "От красного к золотому"

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
    title="🎭 Анимация: Театр с масками (градиент от красного к золотому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания красного цвета с минимальными высотами и маски с выражением "комедия"
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Маски начинают расти и менять выражение (шаги 4-7)
  - Цвет всех элементов плавно переходит от красного (`#DC143C`) к насыщенному золотому (`#FFD700`)
  - Выражение масок меняется с "комедия" на "трагедия" и обратно
  - Рот маски меняет форму (улыбка/грусть)
  - Занавес на входе в театр виден в красном цвете
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" театрального архитектурного ансамбля
