Homework #5
1. Start a new terminal, do “mkdir fs_test”, then do “cd fs_test”, then do “su” (password required),
  then do “fdisk /dev/sda”, then press “p”, then press ”q”, then do “exit”
- Take a screenshot

  ![image-20190509012549089](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509012549089.png)

  **Docker를 사용하고있어 vmWare와는 환경이 다른것으로 예상됩니다. 웹에 검색해 사진을 첨부하도록 하겠습니다.**
  ![image-20190509012714489](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509012714489.png)

- What is the specification of the disk “sda”? (heads, sectors, cylinders, sector bytes)
  **225 heads, 63 sectors, 8225280 bytes cylinders , 320072933376 bytes**

- How many partitions are there?

  **파티션이 2개 존재합니다.**
2. Do “clear”, then do “mkdir lntest”, then do “echo “test ln” | cat > testln”, then do “ln –s lntest
  lntest_s”, then do “ln –s testln testln_s”, then do “ls –l”
- Take a screenshot

  ![image-20190509013036571](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509013036571.png)

- What are the files named “lntest_s” and “testln_s”?

  **lntest, testln**
3. Do “clear”, then do “cat testln”, then do “cat testln_s”, then do “echo “addition” >> testln”, then
  do “echo “addition2” >> testln_s”, then do “cat testln”
- Take a screenshot
  ![image-20190509013231889](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509013231889.png)
- What is the last result? Why?
  **링크가 되있기때문에** 
4. Do “clear”, then do “cd lntest_s”, then do “touch b”, then do “pwd”, then do “cd ..”, then do “cd
  lntest”, then do “ls”, then do “pwd”
- Take a screenshot
  ![image-20190509013344364](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509013344364.png)

- Compare the results between first “pwd” and second “pwd”.
  **보이는 경로가 각각 lntest_s, lntest로 다르다**

- What is the result of “ls”? Why?

  **결과 : b 이다. 폴더가 링크되어있을경우, 같은 폴더를 가리키므로 lntest_s폴더에서 파일을 생성하면 lntest에서도 결국 그 폴더를 가르키기 때문에, 파일이 생성되어있다.**
5. Do “cd ..”, then do “clear”, then do “ln testln testln_h”, then do “echo “addition3” >> testln_h”,
  then do “cat testln_s”, then do “ls –l”
- Take a screenshot
  ![image-20190509013641921](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509013641921.png)

- What is the file “testln_h”?
  **testln에 하드링크가 걸린 파일, 같은 파일을 가리킨다.**

- What is the difference between “testln_s” and “testln_h”

  **파일의 크기가 차이가 나고, soft링크는 inode가 inode를 가리키는 구조이지만, 하드링크는 두 inode가 모두 파일을 가리킨다.**
6. Do “clear”, then do “mv testln testln_moved”, then do “cat testln_h”, then do “cat testln_s”, then
  do “ls –l”, then do “ls –i"
- Take a screenshot
  ![image-20190509013942800](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509013942800.png)

- Compare the results between the first “cat” and the second “cat”
  **하드링크로 된 파일은 출력되지만 soft링크는 그렇지 못하다**

- What is the difference between “testln_s” and “testln_h” from the results of “ls –l” and “ls –i"?

  ls -l에서는 용량, 권한등이 다르다 하지만 ls -i에서는 inode의 번호가 하드링크는 같고, 소프트링크는 다르다.
7. Do “clear”, then do “dd if=/dev/zero of=disk.img bs=1024 count=1400”, then do “mke2fs –F
  disk.img”, then do “mkdir disk”, then do “ls –l disk”, then do “su” (password required), then do
  “mount –o loop disk.img disk”, then do “exit”, then do “ls –l disk”
- Take a screenshot
  ![image-20190509014736038](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509014736038.png)

- What is the functionality of “dd” command?
  **device driver파일을 접근할 수 있게해줌**

- What is the functionality of “mke2fs” command?

  **파일 시스템 생성**

- Compare the difference between the results of the first “ls” and the second “ls”. Why?
  **에러가나서 제대로 동작하지 않았습니다.**
8. Do “clear”, then do “df”, then do “df –h”, then “df –i”, then do “df .”
- Take a screenshot
  ![image-20190509015126558](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509015126558.png)
- What are the options “-h” and “–i" for?
  **-h는 사람이 볼수있게 형태를 보여주고 , -i 는 inode에 대한 정보를 보여줍니다.**
9. Do “su”(password required), then do “umount disk”, then do “apt-get install quota” (press Y for
  all questions), then do “mount –o loop,rw,usrquota,grpquota disk.img disk”, then do “mkdir
  disk/shared”, then do “chmod 777 disk/shared”, then do “chgrp neverland disk/shared”, then do
  “chmod g+s,o+t disk/shared”, do “quotacheck –cug disk”, then do “ls –l disk”. (Assumption : There
  are users named as peterpan and hook. Both users are members of a group named as neverland.)
- Take a screenshot
  ![image-20190509020107333](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509020107333.png)

- What are the properties of the new directory shared?
  **모든 사용자에 대해 읽고쓰는 권한을 부여합니다. 소유권은 root유저와 neverland그룹, 크기는 1024입니다.**

- What is the functionality of “quotacheck”?

  **할당량 규정에대한 사용자와 그룹을 확인하는 기능을 합니다.**
10. Do “clear”, then do “setquota –u peterpan 2 4 2 4 disk”, then do “setquota –u hook 2 4 2 4 disk”,
  then do “setquota –g neverland 4 6 4 6 disk”, then do “repquota –v disk”, then do “repquota –vg
  disk”
- Take a screenshots for repquota commands
  ![image-20190509022541279](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509022541279.png)

- What is the functionality of “setquota”? Explain the options “-u” and “-g” and arguments.
  Setquota 
  할당량을 제한하는 명령어, u 와 g는 각각 유저와 그룹을 설정할때 사용된다.

  ```
  -u, --user                 set limits for user
  -g, --group                set limits for group
  ```

  
11. Do “clear”, then do “quotaon disk”, then do “su peterpan”, then do “touch disk/shared/a”, then
   do “touch disk/shared/b”, then do “touch disk/shared/c”, then do “quota peterpan”, then do “touch
   disk/shared/d”, then do “touch disk/shared/e”, then do “quota –g neverland”, then do “exit”
- Take a screenshot
  ![image-20190509024519615](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509024519615.png)
- What is the difference between the soft limit and hard limit?
  soft limit 경고를 보내주는 limit이고, hard limit은 제한 limit입니다.

  
12. Do “clear”, then do “su hook”, then do “touch disk/shared/h1”, then do “touch disk/shared/h2”,
   then do “quota hook”, then do “quota –g neverland”, then do “exit”, then do “quotaoff disk”, then
   do “du –h disk”
- Take a screenshot
  ![image-20190509025134693](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509025134693.png)

- What happens during creating h1 and h2? Why?
  퍼미션 에러가 났습니다.

- What is the command “du” for?
  directory에서 용량을 얼마나 사용했는지 보여줍니다.

  

  == Problems ==
1. Make a local file named as “backup_disk.img” and initialize an “ext4” filesystem in the file whose
  capacity is about 100MB with 1024 Byte block size. The filesystem in the “backup_disk.img” file is
  mounted on the local directory “backup_disk”.
- List the required commands

  ```
  dd if=/dev/zero of=backup_disk.img bs=1024 count=100000
  mkdir backup_disk
  mke2fs -F backup_disk.img
  mount -o loop backup_disk.img backup_disk
  ```

  

- Evaluate your operation by using “ls –hl backup_disk.img”, “df –T backup_disk”, and “ls –hl
  backup_disk”
  ![image-20190509034046116](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190509034046116.png)

- Describe how the ownership and size of backup_disk directory change.
  **0으로 변함이 없습니다.**
2. What are the default mount options in a linux system? What are the meanings of these options?
   옵션 : 
   -a: /etc/fstab에 있는 파일 시스템을 모두 마운트합니다.
   -t: 파일시스템을 지정합니다.
   -o: 다른 옵션을 사용합니다.

   ro: 읽기 전용
   remount: 다시 마운트
   loop: iso파일 마운트
   noatime: 파일 읽기 전까진 열람만으론 access time을 변경하지 않음
   username=계정 password=암호: 마운트 시 계정인증이 필요할 때 사용
   acl: Access Control Lists를 마운트할 때 사용
   async: 파일시스템에 대한 입출력 비동기적
   sync: 파일시스템에 대한 입출력 동기적

3. What is the objective of “sync” option in “mount” command? When do we need to take care of
  using “sync” option?
  **파일 입출력에 동기화를 보장합니다.** 

4. There is a local file named as “vfat_disk.img” and initialize an “vfat” filesystem in the file whose
  capacity is about 100MB with 1024 Byte block size. The filesystem in the “vfat_disk.img” file is
  mounted on the local directory “backup_disk”. In here, a normal user whose UID is 1000 and GID is
  1000 wants to create a file into this mounted disk. What are the required commands? (Hint : mkfs.vat, mount options for vfat: uid, gid, umask)

5. Let’s assume that a file “a” locates under “backup_disk”, a file “b” locates under “backup_disk_2”
  and a file “c” locates in the local directory. Evaluate whether the following operations correctly works
  or not. (Assumption: the current directory is the parent directory of “backup_disk” and
  “backup_disk_2”. The following commands are executed on the current directory.)

  1) ln c c_h 
  **정상 작동합니다.**

  2) ln –s c c_s

  **정상 작동합니다.**

  3) ln backup_disk/a a_h

  **부적절한 장치간 연결 에러**
  4) ln –s backup_disk/a a_s

  **정상 작동합니다.**
  5) ln backup_disk/a backup_disk/a_h

  **file already exist 에러가 납니다.**

  6) ln –s backup_disk/a backup_disk/a_s

  **root권한을 얻으면 정상 작동 그렇지 않으년 허가거부 에러가 발생합니다.**
  7) ln backup_disk_2/b backup_disk/b_h

  **부적절한 장치간 연결 에러가 발생합니다.**
  8) ln backup_disk_2/b backup_disk_2/b_h

  **정상 작동합니다.**
  9) ln –s backup_disk_2/b backup_disk/b_s

  **root권한을 얻으면 정상 작동 그렇지 않으년 허가거부 에러 발생합니다.**