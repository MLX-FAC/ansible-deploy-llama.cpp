# Deploy Lora Model

<img src="https://cdn.mlx.institute/assets/ansible-deploy-llama.cpp.avif" >

This workflow is supposed to take your model, convert it a form that's understandable by [llama.cpp](https://github.com/ggerganov/llama.cpp), which is a project that's aimed to do LLM inference on CPUs, and then tranfer that file over to your remote machine where your docker container will be downloaded and launched according to the docker-compose in the github repo.

When writing your Server code, you'll use the [`llama-cpp-python`](https://github.com/abetlen/llama-cpp-python) package, that will let your load it in, and do inference like you would do in pytorch.

Ex.

```python
#
#
#
#
import os
import time
import fastapi
import uvicorn
from llama_cpp import Llama

#
#
#
#
print("Loading Model...")
llm = Llama("/model/model.gguf", verbose=False)


#
#
#
#
print("Model Loaded. Starting server...")
app = fastapi.FastAPI()


#
#
#
#
@app.post("/")
def inference(input_text: str):
  start_time = time.time()
  output = llm( input_text, max_tokens=32 )
  return {
    "output": f"{input_text}{output['choices'][0]['text']}",
    "time_elapsed": time.time() - start_time
    }


#
#
#
#
if __name__ == "__main__":
  uvicorn.run(app, host="0.0.0.0", port=8000)
```


## Step 0 - Prerequisites
Create a conda environment with python 3.11
```bash
conda create -n deploy python=3.11
conda deactivate
conda activate deploy
```

## Step 1 - Setup the ansible variables

### ansible.cfg
```ini
[defaults]
inventory = inventory
host_key_checking = False
remote_user = "<CHANGE_ME>"
```

### Inventory
```ini
[all]
remote-server  ansible_host="<CHANGE_ME>" ansible_port="<CHANGE_ME>"  ansible_ssh_private_key_file="<CHANGE_ME>"
```

## Step 2 - Run the convert script
The convert script will take 2 flags.
1. `--token`     - The token for huggingface used for downloading gemma-2b
2. `--lora-repo` - The huggingface repo to download your lora/peft adapter from
```bash
python3 convert.py --token [TOKEN] --lora-repo [USERNAME/LORA]
```
