# ISTIO

Edmundo Gomez Alvarez

## Introduccion

Istio es una plataforma de servicio de malla (service mesh) de código abierto que se utiliza para gestionar y controlar la comunicación entre los servicios de una aplicación distribuida en un entorno de Kubernetes. Proporciona capacidades avanzadas de enrutamiento, seguridad, monitoreo y observabilidad para los microservicios que se ejecutan en un clúster de Kubernetes.

En un entorno de microservicios, donde una aplicación se compone de varios servicios interconectados, la gestión del tráfico entre ellos puede volverse compleja. Istio simplifica esta tarea al proporcionar una capa de abstracción y control entre los servicios, permitiendo así el enrutamiento, la seguridad y la observabilidad de manera centralizada.

Istio se implementa como una infraestructura adicional en el clúster de Kubernetes, utilizando componentes adicionales como sidecars (contenedores auxiliares) y un plano de control (control plane) que se encarga de administrar y controlar el tráfico de red entre los servicios.

Una de las principales características de Istio es su capacidad de enrutamiento avanzado. Permite definir reglas de enrutamiento basadas en diferentes criterios, como la versión del servicio, las cabeceras de la solicitud, el peso del tráfico, entre otros. Esto facilita la implementación de estrategias de despliegue gradual (canary deployments) o pruebas A/B, donde se pueden enviar diferentes porcentajes de tráfico a diferentes versiones de un servicio.

Además del enrutamiento, Istio también ofrece funciones de seguridad, como la autenticación y autorización entre servicios, el cifrado del tráfico de red y la detección de amenazas. También proporciona capacidades de monitoreo y observabilidad, permitiendo recopilar métricas y registros de los servicios, así como generar informes y alertas sobre el estado de la aplicación.

## Desarrollo

La implementación de Istio en Kubernetes se simplifica en gran medida mediante el uso de Helm, una herramienta de administración de paquetes que facilita la instalación, configuración y gestión de aplicaciones en un clúster de Kubernetes. Con Helm, es posible desplegar Istio en el clúster de manera rápida y sencilla.

Una vez que hemos instalado Istio utilizando Helm, aún hay algunas configuraciones adicionales que debemos realizar para que nuestra aplicación funcione correctamente. Estas configuraciones se centran en la definición de los servicios virtuales y los gateways, dos componentes clave de Istio que permiten el enrutamiento del tráfico hacia nuestros microservicios.

Los servicios virtuales en Istio definen las reglas de enrutamiento para el tráfico que llega a nuestra aplicación. Podemos especificar qué solicitudes deben ser enviadas a qué microservicios en función de diferentes criterios, como las cabeceras de la solicitud, las rutas o los pesos de tráfico. Esto nos brinda un control preciso sobre cómo se distribuye el tráfico entre los diferentes componentes de nuestra aplicación.

Por otro lado, los gateways en Istio actúan como puntos de entrada para el tráfico externo que llega a nuestra aplicación. Configuramos un gateway para exponer nuestros servicios a través de una dirección IP externa o un nombre de dominio, y luego definimos reglas de virtual host para dirigir el tráfico entrante hacia los servicios virtuales correspondientes. Esto nos permite enrutar el tráfico externo de manera eficiente hacia nuestros microservicios internos.

La configuración de servicios virtuales y gateways en Istio se realiza mediante archivos de configuración en formato YAML. Estos archivos contienen la definición de las reglas de enrutamiento, los hosts, los puertos y otra información relevante para cada componente. Podemos definir múltiples servicios virtuales y gateways según nuestras necesidades de aplicación.

![Captura de pantalla 2023-05-29 141718](https://github.com/edez5558/Service_Mesh/assets/122659695/a5c0abd2-1a47-49f7-9861-bf573c0e8910)



```
---
apiVersion: networking.istio.io/v1beta1
kind: Gateway 
metadata:
  name: api-gateway
spec:
  servers:
    - port: 
        number: 80
        name: http
        protocol: HTTP
      hosts:
      - pirata.20.88.189.150.nip.io
  selector:
    istio: ingressgateway 
---
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: api-virtual
spec:
  hosts:
    - pirata.20.88.189.150.nip.io
  gateways:
    - api-gateway
  http:
  - match:
    - uri:
        prefix: /api
    route:
    - destination:
        port:
          number: 80
        host: items-api-svc
  - match:
    - uri:
        prefix: /upload
    route:
    - destination:
        port:
            number: 80
        host: uploader-api-svc
  - match:
    - uri:
        prefix: /predict
    route:
    - destination:
        port:
            number: 80
        host: ai-api-svc

```

Una vez que hayamos configurado adecuadamente los servicios virtuales y gateways, nuestra aplicación estará lista para aprovechar todas las capacidades de Istio, como el enrutamiento avanzado, la seguridad y la observabilidad.

![Captura de pantalla 2023-05-29 142918](https://github.com/edez5558/Service_Mesh/assets/122659695/95b36016-85a5-4360-b79a-6a4864b8d6ca)
