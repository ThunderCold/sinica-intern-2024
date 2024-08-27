# Weather Dataset Validation

## Dataset

- [氣象站資料-海洋研究站-2023](https://data.depositar.io/dataset/ltser-lyudao-weather/resource/cd83f13c-cbf0-446c-a3f6-59085e7fbc1f)

## Dataset specification

- [LTSER Lyudao 環境監測-氣象資料 - 資料集規範](https://data.depositar.io/dataset/ltser-dataset-specification/resource/1c0245da-44d4-4d3e-ad29-5597aae84b4f)

## Validation result

The validation result shows "There are 1000 errors in the dataset." (In fact, there should be over 1000 errors.)

The first error is `Constraint Error`. In row at position 746 of field "SolarRadiation", the value is 1276.9. However, according to the dataset specification, the maximum value set for this field is 1100.0, so an error message appears.

The following errors are all `Type Error`. In the "WindSpeed", "GustSpeed", and "precipitation" fields, there are values that appear as "NA", which does not match the "number" type of these fields, so errors occur.

![](_static/weather.png)