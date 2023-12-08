---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Core

Install Angular Auxiliary Library.

```shell
npm install @weplanx/ng
```

## WpxService

* Weplanx Assisted Management Services
* Public
  * assets `string` Assets loading address, default `/assets`
  * upload `AsyncSubject<UploadOption>` Upload configuration status
  * scripts `Map<string, AsyncSubject<void>>` Script loading status

### Set Assets

* **setAssets(v: string): void**
  * v `string` Local or remote path

For example: the assets path is the address https://cdn.xxx.com of CDN distribution, add in `app.component.ts`

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

### Set Upload

* **setUpload(v: UploadOption): void**
  * v `UploadOption` Upload configuration
    * UploadOption
      * type `string` platform
      * url `string` url
      * limit `number` Upload size limit

Just get it from the backend

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

### Create Model

* **setModel\<T>(key: string, api: WpxApi\<T>): WpxModel\<T>**
  * key `string` Index, which automatically associates IndexedDB
  * api `WpxApi<T>` Common API

Initialize a WpxModel

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

### Set Locale

* **setLocale(id: string): void**
  * id `string` identification
    * `zh-Hans` Simplified Chinese
    * `en-US` American English

### Load Script

* **loadScript(key: string, url: string, plugins: string\[]): void**
  * key `string` script identity
  * url `string` script url
  * plugins `string[]` plugins url of script

Example: import external UMD script

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

### Get Values

* **getValues(keys?: string\[]): Observable\<R>**
  * keys `string[]` configuration keys

### Update Values

* **setValues(update: R): Observable\<R>**
  * update `R` Update data

### Delete Values

* **deleteValue(key: string): Observable\<R>**
  * key `string` configuration key

### Transaction

* **transaction(): Observable\<TransactionResult>**

### Commit Transaction

* **commit(txn: string): Observable\<void>**
  * txn `string` Transaction ID

### Get COS Pre-signed

* **cosPresigned(): Observable\<R>**

### Get COS Picture Info

* **cosImageInfo(url: string): Observable\<WpxImageInfo>**
  * url `string` Image object

## WpxStoreService

* Local storage based on IndexedDB

### Set

* **set\<T>(key: string, value: T): Observable\<void>**

### Get

* **get\<T>(key: string): Observable\<T | undefined>**

### Remove

* **remove(key: string): Observable\<void>**

### Clear

* **clear(): Observable\<R>**

## WpxApi\<T>

* Universal API abstract class

### Create

* **create(doc: T, options?: CreateOption\<T>): Observable\<R>**
  * doc `T` data
  * options `CreateOption<T>`
    * xdata `XData` doc format conversion
    * txn `string` transaction ID

### BulkCreate

* **bulkCreate(docs: T\[], options?: CreateOption\<T>): Observable\<R>**
  * docs `T[]` array data
  * options `CreateOption<T>`
    * xdata `XData` docs.$ format conversion
    * txn `string` transaction ID

### Size

* **size(filter: Filter\<T>, options?: FilterOption\<T>): Observable\<number>**
  * filter `Filter<T>`
  * options `FilterOption<T>`
    * xfilter `XFilter<T>` filter format conversion

### Exists

* **exists(filter: Filter\<T>, options?: FilterOption\<T>): Observable\<boolean>**
  * filter `Filter<T>`
  * options `FilterOption<T>`
    * xfilter `XFilter<T>` filter format conversion

### Find

* **find(filter: Filter\<T>, options?: FindOption\<T>): Observable\<FindResult\<T>>**
  * filter `Filter<T>`
  * options `FindOption<T>`
    * keys `Array<keyof AnyDto<T>>` projection
    * sort `Sort<T>` sort
    * page `number` page index
    * pagesize `number` page size
    * xfilter `XFilter<T>` filter format conversion

### FindOne

* **findOne(filter: Filter\<T>, options?: FindOneOption\<T>): Observable\<AnyDto\<T>>**
  * filter `Filter<T>`
  * options `FindOneOption<T>`
    * keys `Array<keyof AnyDto<T>>` projection
    * xfilter `XFilter<T>` filter format conversion

### FindById

* **findById(id: string, options?: FindByIdOption\<T>): Observable\<AnyDto\<T>>**
  * id `string`
  * options `FindByIdOption<T>`
    * keys `Array<keyof AnyDto<T>>` projection

### Update

* **update(filter: Filter\<T>, update: Update\<T>, options?: UpdateOption\<T>): Observable\<R>**
  * filter `Filter<T>`
  * update `Update<T>` update data
  * options `UpdateOption<T>`
    * xfilter `XFilter<T>` filter format conversion
    * xdata `XData` update format conversion
    * txn `string` transaction ID

### UpdateById

* **updateById(id: string, update: Update\<T>, options?: UpdateOneByIdOption\<T>): Observable\<R>**
  * id `string`
  * update `Update<T>` update data
  * options `UpdateOneByIdOption<T>`
    * xdata `XData` update format conversion
    * txn `string` transaction ID

### Replace

* **replace(id: string, doc: T, options?: ReplaceOption\<T>): Observable\<R>**
  * id `string`
  * doc `T` data
  * options `ReplaceOption<T>`
    * xdata `XData` update format conversion
    * txn `string` transaction ID

### Delete

* **delete(id: string, options?: DeleteOption\<T>): Observable\<R>**
  * id `string`
  * options `DeleteOption<T>`
    * txn `string` transaction ID

### BulkDelete

* **bulkDelete(filter: Filter\<T>, options?: BulkDeleteOption\<T>): Observable\<R>**
  * filter `Filter<T>`
  * options `BulkDeleteOption<T>`
    * xfilter `XFilter<T>` filter format conversion
    * txn `string` transaction ID

### Sort

* **sort(key: string, values: string\[], options?: SortOption\<T>): Observable\<R>**
  * key `string` Sort field
  * values `string[]` ID array, array index is sorted
  * options `SortOption<T>`
    * txn `string` transaction ID

## WpxItems\<T>

* Universal tuple abstract class
* Public
  * data `T[]`
  * loading `bool` load state
  * searchText `string` keyword search text
  * checked `boolean` whether all are selected
  * indeterminate `boolean` whether to select part
  * selection `Map<string, T>` selected value

### Check

* **setSelection(data: T): void**
  * data `T`

### Uncheck

* **removeSelection(id: string): void**
  * id `string`

### Checked All

* **setCurrentSelections(checked: boolean): void**
  * checked `boolean`

## WpxModel\<T>

* Universal data model abstract class
* Public
  * data `signal<AnyDto<T>[]>`
  * dataFilter `AnyDto<T>[]` not disabled data
  * loading `signal<boolean>` load state
  * total `number` data total
  * page `number` model page index
  * pagesize `number` model page size
  * advanced `signal<NzDrawerRef | null>` search panel
  * searchText `string` keyword search text
  * keywords `Any[]`
  * checked `boolean` whether all are selected
  * indeterminate `boolean` whether to select part
  * selection `Map<string, T>` selected value

### Ready

* **ready(xfilter?: XFilter): Observable\<R>**
  * xfilter `XFilter` default filter formatting

### Fetch Data

* **fetch(search?: Filter\<T>, sort?: Sort\<T>): Observable\<FindResult\<T>>**
  * search `Filter<T>` custom filter
  * sort `Sort<T>` custom sort

### Check

* **setSelection(data: T): void**
  * data `T`

### Uncheck

* **removeSelection(id: string): void**
  * id `string`

### Checked All

* **setCurrentSelections(checked: boolean): void**
  * checked `boolean`
