# Video 1 — Full Transcript (Spanish)

Hola a todos. Hoy vamos a trabajar en el desarrollo de un bot de trading para el trading algorítmico.
Comenzaremos desde cero, empezando por definir la tarea en sí. En cuanto a dónde terminará exactamente este video en particular, no estoy seguro todavía.

Para esta investigación, me he fijado un plazo tentativo de tres semanas.
Cualquier cosa que logre completar dentro de ese marco de tiempo se incluirá en el video.
Después de eso, pasaré una semana adicional resumiendo los resultados, refinando los puntos clave, etcétera.

Si eres nuevo en el canal y no estás seguro de qué trata, te recomiendo consultar este enlace.

Para esta lista de reproducción, he creado un repositorio en GitHub, donde podrás encontrar materiales para cada video en carpetas separadas.
A veces serán los puntos principales, y otras veces habrá código adjunto, por ejemplo.

Entonces, hemos decidido crear un bot. ¿Por dónde empezamos?

Dado que tengo experiencia en ingeniería y científica, esto ha moldeado mi metodología y mi forma de pensar.
Creo que el primer paso debería ser una comprensión completa del objeto de estudio.
Por supuesto, he estudiado esto antes; no es algo que puedas captar en cinco minutos o incluso en cinco días, y construiré sobre el conocimiento que ya tengo.

Voy a plantear las preguntas que deben responderse antes de que podamos avanzar, y las responderé de inmediato.

Aquí está la primera pregunta: **¿En qué mercado estoy interesado?**

Podría ser criptomonedas, acciones, mercado Forex (mercado de divisas), etcétera. Esta es puramente una opinión subjetiva basada en mi propia investigación.
Para mí, el mercado de valores, especialmente cuando se limita a una región específica, es relativamente bajo en volatilidad.
Generalmente lo veo como un vehículo de inversión a largo plazo, así que, por ahora, lo descarto.

Las criptomonedas son demasiado volátiles y están fuertemente impulsadas por las noticias. Dada la inestabilidad global actual, personalmente me resulta difícil pronosticar sus movimientos.
Si compras Bitcoin ahora y lo mantienes durante los próximos 10 años, ¿por qué no?
Pero cuando se trata de especulación, pasaré por ahora.

Estoy más inclinado hacia el mercado Forex: el comercio de divisas. El mercado Forex es un mercado de valores relativos.
Una divisa no es un activo por sí misma, sino una relación entre dos economías. Esto crea ciclicidad y fases de tendencia.
En otras palabras, el mercado no es completamente caótico; está estructurado por balances macroeconómicos.
Además, en comparación con las criptomonedas, la volatilidad aquí está más acotada estructuralmente, aunque los grandes eventos macroeconómicos pueden generar picos extremos temporalmente.
El apalancamiento está disponible, lo cual me viene bien.

Basándonos en esta elección, veamos las características que debemos considerar para que el bot opere de manera efectiva.
Esto es importante, no necesariamente para encontrar una estrategia rentable de inmediato, sino para entender cómo funciona el sistema y qué suposiciones influyen en su comportamiento.

Entonces la pregunta se convierte en: **¿Qué características son clave para este tipo de mercado?**

Esta es una de las preguntas más fundamentales en el trading.
Cuando no nos vinculamos a un mercado específico, nos vemos obligados a buscar características universales que determinen qué estrategia, indicador o mecanismo realmente tiene sentido aplicar.
Esta tarea es extremadamente compleja, especialmente en la etapa inicial.

Para el mercado Forex, existe de hecho un conjunto de características que influyen fuertemente en qué estrategias, indicadores y filtros funcionarán, y cuáles fallarán rápidamente.
He identificado las más importantes para analizar al construir un bot.
Pueden dividirse en características constantes (estructurales) y características dinámicas (aquellas que cambian día a día y deben ser monitoreadas por el bot en tiempo real).

Toquemos brevemente el análisis a nivel de sistema del entorno.
Es importante aclarar una cosa: no estamos estudiando el mercado solo por el hecho de operar.
Estamos estudiando el proceso de diseño de un sistema en un entorno no estacionario.
El mercado Forex es simplemente un laboratorio conveniente: suficientemente complejo, dinámico y sensible a cambios de régimen.
Si un método demuestra ser robusto aquí, puede potencialmente transferirse a otros dominios también.

Antes de hablar sobre estrategias, es importante dar un paso atrás.
En cualquier sistema complejo, primero analizas el entorno, y solo entonces el comportamiento dentro de él.
No estamos buscando un punto de entrada en este momento; estamos construyendo un mapa del entorno.

Comencemos con las características estructurales, las que siempre deben considerarse durante la fase de diseño.

**Primero**, el bot opera las 24 horas del día, cinco días a la semana, sin horarios fijos de apertura y cierre como en las bolsas de valores.
Los datos fluyen continuamente, pero la actividad varía significativamente dependiendo de la hora del día.
Por lo tanto, el bot debe filtrar por sesión de trading: la sesión asiática suele ser tranquila, mientras que la sesión de Londres y la sesión de Nueva York son las más activas, especialmente durante su solapamiento.
También debemos tener en cuenta el swap (interés overnight), mantener posiciones durante la noche, ya que conlleva sus propios detalles y sorpresas.

**Segundo**, el mercado tiene una liquidez enorme, una de las más altas en las finanzas globales, alrededor de 10 billones de dólares en volumen diario.
En los pares principales como el euro frente al dólar, el deslizamiento es mínimo y los spreads son estrechos, lo que incluso permite un scalping agresivo.
Sin embargo, en los pares cruzados, y especialmente en los pares exóticos, la liquidez es menor, el deslizamiento se vuelve más notable y se requiere precaución.

**Tercero**, el mercado es descentralizado; no existe una bolsa central única.
Las cotizaciones pueden diferir ligeramente entre brokers, los spreads varían y a veces ocurren recotizaciones.
Eso significa que presionas el botón de "comprar", pero el broker no ejecuta la orden de inmediato; en su lugar, recibes una notificación de que el precio ha cambiado y te ofrecen uno nuevo.

Podría surgir una pregunta lógica: ¿por qué me importa esto?
Opero con un broker y todas mis ejecuciones se basan en sus datos de todos modos.
Si estuviéramos haciendo arbitraje, entonces sí, las discrepancias serían críticas.
De hecho, las diferencias en las cotizaciones no afectan directamente nuestras operaciones.
Abrimos, mantenemos y cerramos posiciones dentro de un ecosistema, a los precios proporcionados por nuestro broker.
Sin embargo, hay matices.

- El broker en sí es esencialmente un mini-mercado. Dado que no hay una bolsa central, cada broker forma sus propias cotizaciones basándose en sus fuentes de liquidez: grandes bancos, otros brokers, agregadores.
Un buen broker agrega cotizaciones de una gran cantidad de fuentes (a veces 20 o más), lo que acerca el precio al valor de mercado "real", mantiene los spreads estrechos y minimiza el deslizamiento.
Algunos brokers pueden mostrar precios ligeramente desplazados a su favor, o durante alta volatilidad decidir si ejecutan una orden al precio solicitado.
- Impacto en la ejecución de órdenes. Cuando colocamos una orden de mercado o un stop-loss, el broker intenta ejecutarlo a la cotización actual.
Pero si el mercado se mueve bruscamente, por ejemplo, debido a noticias, el broker puede aplicar deslizamiento, emitir una recotización, ejecutar la orden parcialmente o incluso rechazarla.
Esto sucede porque la sincronización de las cotizaciones no es perfecta, no necesariamente porque alguien esté tratando de engañarnos.
Cuanto más delgado sea el pool de liquidez, peor puede ser este efecto.
- Backtesting (prueba histórica) frente a la realidad. Cuando desarrollamos un bot y lo probamos con datos históricos, esos datos generalmente provienen de un feed agregado: un flujo de datos único compilado y procesado de múltiples fuentes internas o externas, luego entregado como un flujo unificado.
En realidad, nuestro broker puede proporcionar máximos y mínimos, spreads e incluso datos tick ligeramente diferentes.
Si una estrategia muestra una tasa de éxito del 70% en el backtesting, puede disminuir en una cuenta real debido a estas micro-diferencias.
Idealmente, deberíamos obtener datos tick directamente de nuestro propio broker en lugar de usar un feed genérico, para que la simulación sea lo más cercana posible a la realidad.
- Influencia indirecta de otros brokers. Si nuestro broker es un creador de mercado y ve que muchos clientes están abriendo posiciones en la misma dirección, puede "ajustar" las cotizaciones o ampliar el spread para equilibrar su libro.
Esto es parte del panorama general del mercado. Durante publicaciones de noticias o condiciones de mercado delgadas, como la sesión asiática o el rollover, las diferencias entre brokers pueden volverse significativas.
Un broker podría tener un spread de 1 pip, mientras que otro tiene 15. El bot puede asumir que todo está tranquilo, mientras que en realidad ya estamos enfrentando un deslizamiento sustancial.

En la práctica, si usamos un broker bien regulado con una sólida reputación, estos problemas se minimizan y son casi imperceptibles bajo condiciones normales de mercado.
Sin embargo, en muchos casos, como el mío, estoy limitado a solo cuatro o cinco brokers dentro de mi país, especialmente si quiero evitar complicaciones con transferencias de fondos, legalidad, etcétera.

No pretendo analizar y compararlos profundamente con los gigantes de la industria.
Eso requeriría un esfuerzo significativo y, al final, podría solo generar una ventaja potencial de 10 pips, y ni siquiera eso está garantizado.
¿Resultará útil esta información al diseñar el bot? No estoy seguro todavía.
Pero vale la pena tenerlo en cuenta; sospecho que puede volverse relevante en algún momento en el futuro.

**Cuarto**, el apalancamiento es alto aquí, típicamente hasta 1:30 con brokers regulados y hasta 1:500 con los offshore.
Incluso pequeños movimientos de precios pueden convertirse en enormes ganancias o pérdidas en la cuenta.
Eso significa que una estricta gestión de riesgo es absolutamente esencial. El riesgo por operación generalmente se mantiene dentro del 0.5–2%;
de lo contrario, un fuerte impulso de noticias, y hola, llamada de margen.
Por supuesto, existen modelos de riesgo más avanzados, pero comenzaremos con algo más simple y robusto.

**Quinto**, los costos de transacción son muy bajos. Los componentes principales son el spread y el swap (interés overnight);
las comisiones son mínimas o inexistentes en muchos casos. Debido a esto, las estrategias con una gran cantidad de operaciones por día se vuelven viables, especialmente en pares con spreads estrechos.
Esto abre la puerta al scalping e incluso a enfoques de alta frecuencia.

**Sexto**, el mercado Forex está fuertemente ligado a la macroeconomía y a las noticias, aunque no en la misma medida que las criptomonedas.
Los principales lanzamientos económicos pueden desencadenar impulsos bruscos, picos y, a veces, incluso gaps.
La volatilidad durante estos momentos puede aumentar de 3 a 10 veces.
Por lo tanto, casi cualquier bot serio debería incluir un filtro de noticias incorporado: de 5 a 30 minutos antes y después de los lanzamientos importantes, el trading debería pausarse o las reglas de entrada deberían endurecerse significativamente.
De lo contrario, el sistema puede caer repetidamente en falsas rupturas.

Estos seis factores forman la base estructural del mercado Forex; no cambian fundamentalmente de año en año.
Si ignoramos esta base al diseñar el bot, el sistema operará aleatoriamente en lugar de sistemáticamente.
Preliminarmente, podemos decir que los filtros de sesión, los filtros de noticias, el control de riesgo estricto y las comprobaciones de liquidez deben incorporarse primero, y solo después de eso deberíamos superponer indicadores u otros componentes de estrategia.

Ahora pasemos a las características dinámicas. Estos son los factores que cambian cada día, cada hora o incluso cada pocos minutos.
Crean el contexto del mercado. El bot debe ser capaz de detectarlos en tiempo real, analizarlos y, basándose en eso, decidir: ¿debería operar ahora o es mejor esperar?
¿Debería usar un indicador de seguimiento de tendencia o cambiar a un enfoque diferente? ¿Endurecer los stop-loss o aflojarlos?

Esto es esencialmente el pulso del mercado. Si el bot lo ignora, seguirá ciegamente las señales de los indicadores incluso cuando las condiciones ya no sean adecuadas.

**Primero y más importante**: el régimen de mercado actual: tendencia, rango, mercado lateral con muchas falsas rupturas (el llamado chop), o alguna fase de transición.
Hay muchas maneras de medir esto, por ejemplo, usando el indicador ADX, las Bandas de Bollinger o un simple análisis estructural.
Pero por ahora, los detalles no importan; en esta etapa, estamos formalizando las tareas.
El régimen puede cambiar en cuestión de horas o incluso minutos debido al comportamiento de los participantes, las noticias o el desequilibrio acumulado.
Una estrategia que funciona en un régimen suele ser poco rentable o aleatoria en otro.
Sin la detección de régimen de mercado, el sistema opera a ciegas.

**Segundo**: volatilidad actual. Esto refleja la magnitud y velocidad de las fluctuaciones de precios en relación con el pasado reciente.
Se dispara durante noticias e impulsos fuertes y cae durante períodos de baja actividad.
La volatilidad afecta el tamaño esperado del movimiento, la confiabilidad del stop-loss, la frecuencia de las falsas rupturas e incluso el tipo de enfoque táctico que tiene sentido.

**Tercero**: actividad de la sesión y ciclicidad intradía. Dependiendo de la hora del día y los solapamientos de las sesiones (Asia, Londres, Nueva York), la intensidad de la negociación varía.
Cada día sigue un ciclo repetitivo con picos y valles predecibles en la actividad.
La mayoría de las señales falsas y los períodos de pérdidas ocurren durante las ventanas de baja actividad.
Muchos de los movimientos más fuertes se observan a menudo durante los solapamientos de sesiones, aunque esto es una tendencia empírica más que una ley estructural y debe validarse estadísticamente.

**Cuarto**: la fuerza del movimiento en sí. ¿Qué tan seguro y sostenible es?
¿El precio continúa en la misma dirección, o se está agotando y preparando para revertirse?
Esto cambia en tiempo real; el momentum puede construirse, debilitarse o desaparecer por completo.
Cuando el movimiento es débil, las señales de continuación se convierten rápidamente en trampas.

**Quinto**: correlaciones entre pares. ¿Se están moviendo juntos en este momento o divergiendo? Esto no es fijo.
A veces la mayoría de los pares se mueven en sincronía siguiendo al dólar; otras veces divergen en direcciones diferentes.
La alta sincronización aumenta el riesgo de duplicar esencialmente la misma posición varias veces.
La divergencia puede crear oportunidades de cobertura o señalar que algo inusual está sucediendo.

**Sexto**: proximidad de noticias y eventos. El calendario cambia cada día: decisiones de tasas de interés, datos de inflación, informes de empleo, discursos de bancos centrales, geopolítica.
Cuando tales eventos están cerca, el mercado puede explotar en cualquier segundo, y los patrones familiares pueden simplemente dejar de funcionar.
Ignorar esto significa exponerse a las pérdidas más frustrantes.

**Séptimo y finalmente**: dinámica de carry trade. Es decir, cómo los diferenciales de tasas de interés afectan el costo de mantener una posición.
Las tasas de los bancos centrales cambian, las expectativas del mercado se desplazan y los swap (interés overnight) se ajustan diariamente.
Para operaciones a corto plazo esto es casi insignificante, pero si las posiciones se mantienen durante días o semanas, el swap puede convertirse en un bono agradable o en un gasto silencioso que lentamente se come las ganancias.

Todos estos factores son dinámicos porque dependen de lo que está sucediendo en este momento: comportamiento del trader, desencadenantes externos, hora del día, sentimiento.
No están escritos en las reglas fijas del mercado.
Son críticos porque crean el contexto real: las condiciones bajo las cuales una estrategia tiene una ventaja estadística, y aquellas en las que se vuelve aleatoria o dañina.
Sin un monitoreo continuo de estos parámetros, el bot no puede adaptarse; simplemente ejecuta operaciones mecánicamente mientras el mercado ya ha cambiado a un estado diferente.
Pero cuando el bot detecta estos cambios y filtra las condiciones en consecuencia, deja de ser una máquina tonta y comienza a entender al menos parcialmente lo que está sucediendo.

Y una vez que hemos revisado las preguntas generales, surge una más fundamental: 

**¿Estoy construyendo un sistema que simplemente gana dinero, o un sistema que se adapta a los regímenes de mercado cambiantes?**

Estas son arquitecturas diferentes. Y esta pregunta va más allá del trading. Se aplica a cualquier modelo que opere en un entorno cambiante.
Sistema estático frente a arquitectura de sistema adaptativo: esta es una bifurcación fundamental en el camino.

Veamos un sistema diseñado puramente para generar ganancias. El camino clásico.
La lógica es sencilla: encontrar un patrón, formalizarlo, optimizar los parámetros, lograr una esperanza matemática positiva y controlar el riesgo.
En este caso, el objetivo principal es la esperanza matemática positiva en datos históricos con un drawdown (reducción máxima de capital) aceptable.

El problema es que los mercados cambian. Lo que solía funcionar eventualmente deja de funcionar.
En ese punto, el sistema continúa operando y muere lentamente, o se desconecta manualmente.
Esta arquitectura de sistema estático puede ser rentable, a veces incluso muy rentable. Pero no sabe cuándo se ha vuelto irrelevante.

Con un sistema que se adapta a los regímenes de mercado, la situación es diferente.
Aquí, el objetivo no es solo ganar dinero, sino determinar: ¿En qué régimen se encuentra actualmente el mercado?
¿Mi estrategia se ajusta a este régimen? ¿Deberían ajustarse los parámetros? ¿Debería desactivarse la estrategia?
¿O debería cambiar a otra?

En este caso, la estrategia se vuelve de dos capas: la estrategia de trading en sí y un meta-sistema que incorpora la detección de régimen de mercado, la adaptación y la lógica de cambio.
En estos dos enfoques, estamos haciendo dos preguntas fundamentalmente diferentes.
En el primer caso: "¿Dónde entro y dónde salgo?"
En el segundo: "¿Cuándo es aplicable mi modelo en absoluto?"

En un entorno no estacionario, una arquitectura que evalúa continuamente la aplicabilidad de su propio modelo es estructuralmente más robusta que una que asume estabilidad por defecto.
Personalmente, busco arquitectura, pensamiento sistémico, robustez y la capacidad de lidiar con la incertidumbre, no ajuste de curvas (curve fitting), optimización de parámetros o debates sobre la tasa de éxito.
Eso es ruido.

Desde la perspectiva de la teoría de la ingeniería, esto nos lleva al dominio del análisis de régimen, distribuciones, estabilidad, degradación del modelo y conceptos relacionados.
Eso es realmente de lo que trata este proyecto. Por supuesto, un sistema adaptativo es mucho más complejo.
Necesito definir los regímenes en sí mismos, detectar cambios de régimen, establecer criterios para desactivar o cambiar estrategias y evitar el sobreajuste a esos regímenes.
En esencia, esto se convierte en un proyecto de investigación.

Pero debemos entender que un sistema totalmente adaptativo es imposible. Cualquier adaptación también se basa en suposiciones.
La pregunta no es sobre construir un modelo perfecto; es sobre qué tan rápido puede el sistema reconocer que ya no entiende el mercado.

Al mismo tiempo, la investigación requiere un objeto de referencia simple desde el cual comenzar.
Así que primero, nos fijaremos la tarea de construir un sistema simple que gane dinero, incluso un poco.
Observaremos sus limitaciones, su degradación y dónde se rompe.
Solo después de eso decidiremos si podemos diseñar un sistema capaz de superar esas debilidades.

También hay un peligro sutil aquí. Cuando analizamos el entorno demasiado profundamente, podemos construir un marco de protección casi perfecto, pero terminar con un componente de señal excesivamente estrecho.
Como resultado, el bot abrirá operaciones muy raramente.
Este es un sesgo de ingeniería clásico: la máxima seguridad conduce a una actividad mínima, lo que a su vez conduce a una significancia estadística insuficiente.

En el siguiente video, demostraré un ejemplo claro de tal proyecto.

Ahora hemos cubierto las características clave. Eso nos permite plantear el siguiente conjunto de preguntas.

**¿Qué tipo de trading estamos considerando?** 

Nos centraremos en el trading a medio plazo: posiciones mantenidas desde varias horas hasta varios días.
Este formato se alinea bien con las características estructurales del mercado Forex.
Nos permite trabajar con la ciclicidad y el comportamiento de régimen sin caer en una dependencia extrema de la micro-ejecución, como en el HFT o el scalping agresivo, donde los datos tick, la latencia y los spreads fraccionarios se vuelven críticos.

Al mismo tiempo, el trading a medio plazo mantiene una frecuencia de operaciones suficiente para la relevancia estadística, evitando al mismo tiempo un cambio hacia la inversión a largo plazo, donde los cambios fundamentales y los ciclos macro dominan.
Este horizonte temporal ofrece un equilibrio entre el control de costos, el riesgo manejable y la capacidad de explotar la volatilidad durante las sesiones activas sin volverse excesivamente sensible al ruido del mercado.

Como se mencionó anteriormente, el deslizamiento es mínimo en los pares principales, pero en los pares cruzados y exóticos, se vuelve notable.
Esto naturalmente conduce a otra pregunta.

**¿En qué par de divisas estamos interesados?**

Principalmente, nos centraremos en pares principales como EUR/USD, GBP/USD, USD/JPY y posiblemente AUD/USD y USD/CAD.
Estos pares ofrecen la mayor liquidez, spreads estrechos y relativamente estables, deslizamiento mínimo y reacciones más predecibles a los factores macroeconómicos.

Para el trading a medio plazo, esto es especialmente importante: reducimos el impacto de los problemas de micro-ejecución, reducimos los costos de transacción y operamos dentro de una estructura de mercado más limpia, sin las anomalías agudas a menudo vistas en pares exóticos.
En esta etapa, tiene sentido excluir los pares cruzados y los pares exóticos para evitar una complejidad innecesaria y fuentes adicionales de inestabilidad en el modelo.

**¿En qué sesión de trading debería operar el bot? ¿Cuándo debería estar activo?**

Para un modelo de medio plazo, tiene sentido activar el bot durante la sesión de Londres y la sesión de Nueva York, especialmente durante su solapamiento, cuando la liquidez está en su pico, los spreads son más estables y los movimientos de precios tienden a ser más estructurados.
Empíricamente, muchos impulsos significativos tienden a formarse durante estas horas, y la dirección diaria a menudo se establece allí, aunque esta tendencia debe ser verificada estadísticamente dentro de nuestro propio conjunto de datos.

En la etapa inicial, la sesión asiática puede lógicamente ser excluida por completo o utilizada solo para gestionar posiciones ya abiertas.
Esta sesión a menudo se caracteriza por una volatilidad comprimida y una mayor frecuencia de movimientos falsos.
Tal enfoque nos permite centrarnos en períodos donde la probabilidad de un movimiento significativo es mayor y el ruido relativo del mercado es menor en comparación con las fases activas.

**¿Debería el bot tratar las noticias como un filtro estricto?**

Sí. En la etapa de diseño de un bot de medio plazo, las noticias deben implementarse como un filtro estricto en lugar de como un indicador adicional.
En un mercado sensible a la macroeconomía, un solo lanzamiento puede alterar completamente el régimen, la volatilidad y la estructura de precios en cuestión de minutos.
Si el bot ignora este factor, puede ingresar al mercado en un momento de máxima incertidumbre.

**¿Cómo contabilizamos las noticias?**

En la etapa inicial, las noticias pueden incorporarse de la siguiente manera:
1. Un filtro de importancia: considerando solo eventos de alto impacto como tasas de interés, inflación o discursos de bancos centrales.
2. Una ventana de bloqueo basada en el tiempo: prohibiendo nuevas posiciones de 15 a 30 minutos antes y después del lanzamiento.
3. Una gestión de posición más estricta: si una posición ya está abierta, reducimos temporalmente el riesgo endureciendo el stop-loss o asegurando parcialmente las ganancias.
4. Un filtro de estabilización de la volatilidad: esperando hasta que los spreads se normalicen y la volatilidad regrese a su rango típico después del lanzamiento.

De esta manera, las noticias se convierten en una herramienta de gestión de riesgo. En esta etapa, nuestro objetivo no es capturar movimientos impulsados por noticias, sino evitar entradas destructivas durante la reestructuración caótica del mercado.

**¿Debería el bot determinar el régimen de mercado?**

De nuevo, sí. El bot debe detectar el régimen de mercado en tiempo real.
Para una estrategia de medio plazo, es crítico entender si el mercado está en tendencia, en rango o atrapado en un chop caótico, ya que la efectividad de la señal y el enfoque táctico apropiado dependen fuertemente del régimen.

Sin tal reconocimiento, el bot seguirá mecánicamente los indicadores incluso cuando el mercado no proporcione condiciones para un movimiento sostenido, lo que lleva a una gran cantidad de operaciones falsas.
La detección de régimen de mercado nos permite ajustar el tamaño de posición, los niveles de stop-loss y los take-profit a la estructura de mercado actual, aumentando la probabilidad de que la estrategia esté operando donde su ventaja estadística realmente existe.

**¿Cómo debemos contabilizar la volatilidad actual?**

La volatilidad actual debe tratarse como un parámetro dinámico clave que influye en cada aspecto de una posición.
Para un bot de medio plazo, esto puede implementarse de la siguiente manera: Usamos la medición de volatilidad, como el indicador ATR o la desviación estándar, para estimar el rango de movimiento actual.
Usamos niveles de stop-loss y take-profit adaptativos: cuanto mayor sea la volatilidad, más amplios deben ser los stop-loss para evitar ser sacudidos por fluctuaciones aleatorias.
Ajustamos el tamaño de posición: cuando aumenta la volatilidad, es prudente reducir el tamaño de la operación para mantener el riesgo bajo control.
Y finalmente, el filtrado de señales: durante una volatilidad extrema, las señales pueden ser ignoradas o activarse criterios de entrada más estrictos.

La volatilidad debe convertirse en una medida contextual de riesgo y calidad de la señal, no solo en un número estadístico.
El bot debe ajustar la estrategia al estado actual del mercado para preservar su ventaja estadística y minimizar las pérdidas aleatorias.
Estos enfoques no son un dogma; deben ser probados y calibrados.

**¿Debería limitarse el trading bajo condiciones anormales?**

Sí. Limitar el trading durante condiciones anormales es extremadamente importante. En el mercado Forex, las situaciones anormales son aquellas en las que la estructura normal del mercado se rompe: picos de volatilidad agudos, spreads extremos, gaps inesperados o fallas técnicas en el broker.
Durante estos momentos, las señales estándar casi siempre se vuelven falsas, el riesgo de los drawdowns (reducción máxima de capital) aumenta drásticamente y la ventaja estadística de la estrategia desaparece temporalmente.

**¿Existe el concepto de un estado de mercado anormal?**

El bot debe tener la noción de un "estado anormal" y actuar en consecuencia: ya sea absteniéndose de operar o reduciendo el riesgo.
Esto podría significar bloquear completamente las nuevas posiciones, reducir el tamaño de las operaciones, endurecer los stop-loss o cerrar parcialmente las posiciones existentes si es necesario.
Tales medidas hacen que el sistema sea más resistente y protegen el capital, lo cual es especialmente importante en el trading a medio plazo, donde las posiciones se mantienen durante horas o días y son sensibles a impulsos repentinos.

**¿Debería el bot operar todo el tiempo o solo cuando las condiciones del mercado se alinean?**

El bot no debería operar continuamente. Su propósito es operar solo cuando las condiciones ambientales clave se alinean y la probabilidad de una ventaja estadística es mayor.
En otras palabras, cada operación debe pasar por un conjunto de filtros: régimen de mercado, sesión, volatilidad, noticias, liquidez y riesgo por operación.
Sin estas restricciones, el bot "empujaría" al mercado durante períodos caóticos, convirtiendo una ventaja estadística en aleatoriedad.

**¿Cuántos filtros son aceptables sin matar la actividad?**

Debe haber suficientes filtros para proteger el capital, pero no tantos que las operaciones se vuelvan demasiado raras y se pierda la significancia estadística.
En la práctica, esto significa de cuatro a seis filtros principales en la etapa inicial: sesión, noticias, régimen de mercado, volatilidad, liquidez y gestión de riesgo.
Se pueden introducir filtros adicionales gradualmente después de las pruebas, evaluando su efecto en la frecuencia de las operaciones y el equilibrio entre la seguridad y una actividad suficiente.

También es importante reconocer que algunos de estos filtros pueden ser interdependientes.
Por ejemplo, la volatilidad afecta la detección de régimen de mercado, las noticias influyen en la volatilidad y la estructura de la sesión impacta la liquidez.
Más adelante, necesitaremos distinguir entre variables de estado primarias y condiciones derivadas para evitar una lógica redundante o circular dentro de la arquitectura.

**¿Cuál es el contexto mínimo requerido para permitir una operación?**

El contexto mínimo para permitir una operación es la alineación de las condiciones ambientales clave que impactan críticamente la ventaja estadística del bot.
Para un bot de medio plazo, esto significa:
1. La sesión de trading está activa.
2. El régimen de mercado coincide con la táctica elegida.
3. La volatilidad está dentro de un rango aceptable.
4. No hay noticias de alto impacto inminentes y el mercado no está en un estado anormal.
5. La liquidez es suficiente y los spreads no son anormalmente amplios.

Solo cuando todas estas condiciones se cumplen simultáneamente se permite una operación.

**¿Será el sistema adaptativo o estático?**

El sistema será un sistema adaptativo. El bot evalúa continuamente los valores actuales de estos parámetros y ajusta sus acciones en tiempo real: tamaño de posición, stop-loss, take-profit e incluso la decisión de entrar en una operación.
Esto le permite responder a los cambios del mercado y mantener su ventaja estadística, en lugar de seguir mecánicamente las señales sin tener en cuenta el contexto actual.

Ahora, habiendo descrito el espacio de trading permisible, podemos hablar sobre encontrar una ventaja estadística dentro de ese espacio: el punto clave.
Sin este marco, una ventaja estadística se convierte en una abstracción. He mencionado este término varias veces antes, pero ahora podemos expandirlo.

**Una ventaja estadística es una ventaja de trading repetible y estadísticamente verificada en la que el rendimiento esperado de una sola operación es positivo incluso después de contabilizar todos los costos: spread, swap (interés overnight) y riesgos.**
La construimos a través de un análisis cuidadoso de los patrones del mercado, pruebas rigurosas, filtrando señales falsas y la verificación continua de que la ventaja persiste bajo condiciones reales.

En esta etapa, todo lo que podemos hacer es formular hipótesis, quizás varias, que serán refinadas o descartadas en cada paso de una investigación posterior.
Solo después de usar estas hipótesis y el marco que hemos construido para definir una ventaja estadística final, podemos hablar sobre una estrategia de trabajo.

Primero, necesitamos entender la base para formular hipótesis de ventaja estadística.
Surgen de una combinación de tres componentes: características constantes (las propiedades estructurales del mercado), características dinámicas (parámetros en tiempo real como régimen y volatilidad) y una simple verificación empírica (la búsqueda de tendencias estadísticas recurrentes).
Una ventaja estadística no es solo una idea; es un sesgo estadístico recurrente que puede formalizarse potencialmente.

¿Deberíamos seleccionar hipótesis de ventaja estadística ahora? En esta etapa, es mejor no fijarse en unas específicas.
En su lugar, describiremos tres direcciones de pensamiento que servirán más adelante como base para hipótesis de ventaja estadística reales y comprobables.

**Primero**: Uso del contexto de la sesión y el momentum. Como discutimos, el mercado Forex está dividido estructuralmente por sesiones de trading.
Londres y Nueva York generan los principales volúmenes e impulsos.
Esto no es aleatorio; es cuando los grandes participantes, bancos y fondos de cobertura entran al mercado.

Hipótesis: Si un impulso direccional se forma al comienzo de una sesión activa durante alta actividad, hay una mayor probabilidad de que el movimiento continúe dentro de la misma fase.
La ventaja estadística aquí se deriva de la estructura conductual al inicio de la sesión.

**Segundo**: Reacción del mercado a las desviaciones de la media. El mercado fluctúa constantemente alrededor de un punto de equilibrio entre oferta y demanda.

Hipótesis: Cuando el precio se desvía significativamente de su equilibrio reciente (más de lo típico para la volatilidad actual), la probabilidad de regresar hacia el equilibrio aumenta.
Cuando el movimiento excede la amplitud normal, los participantes toman ganancias, los nuevos jugadores dudan en entrar y la liquidez se agota.
La ventaja estadística aquí proviene de la reacción asimétrica del mercado a los extremos.

En este punto, debemos reconocer una tensión conceptual: la primera hipótesis asume la continuación del momentum, mientras que la segunda asume la reversión a la media.
Estos son comportamientos estructuralmente opuestos. Esto hace que la detección de régimen de mercado sea un nodo arquitectónico crítico de todo el sistema.
Sin identificar correctamente cuándo domina la continuación y cuándo prevalece la reversión, el sistema intentaría aplicar lógicas mutuamente incompatibles simultáneamente.

**Tercero**: Influencia de los niveles de liquidez clave y puntos estructurales. El mercado Forex es un mercado donde una gran cantidad de órdenes stop se agrupan alrededor de máximos y mínimos obvios.
Estas zonas son pools de liquidez. 

Hipótesis: El precio tiende a acelerarse después de capturar liquidez (una ruptura) o revertirse bruscamente después de que se eliminan los stop-loss (una falsa ruptura).
Esto funciona porque los grandes participantes necesitan liquidez para entrar en posiciones. Los stop-loss de la multitud actúan como contra-órdenes listas.
La ventaja estadística potencial se basa en el comportamiento del precio cerca de los extremos estructurales, no simplemente en el hecho de que se rompió un nivel.

Estas ideas no son nuevas; no estoy revelando conocimiento secreto aquí.
Son conceptos de trading clásicos basados en la economía conductual, el flujo de órdenes y las estadísticas del mercado.
Sin embargo, siguen siendo altamente relevantes, especialmente en el contexto de la IA y el trading algorítmico, que solo amplifican estos patrones.

Con estas tres ideas, terminaré este video.

Hemos estudiado el objeto de investigación, planteado preguntas clave, proporcionado respuestas y esbozado tres direcciones para posibles estrategias.
Esto representa el trabajo esencial para esta etapa.

Recuerda: cualquier modelo es solo una hipótesis que vive solo mientras el entorno la valide.
La pregunta real no es "¿El sistema funciona?", es "¿Sabrá cuándo deja de funcionar?".
