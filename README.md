# Makora Engine

A small Java-based 2D game-engine prototype built with LWJGL and OpenGL. The project provides a window/game loop, scene switching, keyboard and mouse input, camera matrices, shader loading, and a basic level-editor scene that renders a colored quad.

> **Project status:** Early-stage prototype. The repository currently demonstrates the engine foundation and rendering pipeline rather than a complete game-authoring toolkit.

## Features

- GLFW window creation and OpenGL context initialization
- Main loop with delta-time and FPS reporting
- Scene abstraction with `LevelEditorScene` and `LevelScene`
- Keyboard and mouse input listeners
- 2D orthographic camera using JOML
- GLSL shader loading, compilation, linking, and matrix uniform uploads
- Basic VAO/VBO/EBO setup for indexed rendering
- Default vertex/fragment shader in `assets/shaders/default.glsl`

## Tech stack

- **Java**
- **Gradle** with the Gradle Wrapper
- **LWJGL 3.4.1**: GLFW, OpenGL, OpenAL, Assimp, STB, and Native File Dialog
- **JOML 1.10.8** for vector and matrix math
- **OpenGL 3.3 Core** shaders
- **JUnit 5.10.0** configured for tests

## Project structure

```text
.
├── assets/
│   └── shaders/
│       └── default.glsl          # Default vertex and fragment shader
├── src/main/java/
│   ├── main.java                 # Application entry point
│   ├── jade/
│   │   ├── Window.java           # GLFW window and main loop
│   │   ├── Scene.java            # Base scene abstraction
│   │   ├── LevelEditorScene.java # OpenGL quad rendering example
│   │   ├── LevelScene.java       # Basic gameplay scene placeholder
│   │   ├── Camera.java           # Orthographic camera matrices
│   │   ├── KeyListener.java      # Keyboard state tracking
│   │   └── MouseListener.java    # Mouse position, buttons, and scroll state
│   ├── renderer/
│   │   └── Shader.java            # GLSL parsing, compilation, and uniforms
│   └── util/
│       └── Time.java              # High-resolution elapsed time helper
├── build.gradle                   # Dependencies and build configuration
├── settings.gradle                # Gradle project settings
├── gradlew                        # Unix-like Gradle wrapper script
└── gradlew.bat                    # Windows Gradle wrapper script
```

## How it works

`main.java` obtains the singleton `jade.Window` and starts the application. `Window` initializes GLFW, creates an OpenGL context, registers keyboard and mouse callbacks, selects the initial `LevelEditorScene`, and enters the render loop.

`LevelEditorScene` creates an orthographic `Camera`, loads `assets/shaders/default.glsl`, creates vertex and index buffers for a colored quad, uploads the camera's projection and view matrices, and draws the geometry with `glDrawElements`.

## Requirements

- Java Development Kit (JDK)
- A platform capable of creating an OpenGL 3.3 Core context
- Internet access on the first Gradle build so dependencies can be downloaded

The current Gradle configuration selects the Windows LWJGL native artifacts:

```gradle
project.ext.lwjglNatives = "natives-windows"
```

For Linux or macOS, update this value to the appropriate LWJGL native classifier.

## Build and run

Clone the repository:

```bash
git clone https://github.com/Coolking5678/makora-Engine.git
cd makora-Engine
```

Build the project and run tests:

```bash
./gradlew build
./gradlew test
```

On Windows:

```bat
gradlew.bat build
gradlew.bat test
```

The application entry point is:

```text
main
```

The repository currently does not define an `application` plugin or a `run` task. Run the `main` class through your IDE, or add an application configuration to `build.gradle`.

## Scenes

The default startup scene is selected in `Window.init()` with:

```java
Window.changeScene(0);
```

Available scene IDs are:

- `0` — `LevelEditorScene`
- `1` — `LevelScene`

## Shader format

`renderer.Shader` expects a single shader file containing sections marked with `#type`:

```glsl
#type vertex
// vertex shader source

#type fragment
// fragment shader source
```

The default shader uses:

- Position data at attribute location `0`
- Color data at attribute location `1`
- `uProjection` matrix uniform
- `uView` matrix uniform

## Development notes

- The current native dependency configuration is Windows-specific.
- `LevelEditorScene` renders a simple indexed quad and moves the camera during updates.
- `LevelScene` is currently a placeholder with an empty update method.
- JUnit 5 is configured, but no tests are currently present.
- No license has been specified for the repository.