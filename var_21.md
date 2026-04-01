# **Вариант № 21**

## **Генеративная художественная композиция "Жилой дом с балконами" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей городской архитектуры, который хочет получить интерактивную генеративную инсталляцию "Жилой дом с балконами". Инсталляция должна создавать уникальные композиции с жилыми домами, где здания и балконы оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и балконы — это "живые" объекты, способные расти, менять цвет от бежевого к коричневому и реагировать на взаимодействие, создавая динамичные композиции в стиле городской жилой застройки.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
balcony_count = 4  # количество балконов на здание
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от бежевого к коричневому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий с балконами, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в жилом стиле
- **`Balcony`** — балкон (выступающая платформа с перилами)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в жилом стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить места для балконов
        pass

class Balcony(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать платформу балкона (прямоугольник)
        # TODO: добавить перила (вертикальные линии)
        # TODO: добавить цветы на балконе (опционально)
        # TODO: использовать градиент от бежевого к коричневому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_balcony_to_building(building, floor)` добавляет балкон к зданию на указанном этаже
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
    
    def add_balcony_to_building(self, building, floor):
        # TODO: создать балкон на указанном этаже здания
        # TODO: позиция: building.x + building.width, building.y + floor * 50
        # TODO: добавить балкон в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от бежевого к коричневому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'balcony', 'railing')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #F5F5DC к #8B4513 (ratio от 0 до 1)

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
                'base_beige': '#F5F5DC',
                'base_brown': '#8B4513'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от бежевого к коричневому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от бежевого к коричневому
        # ratio: 0.0 = бежевый, 1.0 = коричневый
        from matplotlib.colors import to_rgb, to_hex
        beige = to_rgb('#F5F5DC')
        brown = to_rgb('#8B4513')
        interpolated = tuple(b + (br - b) * ratio for b, br in zip(beige, brown))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 4 балконами на каждом, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - По 4 Balcony на каждом здании

# Пример создания балкона:
# balcony = Balcony(building.x + building.width, building.y + floor * 50, 
#                   40, 30, "Balcony1")
# canvas.add_element(balcony)

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
        # TODO: создать Rectangle для фасада в жилом стиле
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

class Balcony(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от этажа)
        ratio = min(1.0, self.height / 100)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать платформу балкона
        platform = Rectangle((self.x, self.y), self.width, self.height * 0.3, 
                            facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(platform)
        # TODO: добавить перила (вертикальные линии)
        num_railings = 5
        for i in range(num_railings):
            rail_x = self.x + self.width * (i + 1) / (num_railings + 1)
            ax.plot([rail_x, rail_x], [self.y + self.height * 0.3, self.y + self.height], 
                   color='#8B4513', linewidth=2)
        # TODO: добавить цветы на балконе (опционально)
        for i in range(3):
            flower_x = self.x + self.width * (i + 1) / 4
            flower = Circle((flower_x, self.y + self.height * 0.15), 5, 
                           facecolor='#FF6347', edgecolor='none')
            ax.add_patch(flower)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и балконы должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" жилой застройки)
- Иметь защиту от некорректных параметров (нельзя сделать балкон с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от бежевого к коричневому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления балконов к зданию, где балконы появляются постепенно на разных этажах, а цветовая динамика подчёркивает процесс "оживления" жилого дома.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Balcony: высота должна быть достаточной для перил (не менее 20px)

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
        self.balcony_count = 0  # количество балконов
    
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

class Balcony(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.railing_count = 5  # количество перил
        self.has_flowers = False  # есть ли цветы
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 20:  # минимальная высота для перил
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
- `add_balcony()` — добавляет балкон к зданию
- `get_color()` — возвращает цвет в градиенте от бежевого к коричневому

**Для Balcony**:
- `add_railing()` — добавляет перила к балкону
- `add_flowers()` — добавляет цветы на балкон
- `get_color()` — возвращает цвет в градиенте от бежевого к коричневому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_balcony(self):
        # TODO: увеличить количество балконов
        self.balcony_count += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от бежевого к коричневому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если balcony_count > 0: добавить балконы
        pass

class Balcony(ArchElement):
    def add_railing(self):
        # TODO: увеличить количество перил
        self.railing_count += 1
        return self
    
    def add_flowers(self):
        # TODO: установить has_flowers = True
        self.has_flowers = True
        return self
    
    def get_color(self):
        # TODO: градиент от бежевого к коричневому по высоте
        ratio = min(1.0, self._height / 100)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать балкон с учётом railing_count и has_flowers
        # TODO: отрисовать платформу, перила и цветы
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление балконов к зданию

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
                'balconies': getattr(elem, 'balcony_count', 0),
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
                              facecolor='#8B7355', edgecolor='none'))
        
        # TODO: для каждого элемента вызвать draw()
        for elem in self.elements:
            elem.draw(ax)
        
        # TODO: добавить заголовок с номером шага
        ax.set_title(f"Шаг {step}: Жилой дом с балконами")
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

Создайте композицию из 2 зданий. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые балконы
- Шаги 4-5: добавляются новые балконы на разных этажах
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

for b in buildings:
    canvas.add_element(b)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Добавление балконов на разных этажах
        for elem in buildings:
            elem.add_balcony()
            # Создание балкона на случайном этаже
            floor = step - 2
            balcony_y = elem.y + floor * 40
            balcony = Balcony(elem.x + elem.width, balcony_y, 
                             40, 30, f"Balcony_{elem.name}_{step}", 800, 600)
            balcony.add_flowers()
            canvas.add_element(balcony)
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    b = getattr(elem, 'balcony_count', 0)
    print(f"{elem.name}: высота={h}, балконы={b}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания бежевого цвета без балконов
- Шаг 1-2: здания растут, цвет постепенно становится коричневым
- Шаг 3: появляются первые балконы на нижних этажах
- Шаг 4-5: добавляются новые балконы на разных этажах с цветами
- Шаг 6: финальная композиция с градиентом от бежевого к насыщенному коричневому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту бежевый→коричневый
- Появлению новых балконов на разных этажах
- Добавлению цветов на балконах
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как жилой дом с балконами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление балконов и трансформацию цветовой схемы от бежевого к коричневому. Важно показать разнообразие: здания и балконы должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Balcony3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для балкона дополнительно хранить `self.balcony_path` (история количества балконов)
- Метод `get_color()` возвращает цвет в градиенте от бежевого к коричневому

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
        # TODO: вернуть цвет в градиенте от бежевого к коричневому
        from matplotlib.colors import to_hex, to_rgb
        beige = to_rgb('#F5F5DC')
        brown = to_rgb('#8B4513')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(b + (br - b) * ratio for b, br in zip(beige, brown))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Balcony3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.railing_count = 5
        self.railing_count = 5
        # TODO: создать self.balcony_path = [0]
        self.balcony_path = [0]
        self.has_flowers = False
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 100)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с жилыми элементами
- `create_balcony_mesh(x, y, width, depth, height, railing_count, has_flowers, color)` — балкон (платформа + перила + цветы)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с жилыми элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_balcony_mesh(x, y, width, depth, height, railing_count, has_flowers, color):
    """Создает балкон: платформа + перила + цветы"""
    traces = []
    
    # Платформа балкона
    platform_h = height * 0.3
    traces.append(create_building_mesh(x, y, width, depth, platform_h, color))
    
    # Перила (вертикальные линии)
    for i in range(railing_count):
        rail_x = x + width * (i + 1) / (railing_count + 1)
        traces.append(go.Scatter3d(
            x=[rail_x, rail_x], y=[y + depth, y + depth], 
            z=[platform_h, platform_h + height * 0.7],
            mode='lines',
            line=dict(color='#8B4513', width=3)
        ))
    
    # Цветы на балконе
    if has_flowers:
        for i in range(3):
            flower_x = x + width * (i + 1) / 4
            flower_y = y + depth / 2
            flower_z = platform_h + 5
            
            traces.append(go.Scatter3d(
                x=[flower_x], y=[flower_y], z=[flower_z],
                mode='markers',
                marker=dict(size=5, color='#FF6347', opacity=0.8)
            ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Balcony3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: добавляются балконы на разных этажах

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Balcony3D(1.8, 1.0, 0.4, 30, "Bal1"),
    Balcony3D(3.9, 1.0, 0.4, 30, "Bal2")
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
        elif isinstance(elem, Balcony3D) and step >= 4:
            # Балконы добавляются на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.railing_count += 1
            elem.balcony_path.append(elem.railing_count)
            if step >= 5:
                elem.has_flowers = True
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все балконы с текущим количеством перил из `balcony_path[step]` и цветами
- Цветовую динамику от бежевого к коричневому

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
        elif isinstance(elem, Balcony3D):
            color = elem.get_color()
            r = elem.balcony_path[step] if step < len(elem.balcony_path) else elem.balcony_path[-1]
            traces.extend(create_balcony_mesh(
                elem.x, elem.y, elem.width, 0.4, h, r, elem.has_flowers, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Жилой дом с балконами".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора жилой застройки
- Добавлены подписи и заголовок с темой "От бежевого к коричневому"

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
    title="🏢 Анимация: Жилой дом с балконами (градиент от бежевого к коричневому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания бежевого цвета с минимальными высотами без балконов
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Балконы начинают появляться на разных этажах (шаги 4-7)
  - Цвет всех элементов плавно переходит от бежевого (`#F5F5DC`) к насыщенному коричневому (`#8B4513`)
  - Количество перил на балконах увеличивается
  - Цветы появляются на балконах (с шага 5)
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" городской жилой застройки
