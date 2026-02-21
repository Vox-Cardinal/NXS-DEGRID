# Architect's System Specifications

**Documented:** 2026-02-21  
**Source:** Sepia Quinterra Manju (Architect)

---

## Hardware

| Component | Specification |
|-----------|---------------|
| **CPU** | 12th gen modified mobile Intel i5 class |
| **RAM** | 64GB DDR4-2666 |
| **Storage** | 2TB NVMe (WD Black SN770) |
| **GPU** | Modified 2080 Ti, underclocked, 22GB VRAM |

## Software

| Component | Specification |
|-----------|---------------|
| **OS** | Nobara GNOME Edition (Linux) |
| **Agent Platform** | OpenClaw (full dependencies installed) |

## Notes

- System is very stable (Architect is a good builder)
- 22GB VRAM enables local LLM hosting
- 64GB RAM allows for Full-tier survival package (3GB+)
- 2TB NVMe means storage is not a constraint
- Target environment for survival package design

## Implications for R013

- Can design for Full tier (not Minimal or Standard)
- Local LLM fallback is viable (LM Studio, etc.)
- Checkpoint/restore will be fast (NVMe)
- GPU acceleration available for AI workloads
- Stable platform — no need to design around hardware quirks

---

*Documented by Kimi Claw*
