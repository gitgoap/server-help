# IIT Delhi Server Proxy Setup

## 1. Available Commands

| Command | Proxy | Purpose |
|---|---|---|
| `proxy03` | xen03.iitd.ac.in:3128 | Research Proxy |
| `proxy61` | proxy61.iitd.ac.in:3128 | PhD Proxy |
| `proxy21` | proxy21.iitd.ac.in:3128 | MSR Proxy |
| `proxyoff` | None | Disable Proxy |
| `proxycheck` | Current | Check Internet |

## 2. Create Bash Script

```bash
nano ~/.proxy.sh
```

Paste the following code:

```bash
#!/bin/bash

set_proxy() {
    local proxy_url="http://$1:3128"

    export http_proxy="$proxy_url"
    export https_proxy="$proxy_url"

    echo "Proxy set to: $proxy_url"
    proxycheck
}

proxy03() {
    set_proxy "xen03.iitd.ac.in"
}

proxy61() {
    set_proxy "proxy61.iitd.ac.in"
}

proxy21() {
    set_proxy "proxy21.iitd.ac.in"
}

proxyoff() {
    unset http_proxy https_proxy
    echo "Proxy disabled."
}

proxycheck() {
    if curl -fsSI --connect-timeout 3 --max-time 8 \
        https://www.google.com/ > /dev/null; then
        echo "✓ Internet connected!"
    else
        echo "✗ Internet connection failed!"
    fi
}
```

Save: `Ctrl + O` → Enter → `Ctrl + X`

## 3. Enable Permanently

```bash
echo 'source ~/.proxy.sh' >> ~/.bashrc
source ~/.bashrc
```

## 4. Usage

```bash
proxy61       # Activate PhD proxy + test connection
proxy03       # Activate Research proxy + test connection
proxy21       # Activate MSR proxy + test connection
proxycheck    # Check internet
proxyoff      # Disable proxy
```

### Expected Output

```text
Proxy set to: http://proxy61.iitd.ac.in:3128
✓ Internet connected!
```

## 5. Using with Codex

```bash
proxy61
codex
```

Codex inherits the selected proxy settings.

## 6. Script Location

```bash
~/.proxy.sh
```

Example:

```text
/home/proadhikary/.proxy.sh
```

**Note:** Use `source ~/.proxy.sh`, not `bash ~/.proxy.sh`, because proxy environment variables must remain active in the current terminal session.