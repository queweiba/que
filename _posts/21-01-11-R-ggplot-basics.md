**坐标系的改变**
1. 修改横纵坐标标题
```r
+xlab("title")+ylab("title")
```

2. 修改横纵坐标极限
```r
xlim(0,29)
```

3. 调整坐标系的标识
```r
scale_x_continuous(breaks = round(seq(min(SIM1$TIME), max(SIM1$TIME), by = 24),1))
```

4. 修改label 标题
```r
labs(color="Birth Weight (g)")
```

**Theme**

 1. 修改图片字体
```r
theme(text=element_text( family="Calibri"))
```

2. 提高整体字体大小
```r
theme_bw(base_size = 13)
```

3. 修改坐标系字体大小以及坐标标题字体大小
```r
theme(axis.title=element_text(size=13))+ theme(axis.text=element_text(size=12))
theme(plot.title = element_text(hjust = 0.5),axis.title=element_text(size=13))
```

4. 去掉legend
```r
theme(legend.position="none")
```

**其它**
1. discrete 数据画图颜色渐变修改
```r
scale_colour_viridis_d(option="plasma",end=0.85)
#用plasma这个色系，只用0.85到0的这个部分
```
```r
scale_colour_viridis(option="plasma")
```

2. 在线条后面加上label
```r
geom_dl(data=xi,aes(label = as.factor(paste(BW,"g"))), method = list(dl.combine( "last.points")), cex = 0.8)
```


**合并图片**

```r
library(cowplot)
plot_grid(a,b, labels = c('A', 'B'))
```
