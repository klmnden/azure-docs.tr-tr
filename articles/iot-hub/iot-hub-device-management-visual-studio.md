---
title: Azure IOT cihaz yönetimini Cloud Explorer ile Visual Studio | Microsoft Docs
description: Cloud Explorer doğrudan yöntemler ve İkizinin istenen özellikleri yönetim seçenekleri Azure IOT Hub cihaz yönetimi için Visual Studio için kullanın.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: xshi
ms.openlocfilehash: 87a0847f5d42e014f3b2691c96446892176b481b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60399581"
---
# <a name="use-cloud-explorer-for-visual-studio-for-azure-iot-hub-device-management"></a>Cloud Explorer Visual Studio için Azure IOT Hub cihaz yönetimi için kullanın.

![Uçtan uca diyagramı](media/iot-hub-device-management-visual-studio/iot-e2e-simple.png)

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) Azure kaynaklarınızı görüntüleyin, özelliklerini inceleme ve Visual Studio temel Geliştirici eylemler gerçekleştirmek olanak tanıyan kullanışlı bir Visual Studio uzantısıdır. Bu, çeşitli görevleri gerçekleştirmek için kullanabileceğiniz yönetim seçenekleri ile birlikte gelir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev                    |
|----------------------------|--------------------------------|
| Doğrudan yöntemler             | Başlatma veya ileti göndermek ya da cihazın yeniden başlatılması durdurma gibi davranacak bir cihaz olun.                                        |
| Cihaz ikizi okuma           | Bir cihaz bildirilen durumunu alın. Örneğin, cihaz LED artık yanıp sönen bildirir.                                    |
| Cihaz ikizi güncelleştir         | Bir cihaz için yeşil bir LED ayarlama veya telemetri gönderme aralığı 30 dakika gibi bazı durumların yerleştirin.         |
| Bulut-cihaz iletilerini   | Bir cihaz için bildirimleri gönderin. Örneğin, "Bugün Yağmur olasılığı çok sağlıyor. Bir terimdir getirmeyi unutmayın."              |

Daha ayrıntılı açıklama farklılıklarla ilgili ve bu seçenekleri kullanma yönergeleri için bkz. [CİHAZDAN buluta iletişim Kılavuzu](iot-hub-devguide-d2c-guidance.md) ve [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md).

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT hub'ı ona bağlanan her cihaz için cihaz ikizi'ni kalıcıdır. Cihaz ikizleri hakkında daha fazla bilgi için bkz: [cihaz ikizlerini kullanmaya başlama](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Öğrenecekleriniz

Cloud Explorer ile çeşitli yönetim seçenekleri geliştirme makinenizde Visual Studio için kullanmayı öğrenin.

## <a name="what-you-do"></a>Neler

Cloud Explorer, Visual Studio için çeşitli yönetim seçenekler ile Çalıştır.

## <a name="what-you-need"></a>Ne gerekiyor

- Etkin bir Azure aboneliği
- Azure IOT Hub, aboneliğiniz altında
- Microsoft Visual Studio 2017 güncelleştirme 8 veya üzeri
- Cloud Explorer bileşeni (Azure iş yükü ile varsayılan olarak seçilidir) Visual Studio Yükleyicisi'nden

## <a name="update-cloud-explorer-to-latest-version"></a>Cloud Explorer'ı en son sürüme güncelleştirme

Visual Studio Yükleyicisi'nden Cloud Explorer bileşen yalnızca cihaz-Bulut ve bulut-cihaz iletilerini izlenmesini de destekler. En son indirip ihtiyacınız [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) yönetim seçeneklerine erişin.

## <a name="sign-in-to-access-your-iot-hub"></a>IOT Hub'ınıza erişmek için oturum açın

1. Visual Studio'da **Cloud Explorer** penceresinde hesap yönetimi simgesine tıklayın. Cloud Explorer penceresini açabilir **görünümü** > **Cloud Explorer** menüsü.

    ![Hesap Yönetimi](media/iot-hub-visual-studio-cloud-device-messaging/click-account-management.png)

1. Tıklayın **hesaplarını yönetme** bulut Gezgini'nde.
1. Tıklayın **Hesap Ekle...**  Azure'a ilk kez oturum açmak için yeni pencerede.
1. Oturum açtıktan sonra Azure abonelik listesi gösterilir. Tıklayın ve görüntülemek istediğiniz Azure aboneliklerini seçin **Uygula**.
1. Genişletin **aboneliğinizi** > **IOT hub'ları** > **uygulamanızın IOT hub'ı**, cihaz listesi, IOT hub'ı düğümünde gösterilir. Bir cihaz yönetim seçenekleri erişmek için sağ tıklayın.

    ![Yönetim seçenekleri](media/iot-hub-device-management-visual-studio/management-options.png)

## <a name="direct-methods"></a>Doğrudan yöntemler

1. Cihazınızı sağ tıklayıp **cihaz doğrudan yöntem çağırma**.
1. Yük ve yöntem adı giriş kutusuna girin.
1. Sonuçları gösterilecek **IOT hub'ı** çıkış bölmesi.

## <a name="read-device-twin"></a>Cihaz ikizi okuma

1. Cihazınızı sağ tıklayıp **cihaz ikizini Düzenle**.
1. Bir **azure-IOT-cihaz-twin.json** dosya cihaz ikizi içeriğini açılacak.

## <a name="update-device-twin"></a>Cihaz ikizi güncelleştir

1. Bazı düzenlemeler, **etiketleri** veya **properties.desired** alanı **azure-IOT-cihaz-twin.json** dosya.
1. Tuşuna **Ctrl + S** cihaz ikizi güncelleştirilemedi.
1. Sonuçları gösterilecek **IOT hub'ı** çıkış bölmesi.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

Cihazınız için IOT hub'ınızdan ileti göndermek için bu adımları izleyin:

1. Cihazınızı sağ tıklayıp **C2D iletisi gönder**.
1. İleti giriş kutusuna girin.
1. Sonuçları gösterilecek **IOT hub'ı** çıkış bölmesi.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio için Cloud Explorer çeşitli yönetim seçeneklerini kullanmayı öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]