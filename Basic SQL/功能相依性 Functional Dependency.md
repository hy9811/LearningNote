# 功能相依性 Functional Dependency

# 目錄

---

## 定義

給定一個關聯 R (A1, A2, …, An) 代表一個關聯表，其中 X 和 Y 為 R 關聯表的屬性子集合，
若 R 的屬性子集 Y 相依於 R 的屬性子集 X ， 則：

- 對於關聯 R 中的每個 X 值，均有唯一的 Y 值對應。
- **若且唯若 (if and only if) R 的 X 值可以唯一決定 Y 的值，則 Y 功能相依於 X** ，
意即，若且唯若 (iff) 無論何時， R 的兩 Tuples 若有相同 X 值，則必有相同 Y 值。

以符號記做：

$$
Y \propto X　意思是　Y功能相依於X
$$

$$
X \rightarrow Y　意思是　X 決定 Y　　　
$$

其中 X 為**決定性因素 (Determinant)** 或稱 **Left-hand side，**
　　 Y 為**相依因素 (Dependent)** 或稱 **Right-hand side**

以資料表圖解舉例，若 $Ar功能相依於A1，(A1\rightarrow Ar)$

$R$

| A1 | A2 | … | Ar | … | An |
| --- | --- | --- | --- | --- | --- |
| X | L | … | Y | … | Z |
| K | G |  | C |  | A |
| X | T | … | Y | … | E |
| J | H |  | W |  | P |

由上表可見，當 A1 出現 X 的時候，Ar 必為 Y。

## 特性

- 若在關聯表 R 中 **$X \rightarrow Y$ 成立，並不表示 $Y \rightarrow X$ 成立。**
- 功能相依性是**多對一**的關係。
    - 多個不同決定因素 X 可能決定出一個相依關係，反之則否。
- 關聯表 R 中的 **X 若為主鍵**(候選鍵)，則關聯 R 中所有**其他屬性必功能相依於 X。**

### Trivial 和 Non-Trivial 相依性

- **Trivial**
    - 若功能相依右邊的相依因素，為左邊決定因素的部分集合時，稱此功能性為 Trivial。
    - 舉例：
    ｛供應商代號，產品代號｝$\rightarrow$ 供應商代號
    因為右邊的供應商代號等同左邊的供應商代號，與產品代號無關，所以此相依性為 Trivial。
- **Non-Trivial**
    - 若功能相依右邊的相依因素，為左邊決定因素的部分集合時，稱此功能性為 Non-Trivial。
    - 舉例：
    ｛供應商代號，產品代號｝$\rightarrow$ 單價
    因為右邊的單價會依照左邊供應商代號與產品代號而不同，
    表示單價受到供應商代號與產品代號決定，所以此相依性為 Non-Trivial。

**Non-Trivial 的功能相依性，在正規化時才有意義**。

---

## 功能相依性的種類

### 完全功能相依 (Full Functional Dependency)

若主鍵是由多個屬性組成，且某非鍵值屬性依賴主鍵之全部而非部分時，
則稱該非鍵值屬性「完全相依」於主鍵。

### 部分功能相依 (Partial Functional Dependency)

若主鍵是由多個屬性組成，而某非鍵值屬性是依賴主鍵之部分屬性時，
則稱該非鍵值屬性「部份相依」於主鍵。

### 遞移相依 (Transitive Dependency)

若存在一個非鍵值屬性 $T$，使得 $I \rightarrow T$ 且 $T \rightarrow J$ 的功能相依性均成立，
則稱屬性 $I$ 與屬性 $J$ 之間具有遞移相依。

## 鍵值屬性 (Key Attribute)

- 能構成主鍵或候選鍵的所有屬性。反之，則稱為非鍵值屬性(NonKey Attribute)

> 參照 [關聯鍵](關聯鍵.md) 筆記
> 

## 最簡功能相依性 (Irreducible Functional Dependence)

**特性**

- 相依因素僅有一個屬性
- 沒有多餘的決定因素
- 沒有多餘的功能相依性

記憶方式：**右邊最簡，左邊最簡，沒有多餘FD**

利用最簡功能相依性特性，透過**阿姆斯壯定理**可以正確的找出候選鍵。

## 功能相依性的推導 - 阿姆斯壯定理 (Armstrong's Axioms)

**目的 ：** 利用已知的功能相依性，透過一些性質，推導出以下可能項目：

- 其它**隱含 (Implicit) 的功能相依性**。
- 一個關聯中的**候選鍵或主鍵**。
- **最簡功能相依性 (Irreducible Functional Dependence)**。

**說明：**

假設 $A, B, C, D$ 為關聯 $R$ 的四個任意屬性子集，且 $AB$ 表示 $A$ 聯集 $B$ ，以此類推。

定理如下：

1. 反身性(Reflexivity)：若 $B$ 是 $A$ 的子集合，則 $A \rightarrow B$
2. 擴張性(Augmentation)：若 $A \rightarrow B$，則 $AC \rightarrow BC$
3. **遞移性(Transitivity)**：若 $A \rightarrow B$ 且 $B \rightarrow C$，則 $A \rightarrow C$
4. 自身決定性(Self-determination)：$A \rightarrow A$
5. **分解性(Decomposition)**：若 $A \rightarrow BC$，則 $A \rightarrow B$ 且 $A \rightarrow C$
6. 聯集性(Union)：若 $A \rightarrow B$ 且 $A \rightarrow C$ ，則 $A \rightarrow BC$
7. **合成性(Composition)**：若 $A \rightarrow B$ 且 $C \rightarrow D$，則 $AC \rightarrow BD$
8. **虛擬遞移性**(Pseudo-transitivity): 若 $A \rightarrow B$ 且 $BC \rightarrow D$ ，則 $AC \rightarrow D$

**舉例：**
某一關聯 $R = \{ A, B, C, D \}$ ，且有以下功能相依：
$\{ C \rightarrow BD,　B \rightarrow D,　C \rightarrow B, 　BC \rightarrow D,　CD \rightarrow A \}$
請找出**最簡功能相依性**，並決定 $R$ 的**鍵值為何**。

解答：

列出最簡功能相依性條件

1. 相依因素僅有一個屬性 (右邊最簡)

觀察上列功能相依是否有一個以上屬性的相依因素，
意思就是檢查有無一個以上的右邊屬性被左邊屬性決定。

根據條件發現
$\{ C \rightarrow BD \}$ 存在一個以上屬性的相依因素，可用阿姆斯壯定理的**分解性**分解，
得到 $\{ C \rightarrow B,　C \rightarrow D \}$。

而整體為 $\{ C \rightarrow B,　C \rightarrow D,　B \rightarrow D,　C \rightarrow B, 　BC \rightarrow D,　CD \rightarrow A \}$
發現 $\{ \textcolor{red}{C \rightarrow B},　C \rightarrow D,　B \rightarrow D,　\textcolor{red}{C \rightarrow B}, 　BC \rightarrow D,　CD \rightarrow A \}$

$\{ C \rightarrow B \}$ 重複，保留一個即可，得到：
$\{ C \rightarrow B,　C \rightarrow D,　B \rightarrow D, 　BC \rightarrow D,　CD \rightarrow A \}$
再次檢查，確認無任何一個以上屬性的相依因素。
 
2. 沒有多餘的決定因素 (左邊最簡)

觀察上列功能相依， $\{ BC \rightarrow D,　CD \rightarrow A \}$ 各有一個以上的決定因素，
嘗試化簡$\{ C \rightarrow B,　C \rightarrow D,　B \rightarrow D, 　BC \rightarrow D,　CD \rightarrow A \}$，

$\because \{ C \rightarrow D, 　CD \rightarrow A \}$ 呈現**虛擬遞移性**，可使 $\{ CD \rightarrow A \}$ 簡化成 **$\{ C \rightarrow A \}$
$\therefore \{ C \rightarrow B,　C \rightarrow D,　B \rightarrow D, 　BC \rightarrow D,　C \rightarrow A \}$

$\because \{ C \rightarrow B, 　BC \rightarrow D \}$** 呈現**虛擬遞移性**，可使 $\{ BC \rightarrow D \}$ 簡化成 $\{ C \rightarrow D \}$
$\therefore \{ C \rightarrow B,　C \rightarrow D,　B \rightarrow D, 　C \rightarrow D,　C \rightarrow A \}$

又因為 $\{ C \rightarrow D\}$ 重複，保留一個即可，得到：
$\therefore \{ C \rightarrow B,　C \rightarrow D,　B \rightarrow D,　C \rightarrow A \}$

3. 沒有多餘的相依因素 (沒有多餘FD)

觀察上列功能相依，$\{ \textcolor{red}{C \rightarrow B},　C \rightarrow D,　\textcolor{red}{B \rightarrow D},　C \rightarrow A \}$
$\because$ 由**遞移相依性**得知，  $\{ C \rightarrow D\}$ 為 $\{ C \rightarrow B,　B \rightarrow D \}$ 組成

為了保持資料**無損分解**，因此此處保留此遞移相依性，選擇展開的方式移除 $\{ C \rightarrow D\}$
$\therefore \{ C \rightarrow B,　B \rightarrow D,　C \rightarrow A \}$ 為**最簡功能相依性**

由上列最簡功能相依性的結果得知， $C$ 能**直接或間接決定其他所有屬性**，所以 $C$ 為**鍵值**。

> 參照 [無損分解](無損分解.md) 筆記
> 

## 結論

本文闡述了功能相依性的定義、特性、種類和推導。

### **重點**

- 特性方面：
功能相依性是多對一的關係，且若關聯表R中X為主鍵(候選鍵)，
則R中所有其他屬性必須功能相依於X。
- 種類方面：
功能相依性分為完全功能相依、部分功能相依和遞移相依。
- 推導方面：
可以利用阿姆斯壯定理推導出隱含的功能相依性、候選鍵或主鍵以及最簡功能相依性。
- 最簡化功能相依性方面：
相依因素僅有一個屬性、沒有多餘的決定因素、沒有多餘的功能相依性。
記憶方式：右邊簡化，左邊簡化，沒有FD(多餘相依)
- 推導方法 - [**阿姆斯壯定理**](功能相依性%20Functional%20Dependency.md)
反身性、擴張性、**遞移性**、自身決定性、分解性、聯集性、**合成性**、**虛擬遞移性**

### 參考資料

[國立聯合大學 資訊管理學系 資料庫系統課程簡報 陳士傑](http://debussy.im.nuu.edu.tw/sjchen/Database/Final/Ch05.pdf)

tags:  `SQL 基本常用` `基本功`