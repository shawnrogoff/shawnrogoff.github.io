---
title: Javascript Code Snippets
date: 2022-08-29 10:00:00 -500  
categories: [docs]
tags: [snippets, code] # TAG names should always be lowercase
---
# Useful Javascript Code Snippets I've Used:

## convert a javascript object to camelCase with recursion:

```Javascript
let formattedObject = this.toCamel(notFormattedObject);

toCamel(o) {
    let newO, origKey, newKey, value
    if (o instanceof Array) {
      return o.map(function(value) {
        if (typeof value === "object") {
          value = this.toCamel(value)
        }
        return value
      })
    } else {
      newO = {}
      for (origKey in o) {
        if (o.hasOwnProperty(origKey)) {
          newKey = (origKey.charAt(0).toLowerCase() + origKey.slice(1) || origKey).toString()
          value = o[origKey]
          if (value instanceof Array || (value !== null && value.constructor === Object)) {
            value = this.toCamel(value)
          }
          newO[newKey] = value
        }
      }
    }
    return newO
  }
  ```
---
## Only allow numbers or letters in a text field:

```typescript
<input matInput
    type="text"
    class="text-dark"
    (input)="onEnterStoreNumber($event)"
    placeholder="Store Number"
    matTooltip="Enter store number."
    autocomplete="off"
    maxlength="3"
    ngModel
    (keypress)="keyPressNumbersOnly($event)">
```

```javascript
  keyPressNumbersOnly(event) {
      let charCode = (event.which) ? event.which : event.keyCode;
      // Only Numbers 0-9
      if ((charCode < 48 || charCode > 57)) {
          event.preventDefault();
          return false;
      } else {
          return true;
      }
  }

  keyPressAllowLettersOnly(event) {
      let input = String.fromCharCode(event.keyCode);

      if (/[a-zA-Z]/.test(input)) {
          return true;
      } else {
          event.preventDefault();
          return false;
      }
  }
```
---

## Check if a variable has a value (strips spaces to check empty strings)
```javascript
stringPropertyHasAValue(property) {
    // strip a variable number of spaces to test if it's truthy
    if (property) {
      property = property.replace(/\s/g, '');
    }

    if(property) {
      return true;
    } else {
      return false;
    }
}
```