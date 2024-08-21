# Validate the dataset

## Validation of dataset specification

完成資料集規範（schema.yaml）的製備後，即可使用frictionless套件中的validate功能確認資料集品質。在檢驗資料集檔案（data.csv）之前，需先檢驗資料集規範（schema.yaml）是否有違反格式或其他錯誤。若資料集規範（schema.yaml）有錯誤，程式將會輸出錯誤數量，並且輸出資料集規範（schema.yaml）中的錯誤細節：

```python
from frictionless import validate

data_filename = 'data.csv'
yaml_filename = f'{data_filename}.schema.yaml'
yaml_report = validate(yaml_filename)

if yaml_report.valid:
    # Perform dataset validation
else:
    print('The .yaml file is not valid:')
    error_num = yaml_report.stats['errors']
        for i in range(error_num):
            print(f'{yaml_report.tasks[0].errors[i].title}:\n{yaml_report.tasks[0].errors[i].message}')
```

## Validation of dataset file

若資料集規範（schema.yaml）無錯誤，則會開始進行資料集檔案（data.csv）驗證。驗證方式同樣是使用frictionless套件中的validate功能。若資料集檔案驗證後無錯誤，則會輸出There are no errors in the dataset.。否則，程式會輸出錯誤數量，並且提供每個錯誤的細節：

```python
from frictionless import validate

data_filename = 'data.csv'
yaml_filename = f'{data_filename}.schema.yaml'

data_report = validate(data_filename, schema=yaml_filename)
error_num = data_report.stats['errors']

if error_num == 0:
    print('There are no errors in the dataset.')
elif error_num == 1:
    print('There is 1 error in the dataset:')
else:
    print(f'There are {error_num} errors in the dataset:')

for i in range(error_num):
    print(f'{data_report.tasks[0].errors[i].title}:\n{data_report.tasks[0].errors[i].message}')