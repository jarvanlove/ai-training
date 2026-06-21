# 2.1.5 健康咨询客户数据预处理

## 题目大意

读取健康咨询客户数据集，完成缺失值处理、年龄类型转换、去重、健身水平标签编码、健身频率饼图绘制、训练测试集划分，并保存处理后的数据。

---

## 空缺部分答案 + 代码解释

### 空 1-3：加载数据并查看基本信息

```python
data = pd.read_csv('健康咨询客户数据集.csv')
print(data.info())
print(data.isnull().sum())
```

**解释**：
- `pd.read_csv()`：读取 CSV 文件。
- `data.info()`：查看数据框结构、列名、非空值数量和数据类型。
- `isnull().sum()`：统计每列缺失值数量。

---

### 空 4：删除缺失值

```python
data_cleaned = data.dropna()
```

**解释**：`dropna()` 删除包含任何缺失值的整行。

---

### 空 5-6：转换年龄为整数类型

```python
data_cleaned.loc[:, 'Your age'] = pd.to_numeric(data_cleaned['Your age'], errors='coerce')
data_cleaned.loc[:, 'Your age'] = data_cleaned['Your age'].astype(int)
```

**解释**：
- `pd.to_numeric(..., errors='coerce')`：先把年龄列转成数字，异常值变为 NaN。
- `astype(int)`：把年龄列转换为整数类型。

---

### 空 7：删除重复值

```python
data_cleaned = data_cleaned.drop_duplicates()
```

**解释**：`drop_duplicates()` 删除重复行。

---

### 空 8：对健身水平列进行标签编码

```python
data_cleaned['How do you describe your current level of fitness ?'] = label_encoder.fit_transform(data_cleaned['How do you describe your current level of fitness ?'])
```

**解释**：`LabelEncoder` 的 `fit_transform()` 把文本类别转成数字编码（如 0, 1, 2...）。

---

### 空 9：绘制健身频率饼图

```python
exercise_frequency_counts.plot.pie(autopct='%1.1f%%', startangle=90, colors=plt.cm.Paired.colors)
```

**解释**：`plot.pie()` 绘制饼图；`autopct='%1.1f%%'` 显示百分比；`startangle=90` 从顶部开始绘制。

---

### 空 10：划分训练集和测试集

```python
train_data, test_data = train_test_split(data_filled, test_size=0.2, random_state=42)
```

**解释**：
- `train_test_split`：划分数据集。
- `test_size=0.2`：测试集占 20%，训练集占 80%。

---

### 空 11-12：保存处理后的数据

```python
cleaned_file_path = '2.1.5_cleaned_data.csv'
data_filled.to_csv(cleaned_file_path, index=False)
```

**解释**：
- `cleaned_file_path`：保存文件的路径和名称。
- `to_csv(..., index=False)`：保存为 CSV，不保存行索引。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `data.info()` | 查看数据框基本信息 |
| `data.isnull().sum()` | 统计每列缺失值数量 |
| `data.dropna()` | 删除包含缺失值的行 |
| `pd.to_numeric()` | 转换为数值类型 |
| `data.astype()` | 转换数据类型 |
| `data.drop_duplicates()` | 删除重复行 |
| `label_encoder.fit_transform()` | 标签编码 |
| `Series.plot.pie()` | 绘制饼图 |
| `train_test_split()` | 划分训练集和测试集 |
| `to_csv()` | 保存为 CSV 文件 |

---

## 注意事项

1. **年龄列 `'Your age'` 要先转数字再转整数**，不能直接用 `astype(int)`，否则异常值会报错。
2. **`pd.to_numeric(..., errors='coerce')`** 把无法转换的值变成 NaN，之后用 `dropna()` 删除。
3. **`drop_duplicates()` 删除重复行**，注意和 `duplicated()`（只判断）区分。
4. **LabelEncoder 是标签编码**，把文本类别映射为整数，不是数值归一化。
5. **饼图用 `plot.pie()`**，不是 `plt.pie()`。
6. **划分数据集时是对 `data_filled` 整体划分**，不是只划分特征 X 和目标 y。
7. **保存 CSV 时写 `index=False`**，避免多出一列索引。

---

## 答题卷：数据清洗和标注规范

### 制定数据清洗规范（每正确回答 1 个规范点得 1 分，最高得 2 分）

1. 加载数据集：查看表的数据类型，表结构和显示每一列的空缺值数量；
2. 缺失值处理：对于含有缺失值的行进行删除操作；
3. 数据类型转换：将“Your age”列的数据类型转换为整数类型，并处理其中的异常值；
4. 数据去重：检查数据集中的重复值并删除所有重复值，并记录删除的行数；
5. 数据归一化处理：对“如何形容你的当前健身水平？”（How do you describe your current level of fitness ?）列中的数据进行归一化处理；

### 制定数据标注规范（每正确回答 1 个规范点得 1 分，最高得 3 分）

1. 数据来源：标注数据的来源，包括数据集的名称、获取日期和数据提供者。
2. 数据描述：提供详细的数据描述，包括每列数据的含义、单位和可能的取值范围。
3. 特征选择：确定对目标变量预测最有用的特征。
4. 目标变量设定：根据业务需求和数据特性，选择最有用的特征。
5. 数据划分：将数据分为训练集和测试集，通常采用 80/20 的比例，以便于模型的训练和评估。
6. 保存处理后的数据：保存处理后的数据，并记录保存文件的路径和文件名。
7. 数据清洗和标注规范文档
