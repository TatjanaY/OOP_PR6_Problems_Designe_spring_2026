# **Вариант № 7**

## **Генеративная художественная композиция "Ветряная мельница" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей сельской истории, который хочет получить интерактивную генеративную инсталляцию "Ветряная мельница". Инсталляция должна создавать уникальные композиции с ветряными мельницами, где здания и лопасти оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и мельницы — это "живые" объекты, способные расти, менять цвет от серого к белому и реагировать на взаимодействие, создавая динамичные композиции в стиле сельской архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
windmill_count = 2  # количество ветряных мельниц
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к белому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и ветряных мельниц, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в сельском стиле
- **`Windmill`** — ветряная мельница с корпусом и вращающимися лопастями

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в сельском стиле
        # TODO: добавить окна (2 ряда по 3 окна)
        # TODO: добавить крышу (треугольник)
        pass

class Windmill(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать корпус мельницы (конус или цилиндр)
        # TODO: отрисовать 4 лопасти (крестообразно)
        # TODO: добавить центр вращения
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
- метод `add_windmill_near(building)` создаёт мельницу рядом со зданием
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
    
    def add_windmill_near(self, building):
        # TODO: создать мельницу рядом со зданием
        # TODO: позиция: building.x + building.width + 50, building.y
        # TODO: добавить мельницу в elements
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
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'windmill', 'blades')
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

Создайте композицию из 3 зданий с 2 ветряными мельницами рядом, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Windmill рядом с зданиями

# Пример создания мельницы:
# windmill = Windmill(building.x + building.width + 50, building.y, 
#                     60, 180, "Windmill1")
# canvas.add_element(windmill)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Polygon, Circle
import numpy as np

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в сельском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#654321', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить крышу (треугольник)
        roof = Polygon([
            (self.x - 10, self.y + self.height),
            (self.x + self.width + 10, self.y + self.height),
            (self.x + self.width/2, self.y + self.height + 50)
        ], facecolor='#654321', edgecolor='#654321', linewidth=2)
        ax.add_patch(roof)
        # TODO: добавить окна (2 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#654321')
                ax.add_patch(window)

class Windmill(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 250)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать корпус мельницы (трапеция)
        body = Polygon([
            (self.x + self.width * 0.2, self.y),
            (self.x + self.width * 0.8, self.y),
            (self.x + self.width * 0.7, self.y + self.height * 0.7),
            (self.x + self.width * 0.3, self.y + self.height * 0.7)
        ], facecolor=color, edgecolor='#654321', linewidth=2)
        ax.add_patch(body)
        # TODO: отрисовать 4 лопасти (крестообразно)
        center_x = self.x + self.width / 2
        center_y = self.y + self.height * 0.7
        blade_length = self.width * 1.5
        # Вертикальная лопасть
        ax.plot([center_x, center_x], 
               [center_y - blade_length/2, center_y + blade_length/2], 
               color='#FFFFFF', linewidth=4)
        # Горизонтальная лопасть
        ax.plot([center_x - blade_length/2, center_x + blade_length/2], 
               [center_y, center_y], 
               color='#FFFFFF', linewidth=4)
        # TODO: добавить центр вращения
        center = Circle((center_x, center_y), 8, facecolor='#654321')
        ax.add_patch(center)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и мельницы должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" сельской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать мельницу с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к белому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления лопастей к мельнице, где лопасти появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" мельницы.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Windmill: высота должна быть достаточной для лопастей (не менее 100px)

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
        self.blade_level = 0  # уровень лопастей
    
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

class Windmill(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.blade_count = 4  # количество лопастей
        self.blade_angle = 0  # угол вращения
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 100:  # минимальная высота для лопастей
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
- `add_blade_decor()` — добавляет декор в сельском стиле
- `get_color()` — возвращает цвет в градиенте от серого к белому

**Для Windmill**:
- `add_blades(n)` — добавляет n лопастей к мельнице
- `rotate(angle)` — вращает лопасти на угол
- `get_color()` — возвращает цвет в градиенте от серого к белому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_blade_decor(self):
        # TODO: увеличить blade_level
        self.blade_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к белому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если blade_level > 0: добавить сельский декор
        pass

class Windmill(ArchElement):
    def add_blades(self, n):
        # TODO: увеличить количество лопастей на n
        self.blade_count += n
        # TODO: увеличить высоту пропорционально
        self.height += 20 * n
        # TODO: добавить запись в history
        self.history.append(self._height)
        # TODO: вернуть self
        return self
    
    def rotate(self, angle):
        # TODO: увеличить угол вращения
        self.blade_angle += angle
        return self
    
    def get_color(self):
        # TODO: градиент от серого к белому по высоте
        ratio = min(1.0, self._height / 250)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать корпус мельницы
        # TODO: отрисовать лопасти с учётом угла вращения
        # TODO: использовать rotate для анимации вращения
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление лопастей к мельнице

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
                'blades': getattr(elem, 'blade_count', 0),
                'angle': getattr(elem, 'blade_angle', 0)
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
        ax.set_title(f"Шаг {step}: Ветряная мельница")
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

Создайте композицию из 2 зданий и 2 мельниц. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: мельницы получают первые лопасти
- Шаги 4-5: мельницы продолжают расти, лопасти вращаются
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
    Building(50, 50, 100, 80, "B1", 800, 600),
    Building(300, 50, 100, 75, "B2", 800, 600)
]

windmills = [
    Windmill(180, 50, 60, 150, "W1", 800, 600),
    Windmill(430, 50, 60, 160, "W2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for w in windmills:
    canvas.add_element(w)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Мельницы добавляют лопасти и вращаются
        for elem in windmills:
            elem.add_blades(1)
            elem.rotate(45)  # вращение на 45 градусов
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    b = getattr(elem, 'blade_count', 0)
    print(f"{elem.name}: высота={h}, лопасти={b}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и мельницы серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится светлее
- Шаг 3: мельницы получают первые лопасти (с 4 до 5)
- Шаг 4-5: мельницы продолжают расти, лопасти вращаются на 45 градусов каждый шаг
- Шаг 6: финальная композиция с градиентом от серого к насыщенному белому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→белый
- Появлению новых лопастей на мельницах
- Вращению лопастей (изменение угла)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как сельский пейзаж с ветряными мельницами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление лопастей и трансформацию цветовой схемы от серого к белому. Важно показать разнообразие: обычные здания и ветряные мельницы должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Windmill3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для мельницы дополнительно хранить `self.blade_path` (история количества лопастей) и `self.rotation_angle` (угол вращения)
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


class Windmill3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.blade_count = 4
        self.blade_count = 4
        # TODO: создать self.blade_path = [4]
        self.blade_path = [4]
        # TODO: создать self.rotation_angle = 0
        self.rotation_angle = 0
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 250)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с крышей
- `create_windmill_mesh(x, y, width, depth, height, blade_count, rotation_angle, color)` — мельница (корпус + лопасти)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с сельскими элементами"""
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


def create_windmill_mesh(x, y, width, depth, height, blade_count, rotation_angle, color):
    """Создает ветряную мельницу: корпус + лопасти"""
    traces = []
    
    # Корпус мельницы (усечённый конус через цилиндр)
    n_segments = 16
    base_x, base_y = [], []
    top_x, top_y = [], []
    
    center_x, center_y = x + width/2, y + depth/2
    base_radius = width/2
    top_radius = width/3
    body_height = height * 0.7
    
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
    
    # Лопасти (линии от центра)
    blade_length = width * 1.5
    blade_z = body_height + 10
    
    for i in range(blade_count):
        angle = (2 * np.pi * i / blade_count) + np.radians(rotation_angle)
        end_x = center_x + blade_length * np.cos(angle)
        end_y = center_y + blade_length * np.sin(angle)
        
        traces.append(go.Scatter3d(
            x=[center_x, end_x],
            y=[center_y, end_y],
            z=[blade_z, blade_z],
            mode='lines',
            line=dict(color='#FFFFFF', width=4)
        ))
    
    # Центр вращения
    traces.append(go.Scatter3d(
        x=[center_x], y=[center_y], z=[blade_z],
        mode='markers',
        marker=dict(size=5, color='#654321')
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Windmill3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: мельницы добавляют лопасти и вращаются

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Windmill3D(2.0, 1.0, 0.6, 150, "W1"),
    Windmill3D(4.0, 1.0, 0.7, 160, "W2")
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
        elif isinstance(elem, Windmill3D) and step >= 4:
            # Мельницы добавляют лопасти и вращаются на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.blade_count += 1
            elem.blade_path.append(elem.blade_count)
            elem.rotation_angle += 45  # вращение на 45 градусов
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (зелёный цвет)
- Все здания с текущей высотой из `path[step]`
- Все мельницы с текущим количеством лопастей из `blade_path[step]` и углом вращения
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
    
    # Поверхность земли
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
        elif isinstance(elem, Windmill3D):
            color = elem.get_color()
            b = elem.blade_path[step] if step < len(elem.blade_path) else elem.blade_path[-1]
            angle = elem.rotation_angle if step >= 4 else 0
            traces.extend(create_windmill_mesh(
                elem.x, elem.y, elem.width, 0.4, h, b, angle, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Ветряная мельница".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора сельского пейзажа
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
    title="🌾 Анимация: Ветряная мельница (градиент от серого к белому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами и мельницы с 4 лопастями
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Мельницы начинают добавлять лопасти (шаги 4-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному белому (`#FFFFFF`)
  - Лопасти мельниц вращаются на 45 градусов каждый шаг
  - Корпус мельницы имеет форму усечённого конуса
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" сельского архитектурного ансамбля
