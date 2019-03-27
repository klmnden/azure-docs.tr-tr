---
title: Azure IOT cihaz yönetimi ile Azure IOT araçları Visual Studio Code için | Microsoft Docs
description: Azure IOT araçları, Visual Studio Code için doğrudan yöntemleri ve İkizinin istenen özellikleri yönetim seçenekleri Azure IOT Hub cihaz yönetimi için kullanın.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: 03df2ceb2df4d857e48f1790703a1d87647e43d0
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445272"
---
# <a name="use-azure-iot-tools-for-visual-studio-code-for-azure-iot-hub-device-management"></a>Azure IOT araçları Visual Studio Code için Azure IOT Hub cihaz yönetimi için kullanın.

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) , IOT hub'ı Yönetim ve IOT uygulama geliştirmeyi kolaylaştırır kullanışlı bir Visual Studio Code uzantısı. Bu, çeşitli görevleri gerçekleştirmek için kullanabileceğiniz yönetim seçenekleri ile birlikte gelir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev                    |
|----------------------------|--------------------------------|
| Doğrudan yöntemler             | Başlatma veya ileti göndermek ya da cihazın yeniden başlatılması durdurma gibi davranacak bir cihaz olun.                                        |
| Cihaz ikizi okuma           | Bir cihaz bildirilen durumunu alın. Örneğin, cihaz LED artık yanıp sönen bildirir.                                    |
| Cihaz ikizi güncelleştir         | Bir cihaz için yeşil bir LED ayarlama veya telemetri gönderme aralığı 30 dakika gibi bazı durumların yerleştirin.         |
| Bulut-cihaz iletilerini   | Bir cihaz için bildirimleri gönderin. Örneğin, "Bugün Yağmur olasılığı çok sağlıyor. Bir terimdir getirmeyi unutmayın."              |

Daha ayrıntılı açıklama farklılıklarla ilgili ve bu seçenekleri kullanma yönergeleri için bkz. [CİHAZDAN buluta iletişim Kılavuzu](iot-hub-devguide-d2c-guidance.md) ve [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md).

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT hub'ı ona bağlanan her cihaz için cihaz ikizi'ni kalıcıdır. Cihaz ikizleri hakkında daha fazla bilgi için bkz: [cihaz ikizlerini kullanmaya başlama](iot-hub-node-node-twin-getstarted.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-you-learn"></a>Öğrenecekleriniz

Geliştirme makinenizde çeşitli yönetim seçenekleri ile Visual Studio Code için Azure IOT araçları kullanarak bilgi edinin.

## <a name="what-you-do"></a>Neler

Azure IOT araçları, Visual Studio Code için çeşitli yönetim seçenekler ile Çalıştır.

## <a name="what-you-need"></a>Ne gerekiyor

* Etkin bir Azure aboneliği.
* Azure IOT hub, aboneliğiniz altında.
* [Visual Studio Code](https://code.visualstudio.com/)
* [VS Code için Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) veya [Visual Studio Code'da bu bağlantının açılabilmesi](vscode:extension/vsciot-vscode.azure-iot-tools).

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

1. İçinde **Gezgini** görüntülemek VS Code, genişletme **Azure IOT Hub cihazları** sol alt köşedeki bölümünde.

2. Tıklayın **IOT Hub'ı seçin** bağlam menüsünde.

3. Bir açılır pencere, Azure'da ilk kez oturum açarken izin vermek için sağ alt köşedeki gösterilir.

4. Oturum açtıktan sonra Azure abonelik listesi gösterilir ve ardından Azure aboneliği ve IOT hub'ı seçin.

5. Cihaz listesinde gösterilecek **Azure IOT Hub cihazları** birkaç saniye içinde sekmesi.

   > [!Note]
   > Ayrıca, ayarlamayı tamamlamak için **IoT Hub Bağlantı Dizesini Ayarla**'yı seçebilirsiniz. Açılır pencerede bağlanır IOT Cihazınızı IOT hub'ının bağlantı dizesini girin.

## <a name="direct-methods"></a>Doğrudan yöntemler

1. Cihazınızı sağ tıklayıp **doğrudan yöntem çağırma**. 

2. Yük ve yöntem adı giriş kutusuna girin.

3. Sonuçları gösterilecek **çıkış** > **Azure IOT hub'ı Araç Seti** görünümü.

## <a name="read-device-twin"></a>Cihaz ikizi okuma

1. Cihazınızı sağ tıklayıp **cihaz ikizini Düzenle**. 

2. Bir **azure-IOT-cihaz-twin.json** dosya cihaz ikizi içeriğini açılacak.

## <a name="update-device-twin"></a>Cihaz ikizi güncelleştir

1. Bazı düzenlemeler, **etiketleri** veya **properties.desired** alan.

2. Sağ **azure-IOT-cihaz-twin.json** dosya.

3. Seçin **güncelleştirme cihaz İkizi** cihaz ikizi güncelleştirilemedi.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

Cihazınız için IOT hub'ınızdan ileti göndermek için bu adımları izleyin:
 
1. Cihazınızı sağ tıklayıp **cihaza C2D iletisi gönder**. 

2. İleti giriş kutusuna girin.

3. Sonuçları gösterilecek **çıkış** > **Azure IOT hub'ı Araç Seti** görünümü.

## <a name="next-steps"></a>Sonraki adımlar

Çeşitli yönetim seçenekleri ile Visual Studio Code için Azure IOT araçları uzantısını kullanmayı öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
