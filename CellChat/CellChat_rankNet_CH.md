# CellChat `rankNet()` 與 Paired Wilcoxon Test：重點整理

本文件整理在使用 **CellChat `rankNet(..., mode = "comparison")`** 時，關於 *information flow*、*relative information flow* 與 *paired Wilcoxon test* 的重要注意事項，避免常見誤解。

---

## 1. Information flow 是什麼？

* **定義**：

  * 某一 signaling pathway 在一個 condition 中，
  * 所有 sender → receiver cell group 對的 **communication probability 總和**。

* 在 `rankNet()` 中對應欄位：

  * `contribution` → **raw information flow**
  * `contribution.scaled` → **scaled information flow（僅供視覺化 / 排序）**

⚠️ `contribution.scaled` **不是線性縮放**，不同 condition 會有不同 scaling factor，**不可直接解讀為倍數變化**。

---

## 2. Relative information flow（stacked 圖）在算什麼？

在 `stacked = TRUE` 時：

* 每一條 pathway 會被拆成兩段（兩個 condition）
* **兩段加起來一定等於 1（x 軸固定 0–1）**

實際計算方式（以 raw 為基礎）：

```
Rel_A = raw_A / (raw_A + raw_B)
Rel_B = raw_B / (raw_A + raw_B)
```

### 重要行為說明

* **不論 `show.raw = TRUE / FALSE`**
* `rankNet(..., stacked = TRUE)` **永遠用 raw (`contribution`) 來算比例**
* CellChat **沒有提供用 `contribution.scaled` 計算 stacked 比例的選項**

👉 因此：

* `gg1_raw` 與 `gg1_sc` 的 stacked 圖看起來會一模一樣

---

## 3. 為什麼會出現「左圖近 1:1，但右圖像 3:1」？

這代表：

* **raw information flow 幾乎相同**（≈ 1:1）
* 但 **scaled information flow 因 scaling factor 不同而被放大**

結論：

> 這不是計算錯誤，而是 **raw 與 scaled 混用所造成的視覺落差**。

---

## 4. Paired Wilcoxon test 在 CellChat 裡「配對」的是什麼？

### 配對單位不是 pathway，而是：

> **同一條 pathway × 同一對 sender–receiver cell groups**

對每一條 pathway（如 NOTCH）：

1. 找出在兩個 condition 中都存在的 sender–receiver cell pair
2. 比較該 cell pair 在兩個 condition 的 communication probability
3. 對所有 cell pair 的差值進行：

```r
wilcox.test(x, y, paired = TRUE)
```

---

## 5. 為什麼很多 pathway 會顯示為黑色？

在 `do.stat = TRUE` 時，字體顏色代表：

* **非黑色**：

  * 偏向某一 condition，且
  * paired Wilcoxon test 顯著（p < 0.05），或
  * 相對比值超出容忍區間（`tol`）

* **黑色文字**：

  * 兩組差異接近，或
  * paired Wilcoxon test **不顯著**

⚠️ 即使 total / scaled flow 很大，只要不同 cell-pair 的變化方向不一致，就很容易不顯著。

---

## 6. 實務建議（強烈建議遵守）

### ✅ 圖形解讀

* stacked（relative）圖：

  * 解讀 **raw 的相對分配**
* non-stacked 圖：

  * 明確說明使用的是 raw 還是 scaled

### ✅ 論文撰寫

* **不要**把 `contribution.scaled` 當作倍數變化解讀
* 統計檢定請明確描述為：

> Paired Wilcoxon signed-rank test comparing matched sender–receiver cell pairs between conditions.

---

## 7. 若需要 scaled-based relative information

CellChat **無內建支援**，需自行計算：

```
scaled_rel_A = scaled_A / (scaled_A + scaled_B)
```

再自行使用 `ggplot2` 繪製 stacked bar。

---

## 8. 一句話總結

> 在 CellChat 中，relative information flow 永遠基於 raw information 計算；
> scaled values 僅用於視覺化與排序，不能直接與 relative 或統計檢定混用解讀。
