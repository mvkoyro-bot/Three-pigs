# Class Diagram: Система "Три поросёнка"

## Обзор

Эта диаграмма классов показывает объектно-ориентированную структуру системы сказки "Три поросёнка".

## Иерархия классов

### Иерархия домов

| Class | Type | Attributes | Methods |
|-------|------|------------|---------|
| BaseHouse | Abstract | # name: string<br># strength: int<br># currentStrength: int<br># resistance: double<br># status: HouseStatus | + BaseHouse(n: string, str: int, res: double)<br>+ takeDamage(damage: int): int<br>+ repair(): void<br>+ build(): void<br>+ showInfo(): void<br>+ getName(): string<br>+ getCurrentStrength(): int<br>+ getStatus(): HouseStatus<br>+ getResistance(): double<br>+ isDestroyed(): bool<br>+ isReady(): bool<br>+ isUnderConstruction(): bool |
| StrawHouse | Concrete | strength = 30<br>resistance = 0.1 (10%) | extends BaseHouse |
| WoodHouse | Concrete | strength = 60<br>resistance = 0.3 (30%) | extends BaseHouse |
| BrickHouse | Concrete | strength = 100<br>resistance = 0.7 (70%) | extends BaseHouse |

### Иерархия персонажей

| Class | Type | Attributes | Methods |
|-------|------|------------|---------|
| Wolf | Concrete | - name: string<br>- strength: int = 100 | + Wolf()<br>+ blow(house: BaseHouse*): void<br>+ tryEnterChimney(house: BrickHouse*): bool |

### Контроллер игры

| Класс | Тип | Атрибуты | Методы |
|-------|------|----------|--------|
| GameController | Concrete | - strawHouse: StrawHouse*<br>- woodHouse: WoodHouse*<br>- brickHouse: BrickHouse*<br>- wolf: Wolf* | + GameController()<br>+ startGame(): void<br>+ buildHouses(): void<br>+ wolfAttack(): void<br>+ showGameStatus(): void<br>+ ~GameController() |

### Перечисление

| Enum | Values |
|------|--------|
| HouseStatus | UNDER_CONSTRUCTION<br>READY<br>DESTROYED |

## Связи

- **BaseHouse** <|-- **StrawHouse**: наследует
- **BaseHouse** <|-- **WoodHouse**: наследует
- **BaseHouse** <|-- **BrickHouse**: наследует
- **GameController** *-- **StrawHouse**: композиция (владеет)
- **GameController** *-- **WoodHouse**: композиция (владеет)
- **GameController** *-- **BrickHouse**: композиция (владеет)
- **GameController** *-- **Wolf**: композиция (владеет)
- **Wolf** --> **BaseHouse**: взаимодействует (ассоциация)

## Шаблоны проектирования

### Фабричный метод (в GameController)

```java
public void buildHouses() {
    strawHouse = new StrawHouse();
    woodHouse = new WoodHouse();
    brickHouse = new BrickHouse();
}
```
## Диаграмма

![Диаграмма классов](class.png)

```plantuml
@startuml
skinparam classAttributeIconSize 0

' --- Иерархия домов ---
abstract class BaseHouse {
    # name: string
    # strength: int
    # currentStrength: int
    # resistance: double
    # status: HouseStatus
    --
    + BaseHouse(n: string, str: int, res: double)
    + takeDamage(damage: int): int
    + repair(): void
    + build(): void
    + showInfo(): void
    + getName(): string
    + getCurrentStrength(): int
    + getStatus(): HouseStatus
    + getResistance(): double
    + isDestroyed(): bool
    + isReady(): bool
    + isUnderConstruction(): bool
}

class StrawHouse extends BaseHouse {
    + StrawHouse()
}

class WoodHouse extends BaseHouse {
    + WoodHouse()
}

class BrickHouse extends BaseHouse {
    + BrickHouse()
}

' --- Класс Волк ---
class Wolf {
    - name: string
    - strength: int
    --
    + Wolf()
    + blow(house: BaseHouse*): void
    + tryEnterChimney(house: BrickHouse*): bool
}

' --- Контроллер игры ---
class GameController {
    - strawHouse: StrawHouse*
    - woodHouse: WoodHouse*
    - brickHouse: BrickHouse*
    - wolf: Wolf*
    --
    + GameController()
    + startGame(): void
    + buildHouses(): void
    + wolfAttack(): void
    + showGameStatus(): void
    + ~GameController()
}

' --- Перечисление ---
enum HouseStatus {
    UNDER_CONSTRUCTION
    READY
    DESTROYED
}

' --- СВЯЗИ ---
BaseHouse <|-- StrawHouse
BaseHouse <|-- WoodHouse
BaseHouse <|-- BrickHouse

GameController *-- StrawHouse
GameController *-- WoodHouse
GameController *-- BrickHouse
GameController *-- Wolf

Wolf --> BaseHouse : взаимодействует

note right of StrawHouse
  Соломенный дом
  прочность = 30
  сопротивление = 10%
end note

note right of WoodHouse
  Деревянный дом
  прочность = 60
  сопротивление = 30%
end note

note right of BrickHouse
  Кирпичный дом
  прочность = 100
  сопротивление = 70%
end note

note right of Wolf
  Волк
  сила атаки = 100
end note

note bottom of GameController
  GameController управляет всей игрой,
  создаёт объекты и координирует
  взаимодействие между Волком и домами.
end note

@enduml
```
