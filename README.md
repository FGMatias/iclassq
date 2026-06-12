# ICLASSQ — Queue Management System

Sistema de gestión de turnos y colas de atención al cliente.
Migración completa del sistema legacy a stack moderno con arquitectura limpia,
multi-tenancy, y tiempo real via WebSocket.

## Stack

| Capa            | Tecnología                                                   |
| --------------- | ------------------------------------------------------------ |
| Frontend        | React 18 + TypeScript + Vite + Shadcn/ui + TanStack          |
| Backend         | Java 21 + Spring Boot 3 + Spring Security + Spring WebSocket |
| Base de datos   | PostgreSQL 16                                                |
| Infraestructura | Docker + Docker Compose + Nginx                              |

## Arquitectura

Monolito modular con Clean Architecture (Hexagonal) por módulo.
Multi-tenancy por schema. WebSocket reemplaza polling para tiempo real.

## Módulos

- **atencion** — core domain: emisión, llamado, atención, derivación de tickets
- **administracion** — gestión de empresa, sucursal, grupos, usuarios, ventanillas, reportes
- **kiosko** - generación de tickets
- **monitor** — visualización en tiempo real para pantallas públicas

## Documentación

- [Glosario del dominio](docs/glosario.md)
- [Documentación funcional](docs/funcional/)
- [Documentación técnica](docs/tecnica/)
- [Decisiones de arquitectura (ADRs)](docs/tecnica/adr/)

## Estado del proyecto

Fase de documentación y planificación
