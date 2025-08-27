# 🌉 Mautrix Telegram Bridge Service

A modular Docker Compose configuration system for Mautrix Telegram bridge with support for multiple environments and extensions.

## 🏗️ Project Structure

```sh
src/mautrix-telegram/
├── components/                              # Source compose components
│   ├── base/                               # Base components
│   │   ├── docker-compose.yml              # Main Mautrix Telegram service
│   │   ├── docker-compose.viewer.yml       # Registration viewer service
│   │   └── .env.example                    # Base environment variables
│   ├── environments/                       # Environment components
│   │   ├── devcontainer/
│   │   │   └── docker-compose.yml          # Dev container integration
│   │   ├── forwarding/
│   │   │   └── docker-compose.yml          # Development with port forwarding
│   │   ├── internal/
│   │   │   └── docker-compose.yml          # Internal network deployment
│   │   ├── letsencrypt/
│   │   │   ├── docker-compose.yml          # Let's Encrypt SSL
│   │   │   └── .env.example                # Let's Encrypt variables
│   │   └── step-ca/
│   │       ├── docker-compose.yml          # Step CA SSL
│   │       └── .env.example                # Step CA variables
│   └── extensions/                         # Extension components
│       ├── step-ca-trust/                  # Step CA trust extension
│       │   ├── docker-compose.yml          # Step CA trust configuration
│       │   └── .env.example                # Step CA trust variables
│       └── .gitkeep                        # Placeholder for future extensions
├── build/                        # Generated configurations (auto-generated)
│   ├── devcontainer/
│   │   ├── base/                 # Dev container + base configuration
│   │   └── step-ca-trust/        # Dev container + step-ca-trust extension
│   ├── forwarding/
│   │   ├── base/                 # Development + base configuration
│   │   └── step-ca-trust/        # Development + step-ca-trust extension
│   ├── internal/
│   │   ├── base/                 # Internal network + base configuration
│   │   └── step-ca-trust/        # Internal network + step-ca-trust extension
│   ├── letsencrypt/
│   │   ├── base/                 # Let's Encrypt + base configuration
│   │   └── step-ca-trust/        # Let's Encrypt + step-ca-trust extension
│   └── step-ca/
│       ├── base/                 # Step CA + base configuration
│       └── step-ca-trust/        # Step CA + step-ca-trust extension
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

# For development with Step CA trust
cd build/forwarding/step-ca-trust/

# For internal network deployment
cd build/internal/base/

# For internal network with Step CA trust
cd build/internal/step-ca-trust/

# For production with Let's Encrypt SSL
cd build/letsencrypt/base/

# For production with Let's Encrypt SSL and Step CA trust
cd build/letsencrypt/step-ca-trust/

# For production with Step CA SSL
cd build/step-ca/base/

# For production with Step CA SSL and trust
cd build/step-ca/step-ca-trust/

# For dev container integration
cd build/devcontainer/base/

# For dev container with Step CA trust
cd build/devcontainer/step-ca-trust/
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

Access: `http://localhost:29317` (for forwarding mode)

## 🔧 Available Configurations

### Environments

- **devcontainer**: Dev container integration for VS Code
- **forwarding**: Development environment with port forwarding (29317)
- **internal**: Internal network deployment without external access
- **letsencrypt**: Production with Let's Encrypt SSL certificates
- **step-ca**: Production with Step CA SSL certificates

### Extensions

- **step-ca-trust**: Automatic installation of Step CA certificates as trusted for the container

### Generated Combinations

Each environment can be combined with any extension:

- `devcontainer/base` - Dev container integration
- `devcontainer/step-ca-trust` - Dev container with Step CA trust
- `forwarding/base` - Development with port forwarding
- `forwarding/step-ca-trust` - Development with Step CA trust
- `internal/base` - Internal network deployment
- `internal/step-ca-trust` - Internal network with Step CA trust
- `letsencrypt/base` - Production with Let's Encrypt SSL
- `letsencrypt/step-ca-trust` - Production with Let's Encrypt SSL and Step CA trust
- `step-ca/base` - Production with Step CA SSL
- `step-ca/step-ca-trust` - Production with Step CA SSL and trust

## 🔧 Environment Variables

### Base Configuration

- `COMPOSE_PROJECT_NAME`: Project name for Docker Compose (default: mautrix-telegram)
- `CONTAINER_NAME`: Container name for the bridge service (default: mautrix-telegram)

### Matrix Homeserver Configuration

- `MATRIX_TELEGRAM_HOMESERVER`: Matrix homeserver URL (e.g., <https://matrix.example.com>)
- `MATRIX_TELEGRAM_DOMAIN`: Matrix homeserver domain (e.g., matrix.example.com)
- `MATRIX_TELEGRAM_VERIFY_SSL`: Verify SSL certificates for homeserver (default: true)

### Application Service Configuration

- `MATRIX_TELEGRAM_APPSERVICE_ADDRESS`: Public URL for the bridge (e.g., <https://mautrix-telegram.example.com>)
- `MATRIX_TELEGRAM_APPSERVICE_TLS_CERT`: Path to TLS certificate file (optional)
- `MATRIX_TELEGRAM_APPSERVICE_TLS_KEY`: Path to TLS key file (optional)
- `MATRIX_TELEGRAM_APPSERVICE_HOSTNAME`: Bind hostname (default: 0.0.0.0)
- `MATRIX_TELEGRAM_APPSERVICE_PORT`: Bridge port (default: 29317)
- `MATRIX_TELEGRAM_APPSERVICE_DATABASE`: Database connection string (default: sqlite:////data/mautrix-telegram.db)

### Bridge Identity Configuration

- `MATRIX_TELEGRAM_APPSERVICE_ID`: Application service ID (default: telegram)
- `MATRIX_TELEGRAM_APPSERVICE_BOT_USERNAME`: Bridge bot username (default: telegrambot)
- `MATRIX_TELEGRAM_APPSERVICE_BOT_DISPLAYNAME`: Bridge bot display name (default: "Telegram Bridge bot")
- `MATRIX_TELEGRAM_APPSERVICE_BOT_AVATAR`: Bridge bot avatar MXC URL

### Telegram API Configuration

- `MATRIX_TELEGRAM_API_ID`: Telegram API ID (required, get from <https://my.telegram.org>)
- `MATRIX_TELEGRAM_API_HASH`: Telegram API hash (required, get from <https://my.telegram.org>)
- `MATRIX_TELEGRAM_BOT_TOKEN`: Telegram bot token (optional, set to "disabled" if not using)

### Bridge Behavior Configuration

- `MATRIX_TELEGRAM_BRIDGE_USERNAME_TEMPLATE`: Template for Telegram user Matrix IDs (default: telegram_{userid})
- `MATRIX_TELEGRAM_BRIDGE_DISPLAYNAME_TEMPLATE`: Template for Telegram user display names (default: "{displayname} (Telegram)")
- `MATRIX_TELEGRAM_BRIDGE_STARTUP_SYNC`: Sync chats on startup (default: true)
- `MATRIX_TELEGRAM_BRIDGE_SYNC_DIRECT_CHATS`: Sync direct chats (default: true)
- `MATRIX_TELEGRAM_BRIDGE_SYNC_CREATE_LIMIT`: Limit for creating new portals (default: 0 = unlimited)
- `MATRIX_TELEGRAM_BRIDGE_COMMAND_PREFIX`: Command prefix for bridge commands (default: "!tg")
- `MATRIX_TELEGRAM_BRIDGE_BACKFILL_ENABLE`: Enable message backfill (default: false)
- `MATRIX_TELEGRAM_BRIDGE_PERMISSIONS_USERS`: Default permission level for domain users (default: puppeting)

### Let's Encrypt Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 29317)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### Step CA Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 29317)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### Step CA Trust Extension

- `STEP_CA_TRUST`: Enable automatic Step CA certificate trust (default: true)
- `STEP_CA_TRUST_RESTART`: Restart container after trust installation (default: true)

## 🌉 Using Mautrix Telegram Bridge

### Initial Setup

1. **Get Telegram API credentials**:
   - Go to <https://my.telegram.org>
   - Log in with your phone number
   - Create a new application
   - Note down your `api_id` and `api_hash`

2. **Configure the bridge**:
   - Set `MATRIX_TELEGRAM_API_ID` and `MATRIX_TELEGRAM_API_HASH` in your `.env` file
   - Configure your Matrix homeserver URL and domain
   - Set the bridge's public address

3. **Register the bridge with your homeserver**:
   - Start the bridge to generate `registration.yaml`
   - Use the Registration Viewer service to easily access the registration content (see [📋 Registration Viewer](#-registration-viewer) section)
   - Copy the registration content to your Matrix homeserver configuration (Conduit, Synapse, etc.)
   - Restart your Matrix homeserver

### Accessing the Bridge

```bash
# For forwarding mode
http://localhost:29317

# For SSL modes
https://your-domain.com
```

### Connecting Your Telegram Account

1. Start a chat with the bridge bot (`@telegrambot:your-domain.com`)
2. Send `!tg login` to start the login process
3. Follow the instructions to authenticate with Telegram
4. Your Telegram chats will start appearing as Matrix rooms

### Managing the Bridge

- **View status**: Send `!tg status` to the bridge bot
- **Logout**: Send `!tg logout` to disconnect your Telegram account
- **Help**: Send `!tg help` for available commands

## 📋 Registration Viewer

The project includes a separate `docker-compose.viewer.yml` file that contains the `mautrix-telegram-registration-viewer` service. This service is designed specifically for initial bridge setup.

### Purpose

- **One-time setup**: Automatically displays the `registration.yaml` content in container logs
- **Easy access**: No need to manually access container files or volumes
- **Temporary service**: Should be removed after copying the registration to your Matrix homeserver

### Usage

1. **Start the registration viewer** (for initial setup only):

   ```bash
   docker-compose -f docker-compose.yml -f docker-compose.viewer.yml up -d
   ```

2. **View the registration content**:

   ```bash
   docker logs mautrix-telegram-registration-viewer
   ```

3. **Copy the registration** to your Matrix homeserver configuration (Conduit, Synapse, etc.)

4. **Remove the viewer service** (after copying registration):

   ```bash
   docker-compose -f docker-compose.viewer.yml down
   docker-compose -f docker-compose.viewer.yml rm -f
   ```

5. **Continue with normal operation**:

   ```bash
   docker-compose up -d
   ```

### Notes

- The viewer service automatically exits after displaying the registration content
- It waits up to 60 seconds for the `registration.yaml` file to be created
- The service has `restart: "no"` policy, so it won't restart automatically
- Only needed during initial bridge setup - not required for normal operation

## 🛠️ Development

### Adding New Environments

1. Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example` file
2. Run `./build.sh` to generate new combinations

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

- **Development**: `mautrix-telegram-network` (internal)
- **Dev container**: `matrix-bridges-workspace-network` (external)
- **Let's Encrypt**: `letsencrypt-network` (external)
- **Step CA**: `step-ca-network` (external)

## 🔒 Security

⚠️ **Production Checklist:**

- Keep Telegram API credentials secure
- Use strong passwords for authentication
- Configure firewall rules
- Regular security updates
- Set proper homeserver secrets
- Limit bridge permissions appropriately

## 🆘 Troubleshooting

**Build Issues:**

- Ensure `yq` is installed: <https://github.com/mikefarah/yq#install>
- Check component file syntax
- Verify all required files exist

**Matrix Connection Issues:**

- Check homeserver URL correctness
- Ensure registration.yaml is properly configured in homeserver
- Verify service accessibility: `docker logs mautrix-telegram`

**Telegram Connection Issues:**

- Verify API ID and hash are correct
- Check if Telegram API is accessible from your server
- Ensure bot token is valid (if using bot mode)

**SSL Issues:**

- **Let's Encrypt**: Verify domain DNS and letsencrypt-manager
- **Step CA**: Check step-ca-manager and virtual network config

**Bridge Issues:**

- Check bridge logs: `docker logs mautrix-telegram`
- Verify Matrix homeserver accepts the application service
- Ensure database is writable and accessible

## 📝 Notes

- The `build/` directory is automatically generated and should not be edited manually
- Environment variables in generated files use `$VARIABLE_NAME` format for proper interpolation
- Each generated configuration includes a complete `docker-compose.yml`, `docker-compose.viewer.yml`, and `.env.example`
- Missing `.env.*` files for components are handled gracefully by the build script
- The bridge creates a `registration.yaml` file that must be added to your Matrix homeserver configuration
- The `docker-compose.viewer.yml` file contains the registration viewer service for initial setup only

## 🔄 Migration from Legacy Deploy Script

The legacy `deploy.sh` script is still available for compatibility, but the new build system is recommended:

**Legacy approach:**

```bash
./deploy.sh --production --letsencrypt
```

**New approach:**

```bash
./build.sh
cd build/letsencrypt/base/
cp .env.example .env
docker-compose up -d
```

## 📚 Additional Resources

- [Official Mautrix Telegram Documentation](https://docs.mau.fi/bridges/python/telegram/index.html)
- [Mautrix Bridge Setup Guide](https://docs.mau.fi/bridges/general/registering-appservices.html)
- [Matrix.org Documentation](https://matrix.org/docs/)
- [Telegram API Documentation](https://core.telegram.org/api)
