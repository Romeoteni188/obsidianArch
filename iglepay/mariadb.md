instalacion

```bash
sudo apt update
```

```bash
sudo apt install mariadb-server mariadb-client -y
```

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb
```

configuracion 

```bash
sudo mysql_secure_installation
```

MariaDB ahora está seguro, con:

- Root protegido con contraseña y unix_socket.
    
- Sin usuarios anónimos.
    
- Sin base de datos de prueba.
    
- Root sin acceso remoto.
    

---


## como logearse

sin ingresar contraseña

```bash
sudo mariadb
```

con contraseña 

```bash
mariadb -u root -p
```

iglepay.com#

## Dentro base datos

- crear base de datos

mariaDB[(none)]> CREATE DATABASE IglePayDB;

- ver las bases de datos 

MariaDB [(none)]> SHOW DATABASES;
\c por si hay un error y salir de las flecha ->

## Crear un usurio con todos los  privilegios


    -> \c
MariaDB [(none)]> CREATE USER 'iglepay'@'localhost' IDENTIFIED BY 'iglepay.com#';
Query OK, 0 rows affected (0.002 sec)

MariaDB [(none)]> GRANT ALL PRIVILEGES ON IglePayDB.* TO 'iglepay'@'localhost';
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;

## nuevo usurio 
mariadb -u iglepay -p
la password es iglepay.com#

## ingresar DB

use IglePayDB;

- ver todas las tablas
show tables;

## **backup**

```bash
mysqldump -u iglepay -p IglePayDB > backup_IglePayDB.sql
```

- Automatizar bash y tareas crontab
- crear el archivo y daler permisos
```bash
sudo chmod +x backup.sh
```
- darle permisos y crear el directorio

  ```bash
  sudo mkdir -p /var/backups/mysql
  sudo chown debian:debian /var/backups/mysql
  ```

- crear tarea crontab pasos

abrir cron
```bash
crontab -e
```

agregar  en la lista

```bash
0 */8 * * * /home/debian/backup.sh
```

verificar tareas
```bash
crontab -l
```

ver si se esta ejecutando

```bash
grep CRON /var/log/syslog
```



script para crear las tablas 

```bash


-- Tabla de tenants
CREATE TABLE tb_tenants (
    id_tenant INT AUTO_INCREMENT PRIMARY KEY,
    dpi VARCHAR(20) NOT NULL UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    telefono VARCHAR(20),
    fecha_inicio DATE,
    direccion VARCHAR(255),
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de roles
CREATE TABLE tb_roles (
    id_rol INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL UNIQUE,
    descripcion VARCHAR(255)
);

-- Tabla de usuarios
CREATE TABLE tb_users (
    id_usuario INT AUTO_INCREMENT PRIMARY KEY,
    id_tenant INT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    contrasena VARCHAR(255) NOT NULL,
    id_rol INT NOT NULL,
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_user_tenant FOREIGN KEY (id_tenant) REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE,
    CONSTRAINT fk_user_rol FOREIGN KEY (id_rol) REFERENCES tb_roles(id_rol)
);

```

- Insertar datos 

```bash
INSERT INTO tb_roles (nombre, descripcion) VALUES
('Admin', 'Administrador del sistema'),
('Pastor', 'Lider encargado de la iglesia'),
('Secretario', 'Administra las actividades'),
('Tesorero', 'Administra area financiera'),
('Miembro', 'Usuario estándar');
```

- insertar un tenant 
```bash
INSERT INTO tb_tenants (dpi, nombre, email, telefono, fecha_inicio, direccion)
VALUES ('1234567890101', 'Iglesia Central', 'contacto@iglesia.com', '555-1234', '2025-09-04', 'Zona 1, Ciudad');
```

- insertar un usurio

```bash
INSERT INTO tb_users (id_tenant, nombre, email, contrasena, id_rol)
VALUES (1, 'Melvin Torres', 'iglepay@gmail.com', '$2b$10$R2M8mDDQpU7zNfQ8tIVF1eq7FvCF5UV8hCeReKjnp2rwOzH7DoD3G', 1);
```

- con este comando te describe los campos de las tablas 
```bash
DESCRIBE tb_users;
```

## segunda parte


```bash
-- Tabla de géneros
CREATE TABLE tb_genero (
    idGenero INT AUTO_INCREMENT PRIMARY KEY,
    nombreGenero VARCHAR(50) NOT NULL,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB;

-- Tabla de estado civil
CREATE TABLE tb_estadocivil (
    idEstado INT AUTO_INCREMENT PRIMARY KEY,
    nombreEstado VARCHAR(50) NOT NULL
) ENGINE=InnoDB;

-- Tabla de grupos
CREATE TABLE tb_grupo (
    idGrupo INT AUTO_INCREMENT PRIMARY KEY,
    idTenant INT NOT NULL,
    nombreGrupo VARCHAR(100) NOT NULL,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_grupo_tenant FOREIGN KEY (idTenant) REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE
) ENGINE=InnoDB;

-- Tabla de miembros
CREATE TABLE tb_miembros (
    idMiembro INT AUTO_INCREMENT PRIMARY KEY,
    idTenant INT NOT NULL,
    dpi VARCHAR(20),
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    telefono VARCHAR(20),
    fechaNacimiento DATE,
    idGenero INT,
    direccion VARCHAR(255),
    idEstado INT,
    fechaLlegada DATE,
    bautismo ENUM('S','N') DEFAULT 'N',
    fechaBautismo DATE,
    servidor ENUM('S','N') DEFAULT 'N',
    procesosTerminado VARCHAR(255),
    idGrupo INT,
    leGusta TEXT,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_miembro_tenant FOREIGN KEY (idTenant) REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE,
    CONSTRAINT fk_miembro_genero FOREIGN KEY (idGenero) REFERENCES tb_genero(idGenero),
    CONSTRAINT fk_miembro_estado FOREIGN KEY (idEstado) REFERENCES tb_estadocivil(idEstado),
    CONSTRAINT fk_miembro_grupo FOREIGN KEY (idGrupo) REFERENCES tb_grupo(idGrupo)
) ENGINE=InnoDB;

-- Tabla de relación muchos a muchos entre miembros y grupos
CREATE TABLE tb_miembrogrupo (
    idMiembro INT NOT NULL,
    idGrupo INT NOT NULL,
    fechaUnion DATE DEFAULT CURRENT_DATE,
    PRIMARY KEY (idMiembro, idGrupo),
    CONSTRAINT fk_miembrogrupo_miembro FOREIGN KEY (idMiembro) REFERENCES tb_miembros(idMiembro) ON DELETE CASCADE,
    CONSTRAINT fk_miembrogrupo_grupo FOREIGN KEY (idGrupo) REFERENCES tb_grupo(idGrupo) ON DELETE CASCADE
) ENGINE=InnoDB;


```

Insertar generos

```bash
INSERT INTO tb_genero (nombreGenero) VALUES
('Masculino'),
('Femenino');
```

Insertar estado civil

```bash
INSERT INTO tb_estadocivil (nombreEstado) VALUES
('Soltero'),
('Casado'),
('Divorciado'),
('Viudo');
```

insertar miembro tenant 1 la primera iglesia

se hizo un alter a la tabla miembro
```bash
ALTER TABLE tb_miembros 
MODIFY COLUMN bautismo VARCHAR(10) DEFAULT 'N',
MODIFY COLUMN servidor VARCHAR(10) DEFAULT 'N';

```

## cuarta parte 

```bash
CREATE TABLE tb_limpieza (
    idLimpieza INT AUTO_INCREMENT PRIMARY KEY,
    idTenant INT NOT NULL,
    idMiembro INT NOT NULL,
    fechaLimpieza DATE NOT NULL,
    fechaCreacion DATETIME DEFAULT CURRENT_TIMESTAMP,
    
    -- Claves foráneas
    CONSTRAINT fk_limpieza_tenant
        FOREIGN KEY (idTenant) REFERENCES tb_tenants(id_tenant)
        ON DELETE CASCADE
        ON UPDATE CASCADE,
        
    CONSTRAINT fk_limpieza_miembro
        FOREIGN KEY (idMiembro) REFERENCES tb_miembros(idMiembro)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);

```

insert

```bash
INSERT INTO tb_limpieza (idTenant, idMiembro, fechaLimpieza)
VALUES 
(1, 1, '2025-09-01'),
(1, 2, '2025-09-02'),
(2, 3, '2025-09-03');
```

```bash
CREATE TABLE tb_actividades (
    idActividad INT AUTO_INCREMENT PRIMARY KEY,
    idTenant INT NOT NULL,
    idMiembro INT NOT NULL,
    idGrupo INT,
    titulo VARCHAR(150) NOT NULL,
    descripcion TEXT,
    fechaActividad DATE NOT NULL,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    -- Claves foráneas
    CONSTRAINT fk_actividad_tenant
        FOREIGN KEY (idTenant) REFERENCES tb_tenants(id_tenant)
        ON DELETE CASCADE
        ON UPDATE CASCADE,
        
    CONSTRAINT fk_actividad_miembro
        FOREIGN KEY (idMiembro) REFERENCES tb_miembros(idMiembro)
        ON DELETE CASCADE
        ON UPDATE CASCADE,
        
    CONSTRAINT fk_actividad_grupo
        FOREIGN KEY (idGrupo) REFERENCES tb_grupo(idGrupo)
        ON DELETE SET NULL
        ON UPDATE CASCADE
) ENGINE=InnoDB;

```

inserte
```bash
INSERT INTO tb_actividades (idTenant, idMiembro, idGrupo, titulo, descripcion, fechaActividad)
VALUES 
(1, 1, 1, 'Convivio de Jóvenes', 'Un tiempo especial de convivencia, música y reflexión.', '2025-09-20'),

(1, 1, 1, 'Campamento Espiritual', 'Retiro de fin de semana con estudios bíblicos, dinámicas y caminatas.', '2025-10-10'),

(1, 1, 1, 'Reunión de Líderes', 'Encuentro para planificar actividades del próximo trimestre.', '2025-09-25'),

(1, 1, 1, 'Noche de Alabanza', 'Evento con música en vivo y participación de diferentes ministerios.', '2025-11-05');

```

hacer un alter 

```bash
ALTER TABLE tb_limpieza
ADD COLUMN idGrupo INT NULL,
ADD CONSTRAINT fk_limpieza_grupo
  FOREIGN KEY (idGrupo) REFERENCES tb_grupo(idGrupo)
  ON DELETE SET NULL;

```

verificar 

```bash
DESCRIBE tb_limpieza;
```

tablas auxiliares 

```bash

-- Tabla de bautizados
CREATE TABLE tb_bautizados (
    idBautizado INT AUTO_INCREMENT PRIMARY KEY,
    idMiembro INT NOT NULL,
    idTenant INT NOT NULL,
    estado VARCHAR(5), -- 'S' | 'N'
    fechaBautismo DATE,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_bautizado_miembro FOREIGN KEY (idMiembro) REFERENCES tb_miembros(idMiembro) ON DELETE CASCADE,
    CONSTRAINT fk_bautizado_tenant FOREIGN KEY (idTenant) REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE
) ENGINE=InnoDB;

-- Tabla de servidores
CREATE TABLE tb_servidores (
    idServidor INT AUTO_INCREMENT PRIMARY KEY,
    idMiembro INT NOT NULL,
    idTenant INT NOT NULL,
    estado VARCHAR(5), -- 'S' | 'N'
    fechaInicio DATE,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_servidor_miembro FOREIGN KEY (idMiembro) REFERENCES tb_miembros(idMiembro) ON DELETE CASCADE,
    CONSTRAINT fk_servidor_tenant FOREIGN KEY (idTenant) REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE
) ENGINE=InnoDB;

-- Alter tabla tb_miembros para quitar default
ALTER TABLE tb_miembros
MODIFY bautismo VARCHAR(5),
MODIFY servidor VARCHAR(5);


```

insert 
```bash
-- Insertar un miembro bautizado
INSERT INTO tb_bautizados (idMiembro, idTenant, estado, fechaBautismo)
VALUES
(1, 1, 'NO', '2025-01-20'),  
(3, 1, 'SI', '2025-01-20');  

-- Insertar un miembro servidor
INSERT INTO tb_servidores (idMiembro, idTenant, estado, fechaInicio)
VALUES
(1, 1, 'NO', '2025-01-20'),           
(2, 1, 'SI', '2025-01-20');   

```

## Modificaciones de las tablas

```bash

-- ===========================
-- TABLA DE BAUTIZADOS
-- ===========================
CREATE TABLE tb_bautizados (
    idBautizado INT AUTO_INCREMENT PRIMARY KEY,
    idTenant INT NOT NULL,
    bautizadoEstado VARCHAR(5), -- 'S' o 'N'
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_bautizado_tenant FOREIGN KEY (idTenant)
        REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE
) ENGINE=InnoDB;

-- ===========================
-- TABLA DE SERVIDORES
-- ===========================
CREATE TABLE tb_servidores (
    idServidor INT AUTO_INCREMENT PRIMARY KEY,
    idTenant INT NOT NULL,
    servidorEstado VARCHAR(5), -- 'S' o 'N'
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_servidor_tenant FOREIGN KEY (idTenant)
        REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE
) ENGINE=InnoDB;

-- ===========================
-- TABLA DE MIEMBROS
-- ===========================
CREATE TABLE tb_miembros (
    idMiembro INT AUTO_INCREMENT PRIMARY KEY,
    idTenant INT NOT NULL,
    dpi VARCHAR(20),
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    telefono VARCHAR(20),
    fechaNacimiento DATE,
    idGenero INT,
    direccion VARCHAR(255),
    idEstado INT,
    fechaLlegada DATE,
    idBautizado INT,
    fechaBautismo DATE,
    idServidor INT,
    procesosTerminado VARCHAR(255),
    idGrupo INT,
    leGusta TEXT,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    -- Relaciones
    CONSTRAINT fk_miembro_tenant FOREIGN KEY (idTenant)
        REFERENCES tb_tenants(id_tenant) ON DELETE CASCADE,
    CONSTRAINT fk_miembro_bautizado FOREIGN KEY (idBautizado)
        REFERENCES tb_bautizados(idBautizado) ON DELETE SET NULL,
    CONSTRAINT fk_miembro_servidor FOREIGN KEY (idServidor)
        REFERENCES tb_servidores(idServidor) ON DELETE SET NULL,
    CONSTRAINT fk_miembro_genero FOREIGN KEY (idGenero)
        REFERENCES tb_genero(idGenero) ON DELETE SET NULL,
    CONSTRAINT fk_miembro_estado FOREIGN KEY (idEstado)
        REFERENCES tb_estadocivil(idEstado) ON DELETE SET NULL,
    CONSTRAINT fk_miembro_grupo FOREIGN KEY (idGrupo)
        REFERENCES tb_grupo(idGrupo) ON DELETE SET NULL
) ENGINE=InnoDB;

```

## Area financiera
tablas nueva 

```bash
-- ============================================
-- Tabla: tb_nomeclatura (Catálogo de cuentas)
-- ============================================
CREATE TABLE tb_nomeclatura (
    idnomeclatura INT AUTO_INCREMENT PRIMARY KEY,
    tenantId INT NOT NULL,
    codigo VARCHAR(20) NOT NULL,
    nombre VARCHAR(50) NOT NULL,
    tipoIE VARCHAR(10) NOT NULL, -- 'Ingreso' o 'Egreso'
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT uq_tb_nomeclatura_codigo UNIQUE (tenantId, codigo)
) ENGINE=InnoDB;

-- ============================================
-- Tabla: tb_operacion (Movimientos financieros)
-- ============================================
CREATE TABLE tb_operacion (
    idoperacion INT AUTO_INCREMENT PRIMARY KEY,
    tenantId INT NOT NULL,
    idnomeclatura INT NOT NULL,
    fecha DATE NOT NULL,
    descripcion VARCHAR(150) NOT NULL,
    monto DECIMAL(12,2) NOT NULL,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT fk_tb_operacion_nomeclatura
        FOREIGN KEY (idnomeclatura) REFERENCES tb_nomeclatura(idnomeclatura)
        ON UPDATE CASCADE ON DELETE RESTRICT
) ENGINE=InnoDB;

```

insert

```bash
-- ============================================
-- INSERT EN TB_NOMECLATURA
-- ============================================
INSERT INTO tb_nomeclatura (tenantId, codigo, nombre, tipoIE)
VALUES
(1, 'I001', 'Donación', 'Ingreso'),
(1, 'I002', 'Ofrenda', 'Ingreso'),
(1, 'E001', 'Pago Energía', 'Egreso'),
(1, 'E002', 'Pago Agua', 'Egreso');

-- ============================================
-- INSERT EN TB_OPERACION
-- ============================================
INSERT INTO tb_operacion (tenantId, idnomeclatura, fecha, descripcion, monto)
VALUES
(1, 1, '2025-09-20', 'Donación especial', 500.00),   -- Ingreso
(1, 2, '2025-09-21', 'Ofrenda de la mañana', 200.00), -- Ingreso
(1, 3, '2025-09-22', 'Pago de energía eléctrica', 150.00), -- Egreso
(1, 4, '2025-09-23', 'Pago de agua potable', 50.00);  -- Egreso
```

resultado 
```bash
SELECT 
    SUM(CASE 
            WHEN n.tipoIE = 'Ingreso' THEN o.monto
            ELSE -o.monto
        END) AS saldo_actual
FROM tb_operacion o
INNER JOIN tb_nomeclatura n 
    ON o.idnomeclatura = n.idnomeclatura
WHERE o.tenantId = 1;

```

Libro diario view

```bash
-- ============================================
-- VIEW: v_libro_diario_por_tenant
-- ============================================
CREATE OR REPLACE VIEW v_libro_diario_por_tenant AS
SELECT
    o.tenantId,
    o.idoperacion,
    o.fecha,
    n.codigo AS codigo_nomenclatura,
    n.nombre AS nombre_nomenclatura,
    n.tipoIE,
    o.descripcion,
    o.monto,
    SUM(
        CASE 
            WHEN n.tipoIE = 'Ingreso' THEN o.monto
            ELSE -o.monto
        END
    ) OVER (PARTITION BY o.tenantId ORDER BY o.fecha, o.idoperacion) AS saldo_acumulado,
    o.fechaCreacion
FROM tb_operacion o
INNER JOIN tb_nomeclatura n 
    ON o.idnomeclatura = n.idnomeclatura;

```

el uso vista 
```bash
SELECT * 
FROM v_libro_diario_por_tenant
WHERE tenantId = 1
ORDER BY fecha, idoperacion;
```



libro mayor 
```bash
-- ============================================
-- VIEW: v_libro_diario_por_tenant
-- ============================================
CREATE OR REPLACE VIEW v_libro_diario_por_tenant AS
SELECT
    o.tenantId,
    o.idoperacion,
    o.fecha,
    n.codigo AS codigo_nomenclatura,
    n.nombre AS nombre_nomenclatura,
    n.tipoIE,
    o.descripcion,
    o.monto,
    SUM(
        CASE 
            WHEN n.tipoIE = 'Ingreso' THEN o.monto
            ELSE -o.monto
        END
    ) OVER (PARTITION BY o.tenantId ORDER BY o.fecha, o.idoperacion) AS saldo_acumulado,
    o.fechaCreacion
FROM tb_operacion o
INNER JOIN tb_nomeclatura n 
    ON o.idnomeclatura = n.idnomeclatura;

```

select 

```bash
SELECT * 
FROM v_libro_mayor_por_tenant
WHERE tenantId = 1
ORDER BY idnomeclatura, fecha, idoperacion;

```


## bitacora 

```bash
DELIMITER //

CREATE TABLE IF NOT EXISTS tb_auditoria_operacion (
    id_auditoria INT AUTO_INCREMENT PRIMARY KEY,
    idoperacion INT,
    tenantId INT,
    idnomeclatura INT,
    descripcion VARCHAR(150),
    monto DECIMAL(12,2),
    tipo_accion ENUM('INSERT','UPDATE','DELETE'),
    usuario_id INT,
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB;

//

CREATE TRIGGER trg_operacion_insert
AFTER INSERT ON tb_operacion
FOR EACH ROW
BEGIN
    INSERT INTO tb_auditoria_operacion 
        (idoperacion, tenantId, idnomeclatura, descripcion, monto, tipo_accion, usuario_id)
    VALUES 
        (NEW.idoperacion, NEW.tenantId, NEW.idnomeclatura, NEW.descripcion, NEW.monto, 'INSERT', @current_user_id);
END; //

DELIMITER ;

```

para selecionar quien modifico la tabla 

```bash
SELECT * 
FROM tb_auditoria_operacion
WHERE tenantId = 1
ORDER BY fecha DESC;
```

## Ultima parte 

```bash
-- ============================================
-- TABLA: tb_familias
-- ============================================
CREATE TABLE IF NOT EXISTS tb_familias (
    idfamilia INT AUTO_INCREMENT PRIMARY KEY,
    tenantId INT NOT NULL,
    idMiembro INT,  -- opcional, líder de la familia
    nombreFamilia VARCHAR(100) NOT NULL,
    cantidadfamilia INT DEFAULT 0,
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB;

-- ============================================
-- TABLA: tb_servicios
-- ============================================
CREATE TABLE IF NOT EXISTS tb_servicios (
    idservicio INT AUTO_INCREMENT PRIMARY KEY,
    tenantId INT NOT NULL,
    horario VARCHAR(50) NOT NULL,    -- Ahora es string para cualquier horario
    descripcion VARCHAR(150),
    fechaCreacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) ENGINE=InnoDB;

-- ============================================
-- TABLA: tb_asistencia
-- ============================================
CREATE TABLE IF NOT EXISTS tb_asistencia (
    idasistencia INT AUTO_INCREMENT PRIMARY KEY,
    tenantId INT NOT NULL,
    idfamilia INT NOT NULL,
    idservicio INT NOT NULL,
    cantidad_asistentes INT NOT NULL,
    fechaRegistro TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_asistencia_familia FOREIGN KEY (idfamilia) REFERENCES tb_familias(idfamilia),
    CONSTRAINT fk_asistencia_servicio FOREIGN KEY (idservicio) REFERENCES tb_servicios(idservicio)
) ENGINE=InnoDB;

```

familias
```bash
INSERT INTO tb_familias (tenantId, nombreFamilia, cantidadfamilia) VALUES
(1, 'Familia López', 4),
(1, 'Familia Pérez', 3),
(1, 'Familia Morales', 5);
```

servicios

```bash
INSERT INTO tb_servicios (tenantId, horario, descripcion) VALUES
(1, 'Domingo 08:00 AM', 'Servicio de la mañana'),
(1, 'Domingo 11:00 AM', 'Servicio de mediodía'),
(1, 'Miércoles 07:00 PM', 'Servicio semanal'),
(1, 'Viernes 07:00 PM', 'Servicio especial');
```

asistencia 

```bash
INSERT INTO tb_asistencia (tenantId, idfamilia, idservicio, cantidad_asistentes) VALUES
-- Familia López
(1, 1, 1, 4),
(1, 1, 2, 3),
(1, 1, 3, 4),
(1, 1, 4, 4),

-- Familia Pérez
(1, 2, 1, 3),
(1, 2, 2, 2),
(1, 2, 3, 3),
(1, 2, 4, 3),

-- Familia Morales
(1, 3, 1, 5),
(1, 3, 2, 5),
(1, 3, 3, 5),
(1, 3, 4, 4);
```

select 

```bash
SELECT s.horario, s.descripcion, SUM(a.cantidad_asistentes) AS total_asistentes
FROM tb_asistencia a
INNER JOIN tb_servicios s ON a.idservicio = s.idservicio
WHERE a.tenantId = 1
GROUP BY s.idservicio
ORDER BY s.fechaCreacion;

```

### Alter tb _familia_
```bash
ALTER TABLE tb_familias DROP COLUMN idMiembro;
```

ahora  crear tabla de muchos a muchos 
```bash
CREATE TABLE tb_familia_miembro (
  idfamilia INT NOT NULL,
  idMiembro INT NOT NULL,
  PRIMARY KEY (idfamilia, idMiembro),
  CONSTRAINT fk_familia FOREIGN KEY (idfamilia) REFERENCES tb_familias(idfamilia) ON DELETE CASCADE,
  CONSTRAINT fk_miembro FOREIGN KEY (idMiembro) REFERENCES tb_miembros(idMiembro) ON DELETE CASCADE
);

```

### alter tb_asistencia

```bash
ALTER TABLE tb_asistencia
ADD COLUMN fechaServicio DATE AFTER cantidad_asistentes;

```


### Alter tb_operacion

```bash
ALTER TABLE tb_operacion ADD COLUMN isDeleted TINYINT(1) DEFAULT 0;
```

para restaurar  de una eliminacion de un datos 

este es un ejemplo como restaurar 

```bash

MariaDB [IglePayDB]> select * from tb_operacion where tenantId=1;
+-------------+----------+---------------+------------+--------------------------------+--------+---------------------+-----------+
| idoperacion | tenantId | idnomeclatura | fecha      | descripcion                    | monto  | fechaCreacion       | isDeleted |
+-------------+----------+---------------+------------+--------------------------------+--------+---------------------+-----------+
|           1 |        1 |             1 | 2025-09-20 | Donación especial              | 500.00 | 2025-09-20 22:44:12 |         0 |
|           2 |        1 |             2 | 2025-09-21 | Ofrenda de la mañana           | 200.00 | 2025-09-20 22:44:12 |         0 |
|           3 |        1 |             3 | 2025-09-22 | Pago de energía eléctrica      | 150.00 | 2025-09-20 22:44:12 |         0 |
|           4 |        1 |             4 | 2025-09-23 | Pago de agua potable           |  50.00 | 2025-09-20 22:44:12 |         0 |
|           5 |        1 |             1 | 2025-09-17 | Para la construcion de iglesia | 200.00 | 2025-09-24 20:57:09 |         0 |
|           6 |        1 |            10 | 2025-09-27 | Basura del dia del niño        |  10.00 | 2025-09-24 22:04:39 |         1 |
+-------------+----------+---------------+------------+--------------------------------+--------+---------------------+-----------+

```

```bash
UPDATE tb_operacion
SET isDeleted = 0
WHERE idoperacion = 6
  AND tenantId = 1;

```

##  tabla permisos
```bash
-- Crear tabla de permisos
CREATE TABLE tb_permission (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL UNIQUE,
    descripcion VARCHAR(255) NULL
);

-- Tabla intermedia entre roles y permisos (muchos a muchos)
CREATE TABLE tb_role_permissions (
    role_id INT NOT NULL,
    permission_id INT NOT NULL,
    PRIMARY KEY (role_id, permission_id),
    CONSTRAINT fk_role FOREIGN KEY (role_id) REFERENCES tb_roles(id_rol) ON DELETE CASCADE,
    CONSTRAINT fk_permission FOREIGN KEY (permission_id) REFERENCES tb_permission(id) ON DELETE CASCADE
);
```

```bash
INSERT INTO tb_permission (nombre, descripcion) VALUES
('ver_finanzas', 'Acceso al módulo de Finanzas'),
('editar_finanzas', 'Puede modificar registros financieros'),
('eliminar_usuario', 'Puede eliminar usuarios del sistema'),
('ver_miembros', 'Puede ver la lista de miembros'),
('editar_miembros', 'Puede editar datos de los miembros');
```

```bash
INSERT INTO tb_role_permissions (role_id, permission_id) VALUES
(1, 1), -- Pastor puede ver finanzas
(1, 2), -- Pastor puede editar finanzas
(1, 3); -- Pastor puede eliminar usuarios
```

```bash
INSERT INTO tb_role_permissions (role_id, permission_id) VALUES
(2, 4), -- Secretario puede ver miembros
(2, 5); -- Secretario puede editar miembros
```

###  Alter foto perfil

```bash
npx prisma migrate dev --name add_foto_perfil_to_user
```

