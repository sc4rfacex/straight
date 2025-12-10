# Diagramas C4 Deployment en Mermaid

## Sintaxis Básica (Oficial + Original Verificada)
`C4Deployment`. Nodos: Deployment_Node(alias, "Label", "Type") { ... }. Contenedores: Container(alias, "Label", "Tech").

- Regla absoluta: Deployment_Node siempre con {} y al menos un hijo.
- No usar Node() solo; siempre con contenedor.

## Errores Más Comunes (Del Original)
- Deployment_Node sin {} o hijo: "Expecting 'LBRACE'".
- Llaves no balanceadas: "Parse error - Expecting '}'".
- Usar Node(): "Unknown diagram type".
- Relaciones a Deployment_Node en lugar de Container.

## Patrones Anti-Error (❌ vs ✅)
- ❌ `Deployment_Node(edge, "Edge")` sin hijo → ✅ añade al menos un `Container` interno.
- ❌ Relacionar `Rel(aws, app, ...)` usando nodos → ✅ `Rel(app, db, ...)` relaciona contenedores.
- ❌ Mezclar `Deployment_Node` y `ContainerDb` sin cerrar llaves → ✅ valida balanceo `{` `}` y anidación clara.
- ❌ Repetir alias en distintos niveles → ✅ alias únicos por nodo/contendedor.
- Checklist rápido: cada Deployment_Node tiene hijo, llaves balanceadas, relaciones solo entre Containers/ContainerDb, alias únicos, título opcional al inicio.

## Ejemplos
### Simple
```mermaid
C4Deployment
    Deployment_Node(aws, "AWS") {
        Container(app, "App", "Tech")
    }
```

### Medio
```mermaid
C4Deployment
    title Deployment - Monolito 3 Tier en AWS
    Deployment_Node(aws, "AWS", "Cloud Provider") {
        Deployment_Node(region, "us-east-1", "AWS Region") {
            Deployment_Node(alb_tier, "Load Balancer Tier", "AWS ALB") {
                Container(alb, "Application Load Balancer", "AWS ALB", "Distribución de carga HTTPS")
            }
            Deployment_Node(app_tier, "Application Tier", "EC2 Auto Scaling Group") {
                Deployment_Node(az1, "Availability Zone us-east-1a") {
                    Deployment_Node(ec2_1, "EC2 Instance 1", "t3.medium 2vCPU 4GB RAM") {
                        Container(app_1, "Aplicación Web", "Node.js 18 + Express", "Aplicación monolítica")
                    }
                }
            }
            Deployment_Node(db_tier, "Database Tier", "RDS Multi-AZ") {
                Deployment_Node(rds_primary, "RDS Primary", "db.t3.large 2vCPU 8GB RAM") {
                    ContainerDb(db_master, "PostgreSQL Master", "PostgreSQL 14", "Base de datos principal - Escrituras")
                }
            }
        }
    }
    Rel(alb, app_1, "Distribuye requests", "HTTPS")
```

### Complejo
```mermaid
C4Deployment
    title Deployment - Microservicios en AWS EKS
    Deployment_Node(aws, "AWS Cloud") {
        Deployment_Node(eks_control, "EKS Control Plane", "Managed by AWS") {
            Container(k8s_api, "Kubernetes API Server", "Kubernetes", "Control plane gestionado")
        }
        Deployment_Node(eks_workers, "EKS Worker Nodes", "EC2 Auto Scaling Group") {
            Deployment_Node(ingress_ns, "Namespace: ingress-nginx", "Kubernetes Namespace") {
                Container(ingress_controller, "NGINX Ingress Controller", "NGINX", "Ingress HTTP/HTTPS - 2 replicas")
            }
            Deployment_Node(prod_ns, "Namespace: production", "Kubernetes Namespace") {
                Container(auth_service, "Auth Service", "Go + Gin", "Autenticación JWT - 3 replicas")
                Container(user_service, "User Service", "Python FastAPI", "CRUD usuarios - 3 replicas")
            }
            Deployment_Node(monitoring_ns, "Namespace: monitoring", "Kubernetes Namespace") {
                Container(prometheus, "Prometheus", "Prometheus", "Métricas y alertas")
            }
        }
        Deployment_Node(rds_service, "RDS for PostgreSQL", "Multi-AZ db.r5.xlarge") {
            ContainerDb(postgres_db, "PostgreSQL Database", "PostgreSQL 15", "Base de datos principal")
        }
    }
    Rel(ingress_controller, auth_service, "Route /auth/*", "HTTP")
    Rel(auth_service, postgres_db, "User credentials", "PostgreSQL")
```

### Multi-cloud Activo-Activo (Industria: Fintech)
```mermaid
C4Deployment
    title Multi-cloud activo-activo
    Deployment_Node(aws, "AWS", "Cloud") {
        Deployment_Node(aws_app, "ECS Fargate", "Cluster") {
            Container(api_aws, "API", "Node.js", "Pico de carga US")
        }
        Deployment_Node(aws_db, "Aurora Global", "PostgreSQL") {
            ContainerDb(db_aws, "Writer US", "PostgreSQL 15", "Primario US")
        }
    }
    Deployment_Node(gcp, "GCP", "Cloud") {
        Deployment_Node(gke, "GKE", "Cluster") {
            Container(api_gcp, "API", "Node.js", "Pico de carga EU")
        }
        Deployment_Node(spanner, "Spanner", "DB") {
            ContainerDb(db_gcp, "Reader EU", "Spanner", "Replica sincronizada")
        }
    }
    Rel(api_aws, db_aws, "Writes", "PSQL")
    Rel(api_gcp, db_gcp, "Reads", "gRPC")
    Rel(api_aws, api_gcp, "Health sync", "HTTPS")
```

### Edge + Cloud para IoT Industrial
```mermaid
C4Deployment
    title Edge + Cloud
    Deployment_Node(factory, "Factory Edge", "Industrial PC") {
        Deployment_Node(gateway, "Gateway", "Linux") {
            Container(collector, "Telemetry Collector", "Go", "MQTT")
            ContainerDb(ts_cache, "TS Cache", "SQLite", "Buffer local")
        }
    }
    Deployment_Node(cloud, "Cloud", "Azure") {
        Deployment_Node(iot_hub, "IoT Hub", "PaaS") {
            Container(iothub, "IoT Hub", "Azure", "Ingesta")
        }
        Deployment_Node(streaming, "Stream Analytics", "PaaS") {
            Container(stream_job, "Stream Job", "SQL streaming", "Detecta anomalías")
        }
        Deployment_Node(data, "Data Lake", "ADLS") {
            ContainerDb(raw, "Raw Zone", "Parquet", "Histórico")
        }
    }
    Rel(collector, iothub, "Publica métricas", "MQTT")
    Rel(iothub, stream_job, "Eventos", "EventHub")
    Rel(stream_job, raw, "Persistencia", "Delta Lake")
```

## Buenas Prácticas
- Siempre incluye hijos en Deployment_Node.
- Usa jerarquía: Cloud > Region > AZ > Server > Container.
- Del original: Verifica llaves balanceadas; relaciones a Containers no Nodes.
- Métricas: Capas <5; conexiones seguras 100%.
- Troubleshooting: revisa alias únicos, balanceo de llaves y que todas las relaciones usen contenedores; valida en Mermaid Live Editor.