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

## Validación documental — 15 de octubre de 2026
**Fuentes oficiales verificadas:**
- Cisco: [anuncio EoL ISR 4461](https://www.cisco.com/c/en/us/products/collateral/routers/4000-series-integrated-services-routers-isr/isr4461-series-platform-eol.html). Última fecha de pedido 20-01-2025; último soporte 31-01-2030. **No adquirir ISR 4461 para una implantación nueva 2026–2031.** Reemplazo recomendado por Cisco: C8300-2N2S-4T2X.
- Cisco: [familia Catalyst 8300](https://www.cisco.com/c/en/us/support/routers/catalyst-8300-series-edge-platforms/series.html), estado «Available Order» consultado en octubre de 2026. La elección exacta requiere validar interfaces, rendimiento criptográfico y cotización.
- Palo Alto Networks: [ciclo de vida PA-850](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/hardware-end-of-life-dates): fin de venta 31-08-2024, fin de soporte 31-08-2029. **No adquirir PA-850 para una implantación nueva 2026–2031.** El fabricante identifica la familia PA-1400 como reemplazo.
- Amazon: [Amazon Leo](https://www.aboutamazon.com/what-we-do/devices-services/amazon-leo) y [despliegue comercial](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-arianespace-first-mission-satellites). Amazon anunció despliegue gradual en 2026; **no se encontró tarifa comercial pública verificable ni habilitación por dirección en Chile** en las fuentes consultadas. No es legítimo presentar como cotización la partida heredada de terminales.
- Starlink: [servicios de prioridad](https://starlink.com/business/service-plans) y [condiciones de prioridad en Chile](https://starlink.com/cl/support/article/d3dcd79f-c332-63e7-9204-3c3ec4f104ae). Los precios visibles dependen del país, volumen de datos y plan; no equiparar un precio de itinerancia residencial a contrato empresarial.

**Consecuencia para este documento:** toda mención de ISR 4461, PA-850, sus módulos y sus precios debe leerse exclusivamente como **antecedente histórico del borrador anterior**, no como equipamiento de compra aprobado. Para el diseño de adquisición se adopta como **candidato Cisco Catalyst 8300 y Palo Alto PA-1400**; las cantidades de equipos siguen siendo hipótesis de dimensionamiento. Los costos basados en hardware retirado de venta **no representan el presupuesto definitivo**. El cierre financiero requiere cotización o escenarios explícitos con precios estimados para las familias nuevas.


## 7.12 Actualización de precios observables y escenario de adquisición vigente (15-10-2026)
**Esta sección prevalece sobre los valores de hardware obsoleto de §7.2 y el TCO heredado de §7.5.** Es una **estimación con precios publicados de referencia**, no una oferta vinculante para Chile. Se documentan moneda, unidades, fecha y fuente; transporte, impuestos, contratos de soporte y configuración pueden modificar sustancialmente el resultado.

### Fuentes de precios con evidencia consultable
| Ítem | Referencia observable | Fecha/alcance | Fuente |
|---|---:|---|---|
| Cisco Catalyst C8300-2N2S-4T2X | **USD 18.249,61** por router, oferta minorista | Página actualizada 01-10-2026; EE. UU., sin licencia/soporte garantizados | [Neutron USA](https://www.neutronusa.com/prod.cfm/5026629) |
| Cisco C8300-2N2S-4T2X | USD 6.632,00 como oferta de otro vendedor; precio de lista USD 36.661,38 | Gran dispersión comercial; referencia no cotizada | [ITPrice](https://itprice.com/cisco-gpl/c8300-2n2) |
| Cisco C9200L-48T-4G-E | **CLP 2.567.140** por switch, pago transferencia | Publicación minorista Chile, sin servicios adicionales confirmados | [SP Digital](https://www.spdigital.cl/cisco-switch-10-gigabit-ethernet-48-10-gigabit-ethernet-c9200l-48t-4g-e/) |
| Cisco C9200L-48P-4G-E + DNA 3 años | **CLP 6.376.900** por switch | Publicación Chile; sin existencias; modelo PoE, no comparable al 48T | [TecnoShopping](https://www.tecnoshopping.cl/catalyst-9200l-48-port-poe-e-licencia-dna-e/) |
| Palo Alto PA-1410 | **USD 4.894,00** por firewall, precio visible de tercero | Publicación Newegg; revisar SKU, licencias y vendedor | [Newegg](https://www.newegg.com/p/pl?N=100158106&d=pa-1410) |
| Threat Prevention PA-1410 | **USD 2.902,79** listado de suscripción | Plazo de licencia no verificable en el extracto; **NO multiplicar como precio de cinco años** | [Newegg](https://www.newegg.com/p/pl?N=100158106&d=pa-1410) |
| Starlink Priority | Tarifas publicadas por país y franquicia | No extrapolar página de Puerto Rico a Chile | [Starlink](https://starlink.com/business/service-plans) |
| Entel Empresas móvil | Planes públicos desde CLP 21.990 / 25.990 según condiciones | **Son planes móviles**, no SLA WAN empresarial garantizado | [Entel Empresas](https://empresas.entel.cl/movil) |
| GTD | Comercializa planes empresariales Starlink y soporte | No constituye prueba de oferta GTD celular 4G/5G en todas las sedes | [GTD](https://www.gtd.cl/portabilidad-starlink-empresas) |

**Tipo de cambio exclusivamente de cálculo:** CLP **950 por USD** (hipótesis redondeada, **no dólar observado certificado del 15-10-2026**). Para evitar mezclar impuestos, todos los importes deben normalizarse a neto/IVA antes de compra; los precios de comercio electrónico aquí se usan como referencias nominales y **no están homologados fiscalmente**.

### Hardware base con referencias actuales
| Partida | Cantidad | Precio referencia CLP | Subtotal CLP |
|---|---:|---:|---:|
| Catalyst C8300-2N2S-4T2X | 25 | 17.337.130 aprox. (USD 18.249,61 × 950) | **433.428.238** |
| Catalyst C9200L-48T-4G-E | 46 | 2.567.140 | **118.088.440** |
| Palo Alto PA-1410 | 24 | 4.649.300 (USD 4.894 × 950) | **111.583.200** |
| **Subtotal hardware principal** | | | **663.099.878** |

El precio de C8300 se usa deliberadamente como referencia conservadora frente a la oferta mucho más baja de ITPrice. La variación evidencia por qué es necesaria cotización de canal. **El switch 48T no ofrece el mismo suministro PoE que un 48P**, por lo que no es una sustitución funcional automática: si la telefonía necesita PoE, deben incluirse switches PoE o inyectores y recalcular.

### Costos de adquisición no cubiertos por ese subtotal
- Equipos de acceso celular compatibles, SIM/planes y posibles gateways externos.
- Terminales y kits Starlink con instalación, montaje y alimentación.
- Terminales Amazon Leo, no cotizables de forma verificable para Chile en este ejercicio.
- Licencias Cisco y Palo Alto, especialmente Threat Prevention **para cinco años**, soporte Smart Net/Solution Support y soporte PAN.
- Instalación, capacitación, transporte a Rapa Nui/Antártica, contingencia, energía de respaldo, IVA y tipo de cambio contractual.

**Escenario presupuestario mínimo ilustrativo (NO cotización):** hardware principal CLP 663.099.878 + provisión de CLP 30.000.000 licencias iniciales + CLP 37.000.000 servicios + CLP 16.000.000 contingencia + CLP 8.088.000 terminales Starlink heredados + CLP 4.800.000 acceso celular heredado = **CLP 758.987.878**. **Excluye expresamente** terminales Amazon Leo, soporte/licencias adicionales a cinco años, impuestos y varios gastos; por tanto **no es CAPEX total definitivo ni techo presupuestario**. El monto del hardware principal supera por sí solo el presupuesto inicial de CLP 160 millones.

### Efecto en la evaluación económica
El TCO de CLP 625.778.000 de §7.5 queda **invalidado como estimación de adquisición vigente**, pues su CAPEX de CLP 224.448.000 usaba equipos fuera de venta. Si se toma **solo a modo de prueba aritmética** el subtotal mínimo ilustrativo CLP 758.987.878 y se suma el OPEX heredado CLP 401.330.000, resulta **CLP 1.160.317.878**, **sin** terminales Leo, licencias de cinco años verificadas, impuestos y otros conceptos. No presentar esta suma como TCO completo. El costo MPLS de CLP 3.326.400.000 sigue siendo un supuesto de CLP 2.310.000 por enlace/mes, no una cotización, por lo que **no corresponde publicar ROI o payback definitivos**.

**Decisión de ingeniería:** mantener la arquitectura sin MPLS y documentar que la viabilidad económica depende de cotizaciones formales, cobertura y sustitución de precios supuestos. La evidencia disponible permite defender un **presupuesto de referencia condicionado**, no un presupuesto contractual cerrado.


## 7.13 Evidencia comercial adicional: conectividad y sedes extremas (15-10-2026)
**Fuentes públicas verificadas en esta revisión**; precios de planes minoristas y licitaciones no equivalen a ofertas empresariales negociadas para Financiera Pacífico.

| Servicio / referencia | Precio publicado | Alcance real de la fuente | Aplicación al proyecto |
|---|---:|---|---|
| Starlink Business Fixed Site Local Priority 50 GB | USD 55/mes | Sitio comercial internacional; no tarifa chilena confirmada | Referencia de respaldo, volumen posiblemente insuficiente |
| Starlink Business Fixed Site Local Priority 500 GB | USD 155/mes | Sitio comercial internacional; velocidades máximas no garantizadas | Referencia para modelar demanda, no precio contractual Chile |
| Starlink Business Fixed Site Local Priority 1 TB | USD 280/mes | Sitio comercial internacional | Referencia de mayor consumo, verificar dirección y plan local |
| Starlink Business Fixed Site Local Priority 2 TB | USD 530/mes | Sitio comercial internacional | Referencia para sedes de mayor consumo |
| Entel — Plan Libre Full | CLP 24.990/mes sin descuento temporal | Oferta móvil pública, no enlace corporativo dedicado ni SLA WAN | Solo cota orientativa de SIM, no servicio empresarial comparable |
| GTD — comparador de planes | Sin precio WAN celular comparable verificable | Publica variables comerciales pero no prueba cobertura celular propia por sede | Mantener partida como supuesto, pedir cotización |
| Base antártica chilena — licitación de servicio Starlink Prioridad Global 500 GB | **CLP 729.659 por mes**, adjudicación pública de 4 meses | Base Aérea Antártica Tte. Luis Carvajal Villarroel, **no Base Frei**; oferta para institución pública | **Referencia de orden de magnitud para logística y plan polar**, no garantía de cobertura en Base Frei |

**Fuente de la licitación antártica:** [TodoLicitaciones, proceso 3268-31-LE26](https://www.todolicitaciones.cl/licitacion/3268-31-LE26). El valor mensual es de la adjudicación reportada, no debe extrapolarse a todas las sucursales ni confundirse con plan terrestre. Esta evidencia muestra que una única tarifa plana nacional puede subestimar sedes extremas.

**Fuentes de operadores:** [Starlink Fixed Site](https://starlink.com/business/fixed-site), [Starlink Chile Enterprise](https://starlink.com/cl/support/article/d6b785b3-7354-0fc8-4e10-6b0c187bff74), [Entel comparador oficial](https://www.entel.cl/comparador-de-planes), [GTD comparador oficial](https://www.gtd.cl/comparador). Starlink indica que la disponibilidad se debe comprobar por dirección y que el SLA de red de planes Priority es 99,9%; no se debe equiparar al objetivo corporativo de 99,95% por sucursal sin redundancia adicional.

**Amazon Leo:** [Amazon anunció prueba empresarial](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-satellite-internet-ultra-pro); una [actualización sectorial del 12-10-2026](https://www.lightreading.com/satellite/amazon-leo-still-tracking-for-2026-service-launch-as-satellite-production-ramps) indica que la puesta en servicio comercial seguía prevista para fines de 2026. Ninguna de estas fuentes demuestra precio estadounidense público ni disponibilidad en Chile en octubre de 2026. En consecuencia, **no corresponde afirmar que ya existe tarifa de Amazon Leo que pueda contratarse en EE. UU. para usar en Chile**.

### Efecto sobre el escenario de costos
La partida de Starlink anterior (CLP 96 millones en cinco años) equivale a CLP 400.000 por terminal y mes si se distribuye entre 24 terminales durante 60 meses: **96.000.000 / (24 × 60) = CLP 66.667/mes**, no CLP 400.000. Es una referencia promedio de **CLP 66.667 mensuales por terminal**; no se debe describir como precio publicado. El caso polar observado (CLP 729.659/mes) es más de diez veces ese promedio. **El OPEX debe segmentarse por tipo de sede y plan**, con un escenario específico para S21/S22, antes de cerrarse.

El costo Starlink Business internacional de USD 155/mes (500 GB), convertido con la hipótesis de 950 CLP/USD, es **CLP 147.250/mes** antes de impuestos y ajustes. Para 24 accesos × 60 meses serían **CLP 212.040.000** solo por esa conectividad, frente a CLP 96.000.000 heredados. Es un **escenario de sensibilidad**, no cotización de Chile; no se sumará automáticamente al presupuesto definitivo.

**Conclusión de validación:** la red multiservicio sigue siendo una alternativa técnica de estudio, pero los precios y la viabilidad de sedes extremas necesitan segmentación. No se recomienda usar el ahorro o payback preliminar como resultado final.


## 7.14 Correcciones de diligencia técnica y escenarios comerciales (15-10-2026)
**Prevalece sobre referencias anteriores cuando haya contradicción.**

### Diferencia crítica entre C8300 y C8300-uCPE
El aviso de fin de venta del **Catalyst 8300 Edge uCPE** (modelo C8300-UCPE-1N20; venta hasta agosto de 2026) **no corresponde a toda la familia Catalyst 8300 Edge Platforms**. No se debe declarar obsoleto el C8300-2N2S-4T2X por ese aviso. [Aviso oficial Cisco para uCPE](https://team-development.cisco.com/c/en/us/products/collateral/routers/catalyst-8300-series-edge-ucpe/cat8300-series-edge-ucpe-select-nfvis-licenses-eol.html). Para comprar el C8300-2N2S-4T2X, verificar su SKU específico y la fecha de soporte vigente.

**Licencias obligatorias:** la [guía oficial de pedido de Catalyst 8300/8200](https://team-development.cisco.com/c/en/us/products/collateral/routers/catalyst-8300-series-edge-platforms/cat-8300-8200-series-edge-plat-og.html) indica suscripciones Cisco DNA obligatorias para la plataforma, con tier y ancho de banda según funciones. Los precios de routers sin esas licencias **no son costos de implementación completos**. La guía distingue soporte de software de soporte del equipo físico; exigir ambas coberturas en la cotización.

### Advertencia sobre precio de Palo Alto
Un segundo distribuidor lista el **PA-1410 en USD 11.663,99**: [Zones](https://www.zones.com/site/product/index.html?id=110975712&page_name=product_tech_spec), frente a la referencia anterior de USD 4.894. La diferencia impide tomar el valor menor como precio representativo sin cotización. A tipo de cambio **supuesto** 950, 24 unidades costarían:
- **Escenario bajo de hardware PA-1410:** 24 × USD 4.894 × 950 = **CLP 111.583.200**.
- **Escenario alto de hardware PA-1410:** 24 × USD 11.663,99 × 950 = **CLP 265.938.972**.
- **Diferencia entre escenarios:** **CLP 154.355.772**, **sin** licencias, soporte, impuestos ni logística.

### Escenarios de hardware principal (no cotizaciones)
| Partida | Escenario bajo CLP | Escenario alto CLP |
|---|---:|---:|
| 25 routers Catalyst 8300, referencia de E7 §7.12 | 433.428.238 | 433.428.238 |
| 46 switches Catalyst 9200L 48T, referencia de E7 §7.12 | 118.088.440 | 118.088.440 |
| 24 NGFW PA-1410 | 111.583.200 | 265.938.972 |
| **Hardware principal** | **663.099.878** | **817.455.650** |

**Advertencia:** la diferencia de precios es una sensibilidad, no un intervalo estadístico de mercado. La selección 48T puede requerir sustitución por 48P/PoE, por lo que ambos escenarios son **pisos incompletos** si hay teléfonos o puntos de acceso que demanden alimentación PoE.

### Starlink y Entel: corrección de interpretación
El [catálogo Starlink en español para Puerto Rico](https://starlink.com/es-pr/business/service-plans) publica **USD 142/mes para Prioridad Local 500 GB** y USD 257/mes para 1 TB al momento de consulta; difiere de referencias internacionales previas. Esto demuestra que **los precios varían por mercado** y **no deben aplicarse directamente a Chile**. Para un análisis de sensibilidad **no contractual**, 24 × 60 × USD 142 × 950 = **CLP 194.256.000** a cinco años para el nivel de 500 GB. Comparar con CLP 96.000.000 heredados; el incremento sería **CLP 98.256.000**. La [ayuda oficial de Starlink Chile](https://starlink.com/cl/support/article/d6b785b3-7354-0fc8-4e10-6b0c187bff74) aclara que se debe verificar cobertura por dirección y que el SLA de disponibilidad de red de Priority es **99,9%**, no la disponibilidad corporativa objetivo de 99,95%.

La [oferta de Entel Empresas](https://empresas.entel.cl/movil) menciona planes SIM con importes desde CLP 21.990–25.990 IVA incluido bajo condiciones específicas. La cobertura **satelital al celular** se refiere a aplicaciones compatibles y **excluye Antártica**: no equivale a servicio WAN IP de 5G para router. **No usar esos valores como cotización de enlace de datos empresarial.**

### Condición de aprobación financiera
No existe evidencia suficiente para declarar un único TCO o payback «definitivo». Se conservan **escenarios numéricos trazables**, se invalidan afirmaciones de ahorro contractual y se exige precio empresarial chileno de hardware, licencias, soporte y conectividad antes de cerrar adquisiciones.


## Corrección documentada: Entel 5G en Base Frei (15-10-2026)
**Hecho verificado, no supuesto académico:** Entel inauguró una **red pública 5G en Villa Las Estrellas, Base Presidente Eduardo Frei Montalva, isla Rey Jorge**, en marzo de 2024. La Fuerza Aérea de Chile confirmó la puesta en marcha el 15-03-2024; Entel reportó continuidad operativa y uso de la red el 09-04-2026. Por tanto, la sede S22 **puede modelar Entel 5G como tercer acceso WAN candidato basado en cobertura pública existente**, sujeto a prueba de señal, compatibilidad del router/SIM, política de datos, capacidad y condiciones de contratación para tráfico empresarial.

**Limitación de resiliencia:** Entel advierte que la salida de la red antártica hacia el continente es satelital. En consecuencia, no se debe presentar Entel 5G como diversidad física garantizada frente a Starlink o Amazon Leo: un evento de segmento satelital, energía o transporte podría afectar más de una ruta. Incluir en E6 un escenario de fallo correlacionado y distinguir *acceso radio 5G* de *backhaul satelital*. La existencia de cobertura pública no demuestra SLA, IP pública fija, velocidad garantizada ni un plan corporativo contratado.

**Fuentes oficiales:** [Entel, despliegue 5G Antártica, 18-03-2024](https://informacioncorporativa.entel.cl/comunicados-de-prensa/la-compania-desplego-la-ultima-tecnologia-en-todo-el-territorio-entel-es-pionera-en-la-entrega-de-conectividad-5g-en-la-antartica); [Entel, dos años de 5G, 09-04-2026](https://informacioncorporativa.entel.cl/comunicados-de-prensa/instagram-facebook-y-tiktok-son-las-aplicaciones-mas-usadas-en-la-antartica); [FACH, 15-03-2024](https://fach.mil.cl/fuerza-aerea-de-chile-participo-en-la-puesta-en-marcha-de-la-red-de); [Entel, topología de red y transporte satelital](https://informacioncorporativa.entel.cl/robustez-red).
