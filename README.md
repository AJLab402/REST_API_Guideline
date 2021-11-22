# REST_API_Guideline

본 가이드라인은 testlab402.com 에서 제공되는 REST API를 이용하기 위한 가이드라인입니다.  
간단한 형태로 구성되어 있어 어렵지는 않을 것으로 생각되지만,  
혹시나 궁금한 점이나 어려운 부분 있으시면 Issue 혹은 웹사이트의 Q&A로 남겨주시면 최대한 빠르게 도와드리겠습니다.

***

+ 이용 가이드라인(Python 기준)

필요 라이브러리 : pandas, requests

```python
import pandas as pd
import requests
```

+ data 다운로드 (json기반)

Ex) 네이버의 2012년 이후 분기별 현금흐름표 다운로드(json)
```python
url = 'https://www.testlab402.com/api'
# 네이버의 종목코드 035420 / 현금흐름표의 타입 값 : CF (재무상태표는 FS, 손익계산서는 BS)
params = {'corp_code': 035420,'fs_type': CF}
r = requests.get(url, params=params)
r.text
```

+ pandas DataFrame 형태로 정제

```python
df = pd.read_json(r.text, orient = 'columns')

# 컬럼 순서 변경
first_col = list(filter(lambda x: x[0] !=str(2) , df.columns))
sec_col = list(filter(lambda x: x[0] ==str(2) , df.columns))

df = df[first_col+sec_col]
# df : 정제 완료된 DataFrame
```

+ 정제 결과

![예시_네이버_rds버전](https://user-images.githubusercontent.com/94692007/142901714-53700060-9d37-4cfc-a72e-feacee7d7c56.JPG)
