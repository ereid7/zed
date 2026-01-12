# Build Instructions for Git Panel Multi-Select Feature

This branch implements GitHub issue #26430 - multi-select files in Git panel with bulk operations.

## Prerequisites

### macOS
1. Install Xcode from the App Store or [Apple Developer](https://developer.apple.com/download/all/)
2. Install Xcode Command Line Tools:
   ```bash
   xcode-select --install
   ```
3. Set Xcode path:
   ```bash
   sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
   sudo xcodebuild -license accept
   ```
4. Install Rust:
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   source ~/.cargo/env
   ```
5. Install CMake:
   ```bash
   brew install cmake
   ```

### Known Build Issue
If you encounter a `webrtc-sys` C++ compilation error about `contiguous_range` or `pointer_traits`, try one of these:

1. **Use Command Line Tools instead of Xcode:**
   ```bash
   sudo xcode-select -s /Library/Developer/CommandLineTools
   ```

2. **Install Xcode 16** from Apple Developer downloads

3. **Downgrade to Xcode 15.2 or earlier**

## Building

```bash
# Clone and checkout the branch
git fetch origin
git checkout feature/git-panel-multiselect

# Clean any previous build artifacts (if needed)
rm -rf target/

# Build release version
cargo build --release -p zed

# Run Zed
./target/release/Zed
```

## Testing the Feature

1. Open a git repository in Zed
2. Open the Git Panel (View > Git Panel or the git icon in the sidebar)
3. Test the new multi-select functionality:
   - **Stage/Unstage Range:** Click a file to stage/unstage it (sets anchor), then Shift+click another file to stage/unstage the range
   - **Restore Range:** Click a file and press Backspace/Delete to restore it (sets anchor), then Shift+Backspace or Shift+Delete to restore a range
   - **Vim mode:** Use `g d` to restore range

## What Changed

- `crates/git/src/git.rs` - Added `UnstageRange` and `RestoreRange` actions
- `crates/git_ui/src/git_panel.rs` - Main implementation
- `assets/keymaps/default-*.json` - Keybindings for all platforms
- `assets/keymaps/vim.json` - Vim keybinding (`g d`)
