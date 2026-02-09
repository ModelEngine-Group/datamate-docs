---
title: Troubleshooting
description: Common issues and solutions for DataMate
weight: 2
---

{{% pageinfo %}}
This document provides troubleshooting steps and solutions for common DataMate issues.
{{% /pageinfo %}}

## Service Startup Issues

### Service Won't Start

#### Symptoms

Service fails to start or exits immediately after running `make install`.

#### Troubleshooting Steps

1. **Check Port Conflicts**

```bash
# Check port usage
lsof -i :8080  # API Gateway
lsof -i :30000 # Frontend
```

If port is occupied:
```bash
# Kill process
kill -9 <PID>
```

2. **View Container Logs**

```bash
# View all containers
docker ps -a

# View specific container logs
docker logs datamate-backend
```

3. **Check Docker Resources**

```bash
# View Docker system info
docker system df

# Clean unused resources
docker system prune -a
```

#### Common Causes and Solutions

| Cause | Solution |
|-------|----------|
| Port occupied | Kill process or modify port mapping |
| Insufficient memory | Increase Docker memory limit |
| Image not pulled | Run `docker pull` |
| Network issues | Check firewall and network config |

### Container Exits Immediately

#### Troubleshooting

```bash
# View exit code
docker ps -a

# View detailed logs
docker logs <container-name> --tail 100
```

## Database Connection Issues

### Cannot Connect to Database

#### Troubleshooting Steps

1. **Check Database Container**

```bash
docker ps | grep datamate-database
docker logs datamate-database
```

2. **Test Database Connection**

```bash
# Enter database container
docker exec -it datamate-database psql -U postgres -d datamate
```

3. **Check Database Config**

```bash
# Check environment variables
docker exec datamate-backend env | grep DB_
```

## Frontend Issues

### Frontend Not Accessible

#### Symptoms

Browser cannot access http://localhost:30000

#### Troubleshooting

1. **Check Frontend Container**

```bash
docker ps | grep datamate-frontend
docker logs datamate-frontend
```

2. **Check Port Mapping**

```bash
docker port datamate-frontend
```

### API Request Failed

#### Troubleshooting

1. **Check Browser Console**

Open browser DevTools → Network tab

2. **Check API Gateway**

```bash
docker ps | grep datamate-gateway
docker logs datamate-gateway
```

3. **Test API**

```bash
curl http://localhost:8080/actuator/health
```

## Task Execution Issues

### Task Stuck

#### Troubleshooting

1. **View Task Logs**

```bash
docker logs datamate-backend --tail 100 | grep <task-id>
docker logs datamate-runtime --tail 100
```

2. **Check System Resources**

```bash
docker stats
```

## Performance Issues

### Slow System Response

#### Troubleshooting

1. **Check System Resources**

```bash
docker stats
```

2. **Check Database Performance**

```sql
-- View active queries
SELECT * FROM pg_stat_activity WHERE state = 'active';
```

### Memory Overflow

#### Troubleshooting

```bash
# Check exit reason
docker inspect <container> | grep OOMKilled
```

## Log Viewing

### View Application Logs

```bash
# Backend logs
docker logs datamate-backend --tail 100 -f

# Frontend logs
docker logs datamate-frontend --tail 100 -f
```

### Log File Locations

| Service | Log Path |
|---------|----------|
| Backend | `/var/log/datamate/backend/app.log` |
| Frontend | `/var/log/datamate/frontend/` |
| Database | `/var/log/datamate/database/` |
| Runtime | `/var/log/datamate/runtime/` |

## Getting Help

If issues persist:

1. **Collect Information**
   - Error messages
   - Log files
   - System environment
   - Reproduction steps

2. **Search Existing Issues**

Visit [GitHub Issues](https://github.com/ModelEngine-Group/DataMate/issues)

3. **Submit New Issue**

Include:
- DataMate version
- OS version
- Docker version
- Detailed error messages
- Reproduction steps

## Related Documentation

- [Installation Guide](/docs/getting-started/installation/) - Deployment guide
- [Configuration](/docs/appendix/configuration/) - Configuration parameters
