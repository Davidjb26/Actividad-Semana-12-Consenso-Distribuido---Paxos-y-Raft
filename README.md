# Prototipo de Consenso Distribuido: Algoritmo Raft

Este repositorio contiene una simulación funcional y didáctica del algoritmo de consenso distribuido **Raft**, programado en Python utilizando programación concurrente (*hilos*). El proyecto simula un clúster de 3 nodos independientes, recreando los procesos críticos de elección de líder, replicación de registros y resiliencia ante fallos de infraestructura.

---

## Características del Prototipo

* **Elección de Líder Autónoma:** Los nodos inician como seguidores (*Followers*) y, mediante temporizadores aleatorios (*Election Timeouts*), gestionan la postulación y votación para elegir un nuevo líder.
* **Replicación de Logs:** El líder centraliza las propuestas de los clientes (ej. `"A=1"`) y las distribuye logrando un consenso por mayoría absoluta (Quorum).
* **Tolerancia a Fallos (CFT):** Simulación en tiempo real de la caída catastrófica del líder. El sistema detecta la ausencia, reelege un líder interino en un nuevo término y continúa operando de forma consistente con los nodos supervivientes.

---

## Paxos vs. Raft: Tabla Comparativa

| Criterio | Paxos (Classic) | Raft |
| :--- | :--- | :--- |
| **Claridad** | **Muy baja.** Formulación puramente matemática y abstracta. | **Alta.** Diseñado explícitamente para ser entendible y gobernable. |
| **Complejidad** | **Alta.** Pasar al código suele requerir variantes complejas (*Multi-Paxos*). | **Moderada.** Estructurado en subproblemas limpios (Elección, Replicación). |
| **Rendimiento** | Alto, pero propenso a *livelocks* bajo alta competencia de proponentes. | Alto y predecible gracias a su esquema de Líder Fuerte. |
| **Casos de Uso** | Google Chubby, capas base de sistemas legados de almacenamiento. | **etcd** (Kubernetes), **Consul**, CockroachDB. |

---

## Estructura del Código

El archivo principal `main.py` se compone de los siguientes bloques lógicos de ingeniería:

* `__init__`: Inicialización del estado interno de Raft (Logs, Términos, Votos y estado de Red del nodo).
* `run`: Bucle síncrono que evalúa los *timeouts* de latidos y despierta las candidaturas.
* `start_election` & `receive_request_vote`: Orquestación del protocolo de votación distribuida para alcanzar el Quorum ($\lfloor N/2 \rfloor + 1$).
* `send_heartbeats` & `receive_heartbeat`: Mecanismo periódico de sincronización de datos y mantenimiento de jerarquía.
* `propose_value`: Interfaz de recepción de comandos y validación de commits por mayoría.

---

## Requisitos e Instalación

El prototipo fue desarrollado utilizando únicamente la biblioteca estándar de **Python 3**, por lo que no requiere dependencias de terceros.

1. Clona este repositorio:
   ```bash
   git clone [https://github.com/TU_USUARIO/TU_REPOSITORIO.git](https://github.com/TU_USUARIO/TU_REPOSITORIO.git)
   cd TU_REPOSITORIO
