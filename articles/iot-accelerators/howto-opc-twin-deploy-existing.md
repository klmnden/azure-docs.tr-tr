---
title: Mevcut bir Azure projesine bir OPC İkizi modülü dağıtma | Microsoft Docs
description: Mevcut bir projeyi OPC İkizi dağıtma
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 5e3be8f0c565f86ab5332730972e0ed960d22255
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603732"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>Mevcut bir projeyi OPC İkizi dağıtma

OPC İkizi modülü, IOT Edge üzerinde çalışır ve birkaç uç hizmetlerinin OPC İkizi ve kayıt defteri hizmetleri sağlar.

OPC İkizi mikro hizmet, Fabrika işleçler ve OPC UA sunucu cihazlarınıza fabrika düzeyinde bir OPC İkizi IOT Edge modülü aracılığıyla arasındaki iletişimi kolaylaştırır. Mikro hizmet, REST API aracılığıyla OPC UA Hizmetleri (göz atma, okuma, yazma ve yürütme) kullanıma sunar. 

OPC UA cihaz kayıt defteri mikro hizmet kayıtlı OPC UA uygulamalar ve uç noktalarını erişim sağlar. Operatörler ve yöneticiler kaydedebilir ve OPC UA yeni uygulama kaydı ve uç noktaları da dahil olmak üzere varolanları göz atın. Uygulama ve uç nokta yönetim ek olarak, kayıt defteri hizmeti ayrıca kayıtlı OPC İkizi IOT Edge modülleri kataloglar. Hizmet API'si size edge modül işlevselliği, örneğin, başlatma veya sunucu bulma (Tarama Hizmetleri) durdurma veya OPC İkizi mikro hizmet kullanılarak erişilebilir yeni uç nokta çiftleri etkinleştirme denetimi.

Bir çekirdek modülünün gözetmen kimliktir. Gözetmen, karşılık gelen OPC UA kayıt API'si kullanılarak etkinleştirilen OPC UA sunucu uç noktaları için karşılık gelen uç nokta ikizi yönetir. Bu uç nokta çiftleri OPC UA durum bilgisi olan güvenli bir kanal üzerinden yönetilen uç noktasına gönderilen OPC UA ikili iletileri içinde bulutta çalışan OPC İkizi mikro hizmetten alınan JSON çevir. Gözetmen, cihaz bulma olayları işlemek, burada bu olayları OPC UA kayıt defterine güncelleştirmesi sonucu OPC UA cihaz ekleme Hizmeti'ne gönderme bulma hizmetleri de sağlar.  Bu makalede, var olan bir projeye OPC İkizi modülü dağıtmayı gösterir.

> [!NOTE]
> Dağıtım ayrıntıları ve yönergeleri hakkında daha fazla bilgi için bkz. GitHub [depo](https://github.com/Azure/azure-iiot-opc-twin-module).

## <a name="prerequisites"></a>Önkoşullar

PowerShell sahip olduğunuzdan emin olun ve [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) yüklü uzantıları. Henüz bunu yapmadıysanız, bu GitHub deposunu kopyalayın. PowerShell'de aşağıdaki komutları çalıştırın:

```powershell
git clone --recursive https://github.com/Azure/azure-iiot-components.git
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Endüstriyel IOT Hizmetleri Azure'da dağıtma

1. PowerShell oturumunuzda çalıştırın:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy.cmd
    ```

2. Web sitesine dağıtım kaynak grubu için bir ad ve bir ad atamak için yönergeleri izleyin.   Betik, mikro hizmetler ve Azure platformu bağımlılıklarını Azure aboneliğinizdeki kaynak grubuna dağıtır.  Betik, bir uygulama OAUTH tabanlı kimlik doğrulamasını desteklemek için Azure Active Directory (AAD) kiracısında de kaydeder.  Dağıtım birkaç dakika sürer.  Çözüm başarıyla dağıtıldıktan sonra görmek bir örnek:

   ![Var olan bir projeye endüstriyel IOT OPC İkizi dağıtma](media/howto-opc-twin-deploy-existing/opc-twin-deploy-existing1.png)

   Çıktı, genel bir uç nokta URL'sini içerir. 

3. Betik başarıyla tamamlandıktan sonra .env dosyasında kaydetmek isteyip istemediğinizi seçin.  Konsolu gibi araçları kullanarak bulut uç noktasına bağlanın veya geliştirme ve hata ayıklama için modüllerini dağıtmak istiyorsanız .env ortam dosyası gerekir.

## <a name="troubleshooting-deployment-failures"></a>Dağıtım sorunlarını giderme

### <a name="resource-group-name"></a>Kaynak grubu adı

Bir kısa ve basit bir kaynak grubu adı kullandığınızdan emin olun.  Ad, uyması gereken ad kaynaklarla şekilde kaynak adlandırma gereksinimlerini için de kullanılır.  

### <a name="website-name-already-in-use"></a>Web sitesi adı zaten kullanımda

Web sitesinin adı zaten kullanımda olduğunu mümkündür.  Bu hatayla çalıştırırsanız, farklı bir uygulama adı kullanmanız gerekir.

### <a name="azure-active-directory-aad-registration"></a>Azure Active Directory (AAD) kaydı

Dağıtım betiği iki AAD uygulaması Azure Active Directory'ye kaydetmeniz dener.  Seçilen AAD kiracısı haklarınızı bağlı olarak, dağıtım başarısız olabilir. İki seçenek vardır:

1. Kiracılar listesinden bir AAD kiracısı seçtiyseniz, betik yeniden başlatın ve farklı bir listeden seçin.
2. Alternatif olarak, özel bir AAD kiracısında başka bir abonelik dağıtma, betiği yeniden başlat ve kullanılacağını seçin.

> [!WARNING]
> Hiçbir zaman kimlik doğrulaması olmadan devam edin.  Bunu yapmayı tercih ederseniz, herkesin kimliği doğrulanmamış Internet'ten, OPC İkizi Uç noktalara erişebilir.   Her zaman seçebilirsiniz ["yerel" dağıtım seçeneği](howto-opc-twin-deploy-dependencies.md) incelemek için.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Bir hepsi bir arada endüstriyel IOT Hizmetleri tanıtım dağıtma

Yalnızca hizmetlerini ve bağımlılıklarını yerine bir hepsi bir arada Tanıtımı da dağıtabilirsiniz.  Bir tanıtım, tüm üç OPC UA sunucuları, OPC İkizi modülü, tüm mikro hizmetler ve örnek bir Web uygulaması içerir.  Bu tanıtım amacıyla tasarlanmıştır.

1. (Yukarıya bakın) deponun bir kopyasını sağlayın. Çalıştırma ve depo kök dizininde bir PowerShell istemi açın:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy -type demo
    ```

2. Kaynak grubunu ve Web sitesi için bir ad için yeni bir ad atamak için istemleri izleyin.  Betik başarıyla dağıtıldıktan sonra web uygulama uç noktası URL'si görüntüler.

## <a name="deployment-script-options"></a>Dağıtım betiği seçenekleri

Betiği aşağıdaki parametreleri alır:

```powershell
-type
```

Dağıtım (yerel, vm, demo) türü

```powershell
-resourceGroupName
```

Var olan veya yeni bir kaynak grubu adı olabilir.

```powershell
-subscriptionId
```

İsteğe bağlı, kaynakların dağıtılacağı abonelik kimliği.

```powershell
-subscriptionName
```

Veya abonelik adı.

```powershell
-resourceGroupLocation
```

İsteğe bağlı, bir kaynak grubu konumu. Belirtilmişse, bu konumda yeni bir kaynak grubu oluşturmak çalışacaktır.

```powershell
-aadApplicationName
```

Altında kaydetmek üzere AAD uygulaması için bir ad.

```powershell
-tenantId
```

AAD kiracısı'kullanılacak.

```powershell
-credentials
```

## <a name="next-steps"></a>Sonraki adımlar

Mevcut bir projeyi OPC İkizi dağıtmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC istemci ve OPC PLC güvenli iletişim](howto-opc-vault-deploy-existing-client-plc-communication.md)
