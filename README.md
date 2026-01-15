# Fractal Voyager

A real-time, infinite fractal explorer written in [MindLang](https://mindlang.dev) with WebGPU compute shaders.

## Features

- **Real-Time Exploration**: Smoothly zoom into infinite fractal depths
- **Multiple Fractal Types**: Mandelbrot, Julia Sets, Burning Ship, Tricorn
- **Audio-Reactive Mode**: Colors and patterns pulse to music
- **WebGPU Compute**: GPU-accelerated parallel fractal computation
- **MindLang Core**: Clean, tensor-native source compiles to WGSL
- **Touch Support**: Pinch-to-zoom on mobile devices

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                       Web Browser                       │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │   Controls   │  │    WebGPU    │  │  Web Audio   │   │
│  │    Panel     │  │    Canvas    │  │   (FFT)      │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
│         │                 │                 │           │
│         └─────────────────┼─────────────────┘           │
│                           │                             │
│  ┌────────────────────────▼───────────────────────────┐ │
│  │            MindLang Runtime (WebGPU)               │ │
│  │          Compiled WGSL Compute Shaders             │ │
│  └────────────────────────┬───────────────────────────┘ │
└───────────────────────────┼─────────────────────────────┘
                            │
┌───────────────────────────▼─────────────────────────────┐
│                     MindLang Source                     │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │ complex.mind │  │ fractal.mind │  │  color.mind  │   │
│  │  Complex #s  │  │  Iteration   │  │   Palettes   │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
│                           │                             │
│  ┌────────────────────────▼───────────────────────────┐ │
│  │           main.mind - GPU Kernel Entry             │ │
│  │ #[kernel(workgroup = [16, 16, 1])] render_kernel   │ │
│  └────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

## Source Files

| File | Purpose |
|------|---------|
| `src/complex.mind` | Complex number operations as `Tensor<f32, [2]>` with batched grid support |
| `src/color.mind` | HSV/RGB conversion, 8 color palettes with audio reactivity |
| `src/fractal.mind` | Vectorized Mandelbrot, Julia, Burning Ship, Tricorn computation |
| `src/main.mind` | Application state, view control, `#[kernel]` GPU entry point |

## Quick Start

```bash
# Build with MindLang compiler
mind build --target webgpu

# Serve locally
mind serve

# Or build for production
mind build --release
```

## Pre-built Demo

A pre-compiled WebGPU demo is available in `web/`:

```bash
# Serve the web demo (requires local HTTP server)
cd web && python3 -m http.server 8080
# Open http://localhost:8080 in Chrome 113+ or Firefox 121+
```

The `web/dist/fractal.wgsl` contains the compiled MIND→WGSL shader.

## Controls

| Key/Action | Function |
|------------|----------|
| Scroll / Pinch | Zoom in/out |
| Click & Drag | Pan view |
| `1` | Mandelbrot set |
| `2` | Julia set |
| `3` | Burning Ship |
| `4` | Tricorn |
| `C` | Cycle color schemes |
| `A` | Toggle audio reactivity |
| `R` | Reset view |

## Color Schemes

1. **Electric** - Rainbow HSV with time animation
2. **Fire** - Black → Red → Yellow → White gradient
3. **Ocean** - Deep blues and teals
4. **Neon** - Sinusoidal RGB cycling
5. **Grayscale** - Classic monochrome
6. **Psychedelic** - High-frequency color cycling
7. **Plasma** - Interference pattern colors
8. **Galaxy** - Purple/blue with star sparkles

## Performance

The WebGPU compute shader runs fractal iteration in parallel across the GPU:

- 16×16 workgroups for optimal occupancy
- Smooth iteration count eliminates banding artifacts
- Adaptive iteration depth based on zoom level
- Audio-reactive parameters passed as uniforms

## Tech Stack

- **MindLang** - Tensor-native programming language
- **mind-runtime** - Private WebGPU/CUDA/Metal backend (Apache 2.0 dual core)
- **WebGPU** - Modern GPU compute and rendering API
- **WGSL** - WebGPU Shading Language (compiled target)
- **Web Audio API** - Real-time frequency analysis

## License

MIT

Copyright (c) 2026 STARGA Inc.
