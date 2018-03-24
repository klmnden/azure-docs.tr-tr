---
title: Gerçek kullanıcı ölçümler için Azure Traffic Manager ile Visual Studio mobil Merkezi | Microsoft Docs
description: Trafik Yöneticisi için gerçek kullanıcı ölçümleri göndermek için Visual Studio mobil merkezi kullanılarak geliştirilen mobil uygulamanızı ayarlama
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 893e84b07b365fb0b534e0ddc021b2249c4174cf
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-send-real-user-measurements-to-traffic-manager-with-visual-studio-mobile-center"></a>Visual Studio mobil Center'la trafik Yöneticisi için gerçek kullanıcı ölçümleri gönderme

Aşağıdaki adımları izleyerek trafik Yöneticisi için gerçek kullanıcı ölçümleri göndermek için Visual Studio mobil merkezi kullanılarak geliştirilen mobil uygulamanız ayarlayabilirsiniz:

>[!NOTE]
> Şu anda, trafik Yöneticisi için gerçek kullanıcı ölçümleri gönderme yalnızca Android için desteklenir.

Gerçek kullanıcı ölçümleri yapılandırmak için bir anahtarı edinin ve uygulamanızı RUM paket ile işaretleme gerekir.

## <a name="step-1-obtain-a-key"></a>1. adım: bir anahtarı edinme
    
Alın ve trafik Yöneticisi için istemci uygulamasından gönderilen ölçümleri gerçek kullanıcı ölçümleri (RUM) anahtarı adı benzersiz bir dize kullanarak hizmeti tarafından tanımlanır. Azure portal, REST API kullanarak RUM bir anahtarı elde edebilirsiniz veya PowerShell kullanarak / CLI arabirimleri.

Aşağıdaki yordamı kullanarak Azure portalını kullanarak RUM anahtarı edinmek için:
   1. Bir tarayıcı üzerinden Azure portalında oturum açın. Zaten bir hesabınız yoksa, ücretsiz bir aylık deneme için kaydolabilirsiniz.
   2. Değiştirmek istediğiniz trafik Yöneticisi profili adı portal'ın arama çubuğunda aramak ve sonuçları trafik Yöneticisi profili ardından, görüntülenen.
   3. Trafik Yöneticisi profili sayfasını tıklatın **gerçek kullanıcı ölçümleri** altında **ayarları**.
   4. Tıklatın **oluşturmak anahtar** yeni RUM anahtarı oluşturmak için.
        
   ![Gerçek kullanıcı ölçümleri anahtarı oluşturma](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Şekil 1: Gerçek kullanıcı ölçümleri anahtar oluşturma**

   5.   Sayfa oluşturulan RUM anahtarı ve HTML sayfanıza gömülü olması gereken bir JavaScript kod parçacığı görüntüler.
 
   ![Gerçek kullanıcı ölçümleri anahtarı için JavaScript kodu](./media/traffic-manager-create-rum-visual-studio/rum-key.png)

   **Şekil 2: Gerçek kullanıcı ölçümleri anahtarı ve ölçüm JavaScript**
 
   6. Tıklatın **kopyalama** düğmesi RUM anahtarı kopyalayın. 

## <a name="step-2-instrument-your-app-with-the-rum-package-of-mobile-center-sdk"></a>2. adım: uygulamanızı Mobile Merkezi SDK'sının RUM paket ile işaretleme

Visual Studio mobil merkezine yeni varsa, [Web sitesi](https://mobile.azure.com). SDK tümleştirmesi hakkında ayrıntılı yönergeler için bkz: [Android SDK'sı ile çalışmaya başlama](https://docs.microsoft.com/mobile-center/sdk/getting-started/Android).

Gerçek kullanıcı ölçümleri kullanmak için aşağıdaki yordamı izleyin:

1.  Projeye SDK'sı ekleme

    ATM RUM SDK'sı Önizleme süresince açıkça Paket Deposu başvurmanız gerekir.

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

2. SDK'yı başlatma

    Uygulamanızın ana etkinlik sınıfıyla açın ve aşağıdaki içeri aktarma deyimlerini ekleyin:

    ```java
    import com.microsoft.azure.mobile.MobileCenter;
    import com.microsoft.azure.mobile.rum.RealUserMeasurements;
    ```

    Ara `onCreate` geri çağırma aynı dosya ve aşağıdaki kodu ekleyin:

    ```java
    RealUserMeasurements.setRumKey("<Your RUM Key>");
    MobileCenter.start(getApplication(), "<Your Mobile Center AppSecret>", RealUserMeasurements.class);
    ```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [gerçek kullanıcı ölçümleri](traffic-manager-rum-overview.md)
- Bilgi [trafik Yöneticisi nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinmek [Mobile Merkezi](https://docs.microsoft.com/mobile-center/)
- [Kaydolun](https://mobile.azure.com) Mobile Merkezi
- Daha fazla bilgi edinmek [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [bir Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

