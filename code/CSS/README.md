# CSS

## CSS 实现一个硬币旋转的效果

```css
@keyframes spin {
  0% {
    transform: rotateY(0deg);
  }
  
  100% {
    transform: rotateY(360deg);
  }
}

.coin {
  position: relative;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background: #f5f5f5;
  transform-style: preserve-3d;
  animation: spin linear infinite 2.5s;
  
  &:after {
    content: ' ';
    position: absolute;
    top: 0;
    left: 50%;
    display: block;
    margin-left: -1px;
    width: 2px;
    height: 100%;
    background: red;
  }
}
```

## 如何画一个三角形

利用边框均分原则

```css
.triangle {
  width: 0;
  height: 0;
  border-top: 10 solid red;
  border-right: 10 solid transparent;
  border-bottom: 10 solid transparent;
  border-left: 10 solid transparent;
}
```