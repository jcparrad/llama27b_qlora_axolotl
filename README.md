# This project trains llama2 1b, 13b or 70b models using qlora and axolotl framework

# run docker

docker run --gpus '"all"' -p 8888:8888 -v /home/jupyter:/workspace --rm -it winglian/axolotl:main-py3.10-cu118-2.0.1

# Inside run jupyter
pip install jupyter
jupyter notebook --ip=0.0.0.0 --port=8888 --allow-root


# preprocess dataset
follow the steps in notebook data_preprocessing.ipynb

check next link to learn how to download datasets:
https://colab.research.google.com/github/huggingface/notebooks/blob/master/course/videos/save_load_dataset.ipynb#scrollTo=oOMKAvjUMNOo


# run training 

accelerate launch -m axolotl.cli.train /workspace/llama27b_qlora_axolotl/llama2_7b_quotes_qlora.yaml



# merge with lora
python3 -m axolotl.cli.merge_lora /workspace/llama27b_qlora_axolotl/llama2_7b_quotes_qlora.yaml --lora_model_dir="./qlora-out" --load_in_8bit=False --load_in_4bit=False


# inference
docker run --gpus all -p 8888:8888 -p 8000:8000 -v /home/jupyter:/workspace -it --rm --ipc=host nvcr.io/nvidia/pytorch:23.10-py3
pip install vllm


 python -m vllm.entrypoints.api_server --model="/workspace/llama27b_qlora_axolotl/qlora-out/merged"