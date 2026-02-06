---
title: 前端架构
description: DataMate React 前端架构设计
weight: 2
---

{{% pageinfo %}}
DataMate 前端基于 React 18 和 TypeScript 构建，采用现代化前端架构。
{{% /pageinfo %}}

## 架构概览

DataMate 前端采用 SPA（单页应用）架构，使用 React 18 + TypeScript + Ant Design 技术栈：

```
┌─────────────────────────────────────────────┐
│              Browser                        │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│              React App                      │
│  ┌──────────────────────────────────────┐  │
│  │         Components                   │  │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐│  │
│  │  │  Pages  │ │Common UI│ │ Forms   ││  │
│  │  └─────────┘ └─────────┘ └─────────┘│  │
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │         State Management             │  │
│  │         (Redux Toolkit)              │  │
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │         Services (API)               │  │
│  │         (Axios)                      │  │
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │         Routing                      │  │
│  │         (React Router)               │  │
│  └──────────────────────────────────────┘  │
└─────────────────────────────────────────────┘
```

## 技术栈

### 核心框架

| 技术 | 版本 | 用途 |
|------|------|------|
| React | 18.x | UI 框架 |
| TypeScript | 5.x | 类型安全 |
| Ant Design | 5.x | UI 组件库 |
| Tailwind CSS | 3.x | 样式框架 |

### 状态管理

| 技术 | 版本 | 用途 |
|------|------|------|
| Redux Toolkit | 2.x | 全局状态管理 |
| React Query | 5.x | 服务器状态管理 |

### 路由和构建

| 技术 | 版本 | 用途 |
|------|------|------|
| React Router | 6.x | 路由管理 |
| Vite | 5.x | 构建工具 |

## 项目结构

```
frontend/
├── public/              # 静态资源
├── src/
│   ├── assets/         # 资源文件
│   ├── components/     # 公共组件
│   │   ├── layout/    # 布局组件
│   │   ├── common/    # 通用组件
│   │   └── charts/    # 图表组件
│   ├── pages/          # 页面组件
│   │   ├── DataCollection/
│   │   ├── DataManagement/
│   │   ├── DataCleansing/
│   │   ├── DataAnnotation/
│   │   ├── SynthesisTask/
│   │   ├── RatioTask/
│   │   ├── DataEvaluation/
│   │   ├── KnowledgeBase/
│   │   ├── OperatorMarket/
│   │   ├── Orchestration/
│   │   └── Agent/
│   ├── services/       # API 服务
│   │   ├── api/      # API 定义
│   │   └── types/    # TypeScript 类型
│   ├── store/          # Redux store
│   │   ├── slices/   # Redux slices
│   │   └── index.ts  # Store 配置
│   ├── hooks/          # 自定义 Hooks
│   ├── utils/          # 工具函数
│   ├── constants/      # 常量定义
│   ├── routes/         # 路由配置
│   ├── App.tsx        # 根组件
│   └── main.tsx       # 入口文件
├── index.html
├── vite.config.ts
├── tailwind.config.js
├── tsconfig.json
└── package.json
```

## 路由设计

### 路由结构

```typescript
// routes/routes.ts
const router = createBrowserRouter([
  {
    path: "/",
    Component: Home,
  },
  {
    path: "/chat",
    Component: AgentPage,
  },
  {
    path: "/orchestration",
    children: [
      { path: "", Component: SmartOrchestrationPage },
      { path: "create-workflow", Component: WorkflowEditor },
    ],
  },
  {
    path: "/data",
    Component: MainLayout,
    children: [
      {
        path: "collection",
        children: [
          { index: true, Component: DataCollection },
          { path: "create-task", Component: CollectionTaskCreate },
        ],
      },
      {
        path: "management",
        children: [
          { index: true, Component: DatasetManagement },
          { path: "create/:id?", Component: DatasetCreate },
          { path: "detail/:id", Component: DatasetDetail },
        ],
      },
      // ... 其他路由
    ],
  },
]);
```

### 路由守卫

```typescript
// hooks/useAuthGuard.ts
export const useAuthGuard = () => {
  const location = useLocation();
  const navigate = useNavigate();
  const { isAuthenticated } = useAuth();

  useEffect(() => {
    if (!isAuthenticated) {
      navigate('/login', { state: { from: location } });
    }
  }, [isAuthenticated, location, navigate]);
};
```

## 状态管理

### Redux Toolkit 配置

```typescript
// store/index.ts
import { configureStore } from '@reduxjs/toolkit';
import dataManagementSlice from './slices/dataManagementSlice';
import userSlice from './slices/userSlice';

export const store = configureStore({
  reducer: {
    dataManagement: dataManagementSlice,
    user: userSlice,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false,
    }),
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### Slice 示例

```typescript
// store/slices/dataManagementSlice.ts
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
import { getDatasets, createDataset } from '@/services/api/dataManagement';

interface DataManagementState {
  datasets: Dataset[];
  loading: boolean;
  error: string | null;
}

const initialState: DataManagementState = {
  datasets: [],
  loading: false,
  error: null,
};

export const fetchDatasets = createAsyncThunk(
  'dataManagement/fetchDatasets',
  async (params: GetDatasetsParams) => {
    const response = await getDatasets(params);
    return response.data;
  }
);

const dataManagementSlice = createSlice({
  name: 'dataManagement',
  initialState,
  reducers: {
    clearError: (state) => {
      state.error = null;
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchDatasets.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchDatasets.fulfilled, (state, action) => {
        state.loading = false;
        state.datasets = action.payload;
      })
      .addCase(fetchDatasets.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message || 'Failed to fetch datasets';
      });
  },
});

export const { clearError } = dataManagementSlice.actions;
export default dataManagementSlice.reducer;
```

## 组件设计

### 页面组件

```typescript
// pages/DataManagement/Home/DataManagement.tsx
import React, { useEffect } from 'react';
import { useAppDispatch, useAppSelector } from '@/store/hooks';
import { fetchDatasets } from '@/store/slices/dataManagementSlice';
import DataTable from '@/components/common/DataTable';

export const DataManagement: React.FC = () => {
  const dispatch = useAppDispatch();
  const { datasets, loading } = useAppSelector((state) => state.dataManagement);

  useEffect(() => {
    dispatch(fetchDatasets({ page: 0, size: 20 }));
  }, [dispatch]);

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-6">数据集管理</h1>
      <DataTable data={datasets} loading={loading} />
    </div>
  );
};
```

### 公共组件

```typescript
// components/common/DataTable.tsx
import React from 'react';
import { Table } from 'antd';

interface DataTableProps {
  data: any[];
  loading: boolean;
}

export const DataTable: React.FC<DataTableProps> = ({ data, loading }) => {
  const columns = [
    { title: '名称', dataIndex: 'name', key: 'name' },
    { title: '类型', dataIndex: 'type', key: 'type' },
    { title: '状态', dataIndex: 'status', key: 'status' },
  ];

  return (
    <Table
      columns={columns}
      dataSource={data}
      loading={loading}
      rowKey="id"
    />
  );
};
```

## API 服务

### Axios 配置

```typescript
// services/api/request.ts
import axios from 'axios';

const request = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL || 'http://localhost:8080',
  timeout: 30000,
});

// 请求拦截器
request.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// 响应拦截器
request.interceptors.response.use(
  (response) => {
    return response.data;
  },
  (error) => {
    if (error.response?.status === 401) {
      // 跳转到登录页
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default request;
```

### API 定义

```typescript
// services/api/dataManagement.ts
import request from './request';
import type { Dataset, CreateDatasetRequest } from './types';

export const getDatasets = (params: any) => {
  return request.get<{ content: Dataset[] }>('/data-management/datasets', {
    params,
  });
};

export const getDataset = (id: string) => {
  return request.get<Dataset>(`/data-management/datasets/${id}`);
};

export const createDataset = (data: CreateDatasetRequest) => {
  return request.post<Dataset>('/data-management/datasets', data);
};

export const updateDataset = (id: string, data: Partial<Dataset>) => {
  return request.put<Dataset>(`/data-management/datasets/${id}`, data);
};

export const deleteDataset = (id: string) => {
  return request.delete(`/data-management/datasets/${id}`);
};
```

## TypeScript 类型定义

```typescript
// services/types/dataManagement.ts
export interface Dataset {
  id: string;
  name: string;
  description: string;
  type: DatasetType;
  status: DatasetStatus;
  tags: Tag[];
  fileCount: number;
  totalSize: number;
  completionRate: number;
  createdAt: string;
  createdBy: string;
}

export interface DatasetType {
  code: string;
  name: string;
  description: string;
  supportedFormats: string[];
}

export interface Tag {
  id: string;
  name: string;
  color: string;
}

export type DatasetStatus = 'ACTIVE' | 'INACTIVE' | 'PROCESSING';

export interface CreateDatasetRequest {
  name: string;
  description?: string;
  type: string;
  tags?: string[];
}
```

## 样式方案

### Tailwind CSS

```tsx
// 使用 Tailwind CSS
<div className="flex items-center justify-between p-4 bg-white rounded-lg shadow">
  <h2 className="text-lg font-semibold">标题</h2>
  <button className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
    按钮
  </button>
</div>
```

### Ant Design

```tsx
// 使用 Ant Design 组件
import { Button, Table, Modal } from 'antd';

<Modal
  title="创建数据集"
  open={visible}
  onOk={handleOk}
  onCancel={handleCancel}
>
  <Form>
    <Form.Item label="名称" name="name" rules={[{ required: true }]}>
      <Input />
    </Form.Item>
  </Form>
</Modal>
```

## 性能优化

### 代码分割

```typescript
// 路由懒加载
import { lazy } from 'react';

const DataManagement = lazy(() => import('@/pages/DataManagement/Home/DataManagement'));

const router = createBrowserRouter([
  {
    path: '/data/management',
    Component: lazy(() => import('@/pages/Layout/MainLayout')),
    children: [
      {
        index: true,
        Component: DataManagement,
      },
    ],
  },
]);
```

### React.memo

```typescript
// 使用 React.memo 避免不必要的重渲染
export const DataCard = React.memo<DataCardProps>(({ data }) => {
  return <div>{data.name}</div>;
});
```

### useMemo 和 useCallback

```typescript
// 使用 useMemo 缓存计算结果
const filteredData = useMemo(() => {
  return data.filter(item => item.status === 'ACTIVE');
}, [data]);

// 使用 useCallback 缓存函数
const handleClick = useCallback(() => {
  console.log('clicked');
}, []);
```

## 测试

### 组件测试

```typescript
// DataManagement.test.tsx
import { render, screen } from '@testing-library/react';
import { DataManagement } from './DataManagement';

test('renders data management page', () => {
  render(<DataManagement />);
  expect(screen.getByText('数据集管理')).toBeInTheDocument();
});
```

## 相关文档

- [后端架构](/docs/developer-guide/backend-architecture/) - 后端架构设计
- [开发环境搭建](/docs/getting-started/development/) - 开发环境配置
- [组件文档](https://ant.design/components/overview/) - Ant Design 组件
