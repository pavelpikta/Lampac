# Quick Submodule Commands

## Automatic Updates
Your GitHub Actions workflows will automatically:
- Update submodules daily at 02:00 UTC
- Create pull requests with changes
- You can trigger manually from GitHub Actions tab

## Manual Commands

### Update all submodules to latest:
```bash
git submodule update --remote --merge
```

### Update specific submodule:
```bash
git submodule update --remote lampa-source
git submodule update --remote Lampac
```

### Check submodule status:
```bash
git submodule status
```

### View what changed in submodules:
```bash
git diff HEAD
git diff --name-only
```

### Initialize and clone submodules (for new checkout):
```bash
git submodule init
git submodule update
```

### Update and initialize in one command:
```bash
git submodule update --init --recursive
```

## Current Submodules:
- `lampa-source` (https://github.com/yumata/lampa-source)
- `Lampac` (https://github.com/immisterio/Lampac)