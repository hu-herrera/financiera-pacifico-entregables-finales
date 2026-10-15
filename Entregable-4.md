# Entregable 4 — Diseño de bajo nivel (LLD)
**Financiera Pacífico | Versión 3.0 | Octubre 2026 | Documento de arquitectura conceptual**

## 4.1 Alcance
23 sedes: Casa Matriz y 22 sucursales. Red ANTES: 24 enlaces MPLS de 100 Mbps (2 CM, 22 sucursales). Red DESPUÉS: 0 MPLS y 0 OneWeb. Transportes previstos: Starlink, Amazon Leo (supuesto futuro), Entel 5G y GTD 4G/5G, sujetos a cobertura local. No se incluyen configuraciones ejecutables de equipos reales; se documentan decisiones y pruebas requeridas.

## 4.2 Direccionamiento
Bloque corporativo oficial: **10.70.0.0/16**. Asignar inicialmente un bloque /24 por sede: CM 10.70.0.0/24, S01 10.70.1.0/24, hasta S22 10.70.22.0/24. Reservar S23–S28 para seis sucursales futuras. La Casa Matriz podrá necesitar bloques adicionales para su datacenter; se ajustarán tras dimensionamiento.

Plantilla por sede, donde N corresponde al identificador numérico:
| VLAN | Uso | Prefijo | Gateway |
|---|---|---|---|
| 10 | Usuarios | 10.70.N.0/26 | 10.70.N.1 |
| 20 | Voz | 10.70.N.64/27 | 10.70.N.65 |
| 30 | Servicios | 10.70.N.96/27 | 10.70.N.97 |
| 40 | Gestión | 10.70.N.128/27 | 10.70.N.129 |
| 50 | Invitados | 10.70.N.160/27 | 10.70.N.161 |
| 60 | Infraestructura | 10.70.N.192/27 | 10.70.N.193 |
| Reserva | Expansión | 10.70.N.224/27 | Por definir |

Ejemplo S08: usuarios 10.70.8.0/26, gateway 10.70.8.1; voz 10.70.8.64/27, gateway 10.70.8.65. El número de hosts por VLAN debe comprobarse; estos bloques son plantilla inicial, no una garantía de capacidad.

## 4.3 Arquitectura de enlaces y túneles
Cada sede considera cuatro accesos WAN candidatos, pero su instalación depende de cobertura. Las sucursales utilizarán túneles de capa 3 cifrados mediante IPsec/IKEv2 hacia terminadores redundantes de Casa Matriz. **Las VLAN son locales y se enrutan como subredes IP; no se extienden directamente sobre IPsec tradicional.**

Para 22 sucursales con cuatro accesos operativos, existen hasta **88 combinaciones de túnel por acceso hacia un concentrador lógico**. Si se requieren terminaciones duplicadas por HA, el inventario de túneles se recalcula. Los accesos de CM no son túneles hacia sí mismos. La red de túneles se reservará en un espacio privado separado del direccionamiento LAN.

## 4.4 Selección de ruta
Orden de preferencia provisional: Starlink, Amazon Leo, Entel 5G y GTD 4G/5G. La política puede cambiar por sede o aplicación según calidad y cobertura. La detección utilizará monitoreo activo de alcance, seguimiento de estado y, cuando corresponda, BFD y OSPF. El retorno al primario tendrá un período de estabilidad para evitar oscilaciones.

**Objetivo:** recuperación de tráfico crítico en cinco minutos o menos ante una falla de enlace que tenga alternativa disponible. No se asegura conmutación subsegundo sin evidencia experimental.

## 4.5 Equipos e interfaces
Casa Matriz: dos routers, dos NGFW en alta disponibilidad y dos switches. Sucursales: un router, un NGFW y dos switches por sede. Un router adicional queda como repuesto. Totales: 25 routers, 24 NGFW y 46 switches. El equipo de borde y la agregación física deben disponer de suficientes interfaces para los accesos WAN y la LAN. La compatibilidad de módulos celulares, puertos y licencias se validará con documentación del fabricante antes de compra.

## 4.6 Seguridad y segmentación
Políticas conceptuales:
- Usuarios a core financiero: permitir únicamente aplicaciones y destinos autorizados.
- Voz a controladores VoIP: permitir servicios requeridos, con prioridad QoS.
- Gestión a dispositivos: restringir a administradores autenticados y estaciones autorizadas.
- Invitados a red corporativa: bloquear.
- Infraestructura e IoT a servicios sensibles: bloquear por defecto, documentar excepciones.
- Acceso público a LAN: bloquear salvo servicios publicados mediante controles específicos.

La autenticación administrativa empleará roles, MFA cuando esté disponible y registro central. La protección del tráfico intersede usará IPsec/IKEv2 con algoritmos modernos soportados por los equipos seleccionados. Estas medidas apoyan controles normativos, pero no acreditan cumplimiento integral.

## 4.7 Calidad de servicio
Clasificar VoIP, core financiero, gestión, tráfico general e invitados. Aplicar prioridad y límites de tráfico según la capacidad útil de cada acceso. No sumar velocidades de enlaces como si fueran una única sesión sin mecanismo probado de agregación. Evaluar pérdida, RTT, jitter y MOS estimado bajo congestión celular.

## 4.8 Gestión y auditoría
El NMS central recibirá estado de enlaces, métricas y alertas. El SIEM correlacionará eventos de seguridad, cambios de configuración, autenticaciones y fallas. Establecer sincronización horaria, respaldo de configuraciones, responsables de cambios y conservación de evidencia. Las plataformas exactas se seleccionarán considerando costo y compatibilidad.

## 4.9 Pruebas y aceptación
1. Verificar inventario de 23 sedes y ausencia de MPLS en DESPUÉS.
2. Confirmar gateways, rutas IP y aislamiento de invitados.
3. Comprobar continuidad de tráfico cifrado por accesos alternativos.
4. Probar caída de acceso principal y secundario.
5. Probar conmutación del router principal de CM.
6. Probar indisponibilidad completa de una sucursal y alertas.
7. Probar operación en 4G/5G y retorno controlado al principal.
8. Registrar tiempos de recuperación, latencia, jitter, pérdida y trazabilidad.

## 4.10 Pendientes técnicos
Validar capacidades de los equipos, vigencia comercial y soporte, cobertura de cada operador, habilitación futura de Amazon Leo, energía de respaldo en sedes extremas, direccionamiento final de túneles, HA central y capacidad del datacenter. El simulador será actualizado al finalizar la revisión de E1–E7.

**Referencias:** ficha Capstone; retroalimentación del profesor; E1–E3 versión 3.0; diseño LLD anterior como antecedente sujeto a revisión.
