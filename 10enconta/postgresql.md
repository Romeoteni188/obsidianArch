```bash
insert into tb_roles (nombrerol)
values ('Admin');
```

crear empresa
```bash
INSERT INTO tb_empresa (nombre, "emailEmpre", direccion)
VALUES ('Empresa 10enconta', '10enconta@empresa.com', 'direccion coban zona 12');
```

```bash
INSERT INTO tb_empresa ("nombre", "emailEmpre", "direccion")
VALUES ('Empresa 10', 'correo@empresa.com', 'zona 12');
```

```bash
INSERT INTO tb_usuario
(nombreuser, email, password, "empresaId", "rolId")
VALUES
('romeo', 'romeo@10.com',
 '$2b$10$pT3qU9rkWZwIbQ9n9fI8/eOJX/SakOB54bcntnji3lx1YOifgFZ76',
 1, 1);
```

crear user

```bash
INSERT INTO tb_usuario (nombreuser, email, password, empresaId, rolId)
VALUES ('romeo', 'romeo@10.com', '$2b$10$pT3qU9rkWZwIbQ9n9fI8/eOJX/SakOB54bcntnji3lx1YOifgFZ76', 1, 1);
```

```bash
INSERT INTO tb_usuario ("nombreuser", "email", "password", "empresaId", "rolId")
VALUES ('romeo', 'romeo@10.com', '$2b$10$pT3qU9rkWZwIbQ9n9fI8/eOJX/SakOB54bcntnji3lx1YOifgFZ76', 1, 1)
```


en kubernetes
```bash
INSERT INTO tb_usuario ("nombreuser", "email", "password", "empresaId", "rolId")
VALUES ('romeo', 'romeo@10.com', '$2b$10$pT3qU9rkWZwIbQ9n9fI8/eOJX/SakOB54bcntnji3lx1YOifgFZ76', 1, 1);
```

```bash
INSERT INTO tb_usuario ("nombreuser", "email", "password", "empresaId", "rolId")
VALUES ('Melvin Torres', 'melvintorresg@gmail.com', '$2b$10$uXq00xl4QFObGa7uh9MZQeFb.e6xHKtlhQxOzhIJ/jWsd8fWHVplG', 1, 1);
```

melvin = Iglepay.com#1
Romeo188+ la pass

para migracion 

```bash
pnpm prisma migrate dev --name snake_case_columns
```

se altero en db

```bash
ALTER TABLE "tb_empresa"
ADD COLUMN nit TEXT,
ADD COLUMN telefono TEXT;
```


```bash
ALTER TABLE "tb_empresa"
ADD COLUMN dpi TEXT;
```

cambiar dpi a unico igual nit en la nueva vps

para sincronizar con la db prisma
```bash
pnpm prisma db pull
```


## dasboar
primera tabla es 
- ventas por mes
    
- IVA por periodo
    
- ticket promedio
    
- ventas por cliente

segunda tabla es para 

- Top productos
    
- Top servicios
    
- Cantidad vendida
    
- Mix de ventas

```bash
CREATE TABLE tb_factura (
  id             SERIAL PRIMARY KEY,
  empresa_id     INTEGER NOT NULL,
  fecha_emision  TIMESTAMPTZ NOT NULL,

  cliente_nombre TEXT NOT NULL,
  cliente_id     TEXT,

  tipo_dte       VARCHAR(10) NOT NULL,
  moneda         VARCHAR(5) NOT NULL,

  total          NUMERIC(14,2) NOT NULL,
  iva            NUMERIC(14,2) NOT NULL,

  fcreacion      TIMESTAMPTZ DEFAULT now(),

  CONSTRAINT fk_factura_empresa
    FOREIGN KEY (empresa_id)
    REFERENCES tb_empresa(idempresa)
    ON DELETE CASCADE
);

CREATE INDEX idx_factura_empresa_fecha
  ON tb_factura (empresa_id, fecha_emision);

```

segunda tabla

```bash
CREATE TABLE tb_factura_item (
  id            SERIAL PRIMARY KEY,
  factura_id    INTEGER NOT NULL,

  producto      TEXT NOT NULL,
  tipo          VARCHAR(20) NOT NULL, -- Bien | Servicio

  cantidad      NUMERIC(14,2) NOT NULL,
  precio_unit   NUMERIC(14,2) NOT NULL,
  total         NUMERIC(14,2) NOT NULL,
  iva           NUMERIC(14,2) NOT NULL,

  fcreacion     TIMESTAMPTZ DEFAULT now(),

  CONSTRAINT fk_item_factura
    FOREIGN KEY (factura_id)
    REFERENCES tb_factura(id)
    ON DELETE CASCADE
);

CREATE INDEX idx_item_factura
  ON tb_factura_item (factura_id);

```


isert

```bash
INSERT INTO tb_factura (
  empresa_id,
  fecha_emision,
  cliente_nombre,
  cliente_id,
  tipo_dte,
  moneda,
  total,
  iva
) VALUES (
  1,
  '2025-11-03 08:48:56-06',
  'UNIVERSIDAD PANAMERICANA',
  '1234567',
  'FACT',
  'GTQ',
  8736.00,
  936.00
) RETURNING id;
```

```bash
INSERT INTO tb_factura_item (
  factura_id,
  producto,
  tipo,
  cantidad,
  precio_unit,
  total,
  iva
) VALUES
(
  10,
  'Servicio docente – Módulo de Auditoría',
  'Servicio',
  1,
  8736.00,
  8736.00,
  936.00
),
(
  10,
  'Material académico',
  'Bien',
  2,
  500.00,
  1000.00,
  120.00
);
```

o esta para factura_item
```bash
INSERT INTO tb_factura_item (
  factura_id,
  producto,
  tipo,
  cantidad,
  precio_unit,
  total,
  iva
) VALUES
(
  3,
  'Servicio docente – Módulo de Auditoría',
  'Servicio',
  1,
  8736.00,
  8736.00,
  936.00
),
(
  3,
  'Material académico',
  'Bien',
  2,
  500.00,
  1000.00,
  120.00
);
```

