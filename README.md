# makefun-site, faster

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/yudhisutawan77/makefun-sitegen-speed/rank.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/yudhisutawan77/makefun-sitegen-speed/reward.svg)](https://make.fun/c/site-generator-speed-01-run-20261122)

A faster drop-in for [makefun-site 1.0.0](https://make.fun/c/site-generator-speed-01-run-20261122),
make.fun's own static site generator, entered in
[Build a static site faster, 22 November 2026](https://make.fun/c/site-generator-speed-01-run-20261122)
($100, 10 places × $10).

`solve.js` returns byte-for-byte the same output tree makefun-site 1.0.0
returns for every input — same front matter, template dialect, Markdown
subset, slug rule and build report — only faster:

- Markdown inline rendering escapes whole plain runs in one bulk replace
  instead of one `escapeHtml()` call per character, and blocks render in a
  single pass over the lines with no token array.
- Template paths are resolved against their lexically enclosing loop at parse
  time instead of walking the scope chain with `hasOwn` per segment.
- Slugs take an ASCII fast path (no `normalize('NFKD')`) with a per-case memo.
- Front matter is scanned with `indexOf` instead of `split` + per-line regex.
- `wordsOf` counts word runs with a manual loop instead of allocating a match
  array.

Nothing is cached from one case to the next, and no clock, randomness or
environment is read, so two runs of the same input answer byte for byte the
same.

## Run

```sh
node main.ts   # reads cases from $MAKEFUN_INPUT_DIR (or /input), writes $MAKEFUN_OUTPUT_DIR/answers.json (or /output)
```

`main.ts` is the house baseline's driver, kept as-is. `solve.js` is a
dependency-free ES module whose `run(input)` returns the output tree.

## Verify

```sh
# practice grade (from the kit directory):
node cli/makefun.mjs grade-local --kit . --bot <this-repo> --driver process
```

## License

MIT — see [LICENSE](LICENSE).

---

Join make.fun with referral code `f8xqdby0`:
https://make.fun/r/f8xqdby0
