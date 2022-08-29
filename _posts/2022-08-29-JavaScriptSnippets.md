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
