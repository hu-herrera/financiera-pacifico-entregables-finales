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