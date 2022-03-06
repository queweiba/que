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
theme(text=element_text(family="Calibri"))
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

3.给图加上text
```r
p <- ggplot(mtcars, aes(x=wt, y=mpg, label=rownames(mtcars)))
p + geom_text(fontface=3)
p + geom_text(aes(fontface=am+1))
p + geom_text(aes(family=c("serif", "mono")[am+1]))
```

**合并图片**
1. cowplot
```r
library(cowplot)
plot_grid(a,b, labels = c('A', 'B'))
```

2. grid.arrage
gridExtra builds upon gtable to arrange multiple grobs on a page.
```r
library(gridExtra)
library(grid)
library(ggplot2)
library(lattice)
p <- qplot(1,1)
p2 <- xyplot(1~1)
r <- rectGrob(gp=gpar(fill="grey90"))
t <- textGrob("text")
grid.arrange(t, p, p2, r, ncol=2)
```
另外一种更为复杂的排列方式
```r
grid.arrange(
  grobs = xxx,
  widths = c(3, 1, 1),
  layout_matrix = rbind(c(1, 2, NA),
                        c(3, 3, 4))
)
```

##提取共同legend
```r
g_legend<-function(a.gplot){
  tmp <- ggplot_gtable(ggplot_build(a.gplot))
  leg <- which(sapply(tmp$grobs, function(x) x$name) == "guide-box")
  legend <- tmp$grobs[[leg]]
  return(legend)}
mylegend<-g_legend(a)
library(gridExtra)
p3 <- grid.arrange(arrangeGrob(a + theme(legend.position="none"),
                               b + theme(legend.position="none"),mylegend,
                               nrow = 1,widths = c(5,5,2.3))
)
```

##颜色的改变
1. 手动改变颜色
```r
scale_color<-scale_color_manual(values = c("#52854C","#C3D7A4","#FFDB6D","#D16103"),limits=c('(0,30]', '(30,50]' ,'(50,100]', '(100,Inf]'))

p <- ggplot(mtcars, aes(mpg, wt)) +
  geom_point(aes(colour = factor(cyl)))
p + scale_colour_manual(values = c("red", "blue", "green"))
# It's recommended to use a named vector
cols <- c("8" = "red", "4" = "blue", "6" = "darkgreen", "10" = "orange")
p + scale_colour_manual(values = cols)
```
