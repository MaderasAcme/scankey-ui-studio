# ScanKey — ARCHIVO BUENO (Fuente de verdad)

## 0) Regla de oro
- SOLO se trabaja desde: `~/WORK/scankey_app`
- `~/WORK/scankey_app` es symlink a: `~/WORK/scankey/app/scankey-v1a1`
- Si no estás en esa carpeta, NO se ejecuta nada.

## 1) Repo oficial (código)
- Path: `~/WORK/scankey_app`
- Origin: `https://github.com/MaderasAcme/scankey-v1a1.git`
- Rama estable: `main`

## 2) Motor / Backend (FastAPI)
- Backend: `backend/`
- Endpoints verificados (local):
  - `/health`
  - `/api/ocr`  (OCR dual: cliente seguro / taller autorizado)

## 3) Datos (inbox)
- Inbox: `~/WORK/scankey/train_inbox/`
  - `RAW/` = cuarentena (subidas masivas)
  - `READY/` = listo para dataset
  - `BAD/RECOVERABLE/` = salvable (blur/luz/inclinación)
  - `BAD/AUX/` = malo pero útil (robustez/OCR/noise) — NO entrenar directo
  - `BAD/DEAD/` = basura total (sin llave/fuera de foco extremo/recorte brutal)
- Logs:
  - `~/WORK/scankey/train_inbox/triage_*.jsonl`
  - `~/WORK/scankey/train_inbox/sort_ready_*.jsonl` (si aplica)

## 4) Datasets
- v1 estable: `~/WORK/scankey/datasets/v1`
- v2 próxima: `~/WORK/scankey/datasets/v2`
- Por ahora foco: `JIS2I` (A/B)

## 5) Modelos (GCS)
- v1 (EXISTE):
  - `gs://scankey-models-scankey-dc007-95b419/scankey/models/v1/labels.json`
  - `gs://scankey-models-scankey-dc007-95b419/scankey/models/v1/modelo_llaves.onnx`
  - `gs://scankey-models-scankey-dc007-95b419/scankey/models/v1/modelo_llaves.onnx.data`
- v2 (cuando toque):
  - `gs://scankey-models-scankey-dc007-95b419/scankey/models/v2/`

## 6) Cuellos de botella / Bloqueos (lista oficial)
1) Falta cara B (sin B no hay entrenamiento serio ni validación de flujo completo).
2) Falta volumen mínimo por lado (objetivo real: 30A + 30B por referencia).
3) Nombres de archivos sin marca A/B → acaban en AUX/UNSORTED y se pierde tiempo.
4) Confusión RAW vs triage: si haces triage, RAW debe quedar en 0 (normal).
5) Canal de subida no fijado = vuelves a bloquearte.

## 7) MegaFactory (módulos que existen)
- `megafactory/ingest/triage.py`  (RAW → READY/RECOVERABLE/DEAD)
- `megafactory/dataset/sort_ready_to_v2.py`
- `megafactory/dataset/import_ready_by_name.py`
- `megafactory/dataset/recover_aux_by_ahash.py`

## 8) Verificación (sin tocar nada)
Ejecuta:
- `bash megafactory/verify/verify_all.sh`

Eso solo inspecciona y te dice ✅/⚠️/❌.
