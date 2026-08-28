Valores-

Descripcion

Valores- es un componente del ecosistema Tapiz orientado a explorar modelos, conectores y herramientas de analisis mediante una arquitectura modular, verificable y extensible.

El proyecto separa deliberadamente las responsabilidades del sistema para evitar que la logica principal dependa directamente de servicios externos, automatizaciones o herramientas de auditoria.

La arquitectura se organiza alrededor de cinco capas:

                    VALORES-
                        |
        +---------------+---------------+
        |               |               |
     LOGICA         CONECTORES      AUTOMATIZACION
        |               |               |
        +---------------+---------------+
                        |
                    SEGURIDAD
                        |
                        v
                    AUDITORIA

El objetivo no es concentrar todas las funciones en un unico componente, sino construir un sistema donde cada pieza pueda ser analizada, sustituida y verificada de manera independiente.

---

Arquitectura

Valores-
 |
 +-- Python
 |    |
 |    +-- runner
 |    |
 |    +-- conectores
 |    |
 |    +-- modelos
 |    |
 |    +-- analisis
 |    |
 |    +-- utilidades
 |
 +-- JavaScript
 |    |
 |    +-- Hardhat
 |    |
 |    +-- contratos
 |    |
 |    +-- scripts
 |
 +-- .github
      |
      +-- workflows
           |
           +-- CodeQL
           |
           +-- verificaciones

La separacion permite mantener una frontera clara entre la logica que pertenece al proyecto y los componentes utilizados para interactuar con sistemas externos.

dato
 |
 v
modelo
 |
 v
logica
 |
 +----> conector externo
 |
 v
resultado
 |
 v
verificacion

Los conectores no deben convertirse en la logica central del sistema.

---

Python

Python contiene la parte principal de exploracion y ejecucion.

Python
 |
 +-- runner
 |    |
 |    +-- ejecucion
 |    +-- coordinacion
 |
 +-- modelos
 |    |
 |    +-- representacion
 |    +-- transformacion
 |
 +-- conectores
 |    |
 |    +-- interfaces externas
 |
 +-- analisis
      |
      +-- datos
      +-- resultados

La responsabilidad del "runner" es coordinar componentes, no absorber su implementacion.

Los modelos representan datos y estados.

Los conectores proporcionan una frontera controlada hacia sistemas externos.

---

JavaScript y contratos

La parte JavaScript esta orientada al entorno de contratos y herramientas asociadas.

JavaScript
    |
    +-- Hardhat
    |     |
    |     +-- compilacion
    |     +-- pruebas
    |     +-- despliegue
    |
    +-- contratos
          |
          +-- Solidity

Hardhat funciona como herramienta de desarrollo y verificacion del entorno de contratos.

La logica de contratos permanece separada de la logica Python.

Python
   |
   | modelo / analisis
   v
resultado
   |
   v
JavaScript
   |
   v
contrato

Esta separacion evita acoplamientos innecesarios entre las distintas capas.

---

Conectores

Los componentes externos deben tratarse como conectores.

                 LOGICA TAPIZ
                      |
                      v
                  INTERFAZ
                      |
          +-----------+-----------+
          |                       |
          v                       v
      conector A             conector B
          |                       |
          v                       v
     servicio externo       servicio externo

Un conector debe limitarse a resolver la comunicacion con el sistema externo.

La logica de negocio, los modelos y las decisiones deben permanecer fuera del conector siempre que sea posible.

Esto permite sustituir un proveedor sin reconstruir el sistema completo.

---

Automatizacion

La automatizacion se mantiene separada de la logica principal.

.github
   |
   +-- workflows
          |
          +-- pruebas
          +-- seguridad
          +-- validaciones
          +-- analisis

Los workflows ejecutan comprobaciones sobre el repositorio, pero no deben convertirse en una dependencia necesaria para comprender o ejecutar la logica central.

---

Seguridad

El repositorio utiliza GitHub Actions con CodeQL para realizar analisis automatico del codigo.

El workflow principal:

.github/workflows/new.codeql.yml

realiza analisis sobre:

JavaScript
    |
    v
  CodeQL
    |
    v
resultado


Python
    |
    v
  CodeQL
    |
    v
resultado

El objetivo es detectar posibles problemas de seguridad durante el desarrollo sin modificar la logica funcional del proyecto.

La seguridad se considera una capa de verificacion:

codigo
  |
  v
analisis
  |
  v
hallazgos
  |
  v
revision

---

Auditoria

La auditoria debe permanecer separada de la ejecucion normal.

             PROYECTO
                |
       +--------+--------+
       |                 |
       v                 v
    EJECUCION         AUDITORIA
       |                 |
       v                 v
    resultado        verificacion
                         |
                         v
                      reporte

Esto permite analizar el sistema sin introducir observadores innecesarios dentro de la logica principal.

---

Principio de separacion

La arquitectura puede resumirse como:

LOGICA
  |
  +-- no depende directamente de servicios externos
  |
  +-- no depende de auditoria
  |
  +-- no depende de automatizacion
  |
  v
INTERFACES
  |
  +-- conectores
  +-- herramientas
  +-- verificadores

Cada capa tiene una responsabilidad definida.

logica
  |
  v
modelo
  |
  v
conector
  |
  v
externo

El flujo inverso tambien debe poder verificarse:

externo
  |
  v
conector
  |
  v
modelo
  |
  v
logica

---

Filosofia

Valores- busca mantener una arquitectura donde los componentes externos sean herramientas y no autoridades sobre la logica interna.

Los servicios externos pueden proporcionar:

- datos
- interfaces
- ejecucion
- verificaciones
- infraestructura

pero la estructura interna debe permanecer bajo control del proyecto.

externo
   |
   v
conector
   |
   v
modelo
   |
   v
logica
   |
   v
decision

La frontera es importante:

+-----------------------------+
|        VALORES-             |
|                             |
|  logica                     |
|  modelos                    |
|  analisis                   |
|  decisiones                 |
|                             |
+-------------+---------------+
              |
              v
       interfaz controlada
              |
              v
+-----------------------------+
|      componentes externos   |
+-----------------------------+

---

Desarrollo

Requisitos

Python
Node.js
npm
Hardhat

Hardhat es necesario cuando se trabaja con contratos.

Instalacion

git clone <repositorio>
cd Valores-

Python

pip install -r requirements.txt

Node.js

npm install

Contratos

Si el proyecto utiliza Hardhat:

npx hardhat

Los comandos concretos de compilacion, prueba o despliegue deben depender de la estructura existente del proyecto.

---

Verificacion

Las comprobaciones automaticas se ejecutan mediante GitHub Actions.

GitHub
   |
   v
Actions
   |
   +-- Python
   |
   +-- JavaScript
   |
   v
CodeQL
   |
   v
analisis
   |
   v
reporte

El flujo de verificacion no sustituye las pruebas del proyecto; funciona como una capa adicional de analisis.

---

Flujo general

La arquitectura completa puede representarse como:

                    DATO
                      |
                      v
                  MODELO
                      |
                      v
                   LOGICA
                      |
          +-----------+-----------+
          |                       |
          v                       v
      CONECTOR                ANALISIS
          |                       |
          v                       |
      EXTERNO                     |
          |                       |
          +-----------+-----------+
                      |
                      v
                   RESULTADO
                      |
                      v
                 VERIFICACION
                      |
                      v
                    DECISION

La automatizacion y la auditoria observan el proceso desde una capa independiente:

                     PROYECTO
                        |
        +---------------+---------------+
        |                               |
        v                               v
     ejecucion                      auditoria
        |                               |
        v                               v
     resultado                       CodeQL
        |                               |
        +---------------+---------------+
                        |
                        v
                     reporte

---

Estado del proyecto

Proyecto en desarrollo.

Las prioridades actuales son:

1. ordenar la arquitectura
2. separar responsabilidades
3. mantener seguridad de dependencias
4. documentar componentes
5. aislar conectores externos
6. automatizar verificaciones
7. mejorar trazabilidad y auditoria

La prioridad arquitectonica es mantener una base pequena, modular y verificable antes de aumentar el numero de dependencias o integraciones.

---

Licencia

Apache 2.0

---

Resumen arquitectonico

                         VALORES-
                            |
             +--------------+--------------+
             |              |              |
           Python       JavaScript      GitHub
             |              |              |
          modelos        Hardhat        Actions
          runners        contratos      CodeQL
          analisis       scripts          |
             |              |              |
             +--------------+--------------+
                            |
                         SEGURIDAD
                            |
                         AUDITORIA
                            |
                            v
                         DECISION

La regla central es simple:

logica interna
      |
      v
interfaces controladas
      |
      v
componentes externos

Los componentes externos conectan, automatizan o verifican; la logica principal permanece separada.
