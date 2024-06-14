# ARC Cluster Guide
This guide provides quick steps to start using the ARC cluster at the University of Calgary. For more detailed information, please refer to the [ARC Cluster Guide wiki](https://rcs.ucalgary.ca/ARC_Cluster_Guide).
## Getting Started

1. **Request Cluster Account**:
   - Email [hpc@support.ucalgary.ca](mailto:hpc@support.ucalgary.ca) to request a cluster account.

2. **Connect to U of C Network**:
   - Connect to the campus network or use the [General VPN](insert_vpn_link_here).

3. **Connecting to the Cluster**:
   - **For Windows Users**:
     - Download and open MobaXterm.
     - Enter the following command in the MobaXterm terminal:
       ```
       ssh -X username@arc.ucalgary.ca
       ```
     (Replace `username` with your email address.)
   
   - **For Mac Users**:
     - Use Terminal or a preferred terminal application (e.g., Termius).
     - Enter the following command in your terminal:
       ```
       ssh username@arc.ucalgary.ca
       ```
     (Replace `username` with your email address.)

## Starting Python

1. **Uploading Files (Code or Data)**:
   - Use the navigator in the left-hand side of MobaXterm or SFTP in Termius for file uploads.

2. **Useful Commands**:

   - **Loading Python**:
     ```
     module load python/anaconda3-2018.12
     ```

   - **Checking Available Modules**:
     ```
     module list
     ```

   - **Removing a Module**:
     ```
     module remove module_name
     ```

   - **Installing Python Libraries** (use before calling Python):
     ```
     pip install --user bctpy
     ```

   - **Starting Python**:
     ```
     python
     ```

   - **Running Your Python Script**:
     ```
     python mob.py
     ```

   - **Importing a Python File** (previously uploaded to the cluster):
     ```
     import python_file_name
     ```

   - **Using Uploaded Datasets in Python**:
     ```python
     import os
     import pickle

     data_file_path = os.path.join(os.getenv("HOME"), "my_project", "data", "data.pkl")
     with open(data_file_path, 'rb') as file:
         data = pickle.load(file)
     ```
## Running a Code for Longer Time Using SLURM

To run a code for an extended duration on the ARC cluster using SLURM, follow these steps:

1. **Opening a Text Editor to Write Your Job Script**:
   - Use a text editor like `nano` to create your job submission script.
  ```bash
nano submit_job.sh
```

2. **Paste the Following Content into Your Text Editor** (`submit_job.sh`):

   ```bash
   #!/bin/bash
   #SBATCH --job-name=my_python_job
   #SBATCH --output=my_python_job_output.log
   #SBATCH --error=my_python_job_error.log
   #SBATCH --time=07:00:00  # Adjust the time as needed (e.g., 07:00:00 for 7 hours)
   #SBATCH --partition=single
   #SBATCH --nodes=1
   #SBATCH --ntasks=1
   #SBATCH --cpus-per-task=4
   #SBATCH --mem=8G
   module load python/anaconda3-2018.12
   python mob.py

3. **You can increase the number of nodes or CPUs, but ensure your code is compatible:**
\
If you increase the number of nodes or CPUs, modify your code to be compatible with higher computational resources. Write sinfo in the terminal to check the possible option.
4. **Save and Exit**
\
Press Ctrl+X to exit nano and Ctrl+O to save the script.
5. **Make the Script Executable:**
```bash
chmod +x submit_job.sh
```
6.**Submitting the Job:**
```bash
sbatch submit_job.sh
```
7.**Verifying Job Submission:**
```bash
squeue -u username
```
8.**Canceling a Job:**
```bash
scancel job_id
```
9.**Checking Job Status and Usage:**
```bash
sacct -j <job_id>
sstat -j <job_id>
```
10.**Setting Email Notifications:**
```bash
#SBATCH --mail-user=your.email@example.com
#SBATCH --mail-type=ALL
```
