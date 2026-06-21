# 2.2.3 健身分析年龄预测模型训练与测试

## 题目大意

读取健身分析数据集，对分类特征进行独热编码，将年龄段转为数值，分别用随机森林回归和 XGBoost 回归训练年龄预测模型，保存模型、预测结果和测试报告，并分析模型性能。

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

**解释**：`get_dummies()` 把文本类别转换成 0/1 数值列（独热编码），方便模型使用。

---

### 空 4：将年龄段转为数值

```python
y = df['Your age'].apply(lambda x: int(x.split(' ')[0]))
```

**解释**：假设年龄列是类似 `'25 岁'` 的字符串，用空格分割后取第一个元素转成整数。

---

### 空 5：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：测试集占 20%，训练集占 80%。

---

### 空 6-7：创建并训练随机森林模型

```python
rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)
```

**解释**：创建 100 棵决策树的随机森林回归模型，并训练。

---

### 空 8：保存模型

```python
pickle.dump(rf_model, model_file)
```

**解释**：把训练好的随机森林模型保存到文件。

---

### 空 9：预测测试集

```python
y_pred = rf_model.predict(X_test)
```

**解释**：用训练好的随机森林模型对测试集进行预测。

---

### 空 10-13：计算评估指标

```python
train_score = rf_model.score(X_train, y_train)
test_score = rf_model.score(X_test, y_test)
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
```

**解释**：
- `score()`：返回模型的 R² 得分。
- `mean_squared_error()`：均方误差，越小越好。
- `r2_score()`：决定系数 R²，越接近 1 越好。

---

### 空 14-16：创建并训练 XGBoost 模型

```python
xgb_model = xgb.XGBRegressor(n_estimators=100, random_state=42)
xgb_model.fit(X_train, y_train)
```

**解释**：创建 XGBoost 回归模型，100 棵树，并训练。

---

### 空 17：XGBoost 预测

```python
y_pred_xgb = xgb_model.predict(X_test)
```

**解释**：用 XGBoost 模型对测试集预测。

---

### 空 18-21：XGBoost 评估指标

```python
xgb_report_file.write(f'XGBoost训练集得分: {xgb_model.score(X_train, y_train)}\n')
xgb_report_file.write(f'XGBoost测试集得分: {xgb_model.score(X_test, y_test)}\n')
xgb_report_file.write(f'XGBoost均方误差(MSE): {mean_squared_error(y_test, y_pred_xgb)}\n')
xgb_report_file.write(f'XGBoost决定系数(R^2): {r2_score(y_test, y_pred_xgb)}\n')
```

**解释**：和随机森林的评估指标一样，只是换成 XGBoost 的预测结果。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `df.head()` | 查看前 5 行 |
| `pd.get_dummies()` | 独热编码 |
| `Series.apply()` | 对列的每个元素应用函数 |
| `train_test_split()` | 划分训练集和测试集 |
| `RandomForestRegressor()` | 创建随机森林回归模型 |
| `rf_model.fit()` | 训练随机森林模型 |
| `pickle.dump()` | 保存模型 |
| `rf_model.predict()` | 随机森林预测 |
| `rf_model.score()` | 计算 R² 得分 |
| `mean_squared_error()` | 计算均方误差 |
| `r2_score()` | 计算决定系数 |
| `xgb.XGBRegressor()` | 创建 XGBoost 回归模型 |
| `xgb_model.fit()` | 训练 XGBoost 模型 |
| `xgb_model.predict()` | XGBoost 预测 |

---

## 注意事项

1. **文件名是 `'fitness analysis.csv'`**，中间有空格。
2. **分类变量要先做 `pd.get_dummies()` 独热编码**，否则模型无法处理文本。
3. **目标变量是年龄段字符串**，需要提取数字部分。
4. **`RandomForestRegressor` 是回归器**，不是分类器。
5. **`xgb.XGBRegressor()` 要加 `xgb.` 前缀**，因为 `import xgboost as xgb`。
6. **`n_estimators=100` 表示 100 棵树**。
7. **MSE 越小越好，R² 越接近 1 越好**；R² 为负数说明模型效果很差。
8. **测试结果中的具体数字由实际运行时数据决定**，不要死记硬背。

---

## 答题卷：测试结果分析报告

### 针对随机森林模型

#### 1、模型性能

| 指标 | 得分 |
|------|------|
| 训练集得分 | 0.12387053768702816 |
| 测试集得分 | -0.09219954290443844 |
| 均方误差（MSE） | 109.76692738027478 |
| 决定系数（R²） | -0.09219954290443844 |

> 注：表中数值为示例，实际考试时根据真实运行结果填写，不要死记硬背。

#### 2、错误分析

欠拟合，特征不够全面；模型相对简单。

**什么是“欠拟合”？**

欠拟合就是模型“学得太少”，连训练数据都没学好，对训练集和测试集都预测不准。

打个比方：一个学生上课完全没听懂，练习册上的题也不会做，考试时同样也不会做。这就是欠拟合。

在代码里怎么判断？看训练集得分和测试集得分：
- 如果训练集得分就很低，测试集得分也低 → 可能欠拟合。
- 如果训练集得分高、测试集得分低 → 可能过拟合。

本题训练集得分只有 0.12，测试集得分还是负数，说明模型根本没学到规律，是典型的欠拟合。

#### 3、改进建议

数据改进，模型替换、参数调优。
