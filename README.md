gr Predictor 
====
## Requirement
Python 3.6~, Anaconda, dask, tesorflow

## License
“gr Predictor” is available under the GNU General Public License.

## Usage
* モジュールのインポート

  `import guv_prediction as gpred`
  
* 予測の実行

  `spliter = gpred.Prediction( pdb_dir, dx_dir, pred_area_center = np.array([x, y, z]), pred_area_range = w )`

  * `pdb_dir` : PDBファイルのパス      
  
  * `dx_dir` : 出力するdxファイルのパス
  
  * `pred_area_center` : 予測領域の中心座標 ( x,y,z [Å] ) 

  * `pred_area_range` : 予測領域の幅 ( w [Å] )    

## Usage for Google Colaboratory


1. GPUを使用するので、「ランタイム」タブ→「ランタイムのタイプを変更」→「GPU」を選択する．

2. colab上で以下を実行  
	`!git clone https://github.com/YoshidomeGroup-Hydration/gr-predictor.git`  
	`%cd /content/gr-predictor`  
	`import guv_prediction_f as gpred`  
	`%cd ../`

3. 予め水素原子を付加したPDBファイルを/content/にアップロードする。ここでは、PDB.pdbとする。
	
4. colab上で以下を実行  
	`spliter = gpred.Prediction("PDB.pdb", "PDB_pred.dx", model_dir="/content/gr-predictor/model_1.h5")`

5. 実行後、しばらくすると水和分布が/content/PDB_pred.dxに出力されるので、これをダウンロードする。PDB_pred.dxはchimeraやvmdで見ることができる。


## Usage for lab
  

* サブオプション一覧（研究室用）
  
  * `guv_dir` : guvファイルのパス。
  * `split_center` : ロスを最小化した部分の大きさ。デフォルトは 16 [voxel]
  * `split_size` : 分割する箱の大きさ。デフォルトは48 [voxel]
 
* クラス内の変数解説
  * spliter.g_pred : 予測g 
  * spliter.g_true : 正解g (guvファイルが与えられたときのみ生成)
  * spliter.g_true_local : 指定した局所領域における正解g　(guvファイルが与えられたときのみ生成)
  * spliter.g_pred_for_compare : 正解gと比較する用の局所領域における予測g。（予測する局所領域が3D-RISMの作るグリッド空間からはみ出す場合、正解gとの比較のためにはみ出した空間を予測gから切り出す。spliter.g_true_localと同じ形状になる）
  * 
* 訓練用プログラム：fukushimamemoの下にある
  *  common_model.ipynb：河間君のプログラム。勾配降下法の計算がSDGになっているので、Adamに変える
  *  common_model_GISTAI.ipynb：^のプログラムをGISTAIで動くように、福島君が修正。
  *  training-using-common_model.py：common_model.ipynbの訓練部分を```python ```で実行できるように修正。計算時間がかかるので、バックグラウンドで実行可能。ただし、npyファイルはcommon_model.ipynbで作成する必要がある。
  *  Deep-3DRISM.py & run_training_1_hydration.sh：福島君のDeepGistの訓練プログラムでgr Predictorの訓練を行う。実行は```sh run_training_1_hydration.sh```
