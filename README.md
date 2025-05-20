# Software Defined Networking
Switches and routers play a critical role in enterprise networks. Traditionally, these network devices have been responsible for deciding which path packets should follow, as well the actual data trasnfer (forwarding). In other words, the control and data flow functionalities of the network devices have been dealt with at each swithch and router, individually. While *control* refers to determining step-by-step movement of a given packet from source to destination, *forwarding* means the flow of data packets from the source to the destination. In tradtional computer networks, switches and routers are responsible for both the control and data forwarding functions.

<p align="center">
  <img src="figures/traditional_switch.png"/>
</p>
<p align="center"><strong>Figure 1:</strong> Non-SDN Switches with control and data planes.</p>


However, the merger of control and data forwarding planes has several limitations including:
## 


# Mininet
-- Get Mininet: https://mininet.org/download/
Mininet is available for common operating systems (OSes), including Linux, macOS and Windows. There are multiple ways to install Mininet in each of the OSes. Among these, the most foolproof method is to install a pre-packaged Mininet Virual Machine (VM) image in a virtualization systems such as [VirtualBox](https://www.virtualbox.org/) and [VMWare](https://www.vmware.com/).
-- Mininet Walkthrough: https://mininet.org/walkthrough/
Software Defined Networking 

# Dataset
### Download the CSV files of the UNSW-NB15 dataset from [CSVs](https://cloudstor.aarnet.edu.au/plus/index.php/s/2DhnLGDdEECo4ys?path=%2FUNSW-NB15%20-%20CSV%20Files). 

Move the downloaded CSV files to the **unsw_nb15_dataset** project subdirectory as shown below.
  ```bash
  mkdir data
  cd data
  mkdir unsw_nb15_dataset
  mv /path/to/UNSW-NB15-CSV-FILES/* ./unsw_nb15_dataset/
  ```

### Download the PCAP files of the UNSW-NB15 dataset from [PCAPs](https://cloudstor.aarnet.edu.au/plus/index.php/s/2DhnLGDdEECo4ys?path=%2FUNSW-NB15%20-%20pcap%20files/).

Move the PCAP files inside **pcaps 17-2-2015** and **pcaps 22-1-2015** to **feb_pcap** and **jan_pcap** project subdirectories, respectively, as shown below.
  ```bash
  mkdir feb_pcap
  mv /path/to/pcaps 17-2-2015/* ./feb_pcap # UNSW-NB15 PCAPs in February
  mkdir jan_pcap
  mv /path/to/pcaps 22-1-2015/* ./jan_pcap  # UNSW-NB15 PCAPs in January
  ```

**Note:**
<!--
+ At the time of writing this document, the CSV and PCAP files of the UNSW-NB15 dataset are available at the above URLs. If the URLs of the files change, they are to be downloaded from the new URLs according to announcements by the authors of the UNSW-NB15 IDS dataset or their affiliated institution.
-->
+ The PCAP files of the dataset are nearly 100 Gigabyte. So, it is important to make sure enough disk storage is avaialbe for the PCAP and CSV files of the UNSW-NB15 dataset as well as free disk space to install the required packages for the project.


# Installation
## Install Docker Engine on Ubuntu
Installation using the Apt repository. Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterwards, you can install and update Docker from the repository. More details at https://docs.docker.com/engine/install/ubuntu/
### Set up Docker's Apt repository.
Add Docker's official GPG key:
  ```bash
  sudo apt-get update
  sudo apt-get install ca-certificates curl gnupg
  sudo install -m 0755 -d /etc/apt/keyrings
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  sudo chmod a+r /etc/apt/keyrings/docker.gpg
  ```
Add the repository to Apt sources:
  ```bash
  echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt-get update
  ```

### Install the Docker packages.
  ```bash
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  ```

### Verify that the Docker Engine installation is successful by running the hello-world image.
The following command downloads a test image and runs it in a container.
  ```bash
  sudo docker run hello-world
  ```
When the container runs, it should print a confirmation message. If so, you have now successfully installed and started Docker Engine.
<!-- ### Install NVIDIA GPU Drivers for Tensorflow on host machine
  ```bash
  sudo apt install nvidia-cuda-toolkit
  ```
 -->
 
# Usuage
Open terminal and run the project using the following commands.
  ```bash
  cd multimodal-nids
  mkdir results
  docker compose build
  docker compose up postgres
  ```

Note that the PosgreSQL database will be stored on docker_data subdirectory, which is on the same directory as the multimodalnids project directory. The container and other associated files are also stored in the docker_data subdirectory.

Finally, ensure that no other application is using port number 5432, which is needed to run postgresql database. If port 5432 is in use, unused port can be setup for multimodalnids in the docker-compose.yaml.

To run Multimodal-NIDS, open another terminal tab (Ctrl + Shift + T) and use the following command to run the project.
  ```bash
  docker compose up multimodalnids # on the new terminal tab
  ```
 Multimodal-NIDS has two ML subsystems, a flow-based susbsytem and a payload-based subsystem, which are trained using flow-based traffic features and the first 32 bytes of protocol payload, respectively. Separate ML models are trained for Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) of the UNSW-NB15 dataset. Note that TCP and UDP constitute more than 97% of total traffic flows in the dataset. The trained ML models as well as the model evaluation results are saved in the *results* subdirectory.
# Citation
Our paper [Network intrusion detection leveraging multimodal features](https://www.sciencedirect.com/science/article/pii/S2590005624000158?via%3Dihub) presents details of how the proposed multimodal NIDS works and the obtained experimental results.
If you would like to refer the paper in your research, please use the following citation details.
```bash
@article{kiflay2024network,
  title={Network intrusion detection leveraging multimodal features},
  author={Kiflay, Aklil and Tsokanos, Athanasios and Fazlali, Mahmood and Kirner, Raimund},
  journal={Array},
  pages={100349},
  year={2024},
  publisher={Elsevier}
```

