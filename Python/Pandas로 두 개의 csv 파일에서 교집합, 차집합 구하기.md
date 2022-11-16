# Pandas로 두 개의 csv 파일에서 교집합, 차집합 구하기





#### A.csv의 데이터 구조


||worker_id|
|------|---|
|0|Worker1|
| 1    |Worker3|
|2|Worker5|



#### B.csv의 데이터 구조


||worker_id|
|------|---|
|0|Worker1|
|1|Worker12|
|2|Worker12|
|3|Worker3|
|4|Worker3|
|5|Worker4|






### python code

```python
import pandas as pd
import numpy as np

if __name__ == '__main__':
    col = ["worker_id"]
    a_csv = pd.read_csv("data/A.csv", usecols=col)
   
    b_csv = pd.read_csv("data/B.csv")

    # how를 outer 로 두고 merge를 한다. 이 때 indicator 값을 True 로 두면 _merge column이 새로 생기면서 각 항목이 어느 쪽에 속하는지 표시된다.
    df = pd.merge(a_csv, b_csv, how="outer", indicator=True)
    
    # query를 이용해 _merge열의 값이 both 인 값만 필터링하고, 마지막으로 _merge column을 drop한다.
    df = df.query("_merge == 'both'").drop(columns=["_merge"])

    print(df)
    
    # to_csv를 통해 new.csv 파일을 쓴다(저장한다)
    df.to_csv("new.csv") 
```

