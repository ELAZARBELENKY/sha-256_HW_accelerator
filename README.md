# sha-256 HW accelerator
Hardware-accelerated SHA-256 implemented on a PYNQ-Z2 FPGA. Includes custom SystemVerilog IP, AXI4-Lite/DMA integration, and Python/Jupyter interface. Achieves faster hashing for large data vs CPU


**🔐 SHA-256 Hardware Accelerator on PYNQ**

This repository contains the full implementation and documentation of a hardware-accelerated SHA-256 hashing engine developed as a final university project.

The project leverages the Xilinx PYNQ-Z2 FPGA platform to accelerate cryptographic hashing using a custom SystemVerilog IP core connected via AXI4-Lite and AXI-Stream DMA. The host interface is implemented in Python using Jupyter Notebook, enabling interactive hashing, benchmarking, and result validation.


**🚀 Features**

- ✅ Full RTL implementation of SHA-256 per FIPS PUB 180-4  
- 🛠️ Custom SystemVerilog IP  
- 🐍 Python interface running on PYNQ’s ARM cores for easy control and testing  
- 📈 Performance comparison vs software hashing  
- 🧪 Includes simulation, testbenches, waveform analysis, and full documentation  


**📦 Contents**

* rtl/                  – SystemVerilog source for the SHA-256 core and wrapper  
* tb/                   – Testbenche, and waveform files
* scripts/              – Project automation scripts TCL  
* notebooks/            – Python Jupyter notebooks for usage, testing, and benchmarking  
* report/               – Full project book in Hebrew describing the theory, design, implementation, and results
* PYNQ_board_file/      - Board definition ZIP for Vivado (PYNQ-Z2)


**📊 Result Highlights**

Hardware beats software in performance for inputs ≥ 70KB

~2.6× speedup over CPU

Demonstrates how FPGAs can effectively offload compute-heavy cryptographic tasks


**🛠 Requirements**
* Vivado 2019.1
* PYNQ-Z2 board
* Python 3.x with PYNQ image
* Jupyter Notebook


## 🧪 Running the Program on the PYNQ Board

### 📋 Step-by-Step Instructions

1. **Connect to the PYNQ Board**
   - Power on the board and connect via USB (for powering the board) and Ethernet.
   - Ensure both your PC and the board are on the same subnet (e.g., `192.168.2.X`).
   - Open a browser and navigate to:

     ```
     http://<PYNQ_BOARD_IP>:9090
     ```

     📝 Example: `http://192.168.2.99:9090`

   - If unsure of the board’s IP:
     - Connect via serial/USB and log in (`username: xilinx`, `password: xilinx`), then run:
       ```bash
       ifconfig
       ```
     - Look for the `eth0` or `usb0` IP address.

   - Log in to the Jupyter interface (default password is often `xilinx`).

2. **Upload Required Files**
   Upload the following files from the repository to the **Jupyter main directory**

   - Bitstream files:
     ```
     bitstream/SHA256_BlockDesign_wrapper.bit
     bitstream/SHA256_BlockDesign_wrapper.hwh
     ```
   - Choose one of the following Python scripts:
     - `notebooks/user_interface.py` — for interactive image hashing
     - `notebooks/graphs.py` — for performance comparison and benchmarking

3. **Create and Run a Notebook**
   - In the Jupyter interface, click **New → Python 3 Notebook**.
   - Paste the contents of either `user_interface.py` or `graphs.py` into the notebook.
   - Run all cells.

---

### 🎨 For `user_interface.py`: Prepare the Image Directory

If you're running `user_interface.py` (the interactive mode for hashing images):

1. In the Jupyter file browser, create a directory named:
    pictures/

2. Upload the image(s) you want to hash into the `pictures/` directory.

3. When you run the notebook, follow the on-screen prompts to select and hash the image using the hardware accelerator.

> 📸 Example:
> Upload a file like `mycat.png` to `pictures/`, run the notebook, and you'll get its SHA-256 hash in real time.

---

That's it! You’re now running a custom cryptographic accelerator on bare-metal FPGA hardware, controlled via Python 🚀



## 🛠️ Rebuilding the Project in Vivado

⚠️ **Note:**
The generated bitstream (*.bit) and hardware description file (*.hwh) are already included in this repository under the bitstream/ folder.
You do not need to rebuild the project unless you want to:
*Inspect or modify the design
*Regenerate the bitstream manually
*Integrate with custom logic or interfaces
The instructions below are for recreating the Vivado project from scratch.

To recreate the Vivado project and regenerate the bitstream:

 0. Add PYNQ-Z2 Board File (Only Once)

    Vivado does **not** include the PYNQ-Z2 board by default. You need to install the board definition files **only if** this is your first time using the PYNQ-Z2 in Vivado 2019.1 on this machine:

   * Locate the following folder in this repository:  
   `sha-256_HW_accelerator/PYNQ_board_file/`

   * Extract the `.zip` file inside it (e.g., `pynq-z2.zip`).

   * Copy the extracted folder to the Vivado board files directory:

        `C:\Xilinx\Vivado\2019.1\data\boards\board_files\`
     > 📝 Replace \2019.1 with your version of vivado.

   * Restart Vivado if it was already open.

   *This step needs to be done only once per Vivado installation.*

1. **Clone this repository:**

       git clone https://github.com/ELAZARBELENKY/sha-256_HW_accelerator.git
       cd sha-256_HW_accelerator/scripts/

2. **Launch Vivado**  (recommended version: 2019.1) 

    On Windows:

       "C:/Xilinx/Vivado/2019.1/bin/vivado.bat"

   > 📝 Replace \2019.1 with your version of vivado.

    On Linux:

       vivado &

3. **Tcl Console**,

   In the Tcl Console (open with `Ctrl+Shift+T` inside Vivado), run:

       pwd

    Make sure the path ends in /scripts. If not, navigate there manually:

        cd /full/path/to/sha-256_HW_accelerator/scripts/

   > 📝 Replace /path/to/... with the actual full path to the project directory.

    Then run:

        source SHA256_HW_accelerator.tcl

4. **Vivado will:**
 *  Create a new project
 *  Add RTL sources and constraints
 *  Rebuild the block design
 *  Generate and add the BD wrapper
 *  Prepare everything for synthesis and implementation

5. **To generate the bitstream (optional step):**


       launch_runs impl_1 -to_step write_bitstream -jobs 4

   You can also do this via the Vivado GUI using Flow → Generate Bitstream.
