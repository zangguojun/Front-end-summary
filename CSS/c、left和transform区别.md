## 让一个元素左移100px，使用left和transform有什么区别？

使用left等属性来设置动画会**一直触发浏览器的重绘**，而使用CSS3中国的**transform会采用GPU硬件加速**，**不会触发重绘**，性能更好

#### 硬件加速的原理

DOM树和CSSOM合并后形成Render树。渲染树中包含了大量的渲染元素，**每一个渲染元素会被分到一个图层中**，**每个图层又会被加载到GPU形成渲染纹理**。GPU中的transform是不会触发重绘的，这一点非常类似3D绘图功能。**最终这些使用tranform的图层都会由独立的合成器进程进行处理**。