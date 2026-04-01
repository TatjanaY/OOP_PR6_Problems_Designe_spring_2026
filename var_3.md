# **Вариант № 3**

## **Генеративная художественная композиция "Тени пирамид" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей древнеегипетской культуры, который хочет получить интерактивную генеративную инсталляцию "Тени пирамид". Инсталляция должна создавать уникальные архитектурные композиции в египетском стиле, где здания и обелиски оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и обелиски — это "живые" объекты, способные расти, менять цвет от светло-серого к чёрному и реагировать на взаимодействие, создавая динамичные композиции в стиле Древнего Египта.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 4
obelisk_probability = 0.5  # вероятность появления обелиска
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от светло-серого к чёрному. Это позволяет плавно менять настроение инсталляции (рассвет/полдень/закат) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и обелисков, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов (здания, обелиски) могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в египетском стиле (иероглифы, колонны)
- **`Obelisk`** — обелиск (сужающаяся кверху четырёхгранная колонна с пирамидионом)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в египетском стиле
        # TODO: добавить окна с иероглифами (2 ряда по 3 окна)
        # TODO: добавить декоративные колонны по краям
        pass

class Obelisk(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать сужающийся ствол обелиска (трапеция)
        # TODO: отрисовать пирамидион на вершине (треугольник)
        # TODO: добавить иероглифы на гранях
        # TODO: использовать градиент от серого к чёрному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_obelisk_near(building)` добавляет обелиск рядом с зданием
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
    
    def add_obelisk_near(self, building):
        # TODO: создать обелиск рядом со зданием
        # TODO: позиция: building.x + building.width + 20, building.y
        # TODO: добавить обелиск в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('dawn', 'noon', 'dusk') с градиентом от светло-серого к чёрному
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'obelisk', 'hieroglyph')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #D3D3D3 к #000000 (ratio от 0 до 1)

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
                'sand': '#E1C699',
                'base_gray': '#D3D3D3',
                'base_black': '#000000'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для dawn/noon/dusk
        # Градиент от светло-серого к чёрному усиливается к dusk
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от светло-серого к чёрному
        # ratio: 0.0 = #D3D3D3, 1.0 = #000000
        from matplotlib.colors import to_rgb, to_hex
        gray = to_rgb('#D3D3D3')
        black = to_rgb('#000000')
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(gray, black))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 4 элементов (здания и обелиски рядом с ними), используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 2 Building на координатах (60, 400), (300, 380)
# - 2 Obelisk рядом с зданиями с вероятностью 50%

# Пример создания обелиска:
# obelisk = Obelisk(building.x + building.width + 30, building.y, 
#                   40, 150, "Obelisk1")
# canvas.add_element(obelisk)

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
        # TODO: создать Rectangle для фасада в египетском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна с иероглифами (2 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.12
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)
                # TODO: добавить символ иероглифа (текст или простой паттерн)
                ax.text(wx + window_w/2, wy + window_h/2, '𓀀', 
                       ha='center', va='center', fontsize=8, color='#8B4513')

class Obelisk(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 200)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать сужающийся ствол (трапеция)
        base_w = self.width
        top_w = self.width * 0.4
        obelisk_body = Polygon([
            (self.x, self.y),
            (self.x + base_w, self.y),
            (self.x + base_w - (base_w - top_w)/2, self.y + self.height * 0.85),
            (self.x + (base_w - top_w)/2, self.y + self.height * 0.85)
        ], facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(obelisk_body)
        # TODO: отрисовать пирамидион на вершине (золотой треугольник)
        pyramidion = Polygon([
            (self.x + (base_w - top_w)/2, self.y + self.height * 0.85),
            (self.x + base_w - (base_w - top_w)/2, self.y + self.height * 0.85),
            (self.x + base_w/2, self.y + self.height)
        ], facecolor='#FFD700', edgecolor='#8B4513', linewidth=1)
        ax.add_patch(pyramidion)
        # TODO: добавить иероглифы на гранях обелиска
        ax.text(self.x + base_w/2, self.y + self.height * 0.4, '𓁐', 
               ha='center', va='center', fontsize=10, color='#8B4513')
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и обелиски должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" египетской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать обелиск с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от светло-серого к чёрному в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию появления обелисков рядом с зданиями, где обелиски "вырастают" постепенно, а цветовая динамика подчёркивает процесс "оживления" древнеегипетского города.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Obelisk: ширина должна сужаться кверху (top_width = width * 0.4)

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
        self.hieroglyph_level = 0  # уровень иероглифического декора
    
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

class Obelisk(ArchElement):
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive numeric")
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_hieroglyphs()` — добавляет иероглифический декор на фасад
- `get_color()` — возвращает цвет в градиенте от светло-серого к чёрному

**Для Obelisk**:
- `add_peak(height)` — увеличивает высоту обелиска и пирамидиона
- `add_inscriptions()` — добавляет иероглифические надписи
- `get_color()` — возвращает цвет в градиенте от светло-серого к чёрному

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        return self
    
    def add_hieroglyphs(self):
        # TODO: увеличить hieroglyph_level
        self.hieroglyph_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от светло-серого к чёрному
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать здание с иероглифами
        # TODO: если hieroglyph_level > 0: добавить больше иероглифов
        pass

class Obelisk(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.peak_height = height * 0.15  # высота пирамидиона
        self.inscription_level = 0
        self.history = [height]
    
    def add_peak(self, height):
        # TODO: увеличить высоту обелиска и пирамидиона
        self.height += height
        self.peak_height += height * 0.15
        self.history.append(self._height)
        return self
    
    def add_inscriptions(self):
        # TODO: увеличить уровень иероглифических надписей
        self.inscription_level += 1
        return self
    
    def get_color(self):
        ratio = min(1.0, (self._height + self.peak_height) / 300)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное появление обелисков рядом с зданиями

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
                'hieroglyphs': getattr(elem, 'hieroglyph_level', 0),
                'peak': getattr(elem, 'peak_height', 0)
            }
            for elem in self.elements
        }
        self.frames.append(frame)
    
    def show_frame(self, step, frame_data):
        fig, ax = plt.subplots(figsize=(10, 6))
        ax.set_xlim(0, self.width)
        ax.set_ylim(0, self.height)
        ax.set_facecolor('#87CEEB')  # небо
        
        # Песок
        ax.add_patch(Rectangle((0, 0), self.width, 50, 
                              facecolor='#E1C699', edgecolor='none'))
        
        # Отрисовка элементов
        for elem in self.elements:
            elem.draw(ax)
        
        ax.set_title(f"Шаг {step}: Тени пирамид")
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
- Шаг 3: появляются обелиски рядом с зданиями
- Шаги 4-5: обелиски растут и получают иероглифические надписи
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
    Building(60, 50, 90, 80, "B1", 800, 600),
    Building(300, 50, 85, 75, "B2", 800, 600)
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
        # Появление обелисков рядом с зданиями
        for b in buildings:
            obelisk = Obelisk(b.x + b.width + 30, b.y, 
                             40, 100, f"Obelisk_{b.name}", 800, 600)
            canvas.add_element(obelisk)
    elif step <= 5:
        # Рост обелисков и добавление иероглифов
        for elem in canvas.elements:
            if isinstance(elem, Obelisk):
                elem.add_peak(15)
                if step >= 4:
                    elem.add_inscriptions()
            elif isinstance(elem, Building) and step == 4:
                elem.add_hieroglyphs()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    p = getattr(elem, 'peak_height', 0)
    print(f"{elem.name}: высота={h}, пирамидион={p}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания светло-серого цвета без обелисков
- Шаг 1-2: здания растут, цвет постепенно темнеет
- Шаг 3: рядом с зданиями появляются обелиски (полупрозрачные, светло-серые)
- Шаг 4-5: обелиски растут, добавляются иероглифические надписи
- Шаг 6: финальная композиция с градиентом от светло-серого к насыщенному чёрному

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→чёрный
- Появлению новых архитектурных элементов (обелисков)
- Добавлению египетского декора и иероглифов
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как древнеегипетский город с обелисками "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление обелисков и трансформацию цветовой схемы от светло-серого к чёрному. Важно показать разнообразие: обычные здания и обелиски должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Obelisk3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для обелиска дополнительно хранить `self.peak_ratio` (пропорция пирамидиона)
- Метод `get_color()` возвращает цвет в градиенте от светло-серого к чёрному

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
        gray = to_rgb('#D3D3D3')
        black = to_rgb('#000000')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (b - g) * ratio for g, b in zip(gray, black))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Obelisk3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        self.peak_ratio = 0.15  # пропорция высоты пирамидиона
        self.inscription_level = 0
    
    def get_color(self):
        total_h = self.path[-1] + self.path[-1] * self.peak_ratio
        self.color_ratio = min(1.0, total_h / 350)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с иероглифами
- `create_obelisk_mesh(x, y, width, depth, height, peak_ratio, color)` — обелиск (сужающийся ствол + пирамидион)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с египетскими элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7,0,0,1,1,2,2,3,3]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4,1,1,2,2,3,3,0,0,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1,5,4,6,5,7,6,4,7,0,3]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_obelisk_mesh(x, y, width, depth, height, peak_ratio, color):
    """Создает обелиск: сужающийся ствол + пирамидион"""
    traces = []
    
    # Ствол обелиска (усечённая пирамида)
    base_w, base_d = width, depth
    top_w, top_d = width * 0.4, depth * 0.4
    shaft_height = height * (1 - peak_ratio)
    
    # 8 вершин усечённой пирамиды
    vertices_x = [x, x+base_w, x+base_w, x, 
                  x + (base_w-top_w)/2, x+base_w - (base_w-top_w)/2, 
                  x+base_w - (base_w-top_w)/2, x + (base_w-top_w)/2]
    vertices_y = [y, y, y+base_d, y+base_d,
                  y + (base_d-top_d)/2, y+base_d - (base_d-top_d)/2,
                  y+base_d - (base_d-top_d)/2, y + (base_d-top_d)/2]
    vertices_z = [0, 0, 0, 0, shaft_height, shaft_height, shaft_height, shaft_height]
    
    # Грани усечённой пирамиды
    i = [0,1,2,3,4,5,6,7,0,1,2,3]
    j = [1,2,3,0,5,6,7,4,4,5,6,7]
    k = [4,5,6,7,4,5,6,7,1,2,3,0]
    
    shaft = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                     i=i, j=j, k=k, color=color, opacity=0.95)
    traces.append(shaft)
    
    # Пирамидион (золотая вершина)
    apex_x, apex_y = x + width/2, y + depth/2
    apex_z = height
    
    pyramid_x = [x + (base_w-top_w)/2, x+base_w - (base_w-top_w)/2, 
                 x+base_w - (base_w-top_w)/2, x + (base_w-top_w)/2, apex_x]
    pyramid_y = [y + (base_d-top_d)/2, y+base_d - (base_d-top_d)/2,
                 y+base_d - (base_d-top_d)/2, y + (base_d-top_d)/2, apex_y]
    pyramid_z = [shaft_height]*4 + [apex_z]
    
    i_pyr = [0,1,2,3]
    j_pyr = [1,2,3,0]
    k_pyr = [4,4,4,4]
    
    peak = go.Mesh3d(x=pyramid_x, y=pyramid_y, z=pyramid_z,
                    i=i_pyr, j=j_pyr, k=k_pyr,
                    color='#FFD700', opacity=0.9)
    traces.append(peak)
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 3 элементов (2 Building3D + 1 Obelisk3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаг 4**: появляется обелиск рядом с первым зданием
- **Шаги 5-7**: обелиск растёт и получает иероглифические надписи

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(2.5, 1.0, 0.9, 70, "B2"),
    Obelisk3D(3.8, 1.0, 0.4, 80, "Ob1")  # появится позже
]

# Обелиск появляется на шаге 4
elements[2].path = [0]  # начальная высота 0

# Коэффициенты роста
growth_rates = [15, 20, 18]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Obelisk3D) and step < 4:
            continue  # обелиск ещё не появился
        
        if isinstance(elem, Building3D):
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Obelisk3D):
            if step == 4:
                # Первое появление обелиска
                elem.path.append(80)
            elif step > 4:
                new_h = elem.path[-1] + growth_rates[i]
                elem.path.append(new_h)
                if step >= 6:
                    elem.inscription_level += 1
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (песочный цвет)
- Все здания с текущей высотой из `path[step]`
- Обелиск, который появляется только с шага 4
- Цветовую динамику от светло-серого к чёрному

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
    
    # Поверхность земли (песочный цвет)
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale=[[0, '#E1C699'], [1, '#C19A6B']], 
        showscale=False, opacity=0.5
    ))
    
    for elem in elements:
        # Пропускаем обелиск, если он ещё не появился
        if isinstance(elem, Obelisk3D) and (step >= len(elem.path) or elem.path[step] == 0):
            continue
        
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Obelisk3D) and h > 0:
            color = elem.get_color()
            traces.extend(create_obelisk_mesh(
                elem.x, elem.y, elem.width, 0.4, h, 0.15, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Тени пирамид".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора египетской архитектуры
- Добавлены подписи и заголовок с темой "От серого к чёрному"

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
        camera=dict(eye=dict(x=3.0, y=2.5, z=2.0)),
        aspectmode='manual',
        aspectratio=dict(x=1.8, y=0.9, z=1.3),
        bgcolor='#87CEEB'
    ),
    title="🏺 Анимация: Тени пирамид (градиент от серого к чёрному)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания светло-серого цвета с минимальными высотами
- При нажатии Play:
  - Здания начинают расти
  - На шаге 4 появляется обелиск рядом с первым зданием
  - Цвет всех элементов плавно переходит от светло-серого к насыщенному чёрному
  - Обелиск получает иероглифические надписи и золотой пирамидион
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" древнеегипетского архитектурного ансамбля
