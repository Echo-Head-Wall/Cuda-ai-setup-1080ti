# errors i used this to fix
ERROR: modpost: gpu-incompatible module nvidia.ko uses gpl-only symbol rcu_read_unlock_strict

in the above case essentially you forgot to restart your computer before trying to install nvidia driver after a upgrade  leaving it in a half broken state

Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running prestart hook #0: exit status 1, stdout: , stderr: Auto-detected mode as 'legacy'
nvidia-container-cli: initialization error: nvml error: driver not loaded: unknown

Running incorrect drivers ( max version is 525 ) for your card

# Cuda-ai-setup-1080ti
working cuda ai install for pascal ( 1080ti tested ) for running ai models like deepseek

# Warnings 
if you have a UI installed on your machine this will probably fuck with it it may be fine but this is reccomend for use on a dedicated machine for ai and nothing else running ubuntu 22.04 ( sadly they dont make the old driver versions for ubuntu 24 noble )
# Warning 2
i wrote all this instructions after spending 5 days figuring out the perfect set up just to run my old cards with very little sleep so use at your own risk. It is nice to see its possible to still get these old cards running stuff. i hope my suffering helps other people 

# pre reqs
Install docker via standard methods
Ubuntu 22.04
# Instructions 
```
   sudo apt purge *nvidia*
   sudo apt purge *cuda*
   sudo apt autoremove
   sudo shutdown -r now 
   sudo add-apt-repository ppa:graphics-drivers/ppa
   sudo apt update
   curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
    curl -s -L https://nvidia.github.io/libnvidia-container/ubuntu22.04/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    sudo apt update
    sudo apt install nvidia-driver-525 nvidia-dkms-525
    sudo apt install nvidia-container-toolkit
    sudo nvidia-ctk runtime configure --runtime=docker
    sudo systemctl restart docker
```
   # To Test use
   ```
    docker run -ti --name local-ai -p 8080:8080 --gpus all localai/localai:latest-gpu-nvidia-cuda-11
```
