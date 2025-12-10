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

## Buenas Prácticas
- Siempre incluye hijos en Deployment_Node.
- Usa jerarquía: Cloud > Region > AZ > Server > Container.
- Del original: Verifica llaves balanceadas; relaciones a Containers no Nodes.
- Métricas: Capas <5; conexiones seguras 100%.