# Qwen-Code Security Skills

Port of [Trail of Bits' Claude Code skills](https://github.com/trailofbits/skills) to [qwen-code](https://github.com/anthropics/qwen-code) format, plus original pentest skills.

73 skills total: 72 ported from Trail of Bits + 1 original (`web-pentest`).

## Quick Install

```bash
# 1. Clone this repo
git clone https://github.com/farnaboldi/skills.git ~/skills
cd ~/skills && git checkout qwen-code-port

# 2. Create ~/.qwen/skills/ if it doesn't exist
mkdir -p ~/.qwen/skills

# 3. Symlink all skills
for dir in ~/skills/qwen-code/skills/*/; do
  name=$(basename "$dir")
  ln -sf "$dir" ~/.qwen/skills/"$name"
done

# 4. Verify
ls ~/.qwen/skills/ | wc -l  # Should show 73
```

## Install Pentest Tools

The `web-pentest` skill uses these CLI tools. Install the ones you need:

### Required (Core)

```bash
# Already on most systems
brew install nmap nikto curl jq

# Subdomain enumeration
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
go install -v github.com/owasp-amass/amass/v4/...@master

# Live host detection & tech fingerprinting
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest

# Vulnerability scanning
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
nuclei -update-templates

# SQL injection
pipx install sqlmap
```

### Recommended (Content Discovery & Crawling)

```bash
# Directory brute-forcing (pick at least one)
go install github.com/ffuf/ffuf/v2@latest
go install github.com/OJ/gobuster/v3@latest
brew install feroxbuster

# Web crawling
go install github.com/projectdiscovery/katana/cmd/katana@latest
go install github.com/hakluke/hakrawler@latest
```

### Optional (Specialized)

```bash
# WAF detection
pipx install wafw00f

# SSL/TLS analysis
brew install testssl
pipx install sslyze

# Web technology fingerprinting
git clone https://github.com/urbanadventurer/WhatWeb.git /opt/homebrew/opt/whatweb
ln -sf /opt/homebrew/opt/whatweb/whatweb /opt/homebrew/bin/whatweb

# XSS testing
go install github.com/hahwul/dalfox/v2@latest

# OSINT
pipx install theHarvester
```

### One-Liner: Install Everything

```bash
# Homebrew
brew install nmap nikto curl jq feroxbuster testssl

# Go tools
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest && \
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest && \
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest && \
go install -v github.com/projectdiscovery/katana/cmd/katana@latest && \
go install -v github.com/owasp-amass/amass/v4/...@master && \
go install github.com/ffuf/ffuf/v2@latest && \
go install github.com/OJ/gobuster/v3@latest && \
go install github.com/hakluke/hakrawler@latest && \
go install github.com/hahwul/dalfox/v2@latest

# Python tools (via pipx to avoid PEP 668 issues)
pipx install wafw00f && \
pipx install sslyze && \
pipx install sqlmap && \
pipx install theHarvester

# Nuclei templates
nuclei -update-templates

# WhatWeb (from source)
git clone https://github.com/urbanadventurer/WhatWeb.git /opt/homebrew/opt/whatweb && \
ln -sf /opt/homebrew/opt/whatweb/whatweb /opt/homebrew/bin/whatweb
```

### PATH Setup

Add to your `~/.zshrc` (or `~/.bashrc`):

```bash
# Go and pipx binaries (pentest tools)
export PATH="$HOME/go/bin:$HOME/.local/bin:$PATH"
```

Then `source ~/.zshrc`.

### Wordlists

```bash
# SecLists - essential wordlists for content discovery
git clone https://github.com/danielmiessler/SecLists.git /usr/share/seclists
```

## Verify Installation

```bash
# Check all pentest tools
for tool in whois dig subfinder amass theHarvester curl jq \
            nmap httpx whatweb wafw00f \
            ffuf gobuster feroxbuster katana hakrawler \
            nuclei nikto testssl.sh sslyze \
            sqlmap dalfox; do
  which $tool > /dev/null 2>&1 && \
    echo "[+] $tool: available" || \
    echo "[-] $tool: NOT FOUND"
done
```

## Usage

Skills are loaded automatically by qwen-code from `~/.qwen/skills/`.

### Automatic (model picks the skill based on your request)

```bash
qwen -i "pentest example.com" --yolo
qwen -i "audit this project's dependencies"
qwen -i "review this PR for security issues"
```

### Explicit invocation

```bash
qwen -i "/skills web-pentest example.com" --yolo
qwen -i "/skills supply-chain-risk-auditor"
qwen -i "/skills differential-review"
```

### Project-level hint (QWEN.md)

For better auto-detection with local models, create a `QWEN.md` in your project directory:

```markdown
# Pentest Project

When asked to pentest or scan, invoke `/skills web-pentest` first.
```

## Available Skills (73)

### Web Application Pentesting (Original)
| Skill | Description |
|-------|-------------|
| `web-pentest` | Structured 7-phase web app pentest methodology |

### Code Auditing (Ported from ToB)
| Skill | Description |
|-------|-------------|
| `differential-review` | Security-focused code review for PRs/commits/diffs |
| `audit-context-building` | Ultra-granular code analysis for security audits |
| `fp-check` | False positive verification for security bugs |
| `insecure-defaults` | Detect fail-open config and hardcoded secrets |
| `sharp-edges` | Identify error-prone APIs and dangerous patterns |
| `variant-analysis` | Find similar vulnerabilities across codebases |
| `supply-chain-risk-auditor` | Audit dependency risk for supply chain attacks |
| `semgrep-rule-creator` | Create custom Semgrep rules |
| `semgrep-rule-variant-creator` | Port Semgrep rules to new languages |

### Static Analysis (Ported from ToB)
| Skill | Description |
|-------|-------------|
| `codeql` | CodeQL database creation and analysis |
| `semgrep` | Semgrep scanning guidance |
| `sarif-parsing` | Parse and analyze SARIF results |

### Verification & Testing (Ported from ToB)
| Skill | Description |
|-------|-------------|
| `constant-time-analysis` | Detect timing side-channels in crypto code |
| `zeroize-audit` | Detect missing zeroization of secrets |
| `mutation-testing` | Mutation testing campaigns |
| `property-based-testing` | Property-based testing guidance |
| `coverage-analysis` | Code coverage analysis |

### Fuzzing (Ported from ToB)
| Skill | Description |
|-------|-------------|
| `aflpp` | AFL++ fuzzing |
| `libfuzzer` | libFuzzer harness writing |
| `cargo-fuzz` | Rust fuzzing with cargo-fuzz |
| `atheris` | Python fuzzing with Atheris |
| `ruzzy` | Ruby fuzzing with Ruzzy |
| `libafl` | LibAFL fuzzing framework |
| `harness-writing` | Fuzzing harness development |
| `fuzzing-dictionary` | Fuzzing dictionary creation |
| `fuzzing-obstacles` | Overcome fuzzing blockers |
| `ossfuzz` | OSS-Fuzz integration |

### Smart Contract Security (Ported from ToB)
| Skill | Description |
|-------|-------------|
| `algorand-vulnerability-scanner` | Algorand contract scanning |
| `cairo-vulnerability-scanner` | Cairo/StarkNet scanning |
| `cosmos-vulnerability-scanner` | Cosmos SDK scanning |
| `solana-vulnerability-scanner` | Solana program scanning |
| `substrate-vulnerability-scanner` | Substrate pallet scanning |
| `ton-vulnerability-scanner` | TON contract scanning |
| `token-integration-analyzer` | Token integration analysis |
| `entry-point-analyzer` | Smart contract entry point mapping |

### Malware Analysis (Ported from ToB)
| Skill | Description |
|-------|-------------|
| `yara-rule-authoring` | YARA detection rule writing |

### Mobile Security (Ported from ToB)
| Skill | Description |
|-------|-------------|
| `firebase-apk-scanner` | Firebase config security in Android APKs |

See `qwen-code/skills/` for the full list of all 73 skills.

## Credits

- Original skills by [Trail of Bits](https://github.com/trailofbits/skills) under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
- Qwen-code port and `web-pentest` skill by [@farnaboldi](https://github.com/farnaboldi)
