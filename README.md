# WP Local Push

Tool used to push changes to a WordPress plugin or theme that have been made on a local (staging) copy using Local (WP) to the live server using SFTP. No need to move Finder windows around anymore.  

## Requirements

- `lftp`
- `fzf`

On macOS:

```bash
brew install lftp fzf
```

## Install

### Option A: clone repo + run installer

```bash
git clone https://github.com/ffabbr/local-wp-push.git
cd wp-local-push
./install.sh
```

If `wp-local-push` isn’t found after install, add this to `~/.zshrc`:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

## First-time setup (wizard)

Run:

```bash
wp-local-push setup
```

- Create a config file at `~/.config/wp-local-push/config`
- Add an FTP login entry to `~/.netrc`
- Create an `lftp` bookmark so you can connect via a short name

## Push a theme or plugin

```bash
wp-local-push push
```

Optional:

- `--dry-run` (show what would happen)
- `--delete` (delete remote files not present locally, use carefully)
- `--exclude RX` (extra exclude pattern, repeatable)
- `--no-excludes` (push everything, dev files included)

## Excluded files

Development files are never pushed: `.git/`, `.gitignore`, `node_modules/`,
`.DS_Store`, `.env`, lock files and manifests (`composer.json`, `package.json`,
`yarn.lock`, ...), build/lint configs (`webpack.config.js`, `.eslintrc`, ...),
`tests/`, editor folders (`.vscode/`, `.idea/`) and `*.log`.

Excluded files are also left alone on the server, so `--delete` will not remove
a remote `.git` or `.env`.

See the effective list:

```bash
wp-local-push excludes
```

Patterns are extended regular expressions, matched against the path relative to
the pushed theme/plugin (directories have a trailing slash). Adjust them in
`~/.config/wp-local-push/config`:

```bash
# add to the built-in list
EXTRA_EXCLUDES=( '(^|/)assets/src/' '\.scss$' )

# or replace it entirely
# EXCLUDES=( '(^|/)\.git' )
```

## Troubleshooting

```bash
wp-local-push doctor
```

## Security notes

- Credentials are stored in `~/.netrc` and that file is set to permission `600`.
