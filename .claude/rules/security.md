# 安全规范

## API 安全

### Controller 参数校验

**必填参数检查**：
```java
@GetMapping("/communities/{id}")
public ResponseEntity<?> getCommunity(@PathVariable Long id) {
    if (id == null || id <= 0) {
        return ResponseEntity.badRequest().body("无效的 ID");
    }
    // ...
}
```

## 前端安全

### 输入验证

**搜索参数做长度限制**：
```typescript
// 前端限制输入长度
if (keyword.length > 100) {
  keyword = keyword.slice(0, 100);
}
```

### 敏感数据处理

- 不在前端存储 Token、Cookie
- API 错误响应不暴露堆栈信息

## 爬虫安全

### Cookie 存储

- Cookie 文件存储在 `scraper/data/` 目录下，仅在需要时使用

## 数据库安全

### SQL 注入防护

JPA/Hibernate 自动参数化查询，但需注意：

**安全用法**：
```java
@Query("SELECT c FROM Community c WHERE c.name = :name")
Community findByName(@Param("name") String name);
```

**避免原生 SQL 拼接**：
```java
// 不安全
String sql = "SELECT * FROM community WHERE name = '" + name + "'";

// 安全
@Query(value = "SELECT * FROM community WHERE name = :name", nativeQuery = true)
```
