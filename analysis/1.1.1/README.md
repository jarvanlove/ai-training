# 1.1.1 患者风险等级与分组统计

## 题目大意

读取患者数据，根据住院天数判断风险等级，再分别按 BMI 区间和年龄区间统计高风险患者的比例和人数。

---

## 空缺部分答案 + 代码解释

### 空 1：读取数据

```python
data = pd.read_csv('patient_data.csv')
```

**解释**：用 pandas 读取 `patient_data.csv` 文件，把表格数据存到 `data` 里。

---

### 空 2-4：创建风险等级列

```python
data['RiskLevel'] = np.where(data['DaysInHospital'] > 7, '高风险患者', '低风险患者')
```

**解释**：
- `np.where(条件, A, B)`：如果条件成立填 A，不成立填 B。
- 条件是 `data['DaysInHospital'] > 7`，即住院天数大于 7。
- 结果保存到新列 `data['RiskLevel']` 中。

---

### 空 5-6：统计风险等级数量

```python
risk_counts = data['RiskLevel'].value_counts()
```

**解释**：
- `data['RiskLevel']`：取出风险等级这一列。
- `.value_counts()`：统计这一列里每个值出现的次数，即高风险、低风险各有多少人。

---

### 空 7-8：计算占比

```python
high_risk_ratio = risk_counts['高风险患者'] / len(data)
low_risk_ratio = risk_counts['低风险患者'] / len(data)
```

**解释**：
- `risk_counts['高风险患者']`：高风险患者数量。
- `len(data)`：数据总行数，即患者总人数。
- 相除得到高风险患者占比，低风险同理。

---

### 空 9-12：按 BMI 划分区间

```python
data['BMIRange'] = pd.cut(data['BMI'], bins=bmi_bins, labels=bmi_labels, right=False)
```

**解释**：
- `pd.cut()`：把连续数值切成几个区间。
- `data['BMI']`：要切分的列。
- `bins=bmi_bins`：区间边界。
- `labels=bmi_labels`：区间标签。
- `right=False`：左闭右开。
- 结果保存到新列 `data['BMIRange']`。

---

### 空 13-14：计算每个 BMI 区间高风险比例

```python
bmi_risk_rate = data.groupby('BMIRange')['RiskLevel'].apply(lambda x: (x == '高风险患者').mean())
```

**解释**：
- `data.groupby('BMIRange')`：按 BMI 区间分组。
- `['RiskLevel']`：只看风险等级这一列。
- `.apply(lambda x: (x == '高风险患者').mean())`：对每组计算高风险患者比例。
  - `x == '高风险患者'` 得到 True/False。
  - `.mean()` 对 True/False 求平均，True 算 1，False 算 0，结果就是比例。

---

### 空 15-16：统计每个 BMI 区间人数

```python
bmi_patient_count = data['BMIRange'].value_counts()
```

**解释**：统计 `BMIRange` 列每个区间出现的次数，即每个 BMI 区间的患者数量。

---

### 空 17-20：按年龄划分区间

```python
data['AgeRange'] = pd.cut(data['Age'], bins=age_bins, labels=age_labels, right=False)
```

**解释**：和 BMI 划分完全一样，只是把 `BMI` 换成 `Age`。

---

### 空 21-22：计算每个年龄区间高风险比例

```python
age_risk_rate = data.groupby('AgeRange')['RiskLevel'].apply(lambda x: (x == '高风险患者').mean())
```

**解释**：和 BMI 高风险比例计算完全一样，分组列换成 `AgeRange`。

---

### 空 23-24：统计每个年龄区间人数

```python
age_patient_count = data['AgeRange'].value_counts()
```

**解释**：统计 `AgeRange` 列每个区间出现的次数，即每个年龄区间的患者数量。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `np.where()` | 按条件填值 |
| `value_counts()` | 统计各类别数量 |
| `len()` | 求数据行数（总人数） |
| `pd.cut()` | 把连续数值切成区间 |
| `groupby()` | 按某列分组 |

---

## 注意事项

1. **`np.where` 三要素**：条件、条件为真填什么、条件为假填什么。
2. **住院天数判断是 `> 7`**，不是 `>=` 7。
3. **`pd.cut` 的参数顺序**：要切的列 → `bins` → `labels` → `right`。
4. **`right=False` 表示左闭右开**，即包含左边界，不包含右边界。
5. **占比的分母是 `len(data)`**，即总人数。
6. **`value_counts()` 别忘了括号**。
7. **列名大小写不要写错**，如 `DaysInHospital`、`RiskLevel`、`BMIRange`、`AgeRange`。
8. **`groupby + apply(lambda x: (x == '高风险患者').mean())`** 是分组求比例的经典套路，理解即可。
