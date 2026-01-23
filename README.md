# AAE5303 Environment Setup Report — Submission

> **Important:** Follow this structure exactly in your submission README.  
> Your goal is to demonstrate **evidence, process, problem-solving, and reflection** — not only screenshots.

---

## 1. System Information

**Laptop model:**  
_Apple MacBook Pro 2021 (16 inch)_

**CPU / RAM:**  
_M1Pro Core,16GB Ram (12GB for VM)_

**Host OS:**  
_macOS 26.2（25C56）, VM: Ubuntu 22.04.5 LTS_

**Linux/ROS environment type:**  
_[Choose one:]_
- [ ] Dual-boot Ubuntu
- [ ] WSL2 Ubuntu
- [X] Ubuntu in VM (UTM ~~/VirtualBox/VMware/Parallels~~)
- [ ] Docker container
- [ ] Lab PC
- [ ] Remote Linux server

---

## 2. Python Environment Check

### 2.1 Steps Taken

Describe briefly how you created/activated your Python environment:

I have tried following the steps as stipulated in the assignment, however, since the MacBook is running in ARM environment and in a VM, open3D is an issue that could only available up to 0.18.0.  As discussed by classmates in the official WeChat Group, the environment is changed to “conda”.

**Tool used:**  
_conda_

**Key commands you ran:**
```bash
locale  # check for UTF-8
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
sudo apt update
sudo apt upgrade
sudo apt install ros-humble-desktop
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
source /opt/ros/humble/setup.bash #another terminal
ros2 run demo_nodes_cpp listener #another terminal
cd ~/Downloads
chmod +x Anaconda3-2025.12-1-Linux-aarch64.sh
./Anaconda3-2025.12-1-Linux-aarch64.sh
git clone https://github.com/qmohsu/PolyU-AAE5303-env-smork-test
conda create -n test1
conda activate test1
cd ~/PolyU-AAE5303-env-smork-test
source /opt/ros/humble/setup.bash
```

**Any deviations from the default instructions:**  
_Using conda as the environment; pip install -r requirements.txt not work, install the dependencies one by one manually_

### 2.2 Test Results

Run these commands and paste the actual terminal output (not just screenshots):

**Input1:**
```bash
python scripts/run_smoke_tests.py
```

**Output1:**
```
========================================
AAE5303 One-command Environment Check
Tip: read README.md for interpretation and fixes.
========================================


========== Step 1: Python + ROS environment checks ==========
Running: /home/mcllouie/anaconda3/envs/test1/bin/python -u /home/mcllouie/PolyU-AAE5303-env-smork-test/scripts/test_python_env.py
========================================
AAE5303 Environment Check (Python + ROS)
Goal: help you verify your environment and understand what each check means.
========================================

Step 1: Environment snapshot
  Why: We capture platform/Python/ROS variables to diagnose common setup mistakes (especially mixed ROS env).
Step 2: Python version
  Why: The course assumes Python 3.10+; older versions often break package wheels.
Step 3: Python imports (required/optional)
  Why: Imports verify packages are installed and compatible with your Python version.
Step 4: NumPy sanity checks
  Why: We run a small linear algebra operation so success means more than just `import numpy`.
Step 5: SciPy sanity checks
  Why: We run a small FFT to confirm SciPy is functional (not just installed).
Step 6: Matplotlib backend check
  Why: We generate a tiny plot image (headless) to confirm plotting works on your system.
Step 7: OpenCV PNG decoding (subprocess)
  Why: PNG decoding uses native code; we isolate it so corruption/codec issues cannot crash the whole report.
Step 8: Open3D basic geometry + I/O (subprocess)
  Why: Open3D is a native extension; ABI mismatches can segfault. Subprocess isolation turns crashes into readable failures.
Step 9: ROS toolchain checks
  Why: The course requires ROS tooling. This check passes if ROS 2 OR ROS 1 is available (either one is acceptable).
  Action: building ROS 2 workspace package `env_check_pkg` (this may take 1-3 minutes on first run)...
  Action: running ROS 2 talker/listener for a few seconds to verify messages flow...
Step 10: Basic CLI availability
  Why: We confirm core commands exist on PATH so students can run the same commands as in the labs.

=== Summary ===
✅ Environment: {
  "platform": "Linux-5.15.0-164-generic-aarch64-with-glibc2.35",
  "python": "3.10.12",
  "executable": "/home/mcllouie/anaconda3/envs/test1/bin/python",
  "cwd": "/home/mcllouie/PolyU-AAE5303-env-smork-test",
  "ros": {
    "ROS_VERSION": "2",
    "ROS_DISTRO": "humble",
    "ROS_ROOT": null,
    "ROS_PACKAGE_PATH": null,
    "AMENT_PREFIX_PATH": "/opt/ros/humble",
    "CMAKE_PREFIX_PATH": null
  }
}
✅ Python version OK: 3.10.12
✅ Module 'numpy' found (v2.2.6).
✅ Module 'scipy' found (v1.15.3).
✅ Module 'matplotlib' found (v3.10.8).
✅ Module 'cv2' found (v4.13.0).
✅ Module 'rclpy' found (vunknown).
✅ numpy matrix multiply OK.
✅ numpy version 2.2.6 detected.
✅ scipy FFT OK.
✅ scipy version 1.15.3 detected.
✅ matplotlib backend OK (Agg), version 3.10.8.
✅ OpenCV OK (v4.13.0), decoded sample image 128x128.
✅ Open3D OK (v0.19.0+80a41cd), NumPy 2.2.6.
✅ Open3D loaded sample PCD with 8 pts and completed round-trip I/O.
✅ ROS 2 CLI OK: /opt/ros/humble/bin/ros2
✅ ROS 1 tools not found (acceptable if ROS 2 is installed).
✅ colcon found: /usr/bin/colcon
✅ ROS 2 workspace build OK (env_check_pkg).
✅ ROS 2 runtime OK: talker and listener exchanged messages.
✅ Binary 'python3' found at /home/mcllouie/anaconda3/envs/test1/bin/python3

All checks passed. You are ready for AAE5303 🚀
✅ Python + ROS environment checks: PASS (9.9s)

========== Step 2: Open3D point cloud pipeline ==========
Running: /home/mcllouie/anaconda3/envs/test1/bin/python -u /home/mcllouie/PolyU-AAE5303-env-smork-test/scripts/test_open3d_pointcloud.py
ℹ️ Loading /home/mcllouie/PolyU-AAE5303-env-smork-test/data/sample_pointcloud.pcd ...
✅ Loaded 8 points.
   • Centroid: [0.025 0.025 0.025]
   • Axis-aligned bounds: min=[0. 0. 0.], max=[0.05 0.05 0.05]
✅ Filtered point cloud kept 7 points.
✅ Wrote filtered copy with 7 points to /home/mcllouie/PolyU-AAE5303-env-smork-test/data/sample_pointcloud_copy.pcd
   • AABB extents: [0.05 0.05 0.05]
   • OBB  extents: [0.08164966 0.07071068 0.05773503], max dim 0.0816 m
🎉 Open3D point cloud pipeline looks good.
✅ Open3D point cloud pipeline: PASS (0.4s)

ℹ️ Cleaned up 1 generated file(s) in data/.

========================================
OVERALL RESULT: PASS
========================================
```
**Screenshot1:** 
![run_smoke_tests](https://github.com/mcllouie/PolyU-AAE5303-assignment-1-Louie-Man/blob/main/Images/%E8%9E%A2%E5%B9%95%E6%88%AA%E5%9C%96%202026-01-22%2019.57.54.png)

**Input2:**
```bash
python scripts/test_python_env.py
```

**Output2:**
```
========================================
AAE5303 Environment Check (Python + ROS)
Goal: help you verify your environment and understand what each check means.
========================================

Step 1: Environment snapshot
  Why: We capture platform/Python/ROS variables to diagnose common setup mistakes (especially mixed ROS env).
Step 2: Python version
  Why: The course assumes Python 3.10+; older versions often break package wheels.
Step 3: Python imports (required/optional)
  Why: Imports verify packages are installed and compatible with your Python version.
Step 4: NumPy sanity checks
  Why: We run a small linear algebra operation so success means more than just `import numpy`.
Step 5: SciPy sanity checks
  Why: We run a small FFT to confirm SciPy is functional (not just installed).
Step 6: Matplotlib backend check
  Why: We generate a tiny plot image (headless) to confirm plotting works on your system.
Step 7: OpenCV PNG decoding (subprocess)
  Why: PNG decoding uses native code; we isolate it so corruption/codec issues cannot crash the whole report.
Step 8: Open3D basic geometry + I/O (subprocess)
  Why: Open3D is a native extension; ABI mismatches can segfault. Subprocess isolation turns crashes into readable failures.
Step 9: ROS toolchain checks
  Why: The course requires ROS tooling. This check passes if ROS 2 OR ROS 1 is available (either one is acceptable).
  Action: building ROS 2 workspace package `env_check_pkg` (this may take 1-3 minutes on first run)...
  Action: running ROS 2 talker/listener for a few seconds to verify messages flow...
Step 10: Basic CLI availability
  Why: We confirm core commands exist on PATH so students can run the same commands as in the labs.

=== Summary ===
✅ Environment: {
  "platform": "Linux-5.15.0-164-generic-aarch64-with-glibc2.35",
  "python": "3.10.12",
  "executable": "/home/mcllouie/anaconda3/envs/test1/bin/python",
  "cwd": "/home/mcllouie/PolyU-AAE5303-env-smork-test",
  "ros": {
    "ROS_VERSION": "2",
    "ROS_DISTRO": "humble",
    "ROS_ROOT": null,
    "ROS_PACKAGE_PATH": null,
    "AMENT_PREFIX_PATH": "/opt/ros/humble",
    "CMAKE_PREFIX_PATH": null
  }
}
✅ Python version OK: 3.10.12
✅ Module 'numpy' found (v2.2.6).
✅ Module 'scipy' found (v1.15.3).
✅ Module 'matplotlib' found (v3.10.8).
✅ Module 'cv2' found (v4.13.0).
✅ Module 'rclpy' found (vunknown).
✅ numpy matrix multiply OK.
✅ numpy version 2.2.6 detected.
✅ scipy FFT OK.
✅ scipy version 1.15.3 detected.
✅ matplotlib backend OK (Agg), version 3.10.8.
✅ OpenCV OK (v4.13.0), decoded sample image 128x128.
✅ Open3D OK (v0.19.0+80a41cd), NumPy 2.2.6.
✅ Open3D loaded sample PCD with 8 pts and completed round-trip I/O.
✅ ROS 2 CLI OK: /opt/ros/humble/bin/ros2
✅ ROS 1 tools not found (acceptable if ROS 2 is installed).
✅ colcon found: /usr/bin/colcon
✅ ROS 2 workspace build OK (env_check_pkg).
✅ ROS 2 runtime OK: talker and listener exchanged messages.
✅ Binary 'python3' found at /home/mcllouie/anaconda3/envs/test1/bin/python3

All checks passed. You are ready for AAE5303 🚀
```
**Screenshot2:** 
![test_python_env](https://github.com/mcllouie/PolyU-AAE5303-assignment-1-Louie-Man/blob/main/Images/%E8%9E%A2%E5%B9%95%E6%88%AA%E5%9C%96%202026-01-22%2019.59.01.png)

**Input3:**
```bash
python scripts/test_open3d_pointcloud.py
```

**Output3:**
```
ℹ️ Loading /home/mcllouie/PolyU-AAE5303-env-smork-test/data/sample_pointcloud.pcd ...
✅ Loaded 8 points.
   • Centroid: [0.025 0.025 0.025]
   • Axis-aligned bounds: min=[0. 0. 0.], max=[0.05 0.05 0.05]
✅ Filtered point cloud kept 7 points.
✅ Wrote filtered copy with 7 points to /home/mcllouie/PolyU-AAE5303-env-smork-test/data/sample_pointcloud_copy.pcd
   • AABB extents: [0.05 0.05 0.05]
   • OBB  extents: [0.08164966 0.07071068 0.05773503], max dim 0.0816 m
🎉 Open3D point cloud pipeline looks good.
```

**Screenshot3:**  
![test_open3d_pointcloud](https://github.com/mcllouie/PolyU-AAE5303-assignment-1-Louie-Man/blob/main/Images/%E8%9E%A2%E5%B9%95%E6%88%AA%E5%9C%96%202026-01-22%2019.59.59.png)

---

## 3. ROS 2 Workspace Check

### 3.1 Build the workspace

Paste the build output summary (final lines only):

```bash
source /opt/ros/humble/setup.bash
colcon build
```

**Expected output:**
```
Summary: 1 package finished [x.xx s]
```

**Your actual output:**
```
Summary: 1 package finished [0.21 s]
```

**Screenshot:**  
![Talker and Listener Running](https://github.com/mcllouie/PolyU-AAE5303-assignment-1-Louie-Man/blob/main/Images/%E8%9E%A2%E5%B9%95%E6%88%AA%E5%9C%96%202026-01-22%2020.43.59.png)

### 3.2 Run talker and listener

Show both source commands:

```bash
source /opt/ros/humble/setup.bash
source install/setup.bash
```

**Then run talker:**
```bash
ros2 run env_check_pkg talker
```

**Output (3–4 lines):**
```
[INFO] [1769086155.213263327] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #29'
[INFO] [1769086155.713226843] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #30'
[INFO] [1769086156.213227942] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #31'
[INFO] [1769086156.714606914] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #32'
```

**Run listener:**
```bash
ros2 run env_check_pkg listener
```

**Output (3–4 lines):**
```
[INFO] [1769086155.213409285] [env_check_pkg_listener]: I heard: 'AAE5303 hello #29'
[INFO] [1769086155.713494218] [env_check_pkg_listener]: I heard: 'AAE5303 hello #30'
[INFO] [1769086156.213376484] [env_check_pkg_listener]: I heard: 'AAE5303 hello #31'
[INFO] [1769086156.714908372] [env_check_pkg_listener]: I heard: 'AAE5303 hello #32'
```

~~**Alternative (using launch file):**~~
```bash
ros2 launch env_check_pkg env_check.launch.py
```

**Screenshot:**  
![Talker and Listener Running](https://github.com/mcllouie/PolyU-AAE5303-assignment-1-Louie-Man/blob/main/Images/%E8%9E%A2%E5%B9%95%E6%88%AA%E5%9C%96%202026-01-22%2020.49.13.png)

---

## 4. Problems Encountered and How I Solved Them

> **Note:** Write 2–3 issues, even if small. This section is crucial — it demonstrates understanding and problem-solving.

### Issue 1: venv Environment Does Not Support Open3D 0.19.0 in ARM System

**Cause / diagnosis:**  
_Open3D is still developing 0.19.0 for ARM linux system_

**Input:**
```bash
pip install open3d==0.19.0
```
**Output:**
```bash
ERROR: Could not find a version that satisfies the requirement open3d==0.19.0 (from versions: 0.16.0, 0.17.0, 0.18.0)
ERROR: No matching distribution found for open3d==0.19.0
```
**Fix:**  
_Use conda environment_

**Conda Environment Setup:**
```bash
cd ~/Downloads
chmod +x Anaconda3-2025.12-1-Linux-aarch64.sh
./Anaconda3-2025.12-1-Linux-aarch64.sh
git clone https://github.com/qmohsu/PolyU-AAE5303-env-smork-test
conda create -n test1
conda activate test1
cd ~/PolyU-AAE5303-env-smork-test
pip install open3d==0.19.0
```
*I intended to "Test" the environment so to name "test1", yet it worked with only one try. (Since the codes works, just don't touch it XD)

**Reference:**  
_Class Official WeChat, Anaconda, Google Ai Reply_

---

### Issue 2: The command: "python -m pip install -r requirements.txt" don't work

**Cause / diagnosis:**  
__

```bash
========================================
AAE5303 One-command Environment Check
Tip: read README.md for interpretation and fixes.
========================================


========== Step 1: Python + ROS environment checks ==========
Running: /home/mcllouie/anaconda3/envs/test1/bin/python -u /home/mcllouie/PolyU-AAE5303-env-smork-test/scripts/test_python_env.py
========================================
AAE5303 Environment Check (Python + ROS)
Goal: help you verify your environment and understand what each check means.
========================================

Step 1: Environment snapshot
  Why: We capture platform/Python/ROS variables to diagnose common setup mistakes (especially mixed ROS env).
Step 2: Python version
  Why: The course assumes Python 3.10+; older versions often break package wheels.
Step 3: Python imports (required/optional)
  Why: Imports verify packages are installed and compatible with your Python version.
Step 4: NumPy sanity checks
  Why: We run a small linear algebra operation so success means more than just `import numpy`.
Step 5: SciPy sanity checks
  Why: We run a small FFT to confirm SciPy is functional (not just installed).
Step 6: Matplotlib backend check
  Why: We generate a tiny plot image (headless) to confirm plotting works on your system.
Step 7: OpenCV PNG decoding (subprocess)
  Why: PNG decoding uses native code; we isolate it so corruption/codec issues cannot crash the whole report.
Step 8: Open3D basic geometry + I/O (subprocess)
  Why: Open3D is a native extension; ABI mismatches can segfault. Subprocess isolation turns crashes into readable failures.
Step 9: ROS toolchain checks
  Why: The course requires ROS tooling. This check passes if ROS 2 OR ROS 1 is available (either one is acceptable).
Step 10: Basic CLI availability
  Why: We confirm core commands exist on PATH so students can run the same commands as in the labs.

=== Summary ===
✅ Environment: {
  "platform": "Linux-5.15.0-164-generic-aarch64-with-glibc2.35",
  "python": "3.10.19",
  "executable": "/home/mcllouie/anaconda3/envs/test1/bin/python",
  "cwd": "/home/mcllouie/PolyU-AAE5303-env-smork-test",
  "ros": {
    "ROS_VERSION": "2",
    "ROS_DISTRO": "humble",
    "ROS_ROOT": null,
    "ROS_PACKAGE_PATH": null,
    "AMENT_PREFIX_PATH": "/opt/ros/humble",
    "CMAKE_PREFIX_PATH": null
  }
}
✅ Python version OK: 3.10.19
❌ Missing required module 'numpy'.
   ↳ Fix: python -m pip install -r requirements.txt
❌ Missing required module 'scipy'.
   ↳ Fix: python -m pip install -r requirements.txt
❌ Missing required module 'matplotlib'.
   ↳ Fix: python -m pip install -r requirements.txt
❌ Missing required module 'cv2'.
   ↳ Fix: python -m pip install -r requirements.txt
✅ Module 'rclpy' found (vunknown).
❌ Failed to import numpy: No module named 'numpy'
   ↳ Fix: pip install -r requirements.txt
❌ Failed to import scipy/fft: No module named 'numpy'
   ↳ Fix: pip install -r requirements.txt
❌ matplotlib import failed: No module named 'matplotlib'
   ↳ Fix: pip install -r requirements.txt
❌ OpenCV subprocess failed: Traceback (most recent call last):
  File "<string>", line 4, in <module>
ModuleNotFoundError: No module named 'cv2'
   ↳ Fix: Reinstall OpenCV: `pip install -r requirements.txt`
❌ Open3D subprocess failed: Traceback (most recent call last):
  File "<string>", line 5, in <module>
ModuleNotFoundError: No module named 'numpy'
   ↳ Fix: Reinstall Open3D: `pip install -r requirements.txt`
✅ ROS 2 CLI OK: /opt/ros/humble/bin/ros2
✅ ROS 1 tools not found (acceptable if ROS 2 is installed).
❌ colcon not found on PATH (required for ROS 2 builds).
   ↳ Fix: On Ubuntu/WSL2:
  - sudo apt update
  - sudo apt install python3-colcon-common-extensions
If apt says 'Unable to locate package python3-colcon-common-extensions', enable the Ubuntu 'universe' repository and retry:
  - sudo apt install -y software-properties-common
  - sudo add-apt-repository universe
  - sudo apt update
  - sudo apt install python3-colcon-common-extensions
If you are inside a minimal container with trimmed apt sources, you can also use pip:
  - python -m pip install -U colcon-common-extensions
✅ Binary 'python3' found at /home/mcllouie/anaconda3/envs/test1/bin/python3

Environment check failed (10 issue(s)).
❌ Python + ROS environment checks: FAIL (exit code 1) (0.4s)

========== Step 2: Open3D point cloud pipeline ==========
Running: /home/mcllouie/anaconda3/envs/test1/bin/python -u /home/mcllouie/PolyU-AAE5303-env-smork-test/scripts/test_open3d_pointcloud.py
❌ Required module missing: numpy. Run `pip install -r requirements.txt`.
❌ Open3D point cloud pipeline: FAIL (exit code 1) (0.0s)

========================================
OVERALL RESULT: FAIL
========================================
```

**Fix:**  
_Install all the dependencies one by one_

```bash
pip install numpy
pip install scipy
pip install matplotlib
pip install opencv-python
sudo apt update
sudo apt install python3-colcon-common-extensions
```

**Reference:**  
_Hint from the test result_

---

### Issue 3: There is no GUI in LTS version of Ubuntu

**Cause / diagnosis:**  
_ARM Server edition does not have GUI when installed_

**Fix:**  
_The same issue was encounter in the last semester in AAE5306_

```bash
sudo apt update 
sudo apt upgrade
sudo apt install slim
sudo apt install ubuntu-desktop
```

**Reference:**  
_My previous experience in AAE5306: https://github.com/weisongwen/AAE5306_2025-26S1/issues/10_

---

## 5. Use of Generative AI (Required)

Choose one of the issues above and document how you used AI to solve it.

> **Goal:** Show critical use of AI, not blind copying.

### 5.1 Exact prompt you asked

**Your prompt:**
```
[Copy-paste your actual message to the AI, not a summary]
```

### 5.2 Key helpful part of the AI's answer

**AI's response (relevant part only):**
```
[Quote only the relevant part of the AI's answer]
```

### 5.3 What you changed or ignored and why

Explain briefly:
- Did the AI recommend something unsafe?
- Did you modify its solution?
- Did you double-check with official docs?

**Your explanation:**  
_[Write your analysis here]_

### 5.4 Final solution you applied

Show the exact command or file edit that fixed the problem:

```bash
[Your final command/code here]
```

**Why this worked:**  
_[Brief explanation]_

---

## 6. Reflection (3–5 sentences)

Short but thoughtful:

- What did you learn about configuring robotics environments?
- What surprised you?
- What would you do differently next time (backup, partitioning, reading error logs, asking better AI questions)?
- How confident do you feel about debugging ROS/Python issues now?

**Your reflection:**

_[Write your 3-5 sentence reflection here]_

---

## 7. Declaration

✅ **I confirm that I performed this setup myself and all screenshots/logs reflect my own environment.**

**Name:**  
_Man Chi Lok Louie_

**Student ID:**  
_25001004G_

**Date:**  
_23 January 2026_

---

## Submission Checklist

Before submitting, ensure you have:

- [X] Filled in all system information
- [X] Included actual terminal outputs (not just screenshots)
- [X] Provided at least 2 screenshots (Python tests + ROS talker/listener)
- [X] Documented 2–3 real problems with solutions
- [ ] Completed the AI usage section with exact prompts
- [ ] Written a thoughtful reflection (3–5 sentences)
- [X] Signed the declaration

---

**End of Report**
