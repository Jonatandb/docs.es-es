---
title: Descripción general de las transformaciones 3D
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- 3D transformations
- transformations [WPF], 3D
ms.assetid: e45e555d-ac1e-4b36-aced-e433afe7f27f
ms.openlocfilehash: 0efd6d247f23760c878c82cc92e99f1b83c1b9d2
ms.sourcegitcommit: 267d092663aba36b6b2ea853034470aea493bfae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "80112354"
---
# <a name="3d-transformations-overview"></a>Descripción general de las transformaciones 3D
En este tema se describe cómo aplicar transformaciones a modelos 3D en el [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] sistema de gráficos. Las transformaciones permiten que el desarrollador cambie la posición y el tamaño de los modelos, así como que los vuelva a orientar sin cambiar los valores base que los definen.  

## <a name="3d-coordinate-space"></a>Espacio de coordenadas 3D  
 El contenido de [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] gráficos 3D <xref:System.Windows.Controls.Viewport3D>en se encapsula en un elemento, que puede participar en la estructura de elementos bidimensionales. El sistema de gráficos trata los objetos Viewport3D como elementos visuales bidimensionales, al igual que ocurre con muchos otros elementos de [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]. Viewport3D funciona como ventana (una ventanilla) de una escena tridimensional. Más exactamente, es una superficie en la que se proyecta una escena 3D.  Aunque puede utilizar Viewport3D con otros objetos de dibujo 2D en el mismo gráfico de escena, no puede interpenetrar objetos 2D y 3D dentro de un Viewport3D. El elemento Viewport3D contiene el espacio de coordenadas descrito en la siguiente descripción.  
  
 El [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] sistema de coordenadas para gráficos 2D localiza el origen en la parte superior izquierda de la superficie de representación (normalmente la pantalla). En el sistema 2D, los valores positivos del eje X proceden a la derecha y los valores positivos del eje Y proceden hacia abajo. En el sistema de coordenadas 3D, sin embargo, el origen se encuentra en el centro de la pantalla, con valores positivos del eje X procediendo hacia la derecha pero valores positivos del eje Y procediendo hacia arriba en su lugar, y valores positivos del eje z que avanzan hacia afuera desde el origen, hacia el origen, hacia el origen, hacia el origen, hacia el origen, hacia el espectador.  
  
 ![Sistemas de coordenadas](./media/coordsystem-1.png "CoordSystem-1")  
Coordinación de la comparación de sistemas  
  
 El espacio definido por estos ejes es el marco [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]estacionario de referencia para objetos 3D en . Cuando se generan modelos en este espacio y se crean luces y cámaras para verlos, es útil distinguir este marco estático de referencia, o "espacio universal", del marco de referencia local creado para cada modelo al aplicarle transformaciones. Recuerde también que los objetos del espacio universal podrían parecer completamente diferentes, o incluso no verse en absoluto, dependiendo de la configuración de las luces y la cámara, pero que la posición de la cámara no cambia la ubicación de los objetos en el espacio universal.  
  
## <a name="transforming-models"></a>Transformación de modelos  
 Al crear modelos, estos tienen una ubicación determinada en la escena. Para mover esos modelos por la escena, girarlos o cambiar su tamaño, no es práctico cambiar los vértices que definen los propios modelos. En su lugar, al igual que en 2D, se aplican transformaciones a los modelos.  
  
 Cada objeto de <xref:System.Windows.Media.Media3D.Model3D.Transform%2A> modelo tiene una propiedad con la que puede mover, reorientar o cambiar el tamaño del modelo. Al aplicar una transformación, en realidad lo que se hace es desplazar todos los puntos del modelo según un vector o valor especificado por la transformación. Es decir, se transforma el espacio de coordenadas en el que se ha definido el modelo ("espacio del modelo"), pero no se cambian los valores que constituyen la geometría del modelo en el sistema de coordenadas de la escena completa ("espacio universal").  
  
## <a name="translation-transformations"></a>Transformaciones de la traslación  
 Las transformaciones 3D heredan <xref:System.Windows.Media.Media3D.Transform3D>de la clase base abstracta; estas incluyen las clases <xref:System.Windows.Media.Media3D.TranslateTransform3D> <xref:System.Windows.Media.Media3D.ScaleTransform3D>de <xref:System.Windows.Media.Media3D.RotateTransform3D>transformación afín , , y . El [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] sistema 3D <xref:System.Windows.Media.Media3D.MatrixTransform3D> también proporciona una clase que le permite especificar las mismas transformaciones en operaciones de matriz más concisas.  
  
 <xref:System.Windows.Media.Media3D.TranslateTransform3D>mueve todos los puntos del Model3D en la dirección <xref:System.Windows.Media.Media3D.TranslateTransform3D.OffsetX%2A> <xref:System.Windows.Media.Media3D.TranslateTransform3D.OffsetY%2A>del <xref:System.Windows.Media.Media3D.TranslateTransform3D.OffsetZ%2A> vector de desfase especificado con las propiedades , y . Por ejemplo, dado un vértice de un cubo en (2,2,2), un vector de desplazamiento de (0,1.6,1) movería el vértice (2,2,2) a (2,3.6,3). El vértice del cubo sigue siendo (2,2,2) en el espacio del modelo, pero ahora dicho espacio ha cambiado su relación al espacio universal, de tal forma que (2,2,2) en el espacio del modelo es (2,3.6,3) en el espacio universal.  
  
 ![Figura de traslación](./media/transforms-translate.png "Transforma-Traducir")  
Traslación con desplazamiento  
  
 En los ejemplos de código siguientes se muestra cómo aplicar una traslación.  
  
 [!code-xaml[animation3dgallery_snip#Translation3DAnimationExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/Translation3DAnimationExample.xaml#translation3danimationexamplewholepage)]  
  
## <a name="scale-transformations"></a>Transformaciones de escala  
 <xref:System.Windows.Media.Media3D.ScaleTransform3D>cambia la escala del modelo por un vector de escala especificado con referencia a un punto central. Especifique una escala uniforme, que escala el modelo mediante el mismo valor en los ejes X, Y y Z, para cambiar proporcionalmente el tamaño del modelo. Por ejemplo, si se <xref:System.Windows.Media.ScaleTransform.ScaleX%2A> <xref:System.Windows.Media.ScaleTransform.ScaleY%2A>establecen <xref:System.Windows.Media.Media3D.ScaleTransform3D.ScaleZ%2A> las propiedades , , y la transformación en 0,5 mitades, se reduce el tamaño del modelo; establecer las mismas propiedades en 2 duplica su escala en los tres ejes.  
  
 ![ScaleTransform3D uniforme](./media/threecubes-uniformscale-1.png "threecubes_uniformscale_1")  
Ejemplo de ScaleVector  
  
 Si se especifica una transformación de escala no uniforme (una transformación de escala cuyos valores X, Y y Z no son iguales), se puede provocar el estiramiento o la contracción del modelo en una o dos dimensiones sin afectar el resto. Por ejemplo, <xref:System.Windows.Media.ScaleTransform.ScaleX%2A> si <xref:System.Windows.Media.ScaleTransform.ScaleY%2A> se establece <xref:System.Windows.Media.Media3D.ScaleTransform3D.ScaleZ%2A> en 1, en 2 y en 1, el modelo transformado se duplicaría de altura, pero permanecería sin cambios a lo largo de los ejes X y Z.  
  
 De forma predeterminada, ScaleTransform3D provoca la expansión o contracción de los vértices en el origen (0,0,0). Sin, embargo, si el modelo que desea transformar no se dibuja partiendo del origen, al escalar el modelo a partir del origen, no se escalará el modelo "en contexto". En su lugar, cuando se multiplican los vértices del modelo por el vector de escala, la operación de la escala tendrá el efecto de trasladar y escalar el modelo.  
  
 ![Tres cubos con escala ajustada y con centro especificado](./media/threecubes-scaledwithcenter-1.png "threecubes_scaledwithcenter_1")  
Ejemplo de ScaleCenter  
  
 Para escalar un modelo "in situ", especifique el centro del modelo <xref:System.Windows.Media.ScaleTransform.CenterX%2A> <xref:System.Windows.Media.ScaleTransform.CenterY%2A>estableciendo <xref:System.Windows.Media.Media3D.ScaleTransform3D.CenterZ%2A> las propiedades , y las propiedades de ScaleTransform3D . Esto garantiza que el sistema de gráficos escala el espacio modelo <xref:System.Windows.Media.Media3D.Point3D>y, a continuación, lo convierte al centro en el archivo . A la inversa, si ha creado el modelo en el origen y especifica un punto central diferente, espere ver el modelo trasladado fuera del origen.  
  
## <a name="rotation-transformations"></a>Transformaciones de giro  
 Puede girar un modelo en 3D de varias maneras diferentes. Una transformación de giro típica especifica un eje y un ángulo de giro alrededor de ese eje. La <xref:System.Windows.Media.Media3D.RotateTransform3D> clase permite definir <xref:System.Windows.Media.Media3D.Rotation3D> a <xref:System.Windows.Media.Media3D.RotateTransform3D.Rotation%2A> con su propiedad. A continuación, especifique <xref:System.Windows.Media.Media3D.AxisAngleRotation3D.Axis%2A> y <xref:System.Windows.Media.Media3D.AxisAngleRotation3D.Angle%2A> propiedades en Rotation3D, en este caso un <xref:System.Windows.Media.Media3D.AxisAngleRotation3D>, para definir la transformación. En los ejemplos siguientes se gira un modelo 60 grados sobre el eje Y.  
  
 [!code-xaml[animation3dgallery_snip#Rotate3DUsingAxisAngleRotation3DExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/Rotat3DUsingAxisAngleRotation3DExample.xaml#rotate3dusingaxisanglerotation3dexamplewholepage)]  
  
 Nota:[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 3D es un sistema diestro, lo que significa que un valor de ángulo positivo para una rotación da como resultado una rotación en sentido contrario a las agujas del reloj sobre el eje.  
  
 Las rotaciones de ángulo de eje asumen rotación <xref:System.Windows.Media.Media3D.RotateTransform3D.CenterX%2A>sobre <xref:System.Windows.Media.Media3D.RotateTransform3D.CenterY%2A>el <xref:System.Windows.Media.Media3D.RotateTransform3D.CenterZ%2A> origen si no se especifica un valor para el , y las propiedades de RotateTransform3D. Como ocurre con el escalado, es útil recordar que el giro transforma el espacio de coordenadas de todo el modelo. Si el modelo no se creó sobre el origen, o se ha trasladado previamente, el giro podría "pivotar" sobre el origen en lugar de girar en contexto.  
  
 ![Rotation con nuevo centro](./media/threecubes-rotationwithcenter-1.png "threecubes_rotationwithcenter_1")  
Rotation con nuevo centro especificado  
  
 Para girar el modelo "en contexto", especifique el centro real del modelo como el centro de giro. Puesto que la geometría se suele modelar en el origen, la mayor parte de las veces puede obtener el resultado esperado de un conjunto de transformaciones si primero ajusta el tamaño del modelo (lo escala), después establece su orientación (lo gira) y, finalmente, lo mueve a la ubicación deseada (lo traslada).  
  
 ![Giro de 60 grados alrededor de los ejes X e Y](./media/twocubes-rotation2axes-1.png "twocubes_rotation2axes_1")  
Ejemplo de giro  
  
 Los giros angulares sobre el eje funcionan bien para las transformaciones estáticas y algunas animaciones. Sin embargo, considere girar un modelo de cubo 60 grados sobre el eje X y luego 45 grados sobre el eje Z. Puede describir esta transformación como dos transformaciones afines discretas o como matriz. Sin embargo, quizá sea difícil animar un giro definido así de forma continua. Aunque las posiciones inicial y final del modelo, calculadas mediante cualquiera de los enfoque son las mismas, varía el cálculo de las posiciones intermedias tomadas por el modelo. Los cuaterniones representan una forma alternativa de calcular la interpolación entre el inicio y el fin de un giro.  
  
 Un cuaternión representa un eje en el espacio 3D y una rotación alrededor de ese eje. Por ejemplo, un cuaternión podría representar un eje (1,1,2) y un giro de 50 grados. La capacidad de los cuaterniones al definir los giros procede de las dos operaciones que puede realizar en ellos: composición e interpolación. La composición de dos cuaterniones aplicada a una geometría significa "girar la geometría sobre el eje2 mediante giro2, después girarla sobre el eje1 mediante giro1". Utilizando la composición, puede combinar los dos giros en la geometría para obtener un solo cuaternión que representa el resultado. Puesto que la interpolación del cuaternión puede calcular una ruta de acceso continuada y correcta desde un eje y la orientación a otro, puede interpolar desde el cuaternión original al compuesto para lograr una transición continuada de uno a otro, permitiéndole animar la transformación. Para los modelos que desea animar, <xref:System.Windows.Media.Media3D.Quaternion> puede especificar un <xref:System.Windows.Media.Media3D.QuaternionRotation3D> destino <xref:System.Windows.Media.Media3D.RotateTransform3D.Rotation%2A> para la rotación mediante un para la propiedad.  
  
## <a name="using-transformation-collections"></a>Uso de colecciones de transformaciones  
 Al crear una escena, es normal aplicar más de una transformación a un modelo. Agregue transformaciones <xref:System.Windows.Media.Media3D.Transform3DGroup.Children%2A> a la <xref:System.Windows.Media.Media3D.Transform3DGroup> colección de la clase para agrupar las transformaciones convenientemente para aplicar las transformaciones a varios modelos de la escena. A menudo es conveniente volver a usar una transformación en varios grupos diferentes, de la misma forma que puede volver a usar un modelo aplicando un conjunto diferente de transformaciones a cada instancia. Observe que el orden en el que las transformaciones se agregan a la colección es importante: las transformaciones de la colección se aplican de la primera a la última.  
  
## <a name="animating-transformations"></a>Animación de transformaciones  
 La [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] implementación 3D participa en el mismo sistema de sincronización y animación que los gráficos 2D. En otras palabras, para animar una escena 3D, anime las propiedades de sus modelos. Es posible animar directamente las propiedades de los elementos primitivos, pero suele ser más fácil animar las transformaciones que cambian la posición o el aspecto de los modelos. Dado que las transformaciones se pueden aplicar tanto a <xref:System.Windows.Media.Media3D.Model3DGroup> objetos como a modelos individuales, es posible aplicar un conjunto de animaciones a los elementos secundarios de un grupo Model3D y otro conjunto de animaciones a un grupo de objetos.  Para información general sobre el sistema de control de tiempo y animación de [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)], consulte los temas [Información general sobre animaciones](animation-overview.md) e [Información general sobre objetos Storyboard](storyboards-overview.md).  
  
 Para animar un objeto en [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)], se crea una escala de tiempo, se define una animación (que, en realidad, es un cambio de algún valor de propiedad a lo largo del tiempo) y se especifica la propiedad a la que aplicar la animación. Esta propiedad debe ser una propiedad de FrameworkElement. Dado que todos los objetos de una escena 3D son elementos secundarios de Viewport3D, las propiedades de destino de cualquier animación que desee aplicar a la escena son propiedades de propiedades de Viewport3D. Es importante obtener la ruta de acceso de la propiedad de la animación correctamente, ya que la sintaxis puede ser prolija.  
  
 Suponga que desea girar un objeto en contexto, pero también aplicar un movimiento oscilante para que se vean más partes del objeto. Podría aplicar RotateTransform3D al modelo y animar el eje de giro de un vector a otro. En el ejemplo de código <xref:System.Windows.Media.Animation.Vector3DAnimation> siguiente se muestra cómo aplicar a la Axis propiedad de la transformación Rotation3D, suponiendo <xref:System.Windows.Media.TransformGroup>que El RotateTransform3D sea una de varias transformaciones aplicadas al modelo con un archivo .  
  
 [!code-csharp[3doverview#3DOverview3DN1](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn1)]
 [!code-vb[3doverview#3DOverview3DN1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn1)]  
  
 [!code-csharp[3doverview#3DOverview3DN3](~/samples/snippets/csharp/VS_Snippets_Wpf/3DOverview/CSharp/Window1.xaml.cs#3doverview3dn3)]
 [!code-vb[3doverview#3DOverview3DN3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/3DOverview/visualbasic/window1.xaml.vb#3doverview3dn3)]  
  
 Utilice una sintaxis similar para que otras propiedades de la transformación muevan o escalen el objeto.  Por ejemplo, puede <xref:System.Windows.Media.Animation.Point3DAnimation> aplicar a la propiedad ScaleCenter en una transformación de escala para hacer que un modelo distorsione suavemente su forma.  
  
 Aunque los ejemplos anteriores <xref:System.Windows.Media.Media3D.GeometryModel3D>transforman las propiedades de , también es posible transformar las propiedades de otros modelos en la escena.  Animando traslaciones aplicadas a objetos Light, por ejemplo, puede crear luz en movimiento y efectos de sombra que pueden cambiar el aspecto de los modelos espectacularmente.  
  
 Puesto que las cámaras también son modelos, igualmente se pueden transformar las propiedades de la cámara.  Aunque puede cambiar el aspecto de la escena transformando la ubicación de la cámara o las distancias de los planos (de hecho, transformando toda la proyección de la escena), tenga en cuenta que muchos de los efectos obtenidos de esta forma quizá no tengan tanto "sentido visual" para el lector como las transformaciones aplicadas a la ubicación o posición de los modelos de la escena.  
  
## <a name="see-also"></a>Consulte también

- [Descripción general de los gráficos 3D](3-d-graphics-overview.md)
- [Información general sobre transformaciones](transforms-overview.md)
- [Muestra de transformaciones 2D](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/2DTransforms)
