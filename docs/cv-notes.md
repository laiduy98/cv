# CV notes — decisions & reminders

Everything decided while rebuilding the CV repo (October 2026). Read this before editing the CV again.
Template features that exist but are not used are listed in [template-options.md](template-options.md).

## 1. Repo & build

- Three languages, each with one main file and one section folder:
  `resume-fr.tex` + `fr/`, `resume-en.tex` + `en/`, `resume-vn.tex` + `vn/`.
  The template (`awesome-cv.cls`), fonts and photo (`assets/`) are shared.
- **Any content change goes into all three versions**, then rebuild all three and check each is still one page (FR is the longest — check it first).
- Local build (TeX Live installed in WSL):
  ```bash
  mkdir -p build
  for l in fr en vn; do xelatex -output-directory=build resume-$l.tex; done
  ```
  Output goes to `build/` (git-ignored).
- **Edit and check locally first; push only once settled** — every push re-runs CI.
- Push as `laiduy98` (remote `git@github-laiduy98:laiduy98/cv.git`, SSH key `~/.ssh/laiduy98-github`),
  committing as `laiduy98@gmail.com`.

## 2. Changes made to the template (`awesome-cv.cls`)

- Default font path is `assets/fonts/` (the class loads fonts before `\fontdir` in the `.tex` file takes effect).
- Removed the `\vspace{-4.0mm}` at the end of `cvitems`: with recent TeX Live it made the last bullet overlap the next heading.
- Skills table (`cvskills`) uses `tabularx`, labels top-aligned (two-line rows no longer centre the label).
- `\cventry` skips the description row when it is empty (Formation has no "Cours connexes").
- The position line is uppercased through `\acvPositionCase`; `education.tex` switches it off locally so degree names stay in normal case.
- Accent colour: `awesome-skyblue` (`\colorlet{awesome}{...}` in `resume-*.tex`; was red). Small sky-blue text is a bit light (~3.6:1 contrast) — `#0B6FB8` is a darker option if needed.
- Font sizes: body text 9.5 pt Regular, company/school names 11 pt, section titles 13 pt, tech stack next to job titles 7.5 pt (set in `xx/experience.tex`).
- Product names are never hyphenated (`\hyphenation{...}` list: OpenAI, OpenSearch, OpenCV, Kubernetes, KubernetesPodOperator, DeepEval, MLflow, BigQuery, CloudWatch, Backend, Rekognition, serverless).
  Where such a word follows a slash, allow a break with `\allowbreak` (e.g. `Backend/\allowbreak Cloud` in the summary), otherwise the line overflows the margin.
- Vietnamese section titles with a space inside the first three (coloured) letters need `Kỹ\ năng`, otherwise it renders as "Kỹnăng".
- Vietnamese name: `\name{{\bfseries\color{text}Lại} Khang Duy}{}` so the family name is bold and the same colour as "Lai" in FR/EN.
- Text copied out of the PDF can show broken Vietnamese ("Cửnhân", "VừA"); that is an extraction artefact, the rendering is correct.

## 3. CI/CD & releases

- Workflow: `.github/workflows/pages.yml`.
  - Minimal TeX Live via `TeX-Live/setup-texlive-action@v4`, cached (~20–30 s per build once cached).
  - FontAwesome must be registered with fontconfig in CI (the `fc-cache` step) because `fontawesome.sty` loads the font by name.
- **Push to `main`** → pre-release `tip`, files named with `git describe`:
  `Khang-Duy-Lai-CV-FR-v5.0.2-3-gabcdef1.pdf` (3 commits after v5.0.2, commit `abcdef1`).
- **Push a `v*` tag** → versioned release + GitHub Pages update:
  ```bash
  git tag v5.1 && git push origin v5.1
  ```
  - Release: `Khang-Duy-Lai-CV-{FR,EN,VN}-vX.Y.pdf` — the file name contains the full name, so send these to recruiters.
  - Pages: `https://laiduy98.github.io/cv/{fr,en,vn}.pdf` — short links, always the latest release.
- A single `main` branch (trunk-based), no `dev` branch: `tip` is work in progress, tags are releases.
- Released so far: `v5.0`, `v5.0.1`, `v5.0.2`, `v5.1`.

### Open tasks

- [ ] Delete old remote branches: `git push origin --delete master cursor/answer-general-questions-3ee3 ci-speed`
- [ ] Remove the Pages deployment rule for `master`:
      `gh api -X DELETE repos/laiduy98/cv/environments/github-pages/deployment-branch-policies/6719246`
- [ ] Switch gh back to the work account: `gh auth switch --user kd-lai-ai6`
- [ ] (optional) `assets/fonts/FontAwesome.ttf` is unused and can be deleted.
- [ ] (optional) Left-align bullets instead of justifying them (some lines have wide word gaps).
- [ ] (optional) Header subtitle (`MACHINE LEARNING ENGINEER · MLOPS`, 8.5 pt) is smaller than body text; consider 10 pt.

## 4. Writing rules agreed on

- **One page** for all three versions.
- **Only verifiable facts**: every skill must appear in at least one experience entry; nothing you can't comfortably discuss in an interview.
- **No heavy adjectives** (éprouvée, critiques, sous forte charge…). State what was done and let the experience section prove it.
- **Few colons** in sentences. Write flowing sentences with "avec", "par", parentheses.
- **FR keeps technical terms in English**: image fingerprinting, preprocessing, data pipelines, embeddings, recall, harness, churn, POC…
  Common words stay French: seuil, recherche sémantique, analyse des logs.
- **Job titles are in English in all three versions** (Machine Learning Engineer, Python Developer, Data Scientist, AI Engineer).
- **EN**: every bullet starts with a past-tense verb (Built, Developed…); British spelling (specialised, analysed, licence).
- **VN**: use the English words Vietnamese tech people actually use — platform (not "nền tảng"), semantic search, model (not "mô hình"),
  English course names (Machine Learning for Data Science). The programming-languages row is "Ngôn ngữ"; spoken languages are "Ngoại ngữ".
  No "vừa học vừa làm" (irrelevant to Vietnamese readers). Language level is "thành thạo" (not "chuyên nghiệp").
- **Titles and degrees as they really are**, even if they sound less impressive:
  - Lundi Matin was **Python Developer** (not ML Engineer).
  - The degree is a **Master**: never "Ingénieur diplômé" (in France that means a CTI-accredited diplôme d'ingénieur). Same for "Kỹ sư" in VN.
  - The FR M1 name stays **"Apprentissage Machine pour la Science des Données"** (the actual programme name).
  - SFR was an **alternance** → FR "(alternance)", EN "(Apprenticeship)" (not co-op/placement/work-study), VN omitted.
- **Header subtitle**: `MACHINE LEARNING ENGINEER · MLOPS` (one clear direction, matches the AI6 title).
- **Summary**: "Plus de 5 ans d'expérience en IA et développement Python, **dont** …" — the 5 years apply to the total, not to each listed area (Kubernetes pipelines are only since 2026).
- **Cloud**: AWS for backend services, GCP for ML. At AI6 the Azure infrastructure is abstracted behind Kubernetes → **no Azure/AKS on the CV**.
- **Personal details**: address is the region only (Région parisienne / Paris region / Vùng Paris); driving licence **B** only (VN: "B, A1" — the Vietnamese motorbike licence matters to Vietnamese readers).
- **Name**: FR/EN header and footer show **"Duy Lai"** (the name used day to day); VN keeps "Lại Khang Duy". Release file names keep the full name (`Khang-Duy-Lai-CV-…`).
- **Footer**: only "Duy Lai · CV" (VN: "Lại Khang Duy · CV") — no date, no page number.

## 5. Section contents — why

- **Skills**: dropped R (not used for years), Computer Vision (kept in experience), SageMaker, GKE, Dagster (overlaps Airflow), Deep Learning, Data Visualisation, Hive/Impala (still in SFR), FastAPI (only backed by the VMO API bullet, which was removed).
  ElastiCache → RDS (more common, matches the experience section). GitOps → **Helm, ArgoCD** (concrete tools; "GitOps" stays in the AI6 bullet).
- **AI6** (Jul. 2026 –): three short bullets since it has only been three months (Airflow platform, 16 DAGs, evaluation harness). **Product resolver left out** to keep it short.
  Airflow is not described as "production" (only staging exists).
- **Lundi Matin** (May 2024 – Jun. 2026): RAG first (looking for ML roles). No service/worker counts; only "~4 M lines" as scale context.
- **SFR** (May 2022 – Jan. 2024): three projects, one bullet each — fraudulent photo detection (CV), churn MLOps POC, customer call analysis with topic modelling (TF-IDF, LDA, BERTopic — not LLMs; 2022–23). Marketing dashboards dropped.
- **VMO** (Oct. 2019 – Dec. 2021): official title "Kỹ sư trí tuệ nhân tạo" → "AI Engineer". Bullets: doorbell CNNs trained from scratch (object/face detection, age estimation), typhoon warning (satellite/radar images, optical flow — classical methods and NVIDIA neural models — plus Navier-Stokes), meeyland LightFM recommender.
  Dropped: "deployed models as APIs with FastAPI" (hardly did it), data-collection scripts and Plotly dashboard (generic), "embarqué" (not remembered precisely).
- **Education**: years only, no months, no "Cours connexes"; degree names in normal case.
- **Languages**: French (TCF B2) → English (TOEIC **885**) → Vietnamese (native), language names in bold.
- Research internship in Ostrava (2019): removed — too old, not worth the space.

## 6. Interview prep (not on the CV but likely to come up)

- **Fraudulent photos (SFR)**: ~90 % of cases were exact re-uploads → image fingerprinting was enough; the rest (cropped, recoloured, flipped) used OpenCV preprocessing + CLIP similarity with a threshold.
  The 90 → 99 % figure is **recall** — double-check how it was measured before sending the CV.
  Context: field technicians upload photos of cable-box repairs to get paid.
- **Izberg (Lundi Matin)**: B2B marketplace platform "renting its backend" to large retailers. ~4 M lines is the size of the system, not code you wrote. Celery uses **ElastiCache** as its broker. Daily work: CloudWatch logs, SQS, RDS.
- **Airflow (AI6)**: runs on AKS but the internal platform (Backstage, ArgoCD) abstracts it behind Kubernetes → you work with Helm/ArgoCD, not Azure directly. Dedicated Keycloak realm, CeleryExecutor, KubernetesPodOperator, one namespace per team.
  You also wrote a Dagster vs Airflow evaluation.
- **Chatbot evaluation harness (AI6)**: replays conversations over WebSocket, LLM-simulated user, DeepEval (coherence, instruction adherence, faithfulness on FAQ answers), scores logged to MLflow. Only one run so far → no improvement numbers yet.
- **Product resolver (AI6, not on the CV)**: aggregates Keepa / Google Shopping / Google Lens, LLM picks the result, confidence scoring; v2.0 in production; an unmerged multi-agent prototype (OpenAI Agents SDK).
- **Typhoon warning (VMO)**: optical flow (classical methods and NVIDIA neural models) between successive satellite/radar images, storm movement computed with the Navier-Stokes equations — be ready to explain the method end to end.
- **Kubernetes / GitOps**: used a lot — prepare concrete examples (ArgoCD sync hooks, secrets not regenerated on every sync…).
