# modl-registry

Model registry for [modl](https://github.com/modl-org/modl) — the CLI model manager for AI image generation.

**[Website](https://modl.run)** · **[Browse Models](https://modl.run/models)** · **[CLI Docs](https://modl.run/docs)** · **[CLI Repo](https://github.com/modl-org/modl)**

This repository contains YAML manifest files that describe models, LoRAs, VAEs, text encoders, recipes, and other assets. The `modl` CLI fetches a compiled index from this registry to know what's available and how to download it.

## Current Registry

| Type | Count | Examples |
|------|-------|---------|
| Checkpoints | 8 | FLUX.1 Dev, FLUX.1 Schnell, SDXL Base, SD 1.5 |
| Diffusion Models | 17 | FLUX Kontext, Z-Image Turbo, Qwen Image, WAN 2.2 |
| LoRAs | 4 | LCM LoRA (SDXL/SD1.5), Z-Image Distill, Qwen Edit Lightning |
| VAEs | 9 | Flux VAE, SDXL VAE, SD VAE, WAN VAE |
| Text Encoders | 10 | T5-XXL, CLIP-L, Flux2 Mistral/Qwen3, UMT5-XXL |
| ControlNets | 6 | FLUX Depth, SD1.5 Canny/Depth/OpenPose, SDXL Canny |
| IP-Adapters | 3 | FLUX Redux, SDXL IP-Adapter, FaceID |
| Upscalers | 3 | 4x UltraSharp, BSRGANx2, RealESRGAN |
| Segmentation | 1 | BiRefNet DIS |
| **Recipes** | **4** | **Flux Face, Flux Style, Flux Product, Z-Image Face** |

### Cloud-Available Models

These models are available on [modl cloud](https://modl.run/docs/cloud) for training and inference:

| Model | Training GPU | Est. Time | Inference GPU |
|-------|-------------|-----------|---------------|
| FLUX.1 Schnell | A100 80GB | ~10 min | A10G |
| FLUX.1 Dev | A100 80GB | ~25 min | A100 80GB |
| Z-Image Turbo | A100 80GB | ~10 min | A10G |
| Qwen Image | A100 80GB | ~15 min | A100 80GB |

## Recipes

Recipes are training presets — shareable, version-controlled best practices for LoRA training. Instead of guessing parameters, pick a recipe:

```bash
modl train --recipe flux-face-lora --dataset ./photos
modl train --recipe flux-product-lora --dataset ./products
```

Each recipe specifies:
- **Base model** and architecture
- **Training parameters** — steps, learning rate, rank, optimizer, resolution
- **Generation defaults** — steps, CFG, sampler for using the trained LoRA
- **Dataset guidance** — min/max images, caption strategy
- **Tips** — practical advice from real usage

Recipes live in `manifests/recipes/` and are first-class registry items.

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to submit your own recipes.

## For Users

You don't interact with this repo directly. Just use the `modl` CLI:

```bash
modl update              # Fetch latest index from this registry
modl search flux         # Search available models
modl install flux-dev    # Install a model
modl info flux-dev       # View model details, cloud availability
```

## Curation Policy

We curate official model weights, functional LoRAs (distillation, editing,
conditioning), and adherence tools — always with pinned hashes and clear
provenance. We do not curate content LoRAs (styles, characters, NSFW);
those can still be installed directly from CivitAI or HuggingFace URLs.

## For Contributors

Want to add a model or recipe? See [CONTRIBUTING.md](CONTRIBUTING.md).

```bash
# Quick start
git clone https://github.com/modl-org/modl-registry
cd modl-registry
pip install pyyaml

# Add your manifest
cp manifests/checkpoints/flux-dev.yaml manifests/checkpoints/my-model.yaml
# Edit the file...

# Validate
python scripts/validate.py manifests/checkpoints/my-model.yaml
python scripts/build_index.py
```

## Schema

All manifests are validated against [manifest.schema.json](schemas/manifest.schema.json).

## License

MIT
