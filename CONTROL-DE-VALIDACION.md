# Control de calidad y estado de entrega — Financiera Pacífico
**Fecha de revisión:** 15 de octubre de 2026

## Estado honesto
Los siete entregables v3 están publicados. **Aún no son documentos definitivos técnicamente y financieramente validados**. Esta matriz distingue trabajo realizado de evidencia no disponible y evita afirmar que se realizaron cotizaciones, pruebas o verificaciones de cobertura inexistentes.

## Decisiones coherentes en E1–E7
| Regla | Valor |
|---|---|
| Alcance | 23 sedes: 1 CM + 22 sucursales |
| Caso académico original | 22 sedes; expansión de 1 sede es decisión del equipo |
| ANTES | 2 MPLS de 100 Mbps en CM + 22 MPLS de 100 Mbps = 24 |
| DESPUÉS | 0 MPLS y 0 OneWeb |
| Accesos candidatos | Starlink, Amazon Leo, Entel 5G, GTD 4G/5G |
| Amazon Leo | Escenario futuro sujeto a autorización, cobertura y oferta comercial |
| Inventario conceptual | 25 routers (24 activos + repuesto), 46 switches, 24 NGFW (2 CM + 22 sucursales) |
| Direccionamiento | 10.70.0.0/16; seis sucursales adicionales reservadas |
| Disponibilidad | 99,99% core y 99,95% sucursales, objetivos no resultados |
| RTO | ≤5 minutos para fallas de enlace con respaldo disponible |
| Prototipo | Simulador didáctico, no emulador de firmware |
| Orden | Documentos primero; simulador después |

## Hallazgos de validación oficial
| Tema | Evidencia | Decisión |
|---|---|---|
| ISR 4461 | [Cisco EoL](https://www.cisco.com/c/en/us/products/collateral/routers/4000-series-integrated-services-routers-isr/isr4461-series-platform-eol.html): fin de venta 20-01-2025; soporte hasta 31-01-2030 | Descartar para nueva compra |
| Catalyst 8300 | [Cisco catálogo](https://www.cisco.com/c/en/us/support/routers/catalyst-8300-series-edge-platforms/series.html): «Available Order» | Candidato preferente; cotizar |
| PA-850 | [Palo Alto EoL](https://www.paloaltonetworks.com/services/support/end-of-life-announcements/hardware-end-of-life-dates): fin de venta 31-08-2024; fin de soporte 31-08-2029 | Descartar para nueva compra |
| PA-1400 | [Palo Alto hardware](https://origin-docs.paloaltonetworks.com/hardware/pa-1400-hardware-reference/pa-1400-series-overview) | Candidato; dimensionar y cotizar |
| Amazon Leo | [Amazon](https://www.aboutamazon.com/what-we-do/devices-services/amazon-leo): despliegue en curso | No afirmar tarifa comercial Chile ni cobertura confirmada |
| Starlink Priority | [Planes](https://starlink.com/business/service-plans), [Chile](https://starlink.com/cl/support/article/d3dcd79f-c332-63e7-9204-3c3ec4f104ae) | Precio depende de región, plan, volumen y terminal; pedir oferta empresarial |

## Pendientes que impiden una versión definitiva
| Prioridad | Actividad | Estado | Evidencia necesaria |
|---|---|---|---|
| Crítica | Rehacer BOM con Cisco Catalyst 8300, PA-1400 y switches vigentes | Pendiente | Modelos exactos, cantidad, unitarios, licencias y soporte a cinco años |
| Crítica | Validar costo de MPLS 100 Mbps por enlace | Pendiente | Cotización o rango defendible; $2.310.000/mes es supuesto, no tarifa |
| Crítica | Verificar cobertura y tarifas Starlink, Entel, GTD por sede | Pendiente | Disponibilidad por dirección, ofertas, datos y SLA |
| Crítica | Resolver Amazon Leo | Pendiente | Oferta comercial verificable o escenario explícitamente hipotético |
| Crítica | Corregir TCO y ROI con BOM nuevo | Pendiente | Modelo de costos comparable con impuestos y años |
| Alta | Verificar viabilidad Rapa Nui y Base Frei | Pendiente | Permisos, cobertura, logística, energía y clima |
| Alta | Resolver HA física de CM y concentradores IPsec | Pendiente | Topología de interfaces y capacidad de equipos |
| Alta | Ejecutar y registrar pruebas E6 | Pendiente | Simulador actualizado y logs reproducibles |
| Alta | Actualizar simulador sin MPLS en DESPUÉS y sin OneWeb | Pendiente por acuerdo | Repositorio `hu-herrera/fin` actualizado después de cerrar documentos |
| Media | Revisar lenguaje, diagramas, enlaces y numeración | Parcial | Revisión editorial integral |

## Qué significan las cifras del E7 v3
**$224.448.000 CAPEX, $401.330.000 OPEX a cinco años, $625.778.000 TCO**: son **cifras heredadas de un escenario preliminar**, con hardware descontinuado y precio Amazon Leo no verificado. **No son presupuesto definitivo**. El MPLS legado de $2.310.000 mensuales por enlace también es una hipótesis. No presentar el ahorro calculado como ahorro contratado.

## Regla de publicación final
Solo cambiar a «definitivo» después de que todas las partidas críticas estén respaldadas por fuentes o marcadas expresamente como supuestos de un escenario académico aceptado, que la aritmética esté conciliada, que la arquitectura tenga equipos viables y que E5/E6 separen claramente pruebas planificadas de resultados ejecutados.

**No se han modificado el repositorio original ni el simulador.**


## Actualización de avance — investigación de precios (15-10-2026)
Se investigaron y añadieron en E7 referencias publicadas de Cisco Catalyst 8300, Cisco Catalyst 9200L y Palo Alto PA-1410, con enlaces y fecha. El subtotal nominal de hardware principal es **CLP 663.099.878** con USD/CLP hipotético de 950. El escenario de inversión inicial parcial asciende a **CLP 758.987.878** y **no incluye todos los rubros**. Por tanto, las cifras de TCO, ROI y payback de la versión anterior **no son conclusiones vigentes**.

**Advertencia transversal para E1–E6:** el inventario por cantidades se conserva como hipótesis de dimensionamiento, pero donde aparezcan modelos ISR 4461 / PA-850 o el TCO antiguo deben considerarse sustituidos por las familias Catalyst 8300 / PA-1400 y por la advertencia financiera del E7 actualizado. El diseño detallado y las pruebas requieren comprobar interfaces, licencias, HA y PoE. No se han ejecutado pruebas de simulador ni cambiado el código.

**Pendiente para versión realmente definitiva:** cotización empresarial chilena de los equipos y soporte por cinco años, verificación de tarifas de conectividad y cobertura por sede, valoración de Amazon Leo como escenario hipotético y recalcular TCO comparable con costos completos.


## Avance de conectividad — 15-10-2026
Se añadieron al E7 fuentes públicas de Starlink Business Fixed Site, Entel, GTD, Amazon Leo y un caso real de adjudicación Starlink Prioridad Global en una base antártica chilena distinta de Base Frei.

**Corrección de unidad económica:** CLP 96.000.000 de Starlink / (24 terminales × 60 meses) = **CLP 66.666,67 mensuales por terminal**. Referencia internacional Starlink Business 500 GB: USD 155/mes, equivalentes a CLP 147.250/mes al tipo de cambio académico de 950. Para 24 terminales durante 60 meses: **CLP 212.040.000**; sensibilidad, **no** oferta en Chile. El caso antártico publicado es CLP 729.659 mensuales y no debe extrapolarse a otras ubicaciones.

**Fuentes:** [Starlink Business](https://starlink.com/business/fixed-site), [Entel](https://www.entel.cl/comparador-de-planes), [GTD](https://www.gtd.cl/comparador), [licitación antártica](https://www.todolicitaciones.cl/licitacion/3268-31-LE26), [Amazon Leo](https://www.aboutamazon.com/news/amazon-leo/amazon-leo-satellite-internet-ultra-pro).

**Pendiente:** matriz comercial por sede, cotizaciones formales y recálculo completo de CAPEX/OPEX/TCO; el estado «definitivo» sigue bloqueado por falta de evidencia.


## Revisión de licencias y sensibilidad comercial — 15-10-2026
1. **Cisco C8300 Edge Platforms no debe confundirse con C8300-uCPE**: el anuncio de EoL para uCPE no invalida por sí solo el SKU C8300-2N2S-4T2X. [Cisco uCPE EoL](https://team-development.cisco.com/c/en/us/products/collateral/routers/catalyst-8300-series-edge-ucpe/cat8300-series-edge-ucpe-select-nfvis-licenses-eol.html).
2. La [guía Cisco 8300/8200](https://team-development.cisco.com/c/en/us/products/collateral/routers/catalyst-8300-series-edge-platforms/cat-8300-8200-series-edge-plat-og.html) exige licencias DNA/alternativa aplicable, y distingue soporte de licencias del soporte de hardware. **La cifra del router sin software no es costo instalado.**
3. Precio PA-1410 de distribuidor [Zones](https://www.zones.com/site/product/index.html?id=110975712&page_name=product_tech_spec) USD 11.663,99 frente a USD 4.894 en otra fuente. El E7 muestra dos escenarios de hardware principal: **CLP 663.099.878 a 817.455.650**; no son cotizaciones ni totales de proyecto.
4. [Starlink Puerto Rico](https://starlink.com/es-pr/business/service-plans) muestra USD 142/mes por 500 GB, distinto de otra referencia internacional; **no aplicar tarifas extranjeras como precio chileno**. [Starlink Chile](https://starlink.com/cl/support/article/d6b785b3-7354-0fc8-4e10-6b0c187bff74) informa SLA Priority de red de 99,9%.
5. [Entel Empresas](https://empresas.entel.cl/movil) menciona servicio satelital al celular para aplicaciones compatibles y excluye Antártica; no constituye enlace WAN 5G empresarial.

**Siguiente cierre necesario:** cotizar hardware y licencias exactas para 5 años; clasificar switches según PoE; definir por sede qué enlaces existen y cuáles son hipótesis; preparar una tabla de costos por año y escenario que no mezcle tarifas públicas de distintos países con ofertas locales.


## Corrección documentada: Entel 5G en Base Frei (15-10-2026)
**Hecho verificado, no supuesto académico:** Entel inauguró una **red pública 5G en Villa Las Estrellas, Base Presidente Eduardo Frei Montalva, isla Rey Jorge**, en marzo de 2024. La Fuerza Aérea de Chile confirmó la puesta en marcha el 15-03-2024; Entel reportó continuidad operativa y uso de la red el 09-04-2026. Por tanto, la sede S22 **puede modelar Entel 5G como tercer acceso WAN candidato basado en cobertura pública existente**, sujeto a prueba de señal, compatibilidad del router/SIM, política de datos, capacidad y condiciones de contratación para tráfico empresarial.

**Limitación de resiliencia:** Entel advierte que la salida de la red antártica hacia el continente es satelital. En consecuencia, no se debe presentar Entel 5G como diversidad física garantizada frente a Starlink o Amazon Leo: un evento de segmento satelital, energía o transporte podría afectar más de una ruta. Incluir en E6 un escenario de fallo correlacionado y distinguir *acceso radio 5G* de *backhaul satelital*. La existencia de cobertura pública no demuestra SLA, IP pública fija, velocidad garantizada ni un plan corporativo contratado.

**Fuentes oficiales:** [Entel, despliegue 5G Antártica, 18-03-2024](https://informacioncorporativa.entel.cl/comunicados-de-prensa/la-compania-desplego-la-ultima-tecnologia-en-todo-el-territorio-entel-es-pionera-en-la-entrega-de-conectividad-5g-en-la-antartica); [Entel, dos años de 5G, 09-04-2026](https://informacioncorporativa.entel.cl/comunicados-de-prensa/instagram-facebook-y-tiktok-son-las-aplicaciones-mas-usadas-en-la-antartica); [FACH, 15-03-2024](https://fach.mil.cl/fuerza-aerea-de-chile-participo-en-la-puesta-en-marcha-de-la-red-de); [Entel, topología de red y transporte satelital](https://informacioncorporativa.entel.cl/robustez-red).
