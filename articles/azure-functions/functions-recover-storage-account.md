---
title: Azure işlevleri çalışma zamanı sorunlarını giderme erişilemiyor.
description: Geçersiz depolama hesabı sorunlarını gidermeyi öğrenin.
services: functions
documentationcenter: ''
author: alexkarcher-msft
manager: cfowler
editor: ''
ms.service: azure-functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 8fca59eeea415581cbfb340c1e5932b1e5113814
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439218"
---
# <a name="how-to-troubleshoot-functions-runtime-is-unreachable"></a>"İşlevler çalışma zamanı erişilemiyor" sorunlarını giderme


## <a name="error-text"></a>Hata metni
Bu belge, İşlevler portalında görüntülendiğinde aşağıdaki hatayı gidermek için tasarlanmıştır.

`Error: Azure Functions Runtime is unreachable. Click here for details on storage configuration`

### <a name="summary"></a>Özet
Bu sorun, Azure işlevleri çalışma zamanı başlatılamıyor oluşur. Gerçekleşmesi bu hatanın en yaygın nedeni, kendi depolama hesabınıza erişim hakkını kaybetmesini işlev uygulamasının aynısıdır. [Depolama hesabı gereksinimleri hakkında daha fazla bilgi](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements)

### <a name="troubleshooting"></a>Sorun giderme
Dört en yaygın hata durumları, belirleme ve nasıl çözümleneceğini her durumda aracılığıyla alacağız.

1. Depolama hesabı silindi
1. Depolama hesabı uygulama ayarlarını silindi
1. Depolama hesabı kimlik bilgileri geçersiz
1. Depolama hesabına erişilemiyor
1. Günlük yürütme kotası tam

## <a name="storage-account-deleted"></a>Depolama hesabı silindi

Her işlev uygulaması çalışması için bir depolama hesabı gerektirir. Bu hesabı silinirse, işlevinizi çalışmaz.

### <a name="how-to-find-your-storage-account"></a>Depolama hesabınızı bulma

Depolama hesabı adınızı uygulama ayarlarınızı bakarak başlayın. Ya da `AzureWebJobsStorage` veya `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` bağlantı dizesinde paketleniyor depolama hesabınızın adını içerir. Detaylara konumunda okuma [burada uygulama ayarı başvurusu](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)

Hala varolup olmadığını görmek için Azure portalındaki depolama hesabınız için arama yapın. Silinmiş olması durumunda, bir depolama hesabını yeniden oluşturun ve depolama bağlantı dizelerinizi değiştirmek gerekir. İşlev kodunuzu kaybedilir ve tekrar tekrar dağıtmanız gerekecektir.

## <a name="storage-account-application-settings-deleted"></a>Depolama hesabı uygulama ayarlarını silindi

Bir depolama hesabı bağlantı dizesi yoksa önceki adımda, silinmiş veya üzerine büyük olasılıkla. Uygulama ayarları silme uygulama ayarlarını belirlemek için dağıtım yuvaları veya Azure Resource Manager betikleri kullanırken en yaygın olarak yapılır.

### <a name="required-application-settings"></a>Gerekli uygulama ayarları

* Gerekli
    * [`AzureWebJobsStorage`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)
* Tüketim planı işlevleri için gerekli
    * [`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#websitecontentazurefileconnectionstring)
    * [`WEBSITE_CONTENTSHARE`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#websitecontentshare)

[Burada bu uygulama ayarlarını okuma](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

### <a name="guidance"></a>Rehber

* "Yuva ayarı" Bu ayarlardan herhangi birini için denetlemez. Dağıtım yuvalarını değiştirme, işlev çalışmamasına neden olur.
* Otomatik dağıtım bir parçası olarak bu ayarları değiştirmeyin.
* Bu ayarlar, oluşturma zamanında belirtilen ve geçerli olması gerekir. Ayarları olaydan sonra eklenen olsa bile, bu ayarları içermeyen bir otomatik dağıtım işlevsel olmayan bir uygulamada neden olur.

## <a name="storage-account-credentials-invalid"></a>Depolama hesabı kimlik bilgileri geçersiz

Depolama anahtarlarını yeniden, yukarıdaki depolama hesabı bağlantı dizeleri güncelleştirilmesi gerekir. [Burada depolama anahtar yönetimi hakkında daha fazla bilgi](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)

## <a name="storage-account-inaccessible"></a>Depolama hesabına erişilemiyor

İşlev uygulamanızı depolama hesabına erişebilir olması gerekir. O blok bir işlevler bir depolama hesabına erişim için sık karşılaşılan sorunlar şunlardır:

* İşlev uygulamaları doğru ağ kuralları App Service ortamları için Dağıtılmış depolama hesabına gelen ve giden trafiğe izin vermek için
* Depolama hesabı güvenlik duvarı etkinleştirilir ve işlevleri gelen ve giden trafiğine izin verecek şekilde yapılandırılmadı. [Burada depolama hesabının güvenlik duvarı yapılandırması hakkında daha fazla bilgi](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

## <a name="daily-execution-quota-full"></a>Günlük yürütme kotası tam

Yapılandırılmış günlük yürütme kota varsa, işlev uygulamanızı geçici olarak devre dışı bırakılır ve portal denetimleri birçoğu kullanılamaz hale gelir. 

* Onay doğrulamak için Platform özellikleri açın > portalında işlev uygulaması ayarları. Kota kullanıyorsanız aşağıdaki iletiyi görürsünüz.
    * `The Function App has reached daily usage quota and has been stopped until the next 24 hours time frame.`
* Kota kaldırın ve sorunu çözmek için uygulamanızı yeniden başlatın.

## <a name="next-steps"></a>Sonraki Adımlar

İşlev uygulamanız arka ve çalışır durumda olduğuna göre amacıyla müşterilerimize hızlı başlangıç kılavuzlarımız ve geliştirici başvuruları ve yeniden çalıştırmayı göz atın!

* [İlk Azure İşlevinizi oluşturma](functions-create-first-azure-function.md)  
  Hemen başlayın ve Azure İşlevleri hızlı başlangıcını kullanarak ilk işlevinizi oluşturun. 
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Azure İşlevleri çalışma zamanı hakkında daha teknik bilgiler ve işlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bir başvuru sağlar.
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar. 
* [Azure Uygulama Hizmeti hakkında daha fazla bilgi edinin](../app-service/overview.md)  
  Azure İşlevleri; dağıtımlar, ortam değişkenleri ve tanılama gibi temel işlevler için Azure App Service’i kullanır. 
