# 2.1.4 医疗研究数据预处理与可视化

## 题目大意

读取医疗数据集，完成日期格式规范、列名修改、新增诊断延迟和病程列、异常值删除、重复值删除、归一化，并绘制治疗结果分布柱状图和年龄与疾病严重程度散点图，最后保存数据。

---

## 空缺部分答案 + 代码解释

### 空 1-2：加载数据并查看结构

```python
data = pd.read_csv('medical_data.csv', encoding='gbk')
print(data.info())
```

**解释**：
- `pd.read_csv(..., encoding='gbk')`：以 GBK 编码读取 CSV，避免中文乱码。
- `data.info()`：查看数据框的基本信息，包括列名、非空值数量、数据类型等。

---

### 空 3：修改列名

```python
data.rename(columns={'病人ID': '患者ID'}, inplace=True)
```

**解释**：`rename()` 用于修改列名；`inplace=True` 直接修改原数据框。

---

### 空 4：增加诊断延迟列

```python
data['诊断延迟'] = (data['诊断日期'] - data['就诊日期']).dt.days
```

**解释**：用诊断日期减去就诊日期，`.dt.days` 提取相差的天数。

---

### 空 5：删除不合理数据

```python
data = data[(data['诊断延迟'] >= 0) & (data['年龄'] > 0) & (data['年龄'] < 120)]
```

**解释**：
- `诊断延迟 >= 0`：诊断不能早于就诊。
- `年龄 > 0` 且 `年龄 < 120`：排除不合理的年龄。

---

### 空 6：删除重复值

```python
data.drop_duplicates(inplace=True)
```

**解释**：`drop_duplicates()` 删除重复行；`inplace=True` 直接修改原数据框。

---

### 空 7-8：归一化年龄、体重、身高

```python
columns_to_normalize = ['年龄', '体重', '身高']
data[columns_to_normalize] = scaler.fit_transform(data[columns_to_normalize])
```

**解释**：对 `年龄`、`体重`、`身高` 三列做 Min-Max 归一化，缩放到 0~1 之间。

---

### 空 9：绘制柱状图

```python
treatment_outcome_distribution.plot(kind='bar', stacked=True)
```

**解释**：`plot(kind='bar', stacked=True)` 绘制堆叠柱状图，展示不同疾病类型下治疗结果的分布。

---

### 空 10：绘制散点图

```python
plt.scatter(data['年龄'], data['疾病严重程度'])
```

**解释**：`plt.scatter(x, y)` 绘制散点图，这里 x 是年龄，y 是疾病严重程度。

---

### 空 11：保存处理后的数据

```python
data.to_csv(output_path, index=False)
```

**解释**：`to_csv()` 保存为 CSV 文件；`index=False` 不保存行索引。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `data.info()` | 查看数据框基本信息 |
| `data.rename()` | 修改列名 |
| `data.drop_duplicates()` | 删除重复行 |
| `scaler.fit_transform()` | 归一化数据 |
| `DataFrame.plot()` | 绘制图表 |
| `plt.scatter()` | 绘制散点图 |
| `to_csv()` | 保存为 CSV 文件 |

---

## 注意事项

1. **读取时要指定 `encoding='gbk'`**，否则中文可能乱码。
2. **`data.info()` 查看表结构**，不是 `data.shape`。
3. **列名修改用字典形式**：`{'旧名': '新名'}`。
4. **日期相减得到 Timedelta**，要用 `.dt.days` 转成天数。
5. **异常值过滤条件用 `&` 连接**，每个条件要加括号。
6. **`drop_duplicates(inplace=True)` 直接修改原数据**。
7. **归一化列是 `['年龄', '体重', '身高']`**，注意列名顺序。
8. **柱状图用 `plot(kind='bar', stacked=True)`** 实现堆叠效果。
9. **散点图用 `plt.scatter(x, y)`**，x 在前，y 在后。
10. **保存 CSV 时写 `index=False`**。

---

## 答题卷：数据清洗和标注规范

### 制定数据清洗规范

1. 数据加载：使用 pandas 库加载数据集，检查数据的基本结构和类型。
2. 检查数据集中的缺失值，使用删除包含缺失值的行的办法处理，记录缺失值处理后的数据行数；
3. 增加“诊断延迟”（诊断日期-就诊日期）和“病程”（当前日期-诊断日期）两列，删除不合理的数据（如负数，年龄为几百岁等）；
4. 检查数据集中的重复值并删除所有重复值，并记录删除的行数；
5. 对数据段[年龄，体重，身高]进行归一化处理；

### 制定数据标注规范

1. 数据来源：标注数据的来源，包括数据集的名称、获取日期和数据提供者。
2. 数据描述：提供详细的数据描述，包括每列数据的含义、单位和可能的取值范围。
3. 特征选择：确定对目标变量预测最有用的特征。
4. 目标变量设定：根据业务需求和数据特性，选择最有用的特征。
5. 数据划分：将数据分为训练集和测试集，通常采用 80/20 的比例，以便于模型的训练和评估。
6. 保存处理后的数据：保存处理后的数据，并记录保存文件的路径和文件名。
7. 数据清洗和标注规范文档
