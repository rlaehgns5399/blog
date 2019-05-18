

리눅스







filesystem : 스토리지에서 파일을 읽고 쓰는것.

![image-20190502135033001](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190502135033001.png)

여러파일시스템을 abstraction layer에서 한번에접근할수 있게해줌.





파일을 읽기위해선 인덱스가 필요. = inodes

• It contains information about the file, but not contents

cat 으로 파일생성할경우 inode가 생성되는것, 그래서 컨텐츠가 없음





![image-20190502135707210](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190502135707210.png)

Inode Table의 block수만큼 파일을 생성할 수 있다.

하나의 block에는 Inode(파일의 정보)로 표현됨. 

블락의 크기 = Inode의 크기에 딱맞음

![image-20190502140047203](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190502140047203.png)

inode는 파일name을 가지고 있지 않다. filename은 directory에 있음.

![image-20190502140400893](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190502140400893.png)

디렉토리 엔트리에는 filename과 inode 넘버가 있음.



우리가 cs로 접근할 경우. Cd /home/peter

쉘을 체크하고, 쉘을 연다음 맨처음 root디렉토리로 감, root에는 

/ 를 까서 디렉토리 엔트리를 본다음 다음에 어디로 가야할지를 봄

Inode20번이다 이러면 20번으로 가서 -> dataentry를 보고 Directory 엔트리로 다시 감.



> . 와 ..는 부모에 맞는 inode값이 주어짐. root는 0



![image-20190502141006663](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190502141006663.png)

위와 같이 계속 root부터 접근하는건 낭비니까 이렇게 캐시를 깔아둠.



inode 는 owner ship을 가지고, file structured은 접근 권한을 관리한다.

inode에서 말하는 permissions는 ownership임.





# Symbolic Link

위의 내용은 이걸 설명하기 위함이었음.

명령어 링크에 씀.





## HardLinks

share inode number



![image-20190502141811539](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190502141811539.png)

2라고 나옴. 숫자는 링크 카운트, 

![image-20190502142232474](/Users/dadadamarine/Desktop/study/blog/dadadamarine.github.io/_posts/assets/images/image-20190502142232474.png)



Sysmbolic link는 새 directory entry가 다른 디렉토리 엔트리를 가르킴.

이름과 이름의 관계, 포인터임, 다른 inode가 다른 directory entry를 의미

이름을 기준으로 감.



하드링크는 디렉토리 엔트리가 inode 자체를 가르김. inode를 가르키는 Directory entry (링크 카운트가 의미하는거 : inode를 가르키는 directory entry의 수)

이름과 시스템의 관계



디렉토리볼때 execute 필요한이유.

inode로 가서 터널에서 실해 봐야함.





disk size뿐아니라, inode가 다 차버려도 파일을 못만듬.



포멧시 : inode 테이블만 지움, 파일을 다 살아있음. 그래서 빠름

데이터 셋까지 지우려면 low level 포맷을 해야함.

아니면 복원가능