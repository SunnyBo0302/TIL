## Try-with-resource

https://androphil.tistory.com/763

매우 훌륭한 기능인 듯

### < java 7
```
Connection conn = ~~
try {
    ...
} finally {
    conn.close()
}
```

### java 7
```
try (Connection conn = ~~ ; Stmt = ~~) { // this should implement AutoClosable
    ...
    try (ResultSet = ~~) {
        ...    
    }
}
```

### java 9
```
Connection conn = ~~;
Statement stmt = ~~;
ResultSet rs = ~~;
try (conn; stmt; rs) {
    ...
}
```
