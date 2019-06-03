## Fill in leading 0s
We first need to know what the expected character length is, as some observations can have 1 leading 0 missing, 2 leading 0s missing, etc. Once we know the character length, we:
1. Repeat the 0's as many times as we need and collapse into a single string
2. Collapse the repeated 0s into a single string
3. Paste the string of 0s with the variable that needs to be fixed
4. Use str_sub() to substitute this long string with only the characters needed. For example, if an observation is missing 2 leading 0s and we know it must be 9 characters, then we have 7 characters remaining. We paste 9 0's to the front so it now has 16 characters (more than enough). We know it should only be a 9 character string so we take only the last 9 characters by specifying minus signs on the start and end points in str_sub().

       char_length <- 9
       df$var_mising_0 <- str_sub(paste0(paste(rep("0", char_length ), collapse = ""),
                                         df$var_missing_0),
                                  -char_length,-1)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczMDE3MzQxNSwxNjg3NzIwMDY1XX0=
-->