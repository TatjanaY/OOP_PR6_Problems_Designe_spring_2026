# **Вариант № 6**

## **Генеративная художественная композиция "Ярусная пагода" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей японской культуры, который хочет получить интерактивную генеративную инсталляцию "Ярусная пагода". Инсталляция должна создавать уникальные архитектурные композиции в японском стиле, где здания и пагоды оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и пагоды — это "живые" объекты, способные расти, менять цвет от красного к золотому и реагировать на взаимодействие, создавая динамичные композиции в стиле японской архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
pagoda_tiers = 5  # количество ярусов пагоды
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от красного к золотому. Это позволяет плавно менять настроение инсталляции (утро/полдень/закат) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и пагод, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в японском стиле (сёдзи, изогнутая крыша)
- **`Pagoda`** — ярусная башня с уменьшающимися ярусами и изогнутыми крышами

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в японском стиле
        # TODO: добавить окна с решётками (сёдзи)
        # TODO: добавить изогнутую крышу
        pass

class Pagoda(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать ярусы пагоды (5 уменьшающихся ярусов)
        # TODO: добавить изогнутые крыши на каждом ярусе
        # TODO: добавить шпиль на вершине
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
- метод `add_pagoda_near(building)` создаёт пагоду рядом со зданием
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
    
    def add_pagoda_near(self, building):
        # TODO: создать пагоду рядом со зданием
        # TODO: позиция: building.x + building.width + 30, building.y
        # TODO: добавить пагоду в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'sunset') с градиентом от красного к золотому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'pagoda', 'roof')
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
        # TODO: установить цвета для morning/noon/sunset
        # Градиент от красного к золотому усиливается к sunset
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

Создайте композицию из 3 зданий с пагодами рядом, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 3 Pagoda рядом с каждым зданием

# Пример создания пагоды:
# pagoda = Pagoda(building.x + building.width + 30, building.y, 
#                 80, 200, "Pagoda1")
# canvas.add_element(pagoda)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Polygon

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в японском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна сёдзи (решётчатые окна)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#F5F5DC', edgecolor='#8B4513')
                ax.add_patch(window)
                # Решётка сёдзи
                ax.plot([wx, wx + window_w], [wy + window_h/2, wy + window_h/2], 
                       color='#8B4513', linewidth=1)
                ax.plot([wx + window_w/2, wx + window_w/2], [wy, wy + window_h], 
                       color='#8B4513', linewidth=1)
        # TODO: добавить изогнутую крышу
        roof = Polygon([
            (self.x - 15, self.y + self.height),
            (self.x + self.width + 15, self.y + self.height),
            (self.x + self.width/2, self.y + self.height + 40)
        ], facecolor='#DC143C', edgecolor='#8B4513', linewidth=2)
        ax.add_patch(roof)

class Pagoda(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 300)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать 5 ярусов с уменьшающейся шириной
        current_width = self.width
        current_y = self.y
        tier_height = self.height / 5
        for i in range(5):
            # Ярус
            tier = Rectangle((self.x + (self.width - current_width)/2, 
                            current_y, current_width, tier_height),
                           facecolor=color, edgecolor='#8B4513', linewidth=2)
            ax.add_patch(tier)
            # Изогнутая крыша яруса
            roof = Polygon([
                (self.x + (self.width - current_width)/2 - 15, current_y + tier_height),
                (self.x + (self.width - current_width)/2 + current_width + 15, current_y + tier_height),
                (self.x + self.width/2, current_y + tier_height + 25)
            ], facecolor='#DC143C', edgecolor='#8B4513', linewidth=2)
            ax.add_patch(roof)
            current_width *= 0.85
            current_y += tier_height
        # TODO: добавить шпиль на вершине
        ax.plot([self.x + self.width/2, self.x + self.width/2], 
               [current_y, current_y + 30], color='#FFD700', linewidth=3)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и пагоды должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" японской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать пагоду с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от красного к золотому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления ярусов к пагоде, где каждый новый ярус появляется постепенно, а цветовая динамика подчёркивает процесс "оживления" японского храма.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Pagoda: количество ярусов должно быть не менее 3 и не более 9

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
        self.tier_level = 0  # уровень декора
    
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

class Pagoda(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.tiers = 5  # количество ярусов
        self.history = [height]
    
    @property
    def tiers(self):
        # TODO: вернуть количество ярусов
        return self._tiers
    
    @tiers.setter
    def tiers(self, value):
        # TODO: проверить что 3 <= value <= 9
        # TODO: установить _tiers
        pass
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_tier_decor()` — добавляет декор в японском стиле
- `get_color()` — возвращает цвет в градиенте от красного к золотому

**Для Pagoda**:
- `add_tier(n)` — добавляет n ярусов к пагоде
- `get_color()` — возвращает цвет в градиенте от красного к золотому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_tier_decor(self):
        # TODO: увеличить tier_level
        self.tier_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от красного к золотому
        #   ratio = min(1.0, self._height / 300)
        #   использовать theme.get_gradient(ratio)
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами сёдзи
        # TODO: если tier_level > 0: добавить японский декор
        pass

class Pagoda(ArchElement):
    def add_tier(self, n):
        # TODO: увеличить количество ярусов на n
        # TODO: увеличить высоту пропорционально
        # TODO: добавить запись в history
        # TODO: вернуть self
        pass
    
    def get_color(self):
        # TODO: градиент от красного к золотому по высоте
        ratio = min(1.0, self._height / 400)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление ярусов к пагоде

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
                'tiers': getattr(elem, 'tiers', 0),
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
        ax.set_title(f"Шаг {step}: Ярусная пагода")
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

Создайте композицию из 2 зданий и 2 пагод. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: пагоды получают первый дополнительный ярус
- Шаги 4-5: пагоды продолжают расти, добавляются ярусы
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

pagodas = [
    Pagoda(180, 50, 80, 150, "P1", 800, 600),
    Pagoda(430, 50, 80, 160, "P2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for p in pagodas:
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
        # Пагоды добавляют ярусы
        for elem in pagodas:
            elem.add_tier(1)
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    t = getattr(elem, 'tiers', 0)
    print(f"{elem.name}: высота={h}, ярусы={t}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и пагоды красного цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится золотистым
- Шаг 3: пагоды получают первый дополнительный ярус (с 5 до 6)
- Шаг 4-5: пагоды продолжают расти, добавляются новые ярусы (до 7-8)
- Шаг 6: финальная композиция с градиентом от красного к насыщенному золотому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту красный→золотой
- Появлению новых ярусов на пагодах
- Добавлению японского декора (сёдзи, изогнутые крыши)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как японский храм с пагодами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление новых ярусов пагоды и трансформацию цветовой схемы от красного к золотому. Важно показать разнообразие: обычные здания и ярусные пагоды должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Pagoda3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для пагоды дополнительно хранить `self.tier_path` (история количества ярусов)
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


class Pagoda3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.tiers = 5
        self.tiers = 5
        # TODO: создать self.tier_path = [5]
        self.tier_path = [5]
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 400)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с японской крышей
- `create_pagoda_mesh(x, y, width, depth, height, tiers, color)` — ярусная пагода (несколько уменьшающихся параллелепипедов)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с японскими элементами"""
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


def create_pagoda_mesh(x, y, width, depth, height, tiers, color):
    """Создает ярусную пагоду: несколько уменьшающихся параллелепипедов"""
    traces = []
    
    tier_height = height / tiers
    current_width = width
    current_y = y
    
    for i in range(tiers):
        # Смещение для центрирования каждого яруса
        offset_x = x + (width - current_width) / 2
        offset_y = current_y
        
        # 8 вершин текущего яруса
        vertices_x = [offset_x, offset_x+current_width, offset_x+current_width, offset_x, 
                     offset_x, offset_x+current_width, offset_x+current_width, offset_x]
        vertices_y = [offset_y, offset_y, offset_y, offset_y,
                     offset_y+tier_height, offset_y+tier_height, 
                     offset_y+tier_height, offset_y+tier_height]
        vertices_z = [0, 0, depth, depth, 0, 0, depth, depth]
        
        # Грани яруса
        i_idx = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
        j_idx = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
        k_idx = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
        
        tier_mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                             i=i_idx, j=j_idx, k=k_idx,
                             color=color, opacity=0.95)
        traces.append(tier_mesh)
        
        # Уменьшаем ширину для следующего яруса
        current_width *= 0.85
        current_y += tier_height
    
    # Шпиль на вершине
    traces.append(go.Scatter3d(
        x=[x + width/2, x + width/2],
        y=[current_y, current_y],
        z=[current_y, current_y + 30],
        mode='lines',
        line=dict(color='#FFD700', width=4)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Pagoda3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: пагоды добавляют ярусы и растут

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Pagoda3D(2.0, 1.0, 0.7, 150, "P1"),
    Pagoda3D(4.0, 1.0, 0.8, 160, "P2")
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
        elif isinstance(elem, Pagoda3D) and step >= 4:
            # Пагоды растут и добавляют ярусы на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            if step % 2 == 0:
                elem.tiers += 1
                elem.tier_path.append(elem.tiers)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (зелёный цвет)
- Все здания с текущей высотой из `path[step]`
- Все пагоды с текущим количеством ярусов из `tier_path[step]`
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
        elif isinstance(elem, Pagoda3D):
            color = elem.get_color()
            t = elem.tier_path[step] if step < len(elem.tier_path) else elem.tier_path[-1]
            traces.extend(create_pagoda_mesh(
                elem.x, elem.y, elem.width, 0.4, h, t, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Ярусная пагода".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора японской архитектуры
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
    title="⛩️ Анимация: Ярусная пагода (градиент от красного к золотому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания красного цвета с минимальными высотами и пагоды с 5 ярусами
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Пагоды начинают добавлять ярусы (шаги 4-7)
  - Цвет всех элементов плавно переходит от красного (`#DC143C`) к насыщенному золотому (`#FFD700`)
  - Каждый новый ярус пагоды визуально меньше предыдущего (эффект уменьшения)
  - На вершине пагоды виден золотой шпиль
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" японского архитектурного ансамбля

