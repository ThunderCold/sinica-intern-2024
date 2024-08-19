# 以 YAML 格式生成 Table Schema 描述

## 使用 frictionless 套件

依照使用者上傳的資料集檔案（data.csv），使用 Frictionless 套件中的 Schema.describe() 功能生成資料集規範（schema.yaml）。

```{note}
資料集檔案（data.csv）格式限制：<br>
第一列為各欄位之 name，其餘每一列皆為一筆資料。
```

```python
from frictionless import Schema

yaml_filename = f'{data_filename}.schema.yaml' # 將資料集規範命名為 資料集名稱 + .schema.yaml
schema = Schema.describe(data_filename)
schema.to_yaml(yaml_filename)
```

```python
with open(description_filename, newline='') as csvfile:
        with open(yaml_filename, 'r', encoding='utf-8') as file:
            csv_reader = csv.reader(csvfile)
            listReport = list(csv_reader)
            yaml_data = yaml.safe_load(file)
            for i in range(len(yaml_data['fields'])):
                yaml_data['fields'][i]['title'] = listReport[1][i]
                yaml_data['fields'][i]['description'] = listReport[2][i]
        with open(yaml_filename, 'w', encoding='utf-8') as file:
            yaml.safe_dump(yaml_data, file)
```

## 使用 Gemini 模型填入 constraints property

但因由 Schema.describe() 功能生成的資料集規範僅包含 name 與 type 兩個property，無法有效進行資料集驗證功能。因此，依照使用者上傳的資料欄位說明檔案（description.csv），將其內容填充至資料集規範（schema.yaml）中，並且使用大型語言模型判斷各欄位之constraints property內容。

這裡使用的大型語言模型為 Google Gemini 中的 gemini-1.5-flash，使用者可先申請免費 API，並將 API Key 填入應用程式欄位中。在選定資料集檔案（data.csv）與資料欄位說明檔案（description.csv）後，程式會自動連接 Gemini 模型，並且詢問每個資料欄位的理論最小值與最大值，若模型輸出結果有最小值或最大值，就會將該值填入資料集規範（schema.yaml）中；否則，若模型輸出為 Null（意即無法判斷）最小值或最大值，則會跳過填入該欄位。

免費版gemini-1.5-flash模型API的使用限制為：15 requests per minute，1500 requests per day。為避免瞬間使用過多資源導致出現ResourceExhausted錯誤，偵測每個欄位進行生成會有一段間隔時間（目前設定為7秒）。

```{note}
資料欄位說明檔案（description.csv）格式限制：<br>
第一列為各欄位之 name，第二列為各欄位之 title，第三列為各欄位之 description，第四列為各欄位之 type。
```

- 用以詢問 Gemini 模型的文本：
```text
name: <name>\ntitle: <title>\ndescription: <description>\ntype: <type>\n\nThe above is the description of a data field. What is the maximum reasonable real-world value ​​for this data field? Please output this value ​​directly without any additional explanation. If the maximum reasonable real-world value ​​cannot be determined, "Null" is output.
```
```text
name: <name>\ntitle: <title>\ndescription: <description>\ntype: <type>\n\nThe above is the description of a data field. What is the minimum reasonable real-world value ​​for this data field? Please output this value ​​directly without any additional explanation. If the minimum reasonable real-world value ​​cannot be determined, "Null" is output.
```

```python
import google.generativeai as genai

def askGemini():
    
    genai.configure(api_key = api_key)
    model = genai.GenerativeModel('gemini-1.5-flash')

    max_question = f'name: {name}\ntitle: {title}\ndescription: {description}\ntype: {type}\n\nThe above is the description of a data field. What is the maximum reasonable real-world value ​​for this data field? Please output this value ​​directly without any additional explanation. If the maximum reasonable real-world value ​​cannot be determined, "Null" is output.'
    max_response = model.generate_content(
        contents=max_question,
        safety_settings=s_settings
    )
    min_question = f'name: {name}\ntitle: {title}\ndescription: {description}\ntype: {type}\n\nThe above is the description of a data field. What is the minimum reasonable real-world value ​​for this data field? Please output this value ​​directly without any additional explanation. If the minimum reasonable real-world value ​​cannot be determined, "Null" is output.'
    min_response = model.generate_content(
        contents=min_question,
        safety_settings=s_settings
    )

    return max_response.text.strip(), min_response.text.strip()
```

在使用 Gemini 模型填入 constraints property 時，應用程式介面中的 System Messege 欄位會顯示系統資訊，預期中有可能會看到以下資訊：
- `Create <yaml_filename> sucessfully`：成功使用 Schema.describe() 功能生成資料集規範（schema.yaml）。
- `Detecting <name> field...`：正在使用模型生成 `<name>` 欄位之理論最小值與最大值。
- `<name> field:\nminimum:<min>, maximum:<max>`：已經完成 `<name>` 欄位之檢測，模型生成之理論最小值為 `<min>`，理論最大值為 `<max>`。
- `ResourceExhausted error occurred, will automatically retry`：超過API資源使用限制，在額外等候一段間隔時間後將自動重試。
- `Multiple ResourceExhausted errors occurred, terminating AI detection`：多次嘗試後仍超過API資源使用限制，將自動中斷模型生成。
- `InvalidArgument error occurred, please check API key`：API Key 可能有錯誤。
- `Unexpected error occurred`：Gemini 模型生成過程出現非預期錯誤。
- `AI detection completed`：已經完成該資料集規範（schema.yaml）之Gemini 模型生成。