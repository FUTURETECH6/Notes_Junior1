# Hidden surface removal

## Object-space

Object precision

```pseudocode
for (each object in the world) {

}
```



**back-face culling**

用点积（平面法向量和视线方向向量）剔除所有背面

无法解决遮挡的问题



**Painter's Algorithm**

无法解决循环遮挡的问题



**Warnock's Area Subdivision**



## Image-space

Image precision

```pseudocode
for (each pixel in the images) {

}
```



**z-buffer**



**BSP Tree**

两个相交图形，将其中一个图形切成两份，以另一个图形为root，切好的两个图形分别作为root的子节点。仅需比较相机在root的位置就可以决定画图的顺序（无需z-buffer）



**k-d Tree**



**Ray Casting**



# Anti-aliasing

**Aliasing**

采样率不够造成的