# TextInput in ReactNative

---
## Summary

- Introduction: a brief explanation about TextInput component
- Use Cases: textual input, numeric input, passwords
- Advanced Usage: controlled input, masks
- Troubleshooting: flickering, rerendering
- References

---
## Introduction

From the React Native Core Component library there may be one of the most used components out there: the TextInput. It is so fundamental in application design that there may be not many applications that avoid using it. If you want to know more about one of the most used components from React Native check this article out.

---
## Use Cases

### Textual Input

Perhaps the most simple and common use case for TextInput is the pure text input handling which doesn't require further configuration as shown in the example implementation:

```js
import React, { useState } from 'react';
import { Text, TextInput, View } from 'react-native';

const Application = () => {
  const [text, setText] = useState('');
  return (
    <View>
      <TextInput
        onChangeText={text => setText(text)}
        defaultValue={text}
      />
      <Text>
        {text}
      </Text>
    </View>
  );
}

export default Application;
```

### Mail adress and URL input

Although the simple pure textual input may fit for many basic use cases in application development there may be a need for some different kind of textual inputs such as url or email which require a simple configuration of the `keyboardType` property to use the `email-address` value (cross-platform) or the platform specific `url, twitter, web-search` (iOS), while there are no android specific values for this same feature:

```xml
  <TextInput
    keyboardType="email-address"
    onChangeText={setText}
    defaultValue={text}
  />
```

### Phone number input

To achieve this feature the TextInput  must be configured with the `keyboardType` property defined to `phone-pad` cross-platform value or `name-phone-pad` iOS specific value:

```xml
  <TextInput
    keyboardType="phone-pad"
    onChangeText={setText}
    defaultValue={text}
  />
```

### Weights, quantities input

This feature may cover different use cases as there are different types of numeric input such as weight, distance or quantities inputs. The TextInput must be configured with the `keyboardType` property defubed to one of the `decimal-pad, numeric` cross-platform values or `numbers-and-punctuation` iOS specific values:

```xml
  <TextInput
    keyboardType="decimal-pad"
    onChangeText={setText}
    defaultValue={text}
  />
```

### Passwords

This use case refers to the most common case of secure textual input seen across any Sign In formulary in the general applications. To enable the hidden content TextInput one must enable the property `secureTextEntry` on the TextInput component as shown in the example:

```xml
  <TextInput
    secureTextEntry
    onChangeText={setText}
    defaultValue={text}
  />
```

---
## Advanced Usage

### Controlled input

Now that we know how to instance a basic version of TextInput with custom keyboard configuration it may be interesting to get to know how to control that input so that we're able to modify TextInput content as the user types. For that we need to change somethings on our first example but basically changing the `defaultValue` property to `value` prop and applying some text processing on the text setter:

```js
import React, { useState } from 'react';
import { Text, TextInput, View } from 'react-native';

const Application = () => {
  const [text, setText] = useState('');
  function onChangeText(text) {
    setText(text.replaceAll(/[0-9]/g,'X'))
  }
  return (
    <View>
      <TextInput
        onChangeText={onChangeText}
        value={text}
      />
      <Text>
        {text}
      </Text>
    </View>
  );
}

export default Application;
```

In this example the TextInput setter processing is made in such a way that all numbers are replaced by the character X. This is the basic method used by many TextInput masks technologies out there.

### Masks

TextInput masks are an easy to achieve feature when using third-party libraries such as [react-native-text-input-mask](https://github.com/react-native-text-input-mask/react-native-text-input-mask) which are able to resolve the mask feature quite easily for the developer. To achieve TextInput masking with this library one may setup a simple mask string which describes the desired text format in the mask. The most basic way to use is in the example as follows:

```js
import TextInputMask from 'react-native-text-input-mask';


<TextInputMask
  mask={"+1 ([000]) [000] [00] [00]"}

  onChangeText={(formatted, extracted) => console.log(formatted, extracted)}
/>
```

The mask used in the example above is commonly used for phone inputs but there are many more:

Type|Mask
-|-
Phone Number|"+1 ([000]) [000] [00] [00]"
Credit Card|"[0000] [0000] [0000] [0000]"
Price|"$[999990],[99]"


---
## Troubleshooting

### Flickering

These problems may occur when using controlled TextInput and setting the `value` prop directly. From the React Native Dev pages [2]:

> [Value prop](https://reactnative.dev/docs/textinput#value): [...] For most uses, this works great, but in some cases this may cause flickering - one common cause is preventing edits by keeping value the same. In addition to setting the same value, either set editable={false}, or set/update maxLength to prevent unwanted edits without flicker.

> [maxLength prop](https://reactnative.dev/docs/textinput#maxlength): Limits the maximum number of characters that can be entered. Use this instead of implementing the logic in JS to avoid flicker.

To resolve flickering problems pay attention to these lines on the documentation and try to debug whether your setters are messing with the component internal value state.

### Rerendering

Sometimes when using a controlled state TextInput one may perceive that the components around the input are rerendering and this may be caused by undesired value setters in the component structure. To resolve this problem try to debug the setters and test whether the use of `useMemo` or `useCallback` may resolve your issue.

---
## References

Below are all reference documentation used to produce this article:

1. https://callstack.github.io/react-native-paper/text-input.html

2. https://reactnative.dev/docs/textinput

3. https://reactnative.dev/docs/handling-text-input

4. https://lefkowitz.me/visual-guide-to-react-native-textinput-keyboardtype-options/

5. https://github.com/react-native-text-input-mask/react-native-text-input-mask