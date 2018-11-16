---
title: 'Öğretici: Android SDK ile görüntüdeki yüzleri algılama ve çerçeveleme'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, algılamak ve bir görüntüdeki yüzleri çerçeve için yüz tanıma API'sini kullanan basit bir Android uygulaması oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 11/12/2018
ms.author: pafarley
ms.openlocfilehash: 4378d04d8909ecb0cd77c3196b74ecd51eb19638
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686237"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Öğretici: Resimdeki yüzleri algılamak ve çerçeve içine almak için Android uygulaması oluşturma

Bu öğreticide, bir resimdeki İnsan yüzlerini algılamak için Java SDK'sı aracılığıyla Azure yüz tanıma API'sini kullanan basit bir Android uygulaması oluşturacaksınız. Uygulama, seçilen görüntü görüntüler ve algılanan her yüz etrafında bir çerçeve çizer.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - Android uygulaması oluşturma
> - Yüz tanıma API'si istemci Kitaplığı'nı yükleyin
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme

![Yüzleri kırmızı dikdörtgenle çerçeve içine alınmış bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted2.1.PNG)

Tam örnek kodu [Bilişsel hizmetler yüz tanıma Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) github deposu.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- Yüz tanıma API'si abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.
- [Android Studio](https://developer.android.com/studio/) API düzeyi 22 veya sonraki bir sürümü ile (yüz istemci kitaplığının gerektirdiği).

## <a name="create-the-android-studio-project"></a>Android Studio projesi oluşturma

Yeni bir Android uygulaması projesi oluşturmak için aşağıdaki adımları izleyin.

1. Android Studio'da **yeni bir Android Studio projesi Başlat**.
1. **Android Projesi Oluştur** ekranında, gerekirse varsayılan alanları değiştirin ve sonra da **İleri**'ye tıklayın.
1. Üzerinde **hedef Android cihazları** ekranında, seçmek için açılır Seçici kullanın **API 22** veya daha sonra ardından **sonraki**.
1. **Boş Etkinlik** öğesini seçin ve **İleri**'ye tıklayın.
1. **Geriye Dönük Uyumluluk** öğesinin işaretini kaldırın ve **Son**'a tıklayın.

## <a name="add-the-initial-code"></a>Başlangıç kodunu ekleme

### <a name="create-the-ui"></a>Kullanıcı Arabirimi oluşturma

Açık *activity_main.xml*. Düzen Düzenleyicisi'ndeki **metin** sekmesine ve ardından içeriğini aşağıdaki kodla değiştirin.

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/res/layout/activity_main.xml?range=1-18)]

### <a name="create-the-main-class"></a>Ana sınıfı oluşturma

Açık *MainActivity.java* ve varolan `import` aşağıdaki kodla deyimleri.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=3-11)]

Ardından, içeriği değiştirin **MainActivity** aşağıdaki kodla sınıfı. Bu olay işleyicisi oluşturur **düğmesi** bir resim seçmek izin vermek için yeni bir etkinlik başlar. Resim görüntüler **ImageView**.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=29-68)]

### <a name="try-the-app"></a>Uygulamayı deneyin

Çağrı yorum **detectAndFrame** içinde **onActivityResult** yöntemi. Tuşuna basarak **çalıştırma** menüsünde uygulamanızı test edin. Bir öykünücü veya bağlı bir cihaz, uygulama açıldığında tıklayın **Gözat** alt. Cihazın dosya seçme iletişim kutusu görüntülenmelidir. Bir görüntü seçin ve pencerenin görüntülendiğini doğrulayın. Ardından, sonraki adıma ilerleyin ve uygulamayı kapatın.

![Yüzler içeren bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted1.1.PNG)

## <a name="add-the-face-sdk"></a>Yüz tanıma SDK'sı ekleyin

### <a name="add-the-gradle-dependency"></a>Gradle bağımlılık Ekle

**Proje** bölmesinde, aşağı açılan seçiciyi kullanarak **Android**'i seçin. **Gradle Scripts**'i genişletin, sonra *build.gradle (Modül: uygulama)* öğesini açın. Aşağıdaki ekran görüntüsünde gösterildiği gibi Yüz Tanıma istemci kitaplığı `com.microsoft.projectoxford:face:1.4.3` için bir bağımlılık ekleyin, sonra da **Şimdi Eşitle**'ye tıklayın.

![Uygulama build.gradle dosyasının Android Studio ekran görüntüsü](../Images/face-tut-java-gradle.png)

### <a name="add-the-face-related-project-code"></a>Yüz tanıma ile ilgili proje kodunu ekleyin

Geri Git **MainActivity.java** ve aşağıdakileri ekleyin `import` ifadeleri:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=13-14)]

Sonra aşağıdaki kodu ekleyin **MainActivity** sınıfı, yukarıdaki **onCreate** yöntemi:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=17-27)]

Değiştirmeniz gerekecektir `<Subscription Key>` abonelik. Ayrıca, değiştirin `<API endpoint>` ile anahtarınız için uygun bir bölge tanımlayıcısı kullanılarak, yüz tanıma API'si uç noktası. Ücretsiz deneme aboneliği anahtarları oluşturulur **westus** bölge. Örnek API uç noktası değeri şu şekilde olur:

```java
apiEndpoint = "https://westus.api.cognitive.microsoft.com/face/v1.0";
```

**Proje** bölmesinde **uygulamayı**, sonra da **bildirimleri** genişletin ve *AndroidManifest.xml* dosyasını açın. Aşağıdaki öğeyi `manifest` öğesinin doğrudan alt öğesi olarak ekleyin:

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/AndroidManifest.xml?range=5)]

## <a name="upload-image-and-detect-faces"></a>Görüntü karşıya yükleme ve yüz algılama

Uygulamanızı çağırarak yüzleri algılar **FaceServiceClient.detect** sarmalar yöntemi [Algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) REST API ve bir listesini döndürür **yüz** örnekleri.

Her döndürülen **yüz** isteğe bağlı yüz öznitelikleri bir dizi ile birlikte konumunu belirtmek için bir dikdörtgen içerir. Bu örnekte, yalnızca yüz dikdörtgenler istenir.

Aşağıdaki iki yöntemden içine Ekle **MainActivity** sınıfı. Yüz algılama tamamlandığında uygulamayı çağırır Not **drawFaceRectanglesOnBitmap** değiştirmek için yöntem **ImageView**. Bu yöntem sonraki tanımlayacaksınız.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=70-150)]

## <a name="draw-face-rectangles"></a>Yüz tanıma dikdörtgenler çizme

Aşağıdaki yardımcı yöntemini içine Ekle **MainActivity** sınıfı. Bu yöntem her dikdörtgen koordinatlar kullanarak algılanan her yüz tanıma, çevresinde bir dikdörtgen çizer **yüz** örneği.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?range=152-173)]

Son olarak, çağrı açıklamadan çıkarın **detectAndFrame** yönteminde **onActivityResult**.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüzlerin yer aldığı bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Her bir görüntüdeki yüzleri kırmızı bir dikdörtgen görmeniz gerekir.

![Çizilen etrafında kırmızı dikdörtgenler ile yüzleri Android ekran görüntüsü](../Images/android_getstarted2.1.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel işlemi, yüz tanıma API'si Java SDK'sını kullanma hakkında bilgi edindiniz ve algılamak ve bir görüntüdeki yüzleri çerçeve için bir uygulama oluşturdunuz. Ardından, yüz algılama ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)