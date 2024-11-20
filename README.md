### Kakip Board

### Prerequisites
Ensure the following:
- Complete the [Getting Started](https://renesas-rz.github.io/rzv_ai_sdk/getting_started) instructions provided by Renesas.
- The Kakip board is set up, including the preparation of the SD card.
- The `rzv2h_ai_sdk_image` Docker container is running on the host machine.

### Kakip Board Resources
For additional information on the Kakip board, please refer to:
- **Website**: [Kakip Board Official Website](https://www.kakip.ai/)
- **GitHub Repository**: [Kakip Board GitHub Repository](https://github.com/Kakip-ai/kakip_ai_apps/tree/main)

Refer to the image below for a visual guide to setting up the Kakip board for development: 
<img src="./Kakip.png" alt="Sample application output"
     margin-right=10px; 
     width=600px;
     height=334px />

# Head Count Top View Application

This repository contains the `11_Head_count_topview` application. Below are the detailed steps for setting up, applying patches, running the application, and building it if needed.

---

## 1. Clone the Repository

To begin, clone this repository using the following command:

```sh
git clone https://github.com/Ignitarium-Renesas/rzv_ai_apps.git
```
 > Note 1: Please verify the git repository url if error occurs.

> Note 2: This command will download whole repository, which include all other applications.<br> 
If you have already downloaded the repository of the same version, you may not need to run this command.

Navigate to the application directory:

```sh
cd rzv_ai_apps/11_Head_count_topview/
```
## 2. Get the Patch File
To get the patch file, clone this repository using the following command:
```sh
cd ../..  # Navigate back to the base directory of rzv_ai_apps
git clone https://github.com/other-repo/patches.git
```

## 3. Apply the Patch File
Apply the provided patch file to the application using the command below:

```sh
cd rzv_ai_apps/11_Head_count_topview/
patch -p0 < ../../Kakip_RZV2H_Demos/Head_count_topview/Head_count_topview.patch
```
## 4. Copy the Object File
Copy the prebuilt object file from this repository to the application executable folder. This step is required to use the application without recompilation.
```sh
cp -r ../../Kakip_RZV2H_Demos/Head_count_topview/head_count_topview_app exe_v2h
```

## 5. Run the Application
To run the application on the Kakip board:

Execute the application with the following command, specifying either USB camera or IMAGE input mode:

- **Image Input**:
    ```sh
    ./head_count_topview_app IMAGE ../img/sample.jpeg
    ```

- **USB Camera Input**:
    ```sh
    ./head_count_topview_app USB
    ```

Following window shows up on HDMI screen.
<img src="./Head_count_topview/output_image.png" alt="Sample application output"
     margin-right=10px; 
     width=600px;
     height=334px />

## 6. Compile the Application (Optional)

If you want to modify the code and recompile the application, follow the steps below:

### Step 1: Refer to the SDK Getting Started Guide
Visit the Renesas RZ/V AI SDK Getting Started Guide for the setup.
### Step 2: Clone the Repository
It is recommended to download/clone the repository on the `data` folder which is mounted on the `rzv2h_ai_sdk_container` docker container as shown below. 
 ```sh
    cd <path_to_data_folder_on_host>/data
    git clone https://github.com/Ignitarium-Renesas/rzv_ai_apps.git
```
> Note 1: Please verify the git repository url if error occurs.

> Note 2: This command will download whole repository, which include all other applications.<br>
     If you have already downloaded the repository of the same version, you may not need to run this command.
### Step 3: Start the Docker Container  
Run (or start) the docker container and open the bash terminal on the container.  
Here, we use the `rzv2h_ai_sdk_container` as the name of container, created from  `rzv2h_ai_sdk_image` docker image.  
    > Note that all the build steps/commands listed below are executed on the docker container bash terminal.  

### Step 4: Set Environment Variables
Set your clone directory to the environment variable.  
```sh
    export PROJECT_PATH=/drp-ai_tvm/data/rzv_ai_apps
```
### Step 5: Navigate to the Application Directory
Move to the source code directory of the application:  
```sh
    cd ${PROJECT_PATH}/11_Head_count_topview/src
```
### Step 6: Build the Application
Build the application by following the commands below.  

```sh
    mkdir -p build && cd build
    cmake -DCMAKE_TOOLCHAIN_FILE=./toolchain/runtime.cmake -DV2H=ON ..
    make -j$(nproc)
```
### Step 7: Locate the Generated Application
The built application file will be available in the following directory:
 ```sh
    ${PROJECT_PATH}/11_Head_count_topview/src/build
```
The generated file will be named:   
```sh
    head_count_topview_app
```