---
title: Frontend Architecture
description: DataMate React frontend architecture design
weight: 2
---

{{% pageinfo %}}
DataMate frontend is built on React 18 and TypeScript with modern frontend architecture.
{{% /pageinfo %}}

## Architecture Overview

DataMate frontend adopts SPA architecture:

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
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │         State Management             │  │
│  │         (Redux Toolkit)              │  │
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │         Services (API)               │  │
│  └──────────────────────────────────────┘  │
│  ┌──────────────────────────────────────┐  │
│  │         Routing                      │  │
│  └──────────────────────────────────────┘  │
└─────────────────────────────────────────────┘
```

## Tech Stack

### Core Frameworks

| Technology | Version | Purpose |
|------------|---------|---------|
| React | 18.x | UI framework |
| TypeScript | 5.x | Type safety |
| Ant Design | 5.x | UI components |
| Tailwind CSS | 3.x | Styling |

### State Management

| Technology | Version | Purpose |
|------------|---------|---------|
| Redux Toolkit | 2.x | Global state |
| React Query | 5.x | Server state |

## Project Structure

```
frontend/
├── src/
│   ├── components/     # Common components
│   ├── pages/          # Page components
│   ├── services/       # API services
│   ├── store/          # Redux store
│   ├── hooks/          # Custom hooks
│   ├── routes/         # Routes config
│   └── main.tsx        # Entry point
```

## Routing Design

```typescript
const router = createBrowserRouter([
  { path: "/", Component: Home },
  { path: "/chat", Component: AgentPage },
  {
    path: "/data",
    Component: MainLayout,
    children: [
      {
        path: "management",
        Component: DatasetManagement
      }
    ]
  }
]);
```

## State Management

### Redux Toolkit Configuration

```typescript
export const store = configureStore({
  reducer: {
    dataManagement: dataManagementSlice,
    user: userSlice,
  },
});
```

### Slice Example

```typescript
export const fetchDatasets = createAsyncThunk(
  'dataManagement/fetchDatasets',
  async (params: GetDatasetsParams) => {
    const response = await getDatasets(params);
    return response.data;
  }
);
```

## Component Design

### Page Component

```typescript
export const DataManagement: React.FC = () => {
  const dispatch = useAppDispatch();
  const { datasets, loading } = useAppSelector(
    (state) => state.dataManagement
  );

  useEffect(() => {
    dispatch(fetchDatasets({ page: 0, size: 20 }));
  }, [dispatch]);

  return (
    <div className="p-6">
      <h1>Data Management</h1>
      <DataTable data={datasets} loading={loading} />
    </div>
  );
};
```

## API Services

### Axios Configuration

```typescript
const request = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 30000,
});

// Request interceptor
request.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

## Performance Optimization

### Code Splitting

```typescript
const DataManagement = lazy(() =>
  import('@/pages/DataManagement/Home/DataManagement')
);
```

### React.memo

```typescript
export const DataCard = React.memo<DataCardProps>(({ data }) => {
  return <div>{data.name}</div>;
});
```

## Related Documentation

- [Backend Architecture](/docs/developer-guide/backend-architecture/) - Backend architecture
- [Development Setup](/docs/getting-started/development/) - Dev environment
