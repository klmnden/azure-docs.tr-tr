---
title: 'Hızlı Başlangıç: Java (Android) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Speech SDK'sı kullanarak Android üzerinde Java Konuşma tanımayı öğrenmesine
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: 690656449fdb86c200a8978f0e17db562e4abbca
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59009187"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak Android'de Java konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Android Bilişsel hizmetler konuşma SDK'sı kullanmaya yönelik bir Java uygulaması geliştirme öğreneceksiniz.
Uygulama, konuşma SDK Maven paketini, sürüm 1.4.0 ve Android Studio 3.3 temel alır.
Konuşma SDK’sı şu anda 32/64 bit ARM işlemcilerine sahip Android cihazlarıyla ve Intel x86/x64 uyumlu işlemcilerle uyumludur.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için bir konuşma Hizmetleri abonelik anahtarı ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Bkz: [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) Ayrıntılar için.

## <a name="create-and-configure-a-project"></a>Projeyi oluşturma ve yapılandırma

1. Android Studio’yu başlatın ve Hoş Geldiniz penceresinde **Yeni bir Android Studio projesi başlat**’ı seçin.

    ![Android Studio Hoş Geldiniz penceresinin ekran görüntüsü](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. **Projenizi seçin** Sihirbazı görüntülenirse, seçin **telefon ve Tablet** ve **boş etkinlik** etkinlik seçimi kutusunda. **İleri**’yi seçin.

   ![Ekran görüntüsü Proje Sihirbazı'nı seçin](media/sdk/qs-java-android-02-target-android-devices.png)

1. İçinde **Nakonfigurovat projekt** ekranında, girin **hızlı** olarak **adı**, **samples.speech.cognitiveservices.microsoft.com** olarak **Paket adı**, proje dizini seçin. İçin **en düşük API düzeyi** çekme **API 23: Android 6.0 (Marshmallow)**, diğer tüm onay kutularının işaretli ve select bırakın **son**.

   ![Proje Sihirbazı'nı yapılandırma ekran görüntüsü](media/sdk/qs-java-android-03-create-android-project.png)

Android Studio’nun yeni Android projenizi hazırlaması biraz zaman alır. Daha sonra, projenizi yapılandırarak Konuşma SDK’sı hakkında bilgi edinmesini ve Java 8’i kullanmasını sağlayın.

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.4.0`.

Android Speech SDK'sı olarak paketlenmiş bir [AAR (Android kitaplık)](https://developer.android.com/studio/projects/android-library), gerekli kitaplıkları ve Android gerekli izinleri içerir.
Https Maven deponun barındırılan:\//csspeechstorage.blob.core.windows.net/maven/.

Konuşma SDK’sını kullanmak için projenizi ayarlayın. Android Studio menü çubuğundan **Dosya** > **Proje Yapısı**’nı seçerek Proje Yapısı penceresini açın. Proje Yapısı penceresinde aşağıdaki değişiklikleri yapın:

1. Pencerenin sol tarafındaki listede **Proje**’yi seçin. **Varsayılan Kitaplık Deposu** ayarlarını, bir virgül ve tek tırnak içine alınan Maven deposu URL'sini ekleyerek düzenleyin. ' https:\//csspeechstorage.blob.core.windows.net/maven/'

   ![Proje Yapısı penceresinin ekran görüntüsü](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Aynı ekranın sol tarafında **uygulamayı** seçin. Sonra da, pencerenin en üstünde **Bağımlılıklar** sekmesini seçin. Yeşil artı işaretini (+) ve açılan menüden **Kitaplık bağımlılığı**’nı seçin.

   ![Proje Yapısı penceresinin ekran görüntüsü](media/sdk/qs-java-android-07-add-module-dependency.png)

1. Açılan pencerede Android için Konuşma SDK’mızın adını ve sürümünü (`com.microsoft.cognitiveservices.speech:client-sdk:1.4.0`) girin. Sonra **Tamam**’ı seçin.
   Artık, aşağıda gösterildiği gibi Konuşma SDK’sı bağımlılıklar listesine eklenmiş olmalıdır:

   ![Proje Yapısı penceresinin ekran görüntüsü](media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. **Özellikler** sekmesini seçin. Hem **Kaynak Uyumluluğu** hem de **Hedef Uyumluluğu** için **1.8**’i seçin.

   ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Proje Yapısı penceresini kapatmak ve değişikliklerinizi projeye uygulamak için **Tamam**’ı seçin.

## <a name="create-user-interface"></a>Kullanıcı arabirimi oluşturma

Uygulama için temel bir kullanıcı arabirimi oluşturacağız. Ana etkinliğiniz `activity_main.xml` için düzende değişiklik yapın. Başlangıçta, bir başlık çubuğu, uygulamanızın adı ve "Hello World!" metni içeren bir TextView düzeni içerir.

* TextView öğesine tıklayın. Sağ üst köşedeki ID özniteliğini `hello` olarak değiştirin.

* `activity_main.xml` penceresinin sol üst tarafındaki Paletten bir düğmeyi metnin üst kısmındaki boş alana sürükleyin.

* Sağ taraftaki düğme özniteliklerde, `onClick` özniteliği değeri için `onSpeechButtonClicked` girin. Düğme olayını işlemek için bu adla bir yöntem yazacağız.  Sağ üst köşedeki ID özniteliğini `button` olarak değiştirin.

* Tasarımcının en üstündeki sihirli değnek simgesini kullanarak düzen kısıtlamalarını ortaya çıkarın.

  ![Sihirli değnek simgesinin ekran görüntüsü](media/sdk/qs-java-android-10-infer-layout-constraints.png)

Metin ve grafik temsilini kullanıcı Arabirimi artık şöyle görünmelidir:

![](media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `MainActivity.java` kaynak dosyasını açın. Bu dosyanın içindeki kodun tamamını aşağıdakiyle değiştirin.

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * `onCreate` yönteminde mikrofon ve İnternet izinleri isteyen ve yerel platform bağlaması başlatan kod vardır. Yerel platform bağlamaları tek bir kez yapılandırılmalıdır. Bu işlem uygulama başlatma sırasında erken bir aşamada yapılmalıdır.

   * `onSpeechButtonClicked` yöntemi daha önce de belirtildiği gibi düğme tıklama işleyicisidir. Düğmeye basılması, konuşmayı metne dönüştürme transkripsiyonunu tetikler.

1. Aynı dosyada, `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Android cihazınızı geliştirme bilgisayarınıza bağlayın. Cihazda [geliştirme modunu ve USB hata ayıklamasını](https://developer.android.com/studio/debug/dev-options) etkinleştirdiğinizden emin olun.

1. Uygulamayı derlemek için Ctrl+F9 tuşlarına basın veya menü çubuğundan **Derle** > **Proje Yap**'ı seçin.

1. Uygulamayı başlatmak için Shift+F10 tuşlarına basın veya **Çalıştır** > **'uygulama' çalıştır**'ı seçin.

1. Görüntülenen dağıtım hedefi penceresinde Android cihazınızı seçin.

   ![Dağıtım Hedefi Seç penceresinin ekran görüntüsü](media/sdk/qs-java-android-12-deploy.png)

Konuşma tanıma bölümünü başlatmak için uygulamada düğmeye basın. İngilizce konuşma sonraki 15 saniye Konuşma hizmetlerine gönderilen ve transcribed. Sonuç, Android uygulamasında ve Android Studio'daki logcat penceresinde gösterilir.

![Android uygulamasının ekran görüntüsü](media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde Java örnekleri keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
