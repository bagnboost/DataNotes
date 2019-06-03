## Fill in leading 0s
We first need to know what the expected character length is, as some observations can have 1 leading 0 missing, 2 leading 0s missing, etc. Once we know the character length, we:
1. Paste char_length of 0s in front of the text
2. 

    char_length <- 9
    df$var_mising_0 <- str_sub(paste0(
                                                                paste(rep("0", char_length ), collapse = ""),
                                                                 "1234"),
                                                    -char_length,-1)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIzMzE0NTUzMCwxNjg3NzIwMDY1XX0=
-->