# Entregable 7 — Presupuesto, TCO, despliegue y migración
**Financiera Pacífico | Versión 3.0 | Octubre 2026 | Escenario económico académico preliminar**

## 7.1 Alcance y advertencia de fuentes
**23 sedes:** Casa Matriz + 22 sucursales. ANTES: **2 MPLS en CM + 22 MPLS en sucursales = 24 enlaces de 100 Mbps**. DESPUÉS: **0 MPLS y 0 OneWeb**; Starlink, Amazon Leo, Entel 5G y GTD 4G/5G. El caso original considera 22 sedes y $160.000.000 CLP de presupuesto de referencia. La ampliación y las tarifas siguientes son **supuestos del equipo**, no cotizaciones formales. El profesor permite superar el presupuesto si se fundamenta.

**Amazon Leo:** se asume habilitación futura en Chile para el ejercicio; no se afirma disponibilidad actual ni que contratarlo en EE. UU. autorice operar en Chile. Los valores económicos de Amazon Leo en esta versión proceden del **modelo académico anterior**, no de una tarifa pública estadounidense verificada. Antes de entrega definitiva deben sustituirse por una referencia fechada o conservarse explícitamente como estimación.

## 7.2 Inventario CAPEX maestro
La versión E7 V3 anterior presupuestaba 22 NGFW ($39.600.000), lo que no cubría 23 sedes ni alta disponibilidad en CM. Se corrige a **24 NGFW** (22 sucursales + 2 CM), usando **$1.800.000 por unidad como supuesto heredado**.

| Partida | Cantidad | Precio unitario CLP | Total CLP | Clasificación |
|---|---:|---:|---:|---|
| Router WAN de referencia ISR 4461 | 25 | 2.280.000 | 57.000.000 | 24 activos + 1 repuesto |
| Switch de referencia C9200-48U | 46 | 600.000 | 27.600.000 | 2 por sede |
| NGFW de referencia PA-850 | **24** | 1.800.000 | **43.200.000** | 22 sucursales + 2 CM HA |
| Módulo/acceso celular de referencia | 25 | 192.000 | 4.800.000 | 24 activos + 1 repuesto |
| Terminal Starlink | 24 | 337.000 | 8.088.000 | 22 sucursales + 2 CM |
| Terminal Amazon Leo | 24 | 31.666,67 aprox. | 760.000 | Supuesto heredado; **alto riesgo de subestimación** |
| Licencias iniciales core | — | — | 30.000.000 | Alcance exacto por desglosar |
| Servicios profesionales | — | — | 37.000.000 | Instalación y puesta en marcha |
| Contingencia | — | — | 16.000.000 | Reserva de proyecto |
| **CAPEX total** | | | **224.448.000** | Escenario base provisional |

**Comprobación:** $57.000.000 + $27.600.000 + $43.200.000 + $4.800.000 + $8.088.000 + $760.000 + $30.000.000 + $37.000.000 + $16.000.000 = **$224.448.000**.

**Pendientes críticos:** cotizaciones reales de modelos vigentes, compatibilidad del módulo celular, terminal Amazon Leo y condiciones de adquisición; licencias Threat Prevention, soporte, capacitación y contingencia detallada. No se afirma que los equipos seleccionados estén actualmente a la venta.

## 7.3 Red MPLS anterior — hipótesis financiera
Se define un precio de **$2.310.000 CLP mensuales por enlace MPLS de 100 Mbps**, **solo como supuesto económico del caso**, no como tarifa de mercado verificada.

| Cálculo | CLP |
|---|---:|
| 22 sucursales × 1 enlace | 22 enlaces |
| CM × 2 enlaces | 2 enlaces |
| Total | **24 enlaces** |
| Costo mensual = 24 × $2.310.000 | **55.440.000** |
| Costo anual × 12 | **665.280.000** |
| Costo cinco años × 5 | **3.326.400.000** |

**Limitación:** este baseline refleja **solo enlaces MPLS**; no incluye por separado mantenimiento de routers legacy, soporte, energía ni posibles descuentos. Se conserva para compararlo con el modelo anterior, pero un TCO formal debe homogeneizar rubros. **No se debe describir como precio cotizado.** El E7 anterior usaba $316.800.000/año y 10 Mbps; esa cifra queda reemplazada **únicamente en este escenario de simulación económica**.

## 7.4 OPEX de la red nueva
Se mantienen inicialmente las partidas de conectividad y soporte del E7 V3 para no inventar nuevas tarifas. **Requieren validación de unidades, cobertura, IVA y reajustes**.

| Partida | Costo cinco años CLP | Promedio anual CLP |
|---|---:|---:|
| Starlink | 96.000.000 | 19.200.000 |
| Amazon Leo | 85.800.000 | 17.160.000 |
| Entel 5G | 58.080.000 | 11.616.000 |
| GTD 4G/5G | 21.450.000 | 4.290.000 |
| **Conectividad** | **261.330.000** | **52.266.000** |
| Soporte y licencias | 140.000.000 | 28.000.000 |
| **OPEX total 5 años** | **401.330.000** | **80.266.000** |

El E7 anterior distinguía primer año de $73.930.000 y años 2–5 de $81.850.000: **$73.930.000 + 4 × $81.850.000 = $401.330.000**. Esta distribución anual es **provisional** y debe explicarse mediante contratos/altas escalonadas antes del cierre. El promedio de $80.266.000 no equivale al gasto del año estabilizado.

## 7.5 TCO y ahorro del escenario
| Indicador | CLP |
|---|---:|
| CAPEX nuevo | **224.448.000** |
| OPEX nuevo 5 años | **401.330.000** |
| **TCO nuevo 5 años** | **625.778.000** |
| MPLS anterior 5 años, solo enlaces | **3.326.400.000** |
| **Diferencia nominal a cinco años** | **2.700.622.000** |
| Reducción porcentual del escenario | **81,19%** |

**Precaución metodológica:** comparar MPLS solo-enlaces con TCO completo nuevo no es una comparación de igual alcance; por ello el ahorro se presenta como **diferencia indicativa del escenario** y no ahorro contractual garantizado. El análisis final deberá incluir costos legacy comparables y eventuales costos de desinstalación/traspaso.

### Recuperación simple de CAPEX (indicativa)
Costo anual MPLS $665.280.000 menos OPEX estabilizado nuevo $81.850.000 = **$583.430.000/año**; equivalente a **$48.619.167/mes**. Recuperación simple $224.448.000 / $48.619.167 ≈ **4,62 meses**, sin descontar flujos ni considerar despliegue escalonado, IVA, financiación o riesgo de cobertura. **No es plazo real garantizado**.

## 7.6 Sensibilidad
| Escenario | Tratamiento |
|---|---|
| MPLS de referencia 25% más barato | Recalcular baseline antes de defender el ahorro |
| OPEX nuevo 25% más alto | Aumentar costo de conectividad y licencias; recalcular TCO |
| CAPEX 25% más alto | Revisar impacto de hardware/licencias y payback |
| Amazon Leo no habilitado en plazo | Posponer alta y mantener Starlink/celular o proveedor sustituto sujeto a cotización |
| Sede extrema sin celular | Reducir enlaces realmente disponibles; reforzar energía/contingencia local |
| Reemplazo de equipos por modelos vigentes | Actualizar BOM completo y contratos de soporte |

**No se puede sostener que 24 terminales Amazon Leo cuesten $760.000 en total como precio de mercado confirmado.** Es una partida heredada de alta incertidumbre.

## 7.7 Despliegue por fases (cinco años)
La suma debe ser exactamente **23 sedes**, sin contar dos veces la CM. La migración conserva MPLS temporalmente como respaldo y lo retira de cada sede **al finalizar aceptación**; en la arquitectura final no queda ninguno.

| Año | Alcance | Sedes incorporadas | Acumulado |
|---|---|---:|---:|
| 1 | CM + S01–S05 RM, piloto y HA | **6** | 6 |
| 2 | S06–S10 Norte | **5** | 11 |
| 3 | S11–S13 Centro + S14–S15 Sur | **5** | 16 |
| 4 | S16–S20 Sur | **5** | 21 |
| 5 | S21 Hanga Roa + S22 Base Frei | **2** | **23** |

Las seis sucursales de crecimiento futuras son capacidad reservada, **no forman parte de estas 23**. La programación de Base Frei depende de logística, permisos y cobertura verificables.

## 7.8 Plan de migración por sede
1. Inventariar y respaldar configuración actual; verificar rutas y aplicaciones críticas.
2. Confirmar acceso, licencias, energía, instalación y cobertura.
3. Instalar equipamiento nuevo en paralelo a MPLS y probar VLAN/ACL/IPsec.
4. Ejecutar pruebas de core financiero, VoIP, gestión y failover.
5. Cambiar rutas en ventana aprobada; monitorizar y registrar evidencia.
6. Si falla criterio crítico, **rollback a MPLS existente** antes de cancelar contrato.
7. Tras aceptación formal, retirar MPLS de esa sede y actualizar inventario/contratos.
8. Registrar costo real, lecciones y riesgos de fase.

## 7.9 Riesgos y aprobación
**Riesgos principales:** precios no cotizados, disponibilidad Amazon Leo, cobertura en Antártica/Rapa Nui, compatibilidad de equipos, costos de licencias y soporte, y diferencia entre TCO académico y contratos reales. **Aprobación recomendada:** condicionada a cotizaciones, comprobación de soporte de equipos y validación técnica de cobertura.

## 7.10 Criterios de aceptación
- 23 sedes y 24 MPLS **solo ANTES**; 0 MPLS y 0 OneWeb **DESPUÉS**.
- Inventario: 25 routers (24 operativos + repuesto), 46 switches, **24 NGFW**, 25 módulos/accesos celulares, 24 terminales de cada proveedor satelital.
- RTO ≤5 min en fallas de enlace recuperables, objetivos de disponibilidad 99,99% core/99,95% sucursales.
- Pruebas reproducibles, registros de cambios y protección del core.
- CAPEX/OPEX/TCO reconciliados y precios diferenciados entre **estimación** y **cotización**.
- El simulador se actualiza **después** de cerrar los documentos, y no se presenta como medición real.

## 7.11 Fuentes y control de versión
- Ficha académica `Capstone_Ingenieria_Conectividad_Redes.xlsx` (datos oficiales).
- Retroalimentación escrita del profesor sobre E1.
- `tesis-financiera-pacifico/2.0/Entregable_7_v3_mejorado.md` (partidas económicas heredadas **no verificadas**).
- E1–E6 versión 3.0 de este repositorio.
- **Pendientes antes de versión definitiva:** cotizaciones y documentación oficial de Cisco/Palo Alto y operadores, fechas, monedas, IVA, cobertura, condiciones de Amazon Leo, soporte y licencias.

**Conclusión:** el modelo indica potencial de ahorro importante, pero la magnitud depende de supuestos comerciales que deben verificarse. El documento prioriza consistencia y transparencia académica frente a afirmar precios o capacidades inexistentes.