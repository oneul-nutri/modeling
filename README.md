# oneul-nutri/modeling

K-Calculator 데이터·ML 노트북.

## 콘텐츠

| 파일 | 용도 |
|------|------|
| [`db-preprocessing/preprocessing.ipynb`](./db-preprocessing/preprocessing.ipynb) | 식약처 영양소 DB → 백엔드 적재용 형식으로 정제 |
| [`db-preprocessing/pnds.ipynb`](./db-preprocessing/pnds.ipynb) | 결측치 처리 (옛 oneul-nutri/missing-value repo에서 흡수) |
| [`data-preprocess-main.ipynb`](./data-preprocess-main.ipynb) | 탐색·실험용 스크래치패드 |

## 사용

```bash
cd db-preprocessing
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab preprocessing.ipynb
```

## 관련 repo

- [oneul-nutri/crawling](https://github.com/oneul-nutri/crawling) — 데이터 크롤링·YOLO 학습 파이프라인. 같은 워크스페이스에 sibling으로 clone하면 `../crawling/data/...` 등 상대경로로 참조 가능.
- [oneul-nutri/backend](https://github.com/oneul-nutri/backend) — 정제된 영양소 DB 적재 대상.

## 워크스페이스 셋업

`oneul-nutri/main` 워크스페이스 일원으로 동작:

```bash
git clone https://github.com/oneul-nutri/main.git oneul-nutri
cd oneul-nutri
mise install
mise run clone-all     # modeling 포함 모든 sub-repo clone
```
