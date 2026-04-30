\# Docker Assignment



\## Task 1.1 — Change the Python version



```powershell

PS C:\\Users\\yucel\\Desktop\\hello-docker\\hello-docker> docker build -t hello-docker .

\[+] Building 18.4s (8/8) FINISHED

&#x20;=> \[internal] load build definition from Dockerfile

&#x20;=> => transferring dockerfile: 118B

&#x20;=> \[internal] load .dockerignore

&#x20;=> => transferring context: 2B

&#x20;=> \[internal] load metadata for docker.io/library/python:3.9-slim

&#x20;=> \[1/3] FROM docker.io/library/python:3.9-slim

&#x20;=> \[internal] load build context

&#x20;=> => transferring context: 148B

&#x20;=> \[2/3] WORKDIR /app

&#x20;=> \[3/3] COPY hello.py .

&#x20;=> exporting to image

&#x20;=> => exporting layers

&#x20;=> => writing image

&#x20;=> => naming to docker.io/library/hello-docker



PS C:\\Users\\yucel\\Desktop\\hello-docker\\hello-docker> docker run --rm hello-docker

Hello from Python 3.9 inside a container!

```



\## Task 1.2 — Failed run with pandas



```powershell

PS C:\\Users\\yucel\\Desktop\\hello-docker\\hello-docker> docker build -t hello-docker .

\[+] Building 0.9s (8/8) FINISHED

&#x20;=> \[internal] load build definition from Dockerfile

&#x20;=> \[internal] load .dockerignore

&#x20;=> \[internal] load metadata for docker.io/library/python:3.9-slim

&#x20;=> \[1/3] FROM docker.io/library/python:3.9-slim

&#x20;=> \[internal] load build context

&#x20;=> CACHED \[2/3] WORKDIR /app

&#x20;=> \[3/3] COPY hello.py .

&#x20;=> exporting to image

&#x20;=> => naming to docker.io/library/hello-docker



PS C:\\Users\\yucel\\Desktop\\hello-docker\\hello-docker> docker run --rm hello-docker

Traceback (most recent call last):

&#x20; File "/app/hello.py", line 1, in <module>

&#x20;   import sys, pandas

ModuleNotFoundError: No module named 'pandas'

```



\## Task 1.2 — Successful run after fixing Dockerfile



```powershell

PS C:\\Users\\yucel\\Desktop\\hello-docker\\hello-docker> docker build -t hello-docker .

\[+] Building 26.6s (9/9) FINISHED

&#x20;=> \[internal] load build definition from Dockerfile

&#x20;=> \[internal] load .dockerignore

&#x20;=> \[internal] load metadata for docker.io/library/python:3.9-slim

&#x20;=> \[1/4] FROM docker.io/library/python:3.9-slim

&#x20;=> \[internal] load build context

&#x20;=> CACHED \[2/4] WORKDIR /app

&#x20;=> \[3/4] RUN pip install pandas==2.2.3

&#x20;=> \[4/4] COPY hello.py .

&#x20;=> exporting to image

&#x20;=> => writing image

&#x20;=> => naming to docker.io/library/hello-docker



PS C:\\Users\\yucel\\Desktop\\hello-docker\\hello-docker> docker run --rm hello-docker

Python 3.9, pandas 2.2.3

```



\## Final Dockerfile



```Dockerfile

FROM python:3.9-slim

WORKDIR /app

RUN pip install pandas==2.2.3

COPY hello.py .

CMD \["python", "hello.py"]

```



\## Question 2.1 — Why pin?



Pinning a package version means that the same version will be installed every time the Docker image is rebuilt. If I only write RUN pip install pandas, Docker will install the newest available version of pandas at that time. This may work today, but in the future pandas could change, remove features, or behave differently. Because of that, the same code might give different results or even stop working when another person rebuilds the image later. Pinning pandas==2.2.3 makes the environment more stable and reproducible.



\## Question 2.2 — Recipe or cake?



For reproducible research, sending the Dockerfile and hello.py is better than only sending the built image. The Dockerfile shows exactly how the environment was created, so the process is transparent and can be checked by another person. A built image may run correctly, but it hides the recipe and makes it harder to understand what was installed. Sharing the Dockerfile also allows the environment setup to be tracked in Git together with the code.

