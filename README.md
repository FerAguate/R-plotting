# PCA biplot split by a factor of two levels
# prs = prcomp(x)
# color_vector = rep(c('yes', 'no'), nrow(x) / 2)
```R
mybiplot <- function(prs, color_vector, color_main = 'categories', colors = c('#1B9E77', '#D95F02'), scale = 1, manyfeatures = T, pcs = c(1, 2)){
  library(ggplot2)
  library(ggnewscale)
  library(cowplot)
  suppressWarnings({
    X <- as.data.frame(prs$x)[, pcs]
    colnames(X) <- c('PC1', 'PC2')
    cats <- unique(color_vector)
    Xrot <- as.data.frame(prs$rotation)[, pcs]
    colnames(Xrot) <- c('PC1', 'PC2')
    varPerc <- round(prs$sdev^2 / sum(prs$sdev^2) * 100)[pcs]
    
    pmain <- ggplot(data = X[color_vector == cats[1],], aes(x = PC1, y = PC2)) + 
      xlab(paste0('PC', pcs[1], ' - Var ', varPerc[1], '%')) +
      ylab(paste0('PC', pcs[2], ' - Var ', varPerc[2], '%')) +
      geom_density2d(aes(color = ..level..)) + 
      scale_color_gradientn(paste(color_main, '=', cats[1]), colours = c('transparent', colors[1])) + 
      new_scale_color() +
      geom_density2d(data = X[color_vector == cats[2], ], aes(x = X[color_vector == cats[2], 'PC1'], y = X[color_vector == cats[2], 'PC2'], color = ..level..)) + 
      scale_color_gradientn(paste(color_main, '=', cats[2]), colours = c('transparent', high = colors[2])) +
      geom_hline(yintercept = 0, color = 'grey', linetype = 'dashed') +
      geom_vline(xintercept = 0, color = 'grey', linetype = 'dashed')
    
    if (!manyfeatures){
      pmain <- pmain + geom_segment(data = Xrot, aes(x = 0, y = 0, xend = PC1 * scale, yend = PC2 * scale), arrow = arrow(angle = 2))
      pmain <- pmain + geom_text(data = Xrot * (scale + .2), label = row.names(Xrot))
    }else{ 
      pmain <- pmain + geom_point(data = Xrot, aes(x = PC1 * scale, y = PC2 * scale), color = 'darkgrey')
      pmain <- pmain + geom_text(data = Xrot * (scale + .2), label = row.names(Xrot), check_overlap = T)
    }
    xdens <- axis_canvas(pmain, axis = "x") +
      geom_density(data = X, aes(x = PC1, fill = as.factor(color_vector)), alpha = 0.5, size = 0.2) + 
      scale_fill_manual(values = colors)
    ydens <- axis_canvas(pmain, axis = "y", coord_flip = TRUE) +
      geom_density(data = X, aes(x = PC2, fill = as.factor(color_vector)), alpha = 0.5, size = 0.2) + 
      scale_fill_manual(values = colors) + 
      coord_flip()
    
    p1 <- insert_xaxis_grob(pmain, xdens, grid::unit(.2, "null"), position = "top")
    p2 <- insert_yaxis_grob(p1, ydens, grid::unit(.2, "null"), position = "right")
  })
  return(ggdraw(p2))
}
```R
