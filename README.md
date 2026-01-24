# Open USP - Universal Service Protocol

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-active-green.svg)](https://github.com/lucasargate/open-usp)

**Language**: [🇧🇷 Português](#-sobre-o-projeto) | [🇺🇸 English](#-about-the-project)

## 📋 Sobre o Projeto

Open USP é uma implementação open-source do **Universal Service Protocol (USP)**, um protocolo universal projetado para conectar agentes de IA ao mundo real de serviços. Enquanto protocolos como o UCP (Universal Commerce Protocol) do Google resolvem efetivamente o problema para o varejo (produtos estáticos, envio padrão, preços fixos), há um grande vácuo na Economia de Serviços.

O USP foi projetado para preencher essa lacuna, permitindo que agentes de IA descubram, entendam e interajam com serviços do mundo real através de uma interface padronizada e universal.

## 🎯 Objetivos

- **Ponte para Agentes de IA**: Conectar agentes de IA ao mundo real de serviços
- **Economia de Serviços**: Resolver o vácuo na economia de serviços (diferente de produtos estáticos)
- **Interoperabilidade**: Permitir comunicação entre diferentes sistemas e serviços
- **Padronização**: Definir contratos e interfaces universais para serviços
- **Flexibilidade**: Suportar diversos tipos de serviços e casos de uso
- **Extensibilidade**: Permitir extensões e customizações específicas de domínio

## 📚 Documentação

A documentação completa do protocolo está disponível na pasta [`manifest/`](./manifest/):

### Manifesto e Visão

- [Abstract](./manifest/ABSTRACT.md) - Manifesto fundador do USP (Português)
- [Abstract](./manifest/ABSTRACT.en.md) - Founding manifesto of USP (English)

### Especificações Técnicas

**Português:**
- [Manifest Principal](./manifest/README.md) - Visão geral e especificação do protocolo
- [Especificação Técnica](./manifest/specification.md) - Detalhes técnicos e implementação
- [Contratos de Serviço](./manifest/contracts.md) - Definição de contratos e interfaces
- [Mensagens](./manifest/messages.md) - Estrutura de mensagens e payloads
- [Segurança](./manifest/security.md) - Especificações de segurança
- [Extensões](./manifest/extensions.md) - Como criar extensões

**English:**
- [Main Manifest](./manifest/README.en.md) - Overview and protocol specification
- [Technical Specification](./manifest/specification.en.md) - Technical details and implementation
- [Service Contracts](./manifest/contracts.en.md) - Contract and interface definitions
- [Messages](./manifest/messages.en.md) - Message structure and payloads
- [Security](./manifest/security.en.md) - Security specifications
- [Extensions](./manifest/extensions.en.md) - How to create extensions

- [Exemplos](./examples/) - Exemplos de implementação e uso

## 🚀 Início Rápido

### Pré-requisitos

- Node.js 18+ ou Python 3.10+
- pnpm (gerenciador de pacotes)

### Instalação

```bash
# Clone o repositório
git clone https://github.com/lucasargate/open-usp.git
cd open-usp

# Instale as dependências
pnpm install
```

## 📁 Estrutura do Projeto

```
open-usp/
├── manifest/           # Especificação e documentação do protocolo
│   ├── README.md      # Visão geral do manifesto
│   ├── specification.md
│   ├── contracts.md
│   └── ...
├── examples/          # Exemplos de implementação
│   ├── basic/
│   ├── advanced/
│   └── ...
├── src/             # Código-fonte (quando implementado)
└── README.md        # Este arquivo
```

## 🤝 Contribuindo

Contribuições são bem-vindas! Por favor, leia o [AGENTS.md](./AGENTS.md) para entender as diretrizes do projeto.

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](./LICENSE) para mais detalhes.

## 👤 Autor

**Lucas Argate**

- GitHub: [@lucasargate](https://github.com/lucasargate)

## 🙏 Agradecimentos

- Comunidade open-source
- Contribuidores e mantenedores

---

## 🌐 About the Project

Open USP is an open-source implementation of the **Universal Service Protocol (USP)**, a universal protocol designed to bridge the gap between AI agents and the real world of services. While protocols like Google's UCP (Universal Commerce Protocol) effectively solve the problem for Retail (static products, standard shipping, fixed prices), there is a massive vacuum in the Service Economy.

USP is designed to fill this gap, enabling AI agents to discover, understand, and interact with real-world services through a standardized and universal interface.

## 🎯 Goals

- **Bridge for AI Agents**: Connect AI agents to the real world of services
- **Service Economy**: Solve the vacuum in the service economy (unlike static products)
- **Interoperability**: Enable communication between different systems and services
- **Standardization**: Define universal contracts and interfaces for services
- **Flexibility**: Support diverse types of services and use cases
- **Extensibility**: Allow domain-specific extensions and customizations

## 📚 Documentation

The complete protocol documentation is available in the [`manifest/`](./manifest/) folder:

**English:**
- [Main Manifest](./manifest/README.en.md) - Overview and protocol specification
- [Technical Specification](./manifest/specification.en.md) - Technical details and implementation
- [Service Contracts](./manifest/contracts.en.md) - Contract and interface definitions
- [Messages](./manifest/messages.en.md) - Message structure and payloads
- [Security](./manifest/security.en.md) - Security specifications
- [Extensions](./manifest/extensions.en.md) - How to create extensions

**Português:**
- [Manifest Principal](./manifest/README.md) - Visão geral e especificação do protocolo
- [Especificação Técnica](./manifest/specification.md) - Detalhes técnicos e implementação
- [Contratos de Serviço](./manifest/contracts.md) - Definição de contratos e interfaces
- [Mensagens](./manifest/messages.md) - Estrutura de mensagens e payloads
- [Segurança](./manifest/security.md) - Especificações de segurança
- [Extensões](./manifest/extensions.md) - Como criar extensões

- [Examples](./examples/) - Implementation examples and usage

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ or Python 3.10+
- pnpm (package manager)

### Installation

```bash
# Clone the repository
git clone https://github.com/lucasargate/open-usp.git
cd open-usp

# Install dependencies
pnpm install
```

## 📁 Project Structure

```
open-usp/
├── manifest/           # Protocol specification and documentation
│   ├── README.md      # Overview (PT)
│   ├── README.en.md   # Overview (EN)
│   ├── specification.md
│   ├── specification.en.md
│   └── ...
├── examples/          # Implementation examples
│   ├── basic/
│   ├── advanced/
│   └── ...
├── src/             # Source code (when implemented)
└── README.md        # This file
```

## 🤝 Contributing

Contributions are welcome! Please read [AGENTS.md](./AGENTS.md) to understand the project guidelines.

1. Fork the project
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for more details.

## 👤 Author

**Lucas Argate**

- GitHub: [@lucasargate](https://github.com/lucasargate)

## 🙏 Acknowledgments

- Open-source community
- Contributors and maintainers

---

**Note**: This project is under active development. The specification may change as the protocol evolves.
