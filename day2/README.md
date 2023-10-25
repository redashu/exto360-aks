# exto360-aks

### app containerization and deployment process workflow 

<img src="a.png">

### sample python script containeriztion 

### ashu.py 

```
import time

while True:
    print("Hello all , welcome to python..!!")
    time.sleep(3)
    print("Welcome to Exto..")
    time.sleep(2)
    print("Welcome to Containers ..!!")
    print("______________________")
    time.sleep(3)
```

### Dockerfile

```
FROM python
# we are refering official python image from Docker hub 
RUN mkdir  /mycode 
COPY ashu.py  /mycode/
CMD python  /mycode/ashu.py 
# whosoever gonna use my image so this will automatically run 
# python code 

```

### building image 

```
[ashu@ip-172-31-60-143 ashu-apps]$ cd  ashu-python/
[ashu@ip-172-31-60-143 ashu-python]$ 
[ashu@ip-172-31-60-143 ashu-python]$ docker  build  -t  ashupython:v1  . 
Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM python
 ---> 17e65561fd2c
Step 2/4 : RUN mkdir  /mycode
 ---> Running in 67e54c0646aa
Removing intermediate container 67e54c0646aa
 ---> 7e0e407ed7b1
Step 3/4 : COPY ashu.py  /mycode/
 ---> 588dca29c1e9
Step 4/4 : CMD python  /mycode/ashu.py
 ---> Running in 9a32f710b7b5
Removing intermediate container 9a32f710b7b5
 ---> c4bd1076b35d
Successfully built c4bd1076b35d
Successfully tagged ashupython:v1
```
