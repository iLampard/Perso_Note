# Pandas Snippets


### Data Processing
  
#### NA Handler

- fillna
```python
df.fillna(df.median(), inplace=True)
```

- countna
```python
df.apply(lambda x: sum(x.isnull()))
```

```python
df = pd.DataFrame({'a':[1,2,np.nan], 'b':[np.nan,1,np.nan]})
df.isnull().sum()
```


#### Outlier Handler
```python
lbound, ubound = train[col].quantile(0.02), train[col].quantile(0.98)
train = train[train[col] < ubound]
train = train[train[col] > lbound]

# bound
train.loc[train[col] < lbound, col] = lbound
train.loc[train[col] > ubound, col] = ubound
```



#### loc and iloc 
- replace
```python
ret.loc[ret['date'] == '2018-06-30', 'date'] = '2018-06-29'
```



#### datetime 

```python
revenue['year'] = pd.DatetimeIndex(revenue['end_date']).year
revenue['quarter'] = pd.DatetimeIndex(revenue['end_date']).quarter
```

