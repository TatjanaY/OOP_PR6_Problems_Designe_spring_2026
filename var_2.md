# **Вариант № 2**

## **Генеративная художественная композиция "Восточные купола" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей восточной культуры, который хочет получить интерактивную генеративную инсталляцию "Восточные купола". Инсталляция должна создавать уникальные архитектурные композиции в восточном стиле, где здания с куполами оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и купола — это "живые" объекты, способные расти, менять цвет от жёлтого к оранжевому и реагировать на взаимодействие, создавая динамичные композиции в стиле восточной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 5
dome_probability = 0.6  # вероятность появления купола
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от жёлтого к оранжевому. Это позволяет плавно менять настроение инсталляции (утро/полдень/закат) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и куполов, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов (здания, купола) могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в восточном стиле (арочные окна, орнамент)
- **`Dome`** — купол (полусферическая крыша с декоративным завершением)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в восточном стиле
        # TODO: добавить арочные окна (3 ряда по 2 окна)
        # TODO: добавить декоративный орнамент по краям
        pass

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать основание купола (прямоугольник)
        # TODO: отрисовать полусферу купола (patches.Arc или Ellipse)
        # TODO: добавить декоративный шпиль на вершине
        # TODO: использовать градиент от жёлтого к оранжевому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_dome_to_building(building)` добавляет купол к зданию
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
    
    def add_dome_to_building(self, building):
        # TODO: создать купол над зданием
        # TODO: позиция купола: по центру здания, высота = building.y + building.height
        # TODO: добавить купол в elements
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

Реализуйте класс `ColorTheme`, использующий **паттерн Borg** (все экземпляры разделяют состояние) для хранения цветовой схемы композиции.

**Требования**:
- все экземпляры класса имеют общий словарь `colors`
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'sunset') с градиентом от жёлтого к оранжевому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'dome', 'accent')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от жёлтого к оранжевому (ratio от 0 до 1)

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
                'ground': '#D2B48C',
                'base_yellow': '#FFD700',
                'base_orange': '#FF8C00'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/sunset
        # Градиент от жёлтого к оранжевому усиливается к sunset
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от жёлтого к оранжевому
        # ratio: 0.0 = жёлтый, 1.0 = оранжевый
        from matplotlib.colors import to_rgb, to_hex
        yellow = to_rgb('#FFD700')
        orange = to_rgb('#FF8C00')
        interpolated = tuple(y + (o - y) * ratio for y, o in zip(yellow, orange))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 5 элементов (здания и купола над ними), используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (80, 400), (280, 380), (480, 420)
# - 2 Dome над зданиями с вероятностью 60%

# Пример создания купола над зданием:
# dome = Dome(building.x + building.width*0.2, building.y + building.height, 
#             building.width*0.6, building.height*0.4, "Dome1")
# canvas.add_element(dome)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Ellipse, Arc

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в восточном стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить арочные окна (3 ряда по 2 окна)
        window_w, window_h = self.width * 0.2, self.height * 0.15
        for row in range(3):
            for col in range(2):
                wx = self.x + self.width * 0.2 + col * (self.width * 0.4)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                # Арочное окно: прямоугольник + полукруг сверху
                window = Rectangle((wx, wy), window_w, window_h*0.7, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)
                arc = Arc((wx + window_w/2, wy + window_h*0.7), 
                         window_w, window_h*0.6, 
                         theta1=0, theta2=180, color='#8B4513', linewidth=1)
                ax.add_patch(arc)

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 150)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать основание купола
        base = Rectangle((self.x, self.y), self.width, self.height*0.3, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(base)
        # TODO: отрисовать полусферу купола
        dome = Arc((self.x + self.width/2, self.y + self.height*0.3), 
                  self.width, self.height*1.4, 
                  theta1=0, theta2=180, 
                  facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(dome)
        # TODO: добавить декоративный шпиль на вершине
        spire_x = self.x + self.width/2
        spire_y = self.y + self.height*1.0
        ax.plot([spire_x, spire_x], [spire_y, spire_y + self.height*0.2], 
               color='#8B4513', linewidth=3)
        ax.plot([spire_x - 5, spire_x + 5], [spire_y + self.height*0.2, spire_y + self.height*0.2], 
               color='#8B4513', linewidth=2)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и купола должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" восточной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать купол с отрицательным радиусом или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от жёлтого к оранжевому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию появления куполов над зданиями, где купола "вырастают" постепенно, а цветовая динамика подчёркивает процесс "оживления" восточного города.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Dome: высота купола должна быть пропорциональна ширине (не более 2:1)

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
        self.ornament_level = 0  # уровень восточного орнамента
    
    @property
    def x(self):
        return self._x
    
    @x.setter
    def x(self, value):
        if not isinstance(value, (int, float)):
            raise ValueError("x must be numeric")
        if not (0 <= value <= self.canvas_width):
            raise ValueError(f"x must be in [0, {self.canvas_width}]")
        self._x = value
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)):
            raise ValueError("height must be numeric")
        if value <= 0:
            raise ValueError("height must be positive")
        self.history.append(self._height)
        self._height = value
        self.floors = int(self._height // 20)

class Dome(ArchElement):
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive numeric")
        # Купол не должен быть слишком высоким относительно ширины
        if value > self._width * 2:
            value = self._width * 2
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_ornament()` — добавляет восточный орнамент на фасад
- `get_color()` — возвращает цвет в градиенте от жёлтого к оранжевому

**Для Dome**:
- `add_dome(height)` — увеличивает высоту купола
- `add_finial()` — добавляет декоративное завершение (шпиль)
- `get_color()` — возвращает цвет в градиенте от жёлтого к оранжевому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        return self
    
    def add_ornament(self):
        # TODO: увеличить ornament_level
        self.ornament_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от жёлтого к оранжевому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать здание с арочными окнами
        # TODO: если ornament_level > 0: добавить узоры по краям
        pass

class Dome(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.finial_height = 0  # высота декоративного завершения
        self.history = [height]
    
    def add_dome(self, height):
        # TODO: увеличить высоту купола
        self.height += height
        self.history.append(self._height)
        return self
    
    def add_finial(self):
        # TODO: увеличить finial_height
        self.finial_height += 15
        return self
    
    def get_color(self):
        ratio = min(1.0, (self._height + self.finial_height) / 250)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное появление куполов над зданиями

### **Шаблон кода**

```python
class AnimatedCity:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.elements = []
        self.frames = []
    
    def add_element(self, element):
        self.elements.append(element)
    
    def record_frame(self):
        # TODO: сохранить текущие параметры всех элементов
        frame = {
            elem.name: {
                'height': getattr(elem, '_height', elem.height),
                'ornament': getattr(elem, 'ornament_level', 0),
                'finial': getattr(elem, 'finial_height', 0)
            }
            for elem in self.elements
        }
        self.frames.append(frame)
    
    def show_frame(self, step, frame_data):
        fig, ax = plt.subplots(figsize=(10, 6))
        ax.set_xlim(0, self.width)
        ax.set_ylim(0, self.height)
        ax.set_facecolor('#87CEEB')  # небо
        
        # Земля
        ax.add_patch(Rectangle((0, 0), self.width, 50, 
                              facecolor='#D2B48C', edgecolor='none'))
        
        # Отрисовка элементов
        for elem in self.elements:
            elem.draw(ax)
        
        ax.set_title(f"Шаг {step}: Восточные купола")
        ax.axis('off')
        plt.tight_layout()
        plt.pause(0.5)
        plt.close(fig)
    
    def animate(self, interval=1.0):
        for i, frame in enumerate(self.frames):
            self.show_frame(i, frame)
```

---

## **4. Основной цикл анимации**

### **Задание**

Создайте композицию из 3 зданий. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются купола над зданиями
- Шаги 4-5: купола растут и получают декоративные завершения
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
    Building(80, 50, 80, 70, "B1", 800, 600),
    Building(280, 50, 75, 85, "B2", 800, 600),
    Building(480, 50, 85, 65, "B3", 800, 600)
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
    elif step == 3:
        # Появление куполов над зданиями
        for b in buildings:
            dome = Dome(b.x + b.width*0.15, b.y + b.height, 
                       b.width*0.7, 50, f"Dome_{b.name}", 800, 600)
            canvas.add_element(dome)
    elif step <= 5:
        # Рост куполов и добавление декора
        for elem in canvas.elements:
            if isinstance(elem, Dome):
                elem.add_dome(10)
                if step >= 4:
                    elem.add_finial()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    f = getattr(elem, 'finial_height', 0)
    print(f"{elem.name}: высота={h}, завершение={f}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания жёлтого цвета без куполов
- Шаг 1-2: здания растут, цвет постепенно становится оранжевым
- Шаг 3: над зданиями появляются купола (полупрозрачные, светло-оранжевые)
- Шаг 4-5: купола растут, добавляются декоративные завершения (шпили)
- Шаг 6: финальная композиция с градиентом от жёлтого к насыщенному оранжевому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту жёлтый→оранжевый
- Появлению новых архитектурных элементов (куполов)
- Добавлению восточного орнамента и декоративных завершений
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как восточный город с куполами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление куполов и трансформацию цветовой схемы от жёлтого к оранжевому. Важно показать разнообразие: обычные здания и купола должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Dome3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для купола дополнительно хранить `self.dome_ratio` (пропорция купола)
- Метод `get_color()` возвращает цвет в градиенте от жёлтого к оранжевому

### **Шаблон кода**

```python
class ArchElement3D:
    def __init__(self, x, y, width, height, name):
        self.x = x
        self.y = y
        self.width = width
        self._height = height
        self.name = name
        self.path = [height]
        self.color_ratio = 0
    
    def get_color(self):
        from matplotlib.colors import to_hex, to_rgb
        yellow = to_rgb('#FFD700')
        orange = to_rgb('#FF8C00')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(y + (o - y) * ratio for y, o in zip(yellow, orange))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Dome3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        self.dome_ratio = 0.5  # пропорция высоты купола к ширине
        self.finial_height = 0
    
    def get_color(self):
        total_h = self.path[-1] + self.finial_height
        self.color_ratio = min(1.0, total_h / 250)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с арочными окнами
- `create_dome_mesh(x, y, width, depth, height, dome_ratio, color)` — купол (полусфера на основании)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с восточными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7,0,0,1,1,2,2,3,3]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4,1,1,2,2,3,3,0,0,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1,5,4,6,5,7,6,4,7,0,3]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_dome_mesh(x, y, width, depth, height, dome_ratio, color):
    """Создает купол: основание + полусфера"""
    traces = []
    
    # Основание
    base_height = height * 0.3
    traces.append(create_building_mesh(x, y, width, depth, base_height, color))
    
    # Полусфера купола (аппроксимация через сетку)
    n_theta, n_phi = 20, 10
    theta = np.linspace(0, 2*np.pi, n_theta)
    phi = np.linspace(0, np.pi/2, n_phi)
    
    radius_x, radius_y = width/2 * dome_ratio, depth/2 * dome_ratio
    center_x, center_y = x + width/2, y + depth/2
    center_z = base_height
    
    X_dome, Y_dome, Z_dome = [], [], []
    
    for p in phi:
        for t in theta:
            X_dome.append(center_x + radius_x * np.sin(p) * np.cos(t))
            Y_dome.append(center_y + radius_y * np.sin(p) * np.sin(t))
            Z_dome.append(center_z + radius_x * np.cos(p))
    
    # Создаём поверхность купола
    dome_surface = go.Mesh3d(
        x=X_dome, y=Y_dome, z=Z_dome,
        color=color, opacity=0.9,
        alphahull=0  # автоматическая триангуляция
    )
    traces.append(dome_surface)
    
    # Декоративное завершение (шпиль)
    traces.append(go.Scatter3d(
        x=[center_x, center_x], y=[center_y, center_y], 
        z=[center_z + radius_x, center_z + radius_x + height*0.2],
        mode='lines', line=dict(color='#8B4513', width=4)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Dome3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаг 4**: появляются купола над зданиями
- **Шаги 5-7**: купола растут и получают декоративные завершения

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(2.5, 1.0, 0.9, 70, "B2"),
    Dome3D(1.2, 1.0, 0.6, 40, "D1"),   # появится позже
    Dome3D(2.7, 1.0, 0.7, 45, "D2")    # появится позже
]

# Купола появляются на шаге 4
for elem in elements[2:]:
    elem.path = [0]  # начальная высота 0

# Коэффициенты роста
growth_rates = [15, 20, 12, 15]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Dome3D) and step < 4:
            continue  # купол ещё не появился
        
        if isinstance(elem, Building3D):
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Dome3D):
            if step == 4:
                # Первое появление купола
                elem.path.append(40)
            elif step > 4:
                new_h = elem.path[-1] + growth_rates[i]
                elem.path.append(new_h)
                if step >= 6:
                    elem.finial_height += 5
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (песочный цвет)
- Все здания с текущей высотой из `path[step]`
- Купола, которые появляются только с шага 4
- Цветовую динамику от жёлтого к оранжевому

### **Шаблон кода**

```python
frames = []
max_steps = max(len(elem.path) for elem in elements)

# Координаты земли
x_ground = np.arange(0, 5, 0.2)
y_ground = np.arange(0, 3, 0.2)
X, Y = np.meshgrid(x_ground, y_ground)

for step in range(max_steps):
    traces = []
    
    # Поверхность земли (песочный цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale=[[0, '#D2B48C'], [1, '#C19A6B']], 
        showscale=False, opacity=0.5
    ))
    
    for elem in elements:
        # Пропускаем купола, если они ещё не появились
        if isinstance(elem, Dome3D) and (step >= len(elem.path) or elem.path[step] == 0):
            continue
        
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Dome3D) and h > 0:
            color = elem.get_color()
            traces.extend(create_dome_mesh(
                elem.x, elem.y, elem.width, 0.4, h, 0.6, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Восточные купола".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора восточной архитектуры
- Добавлены подписи и заголовок с темой "От жёлтого к оранжевому"

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
        camera=dict(eye=dict(x=2.5, y=2.0, z=1.8)),
        aspectmode='manual',
        aspectratio=dict(x=1.5, y=0.8, z=1.2),
        bgcolor='#87CEEB'
    ),
    title="🕌 Анимация: Восточные купола (градиент от жёлтого к оранжевому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания жёлтого цвета с минимальными высотами
- При нажатии Play:
  - Здания начинают расти
  - На шаге 4 появляются купола над зданиями
  - Цвет всех элементов плавно переходит от жёлтого к насыщенному оранжевому
  - Купола получают декоративные завершения (шпили)
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" восточного архитектурного ансамбля
