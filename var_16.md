# **Вариант № 16**

## **Генеративная художественная композиция "Крепостная стена" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей средневековой истории, который хочет получить интерактивную генеративную инсталляцию "Крепостная стена". Инсталляция должна создавать уникальные композиции с крепостными стенами, где здания, башни и стены оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания, башни и стены — это "живые" объекты, способные расти, менять цвет от серого к тёмно-серому и реагировать на взаимодействие, создавая динамичные композиции в стиле средневековых крепостей.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
wall_count = 2  # количество участков стены
tower_count = 2  # количество башен
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к тёмно-серому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий, башен и крепостных стен, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

Создайте три класса, наследующихся от `ArchElement`:

- **`Building`** — прямоугольное здание с окнами в крепостном стиле
- **`Tower`** — башня с пирамидальной крышей и шпилем
- **`Wall`** — крепостная стена с зубцами

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в крепостном стиле
        # TODO: добавить узкие окна-бойницы
        # TODO: добавить каменную текстуру
        pass

class Tower(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать башню с пирамидальной крышей
        # TODO: добавить шпиль на вершине
        # TODO: добавить бойницы по периметру
        # TODO: использовать градиент от серого к тёмно-серому
        pass

class Wall(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать крепостную стену
        # TODO: добавить зубцы на верхней части
        # TODO: добавить ворота в центре (опционально)
        # TODO: использовать градиент от серого к тёмно-серому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_wall_between(elem1, elem2)` создаёт стену между двумя элементами
- метод `add_tower_at(position)` добавляет башню в указанной позиции
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
    
    def add_wall_between(self, elem1, elem2):
        # TODO: создать стену между elem1 и elem2
        # TODO: позиция: от elem1.x + elem1.width до elem2.x
        # TODO: добавить стену в elements
        pass
    
    def add_tower_at(self, x, y):
        # TODO: создать башню в указанной позиции
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от серого к тёмно-серому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'wall', 'tower')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #808080 к #404040 (ratio от 0 до 1)

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
                'base_dark_gray': '#404040'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от серого к тёмно-серому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от серого к тёмно-серому
        # ratio: 0.0 = серый, 1.0 = тёмно-серый
        from matplotlib.colors import to_rgb, to_hex
        gray = to_rgb('#808080')
        dark_gray = to_rgb('#404040')
        interpolated = tuple(g + (d - g) * ratio for g, d in zip(gray, dark_gray))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий, 2 башен и 2 участков стены, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Tower на координатах (150, 350), (350, 360)
# - 2 Wall между зданиями

# Пример создания стены:
# wall = Wall(elem1.x + elem1.width, elem1.y, 
#             elem2.x - (elem1.x + elem1.width), 80, "Wall1")
# canvas.add_element(wall)

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
        # TODO: создать Rectangle для фасада в крепостном стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#404040', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить узкие окна-бойницы (3 ряда по 2)
        window_w, window_h = self.width * 0.08, self.height * 0.1
        for row in range(3):
            for col in range(2):
                wx = self.x + self.width * 0.15 + col * (self.width * 0.35)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#696969', edgecolor='#404040')
                ax.add_patch(window)

class Tower(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 300)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать башню (прямоугольник с крышей)
        tower_body = Rectangle((self.x, self.y), self.width, self.height * 0.85, 
                              facecolor=color, edgecolor='#404040', linewidth=2)
        ax.add_patch(tower_body)
        # TODO: добавить пирамидальную крышу
        roof = Polygon([
            (self.x - 10, self.y + self.height * 0.85),
            (self.x + self.width + 10, self.y + self.height * 0.85),
            (self.x + self.width/2, self.y + self.height)
        ], facecolor='#404040', edgecolor='#404040', linewidth=2)
        ax.add_patch(roof)
        # TODO: добавить шпиль на вершине
        ax.plot([self.x + self.width/2, self.x + self.width/2], 
               [self.y + self.height, self.y + self.height + 20], 
               color='#404040', linewidth=3)
        # TODO: добавить бойницы
        for i in range(4):
            loophole_y = self.y + i * (self.height * 0.2)
            loophole = Rectangle((self.x + self.width/2 - 5, loophole_y), 
                                10, 15, facecolor='#696969', edgecolor='#404040')
            ax.add_patch(loophole)

class Wall(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 150)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать крепостную стену
        wall_body = Rectangle((self.x, self.y), self.width, self.height * 0.8, 
                             facecolor=color, edgecolor='#404040', linewidth=2)
        ax.add_patch(wall_body)
        # TODO: добавить зубцы на верхней части
        battlement_w = self.width / 10
        for i in range(10):
            if i % 2 == 0:
                battlement = Rectangle((self.x + i * battlement_w, 
                                       self.y + self.height * 0.8),
                                      battlement_w, self.height * 0.2,
                                      facecolor=color, edgecolor='#404040')
                ax.add_patch(battlement)
        # TODO: добавить ворота в центре (опционально)
        gate_w = self.width * 0.15
        gate = Rectangle((self.x + self.width/2 - gate_w/2, self.y), 
                        gate_w, self.height * 0.5,
                        facecolor='#654321', edgecolor='#404040', linewidth=2)
        ax.add_patch(gate)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания, башни и стены должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" крепостной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать стену с отрицательной длиной или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к тёмно-серому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления участков стены между башнями, где стена появляется постепенно, а цветовая динамика подчёркивает процесс "оживления" крепости.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Wall: длина должна быть достаточной для зубцов (не менее 50px)

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
        self.wall_level = 0  # уровень стены
    
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

class Wall(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.battlement_count = 10  # количество зубцов
        self.history = [height]
    
    @property
    def width(self):
        return self._width
    
    @width.setter
    def width(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("width must be positive")
        if value < 50:  # минимальная длина для зубцов
            value = 50
        self.history.append(self._width)
        self._width = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_wall_decor()` — добавляет крепостной декор
- `get_color()` — возвращает цвет в градиенте от серого к тёмно-серому

**Для Tower**:
- `add_spire(height)` — увеличивает высоту шпиля
- `get_color()` — возвращает цвет в градиенте от серого к тёмно-серому

**Для Wall**:
- `add_wall(length)` — увеличивает длину стены
- `add_battlement()` — добавляет зубцы к стене
- `get_color()` — возвращает цвет в градиенте от серого к тёмно-серому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_wall_decor(self):
        # TODO: увеличить wall_level
        self.wall_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к тёмно-серому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с бойницами
        # TODO: если wall_level > 0: добавить крепостной декор
        pass

class Tower(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.spire_height = 0
        self.history = [height]
    
    def add_spire(self, height):
        # TODO: увеличить высоту шпиля
        self.spire_height += height
        self.history.append(self.spire_height)
        return self
    
    def get_color(self):
        ratio = min(1.0, (self._height + self.spire_height) / 350)
        return theme.get_gradient(ratio)

class Wall(ArchElement):
    def add_wall(self, length):
        # TODO: увеличить длину стены
        self.width += length
        self.history.append(self._width)
        return self
    
    def add_battlement(self):
        # TODO: увеличить количество зубцов
        self.battlement_count += 2
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 150)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление участков стены

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
                'width': getattr(elem, '_width', elem.width),
                'battlements': getattr(elem, 'battlement_count', 0)
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
        ax.set_title(f"Шаг {step}: Крепостная стена")
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

Создайте композицию из 2 зданий, 2 башен и 2 участков стены. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются башни
- Шаги 4-5: стены растут, добавляются зубцы
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

towers = [
    Tower(180, 100, 50, 150, "T1", 800, 600),
    Tower(430, 100, 50, 160, "T2", 800, 600)
]

walls = [
    Wall(150, 150, 30, 80, "W1", 800, 600),
    Wall(400, 150, 30, 80, "W2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for t in towers:
    canvas.add_element(t)
for w in walls:
    canvas.add_element(w)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step == 3:
        # Появление башен
        for elem in towers:
            elem.add_spire(20)
    elif step <= 5:
        # Стены растут и добавляют зубцы
        for elem in walls:
            elem.add_wall(20)
            elem.add_battlement()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    w = getattr(elem, '_width', getattr(elem, 'width', 'N/A'))
    b = getattr(elem, 'battlement_count', 0)
    print(f"{elem.name}: высота={h}, длина={w}, зубцы={b}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания, башни и стены серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится тёмно-серым
- Шаг 3: башни увеличиваются, появляются шпили
- Шаг 4-5: стены растут в длину, добавляются новые зубцы
- Шаг 6: финальная композиция с градиентом от серого к насыщенному тёмно-серому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→тёмно-серый
- Появлению новых участков стены
- Добавлению зубцов на стенах
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как средневековая крепость "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление башен и стен и трансформацию цветовой схемы от серого к тёмно-серому. Важно показать разнообразие: здания, башни и стены должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D`, `Tower3D` и `Wall3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для стены дополнительно хранить `self.wall_path` (история длины стены)
- Метод `get_color()` возвращает цвет в градиенте от серого к тёмно-серому

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
        # TODO: вернуть цвет в градиенте от серого к тёмно-серому
        from matplotlib.colors import to_hex, to_rgb
        gray = to_rgb('#808080')
        dark_gray = to_rgb('#404040')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (d - g) * ratio for g, d in zip(gray, dark_gray))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Tower3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        self.spire_height = height * 0.15
        self.spire_path = [self.spire_height]
    
    def get_color(self):
        total_h = self.path[-1] + self.spire_path[-1]
        self.color_ratio = min(1.0, total_h / 350)
        return super().get_color()


class Wall3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.battlement_count = 10
        self.battlement_count = 10
        # TODO: создать self.wall_path = [width]
        self.wall_path = [width]
    
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 150)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с крепостными элементами
- `create_tower_mesh(x, y, width, depth, height, spire_height, color)` — башня (цилиндр + крыша + шпиль)
- `create_wall_mesh(x, y, width, depth, height, battlement_count, color)` — стена (прямоугольник + зубцы)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с крепостными элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_tower_mesh(x, y, width, depth, height, spire_height, color):
    """Создает башню: цилиндр + крыша + шпиль"""
    traces = []
    
    # Корпус башни (цилиндр)
    n_segments = 16
    base_x, base_y = [], []
    top_x, top_y = [], []
    
    center_x, center_y = x + width/2, y + depth/2
    radius = width/2
    tower_height = height * 0.85
    
    for angle in np.linspace(0, 2*np.pi, n_segments):
        base_x.append(center_x + radius * np.cos(angle))
        base_y.append(center_y + radius * np.sin(angle))
        top_x.append(center_x + radius * np.cos(angle))
        top_y.append(center_y + radius * np.sin(angle))
    
    body_z = [0]*n_segments + [tower_height]*n_segments
    tower_mesh = go.Mesh3d(
        x=base_x + top_x,
        y=base_y + top_y,
        z=body_z,
        color=color, opacity=0.95,
        alphahull=0
    )
    traces.append(tower_mesh)
    
    # Пирамидальная крыша
    apex_x, apex_y = center_x, center_y
    apex_z = tower_height + height * 0.15
    
    roof_x = [center_x + radius * np.cos(a) for a in np.linspace(0, 2*np.pi, n_segments)]
    roof_y = [center_y + radius * np.sin(a) for a in np.linspace(0, 2*np.pi, n_segments)]
    roof_z = [tower_height] * n_segments
    
    roof_x.append(apex_x)
    roof_y.append(apex_y)
    roof_z.append(apex_z)
    
    roof_mesh = go.Mesh3d(
        x=roof_x, y=roof_y, z=roof_z,
        color='#404040', opacity=0.9,
        alphahull=0
    )
    traces.append(roof_mesh)
    
    # Шпиль
    traces.append(go.Scatter3d(
        x=[apex_x, apex_x], y=[apex_y, apex_y], 
        z=[apex_z, apex_z + spire_height],
        mode='lines',
        line=dict(color='#404040', width=4)
    ))
    
    return traces


def create_wall_mesh(x, y, width, depth, height, battlement_count, color):
    """Создает стену: прямоугольник + зубцы"""
    traces = []
    
    # Основное тело стены
    wall_body = create_building_mesh(x, y, width, depth, height * 0.8, color)
    traces.append(wall_body)
    
    # Зубцы на верхней части
    battlement_w = width / battlement_count
    battlement_h = height * 0.2
    
    for i in range(battlement_count):
        if i % 2 == 0:  # каждый второй зубец
            bx = x + i * battlement_w
            traces.append(create_building_mesh(
                bx, y, battlement_w, depth, battlement_h, color
            ))
    
    # Ворота в центре
    gate_w = width * 0.15
    traces.append(create_building_mesh(
        x + width/2 - gate_w/2, y, gate_w, depth * 1.1, height * 0.5,
        '#654321'
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 5 элементов (2 Building3D + 2 Tower3D + 1 Wall3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-5**: башни увеличиваются
- **Шаги 6-7**: стена растёт, добавляются зубцы

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Tower3D(2.0, 1.0, 0.5, 150, "T1"),
    Tower3D(4.0, 1.0, 0.6, 160, "T2"),
    Wall3D(2.5, 1.0, 1.0, 80, "W1")
]

# Коэффициенты роста
growth_rates = [15, 20, 18, 18, 25]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Tower3D) and step >= 4 and step <= 5:
            # Башни растут на шагах 4-5
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.spire_height += 5
            elem.spire_path.append(elem.spire_height)
        elif isinstance(elem, Wall3D) and step >= 6:
            # Стена растёт на шагах 6-7
            new_w = elem._width + growth_rates[i]
            elem._width = new_w
            elem.wall_path.append(new_w)
            elem.battlement_count += 2
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все башни с текущей высотой из `path[step]`
- Стена с текущей длиной из `wall_path[step]` и количеством зубцов
- Цветовую динамику от серого к тёмно-серому

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
        elif isinstance(elem, Tower3D):
            color = elem.get_color()
            sh = elem.spire_path[step] if step < len(elem.spire_path) else elem.spire_path[-1]
            traces.extend(create_tower_mesh(
                elem.x, elem.y, elem.width, 0.4, h, sh, color
            ))
        elif isinstance(elem, Wall3D):
            color = elem.get_color()
            w = elem.wall_path[step] if step < len(elem.wall_path) else elem.wall_path[-1]
            traces.extend(create_wall_mesh(
                elem.x, elem.y, w, 0.4, h, elem.battlement_count, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Крепостная стена".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора крепостной архитектуры
- Добавлены подписи и заголовок с темой "От серого к тёмно-серому"

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
    title="🏰 Анимация: Крепостная стена (градиент от серого к тёмно-серому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами, башни и стена
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Башни увеличиваются, шпили растут (шаги 4-5)
  - Стена растёт в длину, добавляются зубцы (шаги 6-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному тёмно-серому (`#404040`)
  - Количество зубцов на стене увеличивается с 10 до 14-16
  - Ворота в центре стены видны в коричневом цвете
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" средневекового крепостного ансамбля
