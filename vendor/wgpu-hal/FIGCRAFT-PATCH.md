# FigCraft patch to wgpu-hal 30.0.1

Source: https://crates.io/crates/wgpu-hal/30.0.1 (MIT OR Apache-2.0; licenses are alongside this file).

FigCraft changes one line in `src/dx12/mod.rs`: the `DXGI_SWAP_CHAIN_DESC1`
created for a surface uses `DXGI_SCALING_NONE` instead of
`DXGI_SCALING_STRETCH`. No other upstream source is changed. The Cargo
`[patch.crates-io]` entry in the workspace manifest applies this to the
existing wgpu dependency graph.

On the affected Windows system, `DXGI_SCALING_STRETCH` resamples FigCraft's
windowed UI even though the renderer's framebuffer is sharp. With `NONE`,
the presented window and framebuffer match pixel for pixel at 1440×900 and
1000×750, including after restoring from a maximized window. See FigCraft
issue #778 for the reproduction and measurements.

Remove this vendor copy when a released wgpu-hal includes the fix and FigCraft
upgrades to it.
