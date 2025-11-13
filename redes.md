<img width="320" height="375" alt="image" src="https://github.com/user-attachments/assets/3ff51c18-03f8-4572-afbf-68f22d3e9d61" /># *RESUMEN FINAL REDES*

## Capa de Transporte

  **la tarea de esta capa es proporcionar un transporte de datos confiable y economico** de la maquina de origen a la de destino, independientemente
  de las redes fisicas de cada una

### Identificacion en la Capa de Transporte 

al igual que en las capas inferiores donde habia un mecanismo de identificacion aqui existe una herramienta de identificacion llamada puerto.

Estos son **numeros de 16 bits que identifican aplicaciones dentro de una maquina**

Cada apicacion dentro de un host que busca establecer una coneccion de red tiene asignado un puerto.

Los puertos desde el 0 hasta el 1023 se llaman puertos conocidos (well known ports) y están reservados para aplicaciones estandar:
  
    Puerto 80 : HTTP Puerto de transferencias de hipertexto (paginas web)
    
    Puerto 23: Telnet Control de terminal remoto.
    
    Puerto 22 : SSH Similar al telnet pero con transferencias seguras.
    
    Puerto 20 y 21 : FTP, protocolo de transferencia de archivos.
    
    Puerto 53: DNS, Servidor de nombres de dominio
    
    Puertos 67 y 68: DHCP, Protocolo de asignación dinámica de direcciones
    
    Puerto 443: HTTPS Transferencia segura de paginas web.
    
    Puerto 115: SFTP: Transferencia de archivos segura.

## NAT Y PAT
  
los puertos tienen un papel muy importante en el internet de hoy en dia, ayudan al ahorro de direcciones ip 

### NAT (network address translation)

NAT traduce las ips privadas dentro de la red a publicas (misma del router) al momento de que el paquete salga de la red del ciente 

### PAT (Port Adress Translation)

asigna a cada equipo un puerto independiente a cada IP publica


## TCP (Transmission Control Protocol)

es un protocolo basado en la capa de transporte. Su funcion es garantizar que los datos de un punto a otro en forma confiable, en orden y 
sin pausas. una coneccion TCP en un flujo de bytes, no de mensajes, siendo de punto a punto y full duplex. cada paquete de bits que 
constituyen las unidades de datos son llamados segmentos y poseen un encabezado con los siguientes componentes

  - source port       (16 bits)  : identifica a la aplicacion transmisora 
  - destination port  (16 bits)  : identifica a la aplicacion receptora
  - sequence number   (32 bits)  : identifica el byte de flujo de datos enviado por el emisor TCP al receptpr TCP   que representa al primer byte del segundo segmento
  - ack               (32 bits)  : contiene el valor del siguiente numero de secuencia que el emisor espera recibir
<img width="380" height="328" alt="image" src="https://github.com/user-attachments/assets/67e06814-02b9-4d7e-883b-067c5fc9af8e" />

  - Header Length     (4 bits)   : longitud de encabezado
  - Reservado         (6 bits)   : reservado para futuros usos
  - URG               (1 Bit)    : indica si hay datos urgentes
  - ACK               (1 Bit)    : indica que el campo ack contiene datoas validos
  - PSH               (1 Bit)    : indica que los datos deben entregarse sin ser almacenados
  - RST               (1 Bit)    : se usa para restabecer de manera repentina

<img width="376" height="325" alt="image" src="https://github.com/user-attachments/assets/2e940434-967b-47b5-938e-4b607360bd3a" />

  - SYN y FIN         (1 Bit)    : se usan para establecer o finalizar una conexion
  - Advertise Window  (16 Bits)  : se utilizan en conjunto con el campo ACK, indica cuantos bytes espera o esta disponible para recibir

<img width="377" height="325" alt="image" src="https://github.com/user-attachments/assets/f96e44fc-73a9-4976-ae68-f0ff506d5275" />


  - CheckSum          (16 Bits)  : suma de verificacion
  - UrgentPointer     (       )  : este campo es valido si el bit URG vale 1 indica dde comienzan los datos no urgentes
  - Opciones          (       )  : se usan para agregar funcionalidad al protocolo
  - Payload           (       )  : los datos que se desean transmitir

<img width="377" height="326" alt="image" src="https://github.com/user-attachments/assets/817f5665-5e25-4185-9ff7-67cebb13c237" />


## Establecimiento de conexion 

  tambien conocido como three way handshake. Es una secuencia de segmentos para establecer una conexion en el que  el cliente manda un 
  segmento con SYN en 1 y un numero aleatorio x en SeqNumber Si el Server decide rechazar la conexión responde con el flag RST en 1. Si 
  la acepta en cambio, responde con los flags SYN y ACK en 1, ademas del siguiente del número aleatorio recibido (x+1) en acknowledgement 
  y otro número aleatorio en SeqNumber (y). A su vez, el cliente responde con el flag ACK en 1 y el siguiente del número aleatorio recibido 
  en acknowledgement.

<img width="427" height="397" alt="image" src="https://github.com/user-attachments/assets/f8adf104-361d-4a38-8e23-a6cbf6ea66b8" />

## Fin de Coneccion 

  Para finalizar una conexión, quien quiera hacerlo un segmento con FIN en 1 y un número aleatorio x en SeqNumber. El otro extremo, responde 
  con el flags ACK en 1, ademas del siguiente del número aleatorio recibido (x+1) en acknowledgement, avisándole a la aplicación que alguien 
  pidió cerrar la conexión Luego de que la aplicación cierra la conexión enviá un segmento con el flag FIN en 1 y un número aleatorio “y” en
  SeqNumber. El otro extremo responde con el flag ACK en 1 y el siguiente del número aleatorio recibido en acknowledgement.

<img width="376" height="374" alt="image" src="https://github.com/user-attachments/assets/8ab21eb2-748d-483d-b44d-be51588fbde0" />

## Transmission Stop and Wait 

  Se envía solo 1 segmento por vez y se espera el ACK para enviar el siguiente.

<img width="607" height="297" alt="image" src="https://github.com/user-attachments/assets/a06b8e07-0d56-4f6a-a4c4-14b12ffcfb26" />

La figura A es el escenario esperable. La figuras B y C ejemplifican la retransmisión por expiración del timer de retransmisión El caso D 
es curioso, el ACK se enviá pero llega tarde, el segmento se retransmitió El receptor deberá descartarlo discriminandolo por su SeqNumber.

<img width="620" height="292" alt="image" src="https://github.com/user-attachments/assets/288ffa8c-f90b-42f2-95f4-350582cc5cd1" />

## Ventana Deslizante 

  El metodo de la ventana deslizante permite enviar mas de un segmento sin esperar el ACK. Luego de enviados todos estos 
  segmentos, espera a recibir los ACK. A medida que recibe los ACK, corre la ventana de transmisión 1 lugar y transmite 
  un segmento. Este método hace mas rápida la Transmisión porque el tiempo de comunicación se divide por el tamaño de 
  la ventana.

<img width="268" height="381" alt="image" src="https://github.com/user-attachments/assets/3b46b972-eede-4432-9f3b-0091cb4f304a" />

  El tamaño de la ventana se negocia al comienzo de la conexión, en función de la memoria disponible que cada aplicación tenga

<img width="262" height="380" alt="image" src="https://github.com/user-attachments/assets/40f596c4-e220-455b-ae6c-782778c42c63" />

## Errores en ventana deslizante 

  El manejo de errores en Stop and Wait es mas sencillo. Cuando un paquete se pierde, se anoticia porque no llega el ACK y se retransmite.
  
  En el Sliding Window, es mas difícil el manejo de errores porque al haber transmitido muchos segmentos, pueden haber llegado algunos y otros no.
  
  De acuerdo a la capacidad de memoria y de procesamiento disponible, existen 2 tipos de manejo de errores en Ventana Deslizante.
  
  Retransmisión Continua y Retransmisión Selectiva.

### Retransmision continua

<img width="717" height="258" alt="image" src="https://github.com/user-attachments/assets/cf125fd2-f0d8-4309-93f0-f52a472c3b59" />

  En el caso de la retransmisión continua, cuando se produce el error, todos los segmentos siguientes son descartados. Cuando el transmisor 
  se da cuenta de que no llego el segmento (2 en este ejemplo) comienza a retransmitir desde ese segmento en adelante (el 2, el 3, el 4 aunque 
  ya los había enviado). Cuando las aplicaciones no tienen mucha capacidad de memoria ni procesamiento, este es el método ideal, aunque con 
  ventanas de transmisión grandes las demoras pueden ser importantes.

### Retransmision selectiva 

<img width="872" height="241" alt="image" src="https://github.com/user-attachments/assets/37d2e7cc-980d-482e-82a1-c187109c6f66" />

  En el caso de la retransmisión selectiva, cuando se produce el error, todos los segmentos siguientes son almacenados hasta que se termina la
  ventana (se envía también un mensaje de no recepción llamado NAK) Cuando el transmisor recibe el NAK, corta la ventana y retransmite el 
  segmento que genero el error (en este ejemplo el 2) y continua con la ventana. La aplicación receptora debe gastar tiempo de procesamiento 
  ahora en ordenar los segmentos. Este es el método mas veloz, pero tiene un mayor consumo de recursos (memoria y procesador).


## Temporizadores TCP

  <img width="411" height="311" alt="image" src="https://github.com/user-attachments/assets/5ae18bb7-8e2c-4da7-b0ba-bfa189180536" />

###   Time Out

  Cuando se envía un segmento, se inicia un temporizador de retransmisiones (Time Out). Si la confirmación de recepción del segmento (ACK) 
  llega antes de que expire el temporizador, éste se detiene. Por otro lado, si el temporizador termina antes de que llegue la confirmación 
  de recepción, se retransmite el segmento (y se inicia de nuevo el temporizador).

<img width="386" height="332" alt="image" src="https://github.com/user-attachments/assets/b49c4535-01ba-486a-b224-617f3be87e6f" />

###   Persistencia

  Está diseñado para evitar el siguiente interbloqueo.

<img width="320" height="375" alt="image" src="https://github.com/user-attachments/assets/58dd64ca-b665-42ee-847c-f234d53788df" />

###  Keep Alive

  Cuando una conexión ha estado inactiva durante demasiado tiempo, el temporizador de seguir con vida puede expirar para ocasionar que un lado 
  compruebe que el otro aún está ahí.

<img width="460" height="355" alt="image" src="https://github.com/user-attachments/assets/27a3ff63-af95-4ee4-9994-01e80814c02f" />












  
  
  
