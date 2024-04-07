![](https://telegraph-image-cvp.pages.dev/file/fb11a2c53bd7d5acca6d4.png)

### 解决方式：

**可能是由于.vue 文件使用了 jsx 且未指定 lang='jsx'**

```vue
<script lang="jsx">
export defult defineComponent({})
</script>
```
