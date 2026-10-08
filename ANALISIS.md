# Análisis — Taller 4

**Predicción inicial (escrita ANTES de programar)**

1. Si uso `remember { mutableStateOf(0) }` para un contador y roto la pantalla,
   ¿el valor se mantiene o se pierde? ¿Por qué?

   > _Rta/_ El valor deberia mantenerse, al rotar la pantalla se realiza una recomposicion y deberia recordarse el valor almacenado en la variable.

2. Si tengo una lista creada con `remember { mutableListOf(...) }` y le agrego
   un elemento con `.add()`, ¿la pantalla se actualiza? ¿Por qué?

   > _Rta/_ No, mutableListOF es un objeto no observable, no se observan con Compose y no activan una recomposicion cuando cambian.

3. ¿Qué diferencia hay entre `remember` y `rememberSaveable`?
   > _Rta/_ ´remember´ ayuda a retener el estado entre recomposiciones, pero no en cambios de configuracion. ´rememberSaveable´ permite guardar valores en un ´Bundle´.

**Verificación de la predicción**
[Compara lo que predijiste con lo que realmente ocurrió.
Sé honesto: si te equivocaste, dilo y explica por qué.]

> _Rta/_ Me equivoque en la prediciion con ´remember´ pensé que ¨remember´ era suficiente instrucción para que al rotar el contador mantuviese su valor, no conocia el comportamiento de ´rememberSaviable´.

**Paso 2.3 - Diagnostico del Bug**

### ¿Por qué ocurre?

[Explica con tus palabras por qué `mutableListOf` no dispara recomposición.
Menciona: qué tipo de objeto devuelve, si Compose lo observa o no,
y qué papel juega `remember` en todo esto.]

> _Rta/_ ´mutableListOf´es un objeto no observable por Compose y no activa una recomposicion cuando cambia, por eso aunque se registre un nuevo valor en "val logros" este no se ve, aunque ´remember´ puede mostrar el estado de la variable, esta no se actualiza por que se esta registrando en ´mutableListOf´ y si Çompose´no lo ve, no se va a mostrar.

**Paso 2.5 — Documenta la corrección**

### Corrección aplicada

[¿Cuál opción usaste y por qué?]

> _Rta/_ Decidí usar la opcion A, me parecio que generaba menos cambios en el códsigo; ademas, encontre mas lógico modificar la lista existente directamente y no crear una nueva lista, aunque no tengo evidencia, pienso que podria consumir mas recursos en memoria.

### ¿Por qué funciona?

[Explica la diferencia entre `mutableListOf` y `mutableStateListOf`.
Menciona el concepto de lista observable / SnapshotStateList.]

> _Rta/_ ´mutableListOf´ es una lista No observable por ¨Compose¨, asi logros.add("") registre el cambio, este no se va a observar en pantalla, ´mutableStateListOf´ es una lista crada con caracteristicas de ´SnaphotStateList´ , es una lista observable; sin embargo, declarar ´SnapshotStateList<String>´ en el parámetro no es lo que hace que la lista sea observable. Lo que la convierte en estado observable es haberla creado con ´mutableStateListOf´. El tipo del parámetro principalmente documenta y restringe qué clase de lista espera nuestra función.

**Paso 3.7 — Documenta el experimento**

### Con `remember`

[¿Qué pasó al rotar? ¿Por qué?]

> _Rta/_ Se inicio la apliocacion y se incremento el contador hasta 5, como lo muestran los ScreenShots del word adjunto. Al rotar el emulador el contador regreso a cero, esto ocurre porque remember conserva el estado mientras la composición permanece activa, pero no está diseñado para conservar el estado cuando la Activity es destruida y creada nuevamente debido a un cambio como la rotación de pantalla.

### Con `rememberSaveable`

[¿Qué pasó al rotar? ¿Por qué?]

> _Rta/_ Se reemplazó ´remember´ por ´rememberSaveable´, se inició de nuevo la app, se incremento el contador a 5 y se realizó la rotacion, en esta ocasión, el contador permaneció en 5 después de la rotación. Esto demuestra que ´rememberSaveable´ permite conservar y restaurar el valor del estado después de que la Activity es recreada.

### Explicación técnica

[Relaciona esto con el ciclo de vida de la Activity visto en la Clase 04:
qué pasa con `onDestroy`/`onCreate` al rotar, y cómo `rememberSaveable`
usa el Bundle de estado para restaurar el valor.]

> _Rta/_ Cuando se rota el dispositivo, se produce un cambio de configuración, la ´Activity´ actual puede ejecutar ´onDestroy´ y posteriormente se crea una nueva instancia mediante ´onCreate´. El estado almacenado únicamente con remember pertenece a la composición actual. cuando la ´Activity´ es destruida y se crea nuevamente, ese estado se pierde y vuelve a su valor inicial. Usar ´rememberSaveable´ permite guardar el valor del estado utilizando el mecanismo de estado guardado de Android, asociado al Bundle, cuando la nueva instancia de la Activity se crea, ese valor puede ser restaurado.

**Justificación de los Modifier usados**
[Elige 3 Modifier de tu código y explica por qué están en ese orden
y qué pasaría si los movieras]

> _Rta/_
> Modifier ¿Qué hace?
> fillMaxSize() Ocupa todo el espacio disponible
> padding(16.dp) Agrega espacio alrededor del contenido
> size(96.dp) Define ancho y alto de 96 dp
> clip(CircleShape) Recorta la forma para que sea circular
> background(...) Define el fondo
> fillMaxWidth() Ocupa todo el ancho disponible
> height(12.dp) Define una altura específica
> Si unicamente se mueven los ´Modifier´; es decir, se cambia de seccion cada uno, esta instruccion se aplicaria a la seccion en la que se declaran, lo mas seguro es que el comportamiento de la app, aunque compile, deberia ser extraño o con una visualizacion extraña.

**Dificultades encontradas**
[Qué te costó más y cómo lo resolviste]

> _Rta/_ Quiza no fue al algo que me hubiese costado más, sino más bien las dudas que se presentaban a medida que avanzaba, una de ellas fue el uso de estas dos declaraciones:
> SnapshotStateList<String>
> mutableStateListOf()
> en que momento u orden usarlas y que significaba cada una, si alguna se podia omitir o reemplazr con la otra, acudí a la pafida (developer.Android.com) para resolver la duda y tener mayor clariudad en su uso.
