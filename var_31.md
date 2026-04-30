# **Вариант № 31**

## **Генеративная художественная композиция "Музей с колоннами" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей классического искусства, который хочет получить интерактивную генеративную инсталляцию "Музей с колоннами". Инсталляция должна создавать уникальные композиции с музеями, где здания, купола и колоннады оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания, купола и колонны — это "живые" объекты, способные расти, менять цвет от белого к золотому и реагировать на взаимодействие, создавая динамичные композиции в стиле классической музейной архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
dome_count = 2  # количество куполов
column_count = 8  # количество колонн в колоннаде
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к золотому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий, куполов и колонн, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание музея с окнами
- **`Dome`** — купол (полусферическая крыша музея)
- **`Column`** — колонна с капителью (колоннада перед музеем)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания музея
        # TODO: добавить окна (3 ряда по 4 окна)
        # TODO: добавить вход с лестницей
        pass

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать купол на крыше здания
        # TODO: добавить декоративные элементы
        # TODO: использовать градиент от белого к золотому
        pass

class Column(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать колонну с базой и капителью
        # TODO: добавить каннелюры (вертикальные линии)
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
- метод `add_dome_to_building(building)` добавляет купол к зданию
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
    
    def add_dome_to_building(self, building):
        # TODO: создать купол над зданием
        # TODO: позиция: по центру здания, высота = building.y + building.height
        # TODO: добавить купол в elements
        pass
    
    def add_colonnade(self, building, count):
        # TODO: создать колоннаду перед зданием
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

Реализуйте класс `ColorTheme`, использующий **паттерн Borg** для хранения цветовой схемы композиции.

**Требования**:
- все экземпляры класса имеют общий словарь `colors`
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от белого к золотому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'dome', 'column')
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
        # TODO: установить цвета для morning/noon/evening
        # Градиент от белого к золотому усиливается к evening
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

Создайте композицию из 3 зданий музея с 2 куполами и 8 колоннами, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (100, 400), (300, 380), (500, 420)
# - 2 Dome над зданиями
# - 8 Column перед центральным зданием

# Пример создания купола:
# dome = Dome(building.x + building.width*0.2, building.y + building.height, 
#             80, 60, "Dome1")
# canvas.add_element(dome)

# Пример создания колонны:
# column = Column(building.x + offset, building.y, 20, 100, "Column1")
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
from matplotlib.patches import Rectangle, Circle, Polygon, Arc

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада музея
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (3 ряда по 4)
        window_w, window_h = self.width * 0.12, self.height * 0.12
        for row in range(3):
            for col in range(4):
                wx = self.x + self.width * 0.08 + col * (self.width * 0.22)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)
        # TODO: добавить вход с лестницей
        entrance = Rectangle((self.x + self.width/2 - 20, self.y), 
                            40, self.height * 0.3,
                            facecolor='#654321', edgecolor='#8B4513')
        ax.add_patch(entrance)

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 100)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать купол (полусфера)
        dome = Arc((self.x + self.width/2, self.y + self.height*0.3), 
                  self.width, self.height*1.4, 
                  theta1=0, theta2=180, 
                  facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(dome)
        # TODO: добавить декоративные элементы
        decoration = Circle((self.x + self.width/2, self.y + self.height), 
                           8, facecolor='#FFD700', edgecolor='#8B4513')
        ax.add_patch(decoration)

class Column(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 150)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать базу колонны
        base = Rectangle((self.x - 5, self.y), self.width + 10, 15, 
                        facecolor=color, edgecolor='#8B4513', linewidth=1)
        ax.add_patch(base)
        # TODO: отрисовать ствол колонны
        shaft = Rectangle((self.x, self.y + 15), self.width, self.height - 35, 
                         facecolor=color, edgecolor='#8B4513', linewidth=1)
        ax.add_patch(shaft)
        # TODO: добавить каннелюры
        for i in range(5):
            line_x = self.x + self.width * (i + 1) / 6
            ax.plot([line_x, line_x], [self.y + 15, self.y + self.height - 20], 
                   color='#8B4513', linewidth=0.5, alpha=0.5)
        # TODO: отрисовать капитель
        capital = Rectangle((self.x - 8, self.y + self.height - 20), 
                           self.width + 16, 20, 
                           facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(capital)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания, купола и колонны должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" музейной архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать купол с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к золотому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления куполов и колонн к музею, где элементы появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" классического музея.

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
        self.dome_level = 0  # уровень купола
        self.column_level = 0  # уровень колонн
    
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

class Column(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.capital_decor = 0  # уровень декора капители
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < self._width * 4:  # минимальная пропорция
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
- `add_dome()` — добавляет купол к зданию
- `add_columns()` — добавляет колонны к зданию
- `get_color()` — возвращает цвет в градиенте от белого к золотому

**Для Dome**:
- `add_dome(height)` — увеличивает высоту купола
- `get_color()` — возвращает цвет в градиенте от белого к золотому

**Для Column**:
- `add_column()` — увеличивает высоту колонны
- `add_capital()` — добавляет декор капители
- `get_color()` — возвращает цвет в градиенте от белого к золотому

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
    
    def add_columns(self):
        # TODO: увеличить column_level
        self.column_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к золотому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если dome_level > 0: добавить купол
        # TODO: если column_level > 0: добавить колонны
        pass

class Dome(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.history = [height]
    
    def add_dome(self, height):
        # TODO: увеличить высоту купола
        self.height += height
        self.history.append(self._height)
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 100)
        return theme.get_gradient(ratio)

class Column(ArchElement):
    def add_column(self):
        # TODO: увеличить высоту колонны
        self.height += 15
        self.history.append(self._height)
        return self
    
    def add_capital(self):
        # TODO: увеличить уровень декора капители
        self.capital_decor += 1
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
- Анимация должна показывать постепенное добавление куполов и колонн

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
                'dome': getattr(elem, 'dome_level', 0),
                'columns': getattr(elem, 'column_level', 0)
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
                              facecolor='#F5DEB3', edgecolor='none'))
        
        # TODO: для каждого элемента вызвать draw()
        for elem in self.elements:
            elem.draw(ax)
        
        # TODO: добавить заголовок с номером шага
        ax.set_title(f"Шаг {step}: Музей с колоннами")
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

Создайте композицию из 2 зданий, 2 куполов и 8 колонн. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются купола над зданиями
- Шаги 4-5: колонны растут, добавляется декор капители
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
    Dome(120, 230, 80, 60, "D1", 800, 600),
    Dome(420, 225, 80, 65, "D2", 800, 600)
]

columns = []
for i in range(8):
    col_x = 250 + i * 25
    column = Column(col_x, 100, 20, 100, f"C{i}", 800, 600)
    columns.append(column)

for b in buildings:
    canvas.add_element(b)
for d in domes:
    canvas.add_element(d)
for c in columns:
    canvas.add_element(c)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step == 3:
        # Появление куполов
        for elem in domes:
            elem.add_dome(20)
    elif step <= 5:
        # Колонны растут и добавляют декор
        for elem in columns:
            elem.add_column()
            elem.add_capital()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    if isinstance(elem, Building):
        d = getattr(elem, 'dome_level', 0)
        c = getattr(elem, 'column_level', 0)
        print(f"{elem.name}: высота={h}, купола={d}, колонны={c}")
    else:
        print(f"{elem.name}: высота={h}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания, купола и колонны белого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится золотистым
- Шаг 3: купола появляются над зданиями
- Шаг 4-5: колонны растут, добавляется декор капители
- Шаг 6: финальная композиция с градиентом от белого к насыщенному золотому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→золотой
- Появлению куполов над зданиями
- Добавлению декора на капителях колонн
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как классический музей с колоннадой "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление куполов и колонн и трансформацию цветовой схемы от белого к золотому. Важно показать разнообразие: здания, купола и колонны должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D`, `Dome3D` и `Column3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для колонны дополнительно хранить `self.capital_path` (история декора капители)
- Метод `get_color()` возвращает цвет в градиенте от белого к золотому

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
        # TODO: вернуть цвет в градиенте от белого к золотому
        from matplotlib.colors import to_hex, to_rgb
        white = to_rgb('#FFFFFF')
        gold = to_rgb('#FFD700')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (g - w) * ratio for w, g in zip(white, gold))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Dome3D(ArchElement3D):
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 100)
        return super().get_color()


class Column3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.capital_decor = 0
        self.capital_decor = 0
        # TODO: создать self.capital_path = [0]
        self.capital_path = [0]
    
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 150)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с музейными элементами
- `create_dome_mesh(x, y, width, depth, height, color)` — купол (полусфера)
- `create_column_mesh(x, y, width, depth, height, capital_decor, color)` — колонна (цилиндр + капитель)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с музейными элементами"""
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
    """Создает купол: полусфера"""
    traces = []
    
    # Полусфера купола (аппроксимация через сетку)
    n_theta, n_phi = 20, 10
    theta = np.linspace(0, 2*np.pi, n_theta)
    phi = np.linspace(0, np.pi/2, n_phi)
    
    radius_x, radius_y = width/2, depth/2
    center_x, center_y = x + width/2, y + depth/2
    center_z = height * 0.3
    
    X_dome, Y_dome, Z_dome = [], [], []
    
    for p in phi:
        for t in theta:
            X_dome.append(center_x + radius_x * np.sin(p) * np.cos(t))
            Y_dome.append(center_y + radius_y * np.sin(p) * np.sin(t))
            Z_dome.append(center_z + radius_x * np.cos(p))
    
    dome_surface = go.Mesh3d(
        x=X_dome, y=Y_dome, z=Z_dome,
        color=color, opacity=0.9,
        alphahull=0
    )
    traces.append(dome_surface)
    
    # Декоративный элемент на вершине
    traces.append(go.Scatter3d(
        x=[center_x], y=[center_y], z=[center_z + radius_x],
        mode='markers',
        marker=dict(size=5, color='#FFD700')
    ))
    
    return traces


def create_column_mesh(x, y, width, depth, height, capital_decor, color):
    """Создает колонну: цилиндр + капитель"""
    traces = []
    
    # Ствол колонны (цилиндр)
    n_segments = 16
    base_x, base_y = [], []
    top_x, top_y = [], []
    
    center_x, center_y = x + width/2, y + depth/2
    radius = width/2
    shaft_height = height - 35
    
    for angle in np.linspace(0, 2*np.pi, n_segments):
        base_x.append(center_x + radius * np.cos(angle))
        base_y.append(center_y + radius * np.sin(angle))
        top_x.append(center_x + radius * np.cos(angle))
        top_y.append(center_y + radius * np.sin(angle))
    
    body_z = [15]*n_segments + [shaft_height]*n_segments
    shaft_mesh = go.Mesh3d(
        x=base_x + top_x,
        y=base_y + top_y,
        z=body_z,
        color=color, opacity=0.95,
        alphahull=0
    )
    traces.append(shaft_mesh)
    
    # Капитель (декоративный элемент сверху)
    capital_h = 20
    traces.append(create_building_mesh(x - 8, y, width + 16, depth, capital_h, color))
    
    # Каннелюры (вертикальные линии)
    for i in range(8):
        angle = 2 * np.pi * i / 8
        groove_x = [center_x + radius * 0.95 * np.cos(angle)] * 2
        groove_y = [center_y + radius * 0.95 * np.sin(angle)] * 2
        groove_z = [15, shaft_height]
        traces.append(go.Scatter3d(
            x=groove_x, y=groove_y, z=groove_z,
            mode='lines', line=dict(color='#8B4513', width=1, opacity=0.5)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 12 элементов (2 Building3D + 2 Dome3D + 8 Column3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-5**: купола увеличиваются
- **Шаги 6-7**: колонны растут и получают декор капители

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 1.0, 60, "B1"),
    Building3D(4.0, 1.0, 1.0, 70, "B2"),
    Dome3D(1.2, 1.0, 0.8, 60, "D1"),
    Dome3D(4.2, 1.0, 0.8, 65, "D2")
]

# Добавляем 8 колонн
for i in range(8):
    col_x = 2.0 + i * 0.25
    elements.append(Column3D(col_x, 1.0, 0.2, 100, f"C{i}"))

# Коэффициенты роста
growth_rates = [15, 20, 12, 12] + [18] * 8

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Building3D) and step <= 3:
            # Здания растут на шагах 1-3
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Dome3D) and step >= 4 and step <= 5:
            # Купола растут на шагах 4-5
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Column3D) and step >= 6:
            # Колонны растут и добавляют декор на шагах 6-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.capital_decor += 1
            elem.capital_path.append(elem.capital_decor)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (мраморный цвет)
- Все здания с текущей высотой из `path[step]`
- Все купола с текущей высотой из `path[step]`
- Все колонны с текущим декором капители из `capital_path[step]`
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
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Dome3D):
            color = elem.get_color()
            traces.extend(create_dome_mesh(
                elem.x, elem.y, elem.width, 0.4, h, color
            ))
        elif isinstance(elem, Column3D):
            color = elem.get_color()
            c = elem.capital_path[step] if step < len(elem.capital_path) else elem.capital_path[-1]
            traces.extend(create_column_mesh(
                elem.x, elem.y, elem.width, 0.4, h, c, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Музей с колоннами".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора классической архитектуры
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
        camera=dict(eye=dict(x=3.0, y=3.0, z=2.5)),
        aspectmode='manual',
        aspectratio=dict(x=1.5, y=0.8, z=1.2),
        bgcolor='#87CEEB'
    ),
    title="🏛️ Анимация: Музей с колоннами (градиент от белого к золотому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами, купола и 8 колонн без декора
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Купола увеличиваются (шаги 4-5)
  - Колонны начинают расти и добавлять декор капители (шаги 6-7)
  - Цвет всех элементов плавно переходит от белого (`#FFFFFF`) к насыщенному золотому (`#FFD700`)
  - Декор капители увеличивается с 0 до 2-3 уровней
  - Каннелюры на колоннах становятся более заметными
  - Золотой декоративный элемент на вершине купола виден
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" классического музейного ансамбля
