# Future Work

## Change the model used to generate the `constraints` field values and increase the applicable fields

In the "Generate dataset specification" stage, we used the Google Gemini API to obtain the value of the `constraints` field, but this may have the following disadvantages:
- The use is affected by the Google Gemini usage limit, and the required value generation cannot be completed quickly.
- The model used is not open-source. If Google changes its policy in the future, the functions in the tool may be affected.

In addition, the generation of `constraints` fields is currently limited to the `minimum` and `maximum` fields. If we change the model to a customizable model in the future, we can consider using the model to generate values ​​for more fields to increase the effectiveness of this function.

## Improve the UI of Data Validation Tool

We can improve the interface of the Data Validation Tool in the future to make it more aesthetically pleasing. In this tool, we can also  add more necessary system messege fields, so that the user can be more clearly informed of the current state of the program.
