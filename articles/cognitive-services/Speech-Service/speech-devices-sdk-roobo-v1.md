---
title: Konuşma cihaz SDK'sı Roobo akıllı ses Dev Seti v1 - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Önkoşullar ve konuşma cihaz SDK'sı ile Roobo akıllı ses Dev Seti v1 başlamak için yönergeler.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: e4aba26238a0d87c8e708ae27c7b2dbdb73f16ab
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604844"
---
# <a name="device-roobo-smart-audio-dev-kit"></a>Cihaz: Roobo akıllı ses Dev Seti

Bu makalede, cihaz Roobo akıllı ses Geliştirme Seti için belirli bilgiler sağlar.

## <a name="set-up-the-development-kit"></a>Geliştirme Seti ' ayarlayın

1. Geliştirme Seti iki mikro USB bağlayıcı vardır. Sol bağlayıcının Geliştirme Seti desteklemek için ve güç aşağıdaki resimde vurgulanmıştır. Bunu kontrol edecek ve işaretlenmiş sağ görüntüde hata ayıklayın.

    ![dev Seti bağlanma](media/speech-devices-sdk/qsg-1.png)

1. Bir bilgisayar için güç bağlantı bağlanın veya bağdaştırıcısı güç mikro USB kablosu kullanarak Geliştirme Seti güçlendirin. Yeşil power göstergesi üst panonun altında görüntülenir.

1. Geliştirme Seti denetlemek için ikinci bir mikro USB kablosu kullanarak hata ayıklama bağlantı bir bilgisayara bağlanın. Güvenli iletişimler sağlamak için yüksek kaliteli kablo kullanmak için gereklidir.

1. Uygulamanızı Geliştirme Seti için döngüsel ya da doğrusal yapılandırma yönlendirmek.

    |Geliştirme Seti yapılandırma|Hizalama|
    |-----------------------------|------------|
    |Döngüsel|Şekilde, tavan mikrofonlar ile yan yana|
    |Doğrusal|(Aşağıdaki görüntüde gösterilmiştir) alt tarafında, mikrofon ile karşılaşmış|

    ![Doğrusal dev Seti yönü](media/speech-devices-sdk/qsg-2.png)

1. Sertifikaları yükleme ve ses cihazı izinlerini ayarlayın. Bir komut istemi penceresinde aşağıdaki komutları yazın:

   ```powershell
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Android hata ayıklama köprüsü bu komutları kullanmak `adb.exe`, Android Studio yüklemesinin bir parçası olduğu. Bu araç C:\Users bulunur\[kullanıcı adı] \AppData\Local\Android\Sdk\platform araçları. Bu dizin çağırmak daha kullanışlı hale getirmek için yola ekleyebilirsiniz `adb`. Aksi takdirde, yüklemenizin adb.exe çağıran her komut için tam yolunu belirtmeniz gerekir `adb`.
    >
    > Bir hata görürseniz `no devices/emulators found` sonra USB kablosu bağlı ve yüksek kaliteli kablo kontrol edin. Kullanabileceğiniz `adb devices` cihazların bir listesini döndürür gibi geliştirme setine bilgisayarınızı geçebildiğinden emin denetlemek için.
    >
    > [!TIP]
    > Bilgisayarınızın mikrofon ve Geliştirme Seti'nın mikrofonlar ile çalıştığından emin olmak Konuşmacı sessiz. Böylece, yanlışlıkla ses bilgisayardan cihazla tetiklemez.

1. Konuşmacı geliştirme setine eklemek istiyorsanız, ses satırına bağlanabilirsiniz. 3,5 mm analog Tak ile kaliteli Konuşmacı seçmeniz gerekir.

    ![Vysor ses](media/speech-devices-sdk/qsg-14.png)

## <a name="development-information"></a>Geliştirme bilgileri

Daha fazla geliştirme için bilgi [Roobo geliştirme Kılavuzu](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

## <a name="audio"></a>Ses

Roobo bellek Flash tüm ses yakalayan bir araç sağlar. Ses sorunları gidermenize yardımcı olabilir. Aracı sürümü her Geliştirme Seti yapılandırması için sağlanır. Üzerinde [Roobo site](https://ddk.roobo.com/)Cihazınızı seçin ve ardından **Roobo Araçları** sayfanın alt kısmındaki bağlantı.

## <a name="next-steps"></a>Sonraki adımlar

* [Android örnek uygulamayı çalıştırma](speech-devices-sdk-android-quickstart.md)
