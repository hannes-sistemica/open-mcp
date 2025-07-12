# OpenMCP - Open Model Context Protocol Implementation

A comprehensive implementation of the Model Context Protocol (MCP) as a RESTful API service, complete with reference implementations and examples.

## 🎯 Project Overview

OpenMCP transforms Anthropic's Model Context Protocol into a production-ready HTTP/REST API, enabling seamless integration with existing infrastructure and tooling. This project provides:

- **OpenAPI Specification**: Complete REST API definition for MCP
- **Go Server Implementation**: Production-ready MCP server
- **Go Client SDK**: Type-safe client library
- **Sample Agent**: Demonstration of AI agent integration
- **Authentication**: Enterprise-grade security options
- **Real-time Streaming**: WebSocket and SSE support

## 🏗️ Project Structure

```
.
├── openmcp/
│   ├── mcp-api-spec.yaml    # OpenAPI 3.0 specification
│   └── README.md            # Specification documentation
├── server/
│   └── go/                  # Go server implementation
│       ├── main.go
│       ├── handlers/        # HTTP request handlers
│       ├── mcp/            # Core MCP logic
│       ├── auth/           # Authentication middleware
│       └── storage/        # Resource/tool storage
├── client/
│   └── go/                 # Go client SDK
│       ├── client.go       # Main client interface
│       ├── resources.go    # Resource operations
│       ├── tools.go        # Tool execution
│       └── streaming.go    # SSE/WebSocket handling
├── examples/
│   ├── agent/              # Sample AI agent
│   │   ├── main.go
│   │   └── agent.go
│   ├── tools/              # Example MCP tools
│   └── resources/          # Example resources
├── docker/
│   ├── Dockerfile.server
│   └── docker-compose.yml
└── scripts/
    ├── generate-sdk.sh     # SDK generation script
    └── test-integration.sh # Integration tests
```

## 🚀 Quick Start

### 1. Run the Server

```bash
# Using Docker
docker-compose up

# Or build from source
cd server/go
go mod download
go run main.go --config config.yaml
```

### 2. Test with Client

```go
package main

import (
    "context"
    "fmt"
    mcp "github.com/yourusername/openmcp/client/go"
)

func main() {
    // Initialize client
    client := mcp.NewClient("http://localhost:8080", mcp.WithAPIKey("your-key"))
    
    // Initialize session
    session, err := client.Initialize(context.Background(), "1.0", mcp.ClientInfo{
        Name:    "example-app",
        Version: "1.0.0",
    })
    
    // List available tools
    tools, err := client.ListTools(context.Background(), session.SessionID)
    
    // Execute a tool
    result, err := client.ExecuteTool(context.Background(), session.SessionID, "search", map[string]interface{}{
        "query": "OpenMCP documentation",
    })
}
```

### 3. Sample Agent Usage

```bash
cd examples/agent
go run main.go --server http://localhost:8080 --prompt "Help me analyze this codebase"
```

## 📦 Features

### Core MCP Implementation
- ✅ Resource management (files, databases, APIs)
- ✅ Tool execution framework
- ✅ Prompt templates
- ✅ Sampling/completion requests
- ✅ Root directory navigation

### Enhanced Capabilities
- 🔐 **Authentication**: API Key, JWT, OAuth2
- 📡 **Streaming**: Real-time updates via WebSocket/SSE
- 📊 **Monitoring**: Prometheus metrics, structured logging
- 🔄 **High Availability**: Clustering support, health checks
- 🚦 **Rate Limiting**: Built-in request throttling
- 📝 **API Documentation**: Auto-generated Swagger UI

## 🛠️ Development

### Generate Client SDKs

```bash
# Generate SDKs for multiple languages
./scripts/generate-sdk.sh python
./scripts/generate-sdk.sh typescript
./scripts/generate-sdk.sh java
```

### Running Tests

```bash
# Unit tests
go test ./...

# Integration tests
./scripts/test-integration.sh

# Load testing
docker run -v $PWD:/mnt/k6 grafana/k6 run /mnt/k6/tests/load-test.js
```

### Building from Source

```bash
# Build server
cd server/go
go build -o mcp-server

# Build client CLI
cd client/go/cmd
go build -o mcp-cli
```

## 📚 Documentation

- [API Specification](openmcp/README.md) - Detailed OpenAPI documentation
- [Server Guide](server/go/README.md) - Server configuration and deployment
- [Client Guide](client/go/README.md) - Client SDK usage examples
- [Agent Tutorial](examples/agent/README.md) - Building AI agents with MCP

## 🤝 Example Use Cases

### 1. Enterprise Data Gateway
Expose corporate resources securely to AI applications:
```yaml
resources:
  - name: "customer-database"
    uri: "postgresql://db.internal/customers"
    security: oauth2
  - name: "documents"
    uri: "s3://corporate-docs/"
    security: apiKey
```

### 2. Tool Marketplace
Create reusable tools for AI agents:
```go
func SearchCodebase(ctx context.Context, args map[string]interface{}) (interface{}, error) {
    query := args["query"].(string)
    language := args["language"].(string)
    
    // Implementation
    return results, nil
}
```

### 3. Multi-Agent Systems
Coordinate multiple AI agents through shared context:
```go
agent1 := NewAgent("researcher", mcpClient)
agent2 := NewAgent("coder", mcpClient)
agent3 := NewAgent("reviewer", mcpClient)

// Agents share resources and tools through MCP
```

## 🔧 Configuration

### Server Configuration
```yaml
server:
  host: 0.0.0.0
  port: 8080
  
auth:
  enabled: true
  providers:
    - type: apikey
    - type: oauth2
      issuer: https://auth.example.com
      
storage:
  type: postgres
  connection: "postgres://localhost/mcp"
  
streaming:
  websocket:
    enabled: true
    maxConnections: 1000
```

## 🚀 Deployment

### Kubernetes
```bash
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### Docker Compose
```bash
docker-compose up -d
```

### Systemd
```bash
sudo cp mcp-server.service /etc/systemd/system/
sudo systemctl enable mcp-server
sudo systemctl start mcp-server
```

## 📈 Monitoring

- **Metrics**: Prometheus endpoint at `/metrics`
- **Health Check**: `/health` returns service status
- **Logging**: Structured JSON logs with trace IDs
- **Tracing**: OpenTelemetry support

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Setup
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Anthropic](https://anthropic.com) for creating the Model Context Protocol
- The OpenAPI Initiative for standardization tools
- Contributors and early adopters

## 🔗 Links

- [MCP Official Documentation](https://modelcontextprotocol.io)
- [OpenAPI Specification](https://www.openapis.org)
- [Project Issues](https://github.com/yourusername/openmcp/issues)
- [Discussions](https://github.com/yourusername/openmcp/discussions)

---

**Ready to connect your AI to the world?** Get started with OpenMCP today! 🚀