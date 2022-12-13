## Vimtutor

**Lesson 1 SUMMARY**

1. The cursor is moved using either the arrow keys or the `hjkl` keys. `h` (left)       `j` (down)       `k` (up)       `l` (right)
2. To start Vim from the shell prompt type:  `vim FILENAME <ENTER>`
3. To exit Vim type:     `<ESC>   :q!   <ENTER>`  to trash all changes.
             OR type:      `<ESC>   :wq   <ENTER>`  to save the changes.
4. To delete the character at the cursor type:  `x`
5. To insert or append text type:
         `i`   type inserted text   <ESC>         insert before the cursor
         `A`   type appended text   <ESC>         append after the line
         
**Lesson 2 SUMMARY**

1. To delete from the cursor up to the next word type:        `dw`
2. To delete from the cursor up to the end of the word type:  `de`
3. To delete from the cursor to the end of a line type:       `d$`
4. To delete a whole line type:                               `dd`
5. To repeat a motion prepend it with a number:   `2w`
6. The format for a change command is:
               `operator   [number]   motion`  where:     
   `operator` - is what to do, such as  `d`  for delete
   `[number]` - is an optional count to repeat the motion
   `motion`   - moves over the text to operate on, such as  `w` (word),
                  `e` (end of word),  `$` (end of the line), etc.                  
7. To move to the start of the line use a zero:  `0`
8. To undo previous actions, type:           `u`  (lowercase u)
     To undo all the changes on a line, type:  `U`  (capital U)
     To undo the undo's, type:                 `CTRL-R`
     
**Lesson 3 SUMMARY**

  1. To put back text that has just been deleted, type   `p`.  This puts the
     deleted text AFTER the cursor (if a line was deleted it will go on the
     line below the cursor).
  2. To replace the character under the cursor, type   `r`   and then the
     character you want to have there.
  3. The change operator allows you to change from the cursor to where the
     motion takes you.  eg. Type  `ce`  to change from the cursor to the end of
     the word,  `c$`  to change to the end of a line.
  4. The format for change is:
         `c   [number]   motion`
