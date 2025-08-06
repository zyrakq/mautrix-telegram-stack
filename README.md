# 🌉 Mautrix Telegram Stack

A comprehensive Docker stack for running Mautrix Telegram bridge with certificate management solutions.

Mautrix Telegram is a Matrix-Telegram bridge that allows you to connect your Matrix homeserver to Telegram, enabling seamless communication between Matrix and Telegram users. Perfect for bridging Matrix rooms with Telegram chats and channels.

## 🧩 Components

### 🔐 SSL Automation

#### [🔒 Let's Encrypt Manager](src/ssl-automation/letsencrypt-manager)

Automatic SSL certificate management from Let's Encrypt for production deployments. Provides seamless HTTPS integration for Docker containers using nginx-proxy and acme-companion.
[Learn more about Let's Encrypt Manager configuration](src/ssl-automation/letsencrypt-manager/README.md).

#### [🏠 Step CA Manager](src/ssl-automation/step-ca-manager)

Local domain stack with trusted self-signed certificates for virtual network deployments. Includes private CA management and local DNS resolution for development environments.
[Learn more about Step CA Manager configuration](src/ssl-automation/step-ca-manager/README.md).

## 🌐 Services

### 🌉 Mautrix Telegram Bridge

**Location:** [`src/mautrix-telegram/`](src/mautrix-telegram/)

Matrix-Telegram bridge service that connects Matrix homeservers to Telegram. Supports bidirectional messaging, media transfer, user puppeting, and channel bridging. Includes automatic configuration generation and registration management.

## 🎯 Use Cases

- **🌐 Production Deployment**: Use Mautrix Telegram + Let's Encrypt Manager for public-facing Matrix-Telegram bridges
- **🏠 Internal Networks**: Use Mautrix Telegram + Step CA Manager for private/development environments
- **🔧 Development**: Use Mautrix Telegram standalone for local development and testing
- **🌉 Personal Bridging**: Connect your personal Matrix account to Telegram
- **🏢 Organization Bridging**: Bridge Matrix rooms with Telegram groups and channels
- **🤖 Bot Integration**: Use Telegram bots through Matrix interface
- **📱 Multi-Platform Communication**: Seamless communication across Matrix and Telegram ecosystems

## 🚀 Quick Start

Each component has its own README with detailed setup instructions. Choose the certificate management solution that fits your deployment scenario.

### For Mautrix Telegram Bridge

1. Navigate to the mautrix-telegram directory:

   ```bash
   cd src/mautrix-telegram/
   ```

2. Build configurations:

   ```bash
   ./build.sh
   ```

3. Choose your desired configuration and follow instructions in [src/mautrix-telegram/README.md](src/mautrix-telegram/README.md)

## 📋 Prerequisites

- Docker and Docker Compose
- Matrix homeserver with application service support
- Telegram API credentials (api_id and api_hash from <https://my.telegram.org>)
- Domain name (for production deployments with SSL)

## 🔧 Configuration Overview

The bridge requires:

1. **Matrix Homeserver**: Configure your Matrix server to accept the bridge's application service
2. **Telegram API**: Register your application at <https://my.telegram.org> to get API credentials
3. **Bridge Configuration**: Set up the bridge with proper homeserver and Telegram settings
4. **SSL Certificates**: Choose between Let's Encrypt or Step CA for secure connections

## 📄 License

This project is dual-licensed under:

- [Apache License 2.0](LICENSE-APACHE)
- [MIT License](LICENSE-MIT)
