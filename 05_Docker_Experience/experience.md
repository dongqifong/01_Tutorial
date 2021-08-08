# Docker 用途介紹

## 觀念

1. 建立映像檔(image)來做出特定的開發環境
2. image上可以有不同互不影響的容器(containers)
3. 每個container都是一個process

## 目的

1. 做出一個隔離個開發環境
2. 方便交付給他人做開發與測試

## 常用的命令

1. 打包images
2. run images
3. 將資料從container拷貝出來

## 打包的流程

1. 在dockerhub找一個基礎的image做基底
2. 將想要複製的資料夾結構寫進Dockerfile裡面
3. 如果有自己想要額外安裝的套件寫在requirements.txt裡面
4. 用docker指令build自己的image
5. 例如你想要在python:3這個image做為基底，建立一個工作目錄，叫做my_workspace，且將所有資料夾的資料都從本機帶入image內，且同時要額外安裝某些套件(寫在requirements.txt內)，那你可以在Dockerfile中這樣書寫:
   ```
   FROM python:3

   WORKDIR /usr/src/my_workspace

   COPY ./ ./

   RUN pip install --no-cache-dir -r requirements.txt

   EXPOSE 8888 
   ```
6. requirements.txt的位置要跟Dockerfile同一層
   
   資料夾結構

   <img src="/images/folder_structure.PNG" width="50%">

   requirements.txt內容

   <img src="/images/requirements.PNG" width="30%">

7. 以docker build指令建立自己的image
   > docker build -t <image_name:tag> .

   > ex: docker build -t my_first_image:0.0.1 .
   
8. 這樣之後每個基於此image所建立的container，都會有my_workspace這個開發資料夾，且所有套件的版本都會相同。


## Run images的流程

1. 找到你想run的image id(可能是在dockerhub pull下來的或自己build的)
2. 以docker指令run，如果是要建立一個jupyterlab的開發環境就要記得產生container連外的街口(port)
   > docker run -it -p 82:8888 6de62981d855 /bin/bash -c "jupyter lab --ip='*' --port=8888 --no-browser --allow-root"
   
   意思是，你要在在某個image(image id=6de62981d855)上，產生一個container，而且要有想要在container中開啟jupyterlab，於是產生一個對外接口(8888)連到本地的接口(82)，這樣我們在本地網頁輸入localhost:82就可以連到container內部的開發環境(jupyterlab)


## 將container檔案拷貝回本機

1. 找到container ID
2. 以docker cp指令把檔案帶回來(也可以把本機資料拷貝到container)
3. 例如你想要把這某container(0936cf2de3b6)中，/usr/src/my_workspace所有檔案都拷貝回本地路徑C:\Users\fire0\Desktop\volume\data_2下，你可以在本地的終端機輸入以下指令:

> docker cp 0936cf2de3b6:/usr/src/my_workspace C:\Users\電腦使用者名稱\Desktop\volume\data_2

## 進階

1. 容器(container)間可以透過Volume來達到資料共享，可參考此[教學](https://jchu.cc/2016/04/19-docker.html)。



## Reference:

1. https://jchu.cc/2016/04/19-docker.html
2. https://blog.gtwang.org/linux/docker-commands-and-container-management-tutorial/
3. https://hackmd.io/@bluewings1211/SJkLOW9_l?type=view#What-is-a-container