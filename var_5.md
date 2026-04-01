# **Вариант № 5**

## **Генеративная художественная композиция "Мосты над городом" (ООП, Python)**

---

## **История**

Вы — **creative technologist** в студии медиа-арта. Ваш заказчик — музей городской инфраструктуры, который хочет получить интерактивную генеративную инсталляцию "Мосты над городом". Инсталляция должна создавать уникальные городские пейзажи с мостами, соединяющими здания, где архитектура оживает и трансформируется в реальном времени.

**Ключевая идея**: использовать ООП для создания системы, где здания, башни и мосты — это "живые" объекты, способные соединяться, менять цвет от коричневого к зелёному и реагировать на взаимодействие, создавая динамичные архитектурные композиции с соединительными элементами.

---

## **Исходные данные**

Базовые параметры композиции:

```python
canvas_size = (800, 600)  # ширина, высота в пикселях
building_count = 4
bridge_probability = 0.5  # вероятность появления моста между зданиями
```

Для хранения цветовой палитры используется **паттерн Borg**, гарантирующий, что все элементы композиции используют одну цветовую схему с градиентом от коричневого к зелёному. Это позволяет плавно менять настроение инсталляции (утро/полдень/вечер) через единый объект.

---

# **Уровень 1 — базовая модель**

## **История**

На первом этапе ваш заказчик хочет увидеть прототип: базовые формы зданий, башен и мостов, которые можно размещать на холсте и визуализировать. Важно показать, как разные типы элементов могут использовать общий интерфейс, но реализовывать свою уникальную визуальную логику.

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
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.name = name
    
    @abstractmethod
    def draw(self, ax):
        pass
    
    def transform(self, scale_x, scale_y):
        self.width *= scale_x
        self.height *= scale_y
        return self
```

---

## **2. Классы-наследники архитектурных элементов**

### **Задание**

Создайте три класса, наследующихся от `ArchElement`:

- **`House`** — прямоугольное здание с окнами и крышей
- **`Tower`** — башня с пирамидальной крышей и шпилем
- **`Bridge`** — мост (соединительный элемент между зданиями)

### **Шаблон кода**

```python
class House(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник здания
        # TODO: добавить крышу (треугольник)
        # TODO: добавить окна (2 ряда по 3 окна)
        pass

class Tower(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать прямоугольник основания
        # TODO: отрисовать пирамидальную крышу
        # TODO: добавить шпиль (линия от вершины)
        pass

class Bridge(ArchElement):
    def draw(self, ax):
        # TODO: отрисовать полотно моста (горизонтальный прямоугольник)
        # TODO: добавить опоры моста (вертикальные линии)
        # TODO: добавить перила (тонкие линии сверху)
        # TODO: использовать градиент от коричневого к зелёному
        pass
```

---

## **3. Класс CityCanvas (итерируемый)**

### **Задание**

Создайте класс `CityCanvas`, который управляет коллекцией архитектурных элементов.

**Требования**:
- хранит список элементов в `self.elements`
- метод `add_element(element)` добавляет элемент
- метод `connect_with_bridge(elem1, elem2)` создаёт мост между двумя элементами
- метод `render()` отображает всю композицию
- класс должен быть итерируемым (реализовать `__iter__`)

### **Шаблон кода**

```python
class CityCanvas:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.elements = []
    
    def add_element(self, element):
        self.elements.append(element)
    
    def connect_with_bridge(self, elem1, elem2):
        # TODO: вычислить позицию моста между elem1 и elem2
        # TODO: создать объект Bridge и добавить его в elements
        bridge_x = elem1.x + elem1.width
        bridge_y = min(elem1.y + elem1.height, elem2.y + elem2.height) - 50
        bridge_w = elem2.x - bridge_x
        bridge = Bridge(bridge_x, bridge_y, bridge_w, 20, "Bridge1")
        self.add_element(bridge)
    
    def render(self):
        fig, ax = plt.subplots(figsize=(10, 6))
        ax.set_xlim(0, self.width)
        ax.set_ylim(0, self.height)
        for elem in self.elements:
            elem.draw(ax)
        plt.show()
    
    def __iter__(self):
        return iter(self.elements)
```

---

## **4. Паттерн Borg для управления цветом**

### **Задание**

Реализуйте класс `ColorTheme`, использующий **паттерн Borg** для хранения цветовой схемы композиции.

**Требования**:
- все экземпляры класса имеют общий словарь `colors`
- метод `set_theme(theme_name)` — устанавливает тему ('morning', 'noon', 'evening') с градиентом от коричневого к зелёному
- метод `get(name)` — возвращает цвет по имени
- метод `get_gradient(ratio)` — возвращает цвет в градиенте от #8B4513 к #228B22 (ratio от 0 до 1)

### **Шаблон кода**

```python
class ColorTheme:
    _shared_state = {}
    
    def __init__(self):
        self.__dict__ = self._shared_state
        if not self._shared_state:
            self.colors = {
                'sky': '#87CEEB',
                'ground': '#8B7355',
                'base_brown': '#8B4513',
                'base_green': '#228B22'
            }
    
    def set_theme(self, theme_name):
        pass
    
    def get(self, name):
        return self.colors.get(name, '#000000')
    
    def get_gradient(self, ratio):
        from matplotlib.colors import to_rgb, to_hex
        brown = to_rgb('#8B4513')
        green = to_rgb('#228B22')
        interpolated = tuple(b + (g - b) * ratio for b, g in zip(brown, green))
        return to_hex(interpolated)
    
    def update(self, name, color):
        self.colors[name] = color
```

---

## **5. Основной цикл создания композиции**

### **Задание**

Создайте композицию из 4 элементов (дома, башни и мосты между ними), используя созданные классы. Отобразите результат.

### **Шаблон кода**

```python
theme = ColorTheme()
theme.set_theme('noon')

canvas = CityCanvas(800, 600)

# TODO: создать и добавить элементы:
# - 2 House на координатах (50, 400), (250, 380)
# - 2 Tower на координатах (450, 350), (650, 360)
# - Мосты между соседними элементами с вероятностью 50%

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

class House(ArchElement):
    def draw(self, ax):
        color = theme.get('base_brown')
        # Здание
        rect = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='black', linewidth=1)
        ax.add_patch(rect)
        # Крыша
        roof = Polygon([
            (self.x - 10, self.y + self.height),
            (self.x + self.width + 10, self.y + self.height),
            (self.x + self.width/2, self.y + self.height + 50)
        ], facecolor='#654321', edgecolor='black')
        ax.add_patch(roof)
        # Окна
        for row in range(2):
            for col in range(3):
                wx = self.x + self.width * 0.1 + col * (self.width * 0.27)
                wy = self.y + self.height * 0.2 + row * (self.height * 0.35)
                window = Rectangle((wx, wy), self.width * 0.15, self.height * 0.15, 
                                  facecolor='#87CEFA', edgecolor='black')
                ax.add_patch(window)

class Bridge(ArchElement):
    def draw(self, ax):
        ratio = min(1.0, self.height / 100)
        color = theme.get_gradient(ratio)
        # Полотно моста
        deck = Rectangle((self.x, self.y), self.width, self.height, 
                        facecolor=color, edgecolor='black', linewidth=2)
        ax.add_patch(deck)
        # Опоры
        for i in range(4):
            support_x = self.x + self.width * (i + 1) / 5
            support = Rectangle((support_x - 5, self.y - 100), 10, 100, 
                               facecolor=color, edgecolor='black')
            ax.add_patch(support)
        # Перила
        ax.plot([self.x, self.x + self.width], [self.y + self.height - 5, self.y + self.height - 5], 
               color='black', linewidth=1)
```

---

# **Уровень 2 — расширенная модель**

## **История**

Заказчик впечатлен прототипом, но хочет больше интерактивности. Теперь здания и мосты должны:
- Сохранять историю своих трансформаций
- Иметь защиту от некорректных параметров
- Визуально демонстрировать изменения: цвет элементов должен меняться в градиенте от коричневого к зелёному

**Дизайнерская задача**: Создать анимацию появления мостов между зданиями, где соединения возникают постепенно.

---

## **1. Инкапсуляция свойств элементов**

### **Задание**

Сделайте координаты и размеры приватными с доступом через свойства.

### **Шаблон кода**

```python
class House(ArchElement):
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
        self.bridge_level = 0
    
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
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("height must be positive")
        self.history.append(self._height)
        self._height = value
        self.floors = int(self._height // 20)
```

---

## **2. Методы трансформации элементов**

### **Задание**

Добавьте специфические методы трансформации.

### **Шаблон кода**

```python
class House(ArchElement):
    def add_floors(self, n):
        self.height += n * 30
        return self
    
    def connect(self, other):
        # TODO: установить связь с другим зданием для моста
        self.bridge_level += 1
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 300)
        return theme.get_gradient(ratio)

class Tower(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.spire_height = 0
        self.history = [height]
    
    def add_spire(self, height):
        self.spire_height += height
        self.history.append(self.spire_height)
        return self
    
    def connect(self, other):
        self.bridge_level += 1
        return self
    
    def get_color(self):
        ratio = min(1.0, (self._height + self.spire_height) / 400)
        return theme.get_gradient(ratio)

class Bridge(ArchElement):
    def __init__(self, x, y, width, height, name, canvas_width, canvas_height):
        super().__init__(x, y, width, height, name)
        self.canvas_width = canvas_width
        self.canvas_height = canvas_height
        self.connected = False
        self.history = [height]
    
    def connect(self):
        self.connected = True
        return self
    
    def get_color(self):
        ratio = min(1.0, self._height / 150)
        return theme.get_gradient(ratio)
```

---

## **3. Визуализация с анимацией**

### **Задание**

Создайте класс `AnimatedCity` для пошаговой анимации.

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
        frame = {
            elem.name: {
                'height': getattr(elem, '_height', elem.height),
                'spire': getattr(elem, 'spire_height', 0),
                'connected': getattr(elem, 'connected', False)
            }
            for elem in self.elements
        }
        self.frames.append(frame)
    
    def show_frame(self, step, frame_data):
        fig, ax = plt.subplots(figsize=(10, 6))
        ax.set_xlim(0, self.width)
        ax.set_ylim(0, self.height)
        ax.set_facecolor('#87CEEB')
        
        ax.add_patch(Rectangle((0, 0), self.width, 50, 
                              facecolor='#8B7355', edgecolor='none'))
        
        for elem in self.elements:
            elem.draw(ax)
        
        ax.set_title(f"Шаг {step}: Мосты над городом")
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

Создайте анимацию в 6 шагов появления мостов между зданиями.

### **Шаблон кода**

```python
theme = ColorTheme()
theme.set_theme('noon')

canvas = AnimatedCity(800, 600)

buildings = [
    House(50, 50, 80, 80, "H1", 800, 600),
    Tower(180, 50, 70, 100, "T1", 800, 600),
    House(300, 50, 75, 70, "H2", 800, 600),
    Tower(430, 50, 65, 110, "T2", 800, 600)
]

for b in buildings:
    canvas.add_element(b)

canvas.record_frame()

for step in range(1, 7):
    if step <= 2:
        for elem in buildings:
            if isinstance(elem, House):
                elem.add_floors(1)
            elif isinstance(elem, Tower):
                elem.add_spire(15)
    elif step == 3:
        # Появление первого моста
        canvas.connect_with_bridge(buildings[0], buildings[1])
    elif step == 4:
        # Появление второго моста
        canvas.connect_with_bridge(buildings[2], buildings[3])
    elif step <= 5:
        for elem in canvas.elements:
            if isinstance(elem, Bridge):
                elem.connect()
                elem.height += 5
    canvas.record_frame()

canvas.animate(interval=1.2)

print("\nИтоговые параметры:")
for elem in canvas.elements:
    h = getattr(elem, '_height', getattr(elem, 'height', 'N/A'))
    print(f"{elem.name}: высота={h}")
```

---

# **Уровень 3 — продвинутая система с интерактивной 3D-анимацией**

## **История**

Музей хочет установить инсталляцию, где посетители смогут наблюдать, как город с мостами "оживает" в 3D-пространстве.

---

## **1. Классы для 3D-элементов**

### **Задание**

Создайте классы `House3D`, `Tower3D` и `Bridge3D`.

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
        brown = to_rgb('#8B4513')
        green = to_rgb('#228B22')
        ratio = min(1.0, self.color_ratio)
        interpolated = tuple(b + (g - b) * ratio for b, g in zip(brown, green))
        return to_hex(interpolated)


class House3D(ArchElement3D):
    def get_color(self):
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


class Bridge3D(ArchElement3D):
    def __init__(self, x, y, width, height, name):
        super().__init__(x, y, width, height, name)
        self.connected = False
    
    def get_color(self):
        if not self.connected:
            return '#8B451300'
        self.color_ratio = min(1.0, self.path[-1] / 150)
        return super().get_color()
```

---

## **2. Функции создания 3D-мешей**

### **Задание**

Создайте функции для визуализации в plotly.

### **Шаблон кода**

```python
import numpy as np
import plotly.graph_objects as go

def create_house_mesh(x, y, width, depth, height, color):
    vertices_x = [x, x+width, x+width, x, x, x+width, x+width, x]
    vertices_y = [y, y, y+depth, y+depth, y, y, y+depth, y+depth]
    vertices_z = [0, 0, 0, 0, height, height, height, height]
    
    i = [0,0,1,1,2,2,3,3,4,4,5,5,6,6,7,7]
    j = [1,1,2,2,3,3,0,0,5,5,6,6,7,7,4,4]
    k = [4,5,5,6,6,7,7,4,1,2,2,3,3,0,0,1]
    
    mesh = go.Mesh3d(x=vertices_x, y=vertices_y, z=vertices_z,
                    i=i, j=j, k=k, color=color, opacity=0.95)
    return mesh


def create_bridge_mesh(x, y, width, depth, height, color):
    traces = []
    
    # Полотно моста
    traces.append(create_house_mesh(x, y, width, depth, height, color))
    
    # Опоры
    for i in range(4):
        support_x = x + width * (i + 1) / 5
        traces.append(create_house_mesh(
            support_x - 5, y, 10, depth, 100, color
        ))
    
    return traces
```

---

## **3. Основной цикл трансформаций**

### **Задание**

Создайте композицию из 5 элементов с анимацией появления мостов.

### **Шаблон кода**

```python
elements = [
    House3D(1.0, 1.0, 0.8, 60, "H1"),
    Tower3D(2.5, 1.0, 0.9, 70, "T1"),
    House3D(4.0, 1.0, 0.7, 50, "H2"),
    Tower3D(5.8, 1.0, 0.8, 80, "T2"),
    Bridge3D(1.8, 1.0, 0.7, 30, "B1"),
    Bridge3D(4.8, 1.0, 0.7, 30, "B2")
]

# Мосты появляются на шаге 4
for elem in elements[4:]:
    elem.path = [0]
    elem.connected = False

growth_rates = [15, 20, 12, 18, 10, 10]

for step in range(1, 8):
    for i, elem in enumerate(elements):
        if isinstance(elem, Bridge3D) and step < 4:
            continue
        
        if isinstance(elem, Bridge3D) and step == 4:
            elem.connected = True
            elem.path.append(30)
        elif isinstance(elem, (House3D, Tower3D)):
            new_h = elem._height + growth_rates[i]
            elem._height = new_h
            elem.path.append(new_h)
        
        elem.get_color()
```

---

## **4. Формирование кадров анимации**

### **Задание**

Создайте кадры для анимации.

### **Шаблон кода**

```python
frames = []
max_steps = max(len(elem.path) for elem in elements)

x_ground = np.arange(0, 8, 0.3)
y_ground = np.arange(0, 3, 0.3)
X, Y = np.meshgrid(x_ground, y_ground)

for step in range(max_steps):
    traces = []
    
    traces.append(go.Surface(
        x=X, y=Y, z=np.zeros_like(X),
        colorscale='Greens', showscale=False, opacity=0.4
    ))
    
    for elem in elements:
        if isinstance(elem, Bridge3D) and not elem.connected:
            continue
        
        h = elem.path[step] if step < len(elem.path) else elem.path[-1]
        
        if isinstance(elem, (House3D, Tower3D)):
            color = elem.get_color()
            traces.append(create_house_mesh(
                elem.x, elem.y, elem.width, 0.5, h, color
            ))
        elif isinstance(elem, Bridge3D):
            color = elem.get_color()
            traces.extend(create_bridge_mesh(
                elem.x, elem.y, elem.width, 0.4, h, color
            ))
    
    frames.append(go.Frame(data=traces, name=f'step{step}'))
```

---

## **5. Интерактивная 3D-визуализация**

### **Задание**

Создайте фигуру plotly с кнопкой Play.

### **Шаблон кода**

```python
fig = go.Figure(data=frames[0].data, frames=frames)

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
        camera=dict(eye=dict(x=2.5, y=2.0, z=1.8)),
        aspectmode='manual',
        aspectratio=dict(x=1.5, y=0.6, z=1.0),
        bgcolor='#87CEEB'
    ),
    title="🌉 Анимация: Мосты над городом (градиент от коричневого к зелёному)",
    showlegend=False,
    margin=dict(l=0, r=0, t=50, b=0)
)

fig.show()
```

---

## **6. Ожидаемый результат**

После выполнения задания должна получиться интерактивная 3D-анимация, где:
- На начальном кадре показаны здания коричневого цвета
- При нажатии Play здания растут
- На шаге 4 появляются мосты между зданиями
- Цвет всех элементов плавно переходит от коричневого к зелёному
- Камеру можно вращать мышью
