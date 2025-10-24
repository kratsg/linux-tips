# ATLAS Linux Tips Fortune File

A curated collection of Linux command tips specifically tailored for ATLAS physicists working at the UChicago Analysis Facility. These tips cover essential commands and workflows for data analysis, remote work, file management, and system monitoring.

## Features

- **300+ practical tips** covering everyday Linux usage
- **ATLAS-specific context** with examples using `.root` files, `lxplus.cern.ch`, and analysis workflows
- **Beginner to advanced** topics including:
  - File viewing and manipulation (`cat`, `less`, `grep`, `find`)
  - Compression and archives (`tar`, `gzip`, `rsync`)
  - Remote work (`ssh`, `scp`, `screen`, `tmux`)
  - Process management (`ps`, `top`, `kill`, `nice`, `nohup`)
  - Text processing (`sed`, `awk`, `cut`, `sort`)
  - Bash scripting tips and best practices
  - Performance monitoring and debugging
  - Development tools (`gcc`, `g++`, `make`, `cmake`, `gdb`)
  - Version control (`git`, `svn`)
  - Programming languages (`python`, `perl`)
  - Code analysis and navigation (`nm`, `objdump`, `readelf`, `ctags`, `cscope`)

## Installation

### Option 1: Using Pixi (Recommended)

If you have [pixi](https://pixi.sh) installed:

```bash
# Install dependencies (Python, pnu-strfile, fortune)
pixi install

# Generate the fortune index file
pixi run build

# Test it works
pixi run test
```

This uses the Python-based `pnu-strfile` and `fortune` packages, which work cross-platform (Linux, macOS, etc.).

### Option 2: System Installation

1. **Install fortune (if not already installed)**:
   ```bash
   # Ubuntu/Debian
   sudo apt-get install fortune-mod

   # RHEL/CentOS/AlmaLinux
   sudo yum install fortune-mod

   # Fedora
   sudo dnf install fortune-mod
   ```

2. **Copy the fortune file to the fortune directory**:
   ```bash
   sudo cp atlas-tips /usr/share/games/fortunes/
   ```

3. **Generate the index file**:
   ```bash
   sudo strfile /usr/share/games/fortunes/atlas-tips /usr/share/games/fortunes/atlas-tips.dat
   ```

### Option 3: User-Local Installation

If you don't have sudo access:

1. **Create a local fortune directory**:
   ```bash
   mkdir -p ~/.local/share/fortunes
   ```

2. **Copy the fortune file**:
   ```bash
   cp atlas-tips ~/.local/share/fortunes/
   ```

3. **Generate the index file**:
   ```bash
   strfile ~/.local/share/fortunes/atlas-tips ~/.local/share/fortunes/atlas-tips.dat
   ```

4. **Use it with**:
   ```bash
   fortune ~/.local/share/fortunes/atlas-tips
   ```

## Usage

### Display a Random Tip

```bash
fortune atlas-tips
```

### Add to MOTD (Message of the Day)

To display a random tip when users log in, add to `/etc/motd` or create a script in `/etc/update-motd.d/`:

**Option 1: Static MOTD**
```bash
echo "" | sudo tee -a /etc/motd
echo "=== Linux Tip of the Day ===" | sudo tee -a /etc/motd
fortune atlas-tips | sudo tee -a /etc/motd
echo "" | sudo tee -a /etc/motd
```

**Option 2: Dynamic MOTD (recommended)**

Create `/etc/update-motd.d/90-atlas-tips`:
```bash
#!/bin/sh
echo ""
echo "=== Linux Tip of the Day ==="
fortune atlas-tips
echo ""
```

Make it executable:
```bash
sudo chmod +x /etc/update-motd.d/90-atlas-tips
```

**Option 3: Add to user's login script**

Add to `~/.bashrc` or `~/.bash_profile`:
```bash
# Display a Linux tip on login
if command -v fortune &> /dev/null; then
    echo ""
    echo "=== Linux Tip of the Day ==="
    fortune ~/.local/share/fortunes/atlas-tips 2>/dev/null || fortune atlas-tips 2>/dev/null || true
    echo ""
fi
```

### Display All Tips

```bash
fortune -a atlas-tips | less
```

### Display Longer Tips

```bash
fortune -l atlas-tips
```

### Display Shorter Tips

```bash
fortune -s atlas-tips
```

### Mix with Other Fortune Files

```bash
fortune 50% atlas-tips 50% computers
```

## Examples

Here are a few sample tips from the collection:

```
The tail -f command is invaluable for monitoring running jobs in real-time.

   tail -f batch_job.log
   tail -f nohup.out | grep ERROR
```

```
Use screen to maintain persistent sessions on remote machines.

   screen                                   # Start new session
   screen -S analysis                       # Start named session
   Ctrl+a, d                                # Detach from session
   screen -r                                # Reattach to session
   screen -ls                               # List all sessions
```

```
Synchronize files and directories efficiently with rsync.

   rsync -avz local/ user@remote:~/backup/  # Sync to remote, compressed
   rsync -avz --delete src/ dest/           # Mirror directories (delete extra files)
   rsync -av --progress large.root remote:/ # Show transfer progress
```

## Tips Format

Each tip follows this format:
- **2-3 lines** of explanatory text
- **1-2 code examples** with inline comments
- **ATLAS-relevant** examples using familiar filenames and servers

## Customization

You can easily add your own tips! Just edit the `atlas-tips` file:

1. Add your tip text
2. Add code examples (indented with 3 spaces)
3. End with a line containing only `%`
4. Regenerate the index: `pixi run build` or `strfile atlas-tips atlas-tips.dat`

Example:
```
Your helpful description here. Keep it concise and practical.

   command --option argument                # What this does
   another-command example                  # Another example
%
```

## Development

```bash
# Install dependencies
pixi install

# Build the fortune database
pixi run build

# Test the fortune file
pixi run test

# Clean generated files
pixi run clean
```

## Contributing

To add new tips or improve existing ones:

1. Edit the `atlas-tips` file
2. Ensure tips are relevant to ATLAS physics analysis workflows
3. Keep the format consistent (2-3 lines + 1-2 examples)
4. Avoid editor-specific tips (vim/emacs/nano wars)
5. Avoid sysadmin-only commands not useful for regular users
6. Regenerate the index file: `pixi run build`

## Files

- `atlas-tips` - The fortune file containing all tips
- `atlas-tips.dat` - Generated index file (created by strfile)
- `pixi.toml` - Pixi project configuration
- `README.md` - This file

## License

This collection is provided as-is for educational purposes to help ATLAS physicists be more productive with Linux command-line tools.

## Acknowledgments

Tips compiled and curated from various Linux documentation sources, tailored specifically for the ATLAS physics analysis environment at UChicago Analysis Facility.

## Additional Resources

- https://github.com/trinib/Linux-Bash-Commands/
- https://linux-commands.labex.io/

---

**Tip**: Run `fortune atlas-tips | cowsay` if you have `cowsay` installed for extra fun! 🐮
