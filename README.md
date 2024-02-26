# libdi component/module/package specification

[TOC]

version 0.0.0

## Overview

- Component: single function, and its context initializer/finalizer/data/dependencies
- Module: multiple internal component and one export component, module ID
- Package: file including multiple modules

Function consisting component function body accepts context and extra custom parameters.

Context initializer accepts data and array of dependencies, and returns `void *` context.

Context finalizer accepts context and returns nothing.

dependencies are in format below:

```c
typedef struct di {
    void *context;
    void *function; // cast to appropriate function pointer type
} di_t;
```

## File structure

All `u16`, `u32`, ... is stored in little-endian order.

### Component

- 0..4: `u32` size of function body
- 4..8: `u32` size of initializer function
- 8..12: `u32` size of finalizer function
- 12..16: `u32` size of data
- 16..18: `u16` number of dependencies for initializer function
- ...: dependencies
  - 0..1: `u8` length of module ID, or 255 for component in same module
  - ...: multiple `u8` for module ID, or `u16` index of component
- ...: function body, initializer/finalizer function, data

### Module

- 0..2: `u16` number of components
- 2..4: `u16` index of export component
- ...: components

### Package

- 0..2: `u16` number of modules
- ...: modules
