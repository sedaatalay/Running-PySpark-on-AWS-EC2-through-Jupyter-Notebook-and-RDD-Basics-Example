# RDD Examples with Jupter Notebook on AWS EC2 Server
<p> <br/ >
 
### 1- Login to AWS and launch an instance
<img width="1234" alt="Ekran Resmi 2022-02-27 20 40 21" src="https://user-images.githubusercontent.com/91700155/155900685-8e6362b9-c33b-46e3-aa73-eb1f055f87f8.png">

### 2- Select Ubuntu Server 18.04 LTS
<img width="1407" alt="Ekran Resmi 2022-02-27 20 41 24" src="https://user-images.githubusercontent.com/91700155/155900701-9728d47c-8905-41f1-b72c-108ed8c4f33b.png">

### 3- Choose an Instance Type (Choose recommended)
<img width="1429" alt="Ekran Resmi 2022-02-27 20 41 58" src="https://user-images.githubusercontent.com/91700155/155900718-151b29a2-e149-4c28-8b02-c5857aebf1b6.png">

### 4- The instance Auto-assing Public IP has to be enable
<img width="1431" alt="Ekran Resmi 2022-02-27 20 42 23" src="https://user-images.githubusercontent.com/91700155/155900732-48b5faad-8d83-4319-974a-fa72fcb4637d.png">

### 5- For larger size installations we can add additional volume
<img width="1428" alt="Ekran Resmi 2022-02-27 20 42 58" src="https://user-images.githubusercontent.com/91700155/155900738-9e03df14-ff0d-4dd5-837d-dc1d18a480cc.png">

### 6- Make sure the security group type has all traffic allowed to inbound and outbound that you need
<img width="1164" alt="Ekran Resmi 2022-03-07 12 01 24" src="https://user-images.githubusercontent.com/91700155/157004601-694e2d72-cfb1-4956-a2b0-654b7509f642.png">

### 7- Create a Private Key file (.pem)
<img width="1421" alt="Ekran Resmi 2022-02-27 20 46 18" src="https://user-images.githubusercontent.com/91700155/155900755-7e00a8c4-3c11-4e8d-a272-9360948cf862.png">

### 8- Launch the Instance
<img width="1423" alt="Ekran Resmi 2022-02-27 20 46 38" src="https://user-images.githubusercontent.com/91700155/155900765-25266061-7b5b-40cf-90d9-c3711367a78b.png">

### 9- Change the permission of Private Key
```console
chmod 400 <pem file name>.pem
```
### 10- Connect using SSH
```console
ssh -i <pem file name> ubuntu@public-dns-name
```
<img width="947" alt="Ekran Resmi 2022-03-07 12 28 58" src="https://user-images.githubusercontent.com/91700155/157004677-96f667a7-f4a3-42ab-9561-1b65318c926e.png">

### 11- Installing Jupyter Notebook
#### Download Anaconda
```console
wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
```
<img width="927" alt="Ekran Resmi 2022-03-07 12 06 33" src="https://user-images.githubusercontent.com/91700155/157005185-13cd87a2-b246-4b6e-915c-6f08e5b78a8d.png">

#### Run bash on the file 
```console
bash Anaconda3-2019.03-Linux-x86_64.sh
```
<img width="927" alt="Ekran Resmi 2022-03-07 12 06 50" src="https://user-images.githubusercontent.com/91700155/157005760-9b910999-972b-4b67-8327-be92959655f5.png">

##### -To all "yes"
<img width="914" alt="G5BbD" src="https://user-images.githubusercontent.com/91700155/157006451-985845d2-240e-4fcf-9b34-2d9a86944509.png">
<img width="948" alt="Ekran Resmi 2022-03-07 12 14 14" src="https://user-images.githubusercontent.com/91700155/157012088-70800d9f-7541-4308-82ea-8edf559b1462.png">

##### -If you miss the second "yes" write to:
```console
source ~/anaconda3/bin/activate
```
#### Configuring Jupyter Notebook’s Path
```console
nano .bashrc
```
##### -Write into:
```console
export PATH=/home/ubuntu/anaconda3/bin:$PATH
```
<img width="945" alt="Ekran Resmi 2022-03-07 12 14 43" src="https://user-images.githubusercontent.com/91700155/157012117-9ae6e4be-e4f0-40c5-a733-998396075738.png">

#### After saving path, you need to run
```console
source .bashrc
```
<img width="943" alt="Ekran Resmi 2022-03-07 12 15 04" src="https://user-images.githubusercontent.com/91700155/157006789-509c8cbe-96e1-43aa-b545-68cb551f5a80.png">

#### Configuring Jupyter Notebook settings
```console
jupyter notebook --generate-config
ipython
from IPython.lib import passwd
passwd()
```
##### -Give a password 
<img width="480" alt="Ekran Resmi 2022-03-07 12 46 54" src="https://user-images.githubusercontent.com/91700155/157007622-8e6386d6-c436-4310-811c-e3ea4b1a067f.png">

##### -Copy the "out[2]:" line 
<img width="946" alt="Ekran Resmi 2022-03-07 12 16 45" src="https://user-images.githubusercontent.com/91700155/157012189-b3632d73-3925-4dbf-badc-2920b69fd74f.png">

#### Jupyter Config File
```console
cd .jupyter
vim jupyter_notebook_config.py_
```
<img width="928" alt="Ekran Resmi 2022-03-07 12 17 11" src="https://user-images.githubusercontent.com/91700155/157008315-0f848675-73c6-4f9b-b21c-4548e993b3c7.png">

##### Write into:
```console
conf = get_config()

conf.NotebookApp.ip = '0.0.0.0'
conf.NotebookApp.password = u'<Paste the out[2] line>'
conf.NotebookApp.port = 8888
conf.NotebookApp.allow_origin = '*'
```
<img width="946" alt="Ekran Resmi 2022-03-07 12 18 15" src="https://user-images.githubusercontent.com/91700155/157008480-f1fdf0c2-f4da-45aa-9243-2b96cd99ea8c.png">

#### Create a directory for your notebooks and ufw allow the port
```console
mkdir MyNotebooks
sudo ufw allow 8888
```
<img width="942" alt="Ekran Resmi 2022-03-07 12 19 08" src="https://user-images.githubusercontent.com/91700155/157008815-fee05b79-d026-43d6-b9fe-09902753fe91.png">

#### Connecting to your EC2 Jupyter Server
```console
jupyter notebook --ip 0.0.0.0 --port 8888
```
<img width="946" alt="Ekran Resmi 2022-03-07 12 19 48" src="https://user-images.githubusercontent.com/91700155/157008908-8ed8415d-a9a3-4cde-920c-7c1b4f6a14b8.png">

##### -Copy the given token
<img width="921" alt="Z8G5B" src="https://user-images.githubusercontent.com/91700155/157012433-24514ed4-2328-4680-bda8-833acfecc183.png">

#### Access your server with the following URL
```console
https://<your public dns>:8888/
```
<img width="1407" alt="Ekran Resmi 2022-03-07 12 20 07" src="https://user-images.githubusercontent.com/91700155/157009243-2b3cab16-fb6c-446a-8da2-f6748935602f.png">

##### -Paste the copied token and create a new password if you want
<img width="686" alt="Ekran Resmi 2022-03-07 12 22 09" src="https://user-images.githubusercontent.com/91700155/157012388-7ae3b1e5-8a1b-4b7e-96dd-1b2ce5935bc1.png">

#### Jupyter Notebook Web UI
<img width="1411" alt="Ekran Resmi 2022-03-07 12 26 53" src="https://user-images.githubusercontent.com/91700155/157010483-a6ce616c-90f2-4786-ad66-4753fc091732.png">

#### To stop jupyter notebook services
```console
jupyter notebook stop 8888
```


<p>  <br />
</p>

### Seda Atalay
