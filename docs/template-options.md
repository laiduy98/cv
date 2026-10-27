# Template options (removed from comments)

Features that `awesome-cv.cls` supports but the CV currently **does not use**. The template's sample comments were removed from `resume-*.tex`; this file keeps them so you know where to add them and how to write them when needed.

Everything below goes in `resume-*.tex` (remember to update all three: `fr`, `en`, `vn`) unless stated otherwise.

## 1. Header — personal information

Place these in the `PERSONAL INFORMATION` block. Omit a line and that item is not shown. Social items appear in the template's fixed order, separated by `|`.

| Command | Shows | Notes |
|---|---|---|
| `\photo[circle,noedge,left]{assets/ava.jpg}` | Profile photo | **Already present as a comment** in `resume-*.tex`. Options: `circle`/`rectangle`, `edge`/`noedge`, `left`/`right`. |
| `\homepage{example.com}` | Personal website | |
| `\gitlab{user}` | GitLab | |
| `\stackoverflow{id}{name}` | Stack Overflow | Two arguments: numeric id and display name |
| `\twitter{@user}` | Twitter/X | |
| `\skype{user}` | Skype | |
| `\reddit{user}` | Reddit | |
| `\medium{user}` | Medium | |
| `\xing{user}` | Xing | |
| `\googlescholar{id}{name}` | Google Scholar | Leave `name` empty to use the full name |
| `\extrainfo{...}` | One free-form line | |
| `\quote{``...''}` | Quote below the header | |

Header alignment: `\makecvheader[C]` — `C` centre, `L` left, `R` right.

## 2. Footer

```latex
\makecvfooter
  {<left>}      % e.g. \today (build date, always printed in English)
  {<centre>}    % currently: Khang Duy Lai · CV
  {<right>}     % e.g. \thepage (page number)
```

The date and page number were removed: the CV is one page and the build date carries no information (see `docs/cv-notes.md`).

## 3. Colours

Place after `\documentclass`.

```latex
% Accent colour (section titles, locations…). Default: awesome-red
\colorlet{awesome}{awesome-skyblue}
% Available: awesome-emerald, awesome-skyblue, awesome-red, awesome-pink,
%            awesome-orange, awesome-nephritis, awesome-concrete, awesome-darknight

% Or any custom colour
\definecolor{awesome}{HTML}{CA63A8}

% Text colours (defaults shown)
\definecolor{darktext}{HTML}{414141}
\definecolor{text}{HTML}{333333}
\definecolor{graytext}{HTML}{5D5D5D}
\definecolor{lighttext}{HTML}{999999}

% Stop colouring the first three letters of section titles (default: true)
\setbool{acvSectionColorHighlight}{false}
```

## 4. Layout & spacing

```latex
\documentclass[11pt, letterpaper]{awesome-cv}   % US Letter instead of A4
\geometry{left=1.4cm, top=.8cm, right=1.4cm, bottom=1.8cm, footskip=.5cm}

% Separator between social items (default: \quad\textbar\quad)
\renewcommand{\acvHeaderSocialSep}{\quad\textbar\quad}

% Spacing in the header / between sections (defaults shown)
\renewcommand{\acvHeaderAfterNameSkip}{.4mm}
\renewcommand{\acvHeaderAfterPositionSkip}{.4mm}
\renewcommand{\acvHeaderAfterSocialSkip}{6mm}
\renewcommand{\acvSectionTopSkip}{1.0mm}
\renewcommand{\acvSectionContentTopSkip}{1.0mm}
```

Font sizes for each element live in `awesome-cv.cls` (the `\...style` commands, around lines 210–255).

## 5. Other content blocks (used in section files)

| Environment / command | Used for | Example |
|---|---|---|
| `cventries` + `\cventry{position}{organisation}{location}{dates}{description}` | Experience, education (in use) | |
| `cvitems` + `\item` | Bullets inside `\cventry` (in use) | |
| `cvskills` + `\cvskill{label}{content}` | Skills table (in use) | |
| `cvparagraph` | Paragraph (in use for the summary) | |
| `cvhonors` + `\cvhonor{award}{event}{location}{date}` | Awards, certifications | `\cvhonor{Finalist}{Kaggle X}{Online}{2025}` |
| `cvsubentries` + `\cvsubentry{position}{title}{date}{description}` | Small sub-items within one experience | |
| `\cvsubsection{...}` | Sub-heading within a section | |

To add a new section (e.g. certifications): create `fr/certifications.tex` (plus `en/` and `vn/`), then add `\input{fr/certifications.tex}` to `resume-fr.tex`.

The **extracurricular** section (`xx/extracurricular.tex`, technical translator 2014–2015) already exists: remove the `%` in front of `% \input{xx/extracurricular.tex}` to show it.

The template's other sample sections (honors, presentation, writing, committees) were deleted because they held the template author's placeholder data — rewrite them from the table above if needed.

`education.tex` switches off uppercasing of the degree line with
`\renewcommand*{\acvPositionCase}[1]{#1}` right after `\begin{cventries}`. The same line works in any other section file.

## 6. Cover letter

The template ships cover-letter commands in the same style as the CV (the sample `coverletter.tex` was deleted):

```latex
\recipient{Company name}{Address\\City}
\letterdate{\today}
\lettertitle{Candidature : Machine Learning Engineer}
\letteropening{Madame, Monsieur,}
\letterclosing{Cordialement,}
\letterenclosure[Pièce jointe]{CV}

\begin{document}
\makecvheader[R]
\makelettertitle
\begin{cvletter}
  \lettersection{Pourquoi vous ?} ...
  \lettersection{Pourquoi moi ?} ...
\end{cvletter}
\makeletterclosing
\end{document}
```

Original sample: https://github.com/posquit0/Awesome-CV/blob/master/examples/coverletter.tex
