## 🗣️ Monólogo – explicación completa para el auditor

> _“Quiero explicarte cómo estamos manejando los regímenes y las frases fiscales en los XML FEL, y también quiero validar desde un  enfoque es correcto desde el punto de vista de auditoría y riesgo.”_

---

### 1️⃣ **Qué hicimos con los regímenes (Frases FEL)**

> _“En los XML FEL, la SAT no envía el régimen como texto, sino como combinaciones de `TipoFrase` y `CodigoEscenario`.  
> Lo que hicimos fue identificar esas combinaciones y mapearlas a un significado fiscal entendible, por ejemplo:_
> 
> - TipoFrase 1 / Escenario 1 → Sujeto a pagos trimestrales ISR
>     
> - TipoFrase 2 / Escenario 1 → Agente de retención del IVA
>     
> - TipoFrase 3 / Escenario 1 → No genera derecho a crédito fiscal
>     
> - TipoFrase 1 / Escenario 2 → Sujeto a retención definitiva ISR Revisar bien
>     
> - TipoFrase 4 / Escenario 5 → Exento de IVA por cuotas periódicas*
>     
> 
> _Esto lo guardamos en una tabla o mapa de regímenes, no para modificar el XML, sino para **interpretarlo y mostrarlo correctamente en el sistema y en los reportes de auditoría**.”_

---

### 2️⃣ **Para qué sirve el “riesgo de régimen”**

> _“Además del texto del régimen, marcamos cada combinación con un nivel de riesgo (bajo, medio o alto).  
> Esto no es fiscalización automática, es una herramienta de control.”_

Ejemplo que le puedes decir:

> _“Si el XML dice ‘No genera derecho a crédito fiscal’ pero el usuario intenta usar ese IVA como crédito en compras, el sistema lo marca como **riesgo alto**.  
> No bloqueamos, pero dejamos evidencia para auditoría.”_

---

### 3️⃣ **Cómo funciona en VENTAS**

> _“En ventas, el régimen afecta principalmente al emisor.”_

Ejemplo:

- Pago trimestral ISR → informativo
    
- Retención definitiva ISR → afecta ingresos netos
    
- Exento IVA → no debe declararse IVA débito
    

> _“En ventas, las frases sirven para **clasificar el ingreso** y validar que los impuestos calculados coincidan con el régimen declarado en el XML.”_

---

### 4️⃣ **Cómo funciona en COMPRAS**

> _“En compras, las frases son todavía más críticas, porque definen si el IVA es acreditable o no.”_

Ejemplo claro para el auditor:

> _“Si en una compra viene la frase ‘No genera derecho a crédito fiscal’, entonces ese IVA **no se puede usar como crédito**, aunque el monto exista en el XML.”_

Aquí es donde el **riesgo** es más importante.

---

### 5️⃣ **Qué pasa con las Notas de Crédito (otro tema, separado)**

> _“Las notas de crédito las estamos tratando como un flujo aparte, porque no son ni venta ni compra normal.”_

Explícalo así:

- En **ventas** → reduce ingresos e IVA débito
    
- En **compras** → reduce costo e IVA crédito
    

> _“Las frases de régimen pueden venir también en notas de crédito, pero su interpretación es la misma; lo que cambia es el efecto contable.”_

👉 Aquí le puedes preguntar:

> _“¿Quieres que las notas de crédito se evalúen con los mismos riesgos fiscales que las facturas, o como un módulo separado?”_

---

### 6️⃣ **Por qué los caracteres con `?` NO se pueden recuperar**

Esta parte es **muy importante**, dila así 👇

> _“Los caracteres como `GARCÍA`, `COBÁN` o `AÑO` aparecen como `GARC?A`, `COB?N`, `A?O` porque el XML fue generado o guardado con un encoding incorrecto.”_

Y deja esto bien claro:

> _“Ese `?` no es un acento mal mostrado: es un **carácter ya perdido**.  
> Cuando el sistema que generó el XML no entendió el acento, lo reemplazó por `?` y la información original se destruyó.”_

⚠️ **No existe forma técnica ni legal de recuperar ese carácter exacto.**

---

### 7️⃣ **Cómo mitigamos ese problema (lo correcto)**

> _“Lo que sí podemos hacer es mitigar el impacto, no corregir el XML.”_

Medidas que puedes mencionar:

1. Validar encoding real del archivo
    
2. Detectar `?` en campos críticos (nombre, dirección)
    
3. Marcar advertencia para auditoría
    
4. Mostrar el texto tal como viene, sin modificarlo
    
5. Dejar evidencia de que el error viene **desde el emisor**
    

> _“Nunca corregimos esos datos automáticamente, porque alterar el XML sería alterar un documento fiscal.”_

---

### 8️⃣ **Preguntas CLAVE que debes hacerle al auditor mañana**

Puedes llevarlas tal cual:

1. **Regímenes**
    
    - _¿Esta interpretación de TipoFrase y CodigoEscenario es válida para auditoría?_
        
    - _¿Hay combinaciones que consideres de riesgo alto automáticamente?_
        
2. **Compras**
    
    - _¿Quieres que el sistema bloquee el crédito fiscal o solo lo marque como advertencia?_
        
3. **Ventas**
    
    - _¿El régimen debe afectar solo reportes o también validaciones contables?_
        
4. **Notas de crédito**
    
    - _¿Las tratamos como módulo independiente o como extensión de ventas/compras?_
        
5. **Encoding**
    
    - _¿Es suficiente dejar evidencia del error de origen o necesitas un reporte formal de calidad del XML?_
        

---

## ✅ Cierre fuerte (muy profesional)

> _“La herramienta no pretende interpretar la ley, sino **reflejar exactamente lo que viene en el XML**, marcar riesgos y dejar evidencia clara para auditoría y cumplimiento fiscal.”_