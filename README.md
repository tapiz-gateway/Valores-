Valores-

Descripcion

Valores- es un componente del ecosistema Tapiz orientado a explorar modelos, conectores y herramientas de analisis con una arquitectura modular.

El proyecto busca mantener una separacion clara entre:

- logica del sistema
- conectores externos
- automatizacion
- seguridad
- auditoria

Arquitectura

Valores-

 |
 +-- Python
 |    |
 |    +-- runner
 |    +-- conectores
 |    +-- modelos
 |
 +-- JavaScript
 |    |
 |    +-- Hardhat
 |    +-- contratos
 |
 +-- .github
      |
      +-- workflows
           |
           +-- CodeQL

Seguridad

El repositorio utiliza GitHub Actions con CodeQL para realizar analisis automatico del codigo.

El workflow principal:

.github/workflows/new.codeql.yml

realiza analisis sobre:

- JavaScript
- Python

El objetivo es detectar problemas de seguridad sin modificar la logica del proyecto.

Filosofia

Los componentes externos deben actuar como conectores y herramientas de verificacion.

La logica principal permanece separada de:

- servicios externos
- herramientas de auditoria
- automatizaciones

Desarrollo

Requisitos:

- Python
- Node.js
- npm
- Hardhat (si se trabaja con contratos)

Instalacion:

git clone <repositorio>
cd Valores-

Python:

pip install -r requirements.txt

Node:

npm install

Verificacion

Para ejecutar las comprobaciones automaticas:

GitHub Actions
        |
        v
     CodeQL
        |
        v
   reporte de seguridad

Estado del proyecto

Proyecto en desarrollo.

Las prioridades actuales son:

1. ordenar la arquitectura
2. mantener seguridad de dependencias
3. documentar componentes
4. automatizar verificaciones

Licencia

Apache 2.0
