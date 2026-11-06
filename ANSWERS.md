# Preguntas de checkpoint

1. **Prop `visible` de `Modal` y por qué se controla con `useState`.** `visible` indica si el modal se muestra (`true`) o está oculto (`false`). Como el modal debe abrirse y cerrarse según lo que haga el usuario, ese valor tiene que vivir en el estado (`modalVisible`): al llamar a `setModalVisible`, React vuelve a renderizar y el modal aparece o desaparece. Una variable normal no provocaría el nuevo render.

2. **`onRequestClose`.** Es la función que se ejecuta cuando el sistema pide cerrar el modal, sobre todo con el botón *atrás* de Android (y en iOS con el gesto de cerrar en algunas presentaciones). Si no cierro el modal dentro de ella (`setModalVisible(false)`), en Android el botón atrás no hace nada y el usuario queda atrapado en el modal; por eso en el paso 2 la dejé apuntando a `closeModal`.

3. **`transparent={true}` y valores de `animationType`.** Con `transparent={true}` el fondo del modal no es blanco opaco, así que se ve la pantalla de abajo y puedo dibujar un *overlay* oscuro semitransparente con una tarjeta encima. Los tres valores de `animationType` son `'none'` (aparece sin animación), `'slide'` (entra desde abajo) y `'fade'` (aparece con desvanecimiento).

4. **Para qué existe `ScrollView`.** Una pantalla no hace scroll sola: si un `View` es más alto que la pantalla, el contenido que no cabe simplemente queda cortado e inaccesible. `ScrollView` agrega el desplazamiento vertical (o horizontal con `horizontal`). Elegiría `FlatList` cuando la lista es larga o de largo desconocido (cientos de filas, datos de una API), porque `ScrollView` renderiza todos los hijos de una vez, mientras que `FlatList` solo renderiza los elementos visibles.

5. **`style` vs `contentContainerStyle`.** `style` da el estilo al contenedor del `ScrollView` en sí, es decir, al "marco" o ventana visible (tamaño, `maxHeight`, fondo, bordes). `contentContainerStyle` da el estilo al contenedor interno que envuelve a los hijos y que es el que se desplaza (padding, `gap`, `flexDirection`, `justifyContent`). Por ejemplo, en la lista de proyectos usé `contentContainerStyle={styles.projectsRow}` para el `gap` entre tarjetas, y en la lista de contactos usé `style={styles.contactList}` para limitar la altura con `maxHeight`.

6. **`SafeAreaView` y `react-native-safe-area-context`.** `SafeAreaView` agrega el espaciado necesario para que el contenido no quede tapado por las zonas del dispositivo que no son aptas para dibujar: el *notch* o cámara frontal, la barra de estado, la barra de gestos o botones de navegación inferior y las esquinas redondeadas. Usamos `react-native-safe-area-context` porque el `SafeAreaView` incluido en `react-native` solo funciona en iOS, mientras que esta librería funciona en iOS, Android y web, y además entrega los márgenes reales con `useSafeAreaInsets`.

7. **Buenas prácticas del paso 5 (con mis palabras).**
   - *Un solo modal, para una acción corta y puntual:* en la pantalla hay un único modal, el de compartir el perfil, que se usa para elegir contactos y confirmar, y no para mostrar información larga.
   - *Contenido limitado dentro de los `ScrollView`:* las únicas listas son `PROJECTS` (5 elementos) y `CONTACTS` (8), por lo que renderizar todo de una vez no afecta el rendimiento; con listas largas habría que usar `FlatList`.
   - *Toda la pantalla dentro de `SafeAreaView`:* así nada queda bajo el notch ni las barras del sistema.

   Un modal **no** es buena idea para mostrar contenido largo o un flujo con varios pasos (por ejemplo, un formulario de registro completo), ni para mensajes que no requieren que el usuario detenga lo que hace (mejor un aviso breve o un *toast*), porque el modal bloquea el resto de la pantalla.
