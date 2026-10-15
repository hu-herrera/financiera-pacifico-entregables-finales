# Entregable 1 — Diagnóstico y definición del problema
**Proyecto:** Modernización WAN y continuidad operacional de Financiera Pacífico  
**Versión:** 3.0 — borrador técnico de revisión, octubre de 2026  
**Horizonte:** cinco años  
**Estado:** Documento de trabajo. Los supuestos de mercado, equipos y costos se validarán antes de cerrar E7.

## 1. Resumen ejecutivo
Financiera Pacífico requiere modernizar su red de comunicaciones para proteger el core financiero, la telefonía VoIP y el intercambio de información entre Casa Matriz y sucursales. El escenario original del Capstone considera Casa Matriz y 21 sucursales (22 sedes). El equipo del proyecto amplía deliberadamente el diseño a **Casa Matriz y 22 sucursales (23 sedes)** para incorporar cobertura territorial adicional, incluida Base Frei en la Antártica. Esta ampliación es una decisión del escenario académico, no un dato de la ficha original.

La red heredada tiene dependencia de MPLS en sucursales, ausencia de contingencia local y procesos de cambio manuales. La arquitectura propuesta sustituye MPLS por una WAN multiservicio con Starlink, Amazon Leo, Entel 5G y GTD 4G/5G, cifrado IPsec, segmentación, políticas de seguridad y monitoreo centralizado. La disponibilidad de los operadores en cada localidad es un **supuesto de diseño por validar**, especialmente en territorios extremos.

## 2. Antecedentes oficiales de la ficha académica
| Variable | Dato oficial |
|---|---|
| Empresa | Financiera Pacífico |
| Sector | Servicios financieros |
| Usuarios | 780 |
| Equipos finales | 650 |
| Sedes | Casa Matriz + 21 sucursales = 22 |
| Servicios críticos | Core financiero, comunicaciones entre sucursales y VoIP |
| Red corporativa | 10.70.0.0/16 |
| WAN existente | MPLS único en sucursales y doble ISP en Casa Matriz |
| Enrutamiento | OSPF |
| Monitoreo | NMS central |
| Problema | Sucursales sin contingencia y cambios manuales |
| Evento de referencia | Caída nacional MPLS de tres horas |
| RTO de sucursal | ≤ 5 minutos |
| Disponibilidad objetivo | Core ≥ 99,99%; sucursales ≥ 99,95% |
| Requisitos de seguridad | Cifrado y auditoría obligatorios |
| Crecimiento esperado | Capacidad para seis sucursales nuevas |
| Presupuesto inicial | $160.000.000 CLP, referencia revisable |
| Horizonte | Cinco años |

**Fuente:** archivo `Capstone_Ingenieria_Conectividad_Redes.xlsx`, hojas «Empresas», «Fichas de Casos» y «Datos Técnicos». La retroalimentación escrita del profesor permite modificar el presupuesto si se fundamenta la inversión.

## 3. Supuestos y decisiones del equipo
Estos valores **no son antecedentes oficiales** ni cotizaciones verificadas.

| Variable | Definición del escenario académico | Estado de validación |
|---|---|---|
| Alcance territorial | 23 sedes: CM + 22 sucursales | Decisión del equipo |
| Sucursal adicional | Base Frei, Antártica | Decisión del equipo |
| WAN anterior en CM | Dos enlaces MPLS de 100 Mbps cada uno | Supuesto de diseño |
| WAN anterior por sucursal | Un enlace MPLS de 100 Mbps | Supuesto de diseño |
| Total MPLS ANTES | 24 enlaces (2 CM + 22 sucursales) | Cálculo del escenario |
| Total MPLS DESPUÉS | Cero enlaces | Decisión de arquitectura |
| WAN DESPUÉS | Starlink, Amazon Leo, Entel 5G, GTD 4G/5G | Propuesta |
| OneWeb | Excluido por decisión del equipo | Definitivo |
| Amazon Leo | Se supone comercialmente habilitado en Chile al momento de su despliegue futuro | Supuesto; no afirmar disponibilidad actual |
| Precios Amazon Leo | Tarifa pública de EE. UU. solo si se encuentra una referencia verificable; de lo contrario, estimación explícita | Pendiente de validación |
| Costo de MPLS | Se modelará para enlaces comparables de 100 Mbps; precio sujeto a respaldo o identificación como supuesto | Pendiente de validación |
| Equipos de red | Selección definitiva sujeta a vigencia, licenciamiento y soporte del fabricante | Pendiente de revisión |
| Cifrado | IPsec/IKEv2 con algoritmos robustos según interoperabilidad y políticas | Diseño por validar |
| Desempeño | Latencia, jitter, pérdida y conmutación se medirán en pruebas simuladas; no constituyen SLA reales | Pendiente de pruebas |

**Nota sobre cobertura:** la contratación de un servicio satelital en otro país no demuestra autorización ni funcionamiento en Chile. En este caso ficticio se asume habilitación local futura, no una excepción regulatoria.

## 4. Distribución territorial del escenario de 23 sedes
| Código | Ubicación |
|---|---|
| CM | Santiago Centro — Casa Matriz y datacenter |
| S01 | Providencia |
| S02 | Las Condes |
| S03 | Maipú |
| S04 | Puente Alto |
| S05 | La Florida |
| S06 | Arica |
| S07 | Iquique |
| S08 | Antofagasta |
| S09 | Copiapó |
| S10 | La Serena |
| S11 | Valparaíso |
| S12 | Rancagua |
| S13 | Talca |
| S14 | Chillán |
| S15 | Concepción |
| S16 | Temuco |
| S17 | Valdivia |
| S18 | Puerto Montt |
| S19 | Coyhaique |
| S20 | Punta Arenas |
| S21 | Hanga Roa, Isla de Pascua |
| S22 | Base Frei, Antártica |

**Comprobación:** 1 CM + 22 sucursales = 23 sedes. El diseño de direccionamiento reservará espacio para **seis sucursales adicionales**, sin confundir capacidad futura con sedes actualmente desplegadas.

## 5. Diagnóstico de la infraestructura ANTES
Se toma como referencia de inventario **hipotético** la primera versión del E1 elaborada por el equipo: Cisco 1921 en sucursales, Cisco 2901 en CM, Catalyst 2960 en sucursales, Catalyst 3750-X en CM y ASA 5512-X en el perímetro central. Estos modelos no provienen de la ficha oficial y sus fechas exactas de soporte deberán comprobarse con avisos del fabricante.

La red antigua **sí puede soportar** funciones como VLAN, 802.1Q, OSPF, QoS y determinadas VPN, dependiendo de licencias y capacidad. No se argumenta la renovación por una supuesta incapacidad absoluta para realizar estas funciones, sino por ciclo de vida, capacidad de cifrado, gestión de vulnerabilidades, continuidad, escalabilidad y costos de operación.

**Inventario conceptual ANTES (escenario ampliado):**
- Casa Matriz: 2 routers heredados, 2 switches heredados, 1 firewall perimetral heredado y **2 MPLS de 100 Mbps**.
- Cada una de las 22 sucursales: 1 router heredado, al menos 1 switch heredado y **1 MPLS de 100 Mbps**, sin segundo enlace contratado.
- Total: **24 routers heredados, al menos 24 switches, 1 firewall central y 24 enlaces MPLS**. Las cantidades de switches son un mínimo lógico para el diagnóstico; el inventario físico detallado se cerrará en E2.
- La caída nacional de MPLS de tres horas proviene del caso. No implica que cada falla de sucursal dure tres horas.

### Brechas principales
1. **Continuidad:** un único acceso MPLS en cada sucursal constituye un punto único de falla.
2. **Seguridad:** se requiere cifrado de tráfico WAN, políticas intersegmento, registro central y trazabilidad de cambios.
3. **Gestión:** los cambios manuales incrementan riesgo operacional; deben implementarse control de versiones, respaldos, aprobación y auditoría.
4. **Escalabilidad:** el direccionamiento, el dimensionamiento y el soporte deben contemplar seis sucursales futuras.
5. **Resiliencia central:** el datacenter debe resistir pérdida de enlace y de router sin depender de un único equipo.
6. **Economía:** comparar el costo total de propiedad de ambas redes durante cinco años con los mismos criterios contables.

## 6. Arquitectura objetivo DESPUÉS
- **MPLS eliminado:** 0 enlaces en la nueva red.
- **Accesos WAN candidatos:** Starlink (satelital), Amazon Leo (satelital futuro), Entel 5G y GTD 4G/5G. No se presupone cobertura efectiva idéntica en todas las sedes.
- **Casa Matriz:** redundancia de routers, perímetro de seguridad, terminación de VPN y servicios críticos. Los dominios de falla de equipos y enlaces deben ser independientes cuando sea posible.
- **Sucursales:** enrutamiento IP entre subredes/VLAN y túneles cifrados; las VLAN **no se extienden como VLAN de capa 2** mediante IPsec tradicional.
- **Conmutación:** OSPF para rutas dinámicas cuando corresponda, complementado por IP SLA/seguimiento de objetos o BFD según capacidades verificadas. No se promete failover subsegundo sin evidencia.
- **Seguridad:** segmentación, ACL, políticas NGFW, registro centralizado y administración autenticada; estas medidas contribuyen a controles normativos, pero no acreditan por sí solas cumplimiento PCI DSS o CMF.
- **Disponibilidad:** metas de ≥99,99% core y ≥99,95% sucursales según ficha. Son objetivos de diseño, no resultados medidos ni SLA garantizados.

## 7. Escenarios obligatorios de continuidad
| ID | Falla a demostrar | Criterio de aceptación propuesto |
|---|---|---|
| F01 | Pérdida del enlace principal de una sucursal | Restablecer servicio crítico mediante respaldo en ≤300 s |
| F02 | Pérdida del acceso de respaldo | Mantener ruta principal y emitir alarma |
| F03 | Caída del router principal en Casa Matriz | Conmutar al equipo redundante y registrar interrupción |
| F04 | Caída completa de una sucursal | Detectar indisponibilidad, alertar y registrar recuperación; la WAN no puede compensar un apagón total sin energía local |
| F05 | Indisponibilidad del proveedor principal | Migrar a otro operador disponible, según cobertura real o supuesta |
| F06 | Operación degradada mediante 4G/5G | Priorizar core y VoIP con QoS, medir throughput, jitter y pérdida |
| F07 | Recuperación del enlace principal | Retorno controlado, evitando oscilaciones de ruta |

## 8. Indicadores y método de evaluación
| Indicador | Objetivo o tratamiento |
|---|---|
| RTO sucursal | ≤5 min ante fallas recuperables por enlaces alternativos |
| Disponibilidad core | ≥99,99% objetivo anual |
| Disponibilidad sucursal | ≥99,95% objetivo anual |
| RTT, jitter y pérdida | Registrar por tecnología y escenario; umbrales se definirán por aplicación |
| Throughput | Medir capacidad efectiva de cada enlace, sin sumar enlaces independientes como si fueran una sola conexión |
| VoIP | Evaluar MOS estimado y prioridad QoS |
| VPN | Verificar establecimiento, cifrado y continuidad de túneles |
| Auditoría | Trazabilidad de cambios y eventos de falla |
| Finanzas | CAPEX, OPEX anual, TCO 5 años, ahorro y recuperación de inversión con supuestos explícitos |

## 9. Evaluación económica preliminar
El presupuesto de **$160.000.000 CLP** es una referencia académica, **no un techo obligatorio**, según la retroalimentación del profesor. E7 deberá separar:
- **CAPEX:** routers, switches, NGFW, módulos, terminales, instalación y capacitación.
- **OPEX:** contratos de conectividad, licencias, soporte, mantención y operación.
- **TCO 5 años:** costos comparables de ANTES y DESPUÉS.
- **Supuestos:** precios sin cotización formal identificados como estimaciones; IVA, tipo de cambio y fechas consistentes.

**No se fija todavía un ahorro porcentual definitivo.** Se calculará una vez validado el inventario y las tarifas, sin inflar artificialmente el costo MPLS.

## 10. Alcance y límites de la demostración
El proyecto entregará diseño físico/lógico, plan IP y VLAN, selección comparada de equipos, plan de continuidad, presupuesto, despliegue por fases y pruebas. El simulador web propio será una **demostración didáctica del comportamiento definido**, no una emulación certificada de Cisco IOS-XE ni Palo Alto PAN-OS; sus métricas serán resultados del modelo y no mediciones de una red real. Su actualización se hará **después de cerrar E1–E7**.

## 11. Trazabilidad de la revisión
Esta versión corrige problemas detectados en `2.0/entregable 1 .txt`: 21/22 sucursales inconsistentes, MPLS de 10 Mbps, OneWeb, promesas de 99,99999% sin validación, cantidades incompatibles de equipos y afirmaciones regulatorias categóricas. También incorpora los criterios expresos de la retroalimentación docente: supuestos declarados, inventario realista, continuidad medible, distinción VLAN/IPsec, justificación económica y documentación técnica.

## 12. Fuentes y pendientes de cierre
1. `Capstone_Ingenieria_Conectividad_Redes.xlsx`: ficha oficial del caso y datos técnicos.
2. Retroalimentación escrita del profesor sobre el primer Entregable 1 (proporcionada por el equipo).
3. `tesis-financiera-pacifico/2.0/entregable 1 .txt`: antecedente de trabajo del equipo.
4. **Pendiente:** fichas técnicas y ciclo de vida del equipamiento elegido; cotizaciones comparables; cobertura y disponibilidad de servicios; fecha de habilitación comercial de Amazon Leo; definición del BOM maestro y costos.

**Regla de consistencia para E2–E7:** 23 sedes = CM + 22 sucursales; ANTES = 24 MPLS de 100 Mbps; DESPUÉS = 0 MPLS y 0 OneWeb; Amazon Leo es hipótesis de disponibilidad futura; las seis sucursales de crecimiento no forman parte de las 23 actuales.


## Validación documental — 15 de octubre de 2026
**Fuentes oficiales verificadas:**
- Cisco: [anuncio EoL ISR 4461](https://www.cisco.com/c/en/us/products/collateral/routers/4000-series-integrated-services-routers-isr/isr4461-series-platform-eol.html). Última fecha de pedido 20-01-2025; último soporte 31-01-2030. **No adquirir ISR 4461 para una implantación nueva 2026–2031.** Reemplazo recomendado por Cisco: C8300-2N2S-4T2X.
- Cisco: [familia Catalyst 8300](https://www.cisco.com/c/en/us/support/routers/catalyst-8300-series-edge-platforms/series.html), estado «Available Order» consultado en octubre de 2026. La elección exacta requiere validar interfaces, rendimiento criptográfico y cotización.
- Palo Alto Networks: [ciclo de vida PA-850](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/hardware-end-of-life-dates): fin de venta 31-08-2024, fin de soporte 31-08-2029. **No adquirir PA-850 para una implantación nueva 2026–2031.** El fabricante identifica la familia PA-1400 como reemplazo.
- Amazon: [Amazon Leo](https://www.aboutamazon.com/what-we-do/devices-services/amazon-leo) y [despliegue comercial](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-arianespace-first-mission-satellites). Amazon anunció despliegue gradual en 2026; **no se encontró tarifa comercial pública verificable ni habilitación por dirección en Chile** en las fuentes consultadas. No es legítimo presentar como cotización la partida heredada de terminales.
- Starlink: [servicios de prioridad](https://starlink.com/business/service-plans) y [condiciones de prioridad en Chile](https://starlink.com/cl/support/article/d3dcd79f-c332-63e7-9204-3c3ec4f104ae). Los precios visibles dependen del país, volumen de datos y plan; no equiparar un precio de itinerancia residencial a contrato empresarial.

**Consecuencia para este documento:** toda mención de ISR 4461, PA-850, sus módulos y sus precios debe leerse exclusivamente como **antecedente histórico del borrador anterior**, no como equipamiento de compra aprobado. Para el diseño de adquisición se adopta como **candidato Cisco Catalyst 8300 y Palo Alto PA-1400**; las cantidades de equipos siguen siendo hipótesis de dimensionamiento. Los costos basados en hardware retirado de venta **no representan el presupuesto definitivo**. El cierre financiero requiere cotización o escenarios explícitos con precios estimados para las familias nuevas.
