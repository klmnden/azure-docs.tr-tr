---
title: Gerçek kullanıcı ölçümleri için Azure Traffic Manager ile Visual Studio Mobile Center | Microsoft Docs
description: Traffic Manager gerçek kullanıcı ölçümleri göndermek için Visual Studio Mobile Center'ı kullanarak geliştirilen mobil uygulamanızı ayarlama
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 1a5b883a8c9688d4545c0e98c00f78a2e982a611
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884062"
---
# <a name="how-to-send-real-user-measurements-to-traffic-manager-with-visual-studio-mobile-center"></a>Traffic Manager için Visual Studio Mobile Center ile gerçek kullanıcı ölçümleri gönderme

Visual Studio Mobile Center adımları izleyerek Traffic Manager'a gerçek kullanıcı ölçümleri gönderme için geliştirilen mobil uygulamanıza ayarlayabilirsiniz:

>[!NOTE]
> Şu anda Traffic Manager'a gerçek kullanıcı ölçümleri gönderme yalnızca Android için desteklenir.

Gerçek kullanıcı ölçümleri'ni yapılandırmak için bir anahtarı almak ve uygulamanızı ÇALIŞTIRMA paket izleme gerekir.

## <a name="step-1-obtain-a-key"></a>1. Adım: Bir anahtarı edinme
    
Olması ve Traffic Manager için istemci uygulamanızdan gönderilen ölçüleri gerçek kullanıcı ölçümleri (RUM) anahtarı adı benzersiz bir dize kullanarak hizmeti tarafından tanımlanır. Azure portalı, REST API kullanarak ÇALIŞTIRMA bir anahtar alabilirsiniz veya PowerShell'i kullanarak / CLI arabirimleri.

Aşağıdaki yordamı kullanarak, Azure portalını kullanarak RUM anahtarı almak için:
1. Azure portalında bir tarayıcıdan oturum açın. Zaten bir hesabınız yoksa, ücretsiz bir aylık deneme için kaydolabilirsiniz.
2. Portalın arama çubuğunda, değiştirmek istediğiniz Traffic Manager profil adı için arama yapın ve sonra sonuçları Traffic Manager profili seçin, görüntülenen.
3. Traffic Manager profili sayfasında tıklatın **gerçek kullanıcı ölçümleri** altında **ayarları**.
4. Tıklayın **anahtar oluştur** RUM yeni bir anahtar oluşturmak için.
        
   ![Gerçek kullanıcı ölçümleri anahtarı oluşturma](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Şekil 1: Gerçek kullanıcı ölçümleri anahtarı oluşturma**

5. Sayfa oluşturulan RUM anahtarı ve HTML sayfanıza eklenmesi gereken JavaScript kod parçacığını görüntüler.
 
   ![Gerçek kullanıcı ölçümleri anahtarı için JavaScript kodu](./media/traffic-manager-create-rum-visual-studio/rum-key.png)

   **Şekil 2: Gerçek kullanıcı ölçümleri anahtarı ve ölçüm JavaScript'i**
 
6. Tıklayın **kopyalama** düğmesini RUM anahtarı kopyalayın. 

## <a name="step-2-instrument-your-app-with-the-rum-package-of-mobile-center-sdk"></a>2. Adım: ÇALIŞTIRMA paketi Mobile Center SDK'sı ile uygulamanızı izleyin

Visual Studio Mobile Center'a yeniyseniz, ziyaret edin, [Web sitesi](https://mobile.azure.com). SDK tümleştirmesi hakkında ayrıntılı yönergeler için bkz: [Android SDK'sı ile çalışmaya başlama](https://docs.microsoft.com/mobile-center/sdk/getting-started/Android).

Gerçek kullanıcı ölçümleri kullanmak için aşağıdaki yordamı izleyin:

1.  SDK'yi projeye Ekle

    ATM RUM SDK Önizleme sırasında açıkça Paket Deposu başvurmanız gerekir.

    İçinde **app/build.gradle** dosyası aşağıdaki satırları ekleyin:

    ```groovy
    repositories {
        maven {
            url "https://dl.bintray.com/mobile-center/mobile-center-snapshot"
        }
    }
    ```
    İçinde **app/build.gradle** dosyası aşağıdaki satırları ekleyin:

    ```groovy
    dependencies {
     
        def mobileCenterSdkVersion = '0.12.1-16+3fe5b08'
        compile "com.microsoft.azure.mobile:mobile-center-rum:${mobileCenterSdkVersion}"
    }
    ```

2. SDK'yı başlatın

    Uygulamanızın ana etkinlik sınıfını açın ve aşağıdaki import deyimlerini ekleyin:

    ```java
    import com.microsoft.azure.mobile.MobileCenter;
    import com.microsoft.azure.mobile.rum.RealUserMeasurements;
    ```

    Aranacak `onCreate` geri çağırma aynı dosya ve aşağıdaki kodu ekleyin:

    ```java
    RealUserMeasurements.setRumKey("<Your RUM Key>");
    MobileCenter.start(getApplication(), "<Your Mobile Center AppSecret>", RealUserMeasurements.class);
    ```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [gerçek kullanıcı ölçümleri](traffic-manager-rum-overview.md)
- Bilgi [Traffic Manager nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinin [Mobile Center](https://docs.microsoft.com/mobile-center/)
- [Kaydolun](https://mobile.azure.com) Mobile Center
- Daha fazla bilgi edinin [trafik yönlendirme yöntemlerini](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

