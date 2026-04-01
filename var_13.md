# **Вариант № 13**

## **Генеративная художественная композиция "Ступенчатая пирамида" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей древней архитектуры, который хочет получить интерактивную генеративную инсталляцию "Ступенчатая пирамида". Инсталляция должна создавать уникальные композиции со ступенчатыми пирамидами, где здания и пирамидальные структуры оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания и пирамиды — это "живые" объекты, способные расти, менять цвет от жёлтого к коричневому и реагировать на взаимодействие, создавая динамичные композиции в стиле древних ступенчатых пирамид.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
pyramid_count = 2  # количество пирамид
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от жёлтого к коричневому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий и ступенчатых пирамид, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в стиле древней архитектуры
- **`Pyramid`** — ступенчатая пирамида с уменьшающимися ярусами

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в древнем стиле
        # TODO: добавить окна (2 ряда по 3 окна)
        # TODO: добавить декоративные элементы
        pass

class Pyramid(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать ступенчатую пирамиду (5 уменьшающихся ступеней)
        # TODO: добавить лестницу на одной стороне
        # TODO: добавить вершину пирамиды
        # TODO: использовать градиент от жёлтого к коричневому
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `add_pyramid_near(building)` создаёт пирамиду рядом со зданием
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
    
    def add_pyramid_near(self, building):
        # TODO: создать пирамиду рядом со зданием
        # TODO: позиция: building.x + building.width + 50, building.y
        # TODO: добавить пирамиду в elements
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
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от жёлтого к коричневому
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'pyramid', 'sand')
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #FFD700 к #8B4513 (ratio от 0 до 1)

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
                'base_brown': '#8B4513'
            }
    
    def set_theme(self, theme_name):
        # TODO: установить цвета для morning/noon/evening
        # Градиент от жёлтого к коричневому усиливается к evening
        pass
    
    def get(self, name):
        # TODO: вернуть цвет по имени
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        # TODO: интерполировать цвет от жёлтого к коричневому
        # ratio: 0.0 = жёлтый, 1.0 = коричневый
        from matplotlib.colors import to_rgb, to_hex
        yellow = to_rgb('#FFD700')
        brown = to_rgb('#8B4513')
        interpolated = tuple(y + (b - y) * ratio for y, b in zip(yellow, brown))
        return to_hex(interpolated)
    
    def update(self, name, color):
        # TODO: обновить конкретный цвет
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 3 зданий с 2 ступенчатыми пирамидами рядом, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (50, 400), (250, 380), (450, 420)
# - 2 Pyramid рядом с зданиями

# Пример создания пирамиды:
# pyramid = Pyramid(building.x + building.width + 50, building.y, 
#                   150, 200, "Pyramid1")
# canvas.add_element(pyramid)

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
        # TODO: создать Rectangle для фасада в древнем стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#8B4513', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить окна (2 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.25 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#8B4513')
                ax.add_patch(window)
        # TODO: добавить декоративные элементы (древний орнамент)
        for i in range(5):
            deco_x = self.x + self.width * 0.05 + i * (self.width * 0.22)
            ax.plot([deco_x, deco_x + 10], [self.y + self.height - 10, self.y + self.height], 
                   color='#8B4513', linewidth=2)

class Pyramid(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 300)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать ступенчатую пирамиду (5 ступеней)
        num_steps = 5
        step_height = self.height / num_steps
        current_width = self.width
        current_x = self.x
        
        for i in range(num_steps):
            step_y = self.y + i * step_height
            step = Rectangle((current_x, step_y), current_width, step_height, 
                           facecolor=color, edgecolor='#8B4513', linewidth=2)
            ax.add_patch(step)
            # Уменьшаем ширину для следующей ступени
            current_width *= 0.8
            current_x += (self.width - current_width) / 2
        # TODO: добавить вершину пирамиды
        apex_x = self.x + self.width / 2
        apex_y = self.y + self.height
        apex = Polygon([
            (apex_x - 15, apex_y),
            (apex_x + 15, apex_y),
            (apex_x, apex_y + 20)
        ], facecolor='#FFD700', edgecolor='#8B4513', linewidth=2)
        ax.add_patch(apex)
        # TODO: добавить лестницу на одной стороне
        for i in range(num_steps):
            stair_x = self.x + self.width
            stair_y = self.y + i * step_height
            ax.plot([stair_x, stair_x + 20], [stair_y, stair_y + step_height], 
                   color='#8B4513', linewidth=2)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и пирамиды должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" древней архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать пирамиду с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от жёлтого к коричневому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления ступеней к пирамиде, где ступени появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" древней пирамиды.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Pyramid: высота должна быть достаточной для ступеней (не менее 100px)

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
        self.step_level = 0  # уровень ступеней
    
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

class Pyramid(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.num_steps = 5  # количество ступеней
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 100:  # минимальная высота для ступеней
            value = 100
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_step_decor()` — добавляет древний декор
- `get_color()` — возвращает цвет в градиенте от жёлтого к коричневому

**Для Pyramid**:
- `add_step(n)` — добавляет n ступеней к пирамиде
- `get_color()` — возвращает цвет в градиенте от жёлтого к коричневому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_step_decor(self):
        # TODO: увеличить step_level
        self.step_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от жёлтого к коричневому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с окнами
        # TODO: если step_level > 0: добавить древний декор
        pass

class Pyramid(ArchElement):
    def add_step(self, n):
        # TODO: увеличить количество ступеней на n
        self.num_steps += n
        # TODO: увеличить высоту пропорционально
        self.height += 20 * n
        # TODO: добавить запись в history
        self.history.append(self._height)
        # TODO: вернуть self
        return self
    
    def get_color(self):
        # TODO: градиент от жёлтого к коричневому по высоте
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        color = self.get_color()
        # TODO: отрисовать ступенчатую пирамиду с учётом num_steps
        # TODO: отрисовать вершину и лестницу
        pass
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity`, который поддерживает пошаговую анимацию трансформаций.

**Требования**:
- метод `save_frame()` — сохраняет текущие параметры всех элементов
- метод `animate(interval)` — показывает анимацию с задержкой между кадрами
- Анимация должна показывать постепенное добавление ступеней к пирамиде

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
                'steps': getattr(elem, 'num_steps', 0),
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
                              facecolor='#D2B48C', edgecolor='none'))
        
        # TODO: для каждого элемента вызвать draw()
        for elem in self.elements:
            elem.draw(ax)
        
        # TODO: добавить заголовок с номером шага
        ax.set_title(f"Шаг {step}: Ступенчатая пирамида")
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

Создайте композицию из 2 зданий и 2 пирамид. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: пирамиды получают первые ступени
- Шаги 4-5: пирамиды продолжают расти, добавляются ступени
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

pyramids = [
    Pyramid(180, 100, 150, 150, "P1", 800, 600),
    Pyramid(430, 100, 150, 160, "P2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for p in pyramids:
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
        # Пирамиды добавляют ступени
        for elem in pyramids:
            elem.add_step(1)
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    s = getattr(elem, 'num_steps', 0)
    print(f"{elem.name}: высота={h}, ступени={s}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания и пирамиды жёлтого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится коричневым
- Шаг 3: пирамиды получают первые ступени (с 5 до 6)
- Шаг 4-5: пирамиды продолжают расти, добавляются новые ступени (до 7-8)
- Шаг 6: финальная композиция с градиентом от жёлтого к насыщенному коричневому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту жёлтый→коричневый
- Появлению новых ступеней на пирамидах
- Увеличению высоты пирамид
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как древний комплекс со ступенчатыми пирамидами "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление ступеней и трансформацию цветовой схемы от жёлтого к коричневому. Важно показать разнообразие: обычные здания и пирамиды должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D` и `Pyramid3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для пирамиды дополнительно хранить `self.step_path` (история количества ступеней)
- Метод `get_color()` возвращает цвет в градиенте от жёлтого к коричневому

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
        # TODO: вернуть цвет в градиенте от жёлтого к коричневому
        from matplotlib.colors import to_hex, to_rgb
        yellow = to_rgb('#FFD700')
        brown = to_rgb('#8B4513')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(y + (b - y) * ratio for y, b in zip(yellow, brown))
        return to_hex(interpolated)


class Building3D(ArchElement3D):
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()


class Pyramid3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.num_steps = 5
        self.num_steps = 5
        # TODO: создать self.step_path = [5]
        self.step_path = [5]
    
    def get_color(self):
        # TODO: обновить color_ratio на основе высоты
        self.color_ratio = min(1.0, self.path[-1] / 300)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте две функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с древними элементами
- `create_pyramid_mesh(x, y, width, depth, height, num_steps, color)` — ступенчатая пирамида (несколько уменьшающихся параллелепипедов)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с древними элементами"""
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_pyramid_mesh(x, y, width, depth, height, num_steps, color):
    """Создает ступенчатую пирамиду: несколько уменьшающихся параллелепипедов"""
    traces = []
    
    step_height = height / num_steps
    current_width = width
    current_depth = depth
    current_x = x
    current_y = y
    
    for i in range(num_steps):
        # Смещение для центрирования каждой ступени
        offset_x = current_x + (width - current_width) / 2
        offset_y = current_y + (depth - current_depth) / 2
        offset_z = i * step_height
        
        # 8 вершин текущей ступени
        vertices_x = [offset_x, offset_x+current_width, offset_x+current_width, offset_x, 
                     offset_x, offset_x+current_width, offset_x+current_width, offset_x]
        vertices_y = [offset_y, offset_y, offset_y+current_depth, offset_y+current_depth,
                     offset_y, offset_y, offset_y+current_depth, offset_y+current_depth]
        vertices_z = [offset_z, offset_z, offset_z, offset_z,
                     offset_z+step_height, offset_z+step_height, 
                     offset_z+step_height, offset_z+step_height]
        
        # Грани ступени
        i_idx = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
        j_idx = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
        k_idx = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
        
        step_mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                             i=i_idx, j=j_idx, k=k_idx,
                             color=color, opacity=0.95)
        traces.append(step_mesh)
        
        # Уменьшаем ширину и глубину для следующей ступени
        current_width *= 0.8
        current_depth *= 0.8
    
    # Вершина пирамиды (золотая)
    apex_x = x + width / 2
    apex_y = y + depth / 2
    apex_z = height
    
    traces.append(go.Scatter3d(
        x=[apex_x], y=[apex_y], z=[apex_z],
        mode='markers',
        marker=dict(size=8, color='#FFD700')
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 4 элементов (2 Building3D + 2 Pyramid3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-7**: пирамиды добавляют ступени и растут

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Pyramid3D(2.0, 1.0, 1.0, 150, "P1"),
    Pyramid3D(4.0, 1.0, 1.1, 160, "P2")
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
        elif isinstance(elem, Pyramid3D) and step >= 4:
            # Пирамиды добавляют ступени и растут на шагах 4-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.num_steps += 1
            elem.step_path.append(elem.num_steps)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (песочный цвет)
- Все здания с текущей высотой из `path[step]`
- Все пирамиды с текущим количеством ступеней из `step_path[step]`
- Цветовую динамику от жёлтого к коричневому

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
        colorscale=[[0, '#D2B48C'], [1, '#C19A6B']], 
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
        elif isinstance(elem, Pyramid3D):
            color = elem.get_color()
            s = elem.step_path[step] if step < len(elem.step_path) else elem.step_path[-1]
            traces.extend(create_pyramid_mesh(
                elem.x, elem.y, elem.width, 0.4, h, s, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Ступенчатая пирамида".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора древней архитектуры
- Добавлены подписи и заголовок с темой "От жёлтого к коричневому"

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
    title="🔺 Анимация: Ступенчатая пирамида (градиент от жёлтого к коричневому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания жёлтого цвета с минимальными высотами и пирамиды с 5 ступенями
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Пирамиды начинают добавлять ступени (шаги 4-7)
  - Цвет всех элементов плавно переходит от жёлтого (`#FFD700`) к насыщенному коричневому (`#8B4513`)
  - Количество ступеней увеличивается с 5 до 9-10
  - Вершина пирамиды светится золотым цветом
  - Лестница на стороне пирамиды становится более заметной
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" древнего архитектурного ансамбля
