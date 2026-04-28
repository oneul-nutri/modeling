# Repository Guidelines

## Project Structure & Module Organization
이 repo는 K-Calculator의 데이터·ML 노트북 모음.

- `db-preprocessing/` — 식약처 영양소 DB 정제 (`preprocessing.ipynb` + `requirements.txt`). 백엔드에 적재할 형식으로 가공.
- `data-preprocess-main.ipynb` — 탐색용 스크래치패드.

크롤링·YOLO 학습 파이프라인은 별도 repo([oneul-nutri/crawling](https://github.com/oneul-nutri/crawling))에 있다. 같은 워크스페이스에 sibling으로 clone하면 `../crawling/data/...` 등 상대경로로 참조 가능. modeling repo에는 submodule로 포함되지 않는다 (1인 운영 부담 절감 목적, 2026-04-28 분리).

## Build, Test, and Development Commands

```bash
# db-preprocessing 노트북
cd db-preprocessing
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab preprocessing.ipynb
```

크롤링·학습 작업은 [oneul-nutri/crawling](https://github.com/oneul-nutri/crawling) repo에서 진행.

## Coding Style & Naming Conventions
Python 소스는 PEP 8 (4-space indents, snake_case functions, UpperCamelCase classes). 신규 모듈에는 타입 힌트. CLI 엔트리는 idempotent하게, 잘못된 경로엔 fail fast. JSON/CSV 요약은 `data/meta/`에 출력.

노트북은 수정 시 사본을 만들고 Markdown 셀에 검증 단계를 기록.

## Testing Guidelines
공식 unit 스위트 없음. 노트북 결과 검증은 출력 셀의 통계·시각화로 확인.

## Commit & Pull Request Guidelines
Conventional Commit prefix (`feat:`, `fix:`, `chore:`, `docs:`). 제목 72자 이하. PR 설명에 영향 받은 run ID, 실행 스크립트, 검증 단계, 후속 작업 명시.

## Security & Data Handling
다운로드 이미지·독점 데이터셋·시크릿은 절대 커밋 금지. 메타데이터·설정만 git에 포함. 노트북에서 자격증명 스크럽. API 키는 환경변수로만 처리, repo에는 두지 않는다.
