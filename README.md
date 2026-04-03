# Qwen-Code Security Skills

Fork of [Trail of Bits' Claude Code skills](https://github.com/trailofbits/skills) ported to [qwen-code](https://github.com/anthropics/qwen-code) format.

73 skills total: 72 ported from Trail of Bits + 1 original (`web-pentest`).

**Tested with:**
- **qwen-code** (via Ollama, local model)
- **Platform:** macOS (Apple Silicon)

## Install

```bash
git clone https://github.com/farnaboldi/skills.git ~/.qwen/skills
```

Skills are loaded automatically by qwen-code from `~/.qwen/skills/`.

## Usage

```bash
# Automatic (model picks the skill)
qwen -i "pentest example.com" --yolo

# Explicit
qwen -i "/skills web-pentest example.com" --yolo
qwen -i "/skills supply-chain-risk-auditor"
```

