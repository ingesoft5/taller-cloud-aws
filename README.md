# Laboratorio Práctico: Despliegue en la Nube AWS (EC2 + Docker Compose)
**Ingeniería de Software V (Período 2026-2) · Universidad ICESI**
**Profesor: Alejandro Muñoz**

---

## 🎯 Objetivo del Laboratorio
Desplegar una aplicación web multi-capa (CRUD de Productos) en infraestructura de nube pública utilizando **Amazon Web Services (AWS)** con cuenta personal aprovechando los \$100 USD en créditos de prueba y la capa gratuita (AWS Free Tier).

Stack Tecnológico:
- **Backend:** Spring Boot (Java 17) REST API con almacenamiento en memoria H2.
- **Frontend:** React 18 + Vite + TypeScript servido a través de **Nginx**.
- **Infraestructura Cloud:** Instancia AWS EC2 (Ubuntu 24.04 LTS).
- **Red y Seguridad Cloud:** AWS Virtual Private Cloud (VPC) y Security Groups (Puertos 22, 80, 8080).
- **Orquestación:** Docker Engine y Docker Compose.
- **Observabilidad:** AWS CloudWatch (Métricas de Instancia) y AWS Budgets (Control de Costos).

---

## 📂 Estructura del Proyecto
```
.
├── backend/
│   ├── Dockerfile             # Multi-stage: Maven 3.9 -> Eclipse Temurin JRE 17 Alpine
│   ├── pom.xml                # Dependencias Spring Boot Web, Data JPA, H2, Lombok
│   └── src/                   # Controlador REST, Entidad Product, Repositorio y Servicio
├── frontend/
│   ├── Dockerfile             # Multi-stage: Node 20 Alpine -> Nginx Alpine
│   ├── nginx.conf             # Servidor web de producción y proxy inverso a /api/
│   ├── package.json           # Dependencias React, TypeScript, Tailwind
│   └── src/                   # Componentes UI de catálogo, formulario y servicios API
├── docker-compose.yml         # Orquestador del stack para despliegue en la nube
└── README.md
```

---

## 🚀 Guía de Despliegue en AWS EC2

### 1. Conexión a la Instancia EC2 por SSH
```bash
chmod 400 ~/.ssh/taller-aws-key.pem
ssh -i ~/.ssh/taller-aws-key.pem ubuntu@<TU-IP-PUBLICA-AWS>
```

### 2. Instalación de Docker y Docker Compose en Ubuntu
```bash
# Actualizar repositorios e instalar Docker
sudo apt-get update
sudo apt-get install -y docker.io docker-compose-v2 git

# Permitir ejecución de Docker sin sudo
sudo usermod -aG docker ubuntu
newgrp docker
```

### 3. Clonación del Repositorio y Despliegue
```bash
git clone <URL-DE-TU-REPOSITORIO>
cd taller/codigo_base

# Levantar el stack completo en segundo plano
docker compose up -d --build

# Verificar estado de los contenedores y healthcheck
docker compose ps
```

### 4. Verificación de Endpoints y Aplicación Web
- **Frontend Web:** Abre en tu navegador `http://<TU-IP-PUBLICA-AWS>`
- **API REST Backend:** `http://<TU-IP-PUBLICA-AWS>:8080/api/products` (o `http://<TU-IP-PUBLICA-AWS>/api/products`)

### 5. Parada de Recursos y Control Presupuestal
Al finalizar la práctica o sesión, detén la instancia EC2 desde la consola de AWS (**Instance State -> Stop Instance**) para evitar consumir créditos innecesariamente, y monitorea tus gastos en el panel de **AWS Budgets / Billing**.
