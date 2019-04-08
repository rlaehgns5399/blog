Homework #3
1. Do “mkdir ~/chmod_test”, then do “cd ~/chmod_test”, then do the following commands one by
  one.
  for t in {0..21}; do if [ $t -lt 10 ]; then touch 0"$t"; else touch $t; fi done
  i=1; for t in {1..7}; do if [ $t -lt 10 ]; then t=0"$t"; fi; chmod 000"$i" $t; i=$(($i+1)); done
  i=1; for t in {8..14}; do if [ $t -lt 10 ]; then t=0"$t"; fi; chmod 00"$i"0 $t; i=$(($i+1)); done
  i=1; for t in {15..21}; do chmod 0"$i"00 $t; i=​$(($i+1)); done
  mkdir 22
  chmod 000 22
  Then, do “ls –l”.
- Take a screenshot

![image-20190404022217355](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404022217355.png)

- List files which can be read by file owner

```
00, 04, 05, 06 ,07
```

- List files which can be executed by file group members

```
00, 11,12,13,14
```

- List files which can be written by others.
```
02, 03, 06, 07
```



2. Do the following commands one by one.
  i=1; for t in {1..7}; do if [ $t -lt 10 ]; then t=0"​$t"; fi; chmod 700"$i" ​$t; i=$((​$i+1)); done
  i=1; for t in {8..14}; do if [ $t -lt 10 ]; then t=0"​$t"; fi; chmod 70"$i"0 ​$t; i=$((​$i+1)); done
  i=1; for t in {15..21}; do chmod 7"$i"00 ​$t; i=$((​$i+1)); done
  Then do “ls –l”.
- Take a screenshot

![image-20190404023253136](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404023253136.png)

 - Discuss the difference between files with “S” or “T” and files “s” or “t”.

```
t vs T
sticky비트로, t는 기타 사용자의 실행권한이 있음을 , T는 실행권한이 없음을 뜻한다.
s vs S 
setgid 비트가 사용됬을때, 실행 권한 자리에 권한이 있으면 s, 권한이 없을경우 S로 표시된다.
 + 앞의 s와 S는 setuid비트를 나타냄
```





3. Do “rm ??”, then press “y” key whenever cursor waits for your input. Then, do “rmdir 22”
- Take a screenshot

![image-20190404025428102](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404025428102.png)

 - List the files which remove without the interactive question. And describe the common
    characteristics of these files.

    ```
    00, 16, 17, 20, 21
    00 : -rw-rw-r--
    16 : --wS--S--T
    17 : --ws--S--T
    20 : -rwS--S--T
    21 : -rws--S--T
    
    user 의 write 권한이 있는 폴더들은 물어보지 않고 삭제한다.
    ```

    
4. Do the following commands one by one.
  touch a b c d e f g h i
  chmod = *
  chmod 714 a
  chmod u+rw b
  chmod u+xs,g+r,o= c
  chmod ug+rw,o+r d
  chmod u+rw,g+rws,o= e
  chmod ugo+x f
  chmod 755 g
  chmod 7744 h
  chmod u=,g=,o=t i
  Then, do “ls –al”
- Take a screenshot

![image-20190404025803295](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404025803295.png)

5. do the following commands.
  for t in {0..7}; do if [ $t -lt 10 ]; then mkdir 0"​$t"; else mkdir $t; fi done
  i=1; for t in {1..7}; do if [ ​$t -lt 10 ]; then t=0"$t"; fi; chmod 0"​$i"00 $t; i=​$(($i+1)); done
  ls -l
- Take a screenshot

  ![image-20190404025836587](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404025836587.png)

- Then, do the following command
  i=1; for t in {1..7}; do if [ $t -lt 10 ]; then t=0"​$t"; fi; echo "touch $t/a"; touch ​$t/a; i=$((​$i+1)); done

- Take a screenshot

![image-20190404025915015](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404025915015.png)

- List directories which contain an empty file “a”. And describe why they can generate the file.
```
03, 07 write권한과 execute권한을 가지고 있었기떼문에.
```



- Then do the following command
  i=1; for t in {1..7}; do if [ $t -lt 10 ]; then t=0"​$t"; fi; echo "ls $t"; ls -al ​$t; i=$((​$i+1)); done
- Take a screenshot
  ![image-20190404031746527](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404031746527.png)
 - List directories which print some results for “ls” command. Why?
```
00, 05, 07
폴더에 접근가능해야하고, 실행가능해야한다 
```



- Find a directory which presents the correct details of its sub files. Why?

  ```
  00, 05, 07
  폴더에 접근가능해야하고, 실행가능해야한다 
  ```

  PROBLEM !!
   How to delete the directory “03” ?

  ```
  root의 권한을 받아서 삭제해야 한다.
  또는 읽기권한을 부여한 후에 삭제해야한다.
  ```

  
6. Do “mkdir ~/homework3”, then do “cd ~/homework3”, then do “su”, then do “cp /bin/bash
  backbash”, then do “chmod u+s backbash”, then do “ls –l backbash”, then do “touch c”, then do
  “chmod 600 c”, then do “exit”, then do “cat c”, then do “id”, then do “./backbash –p”, then do “cat c”,
  then do “id”, then do “exit”. Then do “rm –f backbash c”
- Take a screenshot

![image-20190404045020509](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404045020509.png)

 - Explain the difference between the first “cat” result and the second “cat” result.

```
처음의 cat은 root 계정으로 만든 c 파일에 대한 접근 권한이 없는반면, 두번째 cat을 실행할때는 euid에 명시되어있듯이 root의 권한이 부여되어 있기에 접근 권한이 있다.
```

 - Describe the difference between the first “id” result and the second “id” result.
```
첫번째 minseok과는 달리 두번째 minseok에는 euid가 설정되어 있어, root의 권한을 가지고 있음을 볼 수 있다.
```

7. Do “su – peterpan” (user peterpan should exist), then do “clear”, then do “mkdir proj1”, then do
  “mkdir proj1/sub1”, then do “ls –l proj1”, then do “chgrp defender proj1”, then do “chmod g+s proj1”,
  then do “mkdir proj1/sub2”, then do “touch proj1/a”, then do “ls –l proj1”.
- Take a screenshot

![image-20190404050529940](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404050529940.png)

 - What is difference between the “sub1” directory and the “sub2” directory? Why?
```
sub1의 경우 setgid가 설정되어 있지않고, sub2의 경우 설정되어 있기 때문에 각 폴더의 내부에서 파일을 만들때 sub2의경우 group onwership을 폴더와 똑같이 가져간다.
```

 - What is the ownership of the file proj1/a. Why?
```
-rw-r--r--, setgid가 상위 폴더에 설정되지 않은채로 a를 만들었기 때문에.
```



8. Do “clear”, then do “mkdir shared”, then do “chmod 777 shared”, then do “mkdir shared_t”, then
  do “chmod 1777 shared_t”, then do “touch shared/a”, then do “touch shared_t/a”, then do “su hook”
  (user hook should exist), then do “rm shared/a”, then do “rm shared_t/a”, then do “exit”, then do
  “exit”.
- Take a screenshot

![image-20190404051238039](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404051238039.png)

 - Which file is removed? Why?
```
777은 읽기, 쓰기, 실행 모든 권한을 주는 것이므로 둘다 삭제가 가능하다.
```



# Problem #
1. 유져 wendy의 home directory에 secret이라는 directory를 만든다. 이 directory의 내용은
  peterpan과 wendy만이 볼수 있다. 이 directory내에 letter_to_peter 라는 파일을 만들고 그 안에
  “Hi Peter, See you on Monday :)” 라는 내용을 적어 넣는다. 이 파일은 peterpan과 wendy만 볼수
  있게 해야 하며, peterpan은 이파일의 내용을 수정하지 못한다. 하지만 peterpan은 이 directory에
  파일을 생성 할 수 있다.
  -수행해야 할 명령어들을 순서대로 나열하고 그 역할을 설명하시오.
  -결과의 확인을 위해 wendy의 home directory에서 “ls –l”을 수행한 결과와, secret directory에서
  “ls –l”을 수행한 결과를 캡쳐한다.
- List the required command in successive order, then explain the role of each command.

```
mkdir secret : 폴더 생성
chmod 771 secret : 폴더에 대해 peterpan도 안에 파일 생성할 수 있게 함.
cat > letter_to_peter : letter_to_peter파일을 생성하여 내용을 집어넣음
Hi Peter, See you on Monday :)

```

- For the purpose of verification, capture the result of “ls –l” command under wendy’s home
  directory and the results of “ls –l” command under the directory “secret”.

![image-20190404054316245](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404054316245.png)

![image-20190404054403421](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404054403421.png)

2. 정확한 접근권한과 소유권을 가지는 다음의 파일들과 디렉토리들을 생성하시오.
  1) 이 작업을 위한 명령어를 순서대로 나열하시오.

  ```
  mkdir sec .sec
  touch a .a b c d e f g h
  root@2bc6b7970cc1:/home/minseok/homework# chmod u-r a b c d e
  root@2bc6b7970cc1:/home/minseok/homework# chmod u-w c d h
  root@2bc6b7970cc1:/home/minseok/homework# chmod u-x .a b d f
  root@2bc6b7970cc1:/home/minseok/homework# chmod g-r a c d e
  root@2bc6b7970cc1:/home/minseok/homework# chmod g+w a .a e
  root@2bc6b7970cc1:/home/minseok/homework# chmod g-x a .a b c d
  root@2bc6b7970cc1:/home/minseok/homework# chmod o-r d e
  root@2bc6b7970cc1:/home/minseok/homework# chmod o+w d e
  root@2bc6b7970cc1:/home/minseok/homework# chmod o-x a .a c d g
  root@2bc6b7970cc1:/home/minseok/homework# chmod u+s d h
  root@2bc6b7970cc1:/home/minseok/homework# chmod g+s b c d
  root@2bc6b7970cc1:/home/minseok/homework# chmod o+s a c d .sec
  ```

  

  2) 확인을 위해 “ls –al” 결과를 첨부하시오.

  ![image-20190404060836388](/Users/dadadamarine/Library/Application Support/typora-user-images/image-20190404060836388.png)