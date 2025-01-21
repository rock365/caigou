这是一个轻量级的PHP全文检索类库，完全免费，可用于中文内容的全文检索，基于倒排索引结构和ngram分词开发，引入即可使用。如果你的文章不多，搜索场景简单，那么这个插件对你来说非常适合。

接口包括：创建、删除索引库，导入数据、构建索引、搜索。注意，此引擎不适合大量数据。


## 引入文件
```php
require_once 'yourdirname/caigou/Caigou.php';
```

## 导入数据

```php
$tableName = 'test';//索引库名称
$Caigou = new \CaigouSearch\Core\Caigou($tableName);
// 创建索$tableName引库
$Caigou->createIndex();

// 导入数据 从数据库查询数据
$where = [
    // ...
];
$result = Db::table('dev')->where($where)->select();
foreach ($result as $v) {
    $id = $v['id'];// 主键
    $title = $v['title']; // 需要索引的字段
    $Caigou->indexer($id, $title);
}
// 循环导入完毕，批量写入文件
$Caigou->batchWrite();
```

## 构建索引

```php
// 循环导入数据
// ...
// 数据批量写入完成，开始构建索引数据
$Caigou->buildIndex();
```


## 开始搜索
```php
// 开始搜索，返回id集合
$tableName = 'test';//索引库名称
$Caigou = new \CaigouSearch\Core\Caigou($tableName);
$query = [
    'query'=>'脚本语言',//搜索内容
    'page'=>1,//第几页
    'list_rows'=>10,//每页多少条
];
// 调用搜索接口
$res = $Caigou->search($query);
// 命中信息 id=>命中个数
$hits = $res['hits'];
// 主键列表
$ids = $res['ids'];
// 构造查询sql语句
$ids = implode(',',$ids);
$sql = "select * from tablename where id in($ids)";
//...
```


## 删除索引库
```php
// 删除test索引库
$tableName = 'test';//索引库名称
$Caigou = new \CaigouSearch\Core\Caigou($tableName);
$Caigou->delIndex();
```


=======================================

如果菜狗搜索无法满足你的需求，这里还有PHP全文检索专业解决方案：[https://github.com/rock365/windsearch](https://github.com/rock365/windsearch)

商业需求请联系vx：azg555666

![](https://github.com/rock365/img/blob/main/afe22e05ee161083cfbd1336f7facd2.jpg)

