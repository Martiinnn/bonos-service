# bonos-service

Microservicio de **bonos y promociones** del casino (FastAPI). Comparte la base de
datos PostgreSQL y el `JWT_SECRET` con `casino-backend` (valida el JWT del backend,
no tiene login propio). Ofrece bonos (bienvenida, recarga, cashback) que acreditan
saldo y registran la transacción.

- Prefijo de rutas: `/api/bonos` · Docs: `/docs`

## Endpoints
| Método | Ruta | Descripción |
|---|---|---|
| GET | `/api/bonos` | Bonos disponibles |
| GET | `/api/bonos/mis-bonos` | Bonos reclamados por el usuario |
| POST | `/api/bonos/{codigo}/reclamar` | Reclamar un bono (acredita saldo) |

## Ejecutar en local
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
# variables: copia .env.example a .env y ajústalas
uvicorn app.main:app --reload --port 8004
```
Requiere una PostgreSQL accesible con las tablas compartidas (`usuarios`,
`transacciones`) que crea `casino-backend`.

## Despliegue en AWS EKS (Kubernetes) - EA3
Este servicio ha sido desplegado exitosamente en AWS EKS como parte de la Experiencia de Aprendizaje 3.

- **Rutas de salud**: Implementadas (`/health/liveness` y `/health/readiness`).
- **Docker**: Contenerizado mediante `Dockerfile` y alojado en **Amazon ECR**.
- **CI/CD**: Workflow de GitHub Actions configurado para construir y desplegar automáticamente en EKS.
- **Kubernetes**: Manifiestos de `Deployment`, `Service` y `HorizontalPodAutoscaler` aplicados.
- **Pruebas de Carga**: Validado mediante Locust con escalado automático (HPA) funcionando correctamente.
