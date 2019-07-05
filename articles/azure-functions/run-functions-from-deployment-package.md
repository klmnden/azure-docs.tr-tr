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
ms.date: 02/26/2019
ms.author: glenga
ms.openlocfilehash: 83a98a493068d3427e34f3ac2ca5c24baa48dda1
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508235"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Bir paket dosyasından, Azure işlevleri'ni çalıştırma

> [!NOTE]
> Bu makalede açıklanan işlevler, Linux üzerinde çalıştırılan işlev uygulamaları için kullanılabilir değil. bir [App Service planı](functions-scale.md#app-service-plan).

Azure'da işlevlerinizin işlev uygulamanızda doğrudan bir dağıtım paketi dosyasından çalıştırabilirsiniz. Dosyalarınızı dağıtmak için diğer seçenek olan `d:\home\site\wwwroot` işlev uygulamanızın dizin.

Bu makalede bir paketten işlevlerinizi çalıştıran avantajları anlatılmaktadır. Ayrıca, işlev uygulamanızda bu işlevi etkinleştirmek nasıl gösterir.

## <a name="benefits-of-running-from-a-package-file"></a>Bir paket dosyasından çalıştıran avantajları
  
Bir paket dosyasından çalıştırmanın çeşitli avantajları vardır:

+ Dosya kopyalama kilitlenmesi sorunları riskini azaltır.
+ Bir üretim uygulaması (yeniden Çalıştır ile) dağıtılabilir.
+ Belirli olabilir uygulamanızda çalışan dosyalar.
+ Performansını artırır [Azure Resource Manager dağıtımları](functions-infrastructure-as-code.md).
+ Özellikle büyük npm paket ağaçları ile JavaScript işlevleri için ilk kez azaltabilir.

Daha fazla bilgi için [Bu duyuru](https://github.com/Azure/app-service-announcements/issues/84).

## <a name="enabling-functions-to-run-from-a-package"></a>İşlevlerini bir paketten çalışacak şekilde etkinleştirme

Bir paketinden çalıştırılacak işlev uygulamanızı etkinleştirmek için az önce eklediğiniz bir `WEBSITE_RUN_FROM_PACKAGE` ayarlamak için işlev uygulaması ayarları. `WEBSITE_RUN_FROM_PACKAGE` Ayarı aşağıdaki değerlerden biri olabilir:

| Değer  | Açıklama  |
|---------|---------|
| **`1`**  | Windows üzerinde çalıştırılan işlev uygulamaları için önerilir. Paket dosyasından çalıştırın `d:\home\data\SitePackages` işlev uygulamanızın klasör. Aksi takdirde [zip ile dağıtmak](#integration-with-zip-deployment), bu seçenek adlı bir dosya de klasöre gerektirir `packagename.txt`. Bu dosya, yalnızca tüm boşluk olmadan klasöründeki paket dosya adını içerir. |
|**`<url>`**  | Çalıştırmak istediğiniz belirli paket dosyasının konumu. BLOB Depolama kullanırken, özel bir kapsayıcı ile kullanması gereken bir [paylaşılan erişim imzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) işlevler çalışma zamanı paketi erişmek etkinleştirmek için. Kullanabileceğiniz [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) paket dosyaları Blob Depolama hesabınıza yüklemek için.         |

> [!CAUTION]
> Bir işlev uygulaması, Windows üzerinde çalışırken, dış URL seçeneği daha da kötüsü soğuk başlangıç performansı verir. İşlev uygulamanız için Windows Dağıtım yaparken, ayarlamalısınız `WEBSITE_RUN_FROM_PACKAGE` için `1` ve zip dağıtımı ile yayımlayın.

Aşağıda, Azure Blob Depolama alanında barındırılan bir .zip dosyasından çalışacak şekilde yapılandırılmış bir işlev uygulaması gösterilmiştir:

![WEBSITE_RUN_FROM_ZIP uygulama ayarı](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Şu anda yalnızca .zip paketi dosyaları desteklenir.

## <a name="integration-with-zip-deployment"></a>Zip dağıtım ile tümleştirme

[Zip dağıtım][Zip deployment for Azure Functions] bir işlev uygulaması projenizi dağıtmanıza olanak tanır, Azure App Service özelliğidir `wwwroot` dizin. Proje, dağıtım .zip dosyası olarak paketlenir. API'leri paketinize dağıtmak için kullanılan `d:\home\data\SitePackages` klasör. İle `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarının değerini `1`, zip dağıtım API'leri kopyalama paketinize `d:\home\data\SitePackages` dosyasına ayıklama yerine klasörü `d:\home\site\wwwroot`. Ayrıca oluşturur `packagename.txt` dosya. İşlev uygulaması paketinden bir yeniden başlatma sonrasında çalıştırılan ve `wwwroot` salt okunur hale gelir. Zip dağıtımı hakkında daha fazla bilgi için bkz. [Zip Azure işlevleri için dağıtım](deployment-zip-push.md).

## <a name="adding-the-websiterunfrompackage-setting"></a>WEBSITE_RUN_FROM_PACKAGE ayarı ekleme

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

## <a name="troubleshooting"></a>Sorun giderme

- Paket çalıştırması yapar `wwwroot` salt okunur, dolayısıyla bu dizine dosya yazılırken bir hata alırsınız.
- Tar ve gzip biçimlerini desteklenmez.
- Bu özellik, yerel önbellek ile compose değil.
- Geliştirilmiş soğuk başlangıç performans için yerel posta seçeneğini kullanın (`WEBSITE_RUN_FROM_PACKAGE`= 1).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
