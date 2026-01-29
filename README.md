# claude-sneakpeek

Get a parallel build of Claude Code that unlocks feature-flagged capabilities like swarm mode.

Demo video of swarm mode in action: https://x.com/NicerInPerson/status/2014989679796347375

This installs a completely isolated instance of Claude Code—separate config, sessions, MCP servers, and credentials. Your existing Claude Code installation is untouched.

## Install

Note: tweakcc (the Claude Code theming/patching tool) may fail when the installed Claude Code version is newer than the published tweakcc npm release. If you see "tweakcc failed." during `quick`/`update`, either use the `--no-tweak` flag or build tweakcc from source and link it locally as described below.



```bash
npx @realmikekelly/claude-sneakpeek quick --name claudesp
```

Add `~/.local/bin` to your PATH if not already (macOS/Linux):

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
```

Then run `claudesp` to launch.

## What gets unlocked?

Local tweakcc installation

If the tweakcc npm release doesn't yet support the Claude Code version installed by claude-sneakpeek, you can clone and build tweakcc locally, then link it into claude-sneakpeek before running `quick`:

```bash
# From the parent directory of claude-sneakpeek
git clone https://github.com/Piebald-AI/tweakcc.git
cd tweakcc
npm install --legacy-peer-deps
npm run build
# In claude-sneakpeek repo
# Update package.json: set "tweakcc": "file:../tweakcc"
npm install
npm run bundle
npx @realmikekelly/claude-sneakpeek quick --name <variant>
```

This approach uses the prompt files included in the tweakcc repo (including newer prompts not yet published to npm) and allows tweakcc to patch newer Claude Code versions.

History of local fix applied in this repo:

- Cloned https://github.com/Piebald-AI/tweakcc and built locally (npm install --legacy-peer-deps && npm run build)
- Linked the local tweakcc by changing `package.json` to `"tweakcc": "file:../tweakcc"`
- Adjusted src/core/tweakcc.ts to resolve tweakcc's entrypoint (try dist/index.mjs then dist/index.js)
- Bundled and created variant `claude3` successfully with tweakcc applied

Commands run during repro and fix (example):

```bash
# clone and build tweakcc
git clone https://github.com/Piebald-AI/tweakcc.git
cd tweakcc
npm install --legacy-peer-deps
npm run build

# in claude-sneakpeek
# update package.json dependency
npm install
npm run bundle
npx @realmikekelly/claude-sneakpeek quick --name claude3
```

Use these steps if `tweakcc failed.` appears during variant creation.


## What gets unlocked?

Features that are built into Claude Code but not yet publicly released:

- **Swarm mode** — Native multi-agent orchestration with `TeammateTool`
- **Delegate mode** — Task tool can spawn background agents
- **Team coordination** — Teammate messaging and task ownership

## Commands

```bash
npx @realmikekelly/claude-sneakpeek quick --name claudesp   # Install
npx @realmikekelly/claude-sneakpeek update claudesp         # Update
npx @realmikekelly/claude-sneakpeek remove claudesp         # Uninstall
```

## Where things live

```
~/.claude-sneakpeek/claudesp/
├── npm/           # Patched Claude Code
├── config/        # Isolated config, sessions, MCP servers
└── variant.json

~/.local/bin/claudesp   # Wrapper script
```

## Alternative providers

Supports Z.ai, MiniMax, OpenRouter, and local models via cc-mirror. See [docs/providers.md](docs/providers.md).

## Credits

Fork of [cc-mirror](https://github.com/numman-ali/cc-mirror) by Numman Ali.

## License

MIT
