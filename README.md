<img  align="left" width="150" style="float: left;" src="https://www.upm.es/sfs/Rectorado/Gabinete%20del%20Rector/Logos/UPM/CEI/LOGOTIPO%20leyenda%20color%20JPG%20p.png">
<img  align="right" width="60" style="float: right;" src="https://www.dit.upm.es/images/dit08.gif">


<br/><br/>
# Práctica Actores Akka

## 1. Objetivo:
- Comprender los conceptos básicos de actores y su implementación en Scala utilizando Akka.

## 2. Dependencias

Para realizar la práctica el alumno deberá tener instalado en su ordenador:
- Herramienta GIT para gestión de repositorios [Github](https://git-scm.com/downloads)
- Scala 2.13.12 [scala](https://www.scala-lang.org/download/2.13.0.html)
- Scala-sbt [scala-sbt](https://www.scala-sbt.org/download/)
- Máquina virtual con sistema operativo linux y distribución Ubuntu 22.04 (Disponible en el enlace compartido en moodle) 


## 3. Parte 1: Introducción a Actores con Akka en Scala 


#### Instrucciones:

1. **Configuración del Proyecto:**
   - Clonar el repositorio y accder al directorio del proyecto
    ```
     git clone https://github.com/Big-Data-ETSIT/PX-actores-akka.git
     cd PX-actores-akka
    ```
   - Verificar el correcto funcionamiento de sbt, ejecutando el código escrito en el fichero AkkaLab
    ```
     cd akka-lab-scala
     sbt 
    ```
    Una vez habilitado el prompt the sbt ejecutar la app usando el comando run
     ```
     sbt:akka-quickstart-scala> run
    ```

   - Modifique  el proyecto Scala en su entorno de desarrollo preferido.
   - Agrega la dependencia de Akka al archivo `build.sbt`:

     ```scala
     libraryDependencies += "com.typesafe.akka" %% "akka-actor" % "2.6.16"
     ```

2. **Definición de un Actor:**
   - Crea un actor simple llamado `HelloActor` que responda a los mensajes con un saludo.
   - Implementa la lógica para imprimir el saludo al recibir un mensaje.

     ```scala
     import akka.actor._

     class HelloActor extends Actor {
       def receive: Receive = {
         case "Saludo" => println("¡Hola desde el actor!")
         case _        => println("Mensaje desconocido")
       }
     }
     ```

3. **Creación y Uso de Actores:**
   - Crea un objeto que instancie y utilice el actor `HelloActor`.
   - Envía mensajes al actor para verificar su funcionamiento.

     ```scala
     import akka.actor._

     object Main extends App {
       val system = ActorSystem("HelloSystem")
       val helloActor = system.actorOf(Props[HelloActor], name = "helloActor")

       helloActor ! "Saludo"
       helloActor ! "OtroMensaje"

       system.terminate()
     }
     ```

4. **Interacción entre Actores:**
   - Extiende tu programa para incluir la interacción entre dos actores.
   - Define un nuevo actor, por ejemplo, `GreetResponder`, que responda a los saludos del `HelloActor`.

     ```scala
     class GreetResponder extends Actor {
       def receive: Receive = {
         case "Saludo" => println("¡Hola desde el GreetResponder!")
         case _        => println("Mensaje desconocido en GreetResponder")
       }
     }
     ```

   - Modifica el `Main` para que `HelloActor` y `GreetResponder` se comuniquen entre sí.

   ```scala
   object Main extends App {
     // Crear el sistema de actores
     val system = ActorSystem("HelloSystem")
   
     // Crear el actor GreetResponder
     val greetResponder = system.actorOf(Props[GreetResponder], name = "greetResponder")
   
     // Crear el actor HelloActor, pasándole el actor GreetResponder
     val helloActor = system.actorOf(Props(new HelloActor(greetResponder)), name = "helloActor")
   
     // Enviar mensaje de saludo a HelloActor
     helloActor ! "Saludo"
     greetResponder ! "Saludo"
   
     // Terminar el sistema después de un tiempo para ver la salida
     Thread.sleep(1000)
     system.terminate()
   }

   ```


5. **Manejo del Ciclo de Vida:**
   - Comprende el ciclo de vida de un actor implementando métodos como `preStart` y `postStop`.
   - Modifica `HelloActor` para imprimir un mensaje cuando se inicia y se detiene.

     ```scala
     class HelloActor extends Actor {
       override def preStart(): Unit = println("HelloActor se ha iniciado.")
       override def postStop(): Unit = println("HelloActor se ha detenido.")

       def receive: Receive = {
         case "Saludo" => println("¡Hola desde el actor!")
         case _        => println("Mensaje desconocido")
       }
     }
     ```

6. **Ejecución y Observación:**
   - Ejecuta tu programa y observa la salida para entender cómo los actores manejan los mensajes y sus ciclos de vida.

## 4. Parte 2: Desarrollo No Guiado 

#### Objetivo:
- Profundizar en el uso de actores con Akka mediante la implementación de un programa más complejo.

#### Instrucciones :


1. **Manejo de Estado:**
   - Introduce el concepto de estado mutable en al menos uno de tus actores.
   - Implementa un actor que mantenga un estado relevante para operar sobre una cuenta bancaria.

   Por ejemplo, crea un actor en el que inicalmente su estado mantenga un valor de 6 y que devuelva dicho valor cuando reciva el mensaje `balance`. A partir de ese valor incial, cada vez que reciba un mensaje de `deposit` , este debe incrementar en una unidad el valor actual.
   Asímismo se debe restar un una unidad el valor actual cuando se reciba el mensaje de `wihtdaw`. Finalmente, el valor de la cuenta se reinicia cuando se recibe el mensaje de `reset`. Para cualquier otro mensaje, el actor debe responder un mensaje de advertencia indicando que no se encuentra la operación.

2. **Mensajes entre actores:**
Crear tres actores para manejar una cuenta bancaria en Scala utilizando Akka. Cada actor tiene una responsabilidad específica en la gestión de la cuenta bancaria:

1. **AccountBalanceActor:** Este actor maneja las consultas de saldo y responde con el saldo actual.


2. **DepositActor:** Este actor se encarga de los depósitos, incrementando el saldo de la cuenta en una unidad.


3. **WithdrawActor:** Este actor se encarga de los retiros, disminuyendo el saldo de la cuenta en una unidad.

4. Defina la clase `main` en donde se vea la interacción entre cada uno de los actores, por ejemplo que se realicen 5 depositos, dos retiros y que se intente realizar una operación no permitida.


## 5. Instrucciones para la Entrega y Evaluación.
El alumno debe subir un fichero pdf a moodle en donde se incluyan el código realizado en la parte 2 y las capturas de la ejecución donde se muestren los mensajes recibidos por cada uno de los actores solicitados en los puntos 1 y 2 junto a una descripción de las mismas.




