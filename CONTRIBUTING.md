# Contributing to Kydos

Thanks for taking the time to contribute. Kydos is a mobile-first social
memecoin launchpad, and contributions of every size are welcome.

## Ground rules

- **Mobile first.** Every change must look and feel right at a 390–440px
  viewport before anything else.
- **Small, focused files.** Components stay short and live in their own file
  under `src/components/<domain>/`.
- **Design tokens only.** Use the semantic Tailwind classes backed by the tokens
  in `src/index.css` — no hardcoded hex values or inline font families.
- **No new market-data dependencies.** Chain data is self-indexed on purpose;
  prefer extending the indexer over adding a third-party aggregator.

## Workflow

1. Fork the repository and create a branch: `git checkout -b feature/my-change`.
2. Keep commits scoped and write imperative commit subjects
   (`add holder distribution chart`), since they become the release changelog.
3. Verify the affected flows end to end — launching, trading, posting, profile.
4. Open a pull request describing what changed, why, and how you verified it.
   Screenshots or a short screen recording at a mobile viewport are appreciated.

## Reporting bugs

Open an issue with the steps to reproduce, the expected and actual behavior, the
device/viewport, and a screenshot where relevant.

## Code of conduct

Be direct, be kind, and assume good faith. Harassment of any kind is not
tolerated.

## License

By contributing you agree that your contributions are licensed under the
[MIT License](LICENSE).
