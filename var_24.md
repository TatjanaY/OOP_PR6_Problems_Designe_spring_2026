# **Вариант № 24**

## **Генеративная художественная композиция "Собор (Санкт-Петербург)" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей русской архитектуры, который хочет получить интерактивную генеративную инсталляцию "Собор (Санкт-Петербург)". Инсталляция должна создавать уникальные композиции с соборами, где здания, купола и шпили оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания, купола и шпили — это "живые" объекты, способные расти, менять цвет от белого к голубому и реагировать на взаимодействие, создавая динамичные композиции в стиле петербургской архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
dome_count = 3  # количество куполов
spire_count = 1  # количество шпилей
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к голубому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий, куполов и шпилей, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в русском стиле
- **`Dome`** — купол (луковичная глава)
- **`Spire`** — шпиль (высокая заострённая башня)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в русском стиле
        # TODO: добавить окна (3 ряда по 3 окна)
        # TODO: добавить декоративные элементы
        pass

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать луковичную главу (овал + крест)
        # TODO: добавить барабан (цилиндр под куполом)
        # TODO: добавить крест на вершине
        # TODO: использовать градиент от белого к голубому
        pass

class Spire(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать шпиль (сужающийся кверху)
        # TODO: добавить звезду/флюгер на вершине
        # TODO: использовать градиент от белого к голубому
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
- метод `add_spire_near(building)` добавляет шпиль рядом со зданием
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
        # TODO: позиция: по центру здания, высота = building.y + building.height
        # TODO: добавить купол в elements
        pass
    
    def add_spire_near(self, building):
        # TODO: создать шпиль рядом со зданием
        # TODO: позиция: building.x + building.width + 30, building.y
        # TODO: добавить шпиль в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от белого к голубому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'dome', 'spire')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #FFFFFF к #87CEEB (ratio от 0 до 1)

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
                'base_white': '#FFFFFF',
                'base_blue': '#87CEEB'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от белого к голубому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от белого к голубому
        # ratio: 0.0 = белый, 1.0 = голубой
        from matplotlib.colors import to_rgb, to_hex
        white = to_rgb('#FFFFFF')
        blue = to_rgb('#87CEEB')
        interpolated = tuple(w + (b - w) * ratio for w, b in zip(white, blue))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 3 куполами и 1 шпилем, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (100, 400), (300, 380), (500, 420)
# - 3 Dome над зданиями
# - 1 Spire рядом с центральным зданием

# Пример создания купола:
# dome = Dome(building.x + building.width*0.2, building.y + building.height, 
#             60, 80, "Dome1")
# canvas.add_element(dome)

# Пример создания шпиля:
# spire = Spire(building.x + building.width + 30, building.y, 
#               40, 200, "Spire1")
# canvas.add_element(spire)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Circle, Polygon, Ellipse

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в русском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#4169E1')
                ax.add_patch(window)
        # TODO: добавить декоративные элементы (карнизы)
        for i in range(4):
            cornice_y = self.y + i * (self.height / 4)
            ax.plot([self.x - 5, self.x + self.width + 5], 
                   [cornice_y, cornice_y], color='#4169E1', linewidth=2)

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 150)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать барабан (цилиндр под куполом)
        drum = Rectangle((self.x + self.width*0.2, self.y), 
                        self.width*0.6, self.height*0.3, 
                        facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(drum)
        # TODO: отрисовать луковичную главу (овал)
        dome = Ellipse((self.x + self.width/2, self.y + self.height*0.5), 
                      self.width*0.7, self.height*0.6, 
                      facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(dome)
        # TODO: добавить крест на вершине
        cross_x = self.x + self.width/2
        cross_y = self.y + self.height*0.8
        ax.plot([cross_x, cross_x], [cross_y, cross_y + 20], 
               color='#FFD700', linewidth=3)
        ax.plot([cross_x - 10, cross_x + 10], [cross_y + 10, cross_y + 10], 
               color='#FFD700', linewidth=3)

class Spire(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 300)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать шпиль (сужающийся кверху)
        spire_body = Polygon([
            (self.x, self.y),
            (self.x + self.width, self.y),
            (self.x + self.width/2, self.y + self.height)
        ], facecolor=color, edgecolor='#4169E1', linewidth=2)
        ax.add_patch(spire_body)
        # TODO: добавить звезду/флюгер на вершине
        star_x = self.x + self.width/2
        star_y = self.y + self.height
        star = Polygon([
            (star_x, star_y + 20),
            (star_x - 10, star_y + 5),
            (star_x + 5, star_y + 5),
            (star_x, star_y - 10),
            (star_x - 5, star_y + 5),
            (star_x + 10, star_y + 5)
        ], facecolor='#FFD700', edgecolor='#4169E1')
        ax.add_patch(star)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания, купола и шпили должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" петербургской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать купол с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к голубому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления куполов к собору, где купола появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" петербургского собора.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Dome: высота должна быть достаточной для луковичной главы (не менее 60px)

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
        self.dome_level = 0  # уровень куполов
    
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

class Dome(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.cross_height = 20  # высота креста
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 60:  # минимальная высота для купола
            value = 60
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_dome()` — добавляет купол к зданию
- `get_color()` — возвращает цвет в градиенте от белого к голубому

**Для Dome**:
- `add_dome(height)` — увеличивает высоту купола
- `get_color()` — возвращает цвет в градиенте от белого к голубому

**Для Spire**:
- `add_spire(height)` — увеличивает высоту шпиля
- `get_color()` — возвращает цвет в градиенте от белого к голубому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_dome(self):
        # TODO: увеличить dome_level
        self.dome_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к голубому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если dome_level > 0: добавить купола
        pass

class Dome(ArchElement):
    def add_dome(self, height):
        # TODO: увеличить высоту купола
        self.height += height
        self.history.append(self._height)
        return self
    
    def get_color(self):
        # TODO: градиент от белого к голубому по высоте
        ratio = min(1.0, self._height / 150)
        return theme.get_gradient(ratio)

class Spire(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.star_size = 15  # размер звезды
        self.history = [height]
    
    def add_spire(self, height):
        # TODO: увеличить высоту шпиля
        self.height += height
        self.history.append(self._height)
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление куполов к собору

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
                'domes': getattr(elem, 'dome_level', 0),
                'spire': getattr(elem, 'star_size', 0)
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
        ax.set_title(f"Шаг {step}: Собор (Санкт-Петербург)")
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

Создайте композицию из 2 зданий, 3 куполов и 1 шпиля. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые купола
- Шаги 4-5: купола растут, появляется шпиль
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
    Building(100, 150, 120, 80, "B1", 800, 600),
    Building(400, 150, 120, 75, "B2", 800, 600)
]

domes = [
    Dome(120, 230, 60, 60, "D1", 800, 600),
    Dome(160, 230, 60, 65, "D2", 800, 600),
    Dome(200, 230, 60, 70, "D3", 800, 600)
]

spire = Spire(300, 100, 40, 200, "S1", 800, 600)

for b in buildings:
    canvas.add_element(b)
for d in domes:
    canvas.add_element(d)
canvas.add_element(spire)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Купола растут и появляется шпиль
        for elem in domes:
            elem.add_dome(15)
        spire.add_spire(20)
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    print(f"{elem.name}: высота={h}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания, купола и шпиль белого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится голубоватым
- Шаг 3: купола появляются над зданиями
- Шаг 4-5: купола растут, шпиль увеличивается
- Шаг 6: финальная композиция с градиентом от белого к насыщенному голубому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→голубой
- Появлению луковичных куполов
- Увеличению высоты шпиля
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как петербургский собор "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление куполов и шпиля и трансформацию цветовой схемы от белого к голубому. Важно показать разнообразие: здания, купола и шпили должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D`, `Dome3D` и `Spire3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для купола дополнительно хранить `self.dome_path` (история высоты купола)
- Метод `get_color()` возвращает цвет в градиенте от белого к голубому

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
        # TODO: вернуть цвет в градиенте от белого к голубому
        from matplotlib.colors import to_hex, to_rgb
        white = to_rgb('#FFFFFF')
        blue = to_rgb('#87CEEB')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (b - w) * ratio for w, b in zip(white, blue))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Dome3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.dome_path = [height]
        self.dome_path = [height]
    
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 150)
        return super().get_color()


class Spire3D(ArchElement3D):
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с русскими элементами
- `create_dome_mesh(x, y, width, depth, height, color)` — луковичный купол
- `create_spire_mesh(x, y, width, depth, height, color)` — шпиль (пирамида + звезда)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с русскими элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_dome_mesh(x, y, width, depth, height, color):
    """Создает луковичный купол"""
    traces = []
    
    # Барабан (цилиндр)
    drum_height = height * 0.3
    traces.append(create_building_mesh(x, y, width, depth, drum_height, color))
    
    # Луковичная глава (эллипсоид)
    center_x, center_y = x + width/2, y + depth/2
    center_z = drum_height + height * 0.5
    
    n_theta, n_phi = 20, 15
    theta = np.linspace(0, 2*np.pi, n_theta)
    phi = np.linspace(0, np.pi, n_phi)
    
    dome_x, dome_y, dome_z = [], [], []
    for p in phi:
        for t in theta:
            dome_x.append(center_x + width * 0.35 * np.sin(p) * np.cos(t))
            dome_y.append(center_y + depth * 0.35 * np.sin(p) * np.sin(t))
            dome_z.append(center_z + height * 0.35 * np.cos(p))
    
    dome_mesh = go.Mesh3d(
        x=dome_x, y=dome_y, z=dome_z,
        color=color, opacity=0.9,
        alphahull=0
    )
    traces.append(dome_mesh)
    
    # Крест на вершине
    traces.append(go.Scatter3d(
        x=[center_x, center_x], y=[center_y, center_y], 
        z=[center_z + height * 0.35, center_z + height * 0.5],
        mode='lines',
        line=dict(color='#FFD700', width=4)
    ))
    
    return traces


def create_spire_mesh(x, y, width, depth, height, color):
    """Создает шпиль: пирамида + звезда"""
    traces = []
    
    # Пирамидальный шпиль
    base_x, base_y = [], []
    apex_x, apex_y = x + width/2, y + depth/2
    
    for angle in np.linspace(0, 2*np.pi, 5):
        base_x.append(x + width/2 + width/2 * np.cos(angle))
        base_y.append(y + depth/2 + depth/2 * np.sin(angle))
    
    base_z = [0] * 5
    apex_z = [height] * 5
    
    # Грани пирамиды
    for i in range(5):
        next_i = (i + 1) % 5
        traces.append(go.Mesh3d(
            x=[base_x[i], base_x[next_i], apex_x],
            y=[base_y[i], base_y[next_i], apex_y],
            z=[0, 0, height],
            color=color, opacity=0.9,
            alphahull=0
        ))
    
    # Звезда на вершине
    traces.append(go.Scatter3d(
        x=[apex_x], y=[apex_y], z=[height],
        mode='markers',
        marker=dict(size=10, color='#FFD700')
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 6 элементов (2 Building3D + 3 Dome3D + 1 Spire3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: купола и шпиль растут

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 1.0, 60, "B1"),
    Building3D(4.0, 1.0, 1.0, 70, "B2"),
    Dome3D(1.2, 1.0, 0.6, 60, "D1"),
    Dome3D(1.6, 1.0, 0.6, 65, "D2"),
    Dome3D(2.0, 1.0, 0.6, 70, "D3"),
    Spire3D(3.0, 1.0, 0.4, 200, "S1")
]

# Коэффициенты роста
growth_rates = [15, 20, 12, 12, 12, 18]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, (Dome3D, Spire3D)) and step >= 4:
            # Купола и шпиль растут на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            if isinstance(elem, Dome3D):
                elem.dome_path.append(new_h)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все купола с текущей высотой из `dome_path[step]`
- Шпиль с текущей высотой из `path[step]`
- Цветовую динамику от белого к голубому

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
        elif isinstance(elem, Dome3D):
            color = elem.get_color()
            d = elem.dome_path[step] if step < len(elem.dome_path) else elem.dome_path[-1]
            traces.extend(create_dome_mesh(
                elem.x, elem.y, elem.width, 0.4, d, color
            ))
        elif isinstance(elem, Spire3D):
            color = elem.get_color()
            traces.extend(create_spire_mesh(
                elem.x, elem.y, elem.width, 0.4, h, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Собор (Санкт-Петербург)".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора петербургской архитектуры
- Добавлены подписи и заголовок с темой "От белого к голубому"

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
        camera=dict(eye=dict(x=3.0, y=3.0, z=2.5)),
        aspectmode='manual',
        aspectratio=dict(x=1.5, y=0.8, z=1.2),
        bgcolor='#87CEEB'
    ),
    title="⛪ Анимация: Собор (Санкт-Петербург) (градиент от белого к голубому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами, купола и шпиль
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Купола и шпиль начинают расти (шаги 4-7)
  - Цвет всех элементов плавно переходит от белого (`#FFFFFF`) к насыщенному голубому (`#87CEEB`)
  - Луковичные купола увеличиваются в размере
  - Шпиль с золотой звездой становится выше
  - Кресты на куполах видны в золотом цвете
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" петербургского архитектурного ансамбля
