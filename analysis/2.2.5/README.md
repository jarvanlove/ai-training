# 2.2.5 每日步数预测决策树模型训练与测试

## 题目大意

读取 fitness analysis 数据集，对分类特征做独热编码，以 daily_steps 为目标变量，用决策树回归模型预测每日步数，保存模型、结果和测试报告，并分析模型性能。

---

## 空缺部分答案 + 代码解释

### 空 1-2：加载数据并显示前五行

```python
df = pd.read_csv('fitness analysis.csv')
print(df.head())
```

**解释**：读取 CSV 文件并显示前 5 行。

---

### 空 3：分类变量转数值变量

```python
X = pd.get_dummies(X)
```

**解释**：`get_dummies()` 把文本类别转换成 0/1 数值列。

---

### 空 4：设置目标变量

```python
y = df['daily_steps']
```

**解释**：目标变量是 `daily_steps`，即每日步数。

---

### 空 5：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：测试集占 20%，训练集占 80%。

---

### 空 6-8：创建并训练决策树回归模型

```python
dt_model = DecisionTreeRegressor(random_state=42)
dt_model.fit(X_train, y_train)
```

**解释**：创建决策树回归模型并训练。

---

### 空 9：保存模型

```python
pickle.dump(dt_model, model_file)
```

**解释**：把训练好的决策树模型保存到 `2.2.5_model.pkl`。

---

### 空 10：预测测试集

```python
y_pred = dt_model.predict(X_test)
```

**解释**：用决策树模型对测试集预测每日步数。

---

### 空 11：保存预测结果

```python
results.to_csv(results_filename, index=False, sep='\t')
```

**解释**：用制表符分隔，把实际值和预测值保存到文本文件。

---

### 空 12-14：写入测试报告

```python
with open(report_filename, 'w') as f:
    f.write(f'均方误差: {mean_squared_error(y_test, y_pred)}\n')
    f.write(f'平均绝对误差: {mean_absolute_error(y_test, y_pred)}\n')
    f.write(f'决定系数: {r2_score(y_test, y_pred)}\n')
```

**解释**：
- `mean_squared_error()`：均方误差（MSE）。
- `mean_absolute_error()`：平均绝对误差（MAE）。
- `r2_score()`：决定系数 R²。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `df.head()` | 查看前 5 行 |
| `pd.get_dummies()` | 独热编码 |
| `train_test_split()` | 划分训练集和测试集 |
| `DecisionTreeRegressor()` | 创建决策树回归模型 |
| `dt_model.fit()` | 训练决策树模型 |
| `pickle.dump()` | 保存模型 |
| `dt_model.predict()` | 决策树预测 |
| `to_csv()` | 保存为 CSV/文本文件 |
| `mean_squared_error()` | 计算均方误差 |
| `mean_absolute_error()` | 计算平均绝对误差 |
| `r2_score()` | 计算决定系数 |

---

## 注意事项

1. **文件名是 `'fitness analysis.csv'`**，中间有空格。
2. **`Your gender ` 列名末尾有一个空格**，和原始数据一致。
3. **分类变量要先做 `pd.get_dummies()` 独热编码**。
4. **目标变量是 `'daily_steps'`**（每日步数）。
5. **`DecisionTreeRegressor` 是回归器**，不是分类器。
6. **保存结果时用 `sep='\t'`**，制表符分隔。
7. **打开文件写报告时要加 `'w'` 模式**。
8. **MSE 和 MAE 越小越好，R² 越接近 1 越好**；R² 为负说明模型效果很差。
9. **测试结果中的具体数字由实际运行时数据决定**，不要死记硬背。

---

## 答题卷：测试结果分析报告

### 1、模型性能

| 指标 | 得分 |
|------|------|
| 均方误差（MSE） | 8096170.758224316 |
| 平均绝对误差（MAE） | 2421.827880665033 |
| 决定系数（R2） | -0.1541458336017123 |

> 注：表中数值为示例，实际考试时根据真实运行结果填写，不要死记硬背。

### 2、错误分析

欠拟合；模型简单；特征不够全面。

**什么是“欠拟合”？**

欠拟合就是模型“学得太少”，连训练数据都没学好，训练集和测试集都预测不准。

打个比方：一个学生上课完全没听懂，练习册和考试都不会做，这就是欠拟合。

判断方法：
- 训练集得分就很低，测试集得分也低 → 可能欠拟合。
- 训练集得分高、测试集得分低 → 可能过拟合。

本题 R² 为负数，说明决策树模型根本没学到规律，是典型的欠拟合。

### 3、改进建议

数据改进，模型替换，参数调优。
