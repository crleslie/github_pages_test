# 🏃 Primary Activity (`activity_primary`)

This question determines the visitor's main activity during their Open Space and Mountain Parks visit.

## 📝 Question Details

| Attribute | Value |
| :--- | :--- |
| **Name** | `activity_primary` |
| **Label (English)** | What was your PRIMARY ACTIVITY at Open Space and Mountain Parks for TODAY? |
| **Label (Español)** | ¿Cuál fue su SU ACTIVIDAD PRINCIPAL HOY en los Espacios Abiertos y Parques de Montaña? |
| **Group** | Common Questions |
| **Theme** | Trip Characteristics |
| **Format** | Multiple Choice: Single Select (Select One) |
| **Year(s)** | 2024 |

## ⚙️ Response Options (list_primary_activity)

The response is a single selection from the following list:

| Value | Label (English) | Label (Español) |
| :--- | :--- | :--- |
| `running` | Running | Correr |
| `horseback_riding` | Horseback riding | Paseos a caballo |
| `e-biking` | Biking (e-bike) | Bicicleta (eléctrica) |
| `biking` | Biking (traditional bike) | Bicicleta (tradicionale) |
| `dog_walking` | Dog walking | Caminar al perro |
| `climbing_bouldering` | Climbing / Bouldering | Escalada/escalada sin cuerda |
| `fishing` | Fishing | Pesca |
| `hiking_walking` | Hiking / Walking | Senderismo / caminar |
| `other` | Other | Otro |

## 🔗 Related Question: Primary Activity – Other

This question has a **follow-up question** (`activity_primary_other`).
If the respondent selects **"Other"** from this choice list, they are prompted to specify their activity in the follow-up question.

| Attribute | Value |
| :--- | :--- |
| **Name** | `activity_primary_other` |
| **Label (English)** | Other activity |
| **Format** | Open-Ended (Text): Short Answer |
| **Character Limit** | 255 |
| **Examples** | rollerblading, mountain biking, paragliding, etc. |

## ✅ Data Validation Standards

| Occurs | Standard/Rule | Action |
| :--- | :--- | :--- |
| **DDC** | Only select one option | Specify as select_one question type in survey platform |
| **ADC** | If no response selected | Update null or blank values to 999 |
| **ADC** | Splitting | Split each activity choice into separate columns. For each column use 1=true and 0=false |
| **DDC (Related)** | If respondent didn't select ‘other’ for ‘primary_activity’ | Update null or blank values to 777 for `activity_primary_other` |
| **ADC (Related)** | If ‘primary_activity_other’ is selected and no written response is provided | Update null or blank values to missing 999 for `activity_primary_other` |
| **ADC (Related)** | If ‘primary_activity_other’ written response is an existing category in `activity_primary` | Recode `activity_primary` with the appropriate category (e.g., jogging to 'Running'). Leave write-in text as-is. |
| **ADC (Related)** | If ‘primary_activity_other’ written response is valid, but not a provided activity choice | Recode to “Other” (activity) for `activity_primary_other`. (Examples: Paragliding and Rollerblading) |
| **ADC (Related)** | If ‘primary_activity_other’ written response is an invalid response | Update to 888 for `activity_primary_other` |