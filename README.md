# NestjsCRUD

API REST de aprendizaje que implementa un CRUD de productos con NestJS 10, Prisma y SQLite. Incluye DTOs, manejo de errores de Prisma y estructura de testing generada por el CLI de Nest.

![NestJS](https://img.shields.io/badge/NestJS%2010-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=prisma&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Jest](https://img.shields.io/badge/Jest-C21325?style=for-the-badge&logo=jest&logoColor=white)

## Estado del proyecto

Proyecto de aprendizaje con un CRUD completo y funcional para la entidad `Product`. El paquete en `package.json` se llama `restapi-backend` y la licencia es `UNLICENSED`. El archivo SQLite (`prisma/dev.db`) vive en el repo como parte del entorno de desarrollo.

## Stack

- Framework: NestJS 10 (`@nestjs/common`, `@nestjs/core`, `@nestjs/platform-express`, `@nestjs/mapped-types`)
- Lenguaje: TypeScript 5.1
- ORM: Prisma 6 con `@prisma/client`
- BD: SQLite (configurable vía `DATABASE_URL`)
- Testing: Jest 29 + Supertest
- Tooling: ESLint 8, Prettier 3

## Requisitos previos

- Node.js >= 18
- npm

## Instalación

```bash
git clone https://github.com/JeanCaicedo/NestjsCRUD.git
cd NestjsCRUD
npm install
```

## Variables de entorno

Crea un archivo `.env` con:

- `DATABASE_URL` — cadena de conexión de Prisma. Para SQLite local: `file:./dev.db`.

## Base de datos

```bash
# Aplicar migraciones en desarrollo
npx prisma migrate dev

# Generar el cliente de Prisma
npx prisma generate

# Explorar datos con Prisma Studio
npx prisma studio
```

## Ejecución

```bash
# Desarrollo con recarga en caliente
npm run start:dev

# Producción
npm run build
npm run start:prod
```

Por defecto el servidor queda en `http://localhost:3000`.

## Scripts disponibles

| Script | Descripción |
| --- | --- |
| `start` | Arranca la aplicación |
| `start:dev` | Arranca en modo watch |
| `start:debug` | Arranca con `--debug --watch` |
| `start:prod` | Ejecuta el build compilado (`dist/main`) |
| `build` | Compila el proyecto con `nest build` |
| `format` | Aplica Prettier sobre `src/` y `test/` |
| `lint` | Corre ESLint con `--fix` |
| `test` | Ejecuta tests unitarios con Jest |
| `test:cov` | Jest con cobertura |
| `test:e2e` | Ejecuta tests end-to-end |

## Modelo de datos

```prisma
model Product {
  id          Int      @id @default(autoincrement())
  name        String   @unique
  description String?
  price       Float
  image       String?
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

## Endpoints

Recurso `products` persistido vía Prisma:

| Método | Ruta | Descripción |
| --- | --- | --- |
| POST | `/products` | Crea un producto (body tipo `CreateProductDto`). Responde 409 si el `name` está duplicado (código Prisma `P2002`) |
| GET | `/products` | Lista todos los productos |
| GET | `/products/:id` | Obtiene un producto por id (404 si no existe) |
| PATCH | `/products/:id` | Actualiza un producto (body tipo `UpdateProductDto`) |
| DELETE | `/products/:id` | Elimina un producto |

`CreateProductDto` se deriva del modelo Prisma como `Omit<Product, 'id' | 'createdAt' | 'updatedAt'>` y `UpdateProductDto` lo extiende como parcial.

## Estructura del proyecto

```
src/
├── app.module.ts
├── main.ts
├── prisma/
│   └── prisma.service.ts
└── products/
    ├── dto/
    │   ├── create-product.dto.ts
    │   └── update-product.dto.ts
    ├── entities/
    │   └── product.entity.ts
    ├── products.controller.ts
    ├── products.controller.spec.ts
    ├── products.module.ts
    ├── products.service.ts
    └── products.service.spec.ts
prisma/
└── schema.prisma
test/                          # Tests e2e
```

## Autor

Jean Caicedo — [@JeanCaicedo](https://github.com/JeanCaicedo)
