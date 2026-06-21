# 2.1.1 汽车燃油效率预测数据预处理

## 题目大意

读取汽车燃油效率数据集，完成加载、缺失值处理、horsepower 异常值处理、标准化、特征选择、训练测试集划分，并保存处理后的数据。

---

## 空缺部分答案 + 代码解释

### 空 1：读取数据

```python
data = pd.read_csv('auto-mpg.csv')
```

**解释**：读取 `auto-mpg.csv` 文件，把汽车数据存到 `data` 中。

---

### 空 2：显示前五行

```python
print(data.head())
```

**解释**：`head()` 默认显示 DataFrame 的前 5 行，用于快速查看数据样例。

---

### 空 3-4：检查缺失值并删除

```python
print(data.isnull().sum())
data = data.dropna()
```

**解释**：
- `isnull().sum()`：统计每一列的缺失值数量。
- `dropna()`：删除包含任何缺失值的整行。

---

### 空 5-6：转换 horsepower 并处理异常值

```python
data['horsepower'] = pd.to_numeric(data['horsepower'], errors='coerce')
data = data.dropna(subset=['horsepower'])
```

**解释**：
- `pd.to_numeric(..., errors='coerce')`：把 `horsepower` 转成数字，无法转换的值（如 `?`）变成 NaN。
- `dropna(subset=['horsepower'])`：只删除 `horsepower` 为 NaN 的行。

---

### 空 7：标准化数值特征

```python
data[numerical_features] = scaler.fit_transform(data[numerical_features])
```

**解释**：`fit_transform()` 先计算均值和标准差，再对 `displacement`、`horsepower`、`weight`、`acceleration` 四列做标准化。

---

### 空 8-10：选择特征和目标变量

```python
selected_features = ['cylinders','displacement','horsepower','weight','acceleration','model year','origin']
X = data[selected_features]
y = data['mpg']
```

**解释**：
- `selected_features`：题目要求的 7 个特征。
- `X`：特征数据（自变量）。
- `y`：目标变量 `mpg`（每加仑英里数，燃油效率）。

---

### 空 11：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：
- `train_test_split`：把数据分成训练集和测试集。
- `test_size=0.2`：测试集占 20%，训练集占 80%。
- `random_state=42`：固定随机种子，结果可复现。

---

### 空 12：保存处理后的数据

```python
cleaned_data.to_csv('2.1.1_cleaned_data.csv', index=False)
```

**解释**：`to_csv()` 把 DataFrame 保存为 CSV 文件；`index=False` 不保存行序号。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `data.head()` | 查看前 5 行 |
| `data.isnull().sum()` | 统计每列缺失值数量 |
| `data.dropna()` | 删除包含缺失值的行 |
| `pd.to_numeric()` | 转换为数值类型 |
| `data.dropna(subset=[...])` | 删除指定列含缺失值的行 |
| `scaler.fit_transform()` | 标准化数据 |
| `train_test_split()` | 划分训练集和测试集 |
| `to_csv()` | 保存为 CSV 文件 |

---

## 注意事项

1. **`horsepower` 列可能包含非数字字符**（如 `?`），需先用 `pd.to_numeric(..., errors='coerce')` 转成 NaN，再删除。
2. **`dropna()` 和 `dropna(subset=[...])` 区别**：前者删所有含空值的行，后者只删指定列为空的行。
3. **标准化只对数值型特征进行**，不要把类别型特征（如 `origin`）放进去。
4. **训练集占 8 成**，对应 `test_size=0.2`。
5. **保存 CSV 时一定要写 `index=False`**，否则会出现多余的 `Unnamed: 0` 列。
6. **特征列表必须包含题目要求的 7 个列**，顺序建议与题目一致。
7. **`random_state=42` 固定随机种子**，保证每次运行划分结果一致。

---

## 答题卷：数据清洗和标注规范

### 制定数据清洗规范（每正确回答 1 个规范点得 1 分，最高得 2 分）

1. 数据加载：加载数据集并显示数据集的前五行
2. 检查缺失值：检查缺失值并删除缺失值所在的行
3. 转换与处理异常值：将 'horsopower' 列转换为数值类型，并（删除）处理转换中的异常值
4. 数据标准化：对数值型数据进行标准化处理

### 制定数据标注规范（每正确回答 1 个规范点得 1 分，最高得 3 分）

1. 数据来源：选择特征、自变量和目标变量
2. 数据描述：
3. 特征选择：将特征和目标变量合并到一个数据框中
4. 目标变量设定：
5. 数据划分：划分数据集为训练集和测试集
