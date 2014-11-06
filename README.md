# goimports-err

There is an error in go imports. Suppose we have a package:

```Go
// foo.go
package foo

import logging "github.com/op/go-logging"

var log = logging.Logger("foo")
```

```Go
// bar.go
package foo

func main() {
  log.Error("whoops")
}
```

And that we also have a package `.../log`. goimports will generate diff:

```
goimports -d .
```
```diff
diff bar.go gofmt/bar.go
--- /var/folders/7d/rkm5lnyd1sg244wxs6q_gfl40000gn/T/gofmt111161908 2014-11-06 12:34:31.000000000 -0800
+++ /var/folders/7d/rkm5lnyd1sg244wxs6q_gfl40000gn/T/gofmt137239555 2014-11-06 12:34:31.000000000 -0800
@@ -1,5 +1,7 @@
 package foo

+import "github.com/getlantern/flashlight/log"
+
 func main() {
  log.Error("whoops")
 }
```

goimports fails to check the other files for a variable before importing. Note
that if the `main` function is in `foo.go`, goimports now correctly finds the
variable and generates no changes.
