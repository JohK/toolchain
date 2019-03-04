docker build -t toolchain:latest .
docker create -i -t -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v /Users/kneer/src/toolchain/jupyter-engineering:/home/jovyan/work --name pynb-kraftwerk johk/toolchain
docker start jupyter_kraftwerk
