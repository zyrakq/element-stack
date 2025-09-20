# 🌐 Element Service

A modular Docker Compose configuration system for Element Web client with support for multiple environments and OIDC integration capabilities.

## 🚀 Quick Start

### 1. Build Configurations

Use the [stackbuilder](https://github.com/zyrakq/stackbuilder) utility to generate all configurations:

```bash
sb build
```

This creates complete Docker Compose configurations in the `build/` directory.

### 2. Choose Configuration

Navigate to your desired environment:

```bash
# Development with port forwarding
cd build/forwarding/base/

# Production with Let's Encrypt SSL
cd build/letsencrypt/base/

# Development with OIDC integration
cd build/forwarding/oidc/
```

### 3. Configure Environment

Copy and customize the environment file:

```bash
cp .env.example .env
# Edit .env with your values
```

### 4. Deploy

Start the services:

```bash
docker compose up --build -d
```

Access Element at `http://localhost:3000` (forwarding mode) or your configured domain.

## 📁 Directory Structure

- **`build/`** - Generated Docker Compose configurations (ready to deploy)
- **`components/`** - Source files used by stackbuilder to generate configurations
  - `base/` - Core Element service configuration
  - `environments/` - Environment-specific settings (dev, production, etc.)
  - `extensions/` - Optional extensions like OIDC integration

## 🔧 Available Environments

- **devcontainer** - Development environment with workspace network
- **forwarding** - Development with port forwarding (3000:80)
- **letsencrypt** - Production with Let's Encrypt SSL certificates
- **step-ca** - Production with Step CA SSL certificates

## 🔌 Extensions

- **oidc** - OIDC identity server integration for enhanced authentication
- **step-ca-trust** - Step CA certificate trust configuration

## 🔐 Environment Variables

### Base Configuration

- `MATRIX_TZ` - Timezone for Element services
- `MATRIX_DOMAIN` - Matrix homeserver domain (e.g., matrix.example.com)

### Let's Encrypt Configuration

- `SYNAPSE_SERVER_NAME` - Server name for Element (e.g., element.example.com)
- `VIRTUAL_HOST` - Domain for nginx-proxy
- `LETSENCRYPT_HOST` - Domain for SSL certificate
- `LETSENCRYPT_EMAIL` - Email for certificate registration

### OIDC Configuration

- `IDENTITY_SERVER_URL` - Identity server URL for enhanced authentication (e.g., <https://idm.example.com>)

## 🔒 Security Checklist

⚠️ **For production deployments:**

- Configure proper Matrix homeserver URL
- Set up proper firewall rules
- Regular security updates
- Configure rate limiting
- Set up proper logging
- Validate SSL certificates

## 🆘 Troubleshooting

**Build Issues:**

- Ensure stackbuilder is installed: `cargo install stackbuilder`
- Check [`stackbuilder.toml`](stackbuilder.toml) configuration

**Element Issues:**

- Check config.json syntax
- Verify Matrix homeserver connection
- Review container logs: `docker logs element`
- Check network connectivity

**SSL Issues:**

- **Let's Encrypt**: Verify domain DNS and letsencrypt-manager
- **Step CA**: Check step-ca-manager and virtual network config

## 🌐 Networks

- **Development**: `element-network` (internal)
- **DevContainer**: `element-workspace-network` (external)
- **Let's Encrypt**: `letsencrypt-network` (external)
- **Step CA**: `step-ca-network` (external)

## ⚙️ Advanced Configuration

For detailed Element Web client configuration options including customization, branding, SSO setup, VoIP settings, and all available `/app/config.json` parameters, see the [official Element Web configuration documentation](https://github.com/element-hq/element-web/blob/develop/docs/config.md).

## 🔗 Integration

Element works seamlessly with:

- **Matrix Synapse** - Primary Matrix homeserver
- **Identity Servers** - Enhanced authentication and discovery
- **Element Call** - Video calling service
- **Scalar** - Widget and integration platform
- **Custom Homeservers** - Any Matrix-compatible server
