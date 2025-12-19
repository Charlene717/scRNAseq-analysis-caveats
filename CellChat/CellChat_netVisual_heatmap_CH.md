# 注意事項：`netVisual_heatmap()` **無法** 依 signaling pathway 進行子集分析

本文件說明 **CellChat `netVisual_heatmap()`** 在比較分析（comparison mode）中的一個**容易被忽略但非常重要的行為限制**。

---
  
  ## ⚠️ 重要警告
  
  > **`netVisual_heatmap()` 目前並不支援依照特定 signaling pathway（`signaling` 參數）來繪製 pathway-specific heatmap。**
  
  即使在函式中明確指定 `signaling`，輸出的 heatmap **仍然代表所有 signaling pathways 的總體（aggregate）cell–cell communication**，而非指定 pathway（例如 SEMA3）。

如果未意識到此行為，**極容易導致錯誤解讀與過度詮釋**。

---
  
  ## 容易誤導的使用範例
  
  ```r
# 目的：想畫 SEMA3 sender → receiver 在 Keloid 與 Normal Skin 間的差異
p_heat_weight <- netVisual_heatmap(
  cellchat_merged,
  signaling  = SIGNALING_USE,      # 例如 "SEMA3"
  comparison = comparison_use,
  measure    = "weight"
) + ggtitle(paste0(SIGNALING_USE, " heatmap weight (Keloid - Normal Skin)"))

p_heat_count <- netVisual_heatmap(
  cellchat_merged,
  signaling  = SIGNALING_USE,
  comparison = comparison_use,
  measure    = "count"
) + ggtitle(paste0(SIGNALING_USE, " heatmap count (Keloid - Normal Skin)"))
```

### ⚠️ 關鍵問題

即使設定了 `signaling = "SEMA3"`，上述 heatmap **實際上仍然顯示的是「全體 signaling network」的差異**，而不是 SEMA3 專屬的訊號變化。

圖的標題看起來是 pathway-specific，但**底層資料並沒有被 pathway 篩選**。

---
  
  ## `netVisual_heatmap()` 實際在計算什麼？
  
  在設定 `comparison` 的情況下，`netVisual_heatmap()` 會：

* 將 **所有 signaling pathways 的 communication probability 進行加總**
  * 計算兩個 condition 之間的差異（例如 Keloid − Normal Skin）
* 以 sender → receiver 為單位視覺化：

* `measure = "weight"`：communication probability 的總和
* `measure = "count"`：顯著 interaction 的數量

⚠️ **`signaling` 參數在此函式中並未真正參與資料子集化（subsetting）**。

---
  
  ## 為什麼這件事很重要？
  
  若將此 heatmap 解讀為 pathway-specific（例如「SEMA3 訊號在 Keloid 中上升」），可能會導致：

* 將**整體通訊增加**錯誤歸因為單一 pathway
* 產生不正確的生物學結論
* figure title、legend 與實際分析內容不一致

在需要精確 pathway 解讀的比較研究中，這是一個**高風險的誤用情境**。

---
  
  ## 建議實務作法
  
  ### ✅ 若目的是觀察「整體 cell–cell communication 差異」
  
  * `netVisual_heatmap()` 是合適的工具
* **圖標題與說明請避免出現 pathway 名稱**
  
  範例：

> Heatmap of global cell–cell communication differences (Keloid − Normal Skin)

---
  
  ### ❌ 若目的是觀察「特定 pathway 的 heatmap」
  
  * **不建議使用 `netVisual_heatmap()`**
  * 該函式不支援 pathway-level heatmap

可考慮替代方式：

* 使用 `subsetCommunication()` 擷取特定 pathway 的 communication 資料
* 搭配 `netAnalysis_signalingRole_*()` 的輸出
* 自行建立 sender × receiver matrix（僅限指定 pathway）
* 使用 `ggplot2::geom_tile()` 繪製 heatmap

---
  
  ## 重點結論（一句話）
  
  > 在 CellChat 中，`netVisual_heatmap()` 永遠呈現的是**所有 signaling pathways 的總體訊號**；`signaling` 參數不會限制 heatmap 為特定 pathway，因此不可用於 pathway-specific 的解讀。

---
  
  ## 建議引用說明（可選）
  
  若在論文或補充資料中使用此類分析，建議明確說明：

> Heatmaps generated using `netVisual_heatmap()` represent global cell–cell communication differences and are not restricted to individual signaling pathways, regardless of the `signaling` argument.


## Package versions at the time of writing

- R: 4.4.1
- OS: Windows 10 x64 (build 19045)
- CellChat: 2.1.2
- Seurat: 5.3.0
- ggplot2: 3.5.1
- Time zone: Asia/Taipei
