docker run --gpus '"all"' -p 8888:8888 -v /path/on/host:/path/in/container --rm -it winglian/axolotl:main-py3.10-cu118-2.0.1

Inside run jupyter
notebook --ip=0.0.0.0 --port=8888 --allow-root

