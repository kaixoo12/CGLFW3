# CGLFW3

Builds [GLFW](https://www.glfw.org) as a library to add to your Swift Package.

This package can work on its own, but it was created as a base for [SwiftGLFW](https://github.com/kaixoo12/SwiftGLFW).

## Getting Started

```swift
.package(url: "https://github.com/kaixoo12/CGLFW3.git", .from: "3.4.1")
```

## 🚀 Beware!!!

> The current build is outdated, I'm only maintaining this because [thepotatoking55](https://github.com/thepotatoking55), the original SwiftGLFW author, stopped maintaining the package.

The original CGLFW, in MacOS, set a flag that disabled Automatic Reference Counting on Skia. The use of `.unsafeFlags` is prohibited to packages in modern Swift, so we can't use this function. In MacOS, this might or might not affect the behaviour of the package - I'm a linux user, so if you wanna test the package from Mac, that would be good!!

## Cross-Platform Support

To expose platform-native functions such as `glfwGetCocoaWindow`, add the following C settings to your target:

```swift
.target(
    name: "ExampleTarget",
    dependencies: ["CGLFW3"],
    cSettings: [
        .define("GLFW_EXPOSE_NATIVE_WIN32", .when(platforms: [.windows])),
        .define("GLFW_EXPOSE_NATIVE_WGL", .when(platforms: [.windows])),
        .define("GLFW_EXPOSE_NATIVE_COCOA", .when(platforms: [.macOS])),
        .define("GLFW_EXPOSE_NATIVE_NSGL", .when(platforms: [.macOS])),
        .define("GLFW_EXPOSE_NATIVE_X11", .when(platforms: [.linux]))
    ]
)
```

I don't have a computer running Linux and Windows support for SwiftPM is rudimentary, so this will probably still take some work to get ported to non-Mac platforms.

## Hello World

A Swift translation of the "hello world" program in [GLFW's documentation](https://www.glfw.org/documentation):

```swift
import CGLFW3

func main() {
    glfwInit()
    
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4)
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 1)
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GLFW_TRUE)
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE)
    
    guard let window = glfwCreateWindow(800, 600, "Hello World", nil, nil) else {
        let error = glfwGetError(nil)
        print(error)
        return
    }

    glfwMakeContextCurrent(window)
    while glfwWindowShouldClose(window) == GLFW_FALSE {
        glfwSwapBuffers(window)
        glfwPollEvents()
    }
    
    glfwTerminate()
}
```
