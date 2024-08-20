# 修改 YAML 檔案

## 簡介

在使用 Schema.describe() 功能與大型語言模型自動填入 constraints property 中的最小值與最大值後，由於此時資料集規範（schema.yaml）中的規範尚不一定為研究所需，所以需要對此進行修正。修改資料集規範（schema.yaml）最快的方式為直接打開此YAML檔案，並且對需要修改的欄位手動修正。不過，考量並不是所有研究人員都熟悉YAML檔案的格式，且直接修改YAML檔案仍有可能因粗心造成筆誤，因此這裡開發了一個可讓使用者方便修改資料集規範（schema.yaml）各欄位值的介面工具。

## 1

在選取欲修正的YAML檔案後，即會載入各欄位之資料至使用者介面，使用者可使用上一頁與下一頁按鈕查看各欄位之資料並修正，按下Save按鈕後即會將修正後之資料覆蓋至原檔案上。修正資料集規範（schema.yaml）內容時，內容必須符合[Table Schema](https://datapackage.org/standard/table-schema/)要求。

### Field Constraints 要求

| Constraints property |說明| Type | Fields |
| -------- | -------- | -------- | -------- |
| `required` | Indicates whether this field cannot be `null`. | boolean | all |
| `unique` | If `true`, then all values for that field must be unique within the data file in which it is found. | boolean | all |
| `minLength` | An integer that specifies the minimum length of a value. | integer | collections (string, array, object) |
| `maxLength` | An integer that specifies the maximum length of a value. | integer | collections (string, array, object) |
| `minimum` | Specifies a minimum value for a field. | integer, number, date, time, datetime, duration, year, yearmonth | integer, number, date, time, datetime, duration, year, yearmonth |
| `maximum` | As for `minimum`, but specifies a maximum value for a field. | integer, number, date, time, datetime, duration, year, yearmonth | integer, number, date, time, datetime, duration, year, yearmonth |
| `exclusiveMinimum` | As for `minimum`, but for expressing exclusive range. | integer, number, date, time, datetime, duration, year, yearmonth | integer, number, date, time, datetime, duration, year, yearmonth |
| `exclusiveMaximum` | As for `maximum`, but for expressing exclusive range. | integer, number, date, time, datetime, duration, year, yearmonth | integer, number, date, time, datetime, duration, year, yearmonth |
| `jsonSchema` | A valid JSON Schema object to validate field values. | object | array, object |
| `pattern` | A regular expression that can be used to test field values. | string | string |
| `enum` | The value of the field must exactly match one of the values in the enum array. | array | all |