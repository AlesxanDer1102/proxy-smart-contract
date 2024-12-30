# Ejercicio: Proxy Upgradable Smart Contract

Este proyecto es un ejercicio práctico para aprender a implementar contratos inteligentes (smart contracts) actualizables utilizando proxies en Solidity. La arquitectura del proyecto se basa en el estándar ERC1967 y utiliza la biblioteca de OpenZeppelin para facilitar la implementación de proxies y contratos actualizables.

## Tabla de Contenido

- [Descripción General](#descripción-general)
- [Contexto del Proyecto](#contexto-del-proyecto)
- [Tecnologías y Herramientas](#tecnologías-y-herramientas)
- [Instalación y Configuración](#instalación-y-configuración)
- [Compilación y Pruebas](#compilación-y-pruebas)
- [Despliegue](#despliegue)
- [Uso](#uso)
- [Arquitectura](#arquitectura)
- [Contribuciones](#contribuciones)
- [Licencia](#licencia)

## Descripción General

El proyecto consiste en un contrato base (BoxV1) que almacena un número y permite obtener su valor. Además, incluye una versión mejorada (BoxV2) y un proxy ERC1967 que permite la actualización del contrato sin cambiar la dirección del contrato desplegado. Esto resulta en un sistema que admite mejoras sin interrumpir las integraciones existentes.

El propósito de este ejercicio es entender cómo funcionan los proxies y cómo se pueden utilizar para crear contratos actualizables en Solidity.

## Contexto del Proyecto

En los sistemas blockchain, los contratos inteligentes desplegados son inmutables, lo que significa que no pueden modificarse después del despliegue. Los proxies resuelven este problema permitiendo la actualización del contrato lógico (implementación) sin cambiar la dirección del contrato que interactúa con los usuarios.

Este proyecto incluye:

- **BoxV1**: Primera versión del contrato.
- **BoxV2**: Versión mejorada con nuevas funcionalidades.
- **ERC1967Proxy**: Proxy que actúa como intermediario entre el usuario y el contrato lógico.

## Tecnologías y Herramientas

- **Solidity**: Versión 0.8.24.
- **Foundry**: Para compilación, pruebas y scripting.
- **OpenZeppelin Contracts**: Para proxies y contratos actualizables.
- **DevOpsTools**: Herramienta para gestionar despliegues.



## Instalación y Configuración

Sigue estos pasos para clonar e instalar el proyecto:

```bash
# Clona el repositorio
git clone https://github.com/AlesxanDer1102/proxy-smart-contract.git
cd proxy-smart-contract

# Instala las dependencias con Foundry
forge install
```

Asegúrate de cumplir con los requisitos previos:

- **Foundry** instalado. Puedes instalarlo siguiendo las instrucciones de la [documentación oficial](https://book.getfoundry.sh/getting-started/installation).
- Cliente Ethereum local (por ejemplo, Anvil).

## Compilación y Pruebas

### Compilación

Para compilar los contratos:

```bash
forge build
```

### Pruebas

Para ejecutar las pruebas unitarias:

```bash
forge test
```

Las pruebas garantizan que el proxy y las actualizaciones del contrato funcionen correctamente.

## Despliegue

### Red Local

Usa el siguiente comando para desplegar el contrato en una red local:

```bash
anvil
```

```bash
forge script script/DeployBox.s.sol:DeployBox --rpc-url http://localhost:8545 --broadcast
```

### Testnet Sepolia

Para desplegar en Sepolia, necesitas configurar las variables de entorno para la clave privada del despliegue. Luego ejecuta:

```bash
forge script script/DeployBox.s.sol:DeployBox --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify --etherscan-api-key $ETHERSCAN_API_KEY -vvvvv
```

**Dirección del contrato desplegado:** (0xb6c5136de571efe2c2aa631f100c9a5fcfb16566)

## Uso

Una vez desplegado, puedes interactuar con el contrato a través del proxy. Aquí hay un ejemplo de cómo llamar funciones:

### Leer el número almacenado

```javascript
const boxProxy = new ethers.Contract(proxyAddress, abi, provider);
const number = await boxProxy.getNumber();
console.log("Número almacenado:", number);
```

### Actualizar a BoxV2

Despliega la nueva versión (BoxV2) y utiliza el script `UpgradeBox`:

```bash
forge script script/UpgradeBox.s.sol:UpgradeBox --rpc-url $SEPOLIA_RPC_URL --private-key $PRIVATE_KEY --broadcast --verify --etherscan-api-key $ETHERSCAN_API_KEY -vvvvv
```

## Arquitectura

### Estructura del Proyecto

- `src/`
  - `BoxV1.sol`: Primera versión del contrato.
  - `BoxV2.sol`: Segunda versión del contrato.
- `script/`
  - `DeployBox.s.sol`: Script para desplegar el contrato.
  - `UpgradeBox.s.sol`: Script para actualizar el contrato.
- `test/`: Pruebas unitarias.

### Diagramas

```
[Usuario] ---> [Proxy] ---> [BoxV1 | BoxV2]
```

## Contribuciones

Las contribuciones son bienvenidas. Por favor, abre un issue o un pull request con tus sugerencias o mejoras.

## Licencia

Este proyecto está bajo la licencia MIT. Consulta el archivo LICENSE para más detalles.

