# 2.1.3 金融信用评分数据预处理

## 题目大意

读取 Finance 数据集，通过箱线图和 IQR 方法处理异常值，删除重复值，进行归一化，选择特征并划分训练测试集，最后保存清洗后的数据。

---

## 空缺部分答案 + 代码解释

### 空 1-2：加载数据并显示前五行

```python
data = pd.read_csv('finance数据集.csv')
print(data.head())
```

**解释**：
- `pd.read_csv()`：读取 CSV 文件。
- `data.head()`：显示数据集前 5 行。

---

### 空 3-5：使用 IQR 计算四分位数和四分位距

```python
Q1 = data[numeric_cols].quantile(0.25)
Q3 = data[numeric_cols].quantile(0.75)
IQR = Q3 - Q1
```

**解释**：
- `quantile(0.25)`：计算第 1 四分位数（Q1）。
- `quantile(0.75)`：计算第 3 四分位数（Q3）。
- `IQR = Q3 - Q1`：四分位距，代表中间 50% 数据的范围。

---

### 空 6：移除异常值

```python
data_cleaned = data[~((data[numeric_cols] < (Q1 - 1.5 * IQR)) | (data[numeric_cols] > (Q3 + 1.5 * IQR))).any(axis=1)]
```

**解释**：
- `Q1 - 1.5 * IQR` 和 `Q3 + 1.5 * IQR` 是异常值判断的上下边界。
- 小于下界或大于上界的值视为异常值。
- `~` 表示取反，保留不在异常范围内的行。
- `.any(axis=1)`：只要某一行中任意一列是异常值，就判断为异常行。

---

### 空 7：检查并处理重复值

```python
duplicates = data_cleaned.duplicated()
```

**解释**：`duplicated()` 返回每一行是否重复的布尔序列，True 表示该行是重复行。

---

### 空 8：归一化处理

```python
data_cleaned[numeric_cols] = scaler.fit_transform(data_cleaned[numeric_cols])
```

**解释**：`MinMaxScaler` 的 `fit_transform()` 把数值缩放到 0~1 之间。

---

### 空 9：设定目标变量

```python
target_variable = 'SeriousDlqin2yrs'
```

**解释**：`SeriousDlqin2yrs` 是目标变量，表示是否出现严重逾期。

---

### 空 10-12：定义特征 X 和目标 y

```python
X = data_cleaned.drop(columns=[target_variable])
y = data_cleaned[target_variable]
```

**解释**：
- `drop(columns=[target_variable])`：从数据集中删除目标变量列，剩下的作为特征 `X`。
- `data_cleaned[target_variable]`：取出目标变量列作为 `y`。

---

### 空 13：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：
- `train_test_split`：划分训练集和测试集。
- `test_size=0.2`：测试集占 20%，训练集占 80%。

---

### 空 14-15：保存清洗后的数据

```python
data_cleaned.to_csv(cleaned_file_path, index=False)
```

**解释**：`to_csv()` 保存为 CSV 文件；`index=False` 不保存行索引。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `data.head()` | 查看前 5 行 |
| `data.quantile()` | 计算分位数 |
| `data.duplicated()` | 判断重复行 |
| `scaler.fit_transform()` | 归一化/标准化数据 |
| `data.drop()` | 删除指定列或行 |
| `train_test_split()` | 划分训练集和测试集 |
| `to_csv()` | 保存为 CSV 文件 |

---

## 注意事项

1. **文件名是 `finance数据集.csv`**，注意是小写 `finance`。
2. **IQR 异常值判断公式**：下界 `Q1 - 1.5 * IQR`，上界 `Q3 + 1.5 * IQR`。
3. **异常值过滤要用 `.any(axis=1)`**，表示只要一行中任一列为异常就删除该行。
4. **`duplicated()` 返回布尔序列**，用 `~duplicates` 取反可保留非重复行。
5. **本题使用 `MinMaxScaler` 做归一化**，不是 `StandardScaler` 标准化。
6. **目标变量是 `'SeriousDlqin2yrs'`**，拼写不要错。
7. **`X` 用 `drop()` 删除目标列得到**，不是手动列举所有特征列。
8. **保存 CSV 时写 `index=False`**，避免多出一列索引。

---

## 答题卷：数据清洗和特征工程规范

### 制定数据清洗规范

1. 数据加载：使用 pandas 库加载数据集，检查数据的基本结构和类型。
2. 检查数据集中的异常值并处理异常值，使用箱线图检测异常值，使用 IQR 方法处理异常值；
3. 检查数据集中的重复值并删除所有重复值，并记录删除的行数；
4. 对数据进行归一化处理；

### 制定特征工程规范

（1）创建新的特征 IncomeToDebtRatio，Monthly Income，并添加到数据集中；
（2）将 SeriousDlqin2yrs 设为目标变量并标注；
（3）对数据进行划分；
（4）保存处理后的数据，将经过清洗和处理后的数据保存为新的 CSV 文件。
