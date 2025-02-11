# Roll-a-Ball: Funcionalidades Avanzadas

En este proyecto analizaremos cómo añadir funcionalidades extra a nuestro juego Roll-a-Ball. Si buscas información sobre los elementos básicos, como el movimiento del jugador o las cámaras, puedes consultar un repositorio anterior donde abordamos esos temas en detalle.

## PlayGround

El PlayGround es uno de los elementos más importantes del juego, ya que define el entorno en el que el jugador se moverá e interactuará. Aunque pueda parecer que se trata solo de la creación de elementos 3D, en realidad es mucho más que eso. A continuación, exploraremos algunas de las mejoras que hemos implementado en el PlayGround para hacer el juego más dinámico y desafiante.

![image](https://github.com/user-attachments/assets/3df51fcb-9546-4f1a-b7a7-2a5f79c0e363)


## Rampa con Boost

Una forma de enriquecer la experiencia de juego es añadiendo potenciadores en el escenario, como ocurre en algunos títulos de Mario Bros. En este caso, hemos implementado una rampa que otorga un aumento de velocidad al jugador al subirla.

![rampaBoost](https://github.com/user-attachments/assets/f5f33d4d-87c5-4151-92b4-899860748f98)


Código de la Rampa con Boost:
```
private void OnTriggerEnter(Collider other)
{
    // Comprobar si el objeto que entra en el trigger es el jugador
    if (other.CompareTag("Player"))
    {
        // Aplicar el aumento de velocidad
        other.GetComponent<Rigidbody>().velocity += new Vector3(0, 0, speedBoost);
    }
}
```

## Cilindros Empujadores

Para aumentar la dificultad, hemos añadido cilindros que empujan al jugador al tocarlos. Esto obliga al jugador a ser más estratégico para evitar caer del mapa o ser arrastrado hacia enemigos.

![cilindros](https://github.com/user-attachments/assets/0f6100ba-9b41-4686-8405-3c4dc8bbc805)

## Tablas Giratorias

Hemos incluido plataformas giratorias que permiten acceder a nuevas áreas con pickups. Estas tablas añaden movimiento al mapa y hacen que el juego sea más dinámico y divertido.

![tabalsgiratoriasd](https://github.com/user-attachments/assets/f05b7d97-307a-471b-aed5-5ca3cca8f552)


Código de la Tabla Giratoria:

(Código correspondiente a la rotación de la tabla)

## Pasarelas Falsas

Para añadir un elemento de sorpresa, hemos incorporado pasarelas falsas, es decir, caminos que solo son visibles pero no tienen colisión física. Esto hace que el jugador pueda caer al vacío si elige la ruta incorrecta.

![pasarelafalsa](https://github.com/user-attachments/assets/c8f10fe8-c927-4f2f-b268-0bc06d815c26)

## Gestión de Caídas

En un juego de tipo Roll-a-Ball, es común que el jugador caiga del escenario. Para solucionar esto, hemos añadido una función en el código del PlayerController que detecta si la bola ha caído por debajo de una altura determinada (ejemplo: y = -50). Si esto ocurre, el nivel se reinicia automáticamente.

![gestioncaidas](https://github.com/user-attachments/assets/62394e81-b0ca-44fa-8f46-64c4051d461f)

## Enemigo con Proyectiles

Además de perseguir al jugador, los enemigos ahora pueden disparar proyectiles en su dirección. Estos proyectiles heredan la dirección del movimiento del enemigo, aumentando la dificultad del juego.

![enemigoshoot](https://github.com/user-attachments/assets/a9df1096-792f-4b15-9db2-98317d8c1cb8)


## Vidas del Jugador

Para gestionar la dificultad, hemos añadido un sistema de vidas. Cada vez que el jugador es alcanzado por un proyectil, su barra de vida disminuye. Si la salud llega a cero, el nivel se reinicia o el jugador pierde la partida.

![vidas](https://github.com/user-attachments/assets/518d7ae4-a5ba-4d71-bd3b-21b4cbc8361a)

Código para la Gestión de Vida:


(Código correspondiente a la disminución de vida del jugador)

## Cambio de Escenas (Niveles)

Para hacer el juego más interesante, hemos implementado múltiples niveles. Una vez configuradas las escenas en Unity (File > Build Settings > Scenes in Build), podemos programar la transición entre niveles usando SceneManager.LoadScene().
![pasarNIvelFinal](https://github.com/user-attachments/assets/846e8104-9075-44db-b74d-1f5b3b4a8c9d)

Código para Cambiar de Nivel:

using UnityEngine.SceneManagement;

void LoadNextLevel()
{
    SceneManager.LoadScene(nextSceneName);
}

## Enemigo Final

En el nivel 3, nos enfrentamos al enemigo final, llamado El Mosca. Este enemigo es estático, pero dispara proyectiles mucho más grandes (representados como Apple Pencil en el juego). Derrotarlo es el último desafío antes de completar el juego.

![elmosca](https://github.com/user-attachments/assets/e3627f45-b57f-4a87-9be0-32c9cab52361)


## Fin del Juego

El nivel final está diseñado para ser el más desafiante. Todos los pickups desaparecen, excepto el Ultimate PickUp, que está situado cerca del enemigo final y rodeado de proyectiles. El jugador debe esquivar los ataques y alcanzar este último pickup para completar la partida.
![GanarJuego](https://github.com/user-attachments/assets/04a5a161-65fc-4b3c-b7ca-8f5011fd859d)

Con estas mejoras, nuestro juego Roll-a-Ball se convierte en una experiencia más dinámica, estratégica y entretenida para los jugadores.
