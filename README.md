Documentacion del Proyecto: Memorama en Godot 4
Descripcion general

Este proyecto es un juego tipo memorama desarrollado en Godot 4 como parte de la actividad de exploracion del editor. El objetivo fue practicar el sistema de nodos, escenas, el Inspector y la conexion de senales.

Numero de pares de cartas: 6 pares, 12 cartas en total.

Tematica de las cartas: cartas de Yu Gi Oh (Adreus Keeper of Armageddon, Alien Ammonite, Alien Mother, Dark Magician Girl the Dragon Knight, Magician of Black Chaos MAX, Magician's Robe).

Enlace jugable: https://twobkaguama.github.io/memorama-test/

Repositorio: https://github.com/TwoBKaguama/memorama-test

Resumen de la documentacion leida

Leccion 1, introduccion a la interfaz del editor: Aprendi a moverme por los paneles principales: el panel de Escena (arbol de nodos), el Inspector (propiedades del nodo seleccionado), el FileSystem (archivos del proyecto: escenas, scripts, assets) y el Viewport (donde se ve y se posiciona todo visualmente). Entendi que casi todo el flujo de trabajo en Godot es seleccionar un nodo en la Escena, ajustar sus propiedades en el Inspector, y ver el resultado en el Viewport.

Leccion 2, creacion del primer juego, escenas y nodos: Aprendi que un proyecto se arma combinando nodos dentro de escenas, y que una escena guardada como archivo .tscn se puede instanciar dentro de otra escena tantas veces como se necesite. Esto lo aplique directamente: mi escena carta.tscn, un TextureButton con su propio script, la instancio 12 veces desde main.gd usando card_scene.instantiate(), en lugar de crear cada carta a mano.

Leccion 3, scripting y senales: Entendi que una senal es una forma de que un nodo avise cuando pasa algo, por ejemplo que lo presionaron, sin que ese nodo necesite saber quien esta escuchando. En mi proyecto use esto declarando una senal propia, signal card_flipped(card_instance), dentro de carta.gd, que se emite cuando el jugador presiona una carta. La conecte desde main.gd con card.card_flipped.connect(_on_card_flipped) para poder comparar las dos cartas seleccionadas.

Preguntas guia para la reflexion

Que diferencia observas entre un nodo y una escena? Un nodo es la pieza mas basica: por ejemplo mi Timer, mi ColorRect o mi Label dentro de PantallaVictoria son nodos sueltos, cada uno con una funcion especifica. Una escena, en cambio, es un conjunto de nodos organizados en jerarquia que se guarda como una unidad reutilizable. En mi caso carta.tscn es una escena completa, el TextureButton con su script carta.gd, que despues puedo instanciar tantas veces como quiera desde main.gd. El nodo es el ladrillo, la escena es la construccion hecha con esos ladrillos.

Por que es importante la jerarquia de nodos, que hijo depende de que padre. Porque el comportamiento de un nodo hijo muchas veces depende del padre. En mi arbol meti GridContainer dentro de CenterContainer, asi el grid completo se centra automaticamente en la pantalla sin que yo calcule posiciones a mano. Otro ejemplo: en PantallaVictoria tengo ColorRect, que contiene a Label y Button; si oculto el nodo padre con pantalla_victoria.hide(), se ocultan tambien el texto y el boton, aunque yo nunca les diga directamente que se oculten.

Como se relaciona el Inspector con el Viewport. El Inspector muestra las propiedades del nodo seleccionado en el arbol, y cualquier cambio que hago ahi, por ejemplo el wait_time de mi Timer o el tamano de una carta, se refleja al instante en el Viewport. Es la relacion de editar y ver: modifico un valor en el Inspector y el Viewport me muestra el resultado sin necesidad de ejecutar el juego.

Que ventajas tiene crear escenas reutilizables, como la escena de la carta? Toda la logica de voltear, flip(), unflip(), match_found(), y la animacion con Tween vive una sola vez dentro de carta.gd. Desde main.gd solo instancio esa escena, le asigno su textura frontal, trasera e id, y conecto su senal card_flipped. Si mas adelante quiero cambiar como se ve o se anima una carta, lo cambio en un solo lugar y se actualiza en las 12 cartas del tablero. Sin escenas reutilizables tendria que repetir ese codigo 12 veces.

Uso de senales

Senal 1, personalizada. Senal usada: card_flipped(card_instance), declarada en carta.gd. Nodo emisor a nodo receptor: cada instancia de carta.tscn, un TextureButton, hacia main.gd. Que hace al activarse: cuando el jugador presiona una carta boca abajo, esta se voltea con flip() y emite card_flipped. main.gd esta escuchando esa senal, card.card_flipped.connect(_on_card_flipped), y en _on_card_flipped() guarda la primera y segunda carta seleccionadas y llama a check_match() para comparar sus id.

Senal 2, nativa de Godot. Senal usada: pressed, del nodo Button dentro de PantallaVictoria. Nodo emisor a nodo receptor: Button hacia main.gd. Que hace al activarse: al presionar el boton de la pantalla de victoria se llama _on_button_pressed(), que recarga la escena actual con get_tree().reload_current_scene() para reiniciar el juego por completo.
<img width="718" height="1351" alt="image" src="https://github.com/user-attachments/assets/5944ca52-d15c-40fa-ad9b-5698c779285d" />
<img width="1315" height="1103" alt="image" src="https://github.com/user-attachments/assets/5624ca5f-14ba-4a1f-9152-ece1f2ef9c82" />


Dificultad principal

Completa con tu experiencia real. Por ejemplo: evitar que el jugador pudiera voltear una tercera carta mientras ya habia dos boca arriba esperando comparacion. Esto se resolvio con la variable input_locked en main.gd.

Animacion de volteo de cartas usando Tween, escala en X a 0 y de regreso a 1, cambiando la textura a la mitad del movimiento, en lugar de un cambio instantaneo de imagen. Pantalla de victoria, PantallaVictoria, con ColorRect, Label y Button para reiniciar la partida sin recargar la pagina. Efecto visual al encontrar un par: la carta se atenua con modulate y se encoge levemente con scale para indicar que ya fue resuelta. Uso de Timer para dar un pequeno margen de tiempo antes de voltear de nuevo las cartas que no coincidieron, en vez de ocultarlas al instante.

Tiempo invertido
Aproximandamente fueron 6-7 horas dividas en la semana
