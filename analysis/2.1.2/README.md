# 2.1.2 大学生低碳生活行为预测数据预处理

## 题目大意

读取“大学生低碳生活行为的影响因素数据集”，完成缺失值处理、重复值删除、标准化、特征选择、训练测试集划分，并保存处理后的数据。

---

## 空缺部分答案 + 代码解释

### 空 1：读取 Excel 数据

```python
data = pd.read_excel('大学生低碳生活行为的影响因素数据集.xlsx')
```

**解释**：读取 Excel 文件，把数据存到 `data` 中。

---

### 空 2-4：处理缺失值并记录行数

```python
initial_row_count = data.shape[0]
data = data.dropna()
final_row_count = data.shape[0]
```

**解释**：
- `data.shape[0]`：获取数据集的行数。
- `dropna()`：删除包含缺失值的行。
- 用处理前后的行数相减，得到删除的行数。

---

### 空 5：删除重复行

```python
data = data.drop_duplicates()
```

**解释**：`drop_duplicates()` 删除数据集中完全重复的行。

---

### 空 6：标准化数值特征

```python
data[numerical_features] = scaler.fit_transform(data[numerical_features])
```

**解释**：`fit_transform()` 对 `numerical_features` 中的列做标准化，使其均值为 0、标准差为 1。

---

### 空 7-9：选择特征和目标变量

```python
selected_features = ['1.您的性别○男性  ○女性',
                     '2.您的年级○大一  ○大二  ○大三  ○大四',
                     '3.您的生源地○农村  ○城镇（乡镇）○地县级城市  ○省会城市及直辖市',
                     '4.您的月生活费○≦1,000元   ○1,001-2,000元   ○2,001-3,000元   ○≧3,001元',
                     '5.您进行过绿色低碳的相关生活方式吗？',
                     '6.您觉得“低碳”，与你的生活关系密切吗？',
                     '7.低碳生活是否会成为未来的主流生活方式？',
                     '8.您是否认为低碳生活会提高您的生活质量？']
X = data[selected_features]
y = data['低碳行为积极性']
```

**解释**：
- `selected_features`：题目要求的 8 个特征列，列名要和数据集中完全一致。
- `X`：特征数据。
- `y`：目标变量“低碳行为积极性”。

---

### 空 10：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：
- `train_test_split`：划分训练集和测试集。
- `test_size=0.2`：测试集占 20%，训练集占 80%。

---

### 空 11-12：合并数据并保存

```python
cleaned_data = pd.concat([X, y], axis=1)
cleaned_data.to_csv('2.1.2_cleaned_data.csv', index=False)
```

**解释**：
- `pd.concat([X, y], axis=1)`：把特征 `X` 和目标 `y` 按列方向合并。
- `to_csv(..., index=False)`：保存为 CSV，不保存行索引。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_excel()` | 读取 Excel 文件 |
| `data.shape[0]` | 获取数据行数 |
| `data.dropna()` | 删除包含缺失值的行 |
| `data.drop_duplicates()` | 删除重复行 |
| `scaler.fit_transform()` | 标准化数据 |
| `train_test_split()` | 划分训练集和测试集 |
| `pd.concat()` | 按列或按行合并数据 |
| `to_csv()` | 保存为 CSV 文件 |

---

## 注意事项

1. **Excel 文件要用 `pd.read_excel()`**，不是 `read_csv()`。
2. **`data.shape[0]` 获取行数**，用于记录处理前后的数据量。
3. **先处理缺失值，再处理重复值**，顺序不要颠倒。
4. **特征列名必须和 Excel 中完全一致**，包括标点、空格、全角半角。
5. **目标变量是 `'低碳行为积极性'`**。
6. **合并 `X` 和 `y` 时 `axis=1`**，表示按列合并。
7. **保存 CSV 时写 `index=False`**，避免多出一列索引。

---

## 答题卷：数据清洗和标注规范

### 制定数据清洗规范（每正确回答 1 个规范点得 1 分，最高得 2 分）

1. 数据加载：读取一个 Excel 文件，并将读取到的数据存储在变量中
2. 打印数据：打印出数据集的前 5 行
3. 检查数据缺失值：处理数据集中的缺失值
4. 检查数据重复值并删除：删除重复行
5. 数据标准化处理：确保数据可以在统一量纲下分析

### 制定数据标注规范（每正确回答 1 个规范点得 1 分，最高得 3 分）

1. 数据来源：标注数据来源
2. 数据描述：提供详细的数据描述，包括数据列名称、单位、取值范围等
3. 特征选择：确定目标变量有用特征
4. 数据划分：数据划分（测试集取 20%）
