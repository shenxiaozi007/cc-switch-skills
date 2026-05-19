# Migration Skeleton（本仓库）

## 新增表模板
```php
public function up(): void
{
    Schema::create('xxx_table', function (Blueprint $table) {
        $table->comment('模块 - 表含义');
        // 主键写法先参考同模块历史；新表默认可用 $table->id()。
        $table->id();
        $table->string('简化的表名_no', 64)->default('')->comment('xx编号')->unique('索引名');

        // 业务字段写在这里；字段类型、长度、默认值优先对齐同模块历史。
        $table->string('ip', 128)->default('')->comment('登录ip')->index('ip');
        $table->string('status', 32)->default('')->comment('状态');
        $table->unsignedInteger('business_at')->default(0)->comment('业务时间，时间戳');
        $table->decimal('amount', 16, 2)->default(0)->comment('金额');

        $table->unsignedInteger('add_time')->default(0)->comment('添加时间');
        $table->unsignedInteger('last_update_time')->default(0)->comment('最后更新时间');
        $table->timestamps();
        $table->softDeletes();
    });
}

public function down(): void
{
    Schema::dropIfExists('xxx_table');
}
```

## 新增表示例
```php
public function up(): void
{
    Schema::create('ord_restrict', function (Blueprint $table) {
        $table->comment('技加工 - 项目开单限制');
        $table->id();
        $table->string('ord_restrict_no', 64)->default('')->comment('xx编号')->unique('ord_restrict_no');

        $table->unsignedInteger('add_time')->default(0)->comment('添加时间');
        $table->unsignedInteger('last_update_time')->default(0)->comment('最后更新时间');
        $table->timestamps();
        $table->softDeletes();
    });
}

public function down(): void
{
    Schema::dropIfExists('ord_restrict');
}
```

## 改表模板
```php
public function up(): void
{
    Schema::table('xxx_table', function (Blueprint $table) {
        $table->string('new_col', 64)->default('')->comment('新字段')->index('idx_new_col');
    });
}

public function down(): void
{
    Schema::table('xxx_table', function (Blueprint $table) {
        $table->dropIndex('idx_new_col');
        $table->dropColumn('new_col');
    });
}
```

## 历史兼容说明
- 同模块旧表如果使用 `$table->increments('id')`、`$table->bigIncrements('id')`、显式 `timestamp('created_at')` / `timestamp('updated_at')` / `string('deleted_at', 45)`，维护同表族时优先保持局部一致。
- 新增字段带索引时，`down()` 先删除索引再删字段；不能安全回滚时要在交付说明里明确原因。
- 历史 migration 存在空 `down()`、拼写错误文件名和表名不一致问题，新文件不要延续。

## 指定执行
```bash
php artisan migrate --path=/database/migrations/完整文件名.php
```
