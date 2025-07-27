# 🌐 Element Service

A modular Docker Compose configuration system for Element Web client with support for multiple environments and OIDC integration capabilities.

## 🏗️ Project Structure

```sh
src/element/
├── components/                              # Source compose components
│   ├── base/                               # Base components
│   │   ├── docker-compose.yml              # Main Element Web service
│   │   └── .env.example                    # Base environment variables
│   ├── environments/                       # Environment components
│   │   ├── devcontainer/
│   │   │   └── docker-compose.yml          # DevContainer environment
│   │   ├── forwarding/
│   │   │   └── docker-compose.yml          # Development with port forwarding
│   │   ├── letsencrypt/
│   │   │   ├── docker-compose.yml          # Let's Encrypt SSL
│   │   │   └── .env.example                # Let's Encrypt variables
│   │   └── step-ca/
│   │       ├── docker-compose.yml          # Step CA SSL
│   │       └── .env.example                # Step CA variables
│   └── extensions/                         # Extension components
│       └── oidc/                           # OIDC identity server extension
│           ├── docker-compose.yml          # Identity server configuration
│           └── .env.example                # OIDC variables
├── build/                        # Generated configurations (auto-generated)
│   ├── devcontainer/
│   │   ├── base/                 # DevContainer + base
│   │   └── oidc/                 # DevContainer + base + OIDC
│   ├── forwarding/
│   │   ├── base/                 # Development + base
│   │   └── oidc/                 # Development + base + OIDC
│   ├── letsencrypt/
│   │   ├── base/                 # Let's Encrypt + base
│   │   └── oidc/                 # Let's Encrypt + base + OIDC
│   └── step-ca/
│       ├── base/                 # Step CA + base
│       └── oidc/                 # Step CA + base + OIDC
├── build.sh                      # Build script
└── README.md                     # This file
```

## 🚀 Quick Start

### 1. Build Configurations

Run the build script to generate all possible combinations:

```bash
./build.sh
```

This will create all combinations in the `build/` directory.

### 2. Choose Your Configuration

Navigate to the desired configuration directory:

```bash
# For development with port forwarding
cd build/forwarding/base/

# For development with port forwarding and OIDC
cd build/forwarding/oidc/

# For DevContainer environment
cd build/devcontainer/base/

# For DevContainer environment with OIDC
cd build/devcontainer/oidc/

# For production with Let's Encrypt SSL
cd build/letsencrypt/base/

# For production with Let's Encrypt SSL and OIDC
cd build/letsencrypt/oidc/

# For production with Step CA SSL
cd build/step-ca/base/

# For production with Step CA SSL and OIDC
cd build/step-ca/oidc/
```

### 3. Configure Environment

Copy and edit the environment file:

```bash
cp .env.example .env
# Edit .env with your values
```

### 4. Deploy

Start the services:

```bash
docker-compose up -d
```

Access: `http://localhost:3000` (for forwarding mode)

## 🔧 Available Configurations

### Environments

- **devcontainer**: Development environment with workspace network
- **forwarding**: Development environment with port forwarding (3000:80)
- **letsencrypt**: Production with Let's Encrypt SSL certificates
- **step-ca**: Production with Step CA SSL certificates

### Extensions

- **oidc**: OIDC identity server integration for enhanced authentication

### Generated Combinations

Each environment can be combined with extensions to provide complete Element deployments:

**Base configurations:**

- `devcontainer/base` - DevContainer development setup
- `forwarding/base` - Development with port forwarding
- `letsencrypt/base` - Production with Let's Encrypt SSL
- `step-ca/base` - Production with Step CA SSL

**OIDC-enabled configurations:**

- `devcontainer/oidc` - DevContainer development with identity server
- `forwarding/oidc` - Development with port forwarding and identity server
- `letsencrypt/oidc` - Production with Let's Encrypt SSL and identity server
- `step-ca/oidc` - Production with Step CA SSL and identity server

## 🔧 Environment Variables

### Base Configuration

- `MATRIX_TZ`: Timezone for Element services
- `MATRIX_DOMAIN`: Matrix homeserver domain (e.g., matrix.example.com)

### Let's Encrypt Configuration

- `SYNAPSE_SERVER_NAME`: Server name for Element (e.g., element.example.com)
- `VIRTUAL_PORT`: Port for nginx-proxy (default: 80)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### Step CA Configuration

- `SYNAPSE_SERVER_NAME`: Server name for Element (e.g., element.local)
- `VIRTUAL_PORT`: Port for nginx-proxy (default: 80)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### OIDC Configuration

- `IDENTITY_SERVER_URL`: Identity server URL for enhanced authentication (e.g., <https://idm.example.com>)

## 🔐 OIDC Integration

The OIDC extension provides integration with external identity servers for enhanced authentication and user management.

### Using OIDC Extension

1. **Build with OIDC extension:**

   ```bash
   ./build.sh
   ```

2. **Choose OIDC-enabled configuration:**

   ```bash
   # For development with OIDC
   cd build/forwarding/oidc/
   
   # For production with Let's Encrypt and OIDC
   cd build/letsencrypt/oidc/
   ```

3. **Configure identity server:**

   ```bash
   cp .env.example .env
   # Edit .env with your identity server details
   ```

4. **Example OIDC configuration:**

   ```bash
   MATRIX_DOMAIN=matrix.example.com
   IDENTITY_SERVER_URL=https://idm.example.com
   ```

5. **Deploy:**

   ```bash
   docker-compose up -d
   ```

## 🛠️ Development

### Adding New Environments

1. Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example` file
2. Run `./build.sh` to generate new combinations

### Adding New Extensions

1. Create directory in `components/extensions/` with `docker-compose.yml` and optional `.env.example` file
2. Run `./build.sh` to generate new combinations with all environments
3. Extensions are automatically combined with all available environments

### File Naming Convention

All component files follow the standard Docker Compose naming convention (`docker-compose.yml`) for:

- **VS Code compatibility**: Full support for Docker Compose language features and IntelliSense
- **IDE integration**: Proper syntax highlighting and validation in all major editors
- **Tool compatibility**: Works with Docker Compose plugins and extensions
- **Standard compliance**: Follows official Docker Compose file naming patterns

### Modifying Existing Components

1. Edit the component files in `components/`
2. Run `./build.sh` to regenerate configurations
3. The `build/` directory will be completely recreated

## 🌐 Networks

- **Development**: `element-network` (internal)
- **DevContainer**: `element-workspace-network` (external)
- **Let's Encrypt**: `letsencrypt-network` (external)
- **Step CA**: `step-ca-network` (external)

## 🔒 Security

⚠️ **Production Checklist:**

- Configure proper Matrix homeserver URL
- Set up proper firewall rules
- Regular security updates
- Configure rate limiting
- Set up proper logging
- Validate SSL certificates

## 🆘 Troubleshooting

**Build Issues:**

- Ensure `yq` is installed: <https://github.com/mikefarah/yq#install>
- Check component file syntax
- Verify all required files exist

**Element Issues:**

- Check config.json syntax
- Verify Matrix homeserver connection
- Review container logs: `docker logs element`
- Check network connectivity

**SSL Issues:**

- **Let's Encrypt**: Verify domain DNS and letsencrypt-manager
- **Step CA**: Check step-ca-manager and virtual network config

**Matrix Connection Issues:**

- Verify Matrix homeserver is accessible
- Check CORS configuration on homeserver
- Validate SSL certificates
- Check network connectivity between Element and Matrix

## 📝 Notes

- The `build/` directory is automatically generated and should not be edited manually
- Environment variables in generated files use `$VARIABLE_NAME` format for proper interpolation
- Each generated configuration includes a complete `docker-compose.yml` and `.env.example`
- Missing `.env.*` files for components are handled gracefully by the build script
- Element requires proper Matrix homeserver configuration for functionality

## 🔄 Configuration Management

The build system automatically:

- Merges base and environment configurations
- Copies additional files and configurations
- Generates complete deployment configurations
- Preserves user `.env` files during rebuilds

**Build approach:**

```bash
./build.sh
cd build/forwarding/base/
cp .env.example .env
# Edit .env with your values
docker-compose up -d
```

## 🎨 Element Features

The Element configuration includes:

- **Modern UI**: Latest Element Web interface
- **Video Calls**: Element Call integration
- **Group Calls**: Support for group video calls
- **Threads**: Activity centre for threaded conversations
- **Labs Settings**: Access to experimental features
- **Rust Crypto**: Enhanced encryption with Rust implementation
- **Mobile Registration**: Simplified mobile device registration
- **Custom Branding**: Configurable branding and styling
- **Integration Support**: Scalar integrations for widgets and bots

## ⚙️ Advanced Configuration

For detailed configuration options of the Element Web client, including customization, branding, SSO setup, VoIP settings, and all available `/app/config.json` parameters, see the official Element Web configuration documentation:

📖 **[Element Web Configuration Guide](https://github.com/element-hq/element-web/blob/develop/docs/config.md)**

This comprehensive guide covers:

- Homeserver configuration options
- Labs flags and feature toggles
- Branding and customization settings
- SSO and OIDC integration
- VoIP and Jitsi call configuration
- Bug reporting and analytics setup
- UI feature flags and administrative options

## 🔗 Integration

Element works seamlessly with:

- **Matrix Synapse**: Primary Matrix homeserver
- **Identity Servers**: Enhanced authentication and discovery
- **Element Call**: Video calling service
- **Scalar**: Widget and integration platform
- **Custom Homeservers**: Any Matrix-compatible server
