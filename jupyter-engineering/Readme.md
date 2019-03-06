

Reminder to myself to build the most current version and push it to my docker repository

```bash
docker build -t johk/toolchain:latest .
docker push
```

To use the docker image run these commands
```bash
docker create -i -t -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v $PWD:/home/jovyan/work --name pynb-kraftwerk johk/toolchain
docker start jupyter_kraftwerk
```
