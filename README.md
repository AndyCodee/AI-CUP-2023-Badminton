"# AI-CUP-2023-Badminton" 
## 運行環境

### 使用 Anaconda 建立環境

```bash
conda  create --name AICUP
conda activate AICUP
pip install -r requirement.txt
```

## 資料

- `data`: 將part1以及part2放入


## 資料處理

step1: 先將影片按幀數轉為圖片
step2: 運用+++++++++++++提供的yololabel，框出訓練資料中人以及球的位置，產出物件的位置txt檔，內含各物件的xywh(中心X座標, 中心Y座標, 物件寬, 物件高)。


##模型

本專案在辨識人以及球上，採用的是++++++++++++++提供的yolov8m.pt神經網絡架構，並依此架構訓練出自己的模型。
訓練好後建議放在weights中。
從++++++++下載yolov7pose放入weights中。

## 訓練

step1: 在'train_yolov8model/dataset.yaml'內設定好訓練圖片路徑以及相對應的位置txt檔。
step2: 在'train_yolov8model/train.ipynb'進行訓練。

## 預測

- 訓練後結果會存放在'train_yolov8model/runs/detect/train'內，此目錄底下的的'weights'會存放'best.pt'為最終訓練好的權重。
- 而訓練的confusion matrix則會自動產出於'train_yolov8model/runs/detect/train/confusion_matrix.png'中。
- 放圖片


## 重要模組介紹

Answer 寫入格式: 我們會先根據HitFrame判斷後續多個判斷式(如:Hitter, RoundHead,...)結果，並各別以List列出。
```python3

HirFrame = [7, 25, 47, 80]
Hitter = [A, B, A, B]
RoundHead = [2, 1, 2, 2]
Backhand = [1, 2, 2, 1]
BallHeight = [2, 2, 1, 2]

#球的位置LandingX,LandingY: 我們直接取用我們偵測到球存入total中的值。
LandingX = int(total[HitFrame[j]][0][0])
LandingY = int(total[HitFrame[j]][0][1])

HitterLocationX = [725, 557, 674, 524]
HitterLocationY = [582, 360, 533, 407]
DefenderLocationX = [520, 517, 680, 553]
DefenderLocationY = [401, 587, 403, 387]
BallType = [1, 3, 6, 6]
Winner = #Hitter[最後一球]的相反球

#最後所有資料以迴圈方式寫入.csv檔中。

writer = csv.writer(csvfile)
for j in range(len(HitFrame)):
  writer.writerow([videoname, j+1, HitFrame[j], Hitter[j], RoundHead[j], Backhand[j], BallHeight[j], int(total[HitFrame[j]][0][0]), int(total[HitFrame[j]][0][1]), HitterLocationX[j], HitterLocationY[j],               DefenderLocationX[j], DefenderLocationY[j], BallType[j], 'B'])
                    
```

## 模型權重(要改epoch)

|           Model Type            |   Epoch              | Public Score | Private Score | URL                                                                           
| :-----------------------------: | :------------------: | :----------: | :-----------: | :------------------------------------------------------------------------------------------------------ |
|    yolo自訓練模型 + yolov7pose   | 100 (train best.pt)  |   0.0709   | **0.0519**  | [Download](https://github.com/ShinoharaHare/AI-CUP-2022-Fall-NLP/releases/download/v0.0.0/s-sp-lstm.pt) |

- 建議將下載好的權重存放至 `weights` 資料夾，演示預測流程的 Jupyer Notebook 預設會從此資料夾載入權重
