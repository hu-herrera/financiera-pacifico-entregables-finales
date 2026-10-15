# Entregable 6 — Plan de pruebas cuantificables y criterios de aceptación
**Financiera Pacífico | Versión 3.0 | Octubre 2026 | Estado: protocolo de prueba; resultados pendientes**

## 6.1 Principio de evidencia
Este documento **no presenta resultados inventados**. Las tablas definen ensayos, parámetros, criterios y espacios para evidencia. Cuando se actualice el simulador se incorporarán valores realmente observados **en el modelo**, diferenciados de mediciones de campo. Los objetivos de disponibilidad proceden de la ficha académica.

**Base:** 23 sedes; ANTES 24 MPLS de 100 Mbps; DESPUÉS 0 MPLS y 0 OneWeb; Starlink, Amazon Leo, Entel 5G y GTD 4G/5G como transportes candidatos. Amazon Leo es supuesto futuro.

## 6.2 Objetivos y umbrales
| Indicador | Objetivo | Cómo se mide |
|---|---|---|
| RTO de sucursal ante falla de enlace recuperable | ≤300 s | Tiempo desde fallo hasta recuperación de tráfico crítico |
| Disponibilidad core | ≥99,99% anual | Tiempo disponible / tiempo total; cálculo en horizonte definido |
| Disponibilidad sucursales | ≥99,95% anual | Igual, por sede |
| RTT | Línea base y variación por transporte | Eco ICMP/sonda sintética |
| Jitter | Registrar p50/p95, evaluar VoIP | Variación de retardo |
| Pérdida | Registrar % por tecnología y escenario | Paquetes perdidos / enviados |
| Throughput | Registrar por enlace, sin suma ficticia | Mbps útiles por prueba |
| MOS VoIP | Estimación con método explícito | Modelo de calidad o medición RTP |
| IPsec | Cifrado y rutas verificables | Estado IKEv2/SA, prueba de tráfico |
| Trazabilidad | 100% de escenarios registrados | Evento, hora, sede, cambio, resultado |

**Importante:** 99,99% equivale aproximadamente a **52,56 min de indisponibilidad anual**; 99,95% a **262,8 min anuales**, usando 365 días. Son presupuestos teóricos de indisponibilidad, no resultados medidos.

## 6.3 Preparación y reproducibilidad
Registrar versión de simulador, commit, fecha, parámetros de velocidades, latencias, pérdida, jitter, prioridades, temporizadores, semilla pseudoaleatoria y sedes afectadas. Repetir cada ensayo al menos 10 veces en el modelo; publicar mediana, p95 y peor caso. El número de repeticiones es una **propuesta metodológica**, no evidencia existente.

## 6.4 Matriz de pruebas obligatorias
| ID | Ensayo | Estímulo | Resultado esperado | Evidencia |
|---|---|---|---|---|
| T01 | Topología | Cargar modo ANTES | 23 sedes; CM 2 MPLS; 22 sucursales 1 MPLS; 100 Mbps | Captura + inventario |
| T02 | Topología nueva | Cargar modo DESPUÉS | 23 sedes, 0 MPLS, 0 OneWeb | Captura + inspección |
| T03 | Enlace principal | Cortar Starlink S08 | Ruta secundaria disponible; RTO ≤300 s | Log y gráfica RTT |
| T04 | Respaldo | Cortar Leo S08 con Starlink sano | Starlink permanece activo; alarma Leo | Log |
| T05 | Router CM | Desactivar router primario | Segundo router toma función; medir interrupción | Traza HA |
| T06 | Sucursal total | Desactivar energía/equipos S21 | Sede no disponible; alarma correcta | Evento y tiempo |
| T07 | Proveedor nacional | Cortar Starlink en todas las sedes | Alternativas disponibles, sin prometer cobertura universal | Matriz por sede |
| T08 | Celular | Dejar solo Entel/GTD donde aplique | Core y VoIP priorizados; medir degradación | Throughput/MOS |
| T09 | Failback | Restaurar Starlink | Retorno estable tras temporizador | Timeline |
| T10 | VLAN | Intentar acceso de invitados al core | Bloqueado | Tabla ACL + resultado |
| T11 | VPN | Cortar/levantar túnel | Ruta solo por VPN autorizada | Estado SA y routing |
| T12 | Auditoría | Cambiar prioridad WAN | Evento con identidad, fecha y valor anterior/nuevo | Registro |
| T13 | QoS | Saturar acceso celular | Menor afectación relativa de VoIP que best effort | Pérdida/jitter por clase |
| T14 | Capacidad | Probar 100 Mbps MPLS ANTES | Configuración nominal 100 Mbps, medir límite del modelo | Parámetros |
| T15 | Costos | Comparar E7 y simulador | Igual CAPEX/OPEX/TCO con mismo escenario | Hoja de conciliación |

## 6.5 Ensayos de rendimiento por transporte
Para cada sede representativa (CM, RM, Norte, Sur, Hanga Roa y Base Frei) medir con mismo tamaño de muestra: RTT promedio y p95, jitter p95, pérdida %, Mbps útiles y tiempo de cambio. **No inferir cobertura satelital o celular real a partir de valores inventados**. Donde el proveedor no tenga cobertura validada, marcar «no confirmado» o «solo supuesto académico».

## 6.6 Registro de resultados — plantilla
| Prueba | Resultado del simulador | Cumple | Evidencia | Observaciones |
|---|---|---|---|---|
| T01–T15 | **Pendiente de ejecutar** | No evaluado | Pendiente | Simulador aún no actualizado |

No sustituir «pendiente» por «cumple» sin ejecución reproducible. La red ficticia puede tener objetivos de desempeño; eso no convierte valores objetivo en resultados observados.

## 6.7 Seguridad y normativas
Pruebas de reglas permitidas/denegadas, IKEv2/IPsec, MFA administrativo, syslog, separación de invitados y gestión. Su aprobación demuestra **controles específicos**, no cumplimiento total de PCI DSS, CMF ni ISO 27001.

## 6.8 Criterios de cierre
- Todas las 23 sedes representadas y correctamente identificadas.
- Modo ANTES con 24 MPLS × 100 Mbps; modo DESPUÉS sin MPLS ni OneWeb.
- Siete escenarios de continuidad ejecutados y trazables.
- Fallas recuperables por WAN cumplen RTO ≤300 s en el modelo o se documentan excepciones.
- Rutas y VPN/ACL verificadas; sin fuga de red de invitados.
- Parámetros financieros concuerdan con E7.
- Evidencia de limitaciones del simulador, metodología y resultados anexada.

**Referencias:** ficha académica, observaciones del profesor, E1–E5 v3. El E6 anterior es antecedente; sus cifras simuladas no se adoptan sin repetir pruebas.

## Validación documental — 15 de octubre de 2026
**Fuentes oficiales verificadas:**
- Cisco: [anuncio EoL ISR 4461](https://www.cisco.com/c/en/us/products/collateral/routers/4000-series-integrated-services-routers-isr/isr4461-series-platform-eol.html). Última fecha de pedido 20-01-2025; último soporte 31-01-2030. **No adquirir ISR 4461 para una implantación nueva 2026–2031.** Reemplazo recomendado por Cisco: C8300-2N2S-4T2X.
- Cisco: [familia Catalyst 8300](https://www.cisco.com/c/en/us/support/routers/catalyst-8300-series-edge-platforms/series.html), estado «Available Order» consultado en octubre de 2026. La elección exacta requiere validar interfaces, rendimiento criptográfico y cotización.
- Palo Alto Networks: [ciclo de vida PA-850](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/hardware-end-of-life-dates): fin de venta 31-08-2024, fin de soporte 31-08-2029. **No adquirir PA-850 para una implantación nueva 2026–2031.** El fabricante identifica la familia PA-1400 como reemplazo.
- Amazon: [Amazon Leo](https://www.aboutamazon.com/what-we-do/devices-services/amazon-leo) y [despliegue comercial](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-arianespace-first-mission-satellites). Amazon anunció despliegue gradual en 2026; **no se encontró tarifa comercial pública verificable ni habilitación por dirección en Chile** en las fuentes consultadas. No es legítimo presentar como cotización la partida heredada de terminales.
- Starlink: [servicios de prioridad](https://starlink.com/business/service-plans) y [condiciones de prioridad en Chile](https://starlink.com/cl/support/article/d3dcd79f-c332-63e7-9204-3c3ec4f104ae). Los precios visibles dependen del país, volumen de datos y plan; no equiparar un precio de itinerancia residencial a contrato empresarial.

**Consecuencia para este documento:** toda mención de ISR 4461, PA-850, sus módulos y sus precios debe leerse exclusivamente como **antecedente histórico del borrador anterior**, no como equipamiento de compra aprobado. Para el diseño de adquisición se adopta como **candidato Cisco Catalyst 8300 y Palo Alto PA-1400**; las cantidades de equipos siguen siendo hipótesis de dimensionamiento. Los costos basados en hardware retirado de venta **no representan el presupuesto definitivo**. El cierre financiero requiere cotización o escenarios explícitos con precios estimados para las familias nuevas.
