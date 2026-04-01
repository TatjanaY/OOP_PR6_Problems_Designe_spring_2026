# **Вариант № 12**

## **Генеративная художественная композиция "Исламская мечеть" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей исламской культуры, который хочет получить интерактивную генеративную инсталляцию "Исламская мечеть". Инсталляция должна создавать уникальные архитектурные композиции в исламском стиле, где здания, купола и минареты оживают и трансформируются в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания, купола и минареты — это "живые" объекты, способные расти, менять цвет от белого к голубому и реагировать на взаимодействие, создавая динамичные композиции в стиле исламской архитектуры.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 3
dome_count = 2  # количество куполов
minaret_count = 2  # количество минаретов
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от белого к голубому. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий, куполов и минаретов, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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

- **`Building`** — прямоугольное здание с окнами в исламском стиле
- **`Dome`** — купол (полусферическая крыша)
- **`Minaret`** — минарет (высокая башня с балконом)

### **Шаблон кода**

```python
class Building(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания в исламском стиле
        # TODO: добавить арочные окна (3 ряда по 3 окна)
        # TODO: добавить орнамент по краям
        pass

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать основание купола
        # TODO: отрисовать полусферу купола (patches.Arc)
        # TODO: добавить полумесяц на вершине
        # TODO: использовать градиент от белого к голубому
        pass

class Minaret(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать ствол минарета (цилиндр)
        # TODO: добавить балкон (шире ствола)
        # TODO: отрисовать шпиль на вершине
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
- метод `add_minaret_near(building)` добавляет минарет рядом со зданием
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
    
    def add_minaret_near(self, building):
        # TODO: создать минарет рядом со зданием
        # TODO: позиция: building.x + building.width + 30, building.y
        # TODO: добавить минарет в elements
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
- метод `get(name)` — возвращает цвет по имени ('sky', 'building', 'dome', 'minaret')
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
                'ground': '#D2B48C',
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

Создайте композицию из 3 зданий с 2 куполами и 2 минаретами, используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
# создание цветовой темы
theme = ColorTheme()
theme.set_theme('noon')

# создание холста
canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 3 Building на координатах (80, 400), (280, 380), (480, 420)
# - 2 Dome над зданиями
# - 2 Minaret рядом с зданиями

# Пример создания купола:
# dome = Dome(building.x + building.width*0.2, building.y + building.height, 
#             building.width*0.6, 80, "Dome1")
# canvas.add_element(dome)

# Пример создания минарета:
# minaret = Minaret(building.x + building.width + 30, building.y, 
#                   40, 180, "Minaret1")
# canvas.add_element(minaret)

canvas.render()
```

---

## **6. Визуализация через matplotlib**

### **Задание**

Реализуйте отрисовку элементов, используя библиотеку `matplotlib.patches`.

### **Шаблон кода**

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle, Polygon, Circle, Arc

class Building(ArchElement):
    def draw(self, ax):
        # TODO: получить базовый цвет из темы
        color = theme.get('building')
        # TODO: создать Rectangle для фасада в исламском стиле
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='#4682B4', linewidth=2)
        ax.add_patch(rect)
        # TODO: добавить арочные окна (3 ряда по 3)
        window_w, window_h = self.width * 0.15, self.height * 0.15
        for row in range(3):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.25)
                window = Rectangle((wx, wy), window_w, window_h, 
                                  facecolor='#87CEFA', edgecolor='#4682B4')
                ax.add_patch(window)
                # Арочное завершение
                arc = Arc((wx + window_w/2, wy + window_h), 
                         window_w, window_h*0.5, 
                         theta1=0, theta2=180, color='#4682B4', linewidth=1)
                ax.add_patch(arc)

class Dome(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту (зависит от высоты)
        ratio = min(1.0, self.height / 150)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать основание купола
        base = Rectangle((self.x, self.y), self.width, self.height*0.3, 
                        facecolor=color, edgecolor='#4682B4', linewidth=2)
        ax.add_patch(base)
        # TODO: отрисовать полусферу купола
        dome = Arc((self.x + self.width/2, self.y + self.height*0.3), 
                  self.width, self.height*1.4, 
                  theta1=0, theta2=180, 
                  facecolor=color, edgecolor='#4682B4', linewidth=2)
        ax.add_patch(dome)
        # TODO: добавить полумесяц на вершине
        crescent_x = self.x + self.width/2
        crescent_y = self.y + self.height*1.0
        crescent = Circle((crescent_x, crescent_y), 10, 
                         facecolor='#FFD700', edgecolor='#4682B4')
        ax.add_patch(crescent)

class Minaret(ArchElement):
    def draw(self, ax):
        # TODO: вычислить цвет по градиенту
        ratio = min(1.0, self.height / 250)
        color = theme.get_gradient(ratio)
        # TODO: отрисовать ствол минарета
        shaft = Rectangle((self.x, self.y), self.width, self.height*0.8, 
                         facecolor=color, edgecolor='#4682B4', linewidth=2)
        ax.add_patch(shaft)
        # TODO: добавить балкон (шире ствола)
        balcony_y = self.y + self.height*0.6
        balcony = Rectangle((self.x - 10, balcony_y), 
                           self.width + 20, self.height*0.1,
                           facecolor=color, edgecolor='#4682B4', linewidth=2)
        ax.add_patch(balcony)
        # TODO: отрисовать шпиль на вершине
        spire_x = self.x + self.width/2
        spire_y = self.y + self.height*0.8
        ax.plot([spire_x, spire_x], [spire_y, spire_y + self.height*0.2], 
               color='#4682B4', linewidth=3)
        # Полумесяц на шпиле
        crescent = Circle((spire_x, spire_y + self.height*0.2), 8, 
                         facecolor='#FFD700', edgecolor='#4682B4')
        ax.add_patch(crescent)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания, купола и минареты должны:
- Сохранять историю своих трансформаций (чтобы можно было отследить "рост" исламской архитектуры)
- Иметь защиту от некорректных параметров (нельзя сделать минарет с отрицательной высотой или разместить элемент за пределами холста)
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от белого к голубому в зависимости от "возраста" или высоты

**Дизайнерская задача**: Создать анимацию добавления куполов и минаретов к зданию, где элементы появляются постепенно, а цветовая динамика подчёркивает процесс "оживления" мечети.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными (`_x`, `_y`, `_width`, `_height`) с доступом через свойства (`@property`).

**Проверки**:
- x, y должны быть в пределах [0, ширина холста] и [0, высота холста]
- width, height должны быть положительными числами
- При изменении высоты автоматически пересчитывается количество этажей (`self.floors = height // 20`)
- Для Minaret: высота должна быть достаточной для балкона (не менее 150px)

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
        self.ornament_level = 0  # уровень орнамента
    
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

class Minaret(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.balcony_level = 1  # количество балконов
        self.history = [height]
    
    @property
    def height(self):
        return self._height
    
    @height.setter
    def height(self, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        if value < 150:  # минимальная высота для балкона
            value = 150
        self.history.append(self._height)
        self._height = value
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте каждому классу-наследнику специфические методы трансформации.

**Для Building**:
- `add_floors(n)` — увеличивает высоту на n*30px
- `add_ornament()` — добавляет исламский орнамент
- `get_color()` — возвращает цвет в градиенте от белого к голубому

**Для Dome**:
- `add_dome(height)` — увеличивает высоту купола
- `get_color()` — возвращает цвет в градиенте от белого к голубому

**Для Minaret**:
- `add_minaret(height)` — увеличивает высоту минарета
- `add_balcony()` — добавляет балкон к минарету
- `get_color()` — возвращает цвет в градиенте от белого к голубому

### **Шаблон кода**

```python
class Building(ArchElement):
    def add_floors(self, n):
        # TODO: увеличить высоту на n * 30
        self.height += n * 30
        # TODO: вернуть self для цепочечных вызовов
        return self
    
    def add_ornament(self):
        # TODO: увеличить ornament_level
        self.ornament_level += 1
        return self
    
    def get_color(self):
        # TODO: вернуть цвет в градиенте от белого к голубому
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)
    
    def draw(self, ax):
        # TODO: получить цвет через get_color()
        # TODO: отрисовать здание с арочными окнами
        # TODO: если ornament_level > 0: добавить исламский орнамент
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
        ratio = min(1.0, self._height / 150)
        return theme.get_gradient(ratio)

class Minaret(ArchElement):
    def add_minaret(self, height):
        # TODO: увеличить высоту минарета
        self.height += height
        self.history.append(self._height)
        return self
    
    def add_balcony(self):
        # TODO: увеличить количество балконов
        self.balcony_level += 1
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
- Анимация должна показывать постепенное добавление куполов и минаретов

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
                'ornament': getattr(elem, 'ornament_level', 0),
                'balconies': getattr(elem, 'balcony_level', 0)
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
        ax.set_title(f"Шаг {step}: Исламская мечеть")
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

Создайте композицию из 2 зданий, 2 куполов и 2 минаретов. Реализуйте анимацию в 6 шагов, где:
- Шаги 1-2: здания добавляют этажи
- Шаг 3: появляются купола над зданиями
- Шаги 4-5: минареты растут, добавляются балконы
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
    Building(80, 150, 100, 80, "B1", 800, 600),
    Building(300, 150, 100, 75, "B2", 800, 600)
]

domes = [
    Dome(100, 230, 60, 60, "D1", 800, 600),
    Dome(320, 225, 60, 65, "D2", 800, 600)
]

minarets = [
    Minaret(200, 100, 40, 150, "M1", 800, 600),
    Minaret(420, 100, 40, 160, "M2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)
for d in domes:
    canvas.add_element(d)
for m in minarets:
    canvas.add_element(m)

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
        # Минареты растут и добавляют балконы
        for elem in minarets:
            elem.add_minaret(20)
            elem.add_balcony()
    canvas.record_frame()

# Запуск анимации
canvas.animate(interval=1.2)

# Вывод итоговых параметров
print("\nИтоговые параметры элементов:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    b = getattr(elem, 'balcony_level', 0)
    print(f"{elem.name}: высота={h}, балконы={b}")
```

---

## **5. Пример ожидаемого результата**

После выполнения задания должна получиться анимация, где:
- Шаг 0: базовые здания, купола и минареты белого цвета с минимальными параметрами
- Шаг 1-2: здания растут, цвет постепенно становится голубоватым
- Шаг 3: купола появляются над зданиями, увеличиваются
- Шаг 4-5: минареты растут, добавляются новые балконы
- Шаг 6: финальная композиция с градиентом от белого к насыщенному голубому

**Визуальные отличия между шагами должны быть явными благодаря:**
- Постепенному изменению цвета по градиенту белый→голубой
- Появлению куполов над зданиями
- Добавлению балконов на минаретах
- Отображению числовых параметров в консоли

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как исламская мечеть "оживает" в 3D-пространстве. Нужно создать анимацию, показывающую пошаговое добавление этажей, появление куполов и минаретов и трансформацию цветовой схемы от белого к голубому. Важно показать разнообразие: здания, купола и минареты должны визуально отличаться в трёх измерениях.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте базовый класс `ArchElement3D` и классы-наследники `Building3D`, `Dome3D` и `Minaret3D`.

**Требования**:
- Каждый элемент хранит историю своих изменений в `self.path` (список высот)
- Для минарета дополнительно хранить `self.balcony_path` (история количества балконов)
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
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 150)
        return super().get_color()


class Minaret3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        # TODO: создать self.balcony_level = 1
        self.balcony_level = 1
        # TODO: создать self.balcony_path = [1]
        self.balcony_path = [1]
    
    def get_color(self):
        self.color_ratio = min(1.0, self.path[-1] / 250)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте три функции для визуализации элементов в plotly:

- `create_building_mesh(x, y, width, depth, height, color)` — параллелепипед с арочными окнами
- `create_dome_mesh(x, y, width, depth, height, color)` — купол (полусфера)
- `create_minaret_mesh(x, y, width, depth, height, balcony_level, color)` — минарет (цилиндр + балконы)

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_building_mesh(x, y, width, depth, height, color):
    """Создает параллелепипед для plotly с исламскими элементами"""
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
    """Создает купол: основание + полусфера"""
    traces = []
    
    # Основание
    base_height = height * 0.3
    traces.append(create_building_mesh(x, y, width, depth, base_height, color))
    
    # Полусфера купола (аппроксимация через сетку)
    n_theta, n_phi = 20, 10
    theta = np.linspace(0, 2*np.pi, n_theta)
    phi = np.linspace(0, np.pi/2, n_phi)
    
    radius_x, radius_y = width/2, depth/2
    center_x, center_y = x + width/2, y + depth/2
    center_z = base_height
    
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
    
    # Полумесяц на вершине
    traces.append(go.Scatter3d(
        x=[center_x], y=[center_y], z=[center_z + radius_x],
        mode='markers',
        marker=dict(size=5, color='#FFD700')
    ))
    
    return traces


def create_minaret_mesh(x, y, width, depth, height, balcony_level, color):
    """Создает минарет: цилиндр + балконы"""
    traces = []
    
    # Ствол минарета (цилиндр)
    n_segments = 16
    base_x, base_y = [], []
    top_x, top_y = [], []
    
    center_x, center_y = x + width/2, y + depth/2
    radius = width/2
    shaft_height = height * 0.8
    
    for angle in np.linspace(0, 2*np.pi, n_segments):
        base_x.append(center_x + radius * np.cos(angle))
        base_y.append(center_y + radius * np.sin(angle))
        top_x.append(center_x + radius * np.cos(angle))
        top_y.append(center_y + radius * np.sin(angle))
    
    body_z = [0]*n_segments + [shaft_height]*n_segments
    shaft_mesh = go.Mesh3d(
        x=base_x + top_x,
        y=base_y + top_y,
        z=body_z,
        color=color, opacity=0.95,
        alphahull=0
    )
    traces.append(shaft_mesh)
    
    # Балконы
    for i in range(balcony_level):
        balcony_z = shaft_height * (0.4 + i * 0.2)
        balcony_radius = radius * 1.3
        
        balcony_x = [center_x + balcony_radius * np.cos(a) for a in np.linspace(0, 2*np.pi, n_segments)]
        balcony_y = [center_y + balcony_radius * np.sin(a) for a in np.linspace(0, 2*np.pi, n_segments)]
        balcony_z_list = [balcony_z] * n_segments
        
        balcony_mesh = go.Mesh3d(
            x=balcony_x, y=balcony_y, z=balcony_z_list,
            color=color, opacity=0.9,
            alphahull=0
        )
        traces.append(balcony_mesh)
    
    # Шпиль на вершине
    traces.append(go.Scatter3d(
        x=[center_x, center_x], y=[center_y, center_y], 
        z=[shaft_height, shaft_height + height*0.2],
        mode='lines',
        line=dict(color='#4682B4', width=4)
    ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 5 элементов (2 Building3D + 2 Dome3D + 1 Minaret3D) с разными координатами и начальными высотами.

Выполните 7 шагов трансформаций, где:
- **Шаги 1-3**: здания растут
- **Шаги 4-5**: купола увеличиваются
- **Шаги 6-7**: минареты растут, добавляются балконы

### **Шаблон кода**

```python
# Создание элементов
elements = [
    Building3D(1.0, 1.0, 0.8, 60, "B1"),
    Building3D(3.0, 1.0, 0.9, 70, "B2"),
    Dome3D(1.2, 1.0, 0.6, 60, "D1"),
    Dome3D(3.2, 1.0, 0.7, 65, "D2"),
    Minaret3D(2.0, 1.0, 0.4, 150, "M1")
]

# Коэффициенты роста
growth_rates = [15, 20, 12, 12, 18]

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
        elif isinstance(elem, Minaret3D) and step >= 6:
            # Минареты растут и добавляют балконы на шагах 6-7
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
            elem.balcony_level += 1
            elem.balcony_path.append(elem.balcony_level)
        
        # Обновление цвета
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации, где каждый кадр показывает:
- Поверхность земли (песочный цвет)
- Все здания с текущей высотой из `path[step]`
- Все купола с текущей высотой из `path[step]`
- Все минареты с текущим количеством балконов из `balcony_path[step]`
- Цветовую динамику от белого к голубому

### **Шаблон кода**

```python
frames = []
max_steps = max(len(elem.path) for elem in elements)

# Координаты земли
x_ground = np.arange(0, 5, 0.3)
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
        elif isinstance(elem, Dome3D):
            color = elem.get_color()
            traces.extend(create_dome_mesh(
                elem.x, elem.y, elem.width, 0.4, h, color
            ))
        elif isinstance(elem, Minaret3D):
            color = elem.get_color()
            b = elem.balcony_path[step] if step < len(elem.balcony_path) else elem.balcony_path[-1]
            traces.extend(create_minaret_mesh(
                elem.x, elem.y, elem.width, 0.4, h, b, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play для анимации "Исламская мечеть".

**Требования**:
- Начальная фигура показывает элементы с минимальными высотами
- Кнопка Play запускает анимацию с интервалом 600 мс
- Камера настроена для обзора исламской архитектуры
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
        camera=dict(eye=dict(x=2.5, y=2.5, z=2.0)),
        aspectmode='manual',
        aspectratio=dict(x=1.5, y=0.8, z=1.2),
        bgcolor='#87CEEB'
    ),
    title="🕌 Анимация: Исламская мечеть (градиент от белого к голубому)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания белого цвета с минимальными высотами, купола и минареты
- При нажатии Play:
  - Здания начинают расти (шаги 1-3)
  - Купола увеличиваются (шаги 4-5)
  - Минареты растут и добавляют балконы (шаги 6-7)
  - Цвет всех элементов плавно переходит от белого (`#FFFFFF`) к насыщенному голубому (`#87CEEB`)
  - Количество балконов на минарете увеличивается с 1 до 3
  - Полумесяцы на куполах и минаретах светятся золотым цветом
- Камеру можно вращать мышью для обзора композиции с разных сторон
- Градиентная цветовая динамика подчёркивает "оживление" исламского архитектурного ансамбля
