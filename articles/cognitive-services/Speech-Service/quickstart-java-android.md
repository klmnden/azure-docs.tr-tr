---
title: "Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sını kullanarak Android'de Java konuşma tanıma | Microsoft Docs"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sı Android kullanarak Java'da Konuşma tanımayı öğrenmesine
services: cognitive-services
author: fmegen
manager: wolfma
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: fmegen
ms.openlocfilehash: 6ee54f1405d94235784831d63e1abe87a495953b
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072599"
---
# <a name="quickstart-recognize-speech-in-java-on-android-using-the-speech-sdk"></a>Hızlı Başlangıç: Java konuşma Speech SDK'sı kullanarak Android'de tanıması

Bu makalede, Konuşmayı metne dönüştürme özelliği Android Bilişsel hizmetler konuşma SDK'sı kullanmaya yönelik bir Java uygulamasının nasıl oluşturulacağını öğreneceksiniz.
Uygulama, Microsoft Bilişsel hizmetler konuşma SDK Maven paketini, 0.5.0 sürümünü ve Android Studio 3.1 temel alır.

> [!NOTE]
> Konuşma cihaz SDK'sı ve Roobo aygıt için lütfen [konuşma cihaz SDK'sı](speech-devices-sdk.md) sayfası.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir bilgisayar (Windows, Linux, Mac) Android Studio çalıştırma yeteneği.
* 3.1 sürümünü [Android Studio](https://developer.android.com/studio/).
* ARM tabanlı bir Android cihazı (API 23: Android 6.0 Marshmallow veya üzeri) [geliştirme için etkin](https://developer.android.com/studio/debug/dev-options) çalışma mikrofon ile.

## <a name="create-an-android-studio-project"></a>Bir Android Studio projesi oluşturma

Android Studio'yu başlatın ve seçin **yeni bir Android Studio projesi Başlat**.

![](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

İçinde **yeni proje oluştur** Sihirbazı gelen yedekleme, aşağıdaki seçimleri yapın:

1. İçinde **Android projesi oluşturma** ekranında, girin **hızlı** olarak **uygulama adı**, **samples.speech.cognitiveservices.microsoft.com** olarak **şirket etki alanı**, proje konumu seçin. Onay kutularını işaretsiz bırakın ve tıklayın **sonraki**.

   ![](media/sdk/qs-java-android-02-create-android-project.png)

1. İçinde **hedef Android cihazları** onay ekranında **telefon ve Tablet** tek seçenek seçin **API 23: Android 6.0 (Marshmallow)** ve tıklatın altındaki açılır listeden **Sonraki**.

   ![](media/sdk/qs-java-android-03-target-android-devices.png)

1. İçinde **bir etkinlik eklemek için mobil** ekranındayken **boş etkinlik** tıklatıp **sonraki**.

   ![](media/sdk/qs-java-android-04-add-an-activity-to-mobile.png)

1. İçinde **yapılandırma etkinlik** ekranında, kullanmak **MainActivity** etkinlik adı olarak ve **etkinlik\_ana** Düzen adı. Her iki onay kutularını işaretleyin ve tıklayın **son**.

   ![](media/sdk/qs-java-android-05-configure-activity.png)

Bir süredir çalıştırdıktan sonra yeni oluşturulan Android Studio projenizin gelmelidir.

## <a name="configure-your-project-for-the-speech-sdk"></a>Projenizi Speech SDK'sı için yapılandırın

[!include[License Notice](includes/license-notice.md)]

Bilişsel hizmetler konuşma SDK'ın geçerli sürümü `0.5.0`.

Android Speech SDK'sı olarak paketlenmiş bir [AAR (Android kitaplık)](https://developer.android.com/studio/projects/android-library), gerekli kitaplıkların yanı sıra kullanım Android gerekli izinleri içerir.
Maven deponun barındırılan https://csspeechstorage.blob.core.windows.net/maven/.

Biz projenizi konuşma SDK'sını ayarlama aşağıda açıklanmaktadır.

Proje yapısı penceresini altında **dosya** \> **Proje yapısı**.
Aşağıdaki değişiklikler yapma gelen penceresinde (tıklayın **Tamam** yalnızca tüm adımları tamamladıktan sonra):

1. Seçin **proje**ve düzenleme **varsayılan kitaplık depo** virgül ve tek tırnak içinde Maven deposu URL'si ekleyerek ayarları `'https://csspeechstorage.blob.core.windows.net/maven/'`:

  ![](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Sol tarafta, aynı ekranda hala seçin **uygulama** modülü ve sağ üst **bağımlılıkları** sekmesi. Ardından sağ üst köşedeki yeşil artı işaretine tıklayın ve seçin **kitaplığı bağımlılığının**.

  ![](media/sdk/qs-java-android-07-add-module-dependency.png)

1. Android için ortaya çıkan penceresine bizim Speech SDK'sı sürümünü ve adını girin `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0`, ardından **Tamam**.
   Speech SDK'sı bağımlılıklar listesine artık, aşağıda gösterildiği gibi eklenmelidir:

  ![](media/sdk/qs-java-android-08-dependency-added.png)

1. Üst köşesindeki **özellikleri** sekmesi. Seçin **1.8** hem **kaynağı Uyumluluk** ve **hedef Uyumluluk**.

  ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Son olarak, tıklayın **Tamam** kapatmak için **Proje yapısı** windows ve tüm güncelleştirmeleri uygulayın.

## <a name="create-a-minimal-ui"></a>En az bir kullanıcı Arabirimi oluşturma

Uygulamanızın ana etkinlik için Düzen `activity_main.xml`.
Varsayılan olarak, uygulamanızın adı ve 'Hello World!' ifadesini içeren bir TextView bir başlık çubuğuyla gündeme.

* Üzerinde TextView'ı tıklatın. Kendi ID özniteliği için sağ üst köşedeki değiştirme `hello`.

* Sol üst köşedeki paletinden, `activity_main.xml` penceresinde yukarıda metni boş alan bir düğme sürükleyin.

* Düğmenin öznitelikler için değer sağdaki `onClick` özniteliği, girin `onSpeechButtonClicked`, bizim düğmesi işleyicisi adı olacaktır.
  Kendi ID özniteliği için sağ üst köşedeki değiştirme `button`.

* Düzeni kısıtlamaları çıkarsanacak istiyorsanız magic Değnek simgesi Tasarımcı üst kısmında kullanın.

  ![](media/sdk/qs-java-android-10-infer-layout-constraints.png)

Metin ve grafik kullanıcı Arabirimi sürümü şimdi şuna benzer görünmelidir:

<table>
<tr>
<td valign="top">
![](media/sdk/qs-java-android-11-gui.png)
</td>
<td valign="top">
[! kod xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]
</td>
</tr>
</table>

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. Düzen `MainActivity.java` kaynak dosyası ve kendi kod (Aşağıda, paket Ekstrenizi) aşağıdakilerle değiştirin:

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * `onCreate` Yöntem, mikrofon ve Internet izinleri istekleri hem de yerel platform bağlama başlatır kodu içerir. Yerel platform bağlamaları yapılandırma olan yalnızca bir kez gerekli, diğer bir deyişle, erken uygulama başlatma sırasında yapılması gerekir.
   
   * Yöntem `onSpeechButtonClicked` düğmesine gibi işleyicisi önceden ayarlama kablolu. Bir düğme basma gerçek konuşma tanıma tetikler.

1. Dize değiştirin `YourSubscriptionKey` abonelik.

1. Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

* Derleme, Ctrl + F9 tuşuna basın veya için **derleme** \> **olun proje**.

* Android Cihazınızı geliştirme için PC bağlayın. Olduğundan emin olun [geliştirme modu ve USB hata ayıklama etkin](https://developer.android.com/studio/debug/dev-options).

* Uygulamayı açmak için Shift + F10 tuşuna basın veya **çalıştırma** \> **'uygulamayı' Çalıştır**.

* Ortaya çıkan dağıtım hedef windows, Android Cihazınızı seçin.

  ![INTO hata ayıklaması uygulamayı başlatın](media/sdk/qs-java-android-12-deploy.png)

* Uygulamanın, Cihazınızda belirmelidir.
  Düğmesine basın, sonra sonraki 15 saniye tanınan ve arabiriminde (logcat pencerenizde Android Studio'da yanıtı görmek de):

  ![Başarılı tanıma sonra kullanıcı Arabirimi](media/sdk/qs-java-android-13-gui-on-device.png)

Bu ekran Android hızlı başlangıç, burada sona eriyor. Tam proje örnek kod örnekleri depodan indirilebilir.

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/java-android` klasör.

## <a name="next-steps"></a>Sonraki adımlar

* Ziyaret [örnekleri sayfası](samples.md) ek örnekler için.
