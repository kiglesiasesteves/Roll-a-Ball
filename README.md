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

## Recogida de PickUp

Otra forma de enriquecer el juego es con una recogida de pickup que de hecho será la forma de ganar el juego y pasar la siguiente nivel. 

![pickup](https://github.com/user-attachments/assets/a49b83d0-2b7c-4f3e-8fa3-3ae1dbb5b430)

Para hacer esto usaremos un trigger: 
```
  void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.CompareTag("PickUp"))
        {
            other.gameObject.SetActive(false);
            count += 1;
            SetCountText();
            Debug.Log("Objeto recogido. Count: " + count);
        }
    }
}
```

## Cilindros Empujadores

Para aumentar la dificultad, hemos añadido cilindros que empujan al jugador al tocarlos. Esto obliga al jugador a ser más estratégico para evitar caer del mapa o ser arrastrado hacia enemigos.

![cilindros](https://github.com/user-attachments/assets/0f6100ba-9b41-4686-8405-3c4dc8bbc805)

Los cilindros empujadores funcionan con una colisión 
```
 public float fuerzaEmpuje = 5f;

    // Método que se llama cuando hay una colisión
    private void OnCollisionEnter(Collision collision)
    {
        // Comprobar si el objeto con el que colisionamos es el jugador
        if (collision.gameObject.CompareTag("Player"))
        {
            // Obtener la dirección de la colisión
            Vector3 direccionEmpuje = collision.transform.position - transform.position;

            // Normalizar la dirección para evitar que el empuje sea más fuerte en ciertas direcciones
            direccionEmpuje = direccionEmpuje.normalized;

            // Aplicar una fuerza de empuje en la dirección opuesta a la colisión
            Rigidbody rbJugador = collision.gameObject.GetComponent<Rigidbody>();
            if (rbJugador != null)
            {
                rbJugador.AddForce(direccionEmpuje * fuerzaEmpuje, ForceMode.Impulse);
            }
        }
    }
```


## Tablas Giratorias

Hemos incluido plataformas giratorias que permiten acceder a nuevas áreas con pickups. Estas tablas añaden movimiento al mapa y hacen que el juego sea más dinámico y divertido.

![tabalsgiratoriasd](https://github.com/user-attachments/assets/f05b7d97-307a-471b-aed5-5ca3cca8f552)


Código de la Tabla Giratoria:
```
 public float rotationSpeed = 30f;

    void Update()
    {
        // Rotar en torno al eje Y (horizontal)
        transform.Rotate(0, rotationSpeed * Time.deltaTime, 0);
    }
```

## Pasarelas Falsas

Para añadir un elemento de sorpresa, hemos incorporado pasarelas falsas, es decir, caminos que solo son visibles pero no tienen colisión física. Esto hace que el jugador pueda caer al vacío si elige la ruta incorrecta.

![pasarelafalsa](https://github.com/user-attachments/assets/c8f10fe8-c927-4f2f-b268-0bc06d815c26)

Como vemos no tiene box Collider que permitiría detectar la colisión. Es por eso que tenemos este efecto. 

![image](https://github.com/user-attachments/assets/b6b08347-0c3b-464b-b869-95d692fe87da)


## Gestión de Caídas

En un juego de tipo Roll-a-Ball, es común que el jugador caiga del escenario. Para solucionar esto, hemos añadido una función en el código del PlayerController que detecta si la bola ha caído por debajo de una altura determinada (ejemplo: y = -50). Si esto ocurre, el nivel se reinicia automáticamente.

![gestioncaidas](https://github.com/user-attachments/assets/62394e81-b0ca-44fa-8f46-64c4051d461f)

Para gestionar eso usamos el siguiente código: 

```
    public float positionY = -50f;
    private void Update()
    {
        if (transform.position.y <= positionY)
        {
            RestartGame();
        }
    }

    // Función para reiniciar el juego
    private void RestartGame()
    {
        // Reiniciar la escena actual
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }
}
```


## Enemigo con Proyectiles

Además de perseguir al jugador, los enemigos ahora pueden disparar proyectiles en su dirección. Estos proyectiles heredan la dirección del movimiento del enemigo, aumentando la dificultad del juego.

![enemigoshoot](https://github.com/user-attachments/assets/a9df1096-792f-4b15-9db2-98317d8c1cb8)

Los proyectiles son prefabs que el enemigo va clonando, para realizar esto hemos usado el siguiente código:
```
 public GameObject projectilePrefab;
    public Transform shootingPoint;
    public float fireRate = 1f;  // Cuánto tiempo pasa entre cada disparo
    private float nextFireTime = 0f;

    void Update()
    {
        // Dispara el proyectil en intervalos regulares
        if (Time.time >= nextFireTime)
        {
            Shoot();
            nextFireTime = Time.time + 1f / fireRate;
        }
    }

void Shoot()
{
    Debug.Log("Disparando proyectil");
    Instantiate(projectilePrefab, shootingPoint.position, shootingPoint.rotation);
}
```


## Vidas del Jugador

Para gestionar la dificultad, hemos añadido un sistema de vidas. Cada vez que el jugador es alcanzado por un proyectil, su barra de vida disminuye. Si la salud llega a cero, el nivel se reinicia o el jugador pierde la partida.

![vidas](https://github.com/user-attachments/assets/518d7ae4-a5ba-4d71-bd3b-21b4cbc8361a)

Código para la Gestión de Vida:
```
void Start()
    {
        UpdateHealthText(); // Actualiza el texto al iniciar
    }

    public void TakeDamage(int damage)
    {
        if (!invulnerable)
        {
            health -= damage;
            UpdateHealthText(); // 🔹 Llamamos a UpdateHealthText() después de reducir la vida

            if (health <= 0)
            {
                Die();
            }
        }
    }

    private void Die()
    {
        Debug.Log("¡El jugador ha muerto!");
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }

    private void UpdateHealthText()
    {
        if (TextoVida != null) // Evita errores si el texto no está asignado
        {
            TextoVida.text = "Vidas: " + health.ToString();
        }
        else
        {
            Debug.LogError("TextoVida no está asignado en el Inspector.");
        }
    }
```
## Estados 
Para definir estados de ciertos elementos hemos usado State que nos permite cambiar el estado de por ejemplo el jugador

![image](https://github.com/user-attachments/assets/92afaae9-28f4-41ec-84ce-4cd9aec15b48)

Primero definimos los estados que queremos usar:
![image](https://github.com/user-attachments/assets/8bc11612-c2f6-4596-9a1e-388d9ecb35f1)

Luego vamos modificansdo el estado del jugador a medida que es preciso en nuestro código: 

![image](https://github.com/user-attachments/assets/cbe7daef-f4d3-45a6-9d28-c816532ebcde)
![image](https://github.com/user-attachments/assets/db59af53-2927-4dc5-bece-2a4a0ca4e199)

## Estados con Animator 
Primero definimos los estados en el Animator 

![image](https://github.com/user-attachments/assets/f6436616-927a-40b0-9715-edec2e6cf4a3)

Cuando tenemos los estados definidos podemos modificarlo como los estados en el código y en la parte que corresponda. 

```
animator.SetTrigger("HasWon");
```




## Cambio de Escenas (Niveles)

Para hacer el juego más interesante, hemos implementado múltiples niveles. Una vez configuradas las escenas en Unity (File > Build Settings > Scenes in Build), podemos programar la transición entre niveles usando SceneManager.LoadScene().
![pasarNIvelFinal](https://github.com/user-attachments/assets/846e8104-9075-44db-b74d-1f5b3b4a8c9d)

Código para Cambiar de Nivel:
```
using UnityEngine.SceneManagement;

void LoadNextLevel()
{
    SceneManager.LoadScene(nextSceneName);
}
```

## Enemigo Final

En el nivel 3, nos enfrentamos al enemigo final, llamado El Mosca. Este enemigo es estático, pero dispara proyectiles mucho más grandes (representados como Apple Pencil en el juego).

![elmosca](https://github.com/user-attachments/assets/e3627f45-b57f-4a87-9be0-32c9cab52361)

El código es igual a un enemigo regular, al diferencia es que no está hecho para moverse, dispara un prefab diferente y tiene una size más grande. 

## Fin del Juego

El nivel final está diseñado para ser el más desafiante. Todos los pickups desaparecen, excepto el Ultimate PickUp, que está situado cerca del enemigo final y rodeado de proyectiles. El jugador debe esquivar los ataques y alcanzar este último pickup para completar la partida.
![GanarJuego](https://github.com/user-attachments/assets/04a5a161-65fc-4b3c-b7ca-8f5011fd859d)

Con estas mejoras, nuestro juego Roll-a-Ball se convierte en una experiencia más dinámica, estratégica y entretenida para los jugadores.

## Juego Android

Para poder utilizar este juego en el móvil hemos utilizado la herramienta propia de unity para codificar para Android. Después de eso colocamos un acelerometro para gestionar el movimiento de la pelota. 

```
    Vector3 dir = Vector3.zero;
        dir.x = -Input.acceleration.y;
        dir.z = Input.acceleration.x;
        if (dir.sqrMagnitude > 1)
            dir.Normalize();
        
        dir *= Time.deltaTime;
        transform.Translate(dir * speed);
```

En la release tenemos la .apk

