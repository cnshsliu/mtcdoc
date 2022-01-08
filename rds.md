# Input: List

A input list is also known as a select input which has multiple options to choose from.

When you define a list in workflow designer, you should:

- give it a name starting with "select\_"

To give it a default value: you should:

- give it a default value in name field

To give it options, you may:

- define options in option field directly, options string are separated by semicolon, such as "option1;option2;option3";
- refer to a List, in option field, input "R:list_name".

To make cascaded list. you may:

- use T:cascade_list_name, for example, you may have province list "select_A" defined as "R:province_list;T:select_B", then, you may define select_B as "R:city_list", then, once use pick a province from select_A, select_B will get the selected value from select_A, and use it as list key to refresh options for select_B, say, get all cities of the selected city.
