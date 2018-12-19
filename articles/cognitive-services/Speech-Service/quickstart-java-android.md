---
title: 'Hızlı Başlangıç: Java (Android) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Konuşma Tanıma Hizmeti SDK'sını kullanarak Android üzerinde Java dilinde konuşma tanımayı öğrenin
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: wolfma
ms.openlocfilehash: 6d245b457eca78dc029bde923616b4d84e04b997
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599032"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak Android'de Java konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Android Bilişsel hizmetler konuşma SDK'sı kullanmaya yönelik bir Java uygulaması geliştirme öğreneceksiniz.
Uygulama, Microsoft Bilişsel hizmetler konuşma SDK Maven paketini, sürümü 1.2.0 olarak güncelleştirilir ve Android Studio 3.1 temel alır.
Konuşma SDK’sı şu anda 32/64 bit ARM işlemcilerine sahip Android cihazlarıyla ve Intel x86/x64 uyumlu işlemcilerle uyumludur.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Bu Hızlı Başlangıcı tamamlamak için bir Konuşma hizmeti abonelik anahtarınız olması gerekir. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [Konuşma hizmetini ücretsiz olarak deneme](get-started.md).

## <a name="create-and-configure-a-project"></a>Projeyi oluşturma ve yapılandırma

1. Android Studio’yu başlatın ve Hoş Geldiniz penceresinde **Yeni bir Android Studio projesi başlat**’ı seçin.

    ![Android Studio Hoş Geldiniz penceresinin ekran görüntüsü](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. **Yeni Proje Oluştur** sihirbazı görüntülenir. **Android Projesi Oluştur** ekranında **uygulama adı** olarak **Quickstart**, **şirket etki alanı** olarak da **samples.speech.cognitiveservices.microsoft.com** girin ve bir proje dizini seçin. C++ ve Kotlin onay kutularını boş bırakın ve **İleri**’yi seçin.

   ![Yeni Proje Oluştur sihirbazının ekran görüntüsü](media/sdk/qs-java-android-02-create-android-project.png)

1. **Hedef Android Cihazları** ekranında yalnızca **Telefon ve Tablet**’i seçin. Altındaki aşağı açılan listesinde seçin **API 23: Android 6.0 (Marshmallow)** seçip **sonraki**.

   ![Yeni Proje Oluştur sihirbazının ekran görüntüsü](media/sdk/qs-java-android-03-target-android-devices.png)

1. **Mobil Cihaza Etkinlik Ekle** ekranında **Boş Etkinlik**’i seçin ve **İleri**’ye tıklayın.

   ![Yeni Proje Oluştur sihirbazının ekran görüntüsü](media/sdk/qs-java-android-04-add-an-activity-to-mobile.png)

1. **Etkinlik Yapılandır** ekranında etkinlik adı olarak **MainActivity**’yi, düzen adı olarak da **activity\_main**’i kullanın. Her iki onay kutusunu da işaretleyin ve **Son**’u seçin.

   ![Yeni Proje Oluştur sihirbazının ekran görüntüsü](media/sdk/qs-java-android-05-configure-activity.png)

Android Studio’nun yeni Android projenizi hazırlaması biraz zaman alır. Daha sonra, projenizi yapılandırarak Konuşma SDK’sı hakkında bilgi edinmesini ve Java 8’i kullanmasını sağlayın.

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.2.0`.

Android Speech SDK'sı olarak paketlenmiş bir [AAR (Android kitaplık)](https://developer.android.com/studio/projects/android-library), gerekli kitaplıkları ve Android gerekli izinleri içerir.
https://csspeechstorage.blob.core.windows.net/maven/ adresindeki Maven deposunda barındırılır.

Konuşma SDK’sını kullanmak için projenizi ayarlayın. Android Studio menü çubuğundan **Dosya** > **Proje Yapısı**’nı seçerek Proje Yapısı penceresini açın. Proje Yapısı penceresinde aşağıdaki değişiklikleri yapın:

1. Pencerenin sol tarafındaki listede **Proje**’yi seçin. **Varsayılan Kitaplık Deposu** ayarlarını, bir virgül ve tek tırnak içine alınan Maven deposu URL'sini ekleyerek düzenleyin. 'https://csspeechstorage.blob.core.windows.net/maven/'

   ![Proje Yapısı penceresinin ekran görüntüsü](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Aynı ekranın sol tarafında **uygulamayı** seçin. Sonra da, pencerenin en üstünde **Bağımlılıklar** sekmesini seçin. Yeşil artı işaretini (+) ve açılan menüden **Kitaplık bağımlılığı**’nı seçin.

   ![Proje Yapısı penceresinin ekran görüntüsü](media/sdk/qs-java-android-07-add-module-dependency.png)

1. Açılan pencerede Android için Konuşma SDK’mızın adını ve sürümünü (`com.microsoft.cognitiveservices.speech:client-sdk:1.2.0`) girin. Sonra **Tamam**’ı seçin.
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

<table>
<tr>
<td valign="top">
![](media/sdk/qs-java-android-11-gui.png)
</td>
<td valign="top">
[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]
</td>
</tr>
</table>

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

Konuşma tanıma bölümünü başlatmak için uygulamada düğmeye basın. Bunu izleyen 15 saniyelik İngilizce konuşma Konuşma hizmetine gönderilir ve transkripsiyonu yapılır. Sonuç, Android uygulamasında ve Android Studio'daki logcat penceresinde gösterilir.

![Android uygulamasının ekran görüntüsü](media/sdk/qs-java-android-13-gui-on-device.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/java-android` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Java için Konuşma SDK’sını kullanarak konuşmadaki amacı tanıma](how-to-recognize-intents-from-speech-java.md)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
