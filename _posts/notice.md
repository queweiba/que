1. notice that when you want to use ifelse, the one you used for replacement should have the same lenght. otherwise there is going to be mistakes
ifelse(u_raw$`Patient Number`%in%u_raw_ULOQ$`Patient Number`,u_raw_ULOQ$NAC_mg_L[match(u_raw$`Patient Number`,u_raw_ULOQ$`Patient Number`)],u_raw$NAC_mg_L) 
