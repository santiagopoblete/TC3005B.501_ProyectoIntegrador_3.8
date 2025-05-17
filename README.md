# TC3005B.501_ProyectoIntegrador_3.8
Este proyecto consiste en una imitación del juego Breakout utilizando Pygame. Se aplicaron múltiples patrones de diseño para mantener una arquitectura clara, escalable y bien estructurada.

## Requisitos del proyecto

- Implemente al menos 4 patrones de diseño.
- Incluir una justificación por patrón.
- Agregar un diagrama de clases UML del diseño.

## Justificación de Patrones de Diseño

### 1. Patrón Creacional – Singleton

#### Clase: GameConfig
#### Descripción:
Este patrón asegura que haya una única instancia global de configuración del juego (resolución, FPS, tamaño de bola, etc.), evitando la creación múltiple de configuraciones que podrían causar inconsistencias.

### 2. Patrón Estructural – Flyweight

#### Clase: BallType, Ball y Ball Factory en conjunto
Descripción:
Se usa para compartir eficientemente recursos pesados como texturas de las bolas (BallType). La fábrica evita cargar la misma textura más de una vez, ahorrando memoria y tiempo de carga.

### 3. Patrón de Comportamiento – Command
#### Clase: InputHandler

Clases involucradas: Command, MoveBallCommand, Invoker (según implementación en el notebook)
Descripción:
Se encapsulan acciones que permiten mover al jugador en objetos comando, permitiendo desacoplar la ejecución de una acción de su invocador.

### 4. Patrón Adicional – Builder

#### Clase: BrickBuilder
Descripción:
Este patrón permite construir objetos complejos como Brick paso a paso, separando la construcción de la representación final.

## Diagrama de Clases (UML)
```mermaid
classDiagram
    GameConfig --> GameConfig
    Ball --> BallType : uses
    BallManager --> Ball : manages
    BallTypeFactory --> BallType : creates
    Command <|-- MoveLeftCommand
    Command <|-- MoveRightCommand
    MoveLeftCommand --> Player : controls
    MoveRightCommand --> Player : controls
    InputHandler --> Command : uses
    BrickBuilder --> Brick : builds
    BrickWall --> Brick : contains
    BrickWall --> BrickBuilder : uses
    GameClient --> GameConfig
    GameClient --> Player
    GameClient --> BallTypeFactory
    GameClient --> BallManager
    GameClient --> BrickWall
    GameClient --> InputHandler
    
    class GameClient{
    }

    class GameConfig {
        -static GameConfig __instance
        +static get_instance()
        -int width
        -int height
        -int fps
        -int ball_size
        +set_width(width)
        +set_height(height)
        +set_fps(fps)
        +set_ball_size(ball_size)
    }

    class BallType {
        +pygame.Surface texture
    }

    class Ball {
        +BallType ball_type
        +int x
        +int y
        +Tuple~int, int~ velocity
        +bool is_active
        +move()
        +get_position() Tuple~int, int~
    }

    class BallManager {
        -list~Ball~ _balls
        +add_ball(ball: Ball)
        +remove_ball(ball: Ball)
        +get_balls() list~Ball~
    }

    class BallTypeFactory {
        -static Dict~str, BallType~ _ball_types
        +static create_ball_type(texture_path: str) BallType
    }

    class Command {
        <<abstract>>
        +execute()
    }

    class MoveLeftCommand {
        +Player player
        +int distance
        +execute()
    }

    class MoveRightCommand {
        +Player player
        +int distance
        +execute()
    }

    class Player {
        +int x
        +int y
        +int width
        +move_left(distance: int)
        +move_right(distance: int)
        +get_position() Tuple~int, int~
    }

    class InputHandler {
        +Dict~int, Command~ commands
        +bind_input(key: int, command: Command)
        +handle_input(event)
    }

    class Brick {
        +int x
        +int y
        +int width
        +int height
        +bool is_active
        +get_position() Tuple~int, int~
        +get_size() Tuple~int, int~
        +hit()
    }

    class BrickBuilder {
        -Brick brick
        +set_position(x: int, y: int) BrickBuilder
        +set_size(width: int, height: int) BrickBuilder
        +build() Brick
    }

    class BrickWall {
        +list~Brick~ bricks
        +int width
        +int height
        +int brick_width
        +int brick_height
        +int spacing
        -BrickBuilder builder
        +create_wall()
        +get_bricks() list~Brick~
        +get_active_bricks() list~Brick~
    }
```

