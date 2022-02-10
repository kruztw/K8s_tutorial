# prepare docker images

## echo server

### Why ?

This project originates from my newbie homeworks of my job.

The first one is create a multi-thread echo server, and the second one is deploy echo server on K8s.

You don't have to learn very detail about echo server, all you need to know is how to run.

## How ?

```shell=
git clone https://github.com/kruztw/echo_server

cd echo_server
make
./server 1234

# open another terminal
nc localhost 1234

make clean
```

## build & push to docker hub

```
docker build -t <your_account>/echo_server:latest .
docker login
docker push <your_account>/echo_server:latest .
```



