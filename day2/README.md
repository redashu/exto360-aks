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

### creating container 

```
ashu@ip-172-31-60-143 ashu-python]$ docker  run  -dit  --name ashuc11   ashupython:v1 
\9f6406ee1fe931e723bb9a344699e2325fb67f92c2c8b1f378c2c7e3bb733750
[ashu@ip-172-31-60-143 ashu-python]$ docker  ps
CONTAINER ID   IMAGE              COMMAND                  CREATED              STATUS                  PORTS     NAMES
7e671f74b1ea   sivapy:v1          "/bin/sh -c 'python …"   2 seconds ago        Up Less than a second             SivA
394de566afd3   rahulpython:v1     "/bin/sh -c 'python …"   2 seconds ago        Up Less than a second             rahulc1
af45fa6fd29d   praveenpython:v1   "/bin/sh -c 'python …"   3 seconds ago        Up 1 second                       praveenC11
9f6406ee1fe9   ashupython:v1      "/bin/sh -c 'python …"   5 seconds ago        Up 4 seconds                      ashuc11
7d5deb98ed91   kovardhan:v1       "python3"                13 seconds ago       Up 11 seconds                     kovardhanPython
8f8c03afdb6f   shilpapython:v1    "/bin/sh -c 'python …"   54 seconds ago       Up 52 seconds                     shlpaC2
31d9f635628b   meghapython:v1     "/bin/sh -c 'python …"   About a minute ago   Up About a minute                 meghacontainer2
```

### checking output 

```
[ashu@ip-172-31-60-143 ashu-python]$ docker  logs  ashuc11 
Hello all , welcome to python..!!
Welcome to Exto..
Welcome to Containers ..!!
______________________
Hello all , welcome to python..!!
Welcome to Exto..
Welcome to Containers ..!!
```

