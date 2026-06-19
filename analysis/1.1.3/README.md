# 1.1.3 信用数据审核与清洗

## 题目大意

读取客户信用数据，检查缺失值和重复值，再按业务规则审核年龄、收入、贷款金额、信用评分的合理性，最后删除不合理数据并保存。

---

## 空缺部分答案 + 代码解释

### 空 1：统计缺失值

```python
missing_values = data.isnull().sum()
```

**解释**：
- `data.isnull()`：判断每个单元格是否缺失，返回 True/False。
- `.sum()`：对每一列求和，得到每列缺失值的数量。

---

### 空 2：统计重复值

```python
duplicate_values = data.duplicated().sum()
```

**解释**：
- `data.duplicated()`：判断每一行是否是重复行，返回 True/False。
- `.sum()`：统计重复行的总数。

---

### 空 3-4：年龄合理性审核

```python
data['is_age_valid'] = data['Age'].between(18, 70)
```

**解释**：`between(18, 70)` 判断年龄是否在 18 到 70 岁之间（包含 18 和 70），结果保存到新列 `is_age_valid`。

---

### 空 5-6：收入合理性审核

```python
data['is_income_valid'] = data['Income'] > 2000
```

**解释**：判断收入是否大于 2000，结果保存到新列 `is_income_valid`。

---

### 空 7-9：贷款金额合理性审核

```python
data['is_loan_amount_valid'] = data['LoanAmount'] < (data['Income'] * 5)
```

**解释**：判断贷款金额是否小于收入的 5 倍，结果保存到新列 `is_loan_amount_valid`。

---

### 空 10-11：信用评分合理性审核

```python
data['is_credit_score_valid'] = data['CreditScore'].between(300, 850)
```

**解释**：判断信用评分是否在 300 到 850 之间，结果保存到新列 `is_credit_score_valid`。

---

### 空 12-14：保存清洗后的数据

```python
cleaned_data.to_csv('cleaned_credit_data.csv', index=False)
```

**解释**：把清洗后的数据保存为 `cleaned_credit_data.csv`，`index=False` 表示不保存行序号。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `isnull()` | 判断缺失值 |
| `sum()` | 求和；对 True/False 序列就是计数 |
| `duplicated()` | 判断重复行 |
| `between(a, b)` | 判断数值是否在 a 和 b 之间 |
| `to_csv()` | 保存为 CSV 文件 |

---

## 注意事项

1. **`isnull().sum()` 是固定搭配**：用来统计每列缺失值数量。
2. **`duplicated().sum()` 是固定搭配**：用来统计重复行数量。
3. **`between(18, 70)` 默认是闭区间**，包含 18 和 70。
4. **收入判断是 `> 2000`**，注意不是 `>=`。
5. **贷款金额和收入比较**：`data['LoanAmount'] < (data['Income'] * 5)`，注意括号。
6. **信用评分范围是 300-850**，和年龄一样用 `between()`。
7. **保存文件时用 `index=False`**，避免多出一列行号。
8. **列名大小写**：`Age`、`Income`、`LoanAmount`、`CreditScore` 不要写错。
