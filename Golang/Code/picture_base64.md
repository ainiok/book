## 图片转base64

```blade
file, _ := os.Open("20190220020008_83722.jpg") // 只能打开本地文件
defer file.Close()

stats, _ := file.Stat()

var size int64 = stats.Size()
data := make([]byte, size)

bufr := bufio.NewReader(file)
_, _ = bufr.Read(data)

encoding := base64.StdEncoding.EncodeToString(data)

database64 := strings.Join([]string{ "data:image/jpeg;base64" ,encoding}, ",")

fmt.Println(database64)
```