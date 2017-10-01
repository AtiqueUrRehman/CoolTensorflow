## Configuring Tensorflow with GPU
### Steps
- Install GUI and RDP server
- Install Nvidia drivers
- Install CUDA
- Install CUDNN
- Install Anaconda
- Install Tensorflow (Gpu version)

1. Follow the steps here to install CUI and RDP server : http://c-nergy.be/blog/?p=8952

2. Update the system and install following 
```
wget -O /tmp/NVIDIA-Linux-x86_64-367.48.run https://go.microsoft.com/fwlink/?linkid=836899
sudo apt-get update
sudo apt install gcc
sudo apt install make
chmod +x /tmp/NVIDIA-Linux-x86_64-367.48.run
sudo sh  /tmp/NVIDIA-Linux-x86_64-367.48.run
```

2. Download cuda from https://developer.nvidia.com/cuda-downloads and follow the steps to install 
```
sudo dpkg -i cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

3. Download cudnn 5.1 from nvidia website and follow these steps
```
tar -xvzf cudnn-8.0-linux-x64-v5.1.tgz
sudo cp -r cuda/include* /usr/local/cuda/include*
sudo cp -r cuda/lib64* /usr/local/cuda/lib64*
```

4. Open bashrc using the following command and paste the following  lines
```
sudo vim ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
export CUDA_HOME=/usr/local/cuda
export PATH=/usr/local/cuda/bin:$PATH
source .bashrc
```
5. Install Nvidia Cuda-tollkit
```
sudo apt-get install nvidia-cuda-toolkit
```
6. Install Anaconda. First download the sh file and the run the following command
```
bash Anaconda2-4.0.0-Linux-x86_64.sh
```
7. Install latest Tensorflow for GPU
```
pip install tensorflow-gpu
```

### Configure jupyter for port forwarding
8.	Generate jupyter config file
```
jupyter-notebook --generate-config
```
9. Go to config directory
```
cd ~/.jupyter
```
10. Generate openssl key
```
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
```

11. Copy the ssh key printed by this command	
```
python -c "import IPython;print(IPython.lib.passwd())"
```

12. Open the config file present in current directory and past the following lines, replace your ssh key
``` python
c = get_config()

# You must give the path to the certificate file.
c.NotebookApp.certfile = u'/home/sibt/.jupyter/mycert.pem'

# Create your own password as indicated above
c.NotebookApp.password = u'sha1:3b669cfe9948:0a9c5156287bebca97565037ac075203163074ad'

# Network and browser details. We use a fixed port (9999) so it matches
# our Azure setup, where we've allowed traffic on that port
c.NotebookApp.ip = '*'
c.NotebookApp.port = 9999
c.NotebookApp.open_browser = False
```
13. Run jupyter notebook
```
jupyter-notebook
```
14. Test in your browser
```
https://<your ip>:9999
```
