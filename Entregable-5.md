# Entregable 5 — Prototipo y demostración de la WAN
**Financiera Pacífico | Versión 3.0 | Octubre 2026 | Estado: especificación para actualización posterior del simulador**

## 5.1 Objetivo y alcance
El prototipo es un **simulador web didáctico** de la red de Financiera Pacífico. Permite mostrar topología, estado de enlaces, conmutación, segmentación, seguridad y métricas bajo parámetros controlados. **No ejecuta firmware Cisco IOS-XE ni Palo Alto PAN-OS**, no certifica interoperabilidad y sus valores no son mediciones de una red física.

**Regla de proyecto:** primero cerrar E1–E7 y **después** actualizar el repositorio del simulador. Este entregable documenta requisitos y pruebas esperadas; **no afirma que el simulador actual ya esté corregido**.

## 5.2 Arquitectura funcional del prototipo
- Interfaz web HTML/CSS/JavaScript con vista geográfica y paneles por sede.
- Estado compartido para 23 sedes: CM + S01–S22, con nombre, zona, router, switches, NGFW, accesos WAN, VPN y VLAN.
- Escenarios ANTES y DESPUÉS claramente separados.
- Motor de eventos de falla y recuperación con timestamps y registro reproducible.
- Cálculo didáctico de RTT, pérdida, jitter, throughput y tiempo de conmutación mediante parámetros declarados.
- Panel de evidencia exportable: versión del modelo, parámetros, escenario, resultados y limitaciones.

## 5.3 Contrato de datos obligatorio
| Variable | ANTES | DESPUÉS |
|---|---|---|
| Sedes | 23 | 23 |
| MPLS en CM | 2 × 100 Mbps | **0** |
| MPLS sucursales | 22 × 100 Mbps | **0** |
| OneWeb | **0** | **0** |
| Starlink | No contratado | Acceso candidato |
| Amazon Leo | No contratado | Acceso candidato futuro |
| Entel 5G | No contratado | Acceso candidato |
| GTD 4G/5G | No contratado | Acceso candidato |
| Router WAN | 24 heredados | 24 operativos + 1 repuesto |
| Switches | Inventario legado de referencia | 46 |
| NGFW | 1 central heredado | **24** (2 CM HA + 22 sucursales) |

**Velocidades DESPUÉS:** no se fijan todavía como valores garantizados. Deben corresponder a la matriz final E7 y distinguir velocidad nominal, throughput útil y capacidad por transporte. La selección de ruta no suma automáticamente los cuatro accesos.

## 5.4 Funcionalidades verificables
| ID | Función | Evidencia mínima |
|---|---|---|
| P01 | Visualizar 23 sedes | CM + S01–S22, sin duplicados |
| P02 | Modo ANTES | 24 MPLS, 100 Mbps cada uno |
| P03 | Modo DESPUÉS | 0 MPLS y 0 OneWeb |
| P04 | Falla de enlace principal | Ruta alternativa, alarma, RTO |
| P05 | Falla de respaldo | Primario sigue activo |
| P06 | Falla de router CM | Conmutación a equipo redundante |
| P07 | Falla completa sucursal | Estado aislado, sin inventar continuidad |
| P08 | Caída de proveedor | Rutas alternativas donde existan |
| P09 | Operación celular | Degradación y QoS para servicios críticos |
| P10 | Failback | Retorno estable sin flapping |
| P11 | VLAN/ACL | Invitados aislados del core |
| P12 | IPsec | Estado de túnel y ruta L3, sin simular VLAN extendida |
| P13 | Costos | Coinciden con E7; fuente/supuesto visible |
| P14 | Auditoría | Registro de fallas, acciones y parámetros |

## 5.5 Siete escenarios docentes
1. Pérdida de enlace principal de sucursal.
2. Pérdida de enlace de respaldo.
3. Pérdida del router principal de CM.
4. Caída completa de sucursal.
5. Indisponibilidad del proveedor principal.
6. Funcionamiento degradado mediante 4G/5G.
7. Recuperación y retorno controlado al principal.

Cada ejecución debe generar: ID, sedes afectadas, accesos disponibles, ruta elegida, tiempo simulado, pérdida/RTT/jitter, resultado esperado, resultado observado y observaciones.

## 5.6 Método de validación
**Prueba funcional:** comparar conteos y estados con E1–E4. **Prueba de coherencia:** verificar que costos y tarifas coinciden con E7. **Prueba de reproducibilidad:** repetir escenario con misma semilla y parámetros. **Prueba de interfaz:** comprobar que cambiar de modo ANTES a DESPUÉS elimina MPLS visual y lógicamente, sin dejar rutas ocultas.

El prototipo es una herramienta para exposición y aprendizaje; no se usarán valores simulados para afirmar disponibilidad garantizada ni cumplimiento normativo.

## 5.7 Estado y pendientes
**Estado real a octubre de 2026:** la versión del simulador alojada en `hu-herrera/fin` aún requiere conciliación. Las versiones antiguas contienen referencias a OneWeb, MPLS de 10 Mbps y conteos históricos. **No se han realizado modificaciones al simulador durante la elaboración de este documento.**

**Secuencia acordada:** aprobar modelo maestro → cerrar E1–E7 → actualizar código → ejecutar matriz E6 → adjuntar capturas/resultados reproducibles → cerrar E5 y E6.

**Referencias:** ficha Capstone, observaciones del profesor, E1–E4 v3 y prototipo anterior del repositorio original.