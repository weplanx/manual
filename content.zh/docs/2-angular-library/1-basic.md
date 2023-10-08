---
weight: 1
title: 基本功能
---

# 基本功能

安装 Angular 辅助库

```shell
npm install @weplanx/ng
```

## WpxService

- Weplanx 辅助管理服务
- 公有属性：
  - assets `string` 资源加载地址，默认 `/assets`
  - upload `AsyncSubject<UploadOption>` 上传配置状态
  - scripts `Map<string, AsyncSubject<void>>` 脚本加载状态

### 设置资源路径

- setAssets(v: string): void
  - v `string` 本地或远程路径

例如：资源路径是 CDN 分发的地址 https://cdn.xxx.com，在 `app.component.ts` 中加入

```typescript
@Component({
  selector: 'app-root',
  template: ` <router-outlet></router-outlet> `
})
export class AppComponent implements OnInit {
  constructor(
    private app: AppService,
    private wpx: WpxService,
    private icon: NzIconService
  ) {}

  ngOnInit(): void {
    this.app.ping().subscribe(() => {
      console.debug('XSRF:ok');
    });
    this.icon.changeAssetsSource(environment.cdn);
    this.wpx.setAssets(environment.cdn);
  }
}
```

### 设置上传配置

- setUpload(v: UploadOption): void
  - v `UploadOption` 上传配置
    - UploadOption
      - type `string` 平台
      - url `string` 地址
      - limit `number` 上传大小

只需要从后端获取并设置即可

```typescript
@Component({
  selector: 'app-root',
  template: ` <router-outlet></router-outlet> `
})
export class AppComponent implements OnInit {
  constructor(
    private app: AppService,
    private wpx: WpxService,
    private icon: NzIconService
  ) {}

  ngOnInit(): void {
    this.app.getUploadOption().subscribe(v => {
      this.wpx.setUpload(v);
    });
  }
}
```

### 创建 Model

- setModel\<T>(key: string, api: WpxApi\<T>): WpxModel\<T>
  - key `string` 索引，会自动关联本地存储
  - api ` WpxApi<T>` 通用接口

初始化一个 WpxModel

```typescript
@Component({
  selector: 'app-admin-users',
  templateUrl: './users.component.html'
})
export class UsersComponent implements OnInit {
  model!: WpxModel<User>;

  constructor(
    private wpx: WpxService,
    public users: UsersService
  ) {}

  ngOnInit(): void {
    this.model = this.wpx.setModel<User>('users', this.users);
    this.model.ready().subscribe(() => {
      this.getData();
    });
  }

  getData(refresh = false): void {
    if (refresh) {
      this.model.page = 1;
    }
    this.model.fetch({}).subscribe(() => {
      console.debug('fetch:ok');
    });
  }
}
```

### 设置显示语言

- setLocale(id: string): void
  - id `string` 语言标识
    - `zh-Hans` 中文
    - `en-US` 英文

### 加载脚本

- loadScript(key: string, url: string, plugins: string[]): void
  - key `string` 脚本唯一标识
  - url `string` 脚本路径
  - plugins `string[]` 附件脚本路径

例如：引入外部 UMD 脚本

```typescript
@Component({
  selector: 'app-root',
  template: ` <router-outlet></router-outlet> `
})
export class AppComponent implements OnInit {
  constructor(
    private app: AppService,
    private wpx: WpxService,
    private icon: NzIconService
  ) {}

  ngOnInit(): void {
    this.wpx.loadScript('cropperjs', 'https://cdn.jsdelivr.net/npm/cropperjs@1.6.0/dist/cropper.min.js', []);
    this.wpx.loadScript('editorjs', 'https://cdn.jsdelivr.net/npm/@editorjs/editorjs@2.27.2/dist/editorjs.umd.min.js', [
      'https://cdn.jsdelivr.net/npm/@editorjs/paragraph@2.10.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/header@2.7.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/delimiter@1.3.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/underline@1.1.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/nested-list@1.3.0/dist/nested-list.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/checklist@1.5.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/table@2.2.2/dist/table.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/quote@2.5.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/code@2.8.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/marker@1.3.0/dist/bundle.min.js',
      'https://cdn.jsdelivr.net/npm/@editorjs/inline-code@1.4.0/dist/bundle.min.js'
    ]);
  }
}
```

### 获取动态配置

- getValues(keys?: string[]): Observable<R>
  - keys `string[]` 配置键

### 更新动态配置

- setValues(update: R): Observable<R>
  - update `R` 更新数据

### 删除动态配置

- deleteValue(key: string): Observable<R>
  - key `string` 配置键

### 发起事务

- transaction(): Observable<TransactionResult>

### 提交事务

- commit(txn: string): Observable<void>
  - txn `string` 事务 ID

### 获取 COS 预签名

- cosPresigned(): Observable<R>

### 获取 COS 中指定图片信息

- cosImageInfo(url: string): Observable<WpxImageInfo>
  - url `string` 图片对象名

## WpxStoreService

- IndexedDB 本地存储服务

### 设置

- set\<T>(key: string, value: T): Observable<void>
  - key `string` 键名
  - value `T` 数值

### 获取

- get\<T>(key: string): Observable<T | undefined>
  - key `string` 键名

### 移除

- remove(key: string): Observable<void>
  - key `string` 键名

### 清空

- clear(): Observable<void>

## WpxApi\<T>

- 通用接口抽象类

### 新增资源

- create(doc: T, options?: CreateOption\<T>): Observable\<R>
  - doc `T` 资源数据
  - options `CreateOption<T>` 配置
    - xdata `XData` doc 格式转换
    - txn `string` 事务 ID

### 批量新增资源

- bulkCreate(docs: T[], options?: CreateOption\<T>): Observable\<R>
  - docs `T[]` 资源数组数据
  - options `CreateOption<T>` 配置
    - xdata `XData` docs.$ 格式转换
    - txn `string` 事务 ID

### 获取资源大小

- size(filter: Filter\<T>, options?: FilterOption\<T>): Observable\<number>
  - filter `Filter<T>` 筛选条件
  - options `FilterOption<T>`
    - xfilter `XFilter<T>` filter 格式转换

### 判断是否存在

- exists(filter: Filter\<T>, options?: FilterOption\<T>): Observable\<boolean>
  - filter `Filter<T>` 筛选条件
  - options `FilterOption<T>`
    - xfilter `XFilter<T>` filter 格式转换

### 获取匹配资源

- find(filter: Filter\<T>, options?: FindOption\<T>): Observable<FindResult\<T>\>
  - filter `Filter<T>` 筛选条件
  - options `FindOption<T>`
    - keys `Array<keyof AnyDto<T>>` 投影字段
    - sort `Sort<T>` 排序
    - page `number` 分页索引
    - pagesize `number` 分页大小
    - xfilter `XFilter<T>` filter 格式转换

### 获取单个资源

- findOne(filter: Filter\<T>, options?: FindOneOption\<T>): Observable<AnyDto\<T>\>
  - filter `Filter<T>` 筛选条件
  - options `FindOneOption<T>`
    - keys `Array<keyof AnyDto<T>>` 投影字段
    - xfilter `XFilter<T>` filter 格式转换

### 获取指定 ID 的资源

- findById(id: string, options?: FindByIdOption\<T>): Observable<AnyDto\<T>\>
  - id `string` 资源 ID
  - options `FindByIdOption<T>`
    - keys `Array<keyof AnyDto<T>>` 投影字段

### 更新匹配资源

- update(filter: Filter\<T>, update: Update\<T>, options?: UpdateOption\<T>): Observable\<R>
  - filter `Filter<T>` 筛选条件
  - update `Update<T>` 更新资源
  - options `UpdateOption<T>`
    - xfilter `XFilter<T>` filter 格式转换
    - xdata `XData` update 格式转换
    - txn `string` 事务 ID

### 更新指定 ID 的资源

- updateById(id: string, update: Update\<T>, options?: UpdateOneByIdOption\<T>): Observable\<R>
  - id `string` 资源 ID
  - update `Update<T>` 更新资源
  - options `UpdateOneByIdOption<T>`
    - xdata `XData` update 格式转换
    - txn `string` 事务 ID

### 替换指定 ID 的资源

- replace(id: string, doc: T, options?: ReplaceOption\<T>): Observable\<R>
  - id `string` 资源 ID
  - doc `T` 数据资源
  - options `ReplaceOption<T>`
    - xdata `XData` update 格式转换
    - txn `string` 事务 ID

### 删除指定 ID 的资源

- delete(id: string, options?: DeleteOption\<T>): Observable\<R>
  - id `string` 资源 ID
  - options `DeleteOption<T>`
    - txn `string` 事务 ID

### 批量删除匹配资源

- bulkDelete(filter: Filter\<T>, options?: BulkDeleteOption\<T>): Observable\<R>
  - filter `Filter<T>` 筛选条件
  - options `BulkDeleteOption<T>`
    - xfilter `XFilter<T>` filter 格式转换
    - txn `string` 事务 ID

### 排序

- sort(key: string, values: string[], options?: SortOption\<T>): Observable\<R>
  - key `string` 排序字段
  - values `string[]` 资源 ID 数组，数组索引即排序
  - options `SortOption<T>`
    - txn `string` 事务 ID

## WpxItems\<T>

- 通用元组抽象类
- 公有属性
  - data `T[]` 数据
  - loading `bool` 加载状态
  - searchText `string` 关键词搜索
  - checked `boolean` 是否全部选中
  - indeterminate `boolean` 是否选中部分
  - selection `Map<string, T>` 选中元素

### 设置选中

- setSelection(data: T): void
  - data `T` 数据

### 取消选中

- removeSelection(id: string): void
  - id `string` 数据 ID

### 设置所有选中状态

- setCurrentSelections(checked: boolean): void
  - checked `boolean` 状态

## WpxModel\<T>

- 通用数据模型抽象类
- 公有属性
  - data `signal<AnyDto<T>[]>` 数据
  - dataFilter `AnyDto<T>[]` 未禁用数据
  - loading `signal<boolean>` 加载状态
  - total `number` 总数
  - page `number` 分页索引
  - pagesize `number` 分页大小
  - advanced `signal<NzDrawerRef | null>` 搜索面板
  - searchText `string` 关键词搜索
  - keywords `Any[]` 关键词
  - checked `boolean` 是否全部选中
  - indeterminate `boolean` 是否选中部分
  - selection `Map<string, T>` 选中元素

### 初始化完成

- ready(xfilter?: XFilter): Observable<WpxModelStore>
  - xfilter `XFilter` 筛选条件格式化

### 获取数据

- fetch(search?: Filter<T>, sort?: Sort<T>): Observable<FindResult<T>>
  - search `Filter<T>` 自定筛选条件
  - sort `Sort<T>` 自定排序

### 设置选中

- setSelection(data: T): void
  - data `T` 数据

### 取消选中

- removeSelection(id: string): void
  - id `string` 数据 ID

### 设置所有选中状态

- setCurrentSelections(checked: boolean): void
  - checked `boolean` 状态
