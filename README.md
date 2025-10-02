# healthcare-navigation-robot

# Healthcare Navigation Assistant Robot

[![ROS-Version-Noetic](https://img.shields.io/badge/ROS-Noetic-blueviolet)](http://wiki.ros.org/noetic)
[![Language-Python](https://img.shields.io/badge/Language-Python-blue)](https://www.python.org/)
[![License-MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

A ROS-based autonomous navigation system for the LoCoBot platform, designed to assist healthcare workers in hospital environments. This project leverages simulation in NVIDIA Isaac Sim and real-world deployment to create a safe and efficient human-robot cooperation system.



## 專案目標

本專案旨在開發一款能夠在繁忙的醫院環境中自主導航的助理機器人。它的核心任務是安全地穿梭於走廊和病房間，同時能識別醫護人員、病患和醫療設備，並與周遭的人進行簡單的互動，以減輕醫護人員的工作負擔。

## 核心功能

* **自主導航**: 使用 ROS `move_base` 在模擬和真實的醫院環境中實現點對點的路徑規劃與導航。
* **人員與物體偵測**: 透過 YOLOv5/v8 偵測醫護人員（不同制服）、病患（輪椅、病號服）以及關鍵醫療設備。
* **安全互動協議**:
    * **主動避障**: 結合 2D 雷射雷達和 3D 點雲資料，動態避開靜態與動態障礙物。
    * **語音提示**: 在接近或需要通過時，播放 "Excuse me, robot passing through" 等語音提示。
    * **狀態指示**: 使用 LED 燈條顯示機器人狀態（如：導航中、等待中、任務完成）。
    * **緊急停止**: 備有物理與軟體緊急停止機制。
* **高擬真模擬**: 在 NVIDIA Isaac Sim 中建立醫院場景，進行開發、測試與演算法驗證，縮短實體部署的迭代週期。

## 系統架構

本系統基於 ROS1 Noetic，各功能模組化為獨立的 ROS 節點，彼此透過 `topics`, `services`, 和 `actions` 進行通訊。

* **/locobot_navigation**: 包含 `move_base` 設定、全域與區域地圖、路徑規劃器等，負責導航任務。
* **/locobot_vision**: 啟動相機驅動並運行 YOLO 偵測節點，將偵測結果（Bounding Boxes 和類別）發布到 ROS topic。
* **/locobot_safety**: 訂閱視覺偵測結果與雷射雷達數據，當偵測到人類或障礙物過近時，會覆寫導航指令或觸發緊急停止。
* **/loc_hri**:管理人機互動，例如根據機器人狀態控制 LED 或播放語音。

## 使用的技術

* **機器人作業系統**: ROS1 Noetic
* **模擬環境**: NVIDIA Isaac Sim
* **電腦視覺**: Python, OpenCV, YOLOv5/v8, PCL
* **導航**: `move_base`, `gmapping`, `amcl`
* **硬體平台**: Interbotix LoCoBot
* **開發工具**: Docker, Git, GitHub

## 安裝與執行

### 前置需求

* Ubuntu 20.04
* ROS1 Noetic
* NVIDIA Isaac Sim 2023.1 or later
* NVIDIA GPU Driver & Docker

### 步驟

1.  **Clone 儲存庫**
    ```bash
    git clone [https://github.com/](https://github.com/)dddcccgw/Healthcare-Navigation-Robot.git
    cd Healthcare-Navigation-Robot/
    ```

2.  **建立 ROS 工作環境**
    ```bash
    catkin_make
    source devel/setup.bash
    ```

3.  **下載模型權重**
    將預訓練的 YOLO 模型權重檔 (`best.pt`) 放入 `src/locobot_vision/weights/` 資料夾中。

### 執行範例

1.  **啟動模擬環境**:
    在 Isaac Sim 中載入 `hospital_environment.usd` 場景。

2.  **啟動機器人**:
    在新終端機中，啟動模擬中的機器人：
    ```bash
    roslaunch locobot_control main_sim.launch
    ```

3.  **啟動導航與視覺節點**:
    在另一個終端機中，啟動導航與視覺功能：
    ```bash
    roslaunch locobot_navigation navigation_sim.launch
    roslaunch locobot_vision vision.launch
    ```

4.  **在 Rviz 中設定目標**:
    使用 Rviz 的 "2D Nav Goal" 工具來指定機器人的導航目標點。

## 專案成果

* **GitHub Repo**: 包含所有程式碼、設定檔與文件。
* **技術報告**: 一份 8-12 頁的詳細報告，說明系統設計、實作細節與實驗結果。
* **展示影片**: [連結至您的 YouTube 展示影片]

## License

本專案採用 MIT License。詳情請見 [LICENSE](LICENSE) 文件。
