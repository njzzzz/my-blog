# Clash Tun mode mixin 

```js
module.exports.parse = ({ content = {}, name, url }, { yaml, axios, notify }) => {
  const rules = content.rules || []
  content = {
    ...content,
    rules: [
       "DOMAIN-KEYWORD,domian.com,Domestic",
      ...rules
    ]
  }
  return content
}

```