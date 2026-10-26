# Ghi chú CV — các quyết định & lưu ý

Tổng hợp những gì đã bàn khi dựng lại repo CV (10/2026). Đọc file này trước khi sửa CV lần sau.

## 1. Repo & build

- 3 ngôn ngữ, mỗi ngôn ngữ 1 file chính + 1 thư mục section:
  `resume-fr.tex` + `fr/`, `resume-en.tex` + `en/`, `resume-vn.tex` + `vn/`.
  Template (`awesome-cv.cls`), font và ảnh (`assets/`) dùng chung.
- **Sửa nội dung là sửa cả 3 bản**, rồi build lại cả 3 để kiểm tra vẫn 1 trang.
- Build local (TeX Live đã cài trên WSL):
  ```bash
  mkdir -p build
  for l in fr en vn; do xelatex -output-directory=build resume-$l.tex; done
  ```
  File sinh ra nằm trong `build/` (đã `.gitignore`).
- **Sửa và kiểm tra ở local trước, chỉ push khi đã chốt**: mỗi lần push là CI chạy lại.
- Push bằng tài khoản `laiduy98` (remote `git@github-laiduy98:laiduy98/cv.git`, SSH key `~/.ssh/laiduy98-github`).
  Commit với `laiduy98@gmail.com`.

## 2. Những chỗ đã sửa trong template (`awesome-cv.cls`)

- Đường dẫn font mặc định đổi thành `assets/fonts/` (class nạp font trước khi `\fontdir` trong file `.tex` có tác dụng).
- Bỏ `\vspace{-4.0mm}` cuối môi trường `cvitems`: với TeX Live mới, nó làm gạch đầu dòng cuối đè lên tiêu đề mục tiếp theo.
- Bảng kỹ năng (`cvskills`) dùng `tabularx`, nhãn căn trên (dòng 2 dòng không còn bị nhãn nằm giữa).
- `\cventry` bỏ hàng mô tả khi mô tả rỗng (dùng cho phần Formation không có "Cours connexes").
- Chữ hoa nhỏ tiếng Việt hiển thị đúng. Nếu trích text từ PDF thấy kiểu "VừA HọC" thì đó chỉ là lỗi khi trích, không phải lỗi hiển thị.
- Tiêu đề section tiếng Việt có dấu cách ở 3 ký tự đầu (tô màu) phải viết `Kỹ\ năng`, nếu không sẽ thành "Kỹnăng".
- Tên tiếng Việt: `\name{{\bfseries\color{text}Lại} Khang Duy}{}` để họ in đậm và cùng màu với "Lai" ở bản FR/EN.

## 3. CI/CD & phát hành

- Workflow: `.github/workflows/pages.yml`.
  - TeX Live tối thiểu qua `TeX-Live/setup-texlive-action@v4`, có cache (~20 s/lần khi đã có cache).
  - FontAwesome phải được đăng ký với fontconfig trong CI (bước `fc-cache`), vì `fontawesome.sty` nạp font theo tên.
- **Push `main`** → pre-release `tip`, file đặt tên theo `git describe`:
  `Khang-Duy-Lai-CV-FR-v5.0-3-gabcdef1.pdf` (3 commit sau v5.0, commit `abcdef1`). Chưa có tag thì chỉ có hash.
- **Push tag `v*`** → release chính thức + cập nhật GitHub Pages:
  ```bash
  git tag v5.0 && git push origin v5.0
  ```
  - Release: `Khang-Duy-Lai-CV-{FR,EN,VN}-v5.0.pdf` (tên có họ tên → gửi file này cho nhà tuyển dụng).
  - Pages: `https://laiduy98.github.io/cv/{fr,en,vn}.pdf` (link ngắn, luôn là bản phát hành mới nhất).
- Một nhánh `main` duy nhất (trunk-based), không cần nhánh `dev`: `tip` là bản đang làm, tag là bản chính thức.

### Việc còn tồn đọng

- [ ] Tạo tag `v5.0` khi chốt v5 (trước đó Pages vẫn là bản FR cũ, link `fr/en/vn.pdf` chưa hoạt động).
- [ ] Xoá nhánh cũ trên remote: `git push origin --delete master cursor/answer-general-questions-3ee3 ci-speed`
- [ ] Xoá quyền deploy Pages của `master`:
      `gh api -X DELETE repos/laiduy98/cv/environments/github-pages/deployment-branch-policies/6719246`
- [ ] Chuyển gh về tài khoản công ty: `gh auth switch --user kd-lai-ai6`
- [ ] (tuỳ chọn) `assets/fonts/FontAwesome.ttf` không được dùng, có thể xoá.

## 4. Nguyên tắc viết CV đã thống nhất

- **1 trang** cho cả 3 bản. Bản FR dài nhất, luôn kiểm tra bản FR trước.
- **Chỉ ghi sự thật kiểm chứng được**: mỗi kỹ năng phải xuất hiện ở ít nhất một mục kinh nghiệm; không ghi thứ mình không tự tin trả lời khi phỏng vấn.
- **Không dùng tính từ nặng** (éprouvée, critiques, sous forte charge…). Nói việc đã làm, để phần kinh nghiệm chứng minh.
- **Hạn chế dấu ":"** trong câu. Viết câu liền mạch với "avec", "par", ngoặc đơn.
- **Bản FR giữ thuật ngữ kỹ thuật bằng tiếng Anh**: image fingerprinting, preprocessing, data pipelines, embeddings, recall, harness, churn, POC…
  Từ thông dụng thì giữ tiếng Pháp: seuil, recherche sémantique, analyse des logs.
- **Bản EN**: mỗi gạch bắt đầu bằng động từ quá khứ (Built, Developed…), chính tả Anh–Anh (specialised, analysed, licence).
- **Bản VN**: dùng từ tiếng Anh như dân tech Việt hay nói: platform (không "nền tảng"), semantic search, model (không "mô hình"),
  tên khoá học và chức danh bằng tiếng Anh (Machine Learning for Data Science, AI Engineer).
  Không ghi "vừa học vừa làm" (người Việt không quan tâm). Ngôn ngữ ghi "thành thạo" (không "chuyên nghiệp").
- **Tên vị trí / bằng cấp ghi đúng như thật**, kể cả khi nghe "kém" hơn:
  - Lundi Matin là **Développeur Python** (không phải ML Engineer).
  - Bằng là **Master**, không viết "Ingénieur diplômé" (ở Pháp nghĩa là có diplôme d'ingénieur CTI). "Kỹ sư" ở bản VN cũng vậy.
  - Tên khoá M1 giữ nguyên **"Apprentissage Machine pour la Science des Données"** (đúng tên khoá).
  - SFR là **alternance** → FR ghi "(alternance)", EN ghi "(Apprenticeship)" (không dùng co-op/placement/work-study), VN bỏ hẳn.
- **Tiêu đề**: `MACHINE LEARNING ENGINEER · MLOPS` (một hướng rõ ràng, khớp với chức danh ở AI6).
- **Cloud**: AWS cho dịch vụ backend, GCP cho ML. Ở AI6 hạ tầng Azure đã được bọc qua Kubernetes → **không ghi Azure/AKS**.

## 5. Nội dung từng mục — lý do chọn

- **Kỹ năng**: bỏ R (lâu không dùng), bỏ Computer Vision khỏi kỹ năng (vẫn giữ trong kinh nghiệm), bỏ SageMaker, GKE, Dagster (trùng Airflow), Deep Learning, Data Visualisation, Hive/Impala (vẫn có ở SFR).
  ElastiCache → RDS (phổ biến hơn, khớp với phần kinh nghiệm). GitOps → **Helm, ArgoCD** (tên công cụ cụ thể; "GitOps" vẫn có ở gạch AI6).
- **AI6** (Juil. 2026 –): giữ 3 gạch ngắn vì mới làm 3 tháng (nền tảng Airflow, 16 DAG, harness đánh giá). **Bỏ product resolver** cho gọn.
  Không ghi "production" cho Airflow (mới có staging).
- **Lundi Matin** (Mai 2024 – Juin 2026): RAG lên đầu (đang tìm việc ML). Không liệt kê số lượng service/worker, chỉ giữ "~4 M lignes" làm bối cảnh quy mô.
- **SFR** (Mai 2022 – Jan. 2024): 3 dự án, mỗi dự án 1 gạch: ảnh gian lận (CV), POC MLOps churn, phân tích cuộc gọi bằng LLM. Bỏ dashboard Marketing.
- **VMO** (Oct. 2019 – Déc. 2021): chức danh chính thức "Kỹ sư trí tuệ nhân tạo" → FR "Ingénieur IA", EN và VN "AI Engineer". Giữ 3 việc liên quan ML, bỏ "embarqué" vì không nhớ rõ.
- **Formation**: chỉ ghi năm, bỏ tháng và "Cours connexes".
- **Ngôn ngữ**: thứ tự Pháp (TCF B2) → Anh (TOEIC **885**) → Việt (bản ngữ), tên ngôn ngữ in đậm.
- **Bằng lái**: B, AM.
- Chân trang (ngày tiếng Anh, "Résumé" ở bản FR): **để nguyên**.

## 6. Làm tiếp lần sau

- Đọc lại bản EN và VN lần cuối (`build/resume-en.pdf`, `build/resume-vn.pdf`, hoặc bản `tip` trong Releases).
- (tuỳ chọn) Dòng cử nhân bản VN: "Cử nhân Công nghệ Thông tin và Truyền thông" → "Cử nhân ICT" cho đồng bộ.
- Chốt xong thì tạo tag `v5.0` (xem mục 3) và làm các việc tồn đọng.

## 7. Chuẩn bị phỏng vấn (những gì CV không ghi nhưng có thể bị hỏi)

- **Ảnh gian lận (SFR)**: ~90 % trường hợp là up lại y hệt ảnh → image fingerprint là đủ; phần còn lại (cắt, đổi màu, lật) dùng preprocessing OpenCV + similarity CLIP với ngưỡng. Con số 90 → 99 % là **recall** — kiểm tra lại cách đo trước khi gửi CV.
  Bối cảnh: kỹ thuật viên upload ảnh sửa hộp cáp để tính tiền.
- **Izberg (Lundi Matin)**: nền tảng marketplace B2B "cho thuê backend" cho các nhà bán lẻ lớn. ~4 M dòng code là quy mô hệ thống, không phải code mình viết. Celery dùng **ElastiCache** làm broker. Công việc hằng ngày: đọc log CloudWatch, SQS, RDS.
- **Airflow (AI6)**: chạy trên AKS nhưng được nền tảng nội bộ (Backstage, ArgoCD) bọc qua Kubernetes → làm việc với Helm/ArgoCD, không cấu hình Azure trực tiếp. Có realm Keycloak riêng, CeleryExecutor, KubernetesPodOperator, mỗi team một namespace.
  Đã viết bản đánh giá Dagster vs Airflow.
- **Harness đánh giá chatbot (AI6)**: replay hội thoại qua WebSocket, người dùng giả lập bằng LLM, DeepEval (coherence, instruction adherence, faithfulness trên FAQ), điểm lưu vào MLflow. Mới có 1 lần chạy → chưa có số liệu cải thiện.
- **Product resolver (AI6, không ghi trên CV)**: tổng hợp Keepa / Google Shopping / Google Lens, LLM chọn kết quả, điểm tin cậy; v2.0 lên production; có prototype multi-agent (OpenAI Agents SDK) chưa merge.
- **Kubernetes / GitOps**: dùng nhiều, nên chuẩn bị ví dụ cụ thể (ArgoCD sync hooks, secrets không bị tạo lại mỗi lần sync…).
