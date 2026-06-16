# A Deep Dive into MediaPipe Pose for Postural Assessment: A Comparative Investigation

## 📝 論文概要
本論文は、姿勢評価（Postural Assessment）におけるGoogle MediaPipe Poseの精度と信頼性を、光学式モーションキャプチャ（OPTO）やAzure Kinect（RGB-D）の基準システムと体系的に比較・評価した研究（2025年）です。MediaPipeの「ネットワーク複雑度（Complexity 0, 1, 2）」および「2D/3Dモデル」の違いによるパフォーマンスの変化を定量化しています。

### 主な貢献と新規性
1. **MediaPipe 3Dモデルの複雑度と精度の反比例の発見**: 直感に反して、MediaPipeの3D再構成は**モデルの複雑度（Complexity）を上げるほど精度が悪化し、深刻な歪みや非対称性（Asymmetry）が生じる**ことが明らかになりました。これは2Dトラッキングのエラーではなく、2Dから3Dへ持ち上げる（3D-Uplifting）処理に起因することが示されています。
2. **2Dモデルの高い実用性**: 逆に、MediaPipeの2D（2.5D）モデルは、前額面（Frontal plane）の分析において非常に優れたパフォーマンスを示し、軽量なアプリケーションにおける実用的な選択肢であることが証明されました。
3. **姿勢評価に対する具体的なガイドラインの提供**: 定量的な姿勢評価において、現在のMediaPipe 3Dモデル（特に高複雑度）は慎重に使用すべきであり、用途に応じて適切なモデルを選択するためのガイドラインを提示しました。

---

## 💡 田中様の研究への応用・インサイト (Notion風メモ)

### 1. MediaPipeのJump（急激な座標飛び）の原因究明への決定的証拠
田中様は `analyze_mp_jumps.py` や `visualize_jump_magnitudes.py` でMediaPipeのJump現象を分析されていますが、本論文の結論はまさにその核心を突いています。「MediaPipeの高Complexityモデルは3D再構成時に深刻な歪みを生む」という事実は、**田中様が観測しているOutlierやJumpの多くが、画像上の認識エラーではなく、MediaPipe内部の3D推論ネットワーク（Uplift）の不安定さに起因している**可能性を強く示唆しています。
👉 **アクション**: 分析対象を3D座標（ワールド座標）から2D画像座標に切り替えてJump頻度を比較するか、MediaPipeの初期化時に `model_complexity=0` または `1` に下げることで、ノイズ（Jump）が激減するかテストする価値が非常に高いです。

### 2. 真の外れ値（True Outlier）の切り分け
この論文の知見を踏まえると、外れ値（Outlier）を以下の2つに分離して分析するアプローチが有効です：
- **A. 2Dトラッキング由来のエラー** (真の認識ミス：オクルージョンやブレによるもの)
- **B. 3D Uplift由来のエラー** (MediaPipeのアルゴリズムの癖による歪み)
👉 **アイデア**: `generate_joint_error_analysis.py` において、2D座標と3D座標の誤差（分散）を個別に評価し、「2Dでは安定しているのに3DでJumpする関節」を特定することで、MediaPipe特有のアーティファクトを分離・補正する強力なアルゴリズムが作れます。

### 3. 左右非対称性（Symmetry Index）の評価基準への導入
論文内で使われている「Symmetry Index（左右対称性インデックス）」は、姿勢の安定性を測る良い指標です。MediaPipeが（本来対称であるはずの）直立姿勢で非対称な3D座標を出力する現象は、エラーの兆候として使えます。