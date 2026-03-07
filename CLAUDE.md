# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Blazor WebAssembly app that benchmarks the performance of `SpawnDev.BlazorJS` HeapView class for direct .NET-to-JavaScript memory access. It compares three interop strategies for writing image data to a canvas:

- **BuiltIn** - .NET's default byte[] serialization (creates a JS copy)
- **HeapView Copy** - Fast copy via `Uint8ClampedArray` constructor
- **HeapView Direct** - Zero-copy using `PutImageBytes` (pins .NET memory, fastest)

## Build & Run

```bash
# From solution root (where .sln is)
dotnet build HeapViewTest/HeapViewTest.csproj
dotnet run --project HeapViewTest/HeapViewTest.csproj
```

## Architecture

- **Single-project solution** targeting `net10.0` with `Microsoft.NET.Sdk.BlazorWebAssembly`
- **Key dependency:** `SpawnDev.BlazorJS` (v3.3.0) — provides `BlazorJSRuntime`, `HeapView`, and typed JS object wrappers (`HTMLCanvasElement`, `CanvasRenderingContext2D`, `Uint8Array`, `Float32Array`, etc.)
- **Program.cs** — Uses `AddBlazorJSRuntime` and `BlazorJSRunAsync()` instead of the standard Blazor host setup, enabling SpawnDev's JS interop layer
- **Pages/Home.razor** — Contains all benchmark logic; generates an 8000x8000 RGBA byte array and measures `PutImageData` performance across the three strategies
- No backend/server component — this is a standalone WASM app
