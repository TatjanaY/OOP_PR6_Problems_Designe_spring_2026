# **Вариант № 1**

## **Генеративная художественная композиция "Арки над городом" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — галерея урбанистического искусства, которая хочет получить интерактивную генеративную инсталляцию "Арки над городом". Инсталляция должна создавать уникальные городские пейзажи с арочными переходами между зданиями, где архитектура оживает и трансформируется в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и арки — это "живые" объекты, способные соединяться, менять цвет от белого к красному и реагировать на взаимодействие, создавая динамичные архитектурные композиции в стиле неоклассики.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 6
arch_probability = 0.4  # вероятность появления арки между зданиями
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к красному. Это позволяет плавно менять настроение инсталляции (утро/полдень/закат) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и арок, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов (здания, башни, арки) могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами (3 ряда по 3 окна)
- **`Tower`** — здание-башня с треугольной крышей и шпилем
- **`Arch`** — арка (соединительный элемент между зданиями)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания (patches.Rectangle)
        # TODO: добавить окна (вложенные прямоугольники)
        pass

class Tower(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник основания
        # TODO: отрисовать треугольную крышу (patches.Polygon)
        # TODO: добавить шпиль (линия от вершины крыши)
        pass

class Arch(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать арку как полукруг или дугу (patches.Arc или Polygon)
        # TODO: добавить опоры арки по бокам
        # TODO: использовать градиент от белого к красному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `connect_with_arch(elem1, elem2)` создаёт арку между двумя элементами
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
    
    def connect_with_arch(self, elem1, elem2):
        # TODO: вычислить позицию арки между elem1 и elem2
        # TODO: создать объект Arch и добавить его в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'sunset') с градиентом от белого к красному
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'arch', 'accent')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от белого к красному (ratio от 0 до 1)

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
                'base_red': '#DC143C'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/sunset
        # Градиент от белого к красному усиливается к sunset
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от белого к красному
        # ratio: 0.0 = белый, 1.0 = красный
        from matplotlib.colors import to_rgb, to_hex
        white = to_rgb('#FFFFFF')
        red = to_rgb('#DC143C')
        interpolated = tuple(w + (r - w) * ratio for w, r in zip(white, red))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 6 элементов (здания, башни и арки между ними), используя созданные классы. Отобразите результат.

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
# - Арки между соседними элементами с вероятностью 40%

# Пример создания арки:
# arch = Arch(x1 + width1/2, y_min, width_gap, height_arch, "Arch1")
# canvas.add_element(arch)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Polygon, Arc

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='black', linewidth=1)
        ax.add_patch(rect)
        # TODO: в цикле создать Rectangle для окон (3x3)
        window_w, window_h = self.width * 0.2, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.15 + col * (self.width * 0.25)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='black')
                ax.add_patch(window)

class Arch(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты или позиции)
        ratio = min(1.0, self.height / 200)  # нормализация
        color = theme.get_gradient(ratio)
        # TODO: отрисовать дугу арки
        arc = Arc((self.x + self.width/2, self.y + self.height/2), 
                 self.width, self.height, 
                 theta1=0, theta2=180, color=color, linewidth=3)
        ax.add_patch(arc)
        # TODO: отрисовать опоры арки
        support_w = self.width * 0.15
        left_support = Rectangle((self.x, self.y), support_w, self.height/2, 
                                facecolor=color, edgecolor='black')
        right_support = Rectangle((self.x + self.width - support_w, self.y), 
                                 support_w, self.height/2,
                                 facecolor=color, edgecolor='black')
        ax.add_patch(left_support)
        ax.add_patch(right_support)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и арки должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать арку с отрицательной шириной или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к красному в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию появления арок между зданиями, где соединения возникают постепенно, а цветовая динамика подчёркивает процесс "оживления" города.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Arch: ширина должна быть достаточной для соединения двух элементов

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
        self.decor_level = 0  # уровень декоративных элементов
    
    @property
    def x(self):
        return self._x
    
    @x.setter
    def x(self, value):
        # TODO: проверить тип (int или float)
        if not isinstance(value, (int, float)):
            raise ValueError("x must be numeric")
        # TODO: проверить границы 0..canvas_width
        if not (0 <= value <= self.canvas_width):
            raise ValueError(f"x must be in [0, {self.canvas_width}]")
        # TODO: установить _x
        self._x = value
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        # TODO: проверить тип
        if not isinstance(value, (int, float)):
            raise ValueError("height must be numeric")
        # TODO: проверить value > 0
        if value <= 0:
            raise ValueError("height must be positive")
        # TODO: сохранить предыдущее значение в history
        self.history.append(self._height)
        # TODO: установить _height
        self._height = value
        # TODO: обновить self.floors = _height // 20
        self.floors = int(self._height // 20)

class Arch(ArchElement):
    @property
    def width(self):
        return self._width
    
    @width.setter
    def width(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("width must be positive numeric")
        self.history.append(self._width)
        self._width = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px (для заметности изменений)
- `add_decor()` — добавляет декоративные элементы (карнизы, орнамент)
- `get_color()` — возвращает цвет в градиенте от белого к красному в зависимости от высоты

**Для Tower**:
- `add_spire(height)` — добавляет/увеличивает шпиль
- `add_decor()` — добавляет декор на башню

**Для Arch**:
- `add_decor()` — добавляет узоры на арку
- `get_color()` — возвращает цвет в градиенте от белого к красному

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_decor(self):
        # TODO: увеличить decor_level
        self.decor_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к красному:
        #   ratio = min(1.0, self.height / 300)
        #   использовать theme.get_gradient(ratio)
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        color = self.get_color()
        # TODO: отрисовать прямоугольник здания
        # TODO: если есть этажи (floors > 0):
        #   - рассчитать размер окна
        #   - для каждого этажа: отрисовать 2 окна
        #   - цвет окон: чередование жёлтого/оранжевого
        # TODO: если decor_level > 0: добавить карниз сверху
        pass

class Tower(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.spire_height = 0
        self.decor_level = 0
        self.history = [height]
    
    def add_spire(self, height):
        # TODO: увеличить _spire_height
        self.spire_height += height
        self.history.append(self.spire_height)
        return self
    
    def add_decor(self):
        self.decor_level += 1
        return self
    
    def get_color(self):
        ratio = min(1.0, (self._height + self.spire_height) / 400)
        return theme.get_gradient(ratio)

class Arch(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.decor_level = 0
        self.history = [height]
    
    def add_decor(self):
        # TODO: увеличить уровень декоративных узоров
        self.decor_level += 1
        return self
    
    def get_color(self):
        # TODO: градиент от белого к красному по высоте
        ratio = min(1.0, self._height / 250)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие высоты всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное появление арок между зданиями

### **Шаблон кода**

```python
class AnimatedCity:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.elements = []      # список элементов
        self.frames = []         # список кадров (параметры на каждом шаге)
    
    def add_element(self, element):
        self.elements.append(element)
    
    def record_frame(self):
        # TODO: сохранить текущие параметры всех элементов в self.frames
        frame = {
            elem.name: {
                'height': elem._height if hasattr(elem, '_height') else elem.height,
                'decor': getattr(elem, 'decor_level', 0),
                'spire': getattr(elem, 'spire_height', 0)
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
        
        # TODO: для каждого элемента:
        #   - временно установить параметры из frame_data
        #   - вызвать draw()
        for elem in self.elements:
            elem.draw(ax)
        
        # TODO: добавить заголовок с номером шага
        ax.set_title(f"Шаг {step}: Арки над городом")
        ax.axis('off')
        
        # TODO: показать фигуру и пауза
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

Создайте композицию из 4 зданий/башен с разными начальными параметрами. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются первые арки между соседними элементами
- Шаги 4-5: арки получают декор, здания растут дальше
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
    Building(50, 50, 70, 80, "B1", 800, 600),
    Tower(180, 50, 60, 100, "T1", 800, 600),
    Building(300, 50, 75, 70, "B2", 800, 600),
    Tower(430, 50, 65, 110, "T2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)

# Сохраняем начальное состояние
canvas.record_frame()

# Анимация трансформаций (6 шагов)
for step in range(1, 7):
    if step <= 2:
        # Здания добавляют этажи
        for i, elem in enumerate(buildings):
            if isinstance(elem, Building):
                elem.add_floors(1)
            elif isinstance(elem, Tower):
                elem.add_spire(15)
    elif step == 3:
        # Появление арок между соседними элементами
        for i in range(len(buildings) - 1):
            e1, e2 = buildings[i], buildings[i+1]
            arch_x = e1.x + e1.width
            arch_w = e2.x - arch_x
            if arch_w > 30:  # достаточно места для арки
                arch = Arch(arch_x, 100, arch_w, 80, f"Arch{i}", 800, 600)
                canvas.add_element(arch)
    elif step <= 5:
        # Декор на арках и рост зданий
        for elem in canvas.elements:
            if isinstance(elem, Arch):
                elem.add_decor()
            elif step == 4:
                elem.add_floors(1) if hasattr(elem, 'add_floors') else None
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    d = getattr(elem, 'decor_level', 0)
    s = getattr(elem, 'spire_height', 0)
    print(f"{elem.name}: высота={h}, декор={d}, шпиль={s}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и башни белого цвета
- Шаг 1-2: здания растут, цвет постепенно краснеет
- Шаг 3: между зданиями появляются арки (полупрозрачные, светло-красные)
- Шаг 4-5: арки украшаются узорами, цвет усиливается до насыщенного красного
- Шаг 6: финальная композиция с градиентом от белого к красному

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту
- Появлению новых архитектурных элементов (арок)
- Добавлению декоративных деталей
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Галерея хочет установить инсталляцию, где посетители смогут наблюдать, как город с арками "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление арок и трансформацию цветовой схемы от белого к красному. Важно показать разнообразие: обычные здания, башни со шпилями и соединительные арки должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D`, `Tower3D` и `Arch3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для башни дополнительно хранить историю высоты шпиля в `self.spire_path`
- Для арки хранить `self.appeared` (появилась ли она к данному шагу)
- Метод `get_color()` возвращает цвет в градиенте от белого к красному

### **Шаблон кода**

```python
class ArchElement3D:
    def __init__(self, x, y, width, height, name):
        self.x = x
        self.y = y
        self.width = width
        self._height = height
        self.name = name
        self.path = [height]  # история высот
        self.color_ratio = 0  # для градиента
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к красному
        from matplotlib.colors import to_hex, to_rgb
        white = to_rgb('#FFFFFF')
        red = to_rgb('#DC143C')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(w + (r - w) * ratio for w, r in zip(white, red))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Tower3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        self.spire_height = height * 0.3
        self.spire_path = [self.spire_height]
    
    def get_color(self):
        total_h = self.path[-1] + self.spire_path[-1]
        self.color_ratio = min(1.0, total_h / 400)
        return super().get_color()
    
    def get_spire_color(self):
        # Шпиль всегда чуть темнее
        base = self.get_color()
        return base  # или можно сделать золотистый оттенок


class Arch3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        self.appeared = False  # появляется на определённом шаге
        self.decor_level = 0
    
    def get_color(self):
        if not self.appeared:
            return '#FFFFFF00'  # прозрачный, если не появилась
        self.color_ratio = min(1.0, self.path[-1] / 250)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с окнами
- `create_tower_mesh(x, y, width, depth, height, spire_height, base_color, spire_color)` — башня с пирамидальной крышей
- `create_arch_mesh(x, y, width, depth, height, color, decor_level)` — арка с опорами

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с окнами"""
    # 8 вершин параллелепипеда
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    # 12 треугольников (6 граней по 2 треугольника)
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7,0,0,1,1,2,2,3,3]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4,1,1,2,2,3,3,0,0,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1,5,4,6,5,7,6,4,7,0,3]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.9)
    return mesh


def create_tower_mesh(x, y, width, depth, height, spire_height, base_color, spire_color):
    """Создает башню: основание + пирамидальная крыша + шпиль"""
    traces = []
    
    # Основание
    traces.append(create_building_mesh(x, y, width, depth, height, base_color))
    
    # Пирамидальная крыша (5 вершин: 4 угла основания + вершина)
    roof_base_z = height
    apex_x, apex_y = x + width/2, y + depth/2
    apex_z = roof_base_z + spire_height * 0.7
    
    roof_x = [x, x+width, x+width, x, apex_x]
    roof_y = [y, y, y+depth, y+depth, apex_y]
    roof_z = [roof_base_z]*4 + [apex_z]
    
    # 4 треугольника крыши
    i_roof = [0,1,2,3]
    j_roof = [1,2,3,0]
    k_roof = [4,4,4,4]
    
    roof_mesh = go.Mesh3d(x=roof_x, y=roof_y, z=roof_z,
                         i=i_roof, j=j_roof, k=k_roof,
                         color=spire_color, opacity=0.95)
    traces.append(roof_mesh)
    
    # Шпиль (тонкая линия/цилиндр)
    spire_top_z = apex_z + spire_height * 0.3
    traces.append(go.Scatter3d(
        x=[apex_x, apex_x], y=[apex_y, apex_y], z=[apex_z, spire_top_z],
        mode='lines', line=dict(color=spire_color, width=4)
    ))
    
    return traces


def create_arch_mesh(x, y, width, depth, height, color, decor_level):
    """Создает арку: две опоры + полукруглая дуга"""
    traces = []
    
    # Левая опора
    support_w = width * 0.15
    traces.append(create_building_mesh(x, y, support_w, depth, height, color))
    
    # Правая опора
    traces.append(create_building_mesh(x + width - support_w, y, support_w, depth, height, color))
    
    # Дуга арки (аппроксимация полукруга через полигон)
    n_segments = 20
    arc_x, arc_z = [], []
    center_x, center_z = x + width/2, height
    radius = width/2 - support_w
    
    for angle in np.linspace(0, np.pi, n_segments):
        arc_x.append(center_x + radius * np.cos(angle))
        arc_z.append(center_z + radius * np.sin(angle))
    
    # Создаём "трубу" дуги через несколько параллельных линий
    for dz in [0, depth/2, depth]:
        traces.append(go.Scatter3d(
            x=arc_x, y=[y+dz]*len(arc_x), z=arc_z,
            mode='lines', line=dict(color=color, width=5 + decor_level*2)
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 5 элементов (2 Building3D + 2 Tower3D + 1 Arch3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания и башни растут
- **Шаг 4**: появляется арка между центральными элементами
- **Шаги 5-7**: все элементы получают декор, цвет усиливается по градиенту

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Tower3D(2.5, 1.0, 0.9, 70, "T1"),
    Building3D(4.0, 1.0, 0.7, 50, "B2"),
    Tower3D(5.8, 1.0, 0.8, 80, "T2"),
    Arch3D(3.2, 1.0, 1.5, 60, "Arch1")  # появится позже
]

# Арка появляется на шаге 4
elements[4].appeared = False

# Коэффициенты роста
growth_rates = [15, 20, 12, 18, 10]

# Инициализация path
for elem in elements:
    elem.path = [elem._height]
    if isinstance(elem, Tower3D):
        elem.spire_path = [elem.spire_height]

# Основной цикл трансформаций (7 шагов)
for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Arch3D) and step < 4:
            continue  # арка ещё не появилась
        
        # Рост элементов
        if isinstance(elem, Arch3D) and step == 4:
            elem.appeared = True
            elem.path.append(elem._height)
        elif isinstance(elem, Building3D):
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        elif isinstance(elem, Tower3D):
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            # Шпиль растёт пропорционально
            elem.spire_height += growth_rates[i] * 0.4
            elem.spire_path.append(elem.spire_height)
        elif isinstance(elem, Arch3D) and step >= 4:
            elem.path.append(elem._height + 5)  # лёгкий рост арки
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (зелёный цвет)
- Все здания с текущей высотой из `path[step]`
- Арку, которая появляется только с шага 4
- Цветовую динамику от белого к красному

### **Шаблон кода**

```python
frames = []
max_steps = len(elements[0].path)

# Координаты земли
x_ground = np.arange(0, 8, 0.3)
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
        # Пропускаем арку, если она ещё не появилась
        if isinstance(elem, Arch3D) and not elem.appeared:
            continue
        
        # Получаем текущую высоту
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, Building3D):
            color = elem.get_color()
            traces.append(create_building_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Tower3D):
            base_c = elem.get_color()
            spire_c = elem.get_spire_color()
            sh = elem.spire_path[step] if step < len(elem.spire_path) else elem.spire_path[-1]
            traces.extend(create_tower_mesh(
                elem.x, elem.y, elem.width, 0.5, h, sh, base_c, spire_c
            ))
        elif isinstance(elem, Arch3D):
            color = elem.get_color()
            decor = min(3, step - 3) if step >= 4 else 0
            traces.extend(create_arch_mesh(
                elem.x, elem.y, elem.width, 0.4, h, color, decor
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Арки над городом".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора архитектурной композиции
- Добавлены подписи и заголовок с темой "От белого к красному"

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
        camera=dict(eye=dict(x=2.0, y=2.0, z=1.5)),
        aspectmode='manual',
        aspectratio=dict(x=1.5, y=0.6, z=1.0),
        bgcolor='#87CEEB'  # цвет неба
    ),
    title="🏛️ Анимация: Арки над городом (градиент от белого к красному)",
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
  - Здания и башни начинают расти
  - На шаге 4 появляется арка между центральными элементами
  - Цвет всех элементов плавно переходит от белого к насыщенному красному
  - Арка украшается декоративными узорами
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" архитектурного ансамбля