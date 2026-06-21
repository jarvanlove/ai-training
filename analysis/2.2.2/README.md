# 2.2.2 汽车燃油效率预测模型训练与测试

## 题目大意

读取汽车燃油效率数据集，清洗 horsepower 列缺失值，使用线性回归管道模型和随机森林回归模型预测 MPG，保存模型、预测结果和测试报告，并分析模型性能。

---

## 空缺部分答案 + 代码解释

### 空 1-2：加载数据并显示前五行

```python
df = pd.read_csv('auto-mpg.csv')
print(df.head())
```

**解释**：读取 CSV 文件并显示前 5 行。

---

### 空 3-4：处理 horsepower 缺失值

```python
df['horsepower'] = pd.to_numeric(df['horsepower'], errors='coerce')
df = df.dropna()
```

**解释**：
- `pd.to_numeric(..., errors='coerce')`：把 horsepower 转成数字，异常值变 NaN。
- `dropna()`：删除包含缺失值的行。

---

### 空 5-6：选择特征和目标变量

```python
X = df[['cylinders', 'displacement', 'horsepower', 'weight', 'acceleration', 'model year', 'origin']]
y = df['mpg']
```

**解释**：
- `X`：7 个特征列。
- `y`：目标变量 `mpg`（燃油效率）。

---

### 空 7：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：测试集占 20%，训练集占 80%。

---

### 空 8：创建 Pipeline

```python
pipeline = Pipeline([('scaler', StandardScaler()),('linreg', LinearRegression())])
```

**解释**：
- `Pipeline`：把多个步骤串联起来。
- 第一步 `scaler` 用 `StandardScaler()` 做标准化。
- 第二步 `linreg` 用 `LinearRegression()` 做线性回归。

---

### 空 9：训练线性回归模型

```python
pipeline.fit(X_train, y_train)
```

**解释**：用训练数据拟合整个管道，先标准化再训练线性回归。

---

### 空 10：保存模型

```python
pickle.dump(pipeline, model_file)
```

**解释**：把训练好的管道模型保存到 `2.2.2_model.pkl`。

---

### 空 11-12：预测并保存结果

```python
y_pred = pipeline.predict(X_test)
results_df = pd.DataFrame(y_pred, columns=['预测结果'])
results_df.to_csv('2.2.2_results.txt', index=False)
```

**解释**：对测试集预测，结果保存为 CSV 文件。

---

### 空 13-14：创建并训练随机森林模型

```python
rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)
```

**解释**：
- `RandomForestRegressor(n_estimators=100)`：创建随机森林回归模型，100 棵决策树。
- `fit()`：训练模型。

---

### 空 15-16：随机森林预测并保存结果

```python
y_pred_rf = rf_model.predict(X_test)
results_rf_df = pd.DataFrame(y_pred_rf, columns=['预测结果'])
results_rf_df.to_csv('2.2.2_results_rf.txt', index=False)
```

**解释**：用随机森林模型预测测试集并保存结果。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `df.head()` | 查看前 5 行 |
| `pd.to_numeric()` | 转换为数值类型 |
| `df.dropna()` | 删除包含缺失值的行 |
| `train_test_split()` | 划分训练集和测试集 |
| `Pipeline()` | 创建机器学习管道 |
| `StandardScaler()` | 标准化数据 |
| `LinearRegression()` | 创建线性回归模型 |
| `pipeline.fit()` | 训练管道模型 |
| `pickle.dump()` | 保存模型 |
| `pipeline.predict()` | 模型预测 |
| `to_csv()` | 保存为 CSV 文件 |
| `RandomForestRegressor()` | 创建随机森林回归模型 |
| `rf_model.fit()` | 训练随机森林模型 |
| `rf_model.predict()` | 随机森林预测 |

---

## 注意事项

1. **horsepower 列可能包含非数字字符**，需先用 `pd.to_numeric(..., errors='coerce')` 处理。
2. **Pipeline 的每一步用元组表示**：`('步骤名', 模型/转换器)`。
3. **`StandardScaler()` 和 `LinearRegression()` 都要加括号**，创建实例。
4. **保存模型用 `pickle.dump(pipeline, model_file)`**，保存的是整个管道。
5. **预测时用 `pipeline.predict()`**，不是 `model.predict()`。
6. **随机森林用 `RandomForestRegressor`**，不是 `RandomForestClassifier`（本题是回归问题）。
7. **`n_estimators=100`** 表示 100 棵决策树。
8. **测试结果中的具体得分由实际运行时数据决定**，不要死记硬背。

---

## 答题卷：测试结果分析报告

### 针对线性回归模型

#### 1、模型性能

| 指标 | 得分 |
|------|------|
| 训练集得分 | 0.826001578671067 |
| 测试集得分 | 0.7901500386760344 |

> 注：表中数值为示例，实际考试时根据真实运行结果填写，不要死记硬背。

#### 2、错误分析

过拟合，特征不够全面；模型相对简单。

**什么是“过拟合”？**

过拟合就是模型“死记硬背”训练数据，在训练集上表现很好，但遇到没见过的测试数据时表现变差。

打个比方：一个学生把练习册的答案全背下来了，练习册上的题全对；但考试时题目稍微变一下，他就不会做了。这就是过拟合。

在代码里怎么判断？看训练集得分和测试集得分：
- 如果训练集得分很高，测试集得分明显低很多 → 可能过拟合。
- 如果两者差不多 → 模型泛化能力较好。

本题训练集得分 0.826，测试集得分 0.790，差距不算特别大，但也说明模型对训练数据的拟合程度比新数据更好一些。

#### 3、改进建议

数据改进，模型替换，参数调优。
