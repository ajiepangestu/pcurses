# pcurses

A curses-based frontend for libalpm (Arch Linux Package Manager).

## About This Fork

This is a patched fork of the original pcurses project with security fixes applied. For detailed information about the applied patches, see [PATCHES.md](PATCHES.md).

- **Upstream**: https://github.com/schuay/pcurses
- **Fork**: https://github.com/ajiepangestu/pcurses

## Features

- Browse and search packages in a terminal user interface
- Regular expression filtering and searching across any package property
- Customizable color-coding and sorting
- Package queue management
- External command execution with package list substitution
- User-defined macros and hotkeys
- Input history navigation

## Installation

### Arch Linux (AUR)

```bash
# Using yay
yay -S pcurses-git

# Using paru
paru -S pcurses-git
```

### Building from Source

```bash
git clone https://github.com/ajiepangestu/pcurses.git
cd pcurses
mkdir build
cd build
cmake ..
make
sudo make install
```

## Usage

### Starting pcurses

```bash
pcurses
```

### Navigation

**Package List Navigation:**

- **Up/k**: Move up one item
- **Down/j**: Move down one item
- **Page Up**: Scroll up by page
- **Page Down**: Scroll down by page
- **Home**: Jump to first item
- **End**: Jump to last item

**Focus and Queue:**

- **Tab**: Switch focus between package list and queue
- **Right arrow**: Add selected package to queue
- **Left arrow**: Remove selected package from queue
- **C** (Shift+c): Clear entire queue

**General:**

- **h**: Display help screen
- **q**: Quit pcurses
- **r**: Reload package database
- **c**: Clear all filters

**Hotkeys:**

- **0-9**: Execute user-defined macros (if configured)

### Filtering

Filtering syntax: `[fields][!][:][!]search_phrase`

**Field Specifiers**:
Field specifiers are highlighted characters in the field names (visible in the info pane):

- `n` - Name
- `d` - Description
- `r` - Repository
- `v` - Version
- `b` - Build date
- etc.

**Examples**:

```
game                    Search "game" in all fields
nd:game                 Search "game" in Name and Description
nd!:game                Search "game" in fields other than Name and Description
/nd!:gnome              Exclude packages matching "gnome" in Name/Description
n:^a                    Packages starting with 'a'
b:2010                  Packages with build date in 2010
```

**Filter Operations**:

- `/`: Enter filter mode
- `n`: Quick filter by name (equivalent to `/n:`)
- `d`: Quick filter by description (equivalent to `/d:`)
- `?`: Enter search mode (find and jump to package)
- `c`: Clear all filters
- **Up/Down arrows** (in input mode): Navigate filter history
- **Chaining**: Multiple filters can be applied sequentially

**Search Behavior**:

- Alphanumeric-only phrases: Fast string search
- Special characters: Regular expression search (case-insensitive)

### Sorting and Color-Coding

- `.`: Sort by field (followed by field specifier, e.g., `.n` for name)
- `;`: Color-code by field (followed by field specifier, e.g., `;r` for repository)

### Command Execution

Execute external commands using the package queue:

```
!sudo pacman -S %p      Install queued packages
!sudo pacman -Rs %p     Remove queued packages
!pacman -Sy             Refresh package database
```

- `!`: Enter command execution mode
- `%p`: Token replaced by space-separated list of queued packages
- `r`: Reload package information after database changes

### Macros

Define macros in `/etc/pcurses.conf`:

```
macroname=command
```

**Examples**:

```
sortbyname=.n
colorbyrepo=;r
filterupdates=/d:update available
chainedmacro=@sortbyname,@colorbyrepo,@filterupdates
startup=@sortbyname,@colorbyrepo
```

**Macro Features**:

- **Chaining**: Separate multiple macros with commas
- **Startup macro**: Define `startup` macro to run on application start
- **Hotkeys**: Define macros named `1`, `2`, etc., triggered by pressing the corresponding key
- **Execution**: Press `@` to execute a macro by name

### Control Commands

Control commands can be executed directly by pressing `%` followed by the command name, or used within macros:

- `scroll_up`, `scroll_down`
- `scroll_home`, `scroll_end`
- `scroll_pageup`, `scroll_pagedown`
- `switch_focus`
- `queue_push`, `queue_pop`, `queue_clear`
- `help`, `quit`, `reload`, `filter_clear`

## Configuration

Configuration file: `/etc/pcurses.conf`

## Dependencies

- ncurses
- pacman (libalpm)
- boost (build-time)
- cmake (build-time)

## Contributing

Contributions are welcome. Please report bugs and feature requests to:
https://github.com/ajiepangestu/pcurses/issues

## License

This project is licensed under the GPL-2.0 License. See the [COPYING](COPYING) file for details.

## Authors

- **Original Author**: Jakob Gruber <jakob.gruber@gmail.com>
- **Fork Maintainer**: Ajie Pangestu <paangestu@gmail.com>

## Acknowledgments

This project is a fork of the original [pcurses](https://github.com/schuay/pcurses) by Jakob Gruber, with patches applied to ensure compatibility with modern build systems and security requirements.

## See Also

- [CONCEPT](CONCEPT) - Design concepts and planned features
- [PATCHES.md](PATCHES.md) - Detailed patch information
