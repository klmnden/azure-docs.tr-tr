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
ms.date: 04/02/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: db01df21c95ee41197344cec719f1c2ab2dfc2ed
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Azure yığınında profilleri tarafından desteklenen kaynak sağlayıcısı API sürümleri

Bir Azure kaynak sağlayıcısı dağıtmak ve Azure Resource Manager aracılığıyla yönetmek kaynaklar sağlar. Her sağlayıcısı, kaynaklarla çalışmak için işlem sunar. Sanal makineler sağlayan Microsoft.Compute, depolama hesabı kaynakları sağlayan, Microsoft.Storage ve web uygulamaları için ilgili kaynakları sağlayan Microsoft.Web bazı ortak kaynak sağlayıcıları içerir. Daha fazla bilgi için bkz. [Kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services).

Aşağıdaki tabloda her kaynak sağlayıcısı için API sürümü desteklenen bir sürümü için Azure yığın profilleri kullanırken gösterir.

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

| Kaynak Türü | API sürümü |
|----------------------------------|----------------------|
| Temsilci sağlayıcısı abonelikleri | 2015-06-01 - Önizleme |
| Temsilci kullanımını toplar | 2015-06-01 - Önizleme |
| Tahmin kaynak harcamanız | 2015-06-01-Önizleme |
| İşlemler | 2015-06-01 - Önizleme |
| Abone kullanımını toplar | 2015-06-01 - Önizleme |
| Kullanım Toplamları | 2015-06-01 - Önizleme |

### <a name="microsoftcompute"></a>Microsoft.Compute

Azure işlem API'leri sanal makineleri ve destekleyici kaynaklarına programlı erişim verin. Daha fazla bilgi için bkz: [Azure işlem](https://docs.microsoft.com/rest/api/compute/).

| Kaynak Türü | API sürümü |
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

| Kaynak Türü | API sürümü |
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
| Uyarı Kuralları | 2016-03-01 |
| Olay kategorisi | 2017-03-01-Önizleme |
| Etkinlik Türleri | 2017-03-01-Önizleme |
| Ölçüm Tanımları | 2016-03-01 |
| İşlemler | 2015-04-01 |

### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Anahtarınızı yönetme anahtarları, gizli ve sertifikaları, anahtar kasalarını içinde yanı sıra kasaları. Daha fazla bilgi için bkz: [Azure anahtar kasası REST API Başvurusu](https://docs.microsoft.com/rest/api/keyvault/).

| Kaynak Türleri | API sürümü |
|-------------------------|--------------|
| İşlemler | 2016-10-01 |
| kasaları | 2016-10-01 |
| Kasaları / erişim ilkeleri | 2016-10-01 |
| Kasalar/parolalar | 2016-10-01 |

### <a name="microsoftkeyvaultadmin"></a>Microsoft.Keyvault.Admin

Anahtarınızı yönetme anahtarları, gizli ve sertifikaları, anahtar kasalarını içinde yanı sıra kasaları. Daha fazla bilgi için bkz: [Azure anahtar kasası REST API Başvurusu](https://docs.microsoft.com/rest/api/keyvault/).

| Kaynak Türleri | API sürümü |
|------------------|--------------------|
| Konumlar | 2017-02-01-Önizleme |
| Konumları/kotaları | 2017-02-01-Önizleme |

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

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)  
