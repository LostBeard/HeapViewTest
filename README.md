# HeapViewTest

A Blazor WebAssembly benchmark app that measures the performance of [SpawnDev.BlazorJS](https://github.com/LostBeard/SpawnDev.BlazorJS) HeapView for direct .NET-to-JavaScript memory interop.

## What it tests

The app compares three strategies for transferring a large RGBA byte array from .NET to a JavaScript canvas via `PutImageData`:

| Method | Description |
|---|---|
| **.NET Built-in** | Default `byte[]` serialization — creates a full copy in JS memory |
| **HeapView Copy** | Fast copy via `Uint8ClampedArray` constructor from pinned .NET heap |
| **HeapView Direct** | Zero-copy — pins .NET memory and passes it directly to `PutImageData` |

HeapView provides a JavaScript `TypedArray` view directly into the .NET WebAssembly linear memory, eliminating unnecessary data copies during interop calls.

## Running

Requires [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0).

```bash
dotnet run --project HeapViewTest/HeapViewTest.csproj
```

The app will be available at the URL shown in the console (typically `https://localhost:5001`).

## Configuration

The benchmark UI lets you configure:
- **Image size** — 2000x2000 (16 MB) up to 8000x8000 (256 MB)
- **Iterations** — 3, 5, 10, or 20 runs per method

Results include per-method summary cards (avg/min/max time, throughput), a comparison bar chart, and a detailed iteration table.

## Links

- [SpawnDev.BlazorJS](https://github.com/LostBeard/SpawnDev.BlazorJS) — Full Blazor WebAssembly JS interop library
- [HeapViewTest](https://github.com/LostBeard/HeapViewTest) — This repository

## License

See [LICENSE.txt](LICENSE.txt).
