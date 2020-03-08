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

## 正方体

原理：

* 6个面相同，经过旋转后他们是互相垂直并相交于6个面中线
* 将垂直于同一坐标轴的两个面互相拉开6个面边长的 1/2 

```html
<div class="wrapper">
  <div class="cube">
    <div class="side front">1</div>
    <div class="side back">6</div>
    <div class="side right">4</div>
    <div class="side left">3</div>
    <div class="side top">5</div>
    <div class="side bottom">2</div>
  </div>
</div>
```




```css
@keyframes flash {
  0%{ transform: rotateX(0) rotateY(0); }
  50%{ transform: rotateX(-180deg) rotateY(-90deg); }
  100%{ transform: rotate(360deg) rotateY(360deg); }
}


.wrapper {
  margin: 0 auto;
  width: 50%;
  
  .cube {
    width: 100px;
    height: 100px;
    position: relative;
    margin: 100px auto 0;
    transform-style: preserve-3d;
    animation: flash 3s linear infinite;
    
    .side {
      position: absolute;
      border: 1px solide rgba(0, 0, 0, .5);
      width: 100px;
      height: 100px;
      line-height: 100px;
      text-align: center;
      color: #fff;
    }
    
    .front {
      background: red;
      transform: translateZ(-50px); 
    }
    
    .bottom {
      background: blue;
      transform: rotateX(90deg) translateZ(50px);
    }
    
    .top {
      background: green;
      transform: rotateX(90deg) translateZ(-50px);
    }
    
    .left {
      background: orange;
      transform: rotateY(90deg) translateZ(50px);
      
    }
    
    .right {
      background: brown;
      transform: rotateY(90deg) translateZ(-50px);
    }
    
    .back {
      background: purple;
      transform: translateZ(50px);
    }
  }
}
```