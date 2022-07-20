ifelse(u_raw$`Patient Number`%in%u_raw_ULOQ$`Patient Number`,u_raw_ULOQ$NAC_mg_L[match(u_raw$`Patient Number`,u_raw_ULOQ$`Patient Number`)],u_raw$NAC_mg_L) 
