---
title: Azure Stack profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri | Microsoft Docs
description: Azure Stack profilleri tarafından desteklenen Azure Resource Manager sürümü hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: bc57d445c334baeb32dbffda814cb10a35956d03
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380225"
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Azure Stack profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri

Bu makalede Azure yığını tarafından kullanılan her bir API profili için bir kaynak sağlayıcısı ve sürüm numaraları bulabilirsiniz. Bu makaledeki tablolar, her kaynak sağlayıcısı ve Profiller API sürümleri için desteklenen sürümleri listelenir. Her kaynak sağlayıcısı, bir dizi kaynak türleri ve belirli sürüm numaraları içeriyor.

API profili üç adlandırma kuralları kullanır:
 - en son
 - Yyyy-aa-gg-karma
 - yyyy-aa-gg-profili

Bir API profillerini ve açıklama sürüm yayın temposudur için Azure Stack için bkz: [yönetme API sürümü profillerini Azure Stack'te](azure-stack-version-profiles.md).

> [!Note]  
> **Son** API profili kaynak sağlayıcısı API sürümünün en son içerir ve bu makalede listelenen değil.

## <a name="overview-of-2018--03-01-hybrid"></a>2018-03-01-karma genel bakış

| Kaynak sağlayıcısı | API sürümü |
|-----------------------------------------------|-----------------------------------------------------|
| Microsoft.Compute | 2017-03-30 |
| Microsoft.Network | 2017-10-01<br>VPN ağ geçidi 2017-03-01 olacaktır |
| Microsoft.Storage (veri düzlemi) | 2017-04-17 |
| Microsoft.Storage (Denetim düzlemi) | 2016-01-01 |
| Microsoft. Web | 2016-08-01<br>azure'daki (şimdi) itibarıyla Son olduğu |
| Microsoft.KeyVault | 2016-10-01 (değiştirmeden) |
| Microsoft.Resources (Azure Resource Manager kendi) | 2016-02-01 |
| Microsoft.Authorization (ilke işlemleri) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft.Keyvault | 2016-10-01 |
| İlke | 2016-10-01 |
| Kaynaklar | 2016-10-01 |
| Resources_Links | 2016-10-01 |
| Resources_Locks | 2016-10-01 |
| Abonelikler | 2016-10-01 |

Daha fazla sürümlerinin listesi için her kaynak türü için API Profil sağlayıcıları için bkz. [2018-03-01-karma ayrıntılarını](#details-for-the-2018-03-01-hybrid) profili.

## <a name="overview-of-2018-03-01-hybrid"></a>2018-03-01-karma genel bakış

| Kaynak sağlayıcısı | API sürümü |
|------------------------------------------------|------------------------------|
| Microsoft.Compute | 2016-03-30 |
| Microsoft.Network | 2015-06-15 |
| Microsoft.Storage (veri düzlemi) | 2015-04-05  |
| Microsoft.Storage (Denetim düzlemi) | 2016-01-01   |
| Microsoft.Websites | 2016-01-01 |
| Microsoft.KeyVault | 2016-10-01<br>(Değiştirme değil) |
| Microsoft.Resources<br>(Azure Resource Manager kendi) | 2016-02-01 |
| Microsoft.Authorization<Br>(ilke işlemleri) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft.Keyvault | 2016-10-01 |
| İlke | 2015-10-01-Önizleme |
| Kaynaklar | 2016-02-01 |
| Resources_Links | 2016-09-01 |
| Resources_Locks | 2016-09-01 |
| Abonelikler | 2016-06-1 |

Daha fazla sürümlerinin listesi için her kaynak türü için API Profil sağlayıcıları için bkz. [2018-03-01-karma için Ayrıntılar](#details-for-the-2018-03-01-hybrid)

## <a name="details-for-the-2018-03-01-hybrid"></a>Ayrıntılar için 2018-03-01-karma

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Kuruluşunuzdaki kullanıcıların kaynakları üzerinde gerçekleştirebileceğiniz eylemlerden yönetmek için rol tabanlı erişim denetimi kullanın. Bu işlem kümesi, rolleri tanımlamak, rolleri kullanıcılara veya gruplara atamak ve izinleri hakkında bilgi almak sağlar. Daha fazla bilgi için [yetkilendirme](https://docs.microsoft.com/rest/api/authorization/).

| Kaynak Türleri | API sürümleri |
|---------------------|--------------------|
| Kilitler | 2017-04-01 |
| İşlemler | 2015-07-01 |
| İzinler | 2015-07-01 |
| İlke Atamaları | 2016-12-01 (2017-06-01-Önizleme) |
| İlke Tanımları | 2016-12-01 |
| Sağlayıcı işlemleri | 2015-07-01-Önizleme |
| Rol Atamaları | 2015-07-01 |
| Rol tanımları | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Kaynak Türü | API Sürümü |
|----------------------------------|----------------------|
| Sağlayıcı temsilcisi abonelikler | 2015-06-01 - Önizleme |
| Temsilci kullanım toplamları | 2015-06-01 - Önizleme |
| Harcamalarınızı tahmin kaynak | 2015-06-01-Önizleme |
| İşlemler | 2015-06-01 - Önizleme |
| Abone kullanım toplamları | 2015-06-01 - Önizleme |
| Kullanım Toplamları | 2015-06-01 - Önizleme |

### <a name="microsoftcompute"></a>Microsoft.Compute

İşlem Azure API'leri, sanal makineler ve destek kaynaklarını programlı erişim sağlar. Daha fazla bilgi için [Azure işlem](https://docs.microsoft.com/rest/api/compute/).

| Kaynak Türü | API Sürümü |
|---------------------------------------------------------------|-------------|
| Kullanılabilirlik Kümeleri | 2016-03-30 |
| Konumlar | 2016-03-30 |
| Konum/işlemleri | 2016-03-30 |
| Konum/yayımcıları | 2016-03-30 |
| Konum/kullanımları | 2016-03-30 |
| Konum/vmSizes | 2016-03-30 |
| İşlemler | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Sanal makineler ve uzantıları | 2016-03-30 |
| Sanal Makine Ölçek Kümeleri | 2016-03-30 |
| Sanal makine ölçek kümeleri ve uzantıları | 2016-03-30 |
| Sanal makine ölçek kümeleri/ağ arabirimleri | 2016-03-30 |
| Sanal makine ölçek kümeleri/sanal makineler | 2016-03-30 |
| Sanal makine ölçek kümeleri/virtualMachines/networkınterface'lerden bazıları | 2016-03-30 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| Kaynak Türü | API Sürümü |
|------------------|-------------|
| Seçki | 2015-04-01 |
| Seçki İçeriği | 2015-04-01 |
| Seçkiden Ayıklananlar | 2015-04-01 |
| Galeri öğeleri | 2015-04-01 |
| İşlemler | 2015-04-01 |
| Portal | 2015-04-01 |
| Arama | 2015-04-01 |
| Öner | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| Kaynak Türleri | API sürümleri |
|--------------------|--------------------|
| İşlemler | 2015-04-01 |
| Etkinlik Türleri | 2015-04-01 |
| Olay kategorisi | 2015-04-01 |
| Ölçüm Tanımları | 2018-01-01 |
| Ölçümler | 2018-01-01 |
| Tanılama ayarları | 2017-05-01-Önizleme |
| Tanılama ayarları kategorileri | 2017-05-01-Önizleme |


### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Anahtarınızı yönetme, anahtarlara, parolalara ve sertifikalara anahtar kasalarınıza içinde yanı sıra kasaları. Daha fazla bilgi için [Azure anahtar kasası REST API Başvurusu](https://docs.microsoft.com/rest/api/keyvault/).

| Kaynak Türleri | API sürümleri |
|-------------------------|--------------|
| İşlemler | 2016-10-01 |
| Kasalar | 2016-10-01 |
| Kasaları / erişim ilkeleri | 2016-10-01 |
| Kasalar/parolalar | 2016-10-01 |

### <a name="microsoftnetwork"></a>Microsoft.Network

İşlem araması sonucu, kullanılabilir ağ bulut işlemleri listesi gösterimidir. Daha fazla bilgi için [işlem REST API](https://docs.microsoft.com/rest/api/operation/).

| Kaynak Türleri | API sürümleri |
|---------------------------|--------------|
| Bağlantılar | 2015-06-15 |
| DNS Bölgeleri | 2016-04-01 |
| Yük Dengeleyiciler | 2015-06-15 |
| Yerel Ağ Geçidi | 2015-06-15 |
| Konumlar | 2016-04-01 |
| Konum/operationResults | 2016-04-01 |
| Konum/işlemleri | 2016-04-01 |
| Konum/kullanımları | 2016-04-01 |
| Ağ Arabirimleri | 2015-06-15 |
| Ağ Güvenlik Grupları | 2015-06-15 |
| İşlemler | 2015-06-15 |
| Genel IP Adresi | 2015-06-15 |
| Yönlendirme Tabloları | 2015-06-15 |
| Sanal Ağ Geçidi | 2015-06-15 |
| Sanal Ağlar | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

Azure Resource Manager dağıtma ve Azure çözümlerinizi için altyapıyı yönetmenize olanak sağlar. Kaynak grupları, ilgili kaynakları düzenlemek ve JSON şablonları kullanarak kaynaklarınızı dağıtma. Kaynakları Resource Manager ile yönetme ve dağıtma için bir giriş için bkz [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

| Kaynak Türleri | API sürümleri |
|-----------------------------------------|-------------------|
| Uygulama kayıtları | 2015-01-01 |
| Kaynak Adını Denetle | 2016-09-01 |
| Sağlayıcı temsilcisi | 2015-01-01 |
| Sağlayıcıları/teklif temsilcisi | 2015-01-01 |
| Teklifler/DelegatedProviders/estimatePrice | 2015-01-01 |
| Dağıtımlar | 2016-09-01 |
| Dağıtımları/işlemleri | 2016-09-01 |
| Uzantı meta verileri | 2015-01-01 |
| Bağlantılar | 2016-09-01 |
| Konumlar | 2015-01-01 |
| Teklifler | 2015-01-01 |
| İşlemler | 2015-01-01 |
| Sağlayıcılar | 2017-08-01 |
| Kaynak Grupları | 2016-09-01 |
| Kaynaklar | 2016-09-01 |
| Abonelikler | 2016-09-01 |
| Abonelikler/konum | 2016-09-01 |
| Abonelik işlem sonuçları | 2016-09-01 |
| Abonelikler/sağlayıcıları | 2017-08-01 |
| Abonelikler/kaynak grupları | 2016-09-01 |
| Abonelikler/resourceGroups/kaynaklar | 2016-09-01 |
| Abonelikler/kaynak | 2016-09-01 |
| Abonelikler/tagNames | 2016-09-01 |
| Abonelikler/tagNames/tagValues | 2016-09-01 |
| Kiracılar | 2017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

Depolama kaynak Sağlayıcısı'nı (SRP), depolama hesabı ve anahtarları programlı bir şekilde yönetmenizi sağlar. Daha fazla bilgi için [Azure depolama kaynak sağlayıcısı REST API Başvurusu](https://docs.microsoft.com/rest/api/storagerp/).

| Kaynak Türleri | API sürümleri |
|-------------------------|--------------|
| Ad Kullanılabilirliğini Denetle | 2016-01-01 |
| Konumlar | 2016-01-01 |
| Konum/kotaları | 2016-01-01 |
| İşlemler | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Kullanımları | 2016-01-01 |

## <a name="details-for-the-2018-03-01-hybrid"></a>Ayrıntılar için 2018-03-01-karma

### <a name="microsoft-authorization"></a>Microsoft Yetkilendirmesi

| Kaynak Türleri | API sürümleri |
|---------------------|---------------------------------|
| Kilitler | 2017-04-01 |
| İşlemler | 2015-07-01 |
| İzinler | 2015-07-01 |
| İlke Atamaları | 2016-12-01 (2017-06-01-Önizleme) |
| İlke Tanımları | 2016-12-01 |
| Sağlayıcı işlemleri | 2015-07-01-Önizleme |
| Rol Atamaları | 2015-07-01 |
| Rol tanımları | 2015-07-01 |

### <a name="microsoftcompute"></a>Microsoft.Compute

| Kaynak Türü | API Sürümü |
|---------------------------------------------------------------|-------------|
| Kullanılabilirlik Kümeleri | 2016-03-30 |
| Konumlar | 2016-03-30 |
| Konum/işlemleri | 2016-03-30 |
| Konum/yayımcıları | 2016-03-30 |
| Konum/kullanımları | 2016-03-30 |
| Konum/vmSizes | 2016-03-30 |
| İşlemler | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Sanal makineler ve uzantıları | 2016-03-30 |
| Sanal Makine Ölçek Kümeleri | 2016-03-30 |
| Sanal makine ölçek kümeleri ve uzantıları | 2016-03-30 |
| Sanal makine ölçek kümeleri/ağ arabirimleri | 2016-03-30 |
| Sanal makine ölçek kümeleri/sanal makineler | 2016-03-30 |
| Sanal makine ölçek kümeleri/virtualMachines/networkınterface'lerden bazıları | 2016-03-30 |

### <a name="microsoftnetwork"></a>Microsoft.Network

| Kaynak Türleri | API sürümleri |
|---------------------------|--------------|
| Bağlantılar | 2015-06-15 |
| DNS Bölgeleri | 2016-04-01 |
| Yük Dengeleyiciler | 2015-06-15 |
| Yerel Ağ Geçidi | 2015-06-15 |
| Konumlar | 2016-04-01 |
| Konum/operationResults | 2016-04-01 |
| Konum/işlemleri | 2016-04-01 |
| Konum/kullanımları | 2016-04-01 |
| Ağ Arabirimleri | 2015-06-15 |
| Ağ Güvenlik Grupları | 2015-06-15 |
| İşlemler | 2015-06-15 |
| Genel IP Adresi | 2015-06-15 |
| Yönlendirme Tabloları | 2015-06-15 |
| Sanal Ağ Geçidi | 2015-06-15 |
| Sanal Ağlar | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

| Kaynak Türleri | API sürümleri |
|-----------------------------------------|--------------|
| Uygulama kayıtları | 2015-01-01 |
| Kaynak Adını Denetle | 2016-09-01 |
| Sağlayıcı temsilcisi | 2015-01-01 |
| Sağlayıcıları/teklif temsilcisi | 2015-01-01 |
| Teklifler/DelegatedProviders/estimatePrice | 2015-01-01 |
| Dağıtımlar | 2016-09-01 |
| Dağıtımları/işlemleri | 2016-09-01 |
| Uzantı meta verileri | 2015-01-01 |
| Bağlantılar | 2016-09-01 |
| Konumlar | 2015-01-01 |
| Teklifler | 2015-01-01 |
| İşlemler | 2015-01-01 |
| Sağlayıcılar | 2017-08-01 |
| Kaynak Grupları | 2016-09-01 |
| Kaynaklar | 2016-09-01 |
| Abonelikler | 2016-09-01 |
| Abonelikler/konum | 2016-09-01 |
| Abonelik işlem sonuçları | 2016-09-01 |
| Abonelikler/sağlayıcıları | 2017-08-01 |
| Abonelikler/kaynak grupları | 2016-09-01 |
| Abonelikler/resourceGroups/kaynaklar | 2016-09-01 |
| Abonelikler/kaynak | 2016-09-01 |
| Subscriptiosn/tagNames | 2016-09-01 |
| Abonelikler/tagNames/tagValues | 2016-09-01 |
| Kiracılar | 2017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage

| Kaynak Türleri | API sürümleri |
|-------------------------|--------------|
| Ad Kullanılabilirliğini Denetle | 2016-01-01 |
| Konumlar | 2016-01-01 |
| Konum/kotaları | 2016-01-01 |
| İşlemler | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Kullanımları | 2016-01-01 |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure Stack kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
