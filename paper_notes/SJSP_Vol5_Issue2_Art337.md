# 下半身キネマティクスにおける姿勢推定モデルの比較：検証研究
### Comparison of different pose estimation models for lower-body kinematics: A validation study

> [!NOTE]
> **💡 一言でいうと？**
> MediaPipeやYOLOなど6つの姿勢推定モデルをVICON（光学式）と徹底比較！ジョギングなどの動的タスクにおいて、MediaPipeはYOLOやMoveNetよりも有意に膝角度の推定精度が高い（エラーが少ない）ことが証明された論文。

## 🚀 主な貢献と新規性

![Architecture](images/SJSP_Vol5_Issue2_Art337_fig_1.png)

- 📌 **複数モデルの網羅的な検証**: MediaPipe, MeTRAbs (Small/X Large), YOLO, MoveNet (Lightning/Thunder) の6つのモデルを、スクワットやジャンプ、歩行、ジョギングの5つのタスクで比較。
- 📌 **MediaPipeの動的タスクにおける優秀性**: 歩行やジョギングといった動きの激しい（ブラーが発生しやすい）タスクにおいて、MediaPipeとMeTRAbsは、YOLOやMoveNetよりも有意に膝関節角度の誤差が少ないことが判明しました。

---

## 💡 研究への応用・インサイト

> [!TIP]
> **🎯 MediaPipeのモデル選定の正当化**

### 1. 「なぜMediaPipeを使うのか？」の強力な裏付け
田中様が現在ベースモデルとして採用されているMediaPipeは、他の軽量な推定モデル（YOLO等）と比較して、下半身のキネマティクス（特に歩行や走行時の膝角度）において優れた精度を持つことが学術的に裏付けられました。論文等の執筆時に「モデル選定の根拠」として引用できます。

### 2. 動的タスクでの誤差傾向の把握
動的なタスクではモデル間での精度の差が顕著になります。MediaPipeでも誤差はゼロではないため、どの動作（歩行なのかジャンプなのか）で精度が落ちやすいかを本論文から把握することで、外れ値（Outlier）除去アルゴリズムの「閾値調整」の参考にできます。

---

<details>
<summary>📄 全文翻訳（詳細）</summary>

## 1. 概要 (Abstract)
姿勢推定は運動学分析において大きな注目を集めており、近年多数のモデルが開発されています。しかし、モデルごとの長所と短所が異なるため、ユーザーが分析に最適なモデルを決定するのは困難です。
本研究の目的は、5人の男性参加者を対象に、スクワット、スクワットジャンプ、垂直跳び、歩行、ジョギングの5つのエクササイズを実施し、12台のRGBカメラと10台のVICONカメラ（基準）で記録し、モデルの精度を比較することです。
対象モデルは、Mediapipe, MeTRAbs (Small/X Large), YOLO, MoveNet (Lightning/Thunder) の6つです。結果として、ジョギングや歩行における膝関節角度の誤差において、YOLOとMoveNetはMediapipeやMeTRAbsと比較して有意に高い（精度が悪い）ことが示されました。

## 2. 実験結果と考察
- **静的タスク vs 動的タスク**: スクワットのような比較的静的なタスクではモデル間の差は小さかったですが、ジョギングのような動的タスクでは、アルゴリズムのトラッキング能力の差が顕著に出ました。
- **MediaPipeの安定性**: MediaPipeは他のモデルと比較して、時間的な連続性やブラーに対する耐性が高く、下半身の角度測定において信頼できる選択肢であることが示されました。

![Result Table](images/SJSP_Vol5_Issue2_Art337_fig_2.png)
![Result Graph](images/SJSP_Vol5_Issue2_Art337_fig_3.png)

</details>
