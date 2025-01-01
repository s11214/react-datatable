大部分由ChatGpt和Cursor写的，留着自己看的
```markdown:README.md
# React DataTable 组件

一个功能强大、灵活的 React 表格组件，支持多种数据类型、筛选、编辑、批量操作等功能。

## 目录

1. [功能特性](#功能特性)
2. [安装](#安装)
3. [基础用法](#基础用法)
4. [API 文档](#api-文档)
5. [数据类型](#数据类型)
6. [高级功能](#高级功能)
7. [注意事项](#注意事项)

## 功能特性

- 🔄 支持多种数据类型的展示和编辑
- 🔍 强大的筛选系统（按选项/条件筛选）
- ✏️ 单元格选择和批量操作
- 📊 灵活的列配置
- 📱 响应式设计
- 🖼️ 图片上传和预览
- 📥 数据导入导出
- 📊 统计面板
- ↩️ 操作撤销
- 🔒 权限控制

## 安装

```bash
npm install @your-org/react-datatable
```

## 基础用法

```jsx
import DataTable from '@your-org/react-datatable';

const App = () => {
  const columns = [
    {
      field: 'name',
      headerName: '名称',
      type: 'string',
      editable: true
    },
    {
      field: 'price',
      headerName: '价格',
      type: 'currency',
      precision: 2,
      prefix: '￥'
    }
  ];

  const data = [
    { id: '1', name: '商品1', price: '99.99' },
    { id: '2', name: '商品2', price: '199.99' }
  ];

  return (
    <DataTable
      tableName="商品列表"
      columns={columns}
      data={data}
      onCellUpdate={(rowId, field, value) => {
        console.log(`更新: ${rowId}, ${field}, ${value}`);
      }}
    />
  );
};
```

## API 文档

### 组件属性

#### 必需属性

| 属性名 | 类型 | 说明 |
|--------|------|------|
| columns | array | 列定义配置 |
| data | array | 表格数据 |
| tableName | string | 表格名称 |

#### 可选属性

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| tableClassName | string | - | 自定义类名 |
| onBatchOperation | function | - | 批量操作回调 |
| onDelete | function | - | 删除操作回调 |
| onAdd | function | - | 添加操作回调 |
| onUpload | function | - | 上传操作回调 |
| onCellUpdate | function | - | 单元格更新回调 |
| onReload | function | - | 重载数据回调 |
| canDeleteRow | function | () => true | 行删除权限控制 |
| canUpload | boolean | false | 是否允许上传 |
| canBatchMenu | boolean | true | 是否显示批量菜单 |
| canDelete | boolean | true | 是否允许删除 |
| canAdd | boolean | true | 是否允许添加 |
| rowsPerPageOptions | array | [10,20,50] | 分页选项 |
| defaultRowsPerPage | number | 50 | 默认每页行数 |

### 列配置

```typescript
interface Column {
  field: string;          // 字段名（必需）
  headerName: string;     // 列标题（必需）
  type: DataType;         // 数据类型（必需）
  editable?: boolean;     // 是否可编辑
  filterable?: boolean;   // 是否可筛选
  visible?: boolean;      // 是否可见
  required?: boolean;     // 是否必填
  prefix?: string;        // 前缀（数字类型）
  suffix?: string;        // 后缀（数字类型）
  precision?: number;     // 精度（数字类型）
  properties?: Property[]; // 对象属性定义
  selectOptions?: string[]; // 下拉选项
  maxWidth?: string;      // 最大宽度
  separator?: string;     // 分隔符（数组显示）
}
```

## 数据类型

组件支持以下数据类型：

### 基础类型
- `string`: 字符串
- `integer`: 整数
- `float`: 浮点数
- `date`: 日期
- `time`: 时间

### 高级类型
- `currency`: 货币（带前缀和精度）
- `percentage`: 百分比（带后缀和精度）
- `array`: 数组（可自定义分隔符）
- `object`: 对象（支持属性配置）
- `images`: 图片（支持上传和预览）
- `hyperlink`: 超链接
- `select`: 下拉选择

### 类型示例

```jsx
const columns = [
  // 货币类型
  {
    field: 'price',
    headerName: '价格',
    type: 'currency',
    precision: 2,
    prefix: '￥'
  },
  
  // 对象类型
  {
    field: 'priceInfo',
    headerName: '价格信息',
    type: 'object',
    properties: [
      { key: 'current', name: '现价', type: 'currency', prefix: '￥', required: true },
      { key: 'change', name: '涨跌', type: 'string' },
      { key: 'percent', name: '涨跌幅', type: 'percentage', precision: 2 }
    ]
  },
  
  // 图片类型
  {
    field: 'images',
    headerName: '图片',
    type: 'images',
    maxWidth: '100px'
  }
];
```

## 高级功能

### 筛选系统

支持两种筛选模式：
1. 按选项筛选
2. 按条件筛选（支持多条件组合）

```jsx
// 条件筛选示例
{
  operator: 'AND',
  conditions: [
    { field: 'price', operator: 'greater_than', value: 100 },
    { field: 'stock', operator: 'not_empty' }
  ]
}
```

### 单元格选择

支持以下选择方式：
- 单击选择
- 拖动选择
- Shift + 点击选择范围
- Ctrl/Cmd + 点击多选

### 批量操作

```jsx
const batchOperations = [
  { label: '批量删除', value: 'delete' },
  { label: '批量导出', value: 'export' }
];

<DataTable
  batchOperations={batchOperations}
  onBatchOperation={(operation, selectedData) => {
    console.log(`执行${operation}操作`, selectedData);
  }}
/>
```

### 统计面板

自动计算选中单元格的：
- 总数
- 非空单元格数
- 数值类型的总和
- 数值类型的平均值

## 注意事项

### 1. 性能优化
- 大数据量时建议使用分页
- 避免频繁更新不必要的状态
- 合理设置 rowsPerPageOptions

### 2. 数据验证
- 必填字段不能为空
- 数值类型需符合格式要求
- 对象类型的必填属性必须有值

### 3. 图片处理
- 支持格式：JPG, PNG, GIF
- 大小限制：5MB
- 建议使用压缩后的图片

### 4. 权限控制
- 可控制整表的增删改查权限
- 支持行级别的删除权限
- 支持列级别的编辑权限

### 5. 键盘操作
- Delete/Backspace: 删除选中单元格内容
- Ctrl/Cmd + Z: 撤销操作
- 方向键: 移动选中单元格
- Enter: 确认编辑
- Esc: 取消编辑

## 贡献

欢迎提交 Issue 和 Pull Request！

## 许可

MIT License
```

这个 README.md 文件提供了组件的完整文档，包括：
1. 基本介绍和功能特性
2. 安装和基础用法
3. 详细的 API 文档
4. 支持的数据类型说明
5. 高级功能的使用说明
6. 重要注意事项

文档结构清晰，包含代码示例，便于用户快速上手和参考。需要补充或修改的部分，请告诉我。
