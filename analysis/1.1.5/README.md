# 1.1.5 车辆交通数据采集与处理

## 题目大意

读取车辆行驶数据，完成采集、清洗（缺失值、类型转换、异常值），再审核不合理数据，最后统计交通事件、性别平均指标和年龄段驾驶员数。

---

## 空缺部分答案 + 代码解释

### 空 1：读取数据

```python
data = pd.read_csv('vehicle_traffic_data.csv')
```

**解释**：读取车辆交通数据文件。

---

### 空 2：打印前 5 条

```python
print(data.head())
```

**解释**：显示前 5 行数据。

---

### 空 3：删除缺失值

```python
data = data.dropna()
```

**解释**：删除包含缺失值的行。

---

### 空 4-5：Age 转 int

```python
data['Age'] = data['Age'].astype(int)
```

**解释**：把年龄列转换为整数。

---

### 空 6-7：Speed 转 float

```python
data['Speed'] = data['Speed'].astype(float)
```

**解释**：把车速列转换为浮点数。

---

### 空 8-9：TravelDistance 转 float

```python
data['TravelDistance'] = data['TravelDistance'].astype(float)
```

**解释**：把行驶距离列转换为浮点数。

---

### 空 10-11：TravelTime 转 float

```python
data['TravelTime'] = data['TravelTime'].astype(float)
```

**解释**：把行驶时间列转换为浮点数。

---

### 空 12-15：处理异常值

```python
data = data[(data['Age'].between(18, 70))  & 
            (data['Speed'].between(0, 200)) & 
            (data['TravelDistance'].between(1, 1000)) & 
            (data['TravelTime'].between(1, 1440))]
```

**解释**：保留同时满足以下条件的数据：
- 年龄在 18-70 岁
- 车速在 0-200 km/h
- 行驶距离在 1-1000 km
- 行驶时间在 1-1440 分钟（24 小时）

---

### 空 16：保存清洗后的数据

```python
data.to_csv('cleaned_vehicle_traffic_data.csv', index=False)
```

**解释**：保存清洗后的数据为 CSV 文件。

---

### 空 17-20：审核不合理数据

```python
unreasonable_data = data[~((data['Age'].between(18, 70)) & 
                           (data['Speed'].between(0, 200)) & 
                           (data['TravelDistance'].between(1, 1000)) & 
                           (data['TravelTime'].between(1, 1440)))]
```

**解释**：`~` 表示取反，即找出**不满足**所有合理条件的数据行。

---

### 空 21：统计每种交通事件发生次数

```python
traffic_event_counts = data['TrafficEvent'].value_counts()
```

**解释**：统计 `TrafficEvent` 列每个类别的发生次数。

---

### 空 22-23：统计不同性别的平均指标

```python
gender_stats = data.groupby('Gender').agg({'Speed':'mean', 'TravelDistance':'mean', 'TravelTime':'mean'})
```

**解释**：
- 按性别分组。
- `agg({列名:'mean', ...})`：同时计算多列的平均值。

---

### 空 24-27：按年龄分组

```python
data['AgeGroup'] = pd.cut(data['Age'], bins=age_bins, labels=age_labels, right=False)
```

**解释**：用 `pd.cut` 把年龄切成 6 个区间。

---

### 空 28：统计不同年龄段驾驶员数

```python
age_group_counts = data['AgeGroup'].value_counts()
```

**解释**：统计每个年龄段的人数。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `head()` | 查看前 5 行 |
| `dropna()` | 删除缺失值 |
| `astype()` | 转换数据类型 |
| `between()` | 判断数值是否在范围内 |
| `to_csv()` | 保存为 CSV |
| `value_counts()` | 统计各类别数量 |
| `groupby()` | 按某列分组 |
| `agg()` | 聚合计算多个统计量 |
| `cut()` | 把连续数值切成区间 |

---

## 注意事项

1. **本题用 `pd.read_csv` 和 `pd.cut`**，不是 `pandas.xxx`（和 1.1.4 区别开）。
2. **`dropna()` 直接删除含缺失值的行**。
3. **四列类型转换**：Age 转 int，Speed/TravelDistance/TravelTime 转 float。
4. **异常值条件用 `&` 连接**，四个条件同时满足。
5. **合理性审核用 `~(...)` 取反**，找出不合理的数据。
6. **`agg({列名:'mean'})`**：可以同时计算多列的平均值。
7. **年龄分组用 `pd.cut`**，参数是 `bins`、`labels`、`right=False`。
8. **行驶时间上限 1440 分钟 = 24 小时**，要记住这个业务含义。
9. **保存文件是 `data.to_csv`**，不是 `cleaned_data.to_csv`。
10. **列名不要写错**：`TrafficEvent`、`TravelDistance`、`TravelTime`。
