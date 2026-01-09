***

crear un usario  

crear la contrasena encriptada

```bash
 php -r "echo password_hash('romeo188+', PASSWORD_ARGON2ID);"
```

copiar la encriptada para la db

```bash
Usuario: admin@tenant.com
Contraseña: romeo188+

```

## DB Posgretsql

Entrar 

```bash
docker exec -it postgres_db bash
```

/d para ver las tablas
roles
```bash
INSERT INTO tb_role (nombrerol, descripcion, fechacreacion, idtenant)
VALUES 
('Admin', 'Rol con todos los permisos del sistema', NOW(), NULL),
('Empresa', 'Rol de administración de empresas', NOW(), NULL),
('Colaborador', 'Rol de colaboradores o asesores', NOW(), NULL);

```

tenat

```bash
INSERT INTO tb_tenant (dpi, nombre, email, telefono, fecha_inicio, direccion, fecha_creacion)
VALUES
('1234567890101', 'Tenant Central', 'central@empresa.com', '12345678', '2025-01-01', 'Av. Principal 123', NOW()),
('9876543210101', 'Tenant Norte', 'norte@empresa.com', '87654321', '2025-02-01', 'Calle Norte 45', NOW()),
('5556667770101', 'Tenant Sur', 'sur@empresa.com', '55555555', '2025-03-01', 'Calle Sur 99', NOW());

```


```bash
INSERT INTO tb_user (nombreuser, email, contrasena, fechacreacion, idtenant, idrol)
VALUES
(
    'Admin Principal',
    'admin@central.com',
    '$argon2id$v=19$m=65536,t=4,p=1$UnBVaFFILjhlZ0R5LkhaeA$Z+BE+TjcT8mXb/skdAXLgr0WasTHZGrnRbkS0QnZPWc%',
    NOW(),
    1,  -- idtenant
    1   -- idrol
);

```