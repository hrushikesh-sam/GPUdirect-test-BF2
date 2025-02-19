# Setting Up GPU Packet Processing on Supermicro Dual-Socket Machine with BlueField-2 DPU

## Prerequisites
Before proceeding with the setup, ensure that the following components are available and properly installed:
- Supermicro dual-socket machine
- NVIDIA RTX A5000 GPU
- NVIDIA BlueField-2 2x25G HHHL DPU
- Ubuntu (latest LTS recommended)
- NVIDIA Toolkit and Drivers


## Step 1: Install/unpack doca host repository and RShim driver

Download the DOCA host repo from the NVIDIA DOCA Downloads page.

Unpack the repo file (deb or rpm) on your system:

For deb-based repo:

```sh
host# sudo dpkg -i <repo_file>
```
RShim is required to communicate with the BlueField-2 DPU over PCIe  Install it using:

```sh
sudo apt-get install -y rshim
```

## Step 2: Start RShim Service
Enable and start the RShim service to allow communication with the DPU:

```sh
sudo systemctl enable rshim
sudo systemctl start rshim
```

Check the status of the service to ensure it is running:

```sh
systemctl status rshim
```

## Step 3: Installing Full DOCA Image on DPU via Host
### Warning
This step overwrites the entire boot partition.

### Note
This installation sets up the OVS bridge.

### Install Bluefield with firmware
#### Note
To change the default Ubuntu password during the BFB bundle installation, proceed to Option 2.

BFB installation is executed as follows:

```sh
host# sudo bfb-install --rshim rshim<N> --bfb <image_path.bfb>
```

Where `rshim<N>` is `rshim0` if you only have one BlueField. You may run the following command to verify:

```sh
host# ls -la /dev/ | grep rshim
```

This is how firmware is installed with BFB.


## Step 4: Assign IP Address to `tmfifo_net0`
The `tmfifo_net0` interface is created when `rshim` starts. Assign an IP address to it:

```sh
sudo ip addr add 192.168.100.1/24 dev tmfifo_net0
sudo ip link set tmfifo_net0 up
```

Verify the IP assignment:

```sh
ip addr show tmfifo_net0
```

## Step 5: Ensure CUDA NVCC and NVLink Are Installed
Ensure the NVIDIA CUDA Compiler (`nvcc`) and NVLink are installed and available in the system path:

```sh
nvcc --version
nvlink --version
```

If they are not installed, install them using:

```sh
sudo apt-get install -y cuda-nvlink
```

## Step 6: Install GDRCopy
GDRCopy is a low-latency GPU memory copy library. Install it from the GitHub repository:

```sh
git clone https://github.com/NVIDIA/gdrcopy.git
cd gdrcopy
make
sudo make install
```

Run the test scripts to verify the installation:

```sh
cd tests
./gdrcopy_sanity
```

## Step 7: Configure Huge Pages
Huge pages improve memory access efficiency for high-performance networking applications. Configure them as follows:

Edit the GRUB configuration:

```sh
sudo vim /etc/default/grub
```

Add the following options to `GRUB_CMDLINE_LINUX_DEFAULT`:

```
default_hugepagesz=1G hugepagesz=1G hugepages=4
```

Update GRUB and reboot:

```sh
sudo update-grub
sudo reboot
```

After reboot, verify huge pages:

```sh
grep -i huge /proc/meminfo
```

## Step 8: Installing meta packages to compile the gpu packet processing application



Ensure the necessary packages are installed:

```sh
sudo apt-get -y install protobuf-compiler \
    doca-sdk-gpunetio libdoca-sdk-gpunetio-dev \
    protobuf-compiler-grpc cmake grpc++ libgrpc++-dev 
```

## Step 9: Set Environment Variables
Set the required environment variables for DOCA and DPDK:

```sh
export PKG_CONFIG_PATH=/opt/mellanox/dpdk/lib/x86_64-linux-gnu/pkgconfig:/opt/mellanox/doca/lib/x86_64-linux-gnu/pkgconfig
export LD_LIBRARY_PATH=/opt/mellanox/dpdk/lib/x86_64-linux-gnu:/opt/mellanox/doca/lib/x86_64-linux-gnu
export PATH=/usr/local/cuda/bin:$PATH
```

## Step 10: Navigate to the DOCA Applications Directory

```sh
cd /opt/mellanox/doca/applications/
```

## Step 11: Modify Meson Options for GPU Packet Processing
Edit `meson-options.txt` to enable only the GPU packet processing application.

```sh
sudo vim meson-options.txt
```

Comment out all other applications and ensure only `gpu_packet_processing` is enabled.

## Step 12: Build the Application
Build the GPU packet processing application using:

```sh
meson build
ninja -C build
```

## Step 13: Run the Application
Navigate to the build directory and run the application with the appropriate PCI addresses:
### Note 
check whether the nvidia_peermem driver is running via lsmod

```
lsmod | grep -i nvidia_peermem

## if it outputs nothing then run

sudo modprobe nvidia_peermem
```
```sh
cd /opt/mellanox/doca/applications/build/gpu_packet_processing
./doca_gpu_packet_processing -n <NIC_PCI_ADDR> -g <GPU_PCI_ADDR> -q 4 -s
```

Replace `<NIC_PCI_ADDR>` and `<GPU_PCI_ADDR>` with the actual PCI addresses of the NIC and GPU.

## Conclusion
Following these steps will set up the system for GPU-accelerated packet processing using DOCA and NVIDIA BlueField-2. Ensure all dependencies are properly installed and the required services are running before executing the application.

