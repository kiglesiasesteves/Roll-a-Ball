# Roll-a-Ball

En este proyecto analizaremos como añadir ciertas funcionalidades extra a nuestro juego del roll-a-ball. Para saber sobre elementos básicos como cámaras existe un repositorio anterior en el que hablamos del movimiento del jugador y de las cámaras. 

## PlayGround

El PlayGround es uno de los elementos más importantes del juego, es lo que va a ver el jugador y donde se va a desarrollar el juego. El playGround puede parecer que solo se basa en la creación de elementos 3D para interactuar con ellos pero es mucho más que eso. 

### Rampa con boost

Una cosa que podemos hacer con nuestro playground es añadir una especie de potenciadores para nuestro jugador en el propio playground (otra opción sería añadirlos en un pickup como pasa en juegos de MarioBros).
Para eso hablaremos de la rampa que nos ofrece un aumento de velocidad cuando pasamos por ella. Veamos el ejemplo. 

![rampaBoost](https://github.com/user-attachments/assets/7572f463-b012-43b1-825f-af94c3bf9390)

Esta rampa tiene un Script que nos permite realizar este proceso. Este es el método exacto:
´´´
private void OnTriggerEnter(Collider other)
    {
        // Comprobar si el objeto que entra en el trigger es el jugador
        if (other.CompareTag("Player"))
        {
            // Aplicar el aumento de velocidad
            other.GetComponent<Rigidbody>().velocity += new Vector3(0, 0, speedBoost);
        }
    }
    ´´´
### Cilindros empujones

Otro extra que he añadido en el playground ha sido cilindros que nos empujan a la hora de tocarlos. Esto hace que la difcultad aumente ya que tenemos que tener cuidado para que no nos tiren para fuera del mapa o nos acerquen a nuestros enemigos 

(Gif cilindrs)

El método que permite reaizar esto es el siguiente:

### Tablas giratorias

La tabla giratoria nos permiten acceder a otra tabla donde tenemos los pick-up que deberemos recoger para ganar el nivel. Tener esta tabla nos permite añadir movimiento a nuestro mapa y diversión. 

(gif tabla giratoria)

El código que nos permite realizar esto es el siguiente
(código)

### Pasarelas falsas

En mi juego también he añadido dos pasarelas, una de las cuales no tiene material físico es decir es tan solo dibujo. Eso hace con que el jugador pueda perder por el hecho de equivocarse de pasarela. 

(gif pasarela)

### Gestionar caídas

Al ser un juego de Roll-A-Ball es muy fácil que la pelota caiga del lugar. Es por eso que he añadido una función en el propio código de Player Controller que nos permite que al caer la bola y encontrarse en el eje y a = - 50 nos vuelva a empezar el juego. Al hacer eso tenemos que tener cuidado ya que debemos comprobar que en nuestro mapa no tengamos una parte que pueda llegar al valor que nosotros hemos decidido para reiniciar el juego ya que tendríamos un problema. 

(gif vuelta inicio)


### Enemigo que dispara

Además de que el enemigo nos persiga y nos mate en el caso de que nos acorrale (no al tocar) también dispara proyectiles. El proyectil tiene como root el enemigo por eso los proyectiles irán en la dirección del movimiento del enemigo y por lo tanto contra el jugador 

(gif dhooting)

### Vidas jugador

Por esto anterior las vidas del jugador disminuyen en el caso de que alguno de esos proyectiles lo alcance, por eso tenemos que gestionar la salud del jugador para que vaya disminuyendo a medida que le tocan los proyectiles. 

(gif vida)

Para este código hemos utilizado este método

### Cambiar escenas (niveles)

Cualquier juego que se precie tiene un número de niveles, es por eso que s importante cambiar entre ellos. Después de añadirles como open scenes en la configuración del FIle de UNity tenemos que crear el método para que cmabie de nivel cuando nosotros queremos y con la condición que deesemos.

(gif cambioNIvel)

Para usar eso hemos usado el LOadScene y este método

### Final Enemy

En el caso de llegar al nivel 3 tendremos el enemigo final, el enemigo final está estático y no se mueve, tiene una tag Enemy que es "El Mosca". Este enemigo expulsa como proyectiles un prefab que es el dibujo de un apple pencil y que es mucho más grande. 

(gif finalEnemy)

### FinalGame

Para el final del juego es decir el nivel 3 nos desaparecen todoso los pick up y solo tenemos el UltimatePickUp que está al lado del final enemy y justo al lado de cuando expulsa los apple pencil projectile. Por eso el final del juego es complejo. 

(gif UltimatePickUp)



