---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 5/23/2019
ms.author: travisw
ms.openlocfilehash: a8118d80e85d562fa4137ed1f1844e6bf9f1793e
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485047"
---
1. Android Studio’yu başlatın ve Hoş Geldiniz penceresinde **Yeni bir Android Studio projesi başlat**’ı seçin.

    ![Android Studio Hoş Geldiniz penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. **Projenizi seçin** Sihirbazı görüntülenirse, seçin **telefon ve Tablet** ve **boş etkinlik** etkinlik seçimi kutusunda. **İleri**’yi seçin.

   ![Ekran görüntüsü Proje Sihirbazı'nı seçin](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-02-target-android-devices.png)

1. İçinde **Nakonfigurovat projekt** ekranında, girin **hızlı** olarak **adı**, **samples.speech.cognitiveservices.microsoft.com** olarak **Paket adı**, proje dizini seçin. İçin **en düşük API düzeyi** çekme **API 23: Android 6.0 (Marshmallow)** , diğer tüm onay kutularının işaretli ve select bırakın **son**.

   ![Proje Sihirbazı'nı yapılandırma ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-03-create-android-project.png)

Android Studio’nun yeni Android projenizi hazırlaması biraz zaman alır. Daha sonra, projenizi yapılandırarak Konuşma SDK’sı hakkında bilgi edinmesini ve Java 8’i kullanmasını sağlayın.

[!INCLUDE [License Notice](cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.6.0`.

Android Speech SDK'sı olarak paketlenmiş bir [AAR (Android kitaplık)](https://developer.android.com/studio/projects/android-library), gerekli kitaplıkları ve Android gerekli izinleri içerir.
Https Maven deponun barındırılan:\//csspeechstorage.blob.core.windows.net/maven/.

Konuşma SDK’sını kullanmak için projenizi ayarlayın. Android Studio menü çubuğundan **Dosya** > **Proje Yapısı**’nı seçerek Proje Yapısı penceresini açın. Proje Yapısı penceresinde aşağıdaki değişiklikleri yapın:

1. Pencerenin sol tarafındaki listede **Proje**’yi seçin. **Varsayılan Kitaplık Deposu** ayarlarını, bir virgül ve tek tırnak içine alınan Maven deposu URL'sini ekleyerek düzenleyin. ' https:\//csspeechstorage.blob.core.windows.net/maven/'

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-06-add-maven-repository.png)

1. Aynı ekranın sol tarafında **uygulamayı** seçin. Sonra da, pencerenin en üstünde **Bağımlılıklar** sekmesini seçin. Yeşil artı işaretini (+) ve açılan menüden **Kitaplık bağımlılığı**’nı seçin.

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-07-add-module-dependency.png)

1. Açılan pencerede Android için Konuşma SDK’mızın adını ve sürümünü (`com.microsoft.cognitiveservices.speech:client-sdk:1.6.0`) girin. Sonra **Tamam**’ı seçin.
   Artık, aşağıda gösterildiği gibi Konuşma SDK’sı bağımlılıklar listesine eklenmiş olmalıdır:

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. **Özellikler** sekmesini seçin. Hem **Kaynak Uyumluluğu** hem de **Hedef Uyumluluğu** için **1.8**’i seçin.

   ![](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-09-dependency-added.png)

1. Proje Yapısı penceresini kapatmak ve değişikliklerinizi projeye uygulamak için **Tamam**’ı seçin.
