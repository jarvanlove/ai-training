# 1.1.2 传感器数据统计与清洗

## 题目大意

读取传感器数据，按传感器类型统计数量和平均值，再筛选温度和湿度按位置求平均，最后标记异常值、填补缺失值并保存清洗后的数据。

---

## 空缺部分答案 + 代码解释

### 空 1：读取数据

```python
data = pd.read_csv('sensor_data.csv')
```

**解释**：读取 `sensor_data.csv` 文件，把传感器数据存到 `data` 中。

---

### 空 2-3：按传感器类型统计

```python
sensor_stats = data.groupby('SensorType')['Value'].agg(['count', 'mean'])
```

**解释**：
- `data.groupby('SensorType')`：按传感器类型分组。
- `['Value']`：只看 `Value` 这一列。
- `.agg(['count', 'mean'])`：同时计算每组的**数量**和**平均值**。

---

### 空 4-5：筛选温度和湿度并按位置分组求平均

```python
location_stats = data[data['SensorType'].isin(['Temperature', 'Humidity'])].groupby(['Location', 'SensorType'])['Value'].mean().unstack()
```

**解释**：
- `data['SensorType'].isin(['Temperature', 'Humidity'])`：筛选出传感器类型是温度或湿度的行。
- `.groupby(['Location', 'SensorType'])`：按位置和传感器类型同时分组。
- `['Value'].mean()`：计算每组的平均值。
- `.unstack()`：把分组结果从"长表"转成"宽表"，方便查看。

---

### 空 6-9：标记异常值

```python
data['is_abnormal'] = np.where(
    ((data['SensorType'] == 'Temperature') & ((data['Value'] < -10) | (data['Value'] > 50))) |
    ((data['SensorType'] == 'Humidity') & ((data['Value'] < 0) | (data['Value'] > 100))),
    True, False
)
```

**解释**：
- `np.where(条件, True, False)`：满足条件标为 True（异常），不满足标为 False（正常）。
- 条件分两部分：
  - 温度传感器：值小于 -10 或大于 50 为异常。
  - 湿度传感器：值小于 0 或大于 100 为异常。
- `&` 表示"并且"，`|` 表示"或者"。

---

### 空 10：输出异常值数量

```python
print("异常值数量:", data['is_abnormal'].sum())
```

**解释**：`is_abnormal` 列是 True/False，True 当作 1，False 当作 0，`.sum()` 就是异常值总数。

---

### 空 11-14：填补缺失值

```python
data['Value'].fillna(method='ffill', inplace=True)
data['Value'].fillna(method='bfill', inplace=True)
```

**解释**：
- `.fillna(method='ffill')`：用前一个有效值填充缺失值（forward fill，前向填充）。
- `.fillna(method='bfill')`：用后一个有效值填充缺失值（backward fill，后向填充）。
- `inplace=True`：直接修改原数据，不需要重新赋值。

---

### 空 15-17：删除异常值标记列并保存

```python
cleaned_data = data.drop(columns=['is_abnormal'])
cleaned_data.to_csv('cleaned_sensor_data.csv', index=False)
```

**解释**：
- `.drop(columns=['is_abnormal'])`：删除 `is_abnormal` 这一列。
- `.to_csv('文件名.csv', index=False)`：把清洗后的数据保存为 CSV 文件，`index=False` 表示不保存行序号。

---

## 用到的 Python 函数（仅空缺部分）

| 函数 | 含义 |
|------|------|
| `pd.read_csv()` | 读取 CSV 文件 |
| `groupby()` | 按某列分组 |
| `agg()` | 聚合计算，可同时算多个统计量 |
| `isin()` | 判断是否在指定列表中 |
| `np.where()` | 按条件填值 |
| `sum()` | 求和；对 True/False 序列就是计数 |
| `fillna()` | 填充缺失值 |
| `drop()` | 删除列或行 |
| `to_csv()` | 保存为 CSV 文件 |

---

## 注意事项

1. **`agg(['count', 'mean'])`**：可以同时计算数量和平均值，注意 `count` 和 `mean` 都要加引号。
2. **`isin()` 里面要放列表**：如 `isin(['Temperature', 'Humidity'])`。
3. **`groupby(['Location', 'SensorType'])`**：多列分组要用方括号括起来。
4. **`.unstack()` 的作用**：把多层索引的结果展开成表格形式，行列更清晰。
5. **`np.where` 的复杂条件**：本题条件很长，注意括号配对，`&` 是"并且"，`|` 是"或者"。
6. **`fillna` 的方向**：`ffill` 是向前填充（用上面的值），`bfill` 是向后填充（用下面的值）。
7. **`inplace=True` 表示原地修改**，不需要写成 `data['Value'] = data['Value'].fillna(...)`。
8. **`to_csv` 保存时加 `index=False`**，否则会把行序号也保存成一列。
9. **列名要写对**：如 `SensorType`、`Value`、`Location`、`is_abnormal`。
