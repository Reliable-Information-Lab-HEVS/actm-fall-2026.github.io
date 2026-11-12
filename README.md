# actm-fall-2026.github.io

Website for the final project of the HSLU course *AI Based Cybersecurity Threat Detection and Mitigation* (ACTM, Fall 2026).

Built with Jekyll and the [Just the Docs](https://just-the-docs.com) remote theme, deployed to GitHub Pages by `.github/workflows/pages.yml` on every push to `main`. In the repository settings, set **Pages → Source** to **GitHub Actions**.

## Values to fill in

Course-wide values (domain, data link, upload links, report deadline) live at the top of `_config.yml` and are used across pages as `{{ site.<name> }}`. Search for `TBC` to find what is still open.

## Local preview

```bash
bundle install
bundle exec jekyll serve
```
