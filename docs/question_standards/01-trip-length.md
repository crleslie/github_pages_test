# 🗺️ Trip Length (trip_length)

This question captures the duration of the visitor's outing, grouped under the **Trip Characteristics** theme.

## 📝 Question Details

| Attribute | Value |
| :--- | :--- |
| **Name** | `trip_length` |
| **Label (English)** | How long was your Open Space and Mountain Parks visit TODAY? |
| **Label (Español)** | ¿De cuánto tiempo fue su visita a los Espacios Abiertos y Parques de Montaña hoy? |
| **Group** | Common Questions |
| **Theme** | Trip Characteristics |
| **Format** | Multiple Choice: Single Select (Select one) |
| **Year(s)** | 2024 |

## ⚙️ Response Options (list_trip_length)

The response is a single selection from the following list:

| Value | Label (English) | Label (Español) |
| :--- | :--- | :--- |
| `10-29_minutes` | 10-29 minutes | 10-29 minutos |
| `30-59_minutes` | 30-59 minutes | 30-59 minutos |
| `60_89_minutes` | 60-89 minutes | 60-89 minutos |
| `90_119_minutes` | 90-119 minutes | 90-119 minutos |
| `2-3_hours` | 2 hours to less then 3 hours | De 2 horas a menos de 3 horas |
| `3-5_hours` | 3 hours to less then 5 hours | De 3 horas a menos de 5 horas |
| `5+_hours` | 5 hours or more | 5 horas o más |

## ✅ Data Validation Standards

| Occurs | Standard/Rule | Action |
| :--- | :--- | :--- |
| **DDC** | Only select one option | Specify as select_one question type in survey platform |
| **ADC** | If no response selected | Update null or blank values to 999 |

## 🔗 Related Questions

None.