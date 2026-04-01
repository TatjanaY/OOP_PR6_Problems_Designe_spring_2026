# **Вариант № 18**

## **Генеративная художественная композиция "Акведук" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей римской истории, который хочет получить интерактивную генеративную инсталляцию "Акведук". Инсталляция должна создавать уникальные композиции с акведуками, где здания, арки и мосты оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания, арки и мосты — это "живые" объекты, способные расти, менять цвет от серого к красному и реагировать на взаимодействие, создавая динамичные композиции в стиле римской архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
aqueduct_count = 2  # количество участков акведука
arch_count = 3  # количество арок в акведуке
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от серого к красному. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий, арок и акведуков, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в римском стиле
- **`Arch`** — арка (полукруглый соединительный элемент)
- **`Aqueduct`** — акведук (серия арок с водным каналом)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в римском стиле
        # TODO: добавить арочные окна (2 ряда по 3 окна)
        # TODO: добавить колонны по фасаду
        pass

class Arch(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать арку (полукруг)
        # TODO: добавить опоры по бокам
        # TODO: использовать градиент от серого к красному
        pass

class Aqueduct(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать серию арок (3-5 арок)
        # TODO: добавить водный канал сверху
        # TODO: добавить опоры между арками
        # TODO: использовать градиент от серого к красному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_aqueduct_between(elem1, elem2)` создаёт акведук между двумя элементами
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
    
    def add_aqueduct_between(self, elem1, elem2):
        # TODO: создать акведук между elem1 и elem2
        # TODO: позиция: от elem1.x + elem1.width до elem2.x
        # TODO: добавить акведук в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от серого к красному
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'aqueduct', 'water')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #808080 к #DC143C (ratio от 0 до 1)

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
                'base_red': '#DC143C',
                'water': '#4682B4'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от серого к красному усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от серого к красному
        # ratio: 0.0 = серый, 1.0 = красный
        from matplotlib.colors import to_rgb, to_hex
        gray = to_rgb('#808080')
        red = to_rgb('#DC143C')
        interpolated = tuple(g + (r - g) * ratio for g, r in zip(gray, red))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 2 акведуками между ними, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Aqueduct между зданиями

# Пример создания акведука:
# aqueduct = Aqueduct(elem1.x + elem1.width, elem1.y, 
#                     elem2.x - (elem1.x + elem1.width), 100, "Aqueduct1")
# canvas.add_element(aqueduct)

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
        # TODO: создать Rectangle для фасада в римском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить арочные окна (2 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)
                # Арочное завершение
                arc = Arc((wx + window_w/2, wy + window_h), 
                         window_w, window_h*0.5, 
                         theta1=0, theta2=180, color='#8B4513', linewidth=1)
                ax.add_patch(arc)
        # TODO: добавить колонны по фасаду
        for i in range(4):
            col_x = self.x + i * (self.width / 4)
            col = Rectangle((col_x - 5, self.y), 10, self.height, 
                           facecolor='#F5F5DC', edgecolor='#8B4513')
            ax.add_patch(col)

class Arch(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 200)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать арку (полукруг)
        center_x = self.x + self.width / 2
        center_y = self.y + self.height / 2
        arch = Arc((center_x, center_y), 
                  self.width, self.height, 
                  theta1=0, theta2=180, 
                  facecolor=color, edgecolor='#8B4513', linewidth=3)
        ax.add_patch(arch)
        # TODO: добавить опоры по бокам
        support_w = self.width * 0.15
        left_support = Rectangle((self.x, self.y), support_w, self.height/2, 
                                facecolor=color, edgecolor='#8B4513')
        right_support = Rectangle((self.x + self.width - support_w, self.y), 
                                 support_w, self.height/2,
                                 facecolor=color, edgecolor='#8B4513')
        ax.add_patch(left_support)
        ax.add_patch(right_support)

class Aqueduct(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 250)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать серию арок (3-5 арок)
        num_arches = 4
        arch_width = self.width / num_arches
        for i in range(num_arches):
            arch_x = self.x + i * arch_width
            arch = Arc((arch_x + arch_width/2, self.y + self.height/2), 
                      arch_width, self.height * 0.8, 
                      theta1=0, theta2=180, 
                      facecolor=color, edgecolor='#8B4513', linewidth=2)
            ax.add_patch(arch)
        # TODO: добавить водный канал сверху
        water_channel = Rectangle((self.x, self.y + self.height * 0.8), 
                                 self.width, self.height * 0.2,
                                 facecolor='#4682B4', edgecolor='#8B4513', linewidth=2)
        ax.add_patch(water_channel)
        # TODO: добавить опоры между арками
        for i in range(num_arches + 1):
            pillar_x = self.x + i * arch_width - 10
            pillar = Rectangle((pillar_x, self.y), 20, self.height * 0.8, 
                              facecolor=color, edgecolor='#8B4513')
            ax.add_patch(pillar)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания, арки и акведуки должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" римской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать акведук с отрицательной длиной или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от серого к красному в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления арок к акведуку, где арки появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" римского акведука.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Aqueduct: длина должна быть достаточной для арок (не менее 100px)

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
        self.aqueduct_level = 0  # уровень акведука
    
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

class Aqueduct(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.num_arches = 4  # количество арок
        self.water_level = 0  # уровень воды
        self.history = [height]
    
    @property
    def width(self):
        return self._width
    
    @width.setter
    def width(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("width must be positive")
        if value < 100:  # минимальная длина для арок
            value = 100
        self.history.append(self._width)
        self._width = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_aqueduct_decor()` — добавляет римский декор
- `get_color()` — возвращает цвет в градиенте от серого к красному

**Для Arch**:
- `add_arch()` — добавляет арку
- `get_color()` — возвращает цвет в градиенте от серого к красному

**Для Aqueduct**:
- `add_arch(n)` — добавляет n арок к акведуку
- `add_water()` — добавляет воду в канал
- `get_color()` — возвращает цвет в градиенте от серого к красному

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_aqueduct_decor(self):
        # TODO: увеличить aqueduct_level
        self.aqueduct_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от серого к красному
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с арочными окнами
        # TODO: если aqueduct_level > 0: добавить римский декор
        pass

class Arch(ArchElement):
    def add_arch(self):
        # TODO: увеличить размер арки
        self.height += 10
        self.history.append(self._height)
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 200)
        return theme.get_gradient(ratio)

class Aqueduct(ArchElement):
    def add_arch(self, n):
        # TODO: увеличить количество арок на n
        self.num_arches += n
        # TODO: увеличить длину пропорционально
        self.width += 30 * n
        # TODO: добавить запись в history
        self.history.append(self._width)
        # TODO: вернуть self
        return self
    
    def add_water(self):
        # TODO: увеличить уровень воды
        self.water_level += 1
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
- Анимация должна показывать постепенное добавление арок к акведуку

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
                'arches': getattr(elem, 'num_arches', 0),
                'water': getattr(elem, 'water_level', 0)
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
        ax.set_title(f"Шаг {step}: Акведук")
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

Создайте композицию из 2 зданий и 2 акведуков. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые арки акведука
- Шаги 4-5: акведуки растут, добавляются арки
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

aqueducts = [
    Aqueduct(150, 150, 150, 100, "A1", 800, 600),
    Aqueduct(400, 150, 150, 100, "A2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for a in aqueducts:
    canvas.add_element(a)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for elem in buildings:
            elem.add_floors(1)
    elif step <= 5:
        # Акведуки добавляют арки
        for elem in aqueducts:
            elem.add_arch(1)
            elem.add_water()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    w = getattr(elem, '_width', getattr(elem, 'width', 'N/A'))
    a = getattr(elem, 'num_arches', 0)
    print(f"{elem.name}: высота={h}, длина={w}, арки={a}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и акведуки серого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится красноватым
- Шаг 3: акведуки получают первые арки (с 4 до 5)
- Шаг 4-5: акведуки продолжают расти, добавляются новые арки (до 6-7)
- Шаг 6: финальная композиция с градиентом от серого к насыщенному красному

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту серый→красный
- Появлению новых арок на акведуках
- Заполнению водного канала (уровень воды)
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как римский акведук "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление арок и трансформацию цветовой схемы от серого к красному. Важно показать разнообразие: здания, арки и акведуки должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D`, `Arch3D` и `Aqueduct3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для акведука дополнительно хранить `self.arch_path` (история количества арок)
- Метод `get_color()` возвращает цвет в градиенте от серого к красному

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
        # TODO: вернуть цвет в градиенте от серого к красному
        from matplotlib.colors import to_hex, to_rgb
        gray = to_rgb('#808080')
        red = to_rgb('#DC143C')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(g + (r - g) * ratio for g, r in zip(gray, red))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Aqueduct3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.num_arches = 4
        self.num_arches = 4
        # TODO: создать self.arch_path = [4]
        self.arch_path = [4]
        self.water_level = 0
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 250)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с римскими элементами
- `create_arch_mesh(x, y, width, depth, height, color)` — арка (полукруг)
- `create_aqueduct_mesh(x, y, width, depth, height, num_arches, water_level, color)` — акведук (серия арок + водный канал)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с римскими элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_arch_mesh(x, y, width, depth, height, color):
    """Создает арку: полукруг с опорами"""
    traces = []
    
    # Опоры арки
    support_w = width * 0.15
    traces.append(create_building_mesh(x, y, support_w, depth, height/2, color))
    traces.append(create_building_mesh(x + width - support_w, y, support_w, depth, height/2, color))
    
    # Дуга арки (аппроксимация через линии)
    n_segments = 20
    center_x, center_z = x + width/2, height/2
    radius = width/2 - support_w
    
    arch_x, arch_z = [], []
    for angle in np.linspace(0, np.pi, n_segments):
        arch_x.append(center_x + radius * np.cos(angle))
        arch_z.append(center_z + radius * np.sin(angle))
    
    for dz in [0, depth/2, depth]:
        traces.append(go.Scatter3d(
            x=arch_x, y=[y+dz]*len(arch_x), z=arch_z,
            mode='lines', line=dict(color=color, width=5)
        ))
    
    return traces


def create_aqueduct_mesh(x, y, width, depth, height, num_arches, water_level, color):
    """Создает акведук: серия арок + водный канал"""
    traces = []
    
    arch_width = width / num_arches
    
    # Серия арок
    for i in range(num_arches):
        arch_x = x + i * arch_width
        traces.extend(create_arch_mesh(arch_x, y, arch_width, depth, height * 0.8, color))
    
    # Опоры между арками
    for i in range(num_arches + 1):
        pillar_x = x + i * arch_width - 10
        traces.append(create_building_mesh(pillar_x, y, 20, depth, height * 0.8, color))
    
    # Водный канал сверху
    water_height = height * 0.2
    water_z = height * 0.8 + water_level * 5
    
    water_mesh = go.Mesh3d(
        x=[x, x+width, x+width, x, x, x+width, x+width, x],
        y=[y, y, y+depth, y+depth, y, y, y+depth, y+depth],
        z=[water_z]*4 + [water_z + water_height]*4,
        color='#4682B4', opacity=0.7
    )
    traces.append(water_mesh)
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Aqueduct3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: акведуки добавляют арки и воду

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Aqueduct3D(2.0, 1.0, 1.5, 100, "A1"),
    Aqueduct3D(4.0, 1.0, 1.5, 100, "A2")
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
        elif isinstance(elem, Aqueduct3D) and step >= 4:
            # Акведуки добавляют арки на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.num_arches += 1
            elem.arch_path.append(elem.num_arches)
            elem.water_level += 1
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (коричневый цвет)
- Все здания с текущей высотой из `path[step]`
- Все акведуки с текущим количеством арок из `arch_path[step]` и уровнем воды
- Цветовую динамику от серого к красному

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
        elif isinstance(elem, Aqueduct3D):
            color = elem.get_color()
            a = elem.arch_path[step] if step < len(elem.arch_path) else elem.arch_path[-1]
            traces.extend(create_aqueduct_mesh(
                elem.x, elem.y, elem.width, 0.4, h, a, elem.water_level, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Акведук".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора римской архитектуры
- Добавлены подписи и заголовок с темой "От серого к красному"

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
    title="🏛️ Анимация: Акведук (градиент от серого к красному)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания серого цвета с минимальными высотами и акведуки с 4 арками
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Акведуки начинают добавлять арки (шаги 4-7)
  - Цвет всех элементов плавно переходит от серого (`#808080`) к насыщенному красному (`#DC143C`)
  - Количество арок увеличивается с 4 до 8-9
  - Уровень воды в канале поднимается
  - Водный канал светится синим цветом
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" римского архитектурного ансамбля
