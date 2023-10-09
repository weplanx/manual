---
weight: 3
title: 集成组件
---

# 集成组件

## 模型关键词

```html
<wpx-keyword></wpx-keyword>
```

- @Input({ required: true }) wpxModel!: WpxModel\<T> 通用模型
- @Input({ required: true }) wpxKeys!: string[] 关键词字段
- @Input() wpxWidth: number 宽度，默认 `220` px
- @Input() wpxPlaceholder: string 提示填充，默认 `关键词搜索`
- @Output() wpxSubmit: EventEmitter\<void> 提交事件

例如：项目 experiment/table ，在动态列表或动态表格中使用

```html
<wpx-keyword
  [wpxModel]="model"
  [wpxKeys]="['no', 'name', 'account']"
  (wpxSubmit)="getData()"
  wpxPlaceholder="订单号、姓名、账户"
></wpx-keyword>
```

## 表格配件

```html
<wpx-toolbox></wpx-toolbox>
```

- @Input({ required: true }) wpxModel!: WpxModel\<T> 通用模型
- @Input() wpxSearchHeight: number 高度，默认：`340` px
- @Input() wpxSearchForm?: FormGroup 自定搜索 FormGroup
- @Input() wpxSearchTitle?: TemplateRef\<Any> 自定搜索面板的标题，默认 `高级检索`
- @Output() wpxClear: EventEmitter\<void> 清空条件事件
- @Output() wpxRefresh: EventEmitter\<void> 数据刷新事件

例如：项目 experiment/table ，在动态列表或动态表格中使用

{{<tabs "toolbox">}}
{{<tab "table.component.html">}}

```html
<wpx-toolbox [wpxModel]="model" [wpxSearchForm]="form" (wpxClear)="clear()" (wpxRefresh)="getData()">
  <form *ngIf="form" nz-form id="search" [formGroup]="form" (wpxSubmit)="search($event)">
    <nz-row [nzGutter]="[16, 16]">
      <nz-col [nzSpan]="6">
        <nz-form-item>
          <nz-form-label>订单号</nz-form-label>
          <nz-form-control>
            <input nz-input formControlName="no" />
          </nz-form-control>
        </nz-form-item>
      </nz-col>
      <nz-col [nzSpan]="6">
        <nz-form-item>
          <nz-form-label>姓名</nz-form-label>
          <nz-form-control>
            <input nz-input formControlName="name" />
          </nz-form-control>
        </nz-form-item>
      </nz-col>
      <nz-col [nzSpan]="6">
        <nz-form-item>
          <nz-form-label>账户</nz-form-label>
          <nz-form-control>
            <input nz-input formControlName="account" />
          </nz-form-control>
        </nz-form-item>
      </nz-col>
    </nz-row>
  </form>
</wpx-toolbox>
```

{{</tab>}}
{{<tab "table.component.ts">}}

```typescript
@Component({
  selector: 'x-table',
  templateUrl: './table.component.html'
})
export class TableComponent implements OnInit {
  model!: WpxModel<Order>;
  form!: FormGroup;
  filter: Filter<Order> = {};

  constructor(
    private wpx: WpxService,
    private fb: FormBuilder,
    private modal: NzModalService,
    private message: NzMessageService,
    public orders: OrdersService
  ) {}

  ngOnInit(): void {
    this.form = this.fb.group({
      no: [],
      name: [],
      account: []
    });
    this.model = this.wpx.setModel<Order>('exp', this.orders);
    this.model.ready().subscribe(() => {
      this.getData();
    });
  }

  getData(refresh = false): void {
    if (refresh) {
      this.model.page = 1;
    }
    this.model.fetch(this.filter).subscribe(() => {
      console.debug('fetch:ok');
    });
  }

  clear(): void {
    this.form.reset();
    this.filter = {};
    this.getData();
  }

  search(data: Any): void {
    for (const [k, v] of Object.entries(data)) {
      if (v) {
        this.filter[k] = { $regex: `${v}` };
      }
    }
    this.getData();
  }
}
```

{{</tab>}}
{{</tabs>}}

效果如图：

![](/images/angular/toolbox.gif)

## 动态表格

```html
<wpx-table></wpx-table>
```

- @Input({ required: true }) wpxModel!: WpxModel\<T> 通用模型
- @Input({ required: true }) wpxColumns!: WpxColumn\<T>[] 表格列定义
- @Input({ required: true }) wpxAction!: TemplateRef<{ $implicit: AnyDto<T> }> 操作自定模板
- @Input() wpxTitle?: TemplateRef\<void> 标题自定模板
- @Input() wpxExtra?: TemplateRef\<void> 附加自定模板
- @Input() wpxX?: string 固定列宽
- @Input() wpxBodyStyle: NgStyleInterface | null 自定卡片样式，默认 `{ height: 'calc(100% - 64px)' }`
- @Output() wpxChange: EventEmitter\<void> 更新事件

例如：项目 experiment/table 有完整的演示，效果如图：

![](/images/angular/table.gif)

## 头像上传

```html
<wpx-upload-avatar></wpx-upload-avatar>
```

- @Input() wpxExt?: string 扩展名
- @Input() wpxAccept: string[] 允许类型
- @Input() wpxFallback!: string 加载中图片

在表单中使用

```html
<wpx-upload-avatar
  wpxExt="image"
  [wpxAccept]="['image/jpeg', 'image/png']"
  [wpxFallback]="['assets', 'photon.svg'] | wpxAssets"
  formControlName="avatar"
>
</wpx-upload-avatar>
```

## 上传进度列表

批量上传并返回进度条的组件

```html
<wpx-upload-transport></wpx-upload-transport>
```

- @Input() wpxExt?: string 扩展名
- @Input() wpxAccept: string[] = [] 允许类型
- @Output() wpxChange: EventEmitter<Transport[]> 传输状态
  - Transport
    - name `string` 名称
    - percent `number` 进度
    - file `NzUploadFile` 上传对象

在资源管理器中使用

```html
<wpx-upload-transport
  *nzSpaceItem
  [wpxExt]="ext"
  [wpxAccept]="accept"
  (wpxChange)="upload($event)"
></wpx-upload-transport>
```

效果如图：

![](/images/angular/transport.gif)

## 富文本编辑器

WpxRichtextComponent 集成 [editorjs](https://editorjs.io/) 块风格编辑器

```html
<wpx-richtext></wpx-richtext>
```

- @Input() wpxPlaceholder?: string 提示填充
- @Input() wpxFallback?: string 加载中图片
- @Input() wpxPictures?: (done: ResolveDone) => void 图库自定义
- @Input() wpxVideos?: (done: ResolveDone) => void 视频自定义

使用他需要载入 UMD 脚本，同时也支持他的插件，例如：

```typescript
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
```

组件预留了图库与视频插入的自定义方式，可以对该组件进行包裹自定，例如在 app/common/components 定义一个 `RichtextComponent`

```typescript
@Component({
  selector: 'app-richtext',
  templateUrl: './richtext.component.html',
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: forwardRef(() => RichtextComponent),
      multi: true
    }
  ],
  encapsulation: ViewEncapsulation.None,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class RichtextComponent implements ControlValueAccessor {
  @ViewChild('footerRef') footerRef!: TemplateRef<Any>;
  @ViewChild(WpxRichtextComponent) richtext!: WpxRichtextComponent;

  placeholder = $localize`直接输入正文`;
  value: RichtextData | null = null;
  modalRef?: NzModalRef<WpxFilebrowserComponent<Any>>;

  onChanged!: (value: Any) => void;
  private onTouched!: () => void;

  constructor(
    private modal: NzModalService,
    private wpx: WpxService,
    @Optional() private pictures: PicturesService,
    @Optional() private videos: VideosService
  ) {}

  registerOnChange(fn: Any): void {
    this.onChanged = fn;
  }

  registerOnTouched(fn: Any): void {
    this.onTouched = fn;
  }

  writeValue(v: RichtextData): void {
    this.value = v;
  }

  get instance(): WpxFilebrowserComponent<WpxFile> {
    return this.modalRef?.componentInstance as WpxFilebrowserComponent<Any>;
  }

  openPictures = (done: ResolveDone): void => {
    this.modalRef = this.modal.create<WpxFilebrowserComponent<WpxFile>, WpxFilebrowserInput<WpxFile>>({
      nzClosable: false,
      nzBodyStyle: { height: '640px', padding: '8px 24px 24px' },
      nzWidth: 1200,
      nzContent: WpxFilebrowserComponent,
      nzData: {
        api: this.pictures,
        type: 'picture',
        fallback: this.richtext.wpxFallback!
      },
      nzFooter: this.footerRef,
      nzOnOk: instance => {
        const data = [...instance.ds.selection.values()][0];
        done({
          assets: this.wpx.assets,
          url: data.url + (!data.query ? '' : '?' + data.query)
        });
      }
    });
  };

  openVideos = (done: ResolveDone): void => {
    this.modalRef = this.modal.create<WpxFilebrowserComponent<WpxFile>, WpxFilebrowserInput<WpxFile>>({
      nzClosable: false,
      nzBodyStyle: { height: '640px', padding: '8px 24px 24px' },
      nzWidth: 1200,
      nzContent: WpxFilebrowserComponent,
      nzData: {
        api: this.videos,
        type: 'video',
        fallback: this.richtext.wpxFallback!
      },
      nzFooter: this.footerRef,
      nzOnOk: instance => {
        const data = [...instance.ds.selection.values()][0];
        done({
          assets: this.wpx.assets,
          url: data.url + (!data.query ? '' : '?' + data.query)
        });
      }
    });
  };
}
```

然后再表单中使用

```html
<app-richtext formControlName="content"> </app-richtext>
```

## 通用分类

用于实现功能重复的分类套件，集成业务逻辑有抽屉分类列表与表单编辑等

```html
<wpx-categories></wpx-categories>
```

- @Input({ required: true }) wpxType!: string 数据库集合 categories 的 type 字段，可以针对 picture、video 等

## 资源管理器

当前实现图库与视频库的集成，目前需 COS 对象存储支持

```html
<wpx-filebrowser></wpx-filebrowser>
```

- @Input({ required: true }) wpxApi!: WpxApi\<T> 通用接口
- @Input({ required: true }) wpxType!: WpxFileType 类型，目前可以是 `picture` 或 `video`
- @Input({ required: true }) wpxFallback!: string 加载图片
- @Input() wpxForm?: (doc: AnyDto\<T>) => void 表单
- @Input() wpxTitle?: TemplateRef\<void> 标题
- @Input() wpxExtra?: TemplateRef\<void> 附加

因为每个项目情况可能不同，为灵活组件只预设了主要功能，所以在实际使用中必须进行包裹组装自定，具体可查看 console 项目中的 app/filebrowser/\*。

此外还包含一个 input 表单组件，同样需要进行包裹自定，具体可查看 console 项目中的 app/common/components/filebrowser-input

```html
<wpx-filebrowser-input></wpx-filebrowser-input>
```

- @Input({ required: true }) wpxValue!: string[] 路径值
- @Input({ required: true }) wpxApi!: WpxApi\<T> 接口
- @Input({ required: true }) wpxType!: WpxFileType 类型
- @Input({ required: true }) wpxFallback!: string 加载中图片
- @Input() wpxHeight: number 高度
- @Input() wpxWidth: number 宽度
- @Input() wpxLimit?: number 限制
- @Output() wpxValueChange: EventEmitter<string[]> 变更状态

选择图库或视频库等将弹出对话框进行选择，如图：

{{<tabs "filebrowser">}}
{{<tab "对话框">}}
![](/images/angular/filebrowser-1.png)
{{</tab>}}
{{<tab "展示">}}
![](/images/angular/filebrowser-2.png)
{{</tab>}}
{{<tab "排序">}}
![](/images/angular/filebrowser-3.png)
{{</tab>}}
{{</tabs>}}

最后可以将其接入富文本编辑器的预留自定

```typescript
openPictures = (done: ResolveDone): void => {
  this.modalRef = this.modal.create<WpxFilebrowserComponent<WpxFile>, WpxFilebrowserInput<WpxFile>>({
    nzClosable: false,
    nzBodyStyle: { height: '640px', padding: '8px 24px 24px' },
    nzWidth: 1200,
    nzContent: WpxFilebrowserComponent,
    nzData: {
      api: this.pictures,
      type: 'picture',
      fallback: this.richtext.wpxFallback!
    },
    nzFooter: this.footerRef,
    nzOnOk: instance => {
      const data = [...instance.ds.selection.values()][0];
      done({
        assets: this.wpx.assets,
        url: data.url + (!data.query ? '' : '?' + data.query)
      });
    }
  });
};

openVideos = (done: ResolveDone): void => {
  this.modalRef = this.modal.create<WpxFilebrowserComponent<WpxFile>, WpxFilebrowserInput<WpxFile>>({
    nzClosable: false,
    nzBodyStyle: { height: '640px', padding: '8px 24px 24px' },
    nzWidth: 1200,
    nzContent: WpxFilebrowserComponent,
    nzData: {
      api: this.videos,
      type: 'video',
      fallback: this.richtext.wpxFallback!
    },
    nzFooter: this.footerRef,
    nzOnOk: instance => {
      const data = [...instance.ds.selection.values()][0];
      done({
        assets: this.wpx.assets,
        url: data.url + (!data.query ? '' : '?' + data.query)
      });
    }
  });
};
```
