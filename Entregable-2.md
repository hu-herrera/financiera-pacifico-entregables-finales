# Entregable 2 — Evaluación de alternativas y decisión técnica
**Financiera Pacífico | Versión 4.0 | Octubre 2026 | Estado: propuesta académica**

## 2.1 Problema y criterios
Se evalúa la modernización de **23 sedes** (Casa Matriz + 22 sucursales). La situación ANTES contempla **24 enlaces MPLS de 100 Mbps**: dos en CM y uno por sucursal. La solución DESPUÉS elimina MPLS y OneWeb. Los requisitos de la ficha incluyen RTO de sucursal ≤5 min, cifrado, auditoría, seis sucursales de crecimiento y disponibilidad objetivo de 99,99% core/99,95% sucursales.

Los criterios ponderados son: continuidad 25%, seguridad 20%, cobertura 20%, costo total a cinco años 15%, escalabilidad 10%, gestión/operación 10%. Las notas siguientes son **juicio de diseño**, no resultados de pruebas.

## 2.2 Alternativas WAN
| Alternativa | Descripción | Ventaja | Desventaja | Decisión |
|---|---|---|---|---|
| A — MPLS reforzado | Mantener MPLS y añadir un segundo circuito por sucursal | Predecibilidad y gestión del carrier | Dependencia de infraestructura terrestre, CAPEX/OPEX adicional | Descartada |
| B — Híbrida | MPLS primario + Internet y 5G de respaldo | Migración gradual | Mantiene contratos MPLS a largo plazo | Transitoria de migración |
| C — Multitransporte sin MPLS | Starlink + Amazon Leo + Entel 5G + GTD 4G/5G con VPN IPsec | Diversidad tecnológica, despliegue flexible | Cobertura, licencias, regulación y disponibilidad por sede | **Seleccionada como escenario objetivo** |

**Precaución:** cuatro enlaces no implican cuatro proveedores independientes en todos los puntos ni sumar sus velocidades para una sola sesión. En Isla de Pascua y Base Frei se requiere verificación de cobertura y una matriz de excepciones. Amazon Leo es una hipótesis de disponibilidad futura, no un servicio operativo confirmado en Chile.

### Evaluación ilustrativa de arquitecturas
Escala 1–5; puntuación ponderada /5. No constituye prueba empírica.

| Criterio (peso) | A | B | C |
|---|---:|---:|---:|
| Continuidad (25%) | 3 | 4 | 5 |
| Seguridad (20%) | 3 | 4 | 5 |
| Cobertura (20%) | 2 | 3 | 3 |
| TCO (15%) | 2 | 3 | 4 |
| Escalabilidad (10%) | 3 | 4 | 5 |
| Operación (10%) | 3 | 4 | 4 |
| **Resultado ponderado** | **2,65** | **3,65** | **4,30** |

La nota de cobertura C es deliberadamente conservadora: la habilitación de Amazon Leo y el acceso celular en ubicaciones extremas deben validarse.

## 2.3 Selección de equipamiento
La primera versión propuso Cisco ISR 4461, Catalyst C9200-48U y Palo Alto PA-850. **No se afirma que estén comercialmente vigentes ni con soporte hasta 2031 sin consultar avisos oficiales**. El profesor exige al menos dos alternativas y justificación de ciclo de vida.

| Familia | Candidata 1 — heredada del proyecto | Candidata 2 — para cotizar | Criterios de comparación |
|---|---|---|---|
| Router WAN | Cisco ISR 4461 | Cisco Catalyst 8300 Edge Platform | IPsec efectivo, interfaces, licencias, soporte, 5G externo/integrado |
| Switch | Catalyst C9200-48U | Catalyst C9300 / familia vigente equivalente | Puertos, PoE, uplinks, apilamiento, soporte |
| Firewall | Palo Alto PA-850 | Palo Alto PA-1400 Series / equivalente vigente | Throughput con inspección, HA, licencias Threat Prevention, soporte |
| Gestión | NMS + SIEM central | Plataforma administrada alternativa | Trazabilidad, costo anual, alertas, exportación de eventos |

**Decisión condicionada:** el presupuesto de E7 utiliza los modelos heredados como **referencia económica provisional**. La adquisición definitiva debe pasar un control de vigencia/fin de venta/soporte, capacidad real de VPN, compatibilidad del módulo celular y precio formal. No se simula compatibilidad de un módulo NIM con un modelo sin validación.

## 2.4 Inventario objetivo de referencia
| Elemento | Casa Matriz | 22 sucursales | Repuesto | Total |
|---|---:|---:|---:|---:|
| Routers WAN | 2 | 22 | 1 | **25** |
| Switches | 2 | 44 (2 por sede) | 0 | **46** |
| NGFW | 2 en HA | 22 | 0 | **24** |
| Módulos/accesos celulares de referencia | 2 | 22 | 1 | **25** |
| Terminales Starlink | 2 | 22 | 0 | **24** |
| Terminales Amazon Leo | 2 | 22 | 0 | **24** |

**Corrección crítica:** 23 firewalls (uno por sede) dejarían la CM con un único NGFW, contradictorio con alta disponibilidad central. El modelo maestro propone **24 NGFW**: dos en CM + 22 sucursales. Su costo incremental frente al E7 V3 se refleja en E7. La HA real requiere licencias, puertos, alimentación y compatibilidad confirmadas.

## 2.5 Arquitectura y mecanismos
- Accesos WAN físicos conectados a equipos de borde mediante interfaces suficientes o equipos de agregación validados.
- Túneles IKEv2/IPsec sobre cada transporte utilizable, con enrutamiento de redes IP corporativas y política de selección de ruta.
- OSPF sobre overlay solo si la plataforma lo soporta; IP SLA + tracking o BFD para detección. Se medirá failover, no se promete <1 s por defecto.
- Firewall NGFW por sede, zonas y reglas de mínimo privilegio; CM con par HA.
- Monitoreo central con SNMPv3, syslog, NetFlow/IPFIX donde aplique, NTP, autenticación centralizada y control de cambios.
- QoS por aplicación; prioridad para core financiero y VoIP durante degradación a 4G/5G.

## 2.6 Riesgos y mitigaciones
| Riesgo | Mitigación / decisión de aprobación |
|---|---|
| Amazon Leo sin disponibilidad local | Contratación condicionada a habilitación futura; respaldo Starlink/celular |
| Sin cobertura GTD o Entel | Levantamiento por dirección; ajustar número de accesos efectivos por sede |
| Equipos con ciclo de vida insuficiente | Cotizar familias vigentes antes de compra |
| Dependencia satelital de clima/obstrucciones | Prueba de sitio, energía de respaldo, diversidad de accesos |
| Variabilidad de costos USD | Sensibilidad de tipo de cambio, IVA, reajustes y licencias |
| Riesgo de migración | Piloto, ventana de cambio, coexistencia temporal MPLS y rollback |

## 2.7 Decisión
Se selecciona la **alternativa C como arquitectura objetivo del ejercicio**, con transición temporal B. Se excluye OneWeb. Se mantiene el escenario ANTES con 24 MPLS × 100 Mbps exclusivamente para diagnóstico y comparación. El cierre comercial y técnico requiere cotizaciones, disponibilidad por sede y validación de modelos. E3 desarrolla HLD; E4, LLD; E7, TCO y fases.

**Referencias:** ficha académica Capstone; retroalimentación del profesor; E1 v3; documentación oficial de fabricantes **pendiente de incorporación con enlaces, versiones y fechas**.

## Validación documental — 15 de octubre de 2026
**Fuentes oficiales verificadas:**
- Cisco: [anuncio EoL ISR 4461](https://www.cisco.com/c/en/us/products/collateral/routers/4000-series-integrated-services-routers-isr/isr4461-series-platform-eol.html). Última fecha de pedido 20-01-2025; último soporte 31-01-2030. **No adquirir ISR 4461 para una implantación nueva 2026–2031.** Reemplazo recomendado por Cisco: C8300-2N2S-4T2X.
- Cisco: [familia Catalyst 8300](https://www.cisco.com/c/en/us/support/routers/catalyst-8300-series-edge-platforms/series.html), estado «Available Order» consultado en octubre de 2026. La elección exacta requiere validar interfaces, rendimiento criptográfico y cotización.
- Palo Alto Networks: [ciclo de vida PA-850](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/hardware-end-of-life-dates): fin de venta 31-08-2024, fin de soporte 31-08-2029. **No adquirir PA-850 para una implantación nueva 2026–2031.** El fabricante identifica la familia PA-1400 como reemplazo.
- Amazon: [Amazon Leo](https://www.aboutamazon.com/what-we-do/devices-services/amazon-leo) y [despliegue comercial](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-arianespace-first-mission-satellites). Amazon anunció despliegue gradual en 2026; **no se encontró tarifa comercial pública verificable ni habilitación por dirección en Chile** en las fuentes consultadas. No es legítimo presentar como cotización la partida heredada de terminales.
- Starlink: [servicios de prioridad](https://starlink.com/business/service-plans) y [condiciones de prioridad en Chile](https://starlink.com/cl/support/article/d3dcd79f-c332-63e7-9204-3c3ec4f104ae). Los precios visibles dependen del país, volumen de datos y plan; no equiparar un precio de itinerancia residencial a contrato empresarial.

**Consecuencia para este documento:** toda mención de ISR 4461, PA-850, sus módulos y sus precios debe leerse exclusivamente como **antecedente histórico del borrador anterior**, no como equipamiento de compra aprobado. Para el diseño de adquisición se adopta como **candidato Cisco Catalyst 8300 y Palo Alto PA-1400**; las cantidades de equipos siguen siendo hipótesis de dimensionamiento. Los costos basados en hardware retirado de venta **no representan el presupuesto definitivo**. El cierre financiero requiere cotización o escenarios explícitos con precios estimados para las familias nuevas.


## Nota de cierre documental v4.0
Este entregable integra el escenario académico de 23 sedes, distingue los hechos del caso original de las hipótesis del equipo y se considera **cerrado para la etapa de diseño documental**. El presupuesto de E7 es un modelo de escenarios, no una cotización vinculante. Amazon Leo sigue siendo disponibilidad futura hipotética. Entel 5G existe públicamente en Base Frei, pero su acceso WAN empresarial no está contratado. El prototipo y las mediciones de E6 quedan para la **etapa siguiente de implementación y pruebas del simulador**, sin resultados inventados. Las familias de adquisición son Catalyst 8300 y PA-1400, con licencias y compatibilidad pendientes de cotización. Las tablas históricas de E7 no deben usarse como presupuesto de compra.
