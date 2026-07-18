#### How to Set Up QIIME 2 in Docker, Configure Jupyter, and Create Notebooks on macOS (Tested on macOS 26.5, M1 Pro)

1. Install docker desktop from official website, from [here](https://www.docker.com/products/docker-desktop/)
2. Launch new terminal (using iTerm2 or any other app you may like)
3. Pull official Qiime2 docker instance from dockerhub as explained [here](https://library.qiime2.org/quickstart/qiime2). Current version is 2026.4 and command to pull Qiime 2026.4 is `docker pull quay.io/qiime2/qiime2:2026.4`
4. List the images installed on local machine and image name would be `quay.io/qiime2/qiime2:2026.4`
5. Start the container with following command: `docker run  --rm -it quay.io/qiime2/qiime2:2026.4`
6. By default, `bash` shell is the command available in docker container
7. You will be placed as root inside the container
8. The OS within container as of 2026 July is Debian Trixie
9. Update the repos, install pip (pip3) and jupyter lab with following commands

```
$ apt update
$ apt install -y python3-pip
$ pip3 install jupyterlab
```

10. Do not exit from the terminal/shell.
11. Open another terminal within host OS (mac machine here) and list the current running containers with following command `docker ps`. It should list the Qiime2 container
12. Note down the name of the container and in the second terminal, that is still open, run below command to save the updated container as a new image `docker commit <container name> <new_image_name>` (eg. qiime2_w_jupyter). Difference between original image and new image is that new image has jupyter lab installed in it.
13. You can close the first terminal now.
14. You can either delete the original image downloaded or delete it to save some space and/or avoid confusion
15. Start the a new container with new image as below:

```
$ docker run -it --rm  -p 8888:8888  -v $(pwd):/data  <new_image_name>  jupyter lab --ip=0.0.0.0 --port=8888 --allow-root
```

16. New image is around ~10 GB. It will few seconds to start, and launch jupyter server. After launching jupyter server, terminal prints URL to access Jupyter server (eg URL: http://127.0.0.1:8888/lab?token=<random_alpha_numberic_string>).
17. Once you have the URL, you can use browser on local machine, to access Jupyter notebook
18. To use this in VSCode, user has two ways: a. access the container b. access the jupyter server
19. To access jupyter server and start creating a jupyter notebook:
    1. Launch New Jupyter Notebook in VSCode
    2. Click on "Select Kernel" on top right corner and new options appear to select python interpreter and Jupyter server
    3. Click on "Existing Jupyter Server" (2nd option)
    4. Paste the URL obtained in step 16
    5. After that, notebook asks you to select python interpreter (python version) to execute the notebook. Select the python interprter
20. Now test if Qiime2 works with following commands in jupyter notebook

```python
import qiime2
qiime2.__version__
```

Expected output:

```
2026.4.0
```
