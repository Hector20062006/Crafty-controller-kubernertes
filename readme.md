# Crafty Controller en Kubernetes

Este repositorio contiene los manifiestos necesarios para desplegar **Crafty Controller v4** en un clúster de Kubernetes.

## Descripción

El despliegue utiliza `hostNetwork: true`, lo que permite que el contenedor comparta la red del nodo anfitrión. Esto facilita el acceso a los puertos del servidor de Minecraft y al panel de control sin necesidad de configurar Ingress o LoadBalancers complejos, pero requiere que los puertos estén libres en el nodo.

## Requisitos Previos

1.  **Clúster de Kubernetes** operativo.
2.  **Directorios en el Host**: El despliegue utiliza `hostPath` para la persistencia de datos. Asegúrate de que los siguientes directorios existan en el nodo donde se ejecutará el pod, o edita el archivo `crafty-controller.yml` para ajustar las rutas:
    - `/srv/docker/crafty-4/backups`
    - `/srv/docker/crafty-4/logs`
    - `/srv/docker/crafty-4/servers`
    - `/srv/docker/crafty-4/config`
    - `/srv/docker/crafty-4/import`

## Instalación

1.  Clona este repositorio o descarga el archivo.
2.  Aplica el manifiesto en tu clúster:

    ```bash
    kubectl apply -f crafty-controller.yml
    ```

Esto creará:
- Un **Namespace** llamado `crafty`.
- Un **Deployment** con Crafty Controller.

## Acceso

Una vez desplegado, puedes acceder al panel de control a través de:

- **HTTPS**: `https://<IP-DEL-NODO>:8443`

## Puertos Configurados

| Servicio | Puerto | Protocolo |
|----------|--------|-----------|
| Panel Web (HTTPS) | 8443 | TCP |
| Dynmap | 8123 | TCP |
| Bedrock | 19132 | UDP |
| Minecraft Range | 25500 | TCP |

## Configuración de Volúmenes

El archivo `crafty-controller.yml` monta los siguientes volúmenes desde el host para asegurar que los datos persistan entre reinicios:

- `backups`: Copias de seguridad.
- `logs`: Registros del sistema.
- `servers`: Archivos de los servidores Minecraft.
- `config`: Configuración de la aplicación.
- `import`: Directorio para importar servidores existentes.

