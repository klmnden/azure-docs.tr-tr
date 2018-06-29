---
title: Kaynak sağlayıcısı API sürümleri Azure yığınında profilleri tarafından desteklenen | Microsoft Docs
description: Azure yığınında profilleri tarafından desteklenen Azure Resource Manager sürümü hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 1a516c890441c3b703d43f31816b7c37cac364fd
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054390"
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Azure yığınında profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri

Bu makalede Azure yığını tarafından kullanılan her bir API profil için kaynak sağlayıcısı ve sürüm numaraları bulabilirsiniz. Bu makaledeki tablolar her kaynak sağlayıcısı ve Profiller API sürümleri için desteklenen sürümleri listelenir. Her kaynak sağlayıcısı, bir dizi kaynak türleri ve belirli bir sürüm numaralarını içerir.

API profili üç adlandırma kuralları kullanır:
 - en son
 - yyyy-aa-gg-karma
 - yyyy-aa-gg-profili

Bir açıklaması API profilleri ve sürüm yayın tempoyla için Azure yığınının bkz [yönetmek API sürümü profilleri Azure yığınında](azure-stack-version-profiles.md).

> [!Note]  
> **Son** API profili kaynak sağlayıcısı API sürümü son içeriyor ve bu makalede listelenmez.

## <a name="overview-of-2018--03-01-hybrid"></a>2018-03-01-karma genel bakış

| Kaynak sağlayıcısı | API sürümü |
|-----------------------------------------------|-----------------------------------------------------|
| Microsoft.Compute | 2017-03-30 |
| Microsoft.Network | 2017-10-01<br>VPN ağ geçidi 2017-03-01 olacaktır |
| Microsoft.Storage (veri düzlemi) | 2017-04-17 |
| Microsoft.Storage (Denetim düzlemi) | 2016-01-01 |
| Microsoft. Web | 2016-08-01<br>(itibariyle şimdi) en son Azure olduğu |
| Microsoft.KeyVault | 2016-10-01 (değiştirmeden) |
| Microsoft.Resources (Azure Resource Manager kendisini) | 2016-02-01 |
| Microsoft.Authorization (ilke işlemleri) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft.Keyvault | 2016-10-01 |
| İlke | 2016-10-01 |
| Kaynaklar | 2016-10-01 |
| Resources_Links | 2016-10-01 |
| Resources_Locks | 2016-10-01 |
| Abonelikler | 2016-10-01 |

Daha fazla sürümlerinin listesi API Profil sağlayıcıları için her kaynak türünün için bkz [2018-03-01-karma ayrıntılarını](#details-for-the-2018-03-01-hybrid) profili.

## <a name="overview-of-2017-03-09-profile"></a>2017-03-09-profili genel bakış

| Kaynak sağlayıcısı | API sürümü |
|------------------------------------------------|------------------------------|
| Microsoft.Compute | 2016-03-30 |
| Microsoft.Network | 2015-06-15 |
| Microsoft.Storage (veri düzlemi) | 2015-04-05  |
| Microsoft.Storage (Denetim düzlemi) | 2016-01-01   |
| Microsoft.Websites | 2016-01-01 |
| Microsoft.KeyVault | 2016-10-01<br>(Değiştirme değil) |
| Microsoft.Resources<br>(Azure Resource Manager kendisini) | 2016-02-01 |
| Microsoft.Authorization<Br>(ilke işlemleri) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft.Keyvault | 2016-10-01 |
| İlke | 2015-10-01-Önizleme |
| Kaynaklar | 2016-02-01 |
| Resources_Links | 2016-09-01 |
| Resources_Locks | 2016-09-01 |
| Abonelikler | 2016 06 1 |

Daha fazla sürümlerinin listesi API Profil sağlayıcıları için her kaynak türünün için bkz [2017-03-09-profili ayrıntıları](#details-for-the-2017-03-09-profile)

## <a name="details-for-the-2018-03-01-hybrid"></a>2018-03-01-karma ayrıntılarını

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Rol tabanlı erişim denetimi, kuruluşunuzdaki kullanıcılar üzerindeki kaynaklara gerçekleştirebileceğiniz eylemleri yönetmek için kullanın. Bu işlem kümesi rolleri tanımlama, kullanıcılar veya gruplar için rolleri atamak ve izinleri hakkında bilgi almak etkinleştirir. Daha fazla bilgi için bkz: [yetkilendirme](https://docs.microsoft.com/rest/api/authorization/).

| Kaynak Türleri | API sürümü |
|---------------------|--------------------|
| Kilitler | 2017-04-01 |
| İşlemler | 2015-07-01 |
| İzinler | 2015-07-01 |
| İlke Atamaları | 2016-12-01 (2017-06-01-Önizleme) |
| İlke Tanımları | 2016-12-01 |
| Sağlayıcısı işlemleri | 2015-07-01-Önizleme |
| Rol atamaları | 2015-07-01 |
| Rol tanımları | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Kaynak Türü | API Sürümü |
|----------------------------------|----------------------|
| Temsilci sağlayıcısı abonelikleri | 2015-06-01 - Önizleme |
| Temsilci kullanımını toplar | 2015-06-01 - Önizleme |
| Tahmin kaynak harcamanız | 2015-06-01-Önizleme |
| İşlemler | 2015-06-01 - Önizleme |
| Abone kullanımını toplar | 2015-06-01 - Önizleme |
| Kullanım Toplamları | 2015-06-01 - Önizleme |

### <a name="microsoftcompute"></a>Microsoft.Compute

Azure işlem API'leri sanal makineleri ve destekleyici kaynaklarına programlı erişim verin. Daha fazla bilgi için bkz: [Azure işlem](https://docs.microsoft.com/rest/api/compute/).

| Kaynak Türü | API Sürümü |
|---------------------------------------------------------------|-------------|
| Kullanılabilirlik Kümeleri | 2016-03-30 |
| Konumlar | 2016-03-30 |
| Konumları/işlemleri | 2016-03-30 |
| Konumları/Yayımcılar | 2016-03-30 |
| Konumları/kullanımları | 2016-03-30 |
| Konumları/vmSizes | 2016-03-30 |
| İşlemler | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Sanal makineler/uzantıları | 2016-03-30 |
| Sanal Makine Ölçek Kümeleri | 2016-03-30 |
| Sanal makine ölçek kümeleri/uzantıları | 2016-03-30 |
| Sanal makine ölçek kümeleri/ağ arabirimleri | 2016-03-30 |
| Sanal makine ölçek kümeleri/sanal makineler | 2016-03-30 |
| Sanal makine ölçek kümeleri/virtualMachines/networkInterfaces | 2016-03-30 |

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

| Kaynak Türleri | API sürümü |
|--------------------|--------------------|
| İşlemler | 2015-04-01 |
| Etkinlik Türleri | 2015-04-01 |
| Olay kategorisi | 2015-04-01 |
| Ölçüm Tanımları | 2018-01-01 |
| Ölçümler | 2018-01-01 |
| Tanılama ayarları | 2017-05-01-Önizleme |
| Tanılama ayarları kategorileri | 2017-05-01-Önizleme |


### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Anahtarınızı yönetme anahtarları, gizli ve sertifikaları, anahtar kasalarını içinde yanı sıra kasaları. Daha fazla bilgi için bkz: [Azure anahtar kasası REST API Başvurusu](https://docs.microsoft.com/rest/api/keyvault/).

| Kaynak Türleri | API sürümü |
|-------------------------|--------------|
| İşlemler | 2016-10-01 |
| kasaları | 2016-10-01 |
| Kasaları / erişim ilkeleri | 2016-10-01 |
| Kasalar/parolalar | 2016-10-01 |

### <a name="microsoftnetwork"></a>Microsoft.Network

İşlem arama sonucu, kullanılabilir ağ bulut işlemleri listesi gösterimidir. Daha fazla bilgi için bkz: [işlemi REST API](https://docs.microsoft.com/rest/api/operation/).

| Kaynak Türleri | API sürümü |
|---------------------------|--------------|
| Bağlantılar | 2015-06-15 |
| DNS bölgeleri | 2016-04-01 |
| Yük Dengeleyiciler | 2015-06-15 |
| Yerel Ağ Geçidi | 2015-06-15 |
| Konumlar | 2016-04-01 |
| Konum/operationResults | 2016-04-01 |
| Konumları/işlemleri | 2016-04-01 |
| Konumları/kullanımları | 2016-04-01 |
| Ağ Arabirimleri | 2015-06-15 |
| Ağ Güvenlik Grupları | 2015-06-15 |
| İşlemler | 2015-06-15 |
| Genel IP Adresi | 2015-06-15 |
| Yönlendirme Tabloları | 2015-06-15 |
| Sanal Ağ Geçidi | 2015-06-15 |
| Sanal Ağlar | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

Azure Resource Manager, dağıtma ve yönetme Azure çözümleriniz için altyapı sağlar. Kaynak grupları ilgili kaynakların düzenlemek ve kaynaklarınızı JSON şablonları ile dağıtın. Dağıtma ve Kaynak Yöneticisi ile kaynakları yönetme giriş için bkz: [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

| Kaynak Türleri | API sürümü |
|-----------------------------------------|-------------------|
| Uygulama kayıtlar | 2015-01-01 |
| Kaynak Adını Denetle | 2015-012016-09-01 |
| Temsilci sağlayıcıları | 2015-01-01 |
| Temsilci sağlayıcıları/teklifleri | 2015-01-01 |
| Teklifler/DelegatedProviders/estimatePrice | 2015-01-01 |
| Dağıtımlar | 2016-0209-01 |
| Dağıtımları/işlemleri | 2016-0209-01 |
| Uzantıları meta verileri | 2015-01-01 |
| Bağlantılar | 2015-012016-09-01 |
| Konumlar | 2015-01-01 |
| Teklifler | 2015-01-01 |
| İşlemler | 2015-01-01 |
| Sağlayıcılar | 2015-012017-08-01 |
| Kaynak Grupları | 2015-012016-09-01 |
| Kaynaklar | 2015-012016-09-01 |
| Abonelikler | 2015-012016-09-01 |
| Abonelikleri/konum | 2015-012016-09-01 |
| İşlem başına abonelikleri sonuçları | 2015-012016-09-01 |
| Abonelikleri/sağlayıcıları | 2015-012017-08-01 |
| Abonelik/kaynak grupları | 2015-012016-09-01 |
| ResourceGroups/abonelikleri/kaynakları | 2015-012016-09-01 |
| Abonelikleri/kaynakları | 2015-012016-09-01 |
| Abonelikleri/tagNames | 2016 0609 01 |
| TagNames/abonelikleri/tagValues | 2016 0609 01 |
| Kiracılar | 2015-012017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

Depolama kaynak Sağlayıcısı'ni (SRP), depolama hesabı ve anahtarları programlı olarak yönetmenizi sağlar. Daha fazla bilgi için bkz: [Azure depolama kaynak sağlayıcısı REST API Başvurusu](https://docs.microsoft.com/rest/api/storagerp/).

| Kaynak Türleri | API sürümü |
|-------------------------|--------------|
| Ad Kullanılabilirliğini Denetle | 2016-01-01 |
| Konumlar | 2016-01-01 |
| Konumları/kotaları | 2016-01-01 |
| İşlemler | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Kullanımları | 2016-01-01 |

## <a name="details-for-the-2017-03-09-profile"></a>2017-03-09-profili ayrıntıları

### <a name="microsoft-authorization"></a>Microsoft Yetkilendirmesi

| Kaynak Türleri | API sürümü |
|---------------------|---------------------------------|
| Kilitler | 2017-04-01 |
| İşlemler | 2015-07-01 |
| İzinler | 2015-07-01 |
| İlke Atamaları | 2016-12-01 (2017-06-01-Önizleme) |
| İlke Tanımları | 2016-12-01 |
| Sağlayıcısı işlemleri | 2015-07-01-Önizleme |
| Rol atamaları | 2015-07-01 |
| Rol tanımları | 2015-07-01 |

### <a name="microsoftcompute"></a>Microsoft.Compute

| Kaynak Türü | API Sürümü |
|---------------------------------------------------------------|-------------|
| Kullanılabilirlik Kümeleri | 2016-03-30 |
| Konumlar | 2016-03-30 |
| Konumları/işlemleri | 2016-03-30 |
| Konumları/Yayımcılar | 2016-03-30 |
| Konumları/kullanımları | 2016-03-30 |
| Konumları/vmSizes | 2016-03-30 |
| İşlemler | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Sanal makineler/uzantıları | 2016-03-30 |
| Sanal Makine Ölçek Kümeleri | 2016-03-30 |
| Sanal makine ölçek kümeleri/uzantıları | 2016-03-30 |
| Sanal makine ölçek kümeleri/ağ arabirimleri | 2016-03-30 |
| Sanal makine ölçek kümeleri/sanal makineler | 2016-03-30 |
| Sanal makine ölçek kümeleri/virtualMachines/networkInterfaces | 2016-03-30 |

### <a name="microsoftnetwork"></a>Microsoft.Network

| Kaynak Türleri | API sürümü |
|---------------------------|--------------|
| Bağlantılar | 2015-06-15 |
| DNS bölgeleri | 2016-04-01 |
| Yük Dengeleyiciler | 2015-06-15 |
| Yerel Ağ Geçidi | 2015-06-15 |
| Konumlar | 2016-04-01 |
| Konum/operationResults | 2016-04-01 |
| Konumları/işlemleri | 2016-04-01 |
| Konumları/kullanımları | 2016-04-01 |
| Ağ Arabirimleri | 2015-06-15 |
| Ağ Güvenlik Grupları | 2015-06-15 |
| İşlemler | 2015-06-15 |
| Genel IP Adresi | 2015-06-15 |
| Yönlendirme Tabloları | 2015-06-15 |
| Sanal Ağ Geçidi | 2015-06-15 |
| Sanal Ağlar | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

| Kaynak Türleri | API sürümü |
|-----------------------------------------|--------------|
| Uygulama kayıtlar | 2015-01-01 |
| Kaynak Adını Denetle | 2016-09-01 |
| Temsilci sağlayıcıları | 2015-01-01 |
| Temsilci sağlayıcıları/teklifleri | 2015-01-01 |
| Teklifler/DelegatedProviders/estimatePrice | 2015-01-01 |
| Dağıtımlar | 2016-09-01 |
| Dağıtımları/işlemleri | 2016-09-01 |
| Uzantıları meta verileri | 2015-01-01 |
| Bağlantılar | 2016-09-01 |
| Konumlar | 2015-01-01 |
| Teklifler | 2015-01-01 |
| İşlemler | 2015-01-01 |
| Sağlayıcılar | 2017-08-01 |
| Kaynak Grupları | 2016-09-01 |
| Kaynaklar | 2016-09-01 |
| Abonelikler | 2016-09-01 |
| Abonelikleri/konum | 2016-09-01 |
| İşlem başına abonelikleri sonuçları | 2016-09-01 |
| Abonelikleri/sağlayıcıları | 2017-08-01 |
| Abonelik/kaynak grupları | 2016-09-01 |
| ResourceGroups/abonelikleri/kaynakları | 2016-09-01 |
| Abonelikleri/kaynakları | 2016-09-01 |
| Subscriptiosn/tagNames | 2016-09-01 |
| TagNames/abonelikleri/tagValues | 2016-09-01 |
| Kiracılar | 2017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage

| Kaynak Türleri | API sürümü |
|-------------------------|--------------|
| Ad Kullanılabilirliğini Denetle | 2016-01-01 |
| Konumlar | 2016-01-01 |
| Konumları/kotaları | 2016-01-01 |
| İşlemler | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Kullanımları | 2016-01-01 |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
