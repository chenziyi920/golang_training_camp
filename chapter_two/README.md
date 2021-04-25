## 第二周作业

### 题目
我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

### 思路
在业务场景中，对于“无数据”的err应该由logic层根据自身业务逻辑处理，因此dao层在遇到err时，不应该对其断言处理，在Wrap相关sql后抛给上层即可

### 代码
##### dao层：
```go
func Query() (*result, error) {
    //do query
    if err != nil {
        return nil, errors.Wrap(err, fmt.Sprintf("sql: %s error:", sql))
    }
    //do something
}
```
##### logic层
```go
func GetXXX() error {
    result, err := dao.Query()
    //由自身逻辑决定是否对err做断言处理
    if err != nil && !errors.Is(err, sql.ErrNoRows) {
        return err
    }
    //do something
}
```
