当遇到一个 sql.ErrNoRows 的时候，不应该 Wrap 这个 error，抛给上层。当进行数据查询时，不管数据查询结果是否为空，我们都需要发出返回，我们可以进行降级处理，做好日志记录,返回nil

demo:

```go
func getData() (*User, error) {
	var user User
	var selectError error = nil
	db, _ := sql.Open("sqlite3", "test.db")
	selectError = db.QueryRow("select * from users where age>?", 100).Scan(&user.Name, &user.Age, &user.Birthday)
	switch selectError {
	case sql.ErrNoRows:
		log.Printf("%s | %d | %s", "E:\\geekTime\\src\\sqlite\\select.go", "25", sql.ErrNoRows)
		return nil,nil
	default:
		return nil,selectError
	}
	return &user, selectError
}
```

