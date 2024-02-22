
# FormKit
Used to create any type of inputs (this is documenting skill sample in job interview)

## Variables
### Exist for styling purposes

| Parameter   | Type     |
:--------     | :------- |
| `id`        | `string`|

### For toggeling VuePersianDatePicker on and off

| Parameter   | Type     |
:--------     | :------- |
| `show`      | `string`|

### The state of input either is focused or not

| Parameter   | Type     |
:--------     | :------- |
| `focus`      | `boolean`|


## Methods
### changeInput
Fires onChange emit and return the input value

#### ex:
```
<TextInput @onChange="(value) => (console.log(value))" />
```
 
### onDateChange
Fires onChange emit and return the date input value (depend on the input type you specify, the event name will be onDateChange or onChange)

#### ex:
```
<TextInput @onDateChange="(value) => (console.log(value))" />
```

### persianNumberToEnglish
Called to convert english number letters to persian

### onChange
Called to filter input value from spesific regex and return it as change and input emits

#### ex:
```
<TextInput @change="(value) => (console.log(value))" 
           @input="(value) => (console.log(value))"
/>
```

### onFocus
To change the focus state



## Props
### This prop will be binded to the input value

| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `value`     | `[string, number]` | required         |

#### ex:
```
<TextInput value="some text here" />
```


### The name attribute of input will be binded to this value

| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `name`      | `string` | ""                         |

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
/>
```


### Label of the input

| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `label`     | `string` | ""                   |

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
           label="full name"
/>
```

### Type of the input

| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `type`  | `text \| number \| select \| password \| date \| textarea \| select2` | "text"|

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
           label="full name"
           type="text"
/>
```

### Select box's options

| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `options`     | `Array` | null                   |

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
           label="country"
           type="select2"
           :options="countriesList"
/>
```

### Disabled state of input

| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `disabled`  | `boolean` | false                     |

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
           label="country"
           type="select2"
           :options="countriesList"
           :disabled="ture"
/>
```


### Icon of the input

| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `endIcon`  | `string`  | null                      |

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
           label="country"
           type="select2"
           :options="countriesList"
           :disabled="ture"
           endIcon="flag"
/>
```

### option name attribute of input
| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `optionName`  | `[string, number]`  | null                      |

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
           label="country"
           type="select2"
           :options="countriesList"
           :disabled="ture"
           :optionName="["something", 0]
/>
```

### option value attribute of input
| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `optionValue`  | `[string, number]`  | null                      |

#### ex:
```
<TextInput value="some text here" 
           name="my-text-input" 
           label="country"
           type="select2"
           :options="countriesList"
           :disabled="ture"
           :optionName="['United State of America', 0]"
           :optionValue="['USA', 0]"
/>
```

### Minimum value of number input
| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `min`  | `number`     | 1                      |

#### ex:
```
<TextInput value="12" 
           name="my-text-input" 
           label="Quantity"
           type="number"
           :disabled="ture"
           :min="10"
/>
```
### Maximum value of number input
| Parameter   | Type     | Required / Default value   |
| :--------   | :------- | :------------------------- |
| `max`  | `number`     | 10                      |

#### ex:
```
<TextInput value="12" 
           name="my-text-input" 
           label="Quantity"
           type="number"
           :disabled="ture"
           :min="10"
           :max="100"
/>
```

## Emits
### Input
Return a string passed through an simple regex depend on input type you specify
| Parameter    | Return Type |
| :--------    | :-------    | 
| `@input`     | `string`    |

#### ex:
```
<TextInput   :value="searchText" 
             @input=(value)=>(console.log(value))
/>
```

### Change
Return a string passed through an simple regex depend on input type you specify
| Parameter    | Return Type |
| :--------    | :-------    | 
| `@change`     | `string`    |

#### ex:
```
<TextInput   :value="searchText"
             @change=(value)=>(console.log(value))
/>
```