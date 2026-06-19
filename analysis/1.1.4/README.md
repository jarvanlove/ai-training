# 1.1.4 用户行为数据采集与处理

## 题目大意

读取电商用户行为数据，完成采集、清洗（缺失值、类型转换、异常值、标准化），最后统计购买类别、性别平均购买金额和年龄段分布。

---

## 空缺部分答案 + 代码解释

### 空 1：读取数据

```python
data = pandas.read_csv('user_behavior_data.csv')
```

**解释**：用 `pandas`（注意本题没有简写成 `pd`）读取 CSV 文件。

---

### 空 2：打印前 5 条

```python
print(data.head())
```

**解释**：`head()` 默认显示前 5 行数据。

---

### 空 3：删除缺失值

```python
data = data.dropna()
```

**解释**：`dropna()` 删除包含缺失值的行。

---

### 空 4-5：Age 转 int

```python
data['Age'] = data['Age'].astype(int)
```

**解释**：`astype(int)` 把 `Age` 列的数据类型转换为整数。

---

### 空 6-7：PurchaseAmount 转 float

```python
data['PurchaseAmount'] = data['PurchaseAmount'].astype(float)
```

**解释**：把 `PurchaseAmount` 列转换为浮点数。

---

### 空 8-9：ReviewScore 转 int

```python
data['ReviewScore'] = data['ReviewScore'].astype(int)
```

**解释**：把 `ReviewScore` 列转换为整数。

---

### 空 10-11：年龄异常值处理

```python
data = data[(data['Age'].between(18, 70)) & 
            (data['PurchaseAmount'] > 0) & 
            (data['ReviewScore'].between(1, 5))]
```

**解释**：保留年龄在 18-70 岁、购买金额大于 0、评分在 1-5 之间的数据。

---

### 空 12-13：评分异常值处理

```python
data = data[(data['Age'].between(18, 70)) & 
            (data['PurchaseAmount'] > 0) & 
            (data['ReviewScore'].between(1, 5))]
```

**解释**：和上一行一起，`ReviewScore.between(1, 5)` 表示评分在 1 到 5 之间。

---

### 空 14-15：PurchaseAmount 标准化

```python
data['PurchaseAmount'] = (data['PurchaseAmount'] - data['PurchaseAmount'].mean()) / data['PurchaseAmount'].std()
```

**解释**：标准差标准化，公式是 `(数值 - 平均值) / 标准差`。

---

### 空 16-17：ReviewScore 标准化

```python
data['ReviewScore'] = (data['ReviewScore'] - data['ReviewScore'].mean()) / data['ReviewScore'].std()
```

**解释**：和 PurchaseAmount 标准化一样。

---

### 空 18-19：保存清洗后的数据

```python
data.to_csv('cleaned_user_behavior_data.csv', index=False)
```

**解释**：保存为 CSV，`index=False` 不保存行序号。

---

### 空 20-21：统计每个购买类别的用户数

```python
purchase_category_counts = data['PurchaseCategory'].value_counts()
```

**解释**：统计 `PurchaseCategory` 列每个类别的用户数量。

---

### 空 22-23：统计不同性别的平均购买金额

```python
gender_purchase_amount_mean = data.groupby('Gender')['PurchaseAmount'].mean()
```

**解释**：按性别分组，计算每组的平均购买金额。

---

### 空 24-27：按年龄分组

```python
data['AgeGroup'] = pandas.cut(data['Age'], bins=bins, labels=labels, right=False)
```

**解释**：用 `pandas.cut` 把年龄切成多个区间，`bins` 是边界，`labels` 是标签，`right=False` 左闭右开。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pandas.read_csv()` | 读取 CSV 文件 |
| `head()` | 查看前 5 行 |
| `dropna()` | 删除缺失值 |
| `astype()` | 转换数据类型 |
| `between()` | 判断数值是否在范围内 |
| `mean()` | 求平均值 |
| `std()` | 求标准差 |
| `to_csv()` | 保存为 CSV |
| `value_counts()` | 统计各类别数量 |
| `groupby()` | 按某列分组 |
| `cut()` | 把连续数值切成区间 |

---

## 注意事项

1. **本题导入的是 `import pandas`**，不是 `import pandas as pd`，所以要用 `pandas.read_csv()` 和 `pandas.cut()`。
2. **`head()` 默认前 5 行**，不需要传参数。
3. **`dropna()` 删除包含缺失值的整行**。
4. **`astype(int)` / `astype(float)`**：数据类型转换，注意括号里是类型名，不是字符串。
5. **异常值条件用 `&` 连接**，三个条件要同时满足。
6. **标准化公式**：`(x - mean) / std`，即减去均值再除以标准差。
7. **保存文件用 `data.to_csv(...)`**，注意是清洗后的 `data`，不是 `cleaned_data`。
8. **按性别分组求平均**：`data.groupby('Gender')['PurchaseAmount'].mean()`。
9. **年龄分组**：`pandas.cut(data['Age'], bins=bins, labels=labels, right=False)`，参数顺序和 1.1.1 的 `pd.cut` 一致。
10. **`right=False` 表示左闭右开**，例如 `[18, 26)` 表示包含 18，不包含 26。
