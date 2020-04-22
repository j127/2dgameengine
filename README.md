# C++ 2D Game Programming

Install development requirements:

```text
$ sudo apt-get install libsdl2-dev
$ sudo apt-get install libsdl2-image-dev  # images
$ sudo apt-get install libsdl2-ttf-dev    # fonts
$ sudo apt-get install libsdl2-mixer-dev  # sounds
$ sudo apt-get install liblua5.3-dev
```

## Run

```text
$ make
$ make run
```

Press `ESC` to exit.

## Project structure:

```
$ tree
.
├── assets/
│   ├── fonts/
│   ├── images/
│   ├── sounds/
│   └── tilemaps/
├── lib/
│   ├── glm/
│   └── lua/
├── Makefile
├── README.md
└── src/
    ├── Constants.h
    ├── Game.cpp
    ├── Game.h
    └── Main.cpp
```

## The Game Loop

1. Process input
2. Update game
3. Render

```cpp
// something like
while (true) {
    game->processInput();
    game->update();
    game->render();
}
```

## Double Buffering

- Color buffer
- Back buffer
- Front buffer

Algorithm:

1. Clear the back buffer
1. Draw all the game objects
1. Swap the front and back buffers

```cpp
// general example
void Game::render() {
    // set background color
    SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);

    // clear the back buffer
    SDL_RenderClear(renderer);

    // ...
    // draw all the game objects here
    // ...

    // swap the front and back buffers
    SDL_RenderPresent(renderer);
}
```

## Entity-Component System

Each entity in the game can have multiple components.

class Entity:
- List<Component>
- Update()
- Render()

```cpp
// example
class Entity {
    vector<Component*> component;
    AddComponent<T>(component);
    Update();
    Render();
}

class EntityManager {
    vector<Entity*> entities;
    AddEntity(entity);
    GetEntity(string entityName);
    Update();
    Render();
}
```

class Component:
- Update()
- Render()

```cpp
// examples
class Component {
    Entity* owner;
    virtual Update();
    virtual Render();
}

class TransformComponent: public Component {
    glm::vec2 position;
    int width;
    int height;
    float scale;
    Update() override {};
    Render() override {};
}

class ColliderComponent: public Component {
    SDL_Rect collider;
    Update() override {};
    Render() override {};
}
```

Example child-class components:

- Transform component
- Sprite component
- Collider component
- ParticleEmitter component

The game loop:

- Game
    - EntityManager (for each entity: `entity.Update()`, `entity.Render()`)
        - Entity (for each component: `component.Update()`, `component.Render()`)

