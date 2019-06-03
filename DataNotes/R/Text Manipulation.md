## Fill in leading 0s
We first need to know what the expected character length is, as some observations can have 1 leading 0 missing, 2 leading 0s missing, etc.

    char_length <- 9
    df$var_mising_0 <- str_sub(paste0("000000000",df$var_missing),-char_length,-1)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzM3NDQyOTA3XX0=
-->