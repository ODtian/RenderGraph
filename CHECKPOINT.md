# RenderGraph RHI rewrite checkpoint

Commit: `61dc55b74af6c0cb7b5c93b22c1db0455a034ca6`

Contents:

- `RenderGraph-source-61dc55b74af6.zip` — clean source archive from `git archive`, no build output.
- `RenderGraph-61dc55b74af6.bundle` — Git bundle containing the same commit.

Validation performed in this container:

```text
dotnet restore RenderGraph.slnx -p:Platform="Any CPU" --source <internal-nuget> --no-cache --disable-parallel
dotnet build RenderGraph.slnx -c Release -p:Platform="Any CPU" --no-restore
dotnet run -c Release --project tests/RenderGraph.Tests --no-build
dotnet run -c Release --project benchmarks/RenderGraph.Benchmarks --no-build -- --quick
```

Summary:

```text
Build: 0 warnings / 0 errors
Tests: 31/31 passed
Compiler-only 500p/1000r/~5000a: median 567.9 us, p95 828.6 us, p99 848.3 us
Stable execute 500 same-queue passes: median 5.972 us, p95 14.636 us, 0 B/frame
Stable execute 500 mixed-queue passes: median 344.860 us, p95 360.300 us, 0 B/frame
```

Limitations:

- `RenderGraph.Rhi.D3D12` references Vortice and implements the public ABI scaffold, but it is not hardware-validated in this Linux/headless container.
- Null backend validates command pool epochs, queue timeline recovery, transient bind group reservations, command-buffer-local transient tokens and repeated bind behavior.
