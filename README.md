# CV — Khang Duy LAI

Résumé in French, English and Vietnamese, built with the [Awesome CV](https://github.com/posquit0/Awesome-CV) LaTeX template.

**PDFs** — latest versioned release, built by GitHub Actions ([workflow](.github/workflows/pages.yml)):

- 🇫🇷 https://laiduy98.github.io/cv/fr.pdf
- 🇬🇧 https://laiduy98.github.io/cv/en.pdf
- 🇻🇳 https://laiduy98.github.io/cv/vn.pdf

**Releases** ([GitHub Releases](https://github.com/laiduy98/cv/releases)):

- `tip` — pre-release, rebuilt on every push to `main`. Files are named with `git describe`, e.g. `Khang-Duy-Lai-CV-FR-v5.0-3-gabcdef1.pdf` (3 commits after v5.0, commit `abcdef1`).
- `vX.Y` — versioned release (`Khang-Duy-Lai-CV-FR-v5.0.pdf`, …), also updates the GitHub Pages links above:

  ```bash
  git tag v5.0 && git push origin v5.0
  ```

## Structure

```
resume-fr.tex         # Main file per language: header (name, contact) and section list
resume-en.tex
resume-vn.tex
fr/ en/ vn/           # Sections per language:
  summary.tex         #   Profil / Summary / Tóm tắt bản thân
  skills.tex          #   Compétences / Skills / Kỹ năng
  experience.tex      #   Expérience / Experience / Kinh nghiệm
  education.tex       #   Formation / Education / Học vấn
  hobbies.tex         #   Autre / Other / Khác (languages, licence, hobbies)
  extracurricular.tex #   Disabled in resume-*.tex
awesome-cv.cls        # Shared template (font path default: assets/fonts/)
assets/
  fonts/              # Roboto + FontAwesome
  ava.jpg             # Photo (enable the \photo line in resume-*.tex)
```

To add or remove a section, toggle its `\input{...}` line in `resume-*.tex`.
When changing content, update all three languages.

## Build

Requires XeLaTeX (the class uses `fontspec`). On Ubuntu:

```bash
sudo apt install texlive-xetex texlive-latex-extra texlive-fonts-extra
```

Then, from the project root:

```bash
mkdir -p build
for l in fr en vn; do xelatex -output-directory=build resume-$l.tex; done   # → build/resume-{fr,en,vn}.pdf
```

All generated files go to `build/`, which can be deleted at any time.
