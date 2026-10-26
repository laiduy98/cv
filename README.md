# CV — Khang Duy LAI (FR)

French résumé built with the [Awesome CV](https://github.com/posquit0/Awesome-CV) LaTeX template.

**PDF:** https://laiduy98.github.io/cv/CV_Khang_Duy_LAI_FR.pdf — rebuilt and published by GitHub Actions on every push to `main` ([workflow](.github/workflows/pages.yml)).

## Structure

```
resume.tex            # Main file: header (name, contact) and section list
resume/
  summary.tex         # Profil
  skills.tex          # Compétences
  experience.tex      # Expérience Professionnelle
  education.tex       # Formation
  extracurricular.tex # Activités extra-professionnelles (disabled in resume.tex)
  hobbies.tex         # Autre (langues, centres d'intérêt)
awesome-cv.cls        # Template class (font path default: assets/fonts/)
assets/
  fonts/              # Roboto + FontAwesome
  ava.jpg             # Photo (enable the \photo line in resume.tex)
```

To add or remove a section, toggle its `\input{...}` line in `resume.tex`.

## Build

Requires XeLaTeX (the class uses `fontspec`). On Ubuntu:

```bash
sudo apt install texlive-xetex texlive-latex-extra texlive-fonts-extra
```

Then, from the project root:

```bash
mkdir -p build && xelatex -output-directory=build resume.tex   # → build/resume.pdf
```

All generated files (`.pdf`, `.aux`, `.log`, `.out`) go to `build/`, which can be deleted at any time.
