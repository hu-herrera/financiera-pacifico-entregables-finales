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

## Validación documental — 15 de octubre de 2026
**Fuentes oficiales verificadas:**
- Cisco: [anuncio EoL ISR 4461](https://www.cisco.com/c/en/us/products/collateral/routers/4000-series-integrated-services-routers-isr/isr4461-series-platform-eol.html). Última fecha de pedido 20-01-2025; último soporte 31-01-2030. **No adquirir ISR 4461 para una implantación nueva 2026–2031.** Reemplazo recomendado por Cisco: C8300-2N2S-4T2X.
- Cisco: [familia Catalyst 8300](https://www.cisco.com/c/en/us/support/routers/catalyst-8300-series-edge-platforms/series.html), estado «Available Order» consultado en octubre de 2026. La elección exacta requiere validar interfaces, rendimiento criptográfico y cotización.
- Palo Alto Networks: [ciclo de vida PA-850](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/hardware-end-of-life-dates): fin de venta 31-08-2024, fin de soporte 31-08-2029. **No adquirir PA-850 para una implantación nueva 2026–2031.** El fabricante identifica la familia PA-1400 como reemplazo.
- Amazon: [Amazon Leo](https://www.aboutamazon.com/what-we-do/devices-services/amazon-leo) y [despliegue comercial](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-arianespace-first-mission-satellites). Amazon anunció despliegue gradual en 2026; **no se encontró tarifa comercial pública verificable ni habilitación por dirección en Chile** en las fuentes consultadas. No es legítimo presentar como cotización la partida heredada de terminales.
- Starlink: [servicios de prioridad](https://starlink.com/business/service-plans) y [condiciones de prioridad en Chile](https://starlink.com/cl/support/article/d3dcd79f-c332-63e7-9204-3c3ec4f104ae). Los precios visibles dependen del país, volumen de datos y plan; no equiparar un precio de itinerancia residencial a contrato empresarial.

**Consecuencia para este documento:** toda mención de ISR 4461, PA-850, sus módulos y sus precios debe leerse exclusivamente como **antecedente histórico del borrador anterior**, no como equipamiento de compra aprobado. Para el diseño de adquisición se adopta como **candidato Cisco Catalyst 8300 y Palo Alto PA-1400**; las cantidades de equipos siguen siendo hipótesis de dimensionamiento. Los costos basados en hardware retirado de venta **no representan el presupuesto definitivo**. El cierre financiero requiere cotización o escenarios explícitos con precios estimados para las familias nuevas.
