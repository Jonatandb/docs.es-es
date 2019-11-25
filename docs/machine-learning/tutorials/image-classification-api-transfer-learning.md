---
title: 'Tutorial: Inspección visual automatizada mediante el aprendizaje de transferencia'
description: En este tutorial se muestra cómo usar el aprendizaje de transferencia para entrenar un modelo de aprendizaje profundo de TensorFlow en ML.NET con la API de detección de imágenes con el fin de clasificar las imágenes de superficies de hormigón como con grietas o sin grietas.
author: luisquintanilla
ms.author: luquinta
ms.date: 11/14/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 443f9e9a83ebf31bb6c62323015af4a554323b67
ms.sourcegitcommit: 81ad1f09b93f3b3e6706a7f2e4ddf50ef229ea3d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74205054"
---
# <a name="tutorial-automated-visual-inspection-using-transfer-learning-with-the-mlnet-image-classification-api"></a><span data-ttu-id="fe7af-103">Tutorial: Inspección visual automatizada mediante el aprendizaje de transferencia con la API Image Classification de ML.NET</span><span class="sxs-lookup"><span data-stu-id="fe7af-103">Tutorial: Automated visual inspection using transfer learning with the ML.NET Image Classification API</span></span>

<span data-ttu-id="fe7af-104">Obtenga información sobre cómo entrenar un modelo de aprendizaje profundo personalizado mediante el aprendizaje de transferencia, un modelo TensorFlow previamente entrenado y la API Image Classification de ML.NET para clasificar las imágenes de superficies de hormigón como con grietas o sin grietas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-104">Learn how to train a custom deep learning model using transfer learning, a pretrained TensorFlow model and the ML.NET Image Classification API to classify images of concrete surfaces as cracked or uncracked.</span></span>

<span data-ttu-id="fe7af-105">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="fe7af-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> - <span data-ttu-id="fe7af-106">Entender el problema</span><span class="sxs-lookup"><span data-stu-id="fe7af-106">Understand the problem</span></span>
> - <span data-ttu-id="fe7af-107">Obtener información sobre la API Image Classification de ML.NET</span><span class="sxs-lookup"><span data-stu-id="fe7af-107">Learn about ML.NET Image Classification API</span></span>
> - <span data-ttu-id="fe7af-108">Comprender el modelo entrenado previamente</span><span class="sxs-lookup"><span data-stu-id="fe7af-108">Understand the pretrained model</span></span>
> - <span data-ttu-id="fe7af-109">Usar el aprendizaje de transferencia para entrenar un modelo de clasificación de imágenes de TensorFlow personalizado</span><span class="sxs-lookup"><span data-stu-id="fe7af-109">Use transfer learning to train a custom TensorFlow image classification model</span></span>
> - <span data-ttu-id="fe7af-110">Clasificar imágenes con el modelo personalizado</span><span class="sxs-lookup"><span data-stu-id="fe7af-110">Classify images with the custom model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe7af-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="fe7af-111">Prerequisites</span></span>

- <span data-ttu-id="fe7af-112">[Visual Studio 2017 15.6 o posterior](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) con la carga de trabajo "Desarrollo multiplataforma de .NET Core" instalada.</span><span class="sxs-lookup"><span data-stu-id="fe7af-112">[Visual Studio 2017 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017) with the ".NET Core cross-platform development" workload installed.</span></span>

## <a name="image-classification-transfer-learning-sample-overview"></a><span data-ttu-id="fe7af-113">Información general del ejemplo de transferencia de aprendizaje de clasificación de imágenes</span><span class="sxs-lookup"><span data-stu-id="fe7af-113">Image classification transfer learning sample overview</span></span>

<span data-ttu-id="fe7af-114">Este ejemplo es una aplicación de consola de .NET Core de C# que clasifica imágenes mediante un modelo TensorFlow de aprendizaje profundo entrenado previamente.</span><span class="sxs-lookup"><span data-stu-id="fe7af-114">This sample is a C# .NET Core console application that classifies images using a pretrained deep learning TensorFlow model.</span></span> <span data-ttu-id="fe7af-115">El código de este ejemplo se puede encontrar en el [repositorio dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="fe7af-115">The code for this sample can be found on the [dotnet/machinelearning-samples repository](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary) on GitHub.</span></span>

## <a name="understand-the-problem"></a><span data-ttu-id="fe7af-116">Entender el problema</span><span class="sxs-lookup"><span data-stu-id="fe7af-116">Understand the problem</span></span>

<span data-ttu-id="fe7af-117">La clasificación de imágenes es un problema de visión informática.</span><span class="sxs-lookup"><span data-stu-id="fe7af-117">Image classification is a computer vision problem.</span></span> <span data-ttu-id="fe7af-118">La clasificación de imágenes toma una imagen como entrada y la categoriza en una clase prescrita.</span><span class="sxs-lookup"><span data-stu-id="fe7af-118">Image classification takes an image as input and categorizes it into a prescribed class.</span></span> <span data-ttu-id="fe7af-119">A continuación se muestran algunos escenarios en los que la clasificación de imágenes es útil:</span><span class="sxs-lookup"><span data-stu-id="fe7af-119">Some scenarios where image classification is useful include:</span></span>

- <span data-ttu-id="fe7af-120">Reconocimiento facial</span><span class="sxs-lookup"><span data-stu-id="fe7af-120">Facial recognition</span></span>
- <span data-ttu-id="fe7af-121">Detección de emociones</span><span class="sxs-lookup"><span data-stu-id="fe7af-121">Emotion detection</span></span>
- <span data-ttu-id="fe7af-122">Diagnóstico médico</span><span class="sxs-lookup"><span data-stu-id="fe7af-122">Medical diagnosis</span></span>
- <span data-ttu-id="fe7af-123">Detección de puntos de referencia</span><span class="sxs-lookup"><span data-stu-id="fe7af-123">Landmark detection</span></span>

<span data-ttu-id="fe7af-124">En este tutorial se entrena un modelo de clasificación de imágenes personalizado para realizar una inspección visual automatizada de los tableros de puente con el fin de identificar las estructuras dañadas por las grietas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-124">This tutorial trains a custom image classification model to perform automated visual inspection of bridge decks to identify structures that are damaged by cracks.</span></span>

## <a name="mlnet-image-classification-api"></a><span data-ttu-id="fe7af-125">API Image Classification de ML.NET</span><span class="sxs-lookup"><span data-stu-id="fe7af-125">ML.NET Image Classification API</span></span>

<span data-ttu-id="fe7af-126">ML.NET proporciona varias formas de realizar la clasificación de imágenes.</span><span class="sxs-lookup"><span data-stu-id="fe7af-126">ML.NET provides various ways of performing image classification.</span></span> <span data-ttu-id="fe7af-127">En este tutorial se aplica el aprendizaje de transferencia mediante la API Image Classification.</span><span class="sxs-lookup"><span data-stu-id="fe7af-127">This tutorial applies transfer learning using the Image Classification API.</span></span> <span data-ttu-id="fe7af-128">La API Image Classification utiliza [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET), una biblioteca de bajo nivel que proporciona enlaces de C# para la API C++ de TensorFlow.</span><span class="sxs-lookup"><span data-stu-id="fe7af-128">The Image Classification API makes use of [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET), a low-level library that provides C# bindings for the TensorFlow C++ API.</span></span>

## <a name="what-is-transfer-learning"></a><span data-ttu-id="fe7af-129">¿Qué es el aprendizaje de transferencia?</span><span class="sxs-lookup"><span data-stu-id="fe7af-129">What is transfer learning?</span></span>

<span data-ttu-id="fe7af-130">El aprendizaje de transferencia aplica los conocimientos obtenidos de la resolución de un problema a otro problema relacionado.</span><span class="sxs-lookup"><span data-stu-id="fe7af-130">Transfer learning applies knowledge gained from solving one problem to another related problem.</span></span>

<span data-ttu-id="fe7af-131">Entrenar un modelo de aprendizaje profundo desde cero requiere establecer varios parámetros, una enorme cantidad de datos de entrenamiento etiquetados y una gran cantidad de recursos informáticos (cientos de horas de GPU).</span><span class="sxs-lookup"><span data-stu-id="fe7af-131">Training a deep learning model from scratch requires setting several parameters, a large amount of labeled training data, and a vast amount of compute resources (hundreds of GPU hours).</span></span> <span data-ttu-id="fe7af-132">El uso de un modelo entrenado previamente, junto con el aprendizaje de transferencia, permite acceso directo al proceso de entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-132">Using a pretrained model along with transfer learning allows you to shortcut the training process.</span></span>

## <a name="training-process"></a><span data-ttu-id="fe7af-133">Proceso de entrenamiento</span><span class="sxs-lookup"><span data-stu-id="fe7af-133">Training process</span></span>

<span data-ttu-id="fe7af-134">La API Image Classification inicia el proceso de entrenamiento mediante la carga de un modelo TensorFlow previamente entrenado.</span><span class="sxs-lookup"><span data-stu-id="fe7af-134">The Image Classification API starts the training process by loading a pretrained TensorFlow model.</span></span> <span data-ttu-id="fe7af-135">El proceso de entrenamiento consta de dos pasos:</span><span class="sxs-lookup"><span data-stu-id="fe7af-135">The training process consists of two steps:</span></span>

1. <span data-ttu-id="fe7af-136">Fase de cuello de botella</span><span class="sxs-lookup"><span data-stu-id="fe7af-136">Bottleneck phase</span></span>
2. <span data-ttu-id="fe7af-137">Fase de entrenamiento</span><span class="sxs-lookup"><span data-stu-id="fe7af-137">Training phase</span></span>

![Pasos de entrenamiento](./media/image-classification-api-transfer-learning/training.png)

### <a name="bottleneck-phase"></a><span data-ttu-id="fe7af-139">Fase de cuello de botella</span><span class="sxs-lookup"><span data-stu-id="fe7af-139">Bottleneck phase</span></span>

<span data-ttu-id="fe7af-140">Durante la fase de cuello de botella, se carga el conjunto de imágenes de aprendizaje y los valores de píxeles se usan como entrada, o características, para las capas inmovilizadas del modelo previamente entrenado.</span><span class="sxs-lookup"><span data-stu-id="fe7af-140">During the bottleneck phase, the set of training images is loaded and the pixel values are used as input, or features, for the frozen layers of the pretrained model.</span></span> <span data-ttu-id="fe7af-141">Las capas inmovilizadas incluyen todas las capas de la red neuronal hasta la penúltima capa, que se conoce como capa de cuello de botella.</span><span class="sxs-lookup"><span data-stu-id="fe7af-141">The frozen layers include all of the layers in the neural network up to the penultimate layer, informally known as the bottleneck layer.</span></span> <span data-ttu-id="fe7af-142">Estas capas se denominan inmovilizadas porque en ellas no se producirá ningún aprendizaje y las operaciones son de paso a través.</span><span class="sxs-lookup"><span data-stu-id="fe7af-142">These layers are referred to as frozen because no training will occur on these layers and operations are pass-through.</span></span> <span data-ttu-id="fe7af-143">En estas capas inmovilizadas es donde se calculan los patrones de nivel inferior que ayudan a un modelo a diferenciar entre las distintas clases.</span><span class="sxs-lookup"><span data-stu-id="fe7af-143">It's at these frozen layers where the lower-level patterns that help a model differentiate between the different classes are computed.</span></span> <span data-ttu-id="fe7af-144">Cuanto mayor sea el número de capas, más intensiva a nivel de computación será este paso.</span><span class="sxs-lookup"><span data-stu-id="fe7af-144">The larger the number of layers, the more computationally intensive this step is.</span></span> <span data-ttu-id="fe7af-145">Afortunadamente, puesto que se trata de un cálculo que se realiza una sola vez, los resultados se pueden almacenar en caché y usar en ejecuciones posteriores cuando se experimente con parámetros distintos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-145">Fortunately, since this is a one-time calculation, the results can be cached and used in later runs when experimenting with different parameters.</span></span>

### <a name="training-phase"></a><span data-ttu-id="fe7af-146">Fase de entrenamiento</span><span class="sxs-lookup"><span data-stu-id="fe7af-146">Training phase</span></span>

<span data-ttu-id="fe7af-147">Una vez calculados los valores de salida de la fase de cuello de botella, se usan como entrada para volver a entrenar la capa final del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-147">Once the output values from the bottleneck phase are computed, they are used as input to retrain the final layer of the model.</span></span> <span data-ttu-id="fe7af-148">Este proceso es iterativo y se ejecuta durante el número de veces que especifican los parámetros del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-148">This process is iterative and runs for the number of times specified by model parameters.</span></span> <span data-ttu-id="fe7af-149">Durante cada ejecución, se evalúan la pérdida y la precisión.</span><span class="sxs-lookup"><span data-stu-id="fe7af-149">During each run, the loss and accuracy are evaluated.</span></span> <span data-ttu-id="fe7af-150">Después, se realizan los ajustes adecuados para mejorar el modelo con el objetivo de minimizar la pérdida y maximizar la precisión.</span><span class="sxs-lookup"><span data-stu-id="fe7af-150">Then, the appropriate adjustments are made to improve the model with the goal of minimizing the loss and maximizing the accuracy.</span></span> <span data-ttu-id="fe7af-151">Una vez finalizado el entrenamiento, se generan dos formatos del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-151">Once training is finished, two model formats are output.</span></span> <span data-ttu-id="fe7af-152">Uno de ellos es la versión `.pb` del modelo y el otro es la versión serializada de ML.NET `.zip`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-152">One of them is the `.pb` version of the model and the other is the `.zip` ML.NET serialized version of the model.</span></span> <span data-ttu-id="fe7af-153">Cuando se trabaja en entornos compatibles con ML.NET, se recomienda usar la versión `.zip` del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-153">When working in environments supported by ML.NET, it is recommended to use the `.zip` version of the model.</span></span> <span data-ttu-id="fe7af-154">Sin embargo, en entornos en los que no se admite ML.NET, se tiene la opción de usar la versión `.pb`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-154">However, in environments where ML.NET is not supported, you have the option of using the `.pb` version.</span></span>

## <a name="understand-the-pretrained-model"></a><span data-ttu-id="fe7af-155">Descripción del modelo entrenado previamente</span><span class="sxs-lookup"><span data-stu-id="fe7af-155">Understand the pretrained model</span></span>

<span data-ttu-id="fe7af-156">El modelo previamente entrenado que se usa en este tutorial es la variante de capa 101 del modelo de Residual Network v2 (ResNet).</span><span class="sxs-lookup"><span data-stu-id="fe7af-156">The pretrained model used in this tutorial is the 101-layer variant of the Residual Network (ResNet) v2 model.</span></span> <span data-ttu-id="fe7af-157">El modelo original se ha entrenado para clasificar las imágenes en mil categorías.</span><span class="sxs-lookup"><span data-stu-id="fe7af-157">The original model is trained to classify images into a thousand categories.</span></span> <span data-ttu-id="fe7af-158">El modelo toma como entrada una imagen de tamaño 224 x 224 y genera las probabilidades de clase para cada una de las clases en las que se ha entrenado.</span><span class="sxs-lookup"><span data-stu-id="fe7af-158">The model takes as input an image of size 224 x 224 and outputs the class probabilities for each of the classes it's trained on.</span></span> <span data-ttu-id="fe7af-159">Parte de este modelo se usa para entrenar un nuevo modelo mediante imágenes personalizadas con el fin de realizar predicciones entre dos clases.</span><span class="sxs-lookup"><span data-stu-id="fe7af-159">Part of this model is used to train a new model using custom images to make predictions between two classes.</span></span>

## <a name="create-console-application"></a><span data-ttu-id="fe7af-160">Creación de una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="fe7af-160">Create console application</span></span>

<span data-ttu-id="fe7af-161">Ahora que tiene conocimientos generales del aprendizaje de transferencia y de la API Image Classification, es momento de compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-161">Now that you have a general understanding of transfer learning and the Image Classification API, it's time to build the application.</span></span>

1. <span data-ttu-id="fe7af-162">Cree una **Aplicación de consola de .NET Core de C#** llamada "DeepLearning_ImageClassification_Binary".</span><span class="sxs-lookup"><span data-stu-id="fe7af-162">Create a **C# .NET Core Console Application** called "DeepLearning_ImageClassification_Binary".</span></span>
1. <span data-ttu-id="fe7af-163">Instale el paquete NuGet **Microsoft.ML**, versión **1.4.0**:</span><span class="sxs-lookup"><span data-stu-id="fe7af-163">Install the **Microsoft.ML** version **1.4.0** NuGet Package:</span></span>
    1. <span data-ttu-id="fe7af-164">En el Explorador de soluciones, haga clic con el botón derecho en **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fe7af-164">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span>
    1. <span data-ttu-id="fe7af-165">Elija "nuget.org" como origen del paquete.</span><span class="sxs-lookup"><span data-stu-id="fe7af-165">Choose "nuget.org" as the Package source.</span></span>
    1. <span data-ttu-id="fe7af-166">Seleccione la pestaña **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="fe7af-166">Select the **Browse** tab.</span></span>
    1. <span data-ttu-id="fe7af-167">Active la casilla **Incluir versión preliminar**.</span><span class="sxs-lookup"><span data-stu-id="fe7af-167">Check the **Include prerelease** checkbox.</span></span>
    1. <span data-ttu-id="fe7af-168">Busque **Microsoft.ML**.</span><span class="sxs-lookup"><span data-stu-id="fe7af-168">Search for **Microsoft.ML**.</span></span>
    1. <span data-ttu-id="fe7af-169">Seleccione el botón **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="fe7af-169">Select the **Install** button.</span></span>
    1. <span data-ttu-id="fe7af-170">Seleccione el botón **Aceptar** en el cuadro de diálogo **Vista previa de cambios** y, a continuación, seleccione el botón **Acepto** del cuadro de diálogo **Aceptación de la licencia** en caso de que esté de acuerdo con los términos de licencia de los paquetes mostrados.</span><span class="sxs-lookup"><span data-stu-id="fe7af-170">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>
    1. <span data-ttu-id="fe7af-171">Repita estos pasos para los paquetes NuGet **Microsoft.ML.Vision**, versión **1.4.0**, **SciSharp.TensorFlow.Redist**, versión **1.15.0** y **Microsoft.ML.ImageAnalytics**, versión **1.4.0**.</span><span class="sxs-lookup"><span data-stu-id="fe7af-171">Repeat these steps for the **Microsoft.ML.Vision** version **1.4.0**, **SciSharp.TensorFlow.Redist** version **1.15.0**, and **Microsoft.ML.ImageAnalytics** version **1.4.0** NuGet packages.</span></span>

### <a name="prepare-and-understand-the-data"></a><span data-ttu-id="fe7af-172">Preparar y entender los datos</span><span class="sxs-lookup"><span data-stu-id="fe7af-172">Prepare and understand the data</span></span>

> [!NOTE]
> <span data-ttu-id="fe7af-173">Los conjuntos de valores de este tutorial están tomados del documento "SDNET2018: A concrete crack image dataset for machine learning applications" (2018) (SDNET2018: Un conjunto de datos de imágenes de hormigón descifradas para aplicaciones de aprendizaje automático) de Marc Maguire, Sattar Dorafshan y Robert J. Thomas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-173">The datasets for this tutorial are from Maguire, Marc; Dorafshan, Sattar; and Thomas, Robert J., "SDNET2018: A concrete crack image dataset for machine learning applications" (2018).</span></span> <span data-ttu-id="fe7af-174">Examinar todos los conjuntos de datos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-174">Browse all Datasets.</span></span> <span data-ttu-id="fe7af-175">Documento 48.</span><span class="sxs-lookup"><span data-stu-id="fe7af-175">Paper 48.</span></span> <span data-ttu-id="fe7af-176">https://digitalcommons.usu.edu/all_datasets/48</span><span class="sxs-lookup"><span data-stu-id="fe7af-176">https://digitalcommons.usu.edu/all_datasets/48</span></span>

<span data-ttu-id="fe7af-177">SDNET2018 es un conjunto de datos de imágenes que contiene anotaciones para estructuras de hormigón con grietas y sin grietas (tableros de puente, muros y pavimento).</span><span class="sxs-lookup"><span data-stu-id="fe7af-177">SDNET2018 is an image dataset that contains annotations for cracked and non-cracked concrete structures (bridge decks, walls, and pavement).</span></span>

![Ejemplos de tablero de puente del conjunto de datos de SDNET2018](./media/image-classification-api-transfer-learning/sdnet2018decksamples.png)

<span data-ttu-id="fe7af-179">Los datos se organizan en tres subdirectorios:</span><span class="sxs-lookup"><span data-stu-id="fe7af-179">The data is organized in three subdirectories:</span></span>

- <span data-ttu-id="fe7af-180">D contiene imágenes de tableros de puente</span><span class="sxs-lookup"><span data-stu-id="fe7af-180">D contains bridge deck images</span></span>
- <span data-ttu-id="fe7af-181">P contiene imágenes de pavimentos</span><span class="sxs-lookup"><span data-stu-id="fe7af-181">P contains pavement images</span></span>
- <span data-ttu-id="fe7af-182">W contiene imágenes de muros</span><span class="sxs-lookup"><span data-stu-id="fe7af-182">W contains wall images</span></span>

<span data-ttu-id="fe7af-183">Cada uno de estos subdirectorios contiene a su vez dos subdirectorios adicionales con prefijos:</span><span class="sxs-lookup"><span data-stu-id="fe7af-183">Each of these subdirectories contains two additional prefixed subdirectories:</span></span>

- <span data-ttu-id="fe7af-184">C es el prefijo usado para las superficies con grietas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-184">C is the prefix used for cracked surfaces</span></span>
- <span data-ttu-id="fe7af-185">U es el prefijo usado para las superficies sin grietas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-185">U is the prefix used for uncracked surfaces</span></span>

<span data-ttu-id="fe7af-186">En este tutorial, solo se usan imágenes de tableros de puente.</span><span class="sxs-lookup"><span data-stu-id="fe7af-186">In this tutorial, only bridge deck images are used.</span></span>

1. <span data-ttu-id="fe7af-187">Descargue el [conjunto de datos](https://github.com/dotnet/machinelearning-samples/raw/master/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/assets.zip) y descomprímalo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-187">Download the [dataset](https://github.com/dotnet/machinelearning-samples/raw/master/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/assets.zip) and unzip.</span></span>
1. <span data-ttu-id="fe7af-188">Cree un directorio llamado "recursos" en el proyecto para guardar los archivos del conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-188">Create a directory named "assets" in your project to save your dataset files.</span></span>
1. <span data-ttu-id="fe7af-189">Copie los subdirectorios *CD* y *UD* del directorio descomprimido recientemente en el directorio *recursos*.</span><span class="sxs-lookup"><span data-stu-id="fe7af-189">Copy the *CD* and *UD* subdirectories from the recently unzipped directory to the *assets* directory.</span></span>

### <a name="create-input-and-output-classes"></a><span data-ttu-id="fe7af-190">Creación de las clases de entrada y salida</span><span class="sxs-lookup"><span data-stu-id="fe7af-190">Create input and output classes</span></span>

1. <span data-ttu-id="fe7af-191">Abra el archivo *Program.cs* y reemplace las instrucciones `using` existentes en la parte superior del archivo por las siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe7af-191">Open the *Program.cs* file and replace the existing `using` statements at the top of the file with the following:</span></span>

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L1-L7)]

1. <span data-ttu-id="fe7af-192">Debajo de la clase `Program` en *Program.cs*, cree una clase llamada `ImageData`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-192">Below the `Program` class in *Program.cs*, create a class called `ImageData`.</span></span> <span data-ttu-id="fe7af-193">Esta clase se utiliza para representar los datos cargados inicialmente.</span><span class="sxs-lookup"><span data-stu-id="fe7af-193">This class is used to represent the initially loaded data.</span></span>

    [!code-csharp [ImageDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L137-L142)]

    <span data-ttu-id="fe7af-194">`ImageData` contiene las propiedades siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe7af-194">`ImageData` contains the following properties:</span></span>

    - <span data-ttu-id="fe7af-195">`ImagePath` es la ruta de acceso completa donde se almacena la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-195">`ImagePath` is the fully qualified path where the image is stored.</span></span>
    - <span data-ttu-id="fe7af-196">`Label` es la categoría a la que pertenece la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-196">`Label` is the category the image belongs to.</span></span> <span data-ttu-id="fe7af-197">Este es el valor que se va a predecir.</span><span class="sxs-lookup"><span data-stu-id="fe7af-197">This is the value to predict.</span></span>

1. <span data-ttu-id="fe7af-198">Cree clases para los datos de entrada y salida.</span><span class="sxs-lookup"><span data-stu-id="fe7af-198">Create classes for your input and output data</span></span>

    1. <span data-ttu-id="fe7af-199">Debajo de la clase `ImageData`, defina el esquema de los datos de entrada en una nueva clase llamada `ModelInput`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-199">Below the `ImageData` class, define the schema of your input data in a new class called `ModelInput`.</span></span>

        [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L144-L153)]

        <span data-ttu-id="fe7af-200">`ModelInput` contiene las propiedades siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe7af-200">`ModelInput` contains the following properties:</span></span>

        - <span data-ttu-id="fe7af-201">`ImagePath` es la ruta de acceso completa donde se almacena la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-201">`ImagePath` is the fully qualified path where the image is stored.</span></span>
        - <span data-ttu-id="fe7af-202">`Label` es la categoría a la que pertenece la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-202">`Label` is the category the image belongs to.</span></span> <span data-ttu-id="fe7af-203">Este es el valor que se va a predecir.</span><span class="sxs-lookup"><span data-stu-id="fe7af-203">This is the value to predict.</span></span>
        - <span data-ttu-id="fe7af-204">`Image` es la representación `byte[]` de la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-204">`Image` is the `byte[]` representation of the image.</span></span> <span data-ttu-id="fe7af-205">El modelo espera que los datos de la imagen sean de este tipo para el entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-205">The model expects image data to be of this type for training.</span></span>
        - <span data-ttu-id="fe7af-206">`LabelAsKey` es la representación numérica de `Label`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-206">`LabelAsKey` is the numerical representation of the `Label`.</span></span>

        <span data-ttu-id="fe7af-207">Solo se usan `Image` y `LabelAsKey` para entrenar el modelo y hacer predicciones.</span><span class="sxs-lookup"><span data-stu-id="fe7af-207">Only `Image` and `LabelAsKey` are used to train the model and make predictions.</span></span> <span data-ttu-id="fe7af-208">Las propiedades `ImagePath` y `Label` se conservan por comodidad para tener acceso al nombre y categoría del archivo de imagen original.</span><span class="sxs-lookup"><span data-stu-id="fe7af-208">The `ImagePath` and `Label` properties are kept for convenience to access the original image file name and category.</span></span>

    1. <span data-ttu-id="fe7af-209">Después, debajo de la clase `ModelInput`, defina el esquema de los datos de salida en una nueva clase llamada `ModelOutput`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-209">Then, below the `ModelInput` class, define the schema of your output data in a new class called `ModelOutput`.</span></span>

        [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L155-L162)]

        <span data-ttu-id="fe7af-210">`ModelOutput` contiene las propiedades siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe7af-210">`ModelOutput` contains the following properties:</span></span>

        - <span data-ttu-id="fe7af-211">`ImagePath` es la ruta de acceso completa donde se almacena la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-211">`ImagePath` is the fully qualified path where the image is stored.</span></span>
        - <span data-ttu-id="fe7af-212">`Label` es la categoría original a la que pertenece la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-212">`Label` is the original category the image belongs to.</span></span> <span data-ttu-id="fe7af-213">Este es el valor que se va a predecir.</span><span class="sxs-lookup"><span data-stu-id="fe7af-213">This is the value to predict.</span></span>
        - <span data-ttu-id="fe7af-214">`PredictedLabel` es el valor que predice el modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-214">`PredictedLabel` is the value predicted by the model.</span></span>

        <span data-ttu-id="fe7af-215">De forma similar a `ModelInput`, solo se requiere `PredictedLabel` para realizar predicciones, ya que contiene la predicción que realiza el modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-215">Similar to `ModelInput`, only the `PredictedLabel` is required to make predictions since it contains the prediction made by the model.</span></span> <span data-ttu-id="fe7af-216">Las propiedades `ImagePath` y `Label` se conservan por comodidad para tener acceso al nombre y categoría del archivo de imagen original.</span><span class="sxs-lookup"><span data-stu-id="fe7af-216">The `ImagePath` and `Label` properties are retained for convenience to access the original image file name and category.</span></span>

### <a name="create-workspace-directory"></a><span data-ttu-id="fe7af-217">Creación del directorio del área de trabajo</span><span class="sxs-lookup"><span data-stu-id="fe7af-217">Create workspace directory</span></span>

<span data-ttu-id="fe7af-218">Cuando los datos de entrenamiento y validación no cambian a menudo, se recomienda almacenar en caché los valores de cuello de botella calculados para las ejecuciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="fe7af-218">When training and validation data do not change often, it is good practice to cache the computed bottleneck values for further runs.</span></span>

1. <span data-ttu-id="fe7af-219">En el proyecto, cree un directorio llamado *workspace* para almacenar los valores de cuello de botella calculados y la versión `.pb` del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-219">In your project, create a new directory called *workspace* to store the computed bottleneck values and `.pb` version of the model.</span></span>

### <a name="define-paths-and-initialize-variables"></a><span data-ttu-id="fe7af-220">Definición de rutas de acceso e inicialización de variables</span><span class="sxs-lookup"><span data-stu-id="fe7af-220">Define paths and initialize variables</span></span>

1. <span data-ttu-id="fe7af-221">Dentro del método `Main`, defina la ubicación de los recursos, los valores de cuello de botella calculados y la versión `.pb` del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-221">Inside the `Main` method, define the location of your assets, computed bottleneck values and `.pb` version of the model.</span></span>

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L15-L17)]

1. <span data-ttu-id="fe7af-222">Luego, inicialice la variable `mlContext` con una instancia nueva de [MLContext](xref:Microsoft.ML.MLContext).</span><span class="sxs-lookup"><span data-stu-id="fe7af-222">Then, initialize the `mlContext` variable with a new instance of [MLContext](xref:Microsoft.ML.MLContext).</span></span>

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L19)]

    <span data-ttu-id="fe7af-223">La clase [MLContext](xref:Microsoft.ML.MLContext) es un punto de partida para todas las operaciones de ML.NET. Al inicializar mlContext se crea un entorno de ML.NET que se puede compartir entre los objetos del flujo de trabajo de creación de modelos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-223">The [MLContext](xref:Microsoft.ML.MLContext) class is a starting point for all ML.NET operations, and initializing mlContext creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="fe7af-224">Como concepto, se parece a `DBContext` en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fe7af-224">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

## <a name="load-the-data"></a><span data-ttu-id="fe7af-225">Carga de los datos</span><span class="sxs-lookup"><span data-stu-id="fe7af-225">Load the data</span></span>

### <a name="create-data-loading-utility-method"></a><span data-ttu-id="fe7af-226">Creación de un método de utilidad de carga de datos</span><span class="sxs-lookup"><span data-stu-id="fe7af-226">Create data loading utility method</span></span>

<span data-ttu-id="fe7af-227">Las imágenes se almacenan en dos subdirectorios.</span><span class="sxs-lookup"><span data-stu-id="fe7af-227">The images are stored in two subdirectories.</span></span> <span data-ttu-id="fe7af-228">Antes de cargar los datos, se debe dar formato a una lista de objetos de `ImageData`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-228">Before loading the data, it needs to be formatted into a list of `ImageData` objects.</span></span> <span data-ttu-id="fe7af-229">Para ello, agregue el método `LoadImagesFromDirectory` debajo del método `Main`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-229">To do so, create the `LoadImagesFromDirectory` method below the `Main` method.</span></span>

```csharp
public static IEnumerable<ImageData> LoadImagesFromDirectory(string folder, bool useFolderNameAsLabel = true)
{

}
```

1. <span data-ttu-id="fe7af-230">En `LoadImagesDirectory`, agregue el código siguiente para obtener todas las rutas de acceso de archivo de los subdirectorios:</span><span class="sxs-lookup"><span data-stu-id="fe7af-230">Inside the `LoadImagesDirectory` add the following code to get all of the file paths from the subdirectories:</span></span>

    [!code-csharp [GetFiles](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L104-L105)]

1. <span data-ttu-id="fe7af-231">Luego, recorra en iteración cada uno de los archivos mediante una instrucción `foreach`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-231">Then, iterate through each of the files using a `foreach` statement.</span></span>

    ```csharp
    foreach (var file in files)
    {

    }
    ```

1. <span data-ttu-id="fe7af-232">En la instrucción `foreach`, compruebe que se admiten las extensiones de archivo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-232">Inside the `foreach` statement, check that the file extensions are supported.</span></span> <span data-ttu-id="fe7af-233">La API Image Classification admite formatos JPEG y PNG.</span><span class="sxs-lookup"><span data-stu-id="fe7af-233">The Image Classification API supports JPEG and PNG formats.</span></span>

    [!code-csharp [CheckExtension](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L109-L110)]

1. <span data-ttu-id="fe7af-234">Después, obtenga la etiqueta del archivo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-234">Then, get the label for the file.</span></span> <span data-ttu-id="fe7af-235">Si el parámetro `useFolderNameAsLabel` está establecido en `true`, el directorio principal donde se guarda el archivo se usa como etiqueta.</span><span class="sxs-lookup"><span data-stu-id="fe7af-235">If the `useFolderNameAsLabel` parameter is set to `true`, then the parent directory where the file is saved is used as the label.</span></span> <span data-ttu-id="fe7af-236">De lo contrario, espera que la etiqueta sea un prefijo del nombre de archivo o el propio nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-236">Otherwise, it expects the label to be a prefix of the file name or the file name itself.</span></span>

    [!code-csharp [GetLabel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L112-L126)]

1. <span data-ttu-id="fe7af-237">Por último, cree una nueva instancia de `ModelInput`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-237">Finally, create a new instance of `ModelInput`.</span></span>

    [!code-csharp [CreateImageData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L128-L132)]

### <a name="prepare-the-data"></a><span data-ttu-id="fe7af-238">Preparar los datos</span><span class="sxs-lookup"><span data-stu-id="fe7af-238">Prepare the data</span></span>

1. <span data-ttu-id="fe7af-239">De vuelta en el método `Main`, use el método de utilidad `LoadFromDirectory` para obtener la lista de imágenes que se usan para el entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-239">Back in the `Main` method, use the `LoadFromDirectory` utility method to get the list of images used for training.</span></span>

    [!code-csharp [LoadImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L21)]

1. <span data-ttu-id="fe7af-240">Luego, cargue las imágenes en un elemento [`IDataView`](xref:Microsoft.ML.IDataView) con el método [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable*).</span><span class="sxs-lookup"><span data-stu-id="fe7af-240">Then, load the images into an [`IDataView`](xref:Microsoft.ML.IDataView) using the [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable*) method.</span></span>

    [!code-csharp [CreateIDataView](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L23)]

1. <span data-ttu-id="fe7af-241">Los datos se cargan en el orden en que se han leído desde los directorios.</span><span class="sxs-lookup"><span data-stu-id="fe7af-241">The data is loaded in the order it was read from the directories.</span></span> <span data-ttu-id="fe7af-242">Para equilibrar los datos, ordénelos aleatoriamente mediante el método [`ShuffleRows`](xref:Microsoft.ML.DataOperationsCatalog.ShuffleRows*).</span><span class="sxs-lookup"><span data-stu-id="fe7af-242">To balance the data, shuffle it using the [`ShuffleRows`](xref:Microsoft.ML.DataOperationsCatalog.ShuffleRows*) method.</span></span>

    [!code-csharp [ShuffleRows](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L25)]

1. <span data-ttu-id="fe7af-243">Los modelos de Machine Learning esperan que la entrada esté en formato numérico.</span><span class="sxs-lookup"><span data-stu-id="fe7af-243">Machine learning models expect input to be in numerical format.</span></span> <span data-ttu-id="fe7af-244">Por lo tanto, es necesario realizar algún procesamiento previo en los datos antes del entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-244">Therefore, some preprocessing needs to be done on the data prior to training.</span></span> <span data-ttu-id="fe7af-245">Cree un elemento [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) formado por las transformaciones [`MapValueToKey`](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey*) y `LoadRawImageBytes`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-245">Create an [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) made up of the [`MapValueToKey`](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey*) and `LoadRawImageBytes` transforms.</span></span> <span data-ttu-id="fe7af-246">La transformación `MapValueToKey` toma el valor de categoría en la columna `Label`, lo convierte en un valor `KeyType` numérico y lo almacena en una nueva columna llamada `LabelAsKey`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-246">The `MapValueToKey` transform takes the categorical value in the `Label` column, converts it to a numerical `KeyType` value and stores it in a new column called `LabelAsKey`.</span></span> <span data-ttu-id="fe7af-247">`LoadImages` toma los valores de la columna `ImagePath`, junto con el parámetro `imageFolder`, con el fin de cargar las imágenes para el entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-247">The `LoadImages` takes the values from the `ImagePath` column along with the `imageFolder` parameter to load images for training.</span></span>

    [!code-csharp [PreprocessingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L27-L33)]

1. <span data-ttu-id="fe7af-248">Use el método [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit*) para aplicar los datos a `preprocessingPipeline`[`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) seguido del método [`Transform`](xref:Microsoft.ML.Data.TransformerChain`1.Transform*), que devuelve un elemento [`IDataView`](xref:Microsoft.ML.IDataView) que contiene los datos procesados previamente.</span><span class="sxs-lookup"><span data-stu-id="fe7af-248">Use the [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit*) method to apply the data to the `preprocessingPipeline` [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) followed by the [`Transform`](xref:Microsoft.ML.Data.TransformerChain`1.Transform*) method, which returns an [`IDataView`](xref:Microsoft.ML.IDataView) containing the pre-processed data.</span></span>

    [!code-csharp [PreprocessData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L35-L37)]

1. <span data-ttu-id="fe7af-249">Para entrenar un modelo, es importante tener un conjunto de datos de entrenamiento, así como un conjunto de datos de validación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-249">To train a model, it's important to have a training dataset as well as a validation dataset.</span></span> <span data-ttu-id="fe7af-250">El modelo se entrena en el conjunto de entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-250">The model is trained on the training set.</span></span> <span data-ttu-id="fe7af-251">La calidad de las predicciones en los datos no vistos la mide el rendimiento en el conjunto de validación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-251">How well it makes predictions on unseen data is measured by the performance against the validation set.</span></span> <span data-ttu-id="fe7af-252">En función de los resultados de ese rendimiento, el modelo realiza ajustes sobre lo que ha aprendido en un esfuerzo por mejorar.</span><span class="sxs-lookup"><span data-stu-id="fe7af-252">Based on the results of that performance, the model makes adjustments to what it has learned in an effort to improve.</span></span> <span data-ttu-id="fe7af-253">El conjunto de validación puede proceder de dividir el conjunto de datos original o de otro origen que ya se haya reservado para esta finalidad.</span><span class="sxs-lookup"><span data-stu-id="fe7af-253">The validation set can come from either splitting your original dataset or from another source that has already been set aside for this purpose.</span></span> <span data-ttu-id="fe7af-254">En este caso, el conjunto de datos procesado previamente se divide en conjuntos de entrenamiento, validación y pruebas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-254">In this case, the pre-processed dataset is split into training, validation and test sets.</span></span>

    [!code-csharp [CreateDataSplits](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L39-L40)]

    <span data-ttu-id="fe7af-255">En el ejemplo de código anterior se realizan dos divisiones.</span><span class="sxs-lookup"><span data-stu-id="fe7af-255">The code sample above performs two splits.</span></span> <span data-ttu-id="fe7af-256">En primer lugar, los datos procesados previamente se dividen y el 70 % de estos se usa para el entrenamiento, mientras que el 30 % restante se utiliza para la validación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-256">First, the pre-processed data is split and 70% is used for training while the remaining 30% is used for validation.</span></span> <span data-ttu-id="fe7af-257">Después, 30 % perteneciente al conjunto de validación se divide en conjuntos de validación y pruebas, donde el 90 % se usa para la validación y el 10 % se usa para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-257">Then, the 30% validation set is further split into validation and test sets where 90% is used for validation and 10% is used for testing.</span></span>

    <span data-ttu-id="fe7af-258">Una manera de pensar en la finalidad de estas particiones de datos es hacer un examen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-258">A way to think about the purpose of these data partitions is taking an exam.</span></span> <span data-ttu-id="fe7af-259">Al estudiar para un examen, se revisan las notas, libros u otros recursos para obtener una idea sobre los conceptos que hay que preparar para el examen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-259">When studying for an exam, you review your notes, books, or other resources to get a grasp on the concepts that are on the exam.</span></span> <span data-ttu-id="fe7af-260">Esto es para lo que sirve el conjunto de entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-260">This is what the train set is for.</span></span> <span data-ttu-id="fe7af-261">Después, se puede realizar un examen ficticio para poner a prueba los conocimientos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-261">Then, you might take a mock exam to validate your knowledge.</span></span> <span data-ttu-id="fe7af-262">Aquí es donde el conjunto de validación es práctico.</span><span class="sxs-lookup"><span data-stu-id="fe7af-262">This is where the validation set comes in handy.</span></span> <span data-ttu-id="fe7af-263">Se quiere comprobar si se tiene una idea correcta de los conceptos antes de realizar el examen real.</span><span class="sxs-lookup"><span data-stu-id="fe7af-263">You want to check whether you have a good grasp of the concepts before taking the actual exam.</span></span> <span data-ttu-id="fe7af-264">En función de estos resultados, tome nota de lo que no haya acertado o de lo que no haya entendido bien e incorpore los cambios a medida que repasa para el examen real.</span><span class="sxs-lookup"><span data-stu-id="fe7af-264">Based on those results, you take note of what you got wrong or didn't understand well and incorporate your changes as you review for the real exam.</span></span> <span data-ttu-id="fe7af-265">Por último, realice el examen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-265">Finally, you take the exam.</span></span> <span data-ttu-id="fe7af-266">Esto es para lo que se usa el conjunto de prueba.</span><span class="sxs-lookup"><span data-stu-id="fe7af-266">This is what the test set is used for.</span></span> <span data-ttu-id="fe7af-267">Nunca ha visto las preguntas que aparecen en el examen y ahora puede usar lo que ha aprendido a partir del entrenamiento y la validación para aplicar sus conocimientos a la tarea que nos ocupa.</span><span class="sxs-lookup"><span data-stu-id="fe7af-267">You've never seen the questions that are on the exam and now use what you learned from training and validation to apply your knowledge to the task at hand.</span></span>

1. <span data-ttu-id="fe7af-268">Asigne a las particiones sus respectivos valores para los datos de entrenamiento, validación y prueba.</span><span class="sxs-lookup"><span data-stu-id="fe7af-268">Assign the partitions their respective values for the train, validation and test data.</span></span>

    [!code-csharp [CreateDatasets](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L42-L44)]

## <a name="define-the-training-pipeline"></a><span data-ttu-id="fe7af-269">Definición de la canalización de entrenamiento</span><span class="sxs-lookup"><span data-stu-id="fe7af-269">Define the training pipeline</span></span>

<span data-ttu-id="fe7af-270">El entrenamiento del modelo consta de un par de pasos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-270">Model training consists of a couple of steps.</span></span> <span data-ttu-id="fe7af-271">En primer lugar, se usa la API Image Classification para entrenar el modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-271">First, Image Classification API is used to train the model.</span></span> <span data-ttu-id="fe7af-272">Luego, las etiquetas codificadas de la columna `PredictedLabel` se convierten de nuevo a su valor de categoría original mediante la transformación `MapKeyToValue`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-272">Then, the encoded labels in the `PredictedLabel` column are converted back to their original categorical value using the `MapKeyToValue` transform.</span></span>

1. <span data-ttu-id="fe7af-273">Cree una variable para almacenar un conjunto de parámetros obligatorios y opcionales para un `ImageClassificationTrainer`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-273">Create a new variable to store a set of required and optional parameters for an `ImageClassificationTrainer`.</span></span> 

    [!code-csharp [ClassifierOptions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L46-L57)]

    <span data-ttu-id="fe7af-274">`ImageClassificationTrainer` admite varios parámetros opcionales:</span><span class="sxs-lookup"><span data-stu-id="fe7af-274">An `ImageClassificationTrainer` takes several optional parameters:</span></span>

    - <span data-ttu-id="fe7af-275">`FeatureColumnName` es la columna que se utiliza como entrada del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-275">`FeatureColumnName` is the column that is used as input for the model.</span></span>
    - <span data-ttu-id="fe7af-276">`LabelColumnName` es la columna del valor que se va a predecir.</span><span class="sxs-lookup"><span data-stu-id="fe7af-276">`LabelColumnName` is the column for the value to predict.</span></span>
    - <span data-ttu-id="fe7af-277">`ValidationSet` es [`IDataView`](xref:Microsoft.ML.IDataView) que contiene los datos de validación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-277">`ValidationSet` is the [`IDataView`](xref:Microsoft.ML.IDataView) containing the validation data.</span></span>
    - <span data-ttu-id="fe7af-278">`Arch` define la arquitectura de modelo previamente entrenada que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="fe7af-278">`Arch` defines which of the pretrained model architectures to use.</span></span> <span data-ttu-id="fe7af-279">En este tutorial se usa la variante de capa 101 del modelo ResNetv2.</span><span class="sxs-lookup"><span data-stu-id="fe7af-279">This tutorial uses the 101-layer variant of the ResNetv2 model.</span></span>
    - <span data-ttu-id="fe7af-280">`MetricsCallback` enlaza una función para realizar un seguimiento del progreso durante el entrenamiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-280">`MetricsCallback` binds a function to track the progress during training.</span></span>
    - <span data-ttu-id="fe7af-281">`TestOnTrainSet` indica al modelo que mida el rendimiento en el conjunto de entrenamiento cuando no haya ningún conjunto de validación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-281">`TestOnTrainSet` tells the model to measure performance against the training set when no validation set is present.</span></span>
    - <span data-ttu-id="fe7af-282">`ReuseTrainSetBottleneckCachedValues` indica al modelo si se deben usar los valores almacenados en caché de la fase de cuello de botella en las ejecuciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="fe7af-282">`ReuseTrainSetBottleneckCachedValues` tells the model whether to use the cached values from the bottleneck phase in subsequent runs.</span></span> <span data-ttu-id="fe7af-283">La fase de cuello de botella es un cálculo de paso a través único que es intensiva a nivel de computación la primera vez que se realiza.</span><span class="sxs-lookup"><span data-stu-id="fe7af-283">The bottleneck phase is a one-time pass-through computation that is computationally intensive the first time it is performed.</span></span> <span data-ttu-id="fe7af-284">Si los datos de entrenamiento no cambian y quiere experimentar con un número distinto de épocas o tamaño de lote, el uso de los valores almacenados en caché reduce considerablemente la cantidad de tiempo necesario para entrenar un modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-284">If the training data does not change and you want to experiment using a different number of epochs or batch size, using the cached values significantly reduces the amount of time required to train a model.</span></span>
    - <span data-ttu-id="fe7af-285">`ReuseValidationSetBottleneckCachedValues` es parecido a `ReuseTrainSetBottleneckCachedValues` solo que, en este caso, se usa para el conjunto de validación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-285">`ReuseValidationSetBottleneckCachedValues` is similar to `ReuseTrainSetBottleneckCachedValues` only that in this case it's for the validation dataset.</span></span>
    - <span data-ttu-id="fe7af-286">`WorkspacePath` define el directorio donde se almacenan los valores de cuello de botella calculados y la versión `.pb` del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-286">`WorkspacePath` defines the directory where to store the computed bottleneck values and `.pb` version of the model.</span></span>

1. <span data-ttu-id="fe7af-287">Defina la canalización de entrenamiento [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) que consta de `mapLabelEstimator` y `ImageClassificationTrainer`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-287">Define the [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) training pipeline that consists of both the `mapLabelEstimator` and the `ImageClassificationTrainer`.</span></span>

    [!code-csharp [TrainingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L59-L60)]

1. <span data-ttu-id="fe7af-288">Use el método [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit*) para entrenar el modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-288">Use the [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit*) method to train your model.</span></span>

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L62)]

## <a name="use-the-model"></a><span data-ttu-id="fe7af-289">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="fe7af-289">Use the model</span></span>

<span data-ttu-id="fe7af-290">Ahora que se ha entrenado el modelo, es el momento de usarlo para clasificar las imágenes.</span><span class="sxs-lookup"><span data-stu-id="fe7af-290">Now that you have trained your model, it's time to use it to classify images.</span></span>

<span data-ttu-id="fe7af-291">Debajo del método `Main`, cree un método de utilidad nuevo llamado `OutputPrediction` para mostrar información de predicción en la consola.</span><span class="sxs-lookup"><span data-stu-id="fe7af-291">Below the `Main` method, create a new utility method called `OutputPrediction` to display prediction information in the console.</span></span>

[!code-csharp [OuputPredictionMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L96-L100)]

### <a name="classify-a-single-image"></a><span data-ttu-id="fe7af-292">Clasificación de una sola imagen</span><span class="sxs-lookup"><span data-stu-id="fe7af-292">Classify a single image</span></span>

1. <span data-ttu-id="fe7af-293">Agregue un método nuevo llamado `ClassifySingleImage` debajo del método `Main` para crear y generar una única predicción de imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-293">Add a new method called `ClassifySingleImage` below the `Main` method to make and output a single image prediction.</span></span>

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. <span data-ttu-id="fe7af-294">Cree un elemento [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) en el método `ClassifySingleImage`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-294">Create a [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) inside the `ClassifySingleImage` method.</span></span> <span data-ttu-id="fe7af-295">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) es una API de conveniencia, que permite pasar datos y luego realizar una predicción en una única instancia de datos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-295">The [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to pass in and then perform a prediction on a single instance of data.</span></span>

    [!code-csharp [CreatePredictionEngine](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L73)]

1. <span data-ttu-id="fe7af-296">Para tener acceso a una única instancia de `ModelInput`, convierta [`IDataView`](xref:Microsoft.ML.IDataView) de `data` en un elemento [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) mediante el método [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) y, después, obtenga la primera observación.</span><span class="sxs-lookup"><span data-stu-id="fe7af-296">To access a single `ModelInput` instance, convert the `data` [`IDataView`](xref:Microsoft.ML.IDataView) into an [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) using the [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) method and then get the first observation.</span></span>

    [!code-csharp [GetTestInputData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L75)]

1. <span data-ttu-id="fe7af-297">Use el método [`Predict`](xref:Microsoft.ML.PredictionEngine%602.Predict*) para clasificar la imagen.</span><span class="sxs-lookup"><span data-stu-id="fe7af-297">Use the [`Predict`](xref:Microsoft.ML.PredictionEngine%602.Predict*) method to classify the image.</span></span>

    [!code-csharp [MakeSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L77)]

1. <span data-ttu-id="fe7af-298">Genere la predicción en la consola con el método `OutputPrediction`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-298">Output the prediction to the console with the `OutputPrediction` method.</span></span>

    [!code-csharp [OuputSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L79-L80)]

1. <span data-ttu-id="fe7af-299">En el método `Main`, llame a `ClassifySingleImage` mediante el conjunto de prueba de imágenes.</span><span class="sxs-lookup"><span data-stu-id="fe7af-299">Inside the `Main` method, call `ClassifySingleImage` using the test set of images.</span></span>

    [!code-csharp [ClassifySingleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L64)]

### <a name="classify-multiple-images"></a><span data-ttu-id="fe7af-300">Clasificación de varias imágenes</span><span class="sxs-lookup"><span data-stu-id="fe7af-300">Classify multiple images</span></span>

1. <span data-ttu-id="fe7af-301">Agregue un método nuevo llamado `ClassifyImages` debajo del método `ClassifySingleImage` para realizar y generar varias predicciones de imágenes.</span><span class="sxs-lookup"><span data-stu-id="fe7af-301">Add a new method called `ClassifyImages` below the `ClassifySingleImage` method to make and output multiple image predictions.</span></span>

    ```csharp
    public static void ClassifyImages(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. <span data-ttu-id="fe7af-302">Cree un elemento [`IDataView`](xref:Microsoft.ML.IDataView) que contenga las predicciones mediante el método [`Transform`](xref:Microsoft.ML.ITransformer.Transform*).</span><span class="sxs-lookup"><span data-stu-id="fe7af-302">Create an [`IDataView`](xref:Microsoft.ML.IDataView) containing the predictions by using the [`Transform`](xref:Microsoft.ML.ITransformer.Transform*) method.</span></span> <span data-ttu-id="fe7af-303">Agregue el código siguiente dentro del método `ClassifyImages`.</span><span class="sxs-lookup"><span data-stu-id="fe7af-303">Add the following code inside the `ClassifyImages` method.</span></span>

    [!code-csharp [MakeMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L85)]

1. <span data-ttu-id="fe7af-304">Para recorrer en iteración las predicciones, convierta [`IDataView`](xref:Microsoft.ML.IDataView) de `predictionData` en un elemento [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) con el método [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) y, después, obtenga las 10 primeras observaciones.</span><span class="sxs-lookup"><span data-stu-id="fe7af-304">In order to iterate over the predictions, convert the `predictionData` [`IDataView`](xref:Microsoft.ML.IDataView) into an [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) using the [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) method and then get the first 10 observations.</span></span>

    [!code-csharp [IEnumerablePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L87)]

1. <span data-ttu-id="fe7af-305">Recorra en iteración y genere las etiquetas originales y previstas de las predicciones.</span><span class="sxs-lookup"><span data-stu-id="fe7af-305">Iterate and output the original and predicted labels for the predictions.</span></span>

    [!code-csharp [OutputMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L89-L93)]

1. <span data-ttu-id="fe7af-306">Por último, en el método `Main`, llame a `ClassifyImages` mediante el conjunto de prueba de imágenes.</span><span class="sxs-lookup"><span data-stu-id="fe7af-306">Finally, inside the `Main` method, call `ClassifyImages` using the test set of images.</span></span>

    [!code-csharp [ClassifyImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification_Binary/Program.cs#L66)]

## <a name="run-the-application"></a><span data-ttu-id="fe7af-307">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="fe7af-307">Run the application</span></span>

<span data-ttu-id="fe7af-308">Ejecute la aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="fe7af-308">Run your console app.</span></span> <span data-ttu-id="fe7af-309">La salida debe ser similar a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="fe7af-309">The output should be similar to that below.</span></span> <span data-ttu-id="fe7af-310">Es posible que vea advertencias o mensajes de procesamiento, si bien se han quitado de los resultados siguientes para mayor claridad.</span><span class="sxs-lookup"><span data-stu-id="fe7af-310">You may see warnings or processing messages, but these messages have been removed from the following results for clarity.</span></span> <span data-ttu-id="fe7af-311">Por motivos de brevedad, la salida se ha resumido.</span><span class="sxs-lookup"><span data-stu-id="fe7af-311">For brevity, the output has been condensed.</span></span>

<span data-ttu-id="fe7af-312">**Fase de cuello de botella**</span><span class="sxs-lookup"><span data-stu-id="fe7af-312">**Bottleneck phase**</span></span>

<span data-ttu-id="fe7af-313">No se imprime ningún valor para el nombre de la imagen porque las imágenes se cargan como un elemento `byte[]` y, por lo tanto, no hay ningún nombre de imagen para mostrar.</span><span class="sxs-lookup"><span data-stu-id="fe7af-313">No value is printed for the image name because the images are loaded as a `byte[]` therefore there is no image name to display.</span></span>

```test
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 279
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 280
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   1
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   2
```

<span data-ttu-id="fe7af-314">**Fase de entrenamiento**</span><span class="sxs-lookup"><span data-stu-id="fe7af-314">**Training phase**</span></span>

```text
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  21, Accuracy:  0.6797619
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  22, Accuracy:  0.7642857
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  23, Accuracy:  0.7916667
```

<span data-ttu-id="fe7af-315">**Salida de clasificación de imágenes**</span><span class="sxs-lookup"><span data-stu-id="fe7af-315">**Classify images output**</span></span>

```text
Classifying single image
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD

Classifying multiple images
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-163.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-210.jpg | Actual Value: UD | Predicted Value: UD
```

<span data-ttu-id="fe7af-316">Al inspeccionar la imagen *7001-220.jpg*, puede ver que, de hecho, no tiene grietas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-316">Upon inspection of the *7001-220.jpg* image, you can see that it in fact is not cracked.</span></span>

![Imagen del conjunto de SDNET2018 que se usa para la predicción](./media/image-classification-api-transfer-learning/predictedimage.jpg)

<span data-ttu-id="fe7af-318">¡Enhorabuena!</span><span class="sxs-lookup"><span data-stu-id="fe7af-318">Congratulations!</span></span> <span data-ttu-id="fe7af-319">Ha compilado correctamente un modelo de aprendizaje profundo para la clasificación de imágenes.</span><span class="sxs-lookup"><span data-stu-id="fe7af-319">You've now successfully built a deep learning model for classifying images.</span></span>

### <a name="improve-the-model"></a><span data-ttu-id="fe7af-320">Mejora del modelo</span><span class="sxs-lookup"><span data-stu-id="fe7af-320">Improve the model</span></span>

<span data-ttu-id="fe7af-321">Si no está satisfecho con los resultados del modelo, puede intentar mejorar su rendimiento probando algunos de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="fe7af-321">If you're not satisfied with the results of your model, you can try to improve its performance by trying some of the following approaches:</span></span>

- <span data-ttu-id="fe7af-322">**Más datos**: cuanto mayor sea el número de ejemplos de los que pueda aprender un modelo, mejor funcionará.</span><span class="sxs-lookup"><span data-stu-id="fe7af-322">**More Data**: The more examples a model learns from, the better it performs.</span></span> <span data-ttu-id="fe7af-323">Descargue el [conjunto de datos SDNET2018](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?filename=2&article=1047&context=all_datasets&type=additional) completo y úselo para entrenar.</span><span class="sxs-lookup"><span data-stu-id="fe7af-323">Download the full [SDNET2018 dataset](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?filename=2&article=1047&context=all_datasets&type=additional) and use it to train.</span></span>
- <span data-ttu-id="fe7af-324">**Aumentar los datos**: Una técnica común para agregar diversidad a los datos es aumentarlos tomando una imagen y aplicando distintas transformaciones (girar, voltear, desplazar o recortar).</span><span class="sxs-lookup"><span data-stu-id="fe7af-324">**Augment the data**: A common technique to add variety to the data is to augment the data by taking an image and applying different transforms (rotate, flip, shift, crop).</span></span> <span data-ttu-id="fe7af-325">Esto agrega ejemplos más variados para que el modelo aprenda de ellos.</span><span class="sxs-lookup"><span data-stu-id="fe7af-325">This adds more varied examples for the model to learn from.</span></span>
- <span data-ttu-id="fe7af-326">**Entrenar durante más tiempo**: cuanto más tiempo se entrene, más ajustado estará el modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-326">**Train for a longer time**: The longer you train, the more tuned the model will be.</span></span> <span data-ttu-id="fe7af-327">Aumentar el número de épocas puede mejorar el rendimiento del modelo.</span><span class="sxs-lookup"><span data-stu-id="fe7af-327">Increasing the number of epochs may improve the performance of your model.</span></span>
- <span data-ttu-id="fe7af-328">**Experimentar con otros hiperparámetros**: además de los parámetros que se usan en este tutorial, se pueden ajustar otros parámetros para mejorar potencialmente el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-328">**Experiment with the hyper-parameters**: In addition to the parameters used in this tutorial, other parameters can be tuned to potentially improve performance.</span></span> <span data-ttu-id="fe7af-329">Cambiar la velocidad de aprendizaje, que determina el tamaño de las actualizaciones realizadas en el modelo después de cada época, puede mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="fe7af-329">Changing the learning rate, which determines the magnitude of updates made to the model after each epoch may improve performance.</span></span>
- <span data-ttu-id="fe7af-330">**Usar una arquitectura de modelo diferente**: en función de lo que parezcan los datos, puede variar el modelo que sea capaz de aprender mejor sus características.</span><span class="sxs-lookup"><span data-stu-id="fe7af-330">**Use a different model architecture**: Depending on what your data looks like, the model that can best learn its features may differ.</span></span> <span data-ttu-id="fe7af-331">Si no está satisfecho con el rendimiento del modelo, intente cambiar la arquitectura.</span><span class="sxs-lookup"><span data-stu-id="fe7af-331">If you're not satisfied with the performance of your model, try changing the architecture.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="fe7af-332">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fe7af-332">Additional Resources</span></span>

- <span data-ttu-id="fe7af-333">[Aprendizaje profundo frente a aprendizaje automático](/azure/machine-learning/service/concept-deep-learning-vs-machine-learning).</span><span class="sxs-lookup"><span data-stu-id="fe7af-333">[Deep Learning vs Machine Learning](/azure/machine-learning/service/concept-deep-learning-vs-machine-learning).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe7af-334">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="fe7af-334">Next steps</span></span>

<span data-ttu-id="fe7af-335">En este tutorial, se ha obtenido información sobre cómo compilar un modelo de aprendizaje profundo personalizado mediante el aprendizaje de transferencia, un modelo TensorFlow de clasificación de imágenes previamente entrenado y la API Image Classification de ML.NET para clasificar las imágenes de superficies de hormigón como con grietas o sin grietas.</span><span class="sxs-lookup"><span data-stu-id="fe7af-335">In this tutorial, you learned how to build a custom deep learning model using transfer learning, a pretrained image classification TensorFlow model and the ML.NET Image Classification API to classify images of concrete surfaces as cracked or uncracked.</span></span>

<span data-ttu-id="fe7af-336">Siga con el siguiente tutorial para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="fe7af-336">Advance to the next tutorial to learn more.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fe7af-337">Detección de objetos</span><span class="sxs-lookup"><span data-stu-id="fe7af-337">Object Detection</span></span>](object-detection-onnx.md)