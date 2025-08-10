[readme_file.md](https://github.com/user-attachments/files/21706026/readme_file.md)
# 🚀 Task Automation API

A powerful FastAPI-based service for remote server management and system administration. Perfect for chatbot integration, mobile apps, or any scenario where you need to manage servers remotely.

> **"Restart your server from your phone!"** - Execute critical system tasks from anywhere with simple HTTP requests.

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-Latest-green.svg)](https://fastapi.tiangolo.com)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## ✨ Features

- 🔒 **Secure Authentication** - Bearer token-based security
- 🖥️ **System Monitoring** - CPU, memory, disk usage, uptime tracking  
- 🔄 **Service Management** - Start/stop/restart system services
- 📊 **Resource Checking** - Disk space and memory monitoring
- 📝 **Log Analysis** - View recent log entries
- ⚡ **Safe Command Execution** - Whitelisted command execution
- 🤖 **Chatbot Ready** - Perfect for Discord, Slack, Telegram bots
- 📱 **Mobile Friendly** - Manage servers from your phone

## 🚀 Quick Start

### Install Dependencies

```bash
pip install fastapi uvicorn psutil
```

### Set Up Authentication

```bash
export API_TOKEN="your-super-secure-token-here"
```

### Run the Server

```bash
python task_api.py
```

Your API is now running at `http://localhost:8000`! 🎉

### Test It Out

```bash
# Check if it's working
curl http://localhost:8000/health

# Get system status (requires authentication)
curl -X POST "http://localhost:8000/execute" \
  -H "Authorization: Bearer your-token" \
  -H "Content-Type: application/json" \
  -d '{"task_type": "check_status"}'
```

## 📋 Available Tasks

| Task | Description | Use Case |
|------|-------------|----------|
| 🔄 `restart_server` | Safely restart the server | Emergency server restart |
| 📊 `check_status` | Get CPU, memory, disk stats | Health monitoring |
| 🛠️ `restart_service` | Restart system services | Fix stuck services |
| 💾 `check_disk_space` | Monitor disk usage | Prevent disk full errors |
| 🧠 `check_memory` | Check RAM and swap usage | Memory monitoring |
| ⚡ `run_command` | Execute safe commands | Quick diagnostics |
| 📝 `check_logs` | View recent log entries | Troubleshooting |

## 💡 Usage Examples

### Python Client

```python
import requests

def restart_nginx():
    response = requests.post(
        "http://your-server:8000/execute",
        headers={"Authorization": "Bearer your-token"},
        json={
            "task_type": "restart_service",
            "parameters": {"service_name": "nginx"}
        }
    )
    return response.json()

# Use it!
result = restart_nginx()
print(result['message'])
```

### Discord Bot Integration

```python
@bot.command(name='server')
async def server_status(ctx):
    # Get server status through the API
    response = requests.post(api_url, headers=headers, 
                           json={"task_type": "check_status"})
    
    data = response.json()['data']
    await ctx.send(f"🖥️ CPU: {data['cpu_percent']}% | 💾 Memory: {data['memory_percent']}%")
```

### Emergency Server Restart (From Your Phone!)

```bash
curl -X POST "http://your-server:8000/execute" \
  -H "Authorization: Bearer your-token" \
  -H "Content-Type: application/json" \
  -d '{
    "task_type": "restart_server",
    "parameters": {
      "delay": 60,
      "confirm": true
    }
  }'
```

## 🔒 Security Features

- ✅ **Token Authentication** - All endpoints protected
- ✅ **Command Whitelisting** - Only safe commands allowed
- ✅ **Input Validation** - Pydantic model validation
- ✅ **Request Logging** - Full audit trail
- ✅ **Rate Limiting Ready** - Easy to add rate limits
- ✅ **HTTPS Support** - Production-ready security

## 🐳 Docker Deployment

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY task_api.py .
EXPOSE 8000

CMD ["python", "task_api.py"]
```

```bash
docker build -t task-api .
docker run -d -p 8000:8000 -e API_TOKEN="your-token" task-api
```

## 🎯 Real-World Use Cases

### DevOps Automation
```python
# Automated health check and service restart
if cpu_usage > 90:
    restart_service("high-cpu-service")
    send_slack_alert("Service restarted due to high CPU")
```

### Chatbot Server Management
```python
# Discord command: !restart nginx
@bot.command()
async def restart(ctx, service):
    result = api_call("restart_service", {"service_name": service})
    await ctx.send(f"✅ {service} restarted successfully!")
```

### Mobile Server Monitoring
```javascript
// React Native app
const getServerStatus = async () => {
  const response = await fetch('/execute', {
    method: 'POST',
    headers: {'Authorization': `Bearer ${token}`},
    body: JSON.stringify({task_type: 'check_status'})
  });
  return response.json();
};
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `API_TOKEN` | Authentication token | Required |
| `HOST` | Server host | `0.0.0.0` |
| `PORT` | Server port | `8000` |
| `LOG_LEVEL` | Logging level | `INFO` |

### Production Setup

```bash
# Install production server
pip install gunicorn

# Run with Gunicorn
gunicorn -w 4 -k uvicorn.workers.UvicornWorker task_api:app --bind 0.0.0.0:8000

# Or use systemd service
sudo systemctl enable task-api
sudo systemctl start task-api
```

## 📚 API Documentation

Once running, visit:
- **Interactive Docs**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

### Quick API Reference

```bash
# Health check (no auth required)
GET /health

# Execute a task (auth required)
POST /execute
{
  "task_type": "check_status",
  "parameters": {}
}

# List available tasks
GET /tasks

# Get system status
GET /status
```

## 🤝 Integration Examples

<details>
<summary><strong>Slack Bot</strong></summary>

```python
from slack_bolt import App

@app.command("/server-status")
def server_status(ack, say):
    ack()
    status = api_call("check_status")
    say(f"Server Status: CPU {status['cpu_percent']}%")
```
</details>

<details>
<summary><strong>Telegram Bot</strong></summary>

```python
from telegram.ext import CommandHandler

def server_status(update, context):
    status = api_call("check_status")
    update.message.reply_text(f"🖥️ CPU: {status['cpu_percent']}%")

app.add_handler(CommandHandler("status", server_status))
```
</details>

<details>
<summary><strong>Home Assistant</strong></summary>

```yaml
# configuration.yaml
rest_command:
  restart_server:
    url: "http://your-server:8000/execute"
    method: POST
    headers:
      Authorization: "Bearer your-token"
    payload: '{"task_type": "restart_server", "parameters": {"delay": 30, "confirm": true}}'
```
</details>

## 🚨 Safety Features

- **Confirmation Required**: Critical operations need explicit confirmation
- **Delay Timers**: Server restarts have configurable delays
- **Command Validation**: Only whitelisted commands can execute
- **Error Handling**: Comprehensive error responses
- **Logging**: All operations logged for audit

## 🛠️ Development

### Running Tests

```bash
pip install pytest httpx
pytest tests/
```

### Adding New Tasks

1. Add task type to `TaskType` enum
2. Create handler method in `TaskHandler`
3. Add routing logic
4. Update documentation

```python
class TaskType(str, Enum):
    YOUR_NEW_TASK = "your_new_task"

class TaskHandler:
    @staticmethod
    def your_new_task(parameters):
        # Implementation here
        return TaskResponse(success=True, message="Done!")
```

## 🐛 Troubleshooting

### Common Issues

| Problem | Solution |
|---------|----------|
| 401 Unauthorized | Check your API token |
| Permission denied | Run with appropriate sudo rights |
| Service not found | Verify service name exists |
| Connection refused | Check if API is running and port is open |

### Debug Mode

```bash
# Run with debug logging
export LOG_LEVEL=DEBUG
python task_api.py
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## ⭐ Star History

If this project helped you, please consider giving it a star! ⭐

## 📞 Support

- 📖 [Full Documentation](docs/API_DOCUMENTATION.md)
- 🐛 [Report Issues](https://github.com/yourusername/task-automation-api/issues)
- 💬 [Discussions](https://github.com/yourusername/task-automation-api/discussions)

---

<div align="center">

**Made with ❤️ for system administrators and DevOps engineers**

*"Because managing servers shouldn't require being physically present!"*

</div>
