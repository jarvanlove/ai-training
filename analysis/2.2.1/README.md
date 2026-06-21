# 2.2.1 金融信用评分 Logistic 回归模型训练与测试

## 题目大意

读取 finance 数据集，使用 Logistic 回归训练信用评分模型，保存模型和预测结果，生成测试报告；然后通过 SMOTE 处理数据不平衡问题，重新训练并评估模型。

---

## 空缺部分答案 + 代码解释

### 空 1-2：加载数据并显示前五行

```python
data = pd.read_csv('finance数据集.csv')
print(data.head())
```

**解释**：
- `pd.read_csv()`：读取 CSV 文件。
- `data.head()`：显示前 5 行数据。

---

### 空 3：划分训练集和测试集

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**解释**：
- `train_test_split`：划分数据集。
- `test_size=0.2`：测试集占 20%，训练集占 80%。

---

### 空 4-5：创建并训练 Logistic 回归模型

```python
model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)
```

**解释**：
- `LogisticRegression(max_iter=1000)`：创建逻辑回归模型，最大迭代次数设为 1000，防止不收敛。
- `model.fit(X_train, y_train)`：用训练数据训练模型。

---

### 空 6：保存模型

```python
pickle.dump(model, file)
```

**解释**：`pickle.dump()` 把训练好的模型对象保存到文件 `2.2.1_model.pkl` 中。

---

### 空 7：预测测试集

```python
y_pred = model.predict(X_test)
```

**解释**：`model.predict()` 用训练好的模型对测试集进行预测。

---

### 空 8：计算模型准确率

```python
accuracy = (y_test == y_pred).mean()
```

**解释**：比较预测值和真实值，相等为 True，求平均即为准确率。

---

### 空 9：使用 SMOTE 处理数据不平衡

```python
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)
```

**解释**：`SMOTE` 过采样，对训练集中的少数类样本进行扩充，使正负样本更平衡。

---

### 空 10-11：重新训练并预测

```python
model.fit(X_resampled, y_resampled)
y_pred_resampled = model.predict(X_test)
```

**解释**：用过采样后的数据重新训练模型，再对测试集预测。

---

### 空 12：计算重新采样后的准确率

```python
accuracy_resampled = (y_test == y_pred_resampled).mean()
```

**解释**：同样用预测值和真实值比较求平均，得到重新采样后的准确率。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `data.head()` | 查看前 5 行 |
| `train_test_split()` | 划分训练集和测试集 |
| `LogisticRegression()` | 创建逻辑回归模型 |
| `model.fit()` | 训练模型 |
| `pickle.dump()` | 保存模型对象 |
| `model.predict()` | 模型预测 |
| `classification_report()` | 生成分类报告 |
| `smote.fit_resample()` | SMOTE 过采样 |

---

## 注意事项

1. **文件名是 `finance数据集.csv`**，注意是小写 `finance`。
2. **Logistic 回归要设 `max_iter=1000`**，否则可能迭代不收敛。
3. **`X` 删除了 `'SeriousDlqin2yrs'` 和 `'Unnamed: 0'` 两列**，前者是目标变量，后者是无用索引。
4. **`pickle.dump(model, file)` 保存模型**，注意参数顺序是对象在前，文件在后。
5. **准确率用 `(y_test == y_pred).mean()` 计算**，不需要额外导入 accuracy_score。
6. **SMOTE 只对训练集 `X_train, y_train` 进行过采样**，不能对测试集做过采样。
7. **重新训练模型时可以直接用同一个 `model` 对象**，也可以新建一个。
8. **测试结果中的具体数字由实际运行时数据决定**，不要死记硬背。

---

## 答题卷：测试结果分析报告

### 针对 Logistic 模型

#### 1、模型性能

| 类别 | precision | recall | f1-score | support |
|------|-----------|--------|----------|---------|
| 0（没有严重逾期） | 0.98 | 0.75 | 0.85 | 26779 |
| 1（有严重逾期） | 0.17 | 0.76 | 0.27 | 1737 |

> 注：表中数值为示例，实际考试时根据真实运行结果填写，不要死记硬背。

#### 2、错误分析

- 0（没有严重逾期）：准确率很高，召回率也很高。
- 1（有严重逾期）：准确率较低，召回率也很低，F1-Score 仅为 0.27。

#### 3、改进建议

1. **数据处理策略调整**
   - 重采样技术：由于数据集存在明显的不平衡，可以考虑使用过采样（如 SMOTE）。
