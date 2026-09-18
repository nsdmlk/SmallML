# SmallML — Architecture & Logic

> Локальный AutoML-инструмент для малых данных (n < 1000).
> Фокус: научные лаборатории, исследователи, малые датасеты.

**Авторы:** Emelyanov I., Kiselev D.
**Лицензия:** Apache 2.0
**Версия:** 0.1.0
**Дата:** сентябрь 2026

---

## 1. Что это

**SmallML** — desktop-приложение для автоматического машинного обучения на малых данных.

**Ключевые принципы:**

- **Локально.** Все данные остаются на компьютере пользователя.
- **Без регистрации.** Скачал → запустил → работает.
- **Без интернета.** Работает offline.
- **Фокус на малых данных.** n < 1000, где обычный ML не работает.
- **Встроенное цитирование.** Каждый отчёт ссылается на SmallGBM / SmallMLP.

**Целевая аудитория:**

- Научные лаборатории (биология, химия, медицина, физика).
- Университетские research-группы.
- Аспиранты и исследователи.
- Малые компании с малыми данными.

---

## 2. Пользовательский путь

### 2.1. Основной сценарий

1. Пользователь **скачивает** SmallML (PyPI / GitHub Releases).
2. Запускает **локально**.
3. **Загружает** файл с данными (CSV / JSON / Parquet / XLSX / TSV).
4. Выбирает **режим**:
   - **Auto** — всё автоматически.
   - **Manual** — ручной выбор метода, ensemble, voting.
5. **Обучает** модель.
6. Получает **метрики** и **отчёт** на GUI.
7. **Сохраняет** модель с именем и описанием.
8. **Использует** модель для предсказаний на новых данных.
9. **Скачивает** файл с предсказаниями.

### 2.2. Два экрана

**Экран 1 — My Models.**

- Список сохранённых моделей.
- Создание новой модели.
- Удаление модели.
- Просмотр метаданных.

**Экран 2 — Model Use.**

- Выбор модели.
- Загрузка новых данных.
- Предсказание.
- Скачивание результата.

**Без профилей.** Все модели — в одной локальной папке.

---

## 3. Архитектура

### 3.1. Общая схема

┌─────────────────────────────────────────┐
 │         GUI                                                                                             					       │
 │   - My Models                             												       │
 │  - Model Use                             												       │
└──────────────┬──────────────────────────┘
               					       │
               					       │ API (Python functions)
              					      ↓
┌─────────────────────────────────────────┐
 │         Pipeline                 														       │
 │  - io                                    													       │
 │  - preprocess                            												       │
 │  - auto                                  													       │
 │  - manual                                												       │
 │  - train                                 													       │
 │  - evaluate                              												       │
 │  - save                                 													       │
 │  - predict                               													       │
 │  - report                                													       │
└──────────────┬──────────────────────────┘
               					       │
               			                       ↓
┌─────────────────────────────────────────┐
 │         Models                           												       │
 │  - SmallGBM                              												       │
 │  - SmallMLP                              												       │
 │  - AutoGluon                            												       │
 │  - Baselines (XGBoost, LightGBM, ...)   										       │
└─────────────────────────────────────────┘

### 3.2. Стек

**Backend (pipeline):**

- Python 3.10+
- pandas, numpy
- scikit-learn
- LightGBM, XGBoost
- SmallGBM (наш)
- SmallMLP (наш, в разработке)
- AutoGluon
- ONNX (экспорт)
- joblib / pickle (сохранение)

**Frontend (GUI):**

- **Определяет Дима** (TBD).
- Варианты: PyQt, Electron, Tauri, Gradio.

**Формат данных:**

- CSV, JSON, Parquet, XLSX, TSV.

**Формат моделей:**

- `.pkl` (pickle) — основное.
- `.onnx` — для деплоя.

**Формат метаданных:**

- `metadata.json` на каждую модель.

---

# 4. Citations — встроенное цитирование

**В каждом отчёте:**

```markdown
## Citations

If you use this model in your research, please cite:

- Emelyanov, I. (2026). SmallGBM: Gradient Boosting with Robust Leaf Regularization for Small-Sample Tabular Data. Zenodo. DOI: 10.5281/zenodo.21934674

- Emelyanov, I. (2026). SmallMLP: [title]. Zenodo. DOI: [TBD]
```

**В `metadata.json` — массив `citations`.**

**В `README.md`** — раздел про цитирование.

**В `NOTICE`** — attribution.

---

# 5. Форматы данных

### 5.1. Входные форматы

| Формат | Метод pandas         |
| ------------ | ------------------------- |
| `.csv`     | `pd.read_csv`           |
| `.tsv`     | `pd.read_csv(sep='\t')` |
| `.json`    | `pd.read_json`          |
| `.parquet` | `pd.read_parquet`       |
| `.xlsx`    | `pd.read_excel`         |

### 5.2. Выходные форматы

**Предсказания:**

- `.csv` (по умолчанию).
- `.xlsx`.
- `.json`.

**Модели:**

- `.pkl` (pickle) — основное.
- `.onnx` — для деплоя.

**Отчёты:**

- `.html` (по умолчанию).
- `.pdf`.

---

# 6. Установка и запуск

### 6.1. Для пользователя

**PyPI:**

```bash
pip install smallml
smallml
```

**GitHub Releases:**

- Скачать `.exe` (Windows), `.dmg` (macOS), `.AppImage` (Linux).
- Запустить.

### 6.2. Для разработчика

```bash
git clone https://github.com/nsdmlk/smallml.git
cd smallml
pip install -r requirements.txt
python -m smallml
```

---

# 7. Команда

- **Ilya Emelyanov** — ML pipeline, SmallGBM, SmallMLP.
- **Dmitry Kiselev** — GUI, backend.

---

*Small Data · Small Models · Big Impact.*
