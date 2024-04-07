## Tree-shaking

- **仅在 esm 模式下有效**
- \*_标记为无副作用注释 `/_#**PURE**\*/`, 一般用于顶层函数调用
- **动态 dead code， tree-shaking， 通过**`definePlugin` 修改`FLAG`

```javascript
if (FLAG) {
  console.log("just log at dev mode");
}
```

---

## Complier、 Runtime(Render)

```javascript
const html = `
<div id="xxx">
</div>
`;
Render(Compiler(html));
```

## Renderder

```javascript
const ComponentA = function MyComponent() {
  return {
    tag: "div",
    props: {},
    children: ["xxxx"],
  };
};

const ComponentB = {
  tag: ComponentA,
};

const ComponentC = {
  tag: "div",
  props: {
    onClick() {
      alert("hello");
    },
  },
  children: ["xxx", ComponentA],
};
function mountElement(vnode, container) {
  const tag = vnode.tag;
  const dom = document.createElement(tag);

  for (prop in vnode.props) {
    if (/^on/.test(prop)) {
      dom.addEventListener(prop.substr(2).toLowercase(), vnode.props[prop]);
    }
  }

  if (typeof vnode.children === "string") {
    dom.appendChild(document.createTextNode(vnode.children));
  }
  if (Array.isArray(vnode.children)) {
    vnode.children.forEach((child) => {
      renderer(child, dom);
    });
  }

  container.appendChild(dom);
}

function mountComponent(vnode, container) {
  // 调用组件函数，获取组件要渲染的内容（虚拟 DOM）
  const subtree = vnode.tag();
  // 递归地调用 renderer 渲染 subtree
  renderer(subtree, container);
}

function renderer(vnode, container) {
  if (typeof vnode.tag === "string") {
    mountElement(vnode, container);
  } else if (typeof vnode.tag === "function") {
    mountComponent(vnode, container);
  }
}
```

### Effective

```javascript
let activeEffect = null;
let effects = new WeakMap();

function effect(fn, options) {
  activeEffect = fn;
  fn();
}

function reactive(obj) {
  return new Proxy(obj, {
    get(target, key, receiver) {
      let deps = effects.get(target);
      if (!deps) {
        effects.set(target, (deps = new Map()));
      }
      let efts = deps.get(key);
      if (!efts) {
        deps.set(key, (efts = new Set()));
      }
      efts.add(activeEffect);
      return Reflect.get(target, key, receiver);
    },
    set(target, key, value, receiver) {
      const deps = effects.get(target);
      if (!deps) {
        return Reflect.set(target, key, value, receiver);
      }
      const efts = deps.get(key);

      Reflect.set(target, key, value, receiver);
      for (effect of efts) {
        effect();
      }
      return true;
    },
  });
}

const origin = { a: 1 };

const obj = reactive(origin);

effect(() => {
  console.log(obj.a);
});
obj.a++;
```

#### usage

```javascript
const origin = { a: 1 };

const obj = reactive(origin);

effect(() => {
  obj.a++;
});
```

### Proxy

| **操作**         | **拦截函数**       |
| ---------------- | ------------------ |
| **fun()**        | **apply**          |
| **'key' in obj** | **has**            |
| **for...in**     | **ownKeys**        |
| **delete obj.x** | **deleteProperty** |
|                  |                    |
|                  |                    |
