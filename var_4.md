# **Вариант № 4**

## **Генеративная художественная композиция "Античные колонны" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей античной культуры, который хочет получить интерактивную генеративную инсталляцию "Античные колонны". Инсталляция должна создавать уникальные архитектурные композиции в древнегреческом стиле, где здания и колонны оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и колонны — это "живые" объекты, способные расти, менять цвет от белого к золотому и реагировать на взаимодействие, создавая динамичные композиции в стиле античной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
column_count = 6  # количество колонн
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к золотому. Это позволяет плавно менять настроение инсталляции (утро/полдень/закат) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и колонн, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов (здания, колонны) могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в античном стиле (фронтоны, карнизы)
- **`Column`** — колонна с базой, стволом и капителью

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в античном стиле
        # TODO: добавить фронтон (треугольник сверху)
        # TODO: добавить окна с арочными завершениями
        pass

class Column(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать базу колонны (прямоугольник внизу)
        # TODO: отрисовать ствол колонны (цилиндр/прямоугольник)
        # TODO: отрисовать капитель (декоративный элемент сверху)
        # TODO: использовать градиент от белого к золотому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_colonnade(building, count)` добавляет колоннаду перед зданием
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
    
    def add_colonnade(self, building, count):
        # TODO: создать ряд колонн перед зданием
        # TODO: равномерно распределить колонны по ширине здания
        # TODO: добавить колонны в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'sunset') с градиентом от белого к золотому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'column', 'capital')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #FFFFFF к #FFD700 (ratio от 0 до 1)

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
                'ground': '#F5DEB3',
                'base_white': '#FFFFFF',
                'base_gold': '#FFD700'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/sunset
        # Градиент от белого к золотому усиливается к sunset
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от белого к золотому
        # ratio: 0.0 = белый, 1.0 = золотой
        from matplotlib.colors import to_rgb, to_hex
        white = to_rgb('#FFFFFF')
        gold = to_rgb('#FFD700')
        interpolated = tuple(w + (g - w) * ratio for w, g in zip(white, gold))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с колоннадами (по 2 колонны перед каждым зданием), используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (100, 400), (300, 380), (500, 420)
# - По 2 Column перед каждым зданием

# Пример создания колонны:
# column = Column(building.x + offset, building.y, 30, 120, "Column1")
# canvas.add_element(column)

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

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в античном стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить фронтон (треугольник сверху)
        pediment = Polygon([
            (self.x - 10, self.y + self.height),
            (self.x + self.width + 10, self.y + self.height),
            (self.x + self.width/2, self.y + self.height + 40)
        ], facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(pediment)
        # TODO: добавить окна с арочными завершениями
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)

class Column(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 200)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать базу колонны
        base = Rectangle((self.x - 5, self.y), self.width + 10, 15, 
                        facecolor=color, edgecolor='#8B4513', linewidth=1)
        ax.add_patch(base)
        # TODO: отрисовать ствол колонны (с каннелюрами - вертикальными линиями)
        shaft = Rectangle((self.x, self.y + 15), self.width, self.height - 35, 
                         facecolor=color, edgecolor='#8B4513', linewidth=1)
        ax.add_patch(shaft)
        # TODO: добавить каннелюры (вертикальные линии на стволе)
        for i in range(5):
            line_x = self.x + self.width * (i + 1) / 6
            ax.plot([line_x, line_x], [self.y + 15, self.y + self.height - 20], 
                   color='#8B4513', linewidth=0.5, alpha=0.5)
        # TODO: отрисовать капитель (декоративный элемент сверху)
        capital = Rectangle((self.x - 8, self.y + self.height - 20), 
                           self.width + 16, 20, 
                           facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(capital)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и колонны должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" античной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать колонну с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к золотому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию появления колоннад перед зданиями, где колонны "вырастают" постепенно, а цветовая динамика подчёркивает процесс "оживления" античного храма.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Column: высота должна быть пропорциональна ширине (не менее 4:1)

### **Шаблон кода**

```python
class Building(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self._x = x
        self._y = y
        self._width = width
        self._height = height
        self.name = name
        self.history = [height]
        self.floors = height // 20
        self.capital_level = 0  # уровень декора капители
    
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

class Column(ArchElement):
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive numeric")
        # Колонна должна быть достаточно высокой относительно ширины
        if value < self._width * 4:
            value = self._width * 4
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_capital()` — добавляет декор капители на колонны
- `get_color()` — возвращает цвет в градиенте от белого к золотому

**Для Column**:
- `add_height(h)` — увеличивает высоту колонны
- `add_capital()` — добавляет декоративные элементы на капитель
- `get_color()` — возвращает цвет в градиенте от белого к золотому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        return self
    
    def add_capital(self):
        # TODO: увеличить capital_level
        self.capital_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к золотому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать здание с фронтоном
        # TODO: если capital_level > 0: добавить декор на фронтон
        pass

class Column(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.capital_decor = 0  # уровень декора капители
        self.history = [height]
    
    def add_height(self, h):
        # TODO: увеличить высоту колонны
        self.height += h
        self.history.append(self._height)
        return self
    
    def add_capital(self):
        # TODO: увеличить уровень декора капители
        self.capital_decor += 1
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 250)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное появление колоннад перед зданиями

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
                'capital': getattr(elem, 'capital_decor', 0),
                'floors': getattr(elem, 'floors', 0)
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
                              facecolor='#F5DEB3', edgecolor='none'))
        
        # Отрисовка элементов
        for elem in self.elements:
            elem.draw(ax)
        
        ax.set_title(f"Шаг {step}: Античные колонны")
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

Создайте композицию из 2 зданий. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются колонны перед зданиями
- Шаги 4-5: колонны растут и получают декор капители
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
    Building(100, 50, 100, 80, "B1", 800, 600),
    Building(400, 50, 100, 75, "B2", 800, 600)
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
        # Появление колонн перед зданиями
        for b in buildings:
            for i in range(2):
                col_x = b.x + 20 + i * 60
                column = Column(col_x, b.y, 25, 100, 
                               f"Column_{b.name}_{i}", 800, 600)
                canvas.add_element(column)
    elif step <= 5:
        # Рост колонн и добавление декора
        for elem in canvas.elements:
            if isinstance(elem, Column):
                elem.add_height(15)
                if step >= 4:
                    elem.add_capital()
            elif isinstance(elem, Building) and step == 4:
                elem.add_capital()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    c = getattr(elem, 'capital_decor', getattr(elem, 'capital_level', 0))
    print(f"{elem.name}: высота={h}, капитель={c}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания белого цвета без колонн
- Шаг 1-2: здания растут, цвет постепенно становится золотистым
- Шаг 3: перед зданиями появляются колонны (полупрозрачные, светло-золотые)
- Шаг 4-5: колонны растут, добавляется декор капители
- Шаг 6: финальная композиция с градиентом от белого к насыщенному золотому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→золотой
- Появлению новых архитектурных элементов (колонн)
- Добавлению античного декора и капителей
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как античный храм с колоннами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление колоннад и трансформацию цветовой схемы от белого к золотому. Важно показать разнообразие: обычные здания и колонны должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Column3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для колонны дополнительно хранить `self.capital_height` (высота капители)
- Метод `get_color()` возвращает цвет в градиенте от белого к золотому

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
        white = to_rgb('#FFFFFF')
        gold = to_rgb('#FFD700')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (g - w) * ratio for w, g in zip(white, gold))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Column3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        self.capital_height = height * 0.15  # высота капители
        self.capital_decor = 0
    
    def get_color(self):
        total_h = self.path[-1] + self.capital_height
        self.color_ratio = min(1.0, total_h / 300)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с фронтоном
- `create_column_mesh(x, y, width, depth, height, capital_height, color)` — колонна (база + ствол + капитель)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с античными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7,0,0,1,1,2,2,3,3]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4,1,1,2,2,3,3,0,0,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1,5,4,6,5,7,6,4,7,0,3]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_column_mesh(x, y, width, depth, height, capital_height, color):
    """Создает колонну: база + ствол + капитель"""
    traces = []
    
    # База колонны
    base_h = height * 0.1
    traces.append(create_building_mesh(x - 5, y, width + 10, depth, base_h, color))
    
    # Ствол колонны (цилиндр через аппроксимацию)
    n_segments = 16
    shaft_x, shaft_y, shaft_z = [], [], []
    center_x, center_y = x + width/2, y + depth/2
    radius = width/2
    shaft_bottom = base_h
    shaft_top = height - capital_height
    
    for angle in np.linspace(0, 2*np.pi, n_segments):
        shaft_x.append(center_x + radius * np.cos(angle))
        shaft_y.append(center_y + radius * np.sin(angle))
        shaft_z.append(shaft_bottom)
    
    for angle in np.linspace(0, 2*np.pi, n_segments):
        shaft_x.append(center_x + radius * np.cos(angle))
        shaft_y.append(center_y + radius * np.sin(angle))
        shaft_z.append(shaft_top)
    
    # Создаём цилиндр через Mesh3d
    shaft_mesh = go.Mesh3d(
        x=shaft_x, y=shaft_y, z=shaft_z,
        color=color, opacity=0.95,
        alphahull=0
    )
    traces.append(shaft_mesh)
    
    # Капитель (декоративный элемент сверху)
    capital_h = capital_height
    traces.append(create_building_mesh(x - 8, y, width + 16, depth, capital_h, color))
    
    # Каннелюры (вертикальные линии)
    for i in range(8):
        angle = 2 * np.pi * i / 8
        groove_x = [center_x + radius * 0.95 * np.cos(angle)] * 2
        groove_y = [center_y + radius * 0.95 * np.sin(angle)] * 2
        groove_z = [shaft_bottom, shaft_top]
        traces.append(go.Scatter3d(
            x=groove_x, y=groove_y, z=groove_z,
            mode='lines', line=dict(color='#8B4513', width=1, opacity=0.5)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Column3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаг 4**: появляются колонны перед зданиями
- **Шаги 5-7**: колонны растут и получают декор капители

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Column3D(1.5, 1.0, 0.3, 80, "C1"),   # появится позже
    Column3D(3.5, 1.0, 0.3, 85, "C2")    # появится позже
]

# Колонны появляются на шаге 4
for elem in elements[2:]:
    elem.path = [0]  # начальная высота 0

# Коэффициенты роста
growth_rates = [15, 20, 18, 18]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Column3D) and step < 4:
            continue  # колонна ещё не появилась
        
        if isinstance(elem, Building3D):
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Column3D):
            if step == 4:
                # Первое появление колонны
                elem.path.append(80)
            elif step > 4:
                new_h = elem.path[-1] + growth_rates[i]
                elem.path.append(new_h)
                if step >= 6:
                    elem.capital_decor += 1
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (мраморный цвет)
- Все здания с текущей высотой из `path[step]`
- Колонны, которые появляются только с шага 4
- Цветовую динамику от белого к золотому

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
    
    # Поверхность земли (мраморный цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale=[[0, '#F5F5DC'], [1, '#E8D7C3']], 
        showscale=False, opacity=0.5
    ))
    
    for elem in elements:
        # Пропускаем колонны, если они ещё не появились
        if isinstance(elem, Column3D) and (step >= len(elem.path) or elem.path[step] == 0):
            continue
        
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Column3D) and h > 0:
            color = elem.get_color()
            traces.extend(create_column_mesh(
                elem.x, elem.y, elem.width, 0.4, h, h * 0.15, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Античные колонны".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора античной архитектуры
- Добавлены подписи и заголовок с темой "От белого к золотому"

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
    title="🏛️ Анимация: Античные колонны (градиент от белого к золотому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами
- При нажатии Play:
  - Здания начинают расти
  - На шаге 4 появляются колонны перед зданиями
  - Цвет всех элементов плавно переходит от белого к насыщенному золотому
  - Колонны получают декор капители и каннелюры
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" античного архитектурного ансамбля
