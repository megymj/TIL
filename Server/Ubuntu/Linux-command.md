# Linux command



<br>



## 1. ; & &&

> reference link: https://opentutorials.org/module/2538/15818, [geeksforgeeks](https://www.geeksforgeeks.org/difference-between-chaining-operators-in-linux/)

```bash
mkdir project;
cd project;

mkdir project &&
cd project &&
```

**;** : The execution of the second command is independent of the exit status of the first command. If the first command does not get successfully executed, then also the second command will get executed.

**&&** : The second command will only execute if the first command has executed successfully

**&** : It makes the command run in the background, and run the second command simultaneously