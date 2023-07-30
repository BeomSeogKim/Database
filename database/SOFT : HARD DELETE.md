# Soft delete & Hard delete

### Soft delete (논리적 삭제)

논리적 삭제의 경우 삭제 여부를 알 수 있는 Column이 별도로 존재한다.(이하 del_col)

데이터를 삭제할 때 row에 있는 값들을 다 지우는 것이 아닌, del_col에 삭제 여부를 기입해 삭제하는 방식이다. 

`UPDATE` SQL을 사용해 삭제를 진행한다. 



[삭제 전]

|  id  |  name  | deleted |
| :--: | :----: | :-----: |
|  1   | Tommy  |  false  |
|  2   | Robert |  false  |

Robert 삭제 

|  id  |  name  | deleted |
| :--: | :----: | :-----: |
|  1   | Tommy  |  false  |
|  2   | Robert |  true   |



### Hard delete (물리적 삭제)

물리적 삭제의 경우 Table의 row를 직접적으로 삭제하는 방식이다. 

`DELETE` SQL을 사용해 삭제를 진행한다. 



[삭제 전]

|  id  |  name  |
| :--: | :----: |
|  1   | Tommy  |
|  2   | Robert |

Robert 삭제 

|  id  | name  |
| :--: | :---: |
|  1   | Tommy |

