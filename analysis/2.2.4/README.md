# 2.2.4 大学生低碳生活行为预测模型训练与测试

## 题目大意

读取大学生低碳生活行为数据集，删除无关列，对分类变量做独热编码，分别用线性回归和 XGBoost 训练预测模型，保存模型、预测结果和测试报告，并分析模型性能。

---

## 空缺部分答案 + 代码解释

### 空 1-2：加载数据并显示前五行

```python
data = pd.read_excel('大学生低碳生活行为的影响因素数据集.xlsx')
print(data.head())
```

**解释**：读取 Excel 文件并显示前 5 行。

---

### 空 3：删除不必要的列

```python
data_cleaned = data.drop(columns=['序号', '所用时间'])
```

**解释**：`drop(columns=[...])` 删除对建模无用的列，这里是序号和填写所用时间。

---

### 空 4-6：定义特征 X 和目标 y

```python
X = data_cleaned.drop(columns=[target])
y = data_cleaned[target]
```

**解释**：
- `X`：删除目标变量列后剩下的特征。
- `y`：目标变量 `'5.您进行过绿色低碳的相关生活方式吗?'`。

---

### 空 7：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：测试集占 20%，训练集占 80%。

---

### 空 8-9：创建并训练线性回归模型

```python
model = LinearRegression()
model.fit(X_train, y_train)
```

**解释**：创建线性回归模型并用训练数据拟合。

---

### 空 10：保存模型

```python
joblib.dump(model, model_filename)
```

**解释**：`joblib.dump()` 把训练好的模型保存到 `2.2.4_model.pkl`。

---

### 空 11：预测测试集

```python
y_pred = model.predict(X_test)
```

**解释**：用训练好的线性回归模型对测试集预测。

---

### 空 12：保存预测结果

```python
results.to_csv(results_filename, index=False, sep='\t')
```

**解释**：用制表符 `\t` 作为分隔符，把实际值和预测值保存为文本文件。

---

### 空 13-14：计算 MSE 和 R²

```python
f.write(f'均方误差: {mean_squared_error(y_test, y_pred)}\n')
f.write(f'决定系数: {r2_score(y_test, y_pred)}\n')
```

**解释**：
- `mean_squared_error()`：计算均方误差。
- `r2_score()`：计算决定系数 R²。

---

### 空 15-17：创建并训练 XGBoost 模型

```python
xgb_model = XGBRegressor(n_estimators=1000, learning_rate=0.05, max_depth=5, subsample=0.8, colsample_bytree=0.8)
xgb_model.fit(X_train, y_train)
```

**解释**：
- `n_estimators=1000`：1000 棵树。
- `learning_rate=0.05`：学习率。
- `max_depth=5`：每棵树最大深度。
- `subsample=0.8` 和 `colsample_bytree=0.8`：随机抽样比例，防止过拟合。

---

### 空 18：XGBoost 预测

```python
y_pred_xg = xgb_model.predict(X_test)
```

**解释**：用 XGBoost 模型对测试集预测。

---

### 空 19-20：XGBoost 评估指标

```python
f.write(f'均方误差: {mean_squared_error(y_test, y_pred_xg)}\n')
f.write(f'决定系数: {r2_score(y_test, y_pred_xg)}\n')
```

**解释**：计算 XGBoost 预测结果的 MSE 和 R²。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_excel()` | 读取 Excel 文件 |
| `data.head()` | 查看前 5 行 |
| `data.drop()` | 删除指定列 |
| `pd.get_dummies()` | 独热编码 |
| `train_test_split()` | 划分训练集和测试集 |
| `LinearRegression()` | 创建线性回归模型 |
| `model.fit()` | 训练模型 |
| `joblib.dump()` | 保存模型 |
| `model.predict()` | 模型预测 |
| `to_csv()` | 保存为 CSV/文本文件 |
| `mean_squared_error()` | 计算均方误差 |
| `r2_score()` | 计算决定系数 |
| `XGBRegressor()` | 创建 XGBoost 回归模型 |
| `xgb_model.fit()` | 训练 XGBoost 模型 |
| `xgb_model.predict()` | XGBoost 预测 |

---

## 注意事项

1. **Excel 文件用 `pd.read_excel()`** 读取。
2. **`drop(columns=[...])` 删除无关列**，注意列名要和数据集中一致。
3. **`pd.get_dummies(drop_first=True)` 做独热编码**，避免多重共线性。
4. **目标变量是 `'5.您进行过绿色低碳的相关生活方式吗?'`**。
5. **保存结果时用 `sep='\t'`**，表示制表符分隔。
6. **`joblib.dump()` 保存模型**，和 `pickle.dump()` 作用类似。
7. **XGBoost 参数较多**，注意 `n_estimators`、`learning_rate`、`max_depth` 的写法。
8. **测试结果中的具体数字由实际运行时数据决定**，不要死记硬背。

---

## 答题卷：测试结果分析报告

### 针对线性回归模型

#### 1、模型性能

| 指标 | 得分 |
|------|------|
| 均方误差（MSE） | 0.024679563713426132 |
| 决定系数（R²） | 0.18477828249843997 |

> 注：表中数值为示例，实际考试时根据真实运行结果填写，不要死记硬背。

#### 2、错误分析

欠拟合，模型简单、特征不够全面。

**什么是“欠拟合”？**

欠拟合就是模型“学得太少”，连训练数据都没学好，训练集和测试集都预测不准。

打个比方：一个学生上课完全没听懂，练习册和考试都不会做，这就是欠拟合。

判断方法：
- 训练集得分就很低，测试集得分也低 → 可能欠拟合。
- 训练集得分高、测试集得分低 → 可能过拟合。

本题 R² 只有 0.18，说明线性回归模型没能很好地捕捉数据规律，属于欠拟合。

#### 3、改进建议

数据改进、替换模型、参数调优。
