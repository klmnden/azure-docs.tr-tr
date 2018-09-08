---
title: Bir paket, Azure işlevleri'ni çalıştırma | Microsoft Docs
description: İşlev uygulaması proje dosyalarınızı içeren bir dağıtım paketi dosyası bağlayarak işlevlerinizin çalıştığı Azure işlevleri çalışma zamanı vardır.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/22/2018
ms.author: glenga
ms.openlocfilehash: a3c42dc0f90b16b14eca2c47608fb90238dd0f72
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095479"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Bir paket dosyasından, Azure işlevleri'ni çalıştırma

> [!NOTE]
> Bu makalede açıklanan işlevi şu anda Önizleme aşamasındadır; Linux üzerinde işlevler için kullanılabilir değil.

Azure'da işlevlerinizin işlev uygulamanızda doğrudan bir dağıtım paketi dosyasından çalıştırabilirsiniz. İşlev proje dosyalarınızı dağıtmak için diğer seçenek olan `d:\home\site\wwwroot` işlev uygulamanızın dizin.

Bu makalede bir paketten işlevlerinizi çalıştıran avantajları anlatılmaktadır. Ayrıca, işlev uygulamanızda bu işlevi etkinleştirmek nasıl gösterir.

## <a name="benefits-of-running-from-a-package-file"></a>Bir paket dosyasından çalıştıran avantajları
  
Bir paket dosyasından çalıştırmanın çeşitli avantajları vardır:

+ Dosya kopyalama kilitlenmesi sorunları riskini azaltır.
+ Bir üretim uygulaması (yeniden Çalıştır ile) dağıtılabilir.
+ Belirli olabilir uygulamanızda çalışan dosyalar.
+ Performansını artırır [Azure Resource Manager dağıtımları](functions-infrastructure-as-code.md).
+ JavaScript işlevleri soğuk başlangıç süresini azaltabilir.

Daha fazla bilgi için [Bu duyuru](https://github.com/Azure/app-service-announcements/issues/84).

## <a name="enabling-functions-to-run-from-a-package"></a>İşlevlerini bir paketten çalışacak şekilde etkinleştirme

Bir paketinden çalıştırılacak işlev uygulamanızı etkinleştirmek için az önce eklediğiniz bir `WEBSITE_RUN_FROM_ZIP` ayarlamak için işlev uygulaması ayarları. `WEBSITE_RUN_FROM_ZIP` Ayarı aşağıdaki değerlerden biri olabilir:

| Değer  | Açıklama  |
|---------|---------|
|**`<url>`**  | Çalıştırmak istediğiniz belirli paket dosyasının konumu. BLOB Depolama kullanırken, özel bir kapsayıcı ile kullanması gereken bir [paylaşılan erişim imzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-a-storage-account-by-using-a-shared-access-signature-sas) işlevler çalışma zamanı paketi erişmek etkinleştirmek için. Kullanabileceğiniz [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) paket dosyaları Blob Depolama hesabınıza yüklemek için.         |
| **`1`**  | Paket dosyasından çalıştırın `d:\home\data\SitePackages` işlev uygulamanızın klasör. Bu seçenek adlı bir dosya de klasöre gerektirir `packagename.txt`. Bu dosya, yalnızca tüm boşluk olmadan klasöründeki paket dosya adını içerir. |

Aşağıda, Azure Blob Depolama alanında barındırılan bir .zip dosyasından çalışacak şekilde yapılandırılmış bir işlev uygulaması gösterilmiştir:

![WEBSITE_RUN_FROM_ZIP uygulama ayarı](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Şu anda yalnızca .zip paketi dosyaları desteklenir.

## <a name="integration-with-zip-deployment"></a>Zip dağıtım ile tümleştirme

[Zip dağıtım] [ Zip deployment for Azure Functions] bir işlev uygulaması projenizi dağıtmanıza olanak tanır, Azure App Service özelliğidir `wwwroot` dizin. Proje, dağıtım .zip dosyası olarak paketlenir. API'leri paketinize dağıtmak için kullanılan `d:\home\data\SitePackages` klasör. İle `WEBSITE_RUN_FROM_ZIP` uygulama ayarının değerini `1`, zip dağıtım API'leri kopyalama paketinize `d:\home\data\SitePackages` dosyasına ayıklama yerine klasörü `d:\home\site\wwwroot`. Ayrıca oluşturur `packagename.txt` dosya. İşlev uygulaması paketinden bir yeniden başlatma sonrasında çalıştırılan ve `wwwroot` salt okunur hale gelir. Zip dağıtımı hakkında daha fazla bilgi için bkz. [Zip Azure işlevleri için dağıtım](deployment-zip-push.md).

## <a name="adding-the-websiterunfromzip-setting"></a>WEBSITE_RUN_FROM_ZIP ayarı ekleme

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
