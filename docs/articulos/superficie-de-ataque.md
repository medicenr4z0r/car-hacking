---
title: "Superficie de ataque: las puertas y ventanas de un auto"
description: "Cómo identificar las interfaces físicas, inalámbricas y remotas de un vehículo sin asumir que todos los autos tienen la misma arquitectura."
---

# Superficie de ataque: las puertas y ventanas de un auto

Antes de analizar un sistema hay que entender por dónde puede entrar la información. En seguridad automotriz, ese inventario se conoce como **superficie de ataque**.

Una comparación útil es pensar el auto como una casa. Las ECUs serían los espacios internos donde ocurren las funciones importantes, mientras que los puertos, las redes y las conexiones inalámbricas serían sus puertas y ventanas.

El objetivo de un análisis responsable no es entrar por esas puertas, sino saber cuáles existen, qué protecciones tienen y qué podría ocurrir si una de ellas se utiliza de forma no autorizada.

{% hint style="warning" %}
Todo análisis debe realizarse sobre vehículos, componentes y laboratorios propios o expresamente autorizados. Este artículo describe superficies y riesgos a nivel conceptual; no es una guía para abrir vehículos, interferir señales o enviar comandos a sistemas reales.
{% endhint %}

## No existe un único auto moderno

Cuando se habla de car hacking suelen aparecer ejemplos de vehículos eléctricos, conectados y definidos por software. Sin embargo, el parque automotor de Argentina y de Latinoamérica es mucho más variado.

En una misma calle pueden convivir:

- Vehículos de principios de los años 2000 con electrónica relativamente simple.
- Modelos como un Clio Mio 2013, con ECUs y diagnóstico, pero sin una conexión permanente a internet de fábrica.
- Vehículos con estéreos, alarmas o sensores instalados posteriormente.
- Autos recientes con telemática, aplicaciones móviles, Wi-Fi, actualizaciones remotas y servicios en la nube.
- Vehículos eléctricos con sistemas de carga y comunicaciones adicionales.

Por eso no se debe afirmar que una superficie de ataque existe en todos los autos. El año, el mercado, el nivel de equipamiento, el fabricante y las modificaciones realizadas cambian completamente el análisis.

## Dos grandes perfiles de exposición

### Autos tradicionales y módulos agregados

En muchos vehículos de uso común, la exposición está concentrada en interfaces físicas y de corto alcance:

- Puerto de diagnóstico.
- Estéreo o unidad multimedia.
- Bluetooth, si está disponible.
- Puertos USB.
- Controles remotos y alarmas.
- Módulos agregados por concesionarios o instaladores.

Las alarmas y accesorios instalados después de fábrica merecen atención especial. Pueden tener documentación limitada, contraseñas predeterminadas, firmware antiguo o una separación insuficiente respecto de otros circuitos del vehículo.

Esto no significa que todo accesorio sea inseguro. Significa que debe incluirse en el inventario y evaluarse como parte real de la arquitectura del auto.

### Vehículos conectados

Los vehículos recientes pueden incorporar unidades telemáticas, conectividad celular, aplicaciones móviles, servicios en la nube, Wi-Fi y actualizaciones remotas.

En estos casos aparecen superficies que no requieren estar físicamente junto al auto:

- Aplicaciones móviles.
- APIs y servicios en la nube.
- Gestión de cuentas y permisos.
- Actualizaciones de software.
- Conectividad celular.
- Servicios de localización y telemetría.
- Sistemas de carga en vehículos eléctricos.

La conectividad amplía las posibilidades del vehículo, pero también agrega dependencias, identidades digitales y componentes externos que deben protegerse.

## Mapa general de superficies

```text
SUPERFICIE DE ATAQUE
├── Interfaces físicas
│   ├── Puerto de diagnóstico
│   ├── Puertos USB y medios extraíbles
│   ├── Cableado y conectores
│   └── Conector de carga en vehículos eléctricos
├── Interfaces inalámbricas de corto alcance
│   ├── Bluetooth
│   ├── Wi-Fi
│   ├── Controles remotos y alarmas
│   └── Sensores inalámbricos, como TPMS
└── Interfaces remotas
    ├── Red celular y telemática
    ├── Aplicaciones móviles
    ├── APIs y servicios en la nube
    └── Actualizaciones remotas
```

Este mapa es un punto de partida. No reemplaza la documentación del vehículo ni una inspección autorizada.

## Interfaces físicas

### Puerto de diagnóstico

El puerto OBD-II permite conectar herramientas de diagnóstico. En muchos vehículos también expone una o más redes internas, pero no debe asumirse que todos los pines tienen la misma función ni que todos los modelos utilizan CAN de la misma manera.

Su presencia lo convierte en una interfaz importante para:

- Leer información de diagnóstico.
- Detectar fallos.
- Realizar mantenimiento autorizado.
- Estudiar la separación entre diagnóstico y funciones críticas.

Desde el punto de vista de seguridad, hay que preguntarse quién puede conectarse, qué operaciones puede realizar una herramienta y qué controles existen entre el diagnóstico y las redes sensibles.

### Puertos USB y medios extraíbles

Una unidad multimedia puede aceptar memorias USB, teléfonos, archivos de audio, imágenes o actualizaciones. Cada formato procesado por el sistema agrega código y, potencialmente, nuevas posibilidades de error.

Los controles importantes incluyen:

- Validación de archivos.
- Actualizaciones firmadas.
- Restricción de privilegios.
- Registro de eventos.
- Separación entre la unidad multimedia y las redes críticas.

### Cableado y conectores

El acceso físico a conectores, arneses o módulos puede exponer interfaces que no están pensadas para el usuario final. En un laboratorio, el estudio debe comenzar con diagramas, documentación y componentes aislados, no con intervenciones sobre un vehículo en uso.

## Interfaces inalámbricas

### Controles remotos y alarmas

Los mandos y alarmas utilizan radiofrecuencia. La seguridad depende de cómo se generan, validan y almacenan los códigos, y de cómo el receptor gestiona mensajes repetidos o fuera de secuencia.

Existen implementaciones antiguas basadas en códigos fijos y otras que utilizan códigos variables. Las diferencias entre ellas son importantes, pero no permiten concluir automáticamente que un sistema sea vulnerable: hace falta identificar el modelo, el circuito y el diseño concreto.

Las investigaciones sobre replay, rolling codes o desincronización deben tratarse como casos de estudio defensivos y realizarse únicamente en dispositivos de laboratorio.

### Bluetooth y Wi-Fi

La unidad multimedia puede utilizar Bluetooth para llamadas, audio y sincronización de contactos. También puede incorporar Wi-Fi para conectividad, actualizaciones o funciones de hotspot.

Las preguntas de seguridad incluyen:

- ¿Qué dispositivos pueden emparejarse?
- ¿Cómo se validan las conexiones?
- ¿Qué datos se almacenan?
- ¿Se aplican actualizaciones de seguridad?
- ¿La unidad multimedia está aislada de las redes críticas?

PerfektBlue es un ejemplo documentado de vulnerabilidades en el stack Bluetooth BlueSDK de OpenSynergy. Debe estudiarse mediante los avisos oficiales y sus mitigaciones, sin convertir el artículo en una receta de explotación.

### Sensores inalámbricos

Sistemas como TPMS pueden enviar información de presión y temperatura a un receptor del vehículo. Su exposición depende del diseño, la autenticación y la forma en que el vehículo procesa los mensajes.

Para un análisis responsable importan tanto la confidencialidad de los identificadores como la posibilidad de que datos falsos generen alertas o diagnósticos incorrectos.

## Interfaces remotas y servicios conectados

En un vehículo conectado, la superficie de ataque también incluye aplicaciones, APIs y servicios en la nube.

Una auditoría autorizada debe revisar, entre otros aspectos:

- Autenticación de usuarios.
- Autorización por recurso y por acción.
- Gestión de sesiones y tokens.
- Protección de datos de localización.
- Registro y trazabilidad de comandos.
- Separación entre funciones informativas y funciones de control.

Un identificador visible, como un VIN, nunca debería ser suficiente para autorizar una acción sensible. Los casos de IDOR deben explicarse como fallos de autorización y probarse únicamente con cuentas, vehículos y entornos incluidos en el alcance.

## Vehículos eléctricos y carga

Los vehículos eléctricos incorporan sistemas de carga y comunicación adicionales. Dependiendo del estándar y la implementación, pueden existir intercambios de datos entre el vehículo y la estación de carga.

Esto agrega nuevos elementos al inventario:

- Conector y controlador de carga.
- Firmware de la estación.
- Comunicación por línea de alimentación.
- Redes y servicios de gestión.
- Archivos de configuración y certificados.

La presencia de estas tecnologías no implica por sí sola una vulnerabilidad. Cada afirmación debe relacionarse con un estándar, modelo, versión de firmware o investigación concreta.

## Fronteras de confianza y movimiento lateral

Una **frontera de confianza** separa componentes que tienen distintos niveles de exposición o privilegio.

Por ejemplo, la unidad multimedia puede estar expuesta a Bluetooth, USB y Wi-Fi, mientras que una ECU de control de motor tiene funciones mucho más sensibles. La arquitectura segura intenta impedir que un problema en el primer componente se convierta automáticamente en acceso al segundo.

Los gateways y las segmentaciones de red pueden ayudar a:

- Filtrar mensajes entre dominios.
- Limitar servicios disponibles.
- Separar entretenimiento de control.
- Registrar eventos relevantes.
- Reducir el impacto de una intrusión.

La segmentación no reemplaza la autenticación ni corrige un protocolo inseguro, pero reduce las posibilidades de propagación y movimiento lateral.

## Cómo analizar la superficie de un vehículo

Una metodología inicial y segura es:

1. Identificar el vehículo, año, mercado y nivel de equipamiento.
2. Registrar modificaciones, alarmas y accesorios instalados.
3. Consultar manuales, diagramas y documentación pública.
4. Enumerar interfaces físicas, inalámbricas y remotas.
5. Clasificar cada interfaz por alcance y nivel de exposición.
6. Identificar qué datos procesa y qué funciones puede afectar.
7. Documentar fronteras de confianza y supuestos.
8. Definir pruebas únicamente dentro del alcance autorizado.

## Ideas principales

- La superficie de ataque cambia mucho entre un auto tradicional y un vehículo conectado.
- Un Clio Mio 2013 no debe analizarse con el mismo modelo mental que un vehículo conectado de 2026.
- Las alarmas y módulos instalados posteriormente forman parte de la arquitectura real.
- OBD-II es una interfaz de diagnóstico, no una garantía de acceso uniforme a todas las redes del vehículo.
- Bluetooth, Wi-Fi, USB, RF, telemática y APIs agregan superficies diferentes.
- La segmentación y los gateways ayudan a proteger las fronteras entre dominios.
- Todo hallazgo debe validarse contra un modelo, versión y configuración concretos.

## Preguntas de repaso

1. ¿Qué diferencias existen entre la superficie de un auto tradicional y la de un vehículo conectado?
2. ¿Por qué los accesorios instalados posteriormente deben incluirse en el inventario?
3. ¿Qué tipos de interfaces físicas, inalámbricas y remotas puede tener un vehículo?
4. ¿Por qué no se puede asumir que todos los puertos OBD-II exponen las mismas redes?
5. ¿Qué función cumple una frontera de confianza?
6. ¿Qué riesgos intenta reducir un gateway de seguridad?
7. ¿Qué datos habría que recopilar antes de evaluar un vehículo concreto?

## Fuentes y referencias

- Craig Smith, *The Car Hacker's Handbook*, capítulos sobre modelos de amenaza y protocolos de bus.
- Avisos oficiales y registros públicos relacionados con PerfektBlue y OpenSynergy BlueSDK.
- CVE-2026-49319 y el aviso de ASRG sobre el sistema de entrada sin llave afectado.
- Material de DEF CON Car Hacking Village sobre seguridad de sistemas automotrices y carga de vehículos eléctricos.

Las referencias específicas, enlaces y versiones afectadas deben incorporarse desde las fuentes originales de NotebookLM antes de considerar este artículo definitivo.
