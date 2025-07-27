# 🌐 Element Stack

Complete Docker-based Element Web deployment with SSL certificate management and identity integration for production and development environments.

## 🧩 Components

### 🔐 SSL Automation

#### [🔒 Let's Encrypt Manager](src/ssl-automation/letsencrypt-manager)

Automatic SSL certificate management from Let's Encrypt for production deployments. Provides seamless HTTPS integration for Docker containers using nginx-proxy and acme-companion.
[Learn more about Let's Encrypt Manager configuration](src/ssl-automation/letsencrypt-manager/README.md).

#### [🏠 Step CA Manager](src/ssl-automation/step-ca-manager)

Local domain stack with trusted self-signed certificates for virtual network deployments. Includes private CA management and local DNS resolution for development environments.
[Learn more about Step CA Manager configuration](src/ssl-automation/step-ca-manager/README.md).

### 🔑 Identity Management

#### [🔐 Keycloak](src/identity-management/keycloak)

Enterprise-grade identity and access management solution. Provides authentication, authorization, and user management for secure application access.
[Learn more about Keycloak configuration](src/identity-management/keycloak/README.md).

For Element integration, see: [Keycloak OIDC Integration](https://element.io/blog/element-web-now-uses-matrix-js-sdk-crypto/)

#### [🔐 Kanidm](src/identity-management/kanidm)

Modern identity and access management server with comprehensive authentication capabilities. Provides secure identity management with modular configuration system and multiple deployment modes.
[Learn more about Kanidm configuration](src/identity-management/kanidm/README.md).

### 💬 Matrix Services

#### [🏠 Synapse](src/matrix/synapse)

Matrix homeserver implementation providing the backend infrastructure for Element Web client. Includes PostgreSQL backend, OIDC integration, and multiple deployment configurations.
[Learn more about Matrix Synapse configuration](src/matrix/synapse/README.md).

## 🌐 Services

### 🌐 [Element Web](src/element/)

Modular Docker Compose configuration system for Element Web client with support for multiple environments and OIDC integration capabilities. Provides complete Matrix web client deployment with customizable configurations for development and production.
[Learn more about Element Web configuration](src/element/README.md).

## 🚀 Quick Start

Each component has its own README with detailed setup instructions. Choose the certificate management solution and identity provider that fits your deployment scenario.

### Basic Setup

1. **Choose SSL Management:**
   - Production: Use Let's Encrypt Manager
   - Development: Use Step CA Manager

2. **Configure Identity (Optional):**
   - Enterprise: Use Keycloak
   - Modern: Use Kanidm

3. **Deploy Matrix Backend:**
   - Set up Synapse homeserver

4. **Deploy Element Web:**
   - Configure Element client to connect to your Matrix homeserver

## 🏗️ Architecture

```sh
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Element Web   │────│  Matrix Synapse │────│   PostgreSQL    │
│   (Frontend)    │    │   (Homeserver)  │    │   (Database)    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │
         │                       │
┌─────────────────┐    ┌─────────────────┐
│ Identity Server │    │  SSL Manager    │
│ (Keycloak/      │    │ (Let's Encrypt/ │
│  Kanidm)        │    │  Step CA)       │
└─────────────────┘    └─────────────────┘
```

## 📋 Requirements

- Docker & Docker Compose
- Domain name (for production deployments)
- Email address (for Let's Encrypt)
- `yq` tool for configuration building

## 🔧 Configuration

All services use modular Docker Compose configurations with:

- **Base components**: Core service definitions
- **Environment components**: Development, production, SSL configurations
- **Extension components**: OIDC, identity integration, additional features
- **Build system**: Automatic generation of deployment combinations

## 🌍 Deployment Scenarios

### Development Environment

```bash
# Element with port forwarding
cd src/element/build/forwarding/base/
docker-compose up -d

# Synapse with port forwarding
cd src/matrix/synapse/build/forwarding/base/
docker-compose up -d
```

### Production Environment

```bash
# Element with Let's Encrypt SSL
cd src/element/build/letsencrypt/base/
docker-compose up -d

# Synapse with Let's Encrypt SSL and OIDC
cd src/matrix/synapse/build/letsencrypt/oidc/
docker-compose up -d
```

### DevContainer Environment

```bash
# Element in DevContainer
cd src/element/build/devcontainer/base/
docker-compose up -d

# Synapse in DevContainer with OIDC
cd src/matrix/synapse/build/devcontainer/oidc/
docker-compose up -d
```

## 🔐 Security Features

- **SSL/TLS Encryption**: Automatic certificate management
- **Identity Integration**: OIDC/SAML authentication
- **Network Isolation**: Docker network segmentation
- **Secret Management**: Environment-based configuration
- **Access Control**: Role-based permissions

## 🆘 Troubleshooting

### Common Issues

- **SSL Certificate Issues**: Check Let's Encrypt/Step CA configuration
- **Identity Integration**: Verify OIDC provider settings
- **Network Connectivity**: Ensure proper Docker network configuration
- **Database Connection**: Check PostgreSQL connectivity for Synapse

### Logs

```bash
# Element logs
docker logs element

# Synapse logs
docker logs matrix

# Identity provider logs
docker logs keycloak  # or kanidm
```

## 📚 Documentation

- [Element Web Configuration](src/element/README.md)
- [Matrix Synapse Setup](src/matrix/synapse/README.md)
- [SSL Automation](src/ssl-automation/)
- [Identity Management](src/identity-management/)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test configurations
5. Submit a pull request

## 📄 License

This project is dual-licensed under:

- [Apache License 2.0](LICENSE-APACHE)
- [MIT License](LICENSE-MIT)

## 🔗 Related Projects

- [Matrix.org](https://matrix.org/) - Open network for secure, decentralized communication
- [Element.io](https://element.io/) - Secure collaboration and messaging
- [Synapse](https://github.com/matrix-org/synapse) - Matrix homeserver implementation
- [Keycloak](https://www.keycloak.org/) - Identity and access management
- [Kanidm](https://kanidm.com/) - Modern identity management
