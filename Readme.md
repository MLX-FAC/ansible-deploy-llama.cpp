# Deploy Lora Model

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
`--token`     - The token for huggingface used for downloading gemma-2b
`--lora-repo` - The huggingface repo to download your lora/peft adapter from
```bash
python3 convert.py --token [TOKEN] --lora-repo [USERNAME/LORA]
```