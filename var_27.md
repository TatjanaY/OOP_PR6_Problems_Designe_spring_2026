# **Вариант № 27**

## **Генеративная художественная композиция "Здание с вертикальным садом" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей эко-архитектуры, который хочет получить интерактивную генеративную инсталляцию "Здание с вертикальным садом". Инсталляция должна создавать уникальные композиции с зданиями и вертикальными садами, где архитектурные элементы и растения оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и сады — это "живые" объекты, способные расти, менять цвет от зелёного к цветному и реагировать на взаимодействие, создавая динамичные композиции в стиле современной эко-архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
garden_count = 4  # количество секций сада
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от зелёного к цветному. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и вертикальных садов, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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
- **`Garden`** — вертикальный сад (растения на фасаде здания)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в эко-стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить места для вертикального сада
        pass

class Garden(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать растения на фасаде (круги/овалы разных цветов)
        # TODO: добавить систему полива (линии)
        # TODO: добавить индикатор здоровья растений
        # TODO: использовать градиент от зелёного к цветному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_garden_to_building(building, sections)` добавляет вертикальный сад к зданию
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
    
    def add_garden_to_building(self, building, sections):
        # TODO: создать вертикальный сад на фасаде здания
        # TODO: позиция: building.x + offset, building.y + offset
        # TODO: добавить сад в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от зелёного к цветному
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'garden', 'plant')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #228B22 к разноцветному (ratio от 0 до 1)

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
                'base_green': '#228B22',
                'base_colorful': '#FF6347'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от зелёного к цветному усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от зелёного к цветному
        # ratio: 0.0 = зелёный, 1.0 = цветной
        colors = ['#228B22', '#32CD32', '#90EE90', '#FF6347', '#FFD700']
        idx = int(ratio * (len(colors) - 1))
        return colors[idx]
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 4 секциями вертикального сада на каждом, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - По 4 Garden секции на каждом здании

# Пример создания сада:
# garden = Garden(building.x + offset, building.y + offset, 
#                 30, 40, "Garden1")
# canvas.add_element(garden)

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
import numpy as np

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в эко-стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#228B22', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#228B22')
                ax.add_patch(window)
        # TODO: добавить места для вертикального сада
        for i in range(4):
            garden_x = self.x + self.width * 0.6 + i * (self.width * 0.1)
            ax.plot([garden_x, garden_x], [self.y, self.y + self.height], 
                   color='#228B22', linewidth=1, linestyle='--')

class Garden(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 100)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать растения на фасаде (круги разных цветов)
        plant_colors = ['#228B22', '#32CD32', '#90EE90', '#FF6347', '#FFD700']
        for i in range(5):
            plant_x = self.x + self.width * (i + 1) / 6
            plant_y = self.y + self.height * (i + 1) / 6
            plant_color = plant_colors[i % len(plant_colors)]
            plant = Circle((plant_x, plant_y), 8, 
                          facecolor=plant_color, edgecolor='#228B22')
            ax.add_patch(plant)
        # TODO: добавить систему полива (линии)
        ax.plot([self.x, self.x + self.width], [self.y + self.height/2, self.y + self.height/2], 
               color='#4169E1', linewidth=1, alpha=0.5)
        # TODO: добавить индикатор здоровья растений
        health = Circle((self.x + self.width/2, self.y + 10), 5, 
                       facecolor='#00FF00' if ratio > 0.5 else '#FF0000', edgecolor='none')
        ax.add_patch(health)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и сады должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" эко-архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать сад с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от зелёного к цветному в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления растений к вертикальному саду, где растения появляются постепенно и расцветают, а цветовая динамика подчёркивает процесс "оживления" эко-здания.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Garden: высота должна быть достаточной для растений (не менее 30px)

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
        self.garden_level = 0  # уровень сада
    
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

class Garden(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.plant_count = 5  # количество растений
        self.bloom_level = 0  # уровень цветения
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 30:  # минимальная высота для растений
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
- `add_garden()` — добавляет секцию сада к зданию
- `get_color()` — возвращает цвет в градиенте от зелёного к цветному

**Для Garden**:
- `add_plant()` — добавляет растение к саду
- `bloom()` — увеличивает уровень цветения
- `get_color()` — возвращает цвет в градиенте от зелёного к цветному

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_garden(self):
        # TODO: увеличить garden_level
        self.garden_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от зелёного к цветному
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если garden_level > 0: добавить вертикальный сад
        pass

class Garden(ArchElement):
    def add_plant(self):
        # TODO: увеличить количество растений
        self.plant_count += 1
        # TODO: увеличить высоту пропорционально
        self.height += 5
        # TODO: добавить запись в history
        self.history.append(self._height)
        # TODO: вернуть self
        return self
    
    def bloom(self):
        # TODO: увеличить уровень цветения
        self.bloom_level += 1
        return self
    
    def get_color(self):
        # TODO: градиент от зелёного к цветному по высоте и цветению
        ratio = min(1.0, (self._height / 100 + self.bloom_level / 5) / 2)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать сад с учётом bloom_level
        # TODO: отрисовать растения и систему полива
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление растений к саду

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
                'garden': getattr(elem, 'garden_level', 0),
                'bloom': getattr(elem, 'bloom_level', 0)
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
        ax.set_title(f"Шаг {step}: Здание с вертикальным садом")
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

Создайте композицию из 2 зданий и 4 садов. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые растения
- Шаги 4-5: сады растут, растения цветут
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

gardens = [
    Garden(70, 180, 30, 40, "G1", 800, 600),
    Garden(120, 180, 30, 40, "G2", 800, 600),
    Garden(320, 175, 30, 40, "G3", 800, 600),
    Garden(370, 175, 30, 40, "G4", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for g in gardens:
    canvas.add_element(g)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Сады растут и цветут
        for elem in gardens:
            elem.add_plant()
            elem.bloom()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, Garden):
        b = getattr(elem, 'bloom_level', 0)
        print(f"{elem.name}: высота={h}, цветение={b}")
    else:
        g = getattr(elem, 'garden_level', 0)
        print(f"{elem.name}: высота={h}, сад={g}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и сады зелёного цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится цветным
- Шаг 3: появляются первые растения на фасадах
- Шаг 4-5: сады растут, растения начинают цвести (разные цвета)
- Шаг 6: финальная композиция с градиентом от зелёного к насыщенному цветному

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту зелёный→цветной
- Появлению новых растений на вертикальных садах
- Расцветанию растений (разные цвета)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как эко-здание с вертикальным садом "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление растений и трансформацию цветовой схемы от зелёного к цветному. Важно показать разнообразие: здания и сады должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Garden3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для сада дополнительно хранить `self.bloom_path` (история уровня цветения)
- Метод `get_color()` возвращает цвет в градиенте от зелёного к цветному

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
        # TODO: вернуть цвет в градиенте от зелёного к цветному
        colors = ['#228B22', '#32CD32', '#90EE90', '#FF6347', '#FFD700']
        ratio = min(1.0, self.color_ratio)
        idx = int(ratio * (len(colors) - 1))
        return colors[idx]


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Garden3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.plant_count = 5
        self.plant_count = 5
        # TODO: создать self.bloom_level = 0
        self.bloom_level = 0
        # TODO: создать self.bloom_path = [0]
        self.bloom_path = [0]
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты и цветения
        self.color_ratio = min(1.0, (self.path[-1] / 100 + self.bloom_level / 5) / 2)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с эко-элементами
- `create_garden_mesh(x, y, width, depth, height, bloom_level, color)` — вертикальный сад (растения на стене)

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


def create_garden_mesh(x, y, width, depth, height, bloom_level, color):
    """Создает вертикальный сад: растения на стене"""
    traces = []
    
    # Стена с садом
    wall_mesh = create_building_mesh(x, y, width, depth, height, '#228B22')
    traces.append(wall_mesh)
    
    # Растения (сферы разных цветов)
    plant_colors = ['#228B22', '#32CD32', '#90EE90', '#FF6347', '#FFD700']
    for i in range(5):
        plant_x = x + width * (i + 1) / 6
        plant_y = y + depth/2
        plant_z = height * (i + 1) / 6
        
        # Цвет зависит от уровня цветения
        color_idx = min(bloom_level, len(plant_colors) - 1)
        plant_color = plant_colors[color_idx % len(plant_colors)]
        
        traces.append(go.Scatter3d(
            x=[plant_x], y=[plant_y], z=[plant_z],
            mode='markers',
            marker=dict(size=8, color=plant_color, opacity=0.9)
        ))
    
    # Система полива (линии)
    traces.append(go.Scatter3d(
        x=[x, x + width], y=[y + depth/2, y + depth/2], z=[height/2, height/2],
        mode='lines',
        line=dict(color='#4169E1', width=2, opacity=0.5)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 6 элементов (2 Building3D + 4 Garden3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: сады растут и цветут

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Garden3D(1.2, 1.0, 0.3, 40, "G1"),
    Garden3D(1.5, 1.0, 0.3, 40, "G2"),
    Garden3D(3.2, 1.0, 0.3, 40, "G3"),
    Garden3D(3.5, 1.0, 0.3, 40, "G4")
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
        elif isinstance(elem, Garden3D) and step >= 4:
            # Сады растут и цветут на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.bloom_level += 1
            elem.bloom_path.append(elem.bloom_level)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (зелёный цвет)
- Все здания с текущей высотой из `path[step]`
- Все сады с текущим уровнем цветения из `bloom_path[step]`
- Цветовую динамику от зелёного к цветному

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
        elif isinstance(elem, Garden3D):
            color = elem.get_color()
            b = elem.bloom_path[step] if step < len(elem.bloom_path) else elem.bloom_path[-1]
            traces.extend(create_garden_mesh(
                elem.x, elem.y, elem.width, 0.4, h, b, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Здание с вертикальным садом".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора эко-архитектуры
- Добавлены подписи и заголовок с темой "От зелёного к цветному"

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
    title="🌿 Анимация: Здание с вертикальным садом (градиент от зелёного к цветному)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания зелёного цвета с минимальными высотами и сады без цветения
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Вертикальные сады начинают расти и цвести (шаги 4-7)
  - Цвет всех элементов плавно переходит от зелёного (`#228B22`) к разноцветному (`#FF6347`, `#FFD700`)
  - Уровень цветения увеличивается с 0 до 4-5
  - Растения меняют цвет по мере цветения
  - Система полива видна на фасаде
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" современной эко-архитектуры
