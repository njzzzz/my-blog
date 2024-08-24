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

# Clash Verge Tun mode script

```js

function main(config = {}, profileName) {
  const rules = config.rules ?? []
  const whiteListDomian = [
    'DOMAIN-KEYWORD,xxxxx,Domestic',
  ]
  config = {
    ...config,
    rules: [
      ...rules,
      ...whiteListDomian
    ]
  }
  return config;
}


```