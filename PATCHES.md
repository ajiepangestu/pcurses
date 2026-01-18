# Patches Applied to This Fork

This repository is a fork of the original [pcurses](https://github.com/schuay/pcurses) project with additional patches applied.

## Repository Information

- **Upstream**: https://github.com/schuay/pcurses
- **Fork**: https://github.com/ajiepangestu/pcurses
- **Maintainer**: ajiepangestu

## Applied Patches

### 1. Format Security Fix (2026-01-18)

**Issue**: Build fails with format security errors when compiled with `-Werror=format-security` flag (default in modern build systems).

**Error Example**:

```
cursesframe.cpp:96:9: error: format not a string literal and no format arguments [-Werror=format-security]
   96 |         mvwprintw(w_border, 0, 1, header.c_str());
      |         ^~~~~~~~~
```

**Solution**: Explicitly specify `"%s"` format string when passing dynamic strings to printf-style functions (`wprintw`, `mvwprintw`).

**Modified File**: `src/cursesframe.cpp`

**Changes**:

- Line 96: `mvwprintw(w_border, 0, 1, header.c_str())` → `mvwprintw(w_border, 0, 1, "%s", header.c_str())`
- Line 99: `mvwprintw(w_border, getmaxy(w_border) - 1, 1, footer.c_str())` → `mvwprintw(w_border, getmaxy(w_border) - 1, 1, "%s", footer.c_str())`
- Line 111: `wprintw(w_main, fitstrtowin(str).c_str())` → `wprintw(w_main, "%s", fitstrtowin(str).c_str())`
- Line 122: `mvwprintw(w_main, y, x, fitstrtowin(str, x).c_str())` → `mvwprintw(w_main, y, x, "%s", fitstrtowin(str, x).c_str())`

**Benefit**:

- Prevents potential format string vulnerabilities
- Ensures compatibility with modern GCC/Clang security flags
- Allows successful compilation on Arch Linux and other distributions with hardened build flags

**Status**: ✅ Applied

## Building

After applying patches, build using:

```bash
mkdir build
cd build
cmake ..
make
```

Or for AUR package:

```bash
makepkg -si
```

## Maintenance Notes

This fork will be maintained to ensure compatibility with Arch Linux AUR. Patches will be synced with upstream when they are merged, or maintained independently if needed.

## Contributing

If you find issues or have suggestions for this fork, please open an issue at:
https://github.com/ajiepangestu/pcurses/issues
