# 在 Binder 服務中驗證資料集

>Binder is an online service for building and running computational environments in the browser, which can be used to reproduce and demonstrate research results hosted by code repositories.
>
>Source: [Depositar Docs](https://docs.depositar.io/en/stable/user-guide/binder.html)

## Introduction

除了使用 Data Validation Tool 對資料集進行品質驗證之外，使用者在使用 depositar 服務時，亦可將修改後的驗證程式連同資料集上傳，在開啟 Binder 服務後，即可使用驗證程式對資料集進行驗證。

```{note}
在 Binder 服務上的驗證程式僅有資料集驗證功能，不包含dataset specification產生與修正，使用者需事先自行完成dataset specification之製備並一同上傳至資料集內。
```

## 實現方式

將 Data Validation Tool 的驗證部分程式碼進行修改，改寫為一 .ipynb 檔案，將系統產生之提示訊息直接輸出供使用者參考，並且新增記錄至一驗證結果檔案（validate_report.txt）中。

```python
from frictionless import validate

data_filename = 'data.csv'
yaml_filename = f'{data_filename}.schema.yaml'
report_filename = f'{data_filename}_validate_report.txt'

def messege(str):
    print(str)
    with open(report_filename, 'a') as file:
        file.write(str)

with open(report_filename, 'w') as file:
    pass

yaml_report = validate(yaml_filename)

if yaml_report.valid:
    data_report = validate(data_filename, schema=yaml_filename)
    error_num = data_report.stats['errors']
    if error_num == 0:
        messege('There are no errors in the dataset.')
    elif error_num == 1:
        messege('There is 1 error in the dataset:')
    else:
        messege(f'There are {error_num} errors in the dataset:')
    for i in range(error_num):
        messege(f'{data_report.tasks[0].errors[i].title}:\n{data_report.tasks[0].errors[i].message}')
else:
    messege('The .yaml file is not valid:')
    error_num = yaml_report.stats['errors']
    for i in range(error_num):
        messege(f'{yaml_report.tasks[0].errors[i].title}:\n{yaml_report.tasks[0].errors[i].message}')
```

## 修改程式碼

