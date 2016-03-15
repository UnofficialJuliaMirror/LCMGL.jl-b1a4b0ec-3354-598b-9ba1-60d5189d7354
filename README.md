# LCMGL

[![Build Status](https://travis-ci.org/rdeits/LCMGL.jl.svg?branch=master)](https://travis-ci.org/rdeits/LCMGL.jl)

This package provides Julia bindings for the [libbot lcmgl package](https://github.com/RobotLocomotion/libbot/tree/master/bot2-lcmgl), which allows OpenGL commands to be executed from a remote process using the [LCM](https://lcm-proj.github.io/) message passing system. It uses Julia's native C support to call directly into the `libbot2-lcmgl-client` library, so it should perform well with minimal overhead. 

# Usage

Construct a named LCMGL Client with:

```julia
lcmgl = LCMGLClient("client_name")
```

Multiple `LCMGL` clients can also share the same `LCM` object:

```julia
lcm = LCM()
gl1 = LCMGLClient(lcm, "gl1")
gl2 = LCMGLClient(lcm, "gl2")
```

A `do`-block syntax is also provided to make it easy to automatically construct and destroy an lcmgl client:

```julia
LCMGLClient("test") do lcmgl
    color(lcmgl, rand(4)...)
    sphere(lcmgl, rand(3), 0.1, 20, 20)
    switch_buffer(lcmgl)
end
```

## Function Names

This package attempts to provide a consistent naming scheme for exported lcmgl functions: the `bot2_lcmgl_` prefix is always removed, and suffixes that exist only to indicate the number or type of arguments have also been removed. So, for example, `bot_lcmgl_vertex2d` has become `vertex(lcmgl, x, y)` and `bot_lcmgl_vertex3d` has become `vertex(lcmgl, x, y, z)`. The only exceptions are `begin` and `end`, which are reserved keywords in Julia: they have become `begin_mode(lcmgl, mode)` and `end_mode(lcmgl)`. 
