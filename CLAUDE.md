# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Baetyl is an open edge computing framework that extends cloud computing, data and service seamlessly to edge devices. This Go-based project provides edge computing capabilities including device connection, message routing, remote synchronization, function computing, and AI inference.

## Development Commands

### Building
- `make all` - Build the project for current platform
- `make build` - Build for all configured platforms
- `make build-local` - Build for local development (creates `baetyl` binary)
- `go mod tidy` - Clean up Go module dependencies

### Testing
- `make test` - Run unit tests with coverage (required before submitting changes)
- `go test ./...` - Run all tests
- `go fmt ./...` - Format Go code

### Development Workflow
- Must run `make test` before pushing changes - all unit tests and data race tests must pass
- Follow [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments) for code style
- Use `git commit --amend` to reduce unnecessary commits

## Architecture

### Core Components

- **cmd/** - CLI command handling and main entry points
- **core/** - Core service functionality (`core.go:StartCoreService`)
- **initz/** - Node initialization service (`initz.go:StartInitService`)
- **engine/** - Application deployment and lifecycle management
- **sync/** - Data synchronization between edge and cloud using shadow (Report/Desire) pattern
- **ami/** - Abstract Machine Interface with implementations:
  - `ami/kube/` - Kubernetes mode implementation
  - `ami/native/` - Native mode implementation (lightweight)

### Plugin System

- **plugin/httplink/** - HTTP link protocol
- **plugin/mqttlink/** - MQTT link protocol
- **plugin/wslink/** - WebSocket link protocol
- **plugin/pubsub/** - Pub/Sub messaging
- **plugin/nodestats/** - Node statistics collection
- **plugin/nvstats/** - NVIDIA GPU statistics
- **plugin/qpsstats/** - QPS statistics

### Key Services

The project registers service hooks in `main.go`:
- `HookNameStartCoreService` → `core.StartCoreService`
- `HookNameStartInitService` → `initz.StartInitService`
- `HookGetFingerprint` → `cmd.GetFingerprint`

### Data Flow

1. **baetyl-init** - Activates edge node to cloud, initializes baetyl-core
2. **baetyl-core** - Manages local node, syncs with cloud, deploys applications
3. **baetyl-function** - Proxy for function runtime services

## Dependencies

- Go 1.18+ required
- Kubernetes/K3S for deployment
- Key external libraries: cobra (CLI), fasthttp (HTTP), bbolt (storage), helm (K8s deployment)

## Testing

- All plugins and core components have comprehensive test coverage
- Tests use golang/mock for mocking dependencies
- Race condition testing is mandatory (`-race` flag in Makefile)

## Reply Guidelines
- Always reference **file path + function name** when explaining code.
- Use **Mermaid diagrams** for flows, call chains, and module dependencies.
- If context is missing, ask explicitly which files to `/add`.
- Never hallucinate non-existing functions or files.

## Excluded Paths
- vendor/
- build/
- dist/
- .git/
- third_party/

## Glossary


## Run Instructions

