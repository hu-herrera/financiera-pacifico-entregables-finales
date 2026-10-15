# Entregable 3 — Diseño de alto nivel (HLD)
**Financiera Pacífico | Versión 3.0 | Octubre 2026**

## 3.1 Alcance y principios
La solución abarca **23 sedes** (CM + S01–S22), **780 usuarios y 650 equipos finales** como escala inicial del caso. La ficha original describía 22 sedes; el equipo añadió Base Frei. Se reserva capacidad para seis sucursales futuras. Los principios son continuidad, defensa en profundidad, trazabilidad, segmentación, capacidad de crecimiento y costos comparables.

## 3.2 Arquitectura física — ANTES y DESPUÉS
**ANTES:** CM con 2 enlaces MPLS de 100 Mbps; cada una de 22 sucursales con 1 MPLS de 100 Mbps. **24 enlaces MPLS** en total; ausencia de WAN alternativa en sucursales.

**DESPUÉS:** **0 MPLS, 0 OneWeb.** Cada sede considera Starlink, Amazon Leo, Entel 5G y GTD 4G/5G como tecnologías candidatas. Amazon Leo se asume habilitado en el futuro del escenario académico. La disponibilidad física de cada acceso se debe verificar por localidad, especialmente Hanga Roa y Base Frei.

```text
              NUBE DE TRANSPORTES IP (SIN MPLS)
     Starlink | Amazon Leo | Entel 5G | GTD 4G/5G
                     |     |     |
             +-------+-----+-----+-------+
             | Routers de borde redundantes|
             |   CM: 2 equipos, HA L3      |
             +--------------+--------------+
                            |
                  +---------+---------+
                  | 2 NGFW en HA (CM) |
                  +---------+---------+
                            |
             Core financiero / servicios / SIEM
                            |
              Overlay IPsec / routing de sedes
                            |
              22 sucursales (1 router y 1 NGFW
              por sede, 2 switches por sede)
```

**Nota:** diagrama lógico, no esquema de puertos físicos ni afirmación de que cuatro operadores cubren cada sede.

## 3.3 Distribución geográfica
- **CM:** Santiago Centro.
- **RM:** S01 Providencia, S02 Las Condes, S03 Maipú, S04 Puente Alto, S05 La Florida.
- **Norte:** S06 Arica, S07 Iquique, S08 Antofagasta, S09 Copiapó, S10 La Serena.
- **Centro:** S11 Valparaíso, S12 Rancagua, S13 Talca.
- **Sur:** S14 Chillán, S15 Concepción, S16 Temuco, S17 Valdivia, S18 Puerto Montt, S19 Coyhaique, S20 Punta Arenas.
- **Especiales:** S21 Hanga Roa (Rapa Nui), S22 Base Frei (Antártica).

**Conteo:** 1 + 5 + 5 + 3 + 7 + 2 = 23 sedes.

## 3.4 Inventario lógico de diseño
| Activo | CM | Sucursales | Repuesto | Total |
|---|---:|---:|---:|---:|
| Router WAN | 2 | 22 | 1 | 25 |
| Switch LAN | 2 | 44 | 0 | 46 |
| NGFW | 2 | 22 | 0 | 24 |
| Módulo celular de referencia | 2 | 22 | 1 | 25 |
| Terminal Starlink | 2 | 22 | 0 | 24 |
| Terminal Amazon Leo | 2 | 22 | 0 | 24 |

El segundo router y NGFW de CM permiten diseñar HA central; **la compatibilidad real de HA debe confirmarse**. Un router único en sucursal continúa siendo punto único de falla local: la redundancia de WAN protege fallas de acceso, no avería del equipo de borde.

## 3.5 Topología lógica y tráfico
- Dominio LAN de cada sede: VLAN de usuarios, voz, servicios, gestión e invitados; subredes separadas dentro de `10.70.0.0/16`.
- Capa 3 entre sedes: rutas IP anunciadas o estáticas sobre túneles IPsec. Las etiquetas VLAN 802.1Q **no viajan de forma nativa** a través de IPsec de capa 3.
- El tráfico de invitados sale a Internet local con filtrado; no tiene acceso al core financiero.
- Los flujos de core, VoIP y administración están sujetos a ACL/NGFW y prioridad QoS.
- El direccionamiento exacto, gateways y redes de túneles se fijan en E4.

## 3.6 Continuidad y siete escenarios
| Falla | Detección y reacción prevista | Restricción |
|---|---|---|
| Starlink de sucursal | IP SLA/tracking retira ruta; migración a Leo si está operativo | RTO ≤300 s objetivo |
| Respaldo Leo | Alerta; mantener Starlink | Sin interrupción esperada del primario |
| Router principal CM | Conmutación a router secundario; validación de firewall HA | Se requiere evitar punto único de falla |
| Sucursal completa | NMS/SIEM registra caída; recuperación local de energía/equipos | No recuperable solo con rutas WAN |
| Proveedor Starlink nacional | Conmutar a proveedores realmente disponibles | Riesgo de fallas correlacionadas |
| Solo 4G/5G | Política de tráfico crítico y limitación de servicios no esenciales | Rendimiento depende de cobertura/carga |
| Retorno Starlink | Temporizador de estabilidad, retorno controlado y log | Evitar flapping |

No se promete reconvergencia subsegundo sin pruebas. OSPF por sí solo no detecta todos los cortes de extremo a extremo; se complementa con BFD/IP SLA y seguimiento de rutas.

## 3.7 Seguridad
**Capas:** controles de acceso físico; VLAN y ACL; NGFW por sede; VPN IKEv2/IPsec; MFA y AAA de administradores; gestión de parches; monitoreo/SIEM; respaldos de configuración y pruebas de restauración.

**Zonas propuestas:** WAN-UNTRUST, LAN-USERS, LAN-VOICE, LAN-CORE (CM), LAN-MGMT, LAN-GUEST y VPN-OVERLAY. Denegar por defecto entre zonas y permitir solo aplicaciones autorizadas.

**Normativa:** estas medidas **contribuyen** a objetivos de seguridad y continuidad relacionados con CMF, PCI DSS e ISO 27001; no acreditan cumplimiento integral ni sustituyen auditoría.

## 3.8 Monitoreo y administración
NMS central y SIEM para alertas de pérdida de enlaces, disponibilidad de túneles, cambios de configuración, consumo de WAN y eventos de firewall. NTP común, logs con identidad de operador, respaldo de configuraciones, roles y control de cambios. Exportar evidencia de pruebas del simulador con fecha, versión y parámetros.

## 3.9 Escalabilidad y continuidad operacional
El bloque `10.70.0.0/16` permite reservar subredes para **seis sucursales futuras**. No se incluyen esas seis en el inventario de 23 ni en CAPEX inicial sin fase aprobada. Metas: core ≥99,99%, sucursales ≥99,95%, RTO ≤5 min para fallas de enlace recuperables. Medir resultados, no confundir metas con disponibilidad garantizada.

## 3.10 Supuestos de validación
1. Habilitación futura de Amazon Leo en Chile; las tarifas estadounidenses, si se usan, son solo referencias económicas.
2. Disponibilidad y permisos por sede para satélite/celular, especialmente Rapa Nui y Antártica.
3. Potencia, respaldo energético, montaje y condiciones climáticas de sedes especiales.
4. Modelos definitivos de routers, NGFW y switches con soporte durante cinco años.
5. Interfaces y capacidad criptográfica suficientes para túneles simultáneos y HA.

**Referencias:** Capstone oficial; observaciones del profesor; E1/E2 v3; E3 anterior del repositorio original. Especificaciones del fabricante y cobertura **pendientes de contraste documental**.

## Validación documental — 15 de octubre de 2026
**Fuentes oficiales verificadas:**
- Cisco: [anuncio EoL ISR 4461](https://www.cisco.com/c/en/us/products/collateral/routers/4000-series-integrated-services-routers-isr/isr4461-series-platform-eol.html). Última fecha de pedido 20-01-2025; último soporte 31-01-2030. **No adquirir ISR 4461 para una implantación nueva 2026–2031.** Reemplazo recomendado por Cisco: C8300-2N2S-4T2X.
- Cisco: [familia Catalyst 8300](https://www.cisco.com/c/en/us/support/routers/catalyst-8300-series-edge-platforms/series.html), estado «Available Order» consultado en octubre de 2026. La elección exacta requiere validar interfaces, rendimiento criptográfico y cotización.
- Palo Alto Networks: [ciclo de vida PA-850](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/hardware-end-of-life-dates): fin de venta 31-08-2024, fin de soporte 31-08-2029. **No adquirir PA-850 para una implantación nueva 2026–2031.** El fabricante identifica la familia PA-1400 como reemplazo.
- Amazon: [Amazon Leo](https://www.aboutamazon.com/what-we-do/devices-services/amazon-leo) y [despliegue comercial](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-arianespace-first-mission-satellites). Amazon anunció despliegue gradual en 2026; **no se encontró tarifa comercial pública verificable ni habilitación por dirección en Chile** en las fuentes consultadas. No es legítimo presentar como cotización la partida heredada de terminales.
- Starlink: [servicios de prioridad](https://starlink.com/business/service-plans) y [condiciones de prioridad en Chile](https://starlink.com/cl/support/article/d3dcd79f-c332-63e7-9204-3c3ec4f104ae). Los precios visibles dependen del país, volumen de datos y plan; no equiparar un precio de itinerancia residencial a contrato empresarial.

**Consecuencia para este documento:** toda mención de ISR 4461, PA-850, sus módulos y sus precios debe leerse exclusivamente como **antecedente histórico del borrador anterior**, no como equipamiento de compra aprobado. Para el diseño de adquisición se adopta como **candidato Cisco Catalyst 8300 y Palo Alto PA-1400**; las cantidades de equipos siguen siendo hipótesis de dimensionamiento. Los costos basados en hardware retirado de venta **no representan el presupuesto definitivo**. El cierre financiero requiere cotización o escenarios explícitos con precios estimados para las familias nuevas.


## Corrección documentada: Entel 5G en Base Frei (15-10-2026)
**Hecho verificado, no supuesto académico:** Entel inauguró una **red pública 5G en Villa Las Estrellas, Base Presidente Eduardo Frei Montalva, isla Rey Jorge**, en marzo de 2024. La Fuerza Aérea de Chile confirmó la puesta en marcha el 15-03-2024; Entel reportó continuidad operativa y uso de la red el 09-04-2026. Por tanto, la sede S22 **puede modelar Entel 5G como tercer acceso WAN candidato basado en cobertura pública existente**, sujeto a prueba de señal, compatibilidad del router/SIM, política de datos, capacidad y condiciones de contratación para tráfico empresarial.

**Limitación de resiliencia:** Entel advierte que la salida de la red antártica hacia el continente es satelital. En consecuencia, no se debe presentar Entel 5G como diversidad física garantizada frente a Starlink o Amazon Leo: un evento de segmento satelital, energía o transporte podría afectar más de una ruta. Incluir en E6 un escenario de fallo correlacionado y distinguir *acceso radio 5G* de *backhaul satelital*. La existencia de cobertura pública no demuestra SLA, IP pública fija, velocidad garantizada ni un plan corporativo contratado.

**Fuentes oficiales:** [Entel, despliegue 5G Antártica, 18-03-2024](https://informacioncorporativa.entel.cl/comunicados-de-prensa/la-compania-desplego-la-ultima-tecnologia-en-todo-el-territorio-entel-es-pionera-en-la-entrega-de-conectividad-5g-en-la-antartica); [Entel, dos años de 5G, 09-04-2026](https://informacioncorporativa.entel.cl/comunicados-de-prensa/instagram-facebook-y-tiktok-son-las-aplicaciones-mas-usadas-en-la-antartica); [FACH, 15-03-2024](https://fach.mil.cl/fuerza-aerea-de-chile-participo-en-la-puesta-en-marcha-de-la-red-de); [Entel, topología de red y transporte satelital](https://informacioncorporativa.entel.cl/robustez-red).
