# mach-glfw master

This is a fork of [mach-glfw](https://github.com/slimsag/mach-glfw), but instead of targeting nominated Mach Zig versions, this one targets the Zig master branch.

## Usage

- Fetch the latest version into your project

```sh
$ # Inside your project root
$ zig fetch --save git+https://github.com/terraquad/mach-glfw-master
info: resolved to commit blah-blah-blah
```

- Include it in your build script:

```zig
// File: build.zig
const std = @import("std");

pub fn build(b: *std.Build) void {
    // snip
    // After const exe = b.addExecutable(.{ ... });
    const glfw_dep = b.dependency("mach_glfw", .{
        .target = target,
        .optimize = optimize,
    });
    exe.root_module.addImport("glfw", glfw_dep.module("mach-glfw"));
    // snip
}
```

- You're ready to go! 🎉

```zig
// File: src/main.zig
const std = @import("std");
const glfw = @import("glfw");

fn errorCallback(code: glfw.ErrorCode, desc: [:0]const u8) void {
    std.log.warn("GLFW encountered an error: {s} (code: {!})", .{desc, code});
}

pub fn main() !void {
    glfw.setErrorCallback(errorCallback);
    // Initialize GLFW
    if (!glfw.init(.{})) {
        std.log.err("Failed to initialize GLFW!", .{});
        return error.InitFailed;
    }
    defer glfw.terminate();

    // Create a window
    const window = glfw.Window.create(1280, 720, "Hello GLFW", null, null, .{}) orelse {
        std.log.err("Failed to create GLFW window!", .{});
        return error.WindowCreationFailed;
    };
    defer window.destroy();

    // Exit the loop when the window should close (e.g. by "clicking the X")
    while (!window.shouldClose()) {
        // Continously check for events like key presses or mouse movement
        glfw.pollEvents();
        if (glfw.getKey(.escape) == .press) {
            // Try to close the window (see while loop condition)
            window.setShouldClose(true);
        }
    }
}
```

## Original README

<a href="https://machengine.org/pkg/mach-glfw">
    <picture>
        <source media="(prefers-color-scheme: dark)" srcset="https://machengine.org/assets/mach/glfw-full-dark.svg">
        <img alt="mach-glfw" src="https://machengine.org/assets/mach/glfw-full-light.svg" height="150px">
    </picture>
</a>

Perfected GLFW bindings for Zig

## Features

* Zero-fuss installation, cross-compilation at the flip of a switch, and broad platform support.
* 100% API coverage. Every function, type, constant, etc. has been exposed in a ziggified API.

See also: [What does a ziggified GLFW API offer?](https://machengine.org/pkg/mach-glfw/)

## Community maintained

The [Mach engine](https://machengine.org/) project no longer uses GLFW, and so this project is now community-maintained. Pull requests are welcome and will be reviewed. The project will still target [nominated Zig versions](https://machengine.org/about/zig-version/) but may not see regular updates as it is no longer a Mach project (see [hexops/mach#1166](https://github.com/hexops/mach/issues/1166)).

Note: [hexops/glfw]()

Some old documentation is available at https://machengine.org/v0.4/pkg/mach-glfw/
