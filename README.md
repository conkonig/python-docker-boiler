# Hello World

Hello world Docker image for python :).

## Usage

### Run it locally

```shell
$ python3 -m venv venv

$ ./venv/bin/pip install -qr requirements.txt

$ ./venv/bin/python3 main.py
 * Serving Flask app "server" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:8000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 196-776-054
127.0.0.1 - - [03/Apr/2019 13:07:30] "GET / HTTP/1.1" 200 -
[...]
```

### Build a Docker image

```shell
$ docker build -t hello-world .
```

### Run it in Docker

Build the image first, then launch it using `docker run`.

```shell
$ docker run --rm -it -p 8000:8000 hello-world
```

### Run it in Kubernetes

Build and push the image first, then launch it using `kubectl run`.

```shell
$ kubectl run hello-world --image=ark3/hello-world --port 8000 --expose
service/hello-world created
deployment.apps/hello-world created

$ kubectl get svc hello-world
NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
hello-world   ClusterIP   10.3.246.174   <none>        8000/TCP   1m

$ kubectl get deploy,po -l run=hello-world
NAME                                DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/hello-world   1         1         1            1           2m

NAME                               READY   STATUS    RESTARTS   AGE
pod/hello-world-776fc969b9-8m457   1/1     Running   0          2m

$ kubectl run curl-from-cluster -it --rm --image=fedora --restart=Never -- curl hello-world:8000
Hello, world!
pod "curl-from-cluster" deleted

$ kubectl logs hello-world-776fc969b9-8m457
 * Serving Flask app "server" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:8000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 305-616-111
10.0.235.232 - - [03/Apr/2019 17:15:29] "GET / HTTP/1.1" 200 -
```
