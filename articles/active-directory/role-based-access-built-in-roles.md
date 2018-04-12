---
title: Azure rol tabanlı erişim denetimi (RBAC) için yerleşik roller | Microsoft Docs
description: Bu konu için rol tabanlı erişim denetimi (RBAC) rollerdeki yerleşik açıklar. Rolleri sürekli olarak eklenir, belgeleri yenilik kontrol edin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 4d9df6743d84310b7db70034d1e84dd3591b3c21
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi için yerleşik roller
Azure rol tabanlı erişim denetimi (RBAC), kullanıcılar, gruplar ve hizmetlere atanmış aşağıdaki yerleşik rolleri ile birlikte gelir. Yerleşik rol tanımlarını değiştiremezsiniz. Ancak, oluşturabileceğiniz [Azure rbac'de özel roller](role-based-access-control-custom-roles.md) , kuruluşunuzun belirli gereksinimlerine uyacak şekilde.

## <a name="built-in-role-descriptions"></a>Yerleşik rol açıklamaları
Aşağıdaki tabloda yerleşik roller kısa açıklamaları sağlar. Ayrıntılı bir listesi görmek için rol adını tıklatın **Eylemler** ve **notactions** rolü. **Eylemler** özelliği, Azure kaynakları izin verilen eylemleri belirtir. Eylem dizeleri joker karakterleri kullanabilirsiniz. **Notactions** özelliği, izin verilen eylemler hariç tutulan eylemleri belirtir.

Belirtilen kaynak türü üzerinde gerçekleştirebileceğiniz ne tür bir operations eylemi tanımlar. Örneğin:
- **Yazma** PUT, POST, düzeltme eki ve silme işlemleri yapmanıza olanak sağlar.
- **Okuma** GET işlemleri yapmanıza olanak sağlar.

Bu makalede yalnızca bugün mevcut farklı rolleri giderir. Ancak, bir kullanıcıya rol atadığınızda, kapsam tanımlayarak daha fazla izin verilen eylemleri sınırlayabilirsiniz. Bu, birisi bir Web sitesi katkıda bulunan, ancak yalnızca bir kaynak grubu için yapmak istiyorsanız yararlıdır.

> [!NOTE]
> Azure rol tanımlarını sürekli olarak artmaktadır. Bu makalede olarak olabildiğince güncel tutulur, ancak her zaman en son rol tanımlarını Azure PowerShell'de bulabilirsiniz. Kullanım [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) cmdlet'ini tüm geçerli rolleri listeler. Belirli bir rol kullanmaya başlayabilirsiniz `(get-azurermroledefinition "<role name>").actions` veya `(get-azurermroledefinition "<role name>").notactions` olarak uygulanabilir. Kullanım [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) listeleme işlemleri belirli bir Azure kaynak sağlayıcıları için.


| Yerleşik rolü | Açıklama |
| --- | --- |
| [Sahibi](#owner) | Kaynaklara erişim dahil olmak üzere her şeyi yönetmenizi sağlar. |
| [Katkıda bulunan](#contributor) | Kaynaklara erişim hariç olmak üzere her şeyi yönetmenizi sağlar. |
| [Okuyucu](#reader) | Her şeyi görüntülemenizi sağlar ancak değişiklik yapmanıza izin vermez. |
| [API Management hizmeti katkıda bulunan](#api-management-service-contributor) | Hizmeti ve API'leri yönetebilir |
| [API Management hizmet işleci rolü](#api-management-service-operator-role) | Hizmeti yönetebilir, ancak API'leri yönetemez |
| [API Management hizmet okuyucu rolü](#api-management-service-reader-role) | Hizmet ve API'lere salt okunur erişim |
| [Uygulama Öngörüler bileşen katkıda bulunan](#application-insights-component-contributor) | Application Insights bileşenlerini yönetebilir |
| [Uygulama Öngörüler anlık görüntü hata ayıklayıcı](#application-insights-snapshot-debugger) | Kullanıcıya Application Insights Anlık Görüntü Hata Ayıklayıcısı özelliklerini kullanma izni verir |
| [Otomasyon iş işleci](#automation-job-operator) | Otomasyon Runbook'larını kullanarak İş oluşturun ve yönetin. |
| [Automation operatörü](#automation-operator) | Otomasyon Operatörleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir |
| [Automation Runbook Operator](#automation-runbook-operator) | Runbook'un İşlerini oluşturabilmek için Runbook özelliklerini okuyun. |
| [Azure yığın kayıt sahibi](#azure-stack-registration-owner) | Azure Stack kayıtlarını yönetmenize imkan sağlar. |
| [Yedekleme katkıda bulunan](#backup-contributor) | Yedekleme hizmetini yönetmenize olanak sağlar ancak kasa oluşturma ve diğer kullanıcılara erişim verme izni sağlamaz |
| [Yedekleme işletmeni](#backup-operator) | Yedekleme kaldırma, kasa oluşturma ve diğer kullanıcılara erişim verme dışındaki yedekleme hizmetlerini yönetmenize olanak sağlar |
| [Yedekleme okuyucusu](#backup-reader) | Yedekleme hizmetlerini görüntüleyebilir ancak değişiklik yapamaz |
| [Faturalama okuyucusu](#billing-reader) | Faturalandırma verilerine okuma erişimi verir |
| [BizTalk katkıda bulunan](#biztalk-contributor) | BizTalk hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [CDN uç noktası katkıda bulunan](#cdn-endpoint-contributor) | CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
| [CDN uç noktası okuyucusu](#cdn-endpoint-reader) | CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
| [CDN profili katkıda bulunan](#cdn-profile-contributor) | CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
| [CDN profili okuyucusu](#cdn-profile-reader) | CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
| [Klasik Ağ Katılımcısı](#classic-network-contributor) | Klasik ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Klasik depolama hesabı katkıda bulunan](#classic-storage-account-contributor) | Klasik depolama hesaplarını yönetmenize izin verir ancak bu hesaplara erişim hakkı vermez. |
| [Klasik depolama hesabı anahtar işleci hizmet rolü](#classic-storage-account-key-operator-service-role) | Klasik Depolama Hesabı Anahtarı İşleçlerine, Klasik Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
| [Klasik sanal makine Katılımcısı](#classic-virtual-machine-contributor) | Klasik sanal makineleri, ancak onlara, erişimi ve sanal ağ veya depolama hesabı bağlı oldukları yönetmenize olanak tanır. |
| [ClearDB MySQL DB katkıda bulunan](#cleardb-mysql-db-contributor) | ClearDB MySQL veritabanlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Cosmos DB hesap okuyucu rolü](#cosmos-db-account-reader-role) | Azure Cosmos DB hesap verileri okuyabilir. Bkz: [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetmek için. |
| [Veri Fabrikası katkıda bulunan](#data-factory-contributor) | Veri fabrikaları ile birlikte bunların alt kaynaklarını oluşturup yönetin. |
| [Data Lake Analytics Geliştirici](#data-lake-analytics-developer) | İşlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics hesabı oluşturmanıza veya silmenize izin vermez. |
| [DevTest Labs kullanıcı](#devtest-labs-user) | Azure DevTest Labs'teki tüm sanal makinelerinize bağlanmanıza, bu makineleri başlatmanıza, yeniden başlatmanıza ve kapatmanıza izin verir. |
| [DNS bölgesi katkıda bulunan](#dns-zone-contributor) | Azure DNS'te, DNS bölgelerini ve kayıt kümelerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
| [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) | Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB önceden DocumentDB bilinirdi. |
| [Akıllı sistemler hesap katkıda bulunan](#intelligent-systems-account-contributor) | Akıllı Sistemler hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Anahtar kasası katkıda bulunan](#key-vault-contributor) | Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişmenize izin vermez. |
| [Laboratuvar Oluşturucusu](#lab-creator) | Oluşturma, yönetme, Azure Laboratuvar hesaplarınızı altında yönetilen labs silme olanak sağlar. |
| [Günlük analizi katkıda bulunan](#log-analytics-contributor) | Günlük analizi katkıda bulunan tüm izleme verilerini okuma ve izleme ayarlarını düzenleyin. İzleme ayarları düzenleme, VM'ler için VM uzantısı eklemeyi içerir; Azure Storage günlüklerinden koleksiyonu yapılandırmak için depolama hesabı anahtarlarını okuma; oluşturma ve Automation hesapları yapılandırma; çözümleri ekleme; ve tüm Azure kaynaklara Azure tanılama yapılandırılıyor. |
| [Günlük analizi okuyucu](#log-analytics-reader) | Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir. |
| [Mantığı uygulamasını katkıda bulunan](#logic-app-contributor) | Mantıksal uygulamayı yönetmenize izin verir, ancak bunlara yönelik erişimi yönetmenize izin vermez. |
| [Mantıksal uygulama işleci](#logic-app-operator) | Mantıksal uygulamayı okumanıza, etkinleştirmenize ve devre dışı bırakmanıza izin verir. |
| [Yönetilen kimlik katkıda bulunan](#managed-identity-contributor) | Oluşturma, okuma, güncelleştirme ve kullanıcı kimliği atanır silme |
| [Yönetilen kimlik işleci](#managed-identity-operator) | Okuma ve kullanıcı kimliği atanır atayın |
| [Katkıda bulunan izleme](#monitoring-contributor) | Tüm izleme verileri okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Okuyucu izleme](#monitoring-reader) | Tüm izleme verilerini (ölçümleri, günlükleri, vb.) okuyabilir. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Ağ Katılımcısı](#network-contributor) | Ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Yeni Relic APM hesap katkıda bulunan](#new-relic-apm-account-contributor) | New Relic Application Performance Management hesaplarını ve uygulamalarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Redis önbelleği katkıda bulunan](#redis-cache-contributor) | Redis Cache'leri yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Zamanlayıcı İş koleksiyonları katkıda bulunan](#scheduler-job-collections-contributor) | Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Arama hizmeti katkıda bulunan](#search-service-contributor) | Search hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Güvenlik Yöneticisi](#security-admin) | Yalnızca Güvenlik Merkezi'nde: görüntülemek güvenlik ilkeleri, güvenlik durumları görüntülemek, güvenlik ilkeleri, Uyarıları görüntüle ve öneriler düzenleme, uyarısı ve öneri yok sayın |
| [Güvenlik Yöneticisi](#security-manager) | Güvenlik bileşenlerini, güvenlik ilkelerini ve sanal makineleri yönetmenizi sağlar |
| [Güvenlik okuyucusu](#security-reader) | Yalnızca Güvenlik Merkezi'nde: önerileri ve uyarılar, güvenlik ilkeleri, güvenlik durumları görüntüleyebilir ancak değişiklik yapamaz görünümü görüntüleyebilirsiniz |
| [Site kurtarma katkıda bulunan](#site-recovery-contributor) | Kasa oluşturma ve rol atama işlemleri dışında Site Recovery hizmetini yönetmenize imkan sağlar |
| [Site kurtarma işleci](#site-recovery-operator) | Yük devretme ve yeniden çalışma dışındaki Site Recovery yönetimi işlemlerini gerçekleştirmenize izin vermez |
| [Site kurtarma okuyucusu](#site-recovery-reader) | Site Recovery durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmenize izin vermez |
| [SQL DB Katılımcısı](#sql-db-contributor) | SQL veritabanları, ancak onlara erişimi yönetmenize olanak tanır. Ayrıca, güvenlikle ilgili ilkelerini veya kendi üst SQL Server'lar yönetemezsiniz. |
| [SQL Güvenlik Yöneticisi](#sql-security-manager) | SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenizi sağlar ancak onlara erişimi yönetme izni vermez. |
| [SQL Server katkıda bulunan](#sql-server-contributor) | SQL sunucularını ve veritabanlarını yönetmenizi sağlar ancak güvenlikle ilgili ilkelerini yönetmenize izin vermez. |
| [Depolama hesabı katkıda bulunan](#storage-account-contributor) | Depolama hesaplarını yönetmenize izin verir ancak bunlara yönelik erişimi yönetmenize izin vermez. |
| [Depolama hesabı anahtar işleci hizmeti rolü](#storage-account-key-operator-service-role) | Depolama Hesabı Anahtarı İşleçlerine, Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
| [Destek isteği katkıda bulunan](#support-request-contributor) | Destek istekleri oluşturmanıza ve bunları yönetmenize olanak sağlar |
| [Trafik Yöneticisi katkıda bulunan](#traffic-manager-contributor) | Traffic Manager profillerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
| [Kullanıcı erişimi Yöneticisi](#user-access-administrator) | Azure kaynaklarına yönelik kullanıcı erişimini yönetmenizi sağlar. |
| [Sanal makine yönetici oturum açma](#virtual-machine-administrator-login) | -Bu rol kullanıcılarla Windows yönetici veya Linux kök kullanıcı ayrıcalıkları bir sanal makineye oturum açma yeteneğine sahip. |
| [Sanal makine Katılımcısı](#virtual-machine-contributor) | Sanal makineler, ancak onlara, erişimi ve sanal ağ veya depolama hesabı bağlı oldukları yönetmenize olanak tanır. |
| [Sanal makine kullanıcı oturum açma](#virtual-machine-user-login) | Bu role sahip kullanıcılar oturum açmak için bir sanal makine için normal bir kullanıcı olarak sahipsiniz. |
| [Web planı katkıda bulunan](#web-plan-contributor) | Web siteleri için web planlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Web sitesi katkıda bulunan](#website-contributor) | Web sitelerini (web planlarını değil) yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |

Aşağıdaki tablolarda her rol için verilen özel izinler açıklanmaktadır. Bu içerebilir **Eylemler**, izinleri verin ve **NotActions**, hangi kısıtlamak bunları.

## <a name="owner"></a>Sahip
Kaynaklara erişim dahil olmak üzere her şeyi yönetmenizi sağlar.

| **Eylemler** |  |
| --- | --- |
| * | Oluşturma ve tüm türlerinin kaynakları yönetme |

## <a name="contributor"></a>Katılımcı
Kaynaklara erişim hariç olmak üzere her şeyi yönetmenizi sağlar.

| **Eylemler** |  |
| --- | --- |
| * | Oluşturma ve tüm türlerinin kaynakları yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete | Rolleri ve rol atamalarını silinemiyor |
| Microsoft.Authorization/*/Write | Rolleri ve rol atamalarını oluşturulamıyor |
| Microsoft.Authorization/elevateAccess/Action | Çağırana kiracı kapsamında Kullanıcı Erişim Yöneticisi erişimi verir |

## <a name="reader"></a>Okuyucu
Her şeyi görüntülemenizi sağlar ancak değişiklik yapmanıza izin vermez.

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |

## <a name="api-management-service-contributor"></a>API Yönetimi Hizmeti Katılımcısı
Hizmeti ve API'leri yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/service/* | Oluşturma ve API Management hizmeti yönetme |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="api-management-service-operator-role"></a>API Management Hizmet Operatörü Rolü
Hizmeti yönetebilir, ancak API'leri yönetemez

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
| Microsoft.ApiManagement/service/backup/action | Bir kullanıcı belirtilen kapsayıcıda yedekleme API Management hizmetine sağlanan depolama hesabı |
| Microsoft.ApiManagement/service/delete | API Management hizmeti örneği Sil |
| Microsoft.ApiManagement/service/managedeployments/action | SKU/birimlerini, API Management Hizmet Ekle/Kaldır bölgesel dağıtımları değiştirme |
| Microsoft.ApiManagement/service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
| Microsoft.ApiManagement/service/restore/action | API Management hizmeti belirtilen bir kullanıcı tarafından sağlanan depolama hesabı kapsayıcısında geri yükleme |
| Microsoft.ApiManagement/service/updatecertificate/action | Bir API Management hizmeti için SSL sertifikasını karşıya yükle |
| Microsoft.ApiManagement/service/updatehostname/action | Kurulum, güncelleştirmeniz ya da bir API Management hizmeti için özel etki alanı adlarını kaldırın |
| Microsoft.ApiManagement/service/write | API Management hizmeti yeni bir örneğini oluşturma |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini al |

## <a name="api-management-service-reader-role"></a>API Management Hizmet Okuyucusu Rolü
Hizmet ve API'lere salt okunur erişim

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
| Microsoft.ApiManagement/service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini al |

## <a name="application-insights-component-contributor"></a>Application Insights Bileşeni Katılımcısı
Application Insights bileşenlerini yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Insights/components/* | Oluşturma ve Insights bileşenlerini yönetme |
| Microsoft.Insights/webtests/* | Oluşturma ve web testleri yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="application-insights-snapshot-debugger"></a>Application Insights Anlık Görüntü Hata Ayıklayıcısı
Kullanıcıya Application Insights Anlık Görüntü Hata Ayıklayıcısı özelliklerini kullanma izni verir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/components/*/read |  |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="automation-job-operator"></a>Otomasyon İşi İşleci
Otomasyon Runbook'larını kullanarak İş oluşturun ve yönetin.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışını alır |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Bir Azure Otomasyonu işini askıya alır |
| Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="automation-operator"></a>Otomasyon Operatörü
Otomasyon Operatörleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışını alır |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Bir Azure Otomasyonu işini askıya alır |
| Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Bir Azure Otomasyonu iş zamanlaması alır |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Bir Azure Otomasyonu iş zamanlaması oluşturur |
| Microsoft.Automation/automationAccounts/linkedWorkspace/read | Otomasyon hesabı bağlı çalışma alır |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
| Microsoft.Automation/automationAccounts/read | Bir Azure Otomasyonu hesabını alır |
| Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
| Microsoft.Automation/automationAccounts/schedules/read | Bir Azure Otomasyonu zamanlama varlığını alır |
| Microsoft.Automation/automationAccounts/schedules/write | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="automation-runbook-operator"></a>Otomasyon Runbook'u İşleci
Runbook'un İşlerini oluşturabilmek için Runbook özelliklerini okuyun.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="azure-stack-registration-owner"></a>Azure Stack Kayıt Sahibi
Azure Stack kayıtlarını yönetmenize imkan sağlar.

| **Eylemler** |  |
| --- | --- |
| Microsoft.AzureStack/registrations/products/listDetails/action | Bir Azure yığın Market ürün ayrıntılarını genişletilmiş alır |
| Microsoft.AzureStack/registrations/products/read | Bir Azure yığın Market ürün özelliklerini alır |
| Microsoft.AzureStack/registrations/read | Bir Azure yığın kaydı özelliklerini alır |

## <a name="backup-contributor"></a>Yedekleme Katılımcısı
Yedekleme hizmetini yönetmenize olanak sağlar ancak kasa oluşturma ve diğer kullanıcılara erişim verme izni sağlamaz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Yedekleme Yönetimi işleminin sonuçlarını yönetme |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Oluşturma ve kurtarma Hizmetleri kasasına yedekleme yapılarındaki yedekleme kapsayıcılarda yönetme |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Oluşturma ve yedekleme yönetimiyle ilgili meta verileri yönetme |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme Yönetimi işlemlerinin sonuçlarını yönetme |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Yedekleme ilkeleri oluşturun ve yönetin |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilir öğelerini oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Yedeklenen öğelerini oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Oluşturma ve yedekleme öğelerini tutan kapsayıcı yönetme |
| Microsoft.RecoveryServices/Vaults/certificates/* | Kurtarma Hizmetleri kasasına yedekleme ilgili sertifikaları oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve kasa ilgili genişletilmiş bilgilerini yönetme |
| Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
| Microsoft.RecoveryServices/Vaults/usages/* | Oluşturma ve kurtarma Hizmetleri kasası kullanımını yönetme |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="backup-operator"></a>Yedekleme İşleci
Yedekleme kaldırma, kasa oluşturma ve diğer kullanıcılara erişim verme dışındaki yedekleme hizmetlerini yönetmenize olanak sağlar

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/backup/action | Korumalı Öğe için Yedekleme gerçekleştirir. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Korumalı öğe nesne ayrıntılarını döndürür |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/restore/action | Korumalı Öğeler için Kurtarma Noktalarını geri yükleyin. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Bir yedekleme korumalı öğesi oluşturma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | İşi iptal |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm iş nesneleri döndürür |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Kurtarma Hizmetleri Kasası için Yedekleme Yönetimi Meta Verilerini döndürür. |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme Yönetimi işlemlerinin sonuçlarını yönetme |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkeleri döndürür |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilir öğelerini oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Tüm Korunabilir Öğelerin listesini döndürür. |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğine ait tüm kapsayıcıları döndürür |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
| Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Hizmet kapsayıcısı kaydetme işlemi, bir kapsayıcı kurtarma Hizmeti'ne kaydolmak için kullanılabilir. |
| Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Korumalı öğe için sağlama anlık öğe kurtarma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Anlık öğe kurtarma için korumalı öğe iptal etme |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
| Microsoft.RecoveryServices/Vaults/certificates/write | Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="backup-reader"></a>Yedekleme Okuyucusu
Yedekleme hizmetlerini görüntüleyebilir ancak değişiklik yapamaz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Korumalı öğe nesne ayrıntılarını döndürür |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm iş nesneleri döndürür |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Kurtarma Hizmetleri Kasası için Yedekleme Yönetimi Meta Verilerini döndürür. |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Kurtarma Hizmetleri Kasası için Yedekleme işleminin Sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkeleri döndürür |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğine ait tüm kapsayıcıları döndürür |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
| Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |

## <a name="billing-reader"></a>Faturalandırma Okuyucusu
Faturalandırma verilerine okuma erişimi verir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Billing/*/read | Faturalama bilgileri okuyun |
| Microsoft.Consumption/*/read |  |
| Microsoft.Commerce/*/read |  |
| Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="biztalk-contributor"></a>BizTalk Katılımcısı
BizTalk hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.BizTalkServices/BizTalk/* | Oluşturma ve BizTalk services yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-endpoint-contributor"></a>CDN Uç Noktası Katkıda Bulunanı
CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/* |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-endpoint-reader"></a>CDN Uç Nokta Okuyucusu
CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/*/read |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-profile-contributor"></a>CDN Profili Katkıda Bulunanı
CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/* |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-profile-reader"></a>CDN Profil Okuyucusu
CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/*/read |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-network-contributor"></a>Klasik Ağ Katılımcısı
Klasik ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.ClassicNetwork/* | Oluşturun ve klasik ağları yönetin |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-storage-account-contributor"></a>Klasik Depolama Hesabı Katılımcısı
Klasik depolama hesaplarını yönetmenize izin verir ancak bu hesaplara erişim hakkı vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.ClassicStorage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-storage-account-key-operator-service-role"></a>Klasik Depolama Hesabı Anahtarı İşleci Hizmet Rolü
Klasik Depolama Hesabı Anahtarı İşleçlerine, Klasik Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.ClassicStorage/storageAccounts/listkeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
| Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Depolama hesabı için var olan erişim anahtarlarını yeniden oluşturur. |

## <a name="classic-virtual-machine-contributor"></a>Klasik Sanal Makine Katılımcısı
Klasik sanal makineleri, ancak onlara, erişimi ve sanal ağ veya depolama hesabı bağlı oldukları yönetmenize olanak tanır.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.ClassicCompute/domainNames/* | Oluşturma ve klasik hesaplama etki alanı adlarını yönetme |
| Microsoft.ClassicCompute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
| Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir IP'ye bağlantı oluşturun |
| Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış IP'leri alır |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağa katılır. |
| Microsoft.ClassicNetwork/virtualNetworks/read | Sanal ağı alın. |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskini döndürür. |
| Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü döndürür. |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
| Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba yönelik depolama hesabını döndürün. |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Katılımcısı
ClearDB MySQL veritabanlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| successbricks.cleardb/Databases/* | Oluşturma ve ClearDB MySQL veritabanları yönetme |

## <a name="cosmos-db-account-reader-role"></a>Cosmos DB Hesabı Okuyucusu Rolü
Azure Cosmos DB hesap verileri okuyabilir. Bkz: [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetmek için.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları, her bir kullanıcıya verilen izinler okuyabilir |
| Microsoft.DocumentDB/*/read | Herhangi bir koleksiyonu okuma |
| Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Veritabanı hesabı readonly anahtarları okur. |
| Microsoft.Insights/MetricDefinitions/read | Ölçüm tanımlarını oku |
| Microsoft.Insights/Metrics/read | Ölçümleri okuma |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="data-factory-contributor"></a>Data Factory Katılımcısı
Veri fabrikaları ile birlikte bunların alt kaynaklarını oluşturup yönetin.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.DataFactory/dataFactories/* | Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin. |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="data-lake-analytics-developer"></a>Data Lake Analytics Geliştiricisi
İşlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics hesabı oluşturmanıza veya silmenize izin vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.BigAnalytics/accounts/* |  |
| Microsoft.DataLakeAnalytics/accounts/* |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.BigAnalytics/accounts/Delete |  |
| Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
| Microsoft.BigAnalytics/accounts/Write |  |
| Microsoft.DataLakeAnalytics/accounts/Delete | Bir DataLakeAnalytics hesabı silin. |
| Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen işleri iptal etme izni verin. |
| Microsoft.DataLakeAnalytics/accounts/Write | Oluşturun veya bir DataLakeAnalytics hesabı güncelleştirin. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Oluşturun veya bir bağlantılı DataLakeStore hesabı DataLakeAnalytics hesabının güncelleştirin. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | DataLakeAnalytics hesap DataLakeStore hesabından bağlantısını. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Oluşturun veya bağlı bir depolama hesabında DataLakeAnalytics hesabının güncelleştirin. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Bir depolama hesabı DataLakeAnalytics hesabından bağlantısını. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Oluşturun veya bir güvenlik duvarı kuralı güncelleştirin. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını siler. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Oluşturun veya bir işlem İlkesi güncelleştirin. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Bir işlem ilkeyi silin. |

## <a name="devtest-labs-user"></a>DevTest Labs Kullanıcısı
Azure DevTest Labs'teki tüm sanal makinelerinize bağlanmanıza, bu makineleri başlatmanıza, yeniden başlatmanıza ve kapatmanıza izin verir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Compute/availabilitySets/read | Bir kullanılabilirlik kümesinin özelliklerini al |
| Microsoft.Compute/virtualMachines/*/read | Bir sanal makine (VM boyutları, çalışma zamanı durumu, VM uzantıları, vb.) özelliklerini okuma |
| Microsoft.Compute/virtualMachines/deallocate/action | Sanal makineyi kapatır ve işlem kaynaklarını serbest bırakır |
| Microsoft.Compute/virtualMachines/read | Bir sanal makinenin özelliklerini alır |
| Microsoft.Compute/virtualMachines/restart/action | Sanal makineyi yeniden başlatır |
| Microsoft.Compute/virtualMachines/start/action | Sanal makineyi başlatır |
| Microsoft.DevTestLab/*/read | Bir laboratuvar özelliklerini okuma |
| Microsoft.DevTestLab/labs/createEnvironment/action | Sanal makineler bir laboratuar ortamında oluşturun. |
| Microsoft.DevTestLab/labs/claimAnyVm/action | Laboratuvar rastgele claimable sanal makinede talep. |
| Microsoft.DevTestLab/labs/formulas/delete | Formülleri silin. |
| Microsoft.DevTestLab/labs/formulas/read | Formülleri okuyun. |
| Microsoft.DevTestLab/labs/formulas/write | Ekleyin veya formüller değiştirin. |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Laboratuvar ilkeyi değerlendirir. |
| Microsoft.DevTestLab/labs/virtualMachines/claim/action | Var olan bir sanal makine sahipliğini alın |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzuna katılır |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralına katılır |
| Microsoft.Network/networkInterfaces/*/read | Bir ağ arabirimi (ağ arabirimi bir parçası olan Örneğin, tüm yük dengeleyicileri) özelliklerini okuma |
| Microsoft.Network/networkInterfaces/join/action | Bir sanal makine bir ağ arabirimiyle birleştirir |
| Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımını alır.  |
| Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya mevcut bir ağ arabirimini güncelleştirir.  |
| Microsoft.Network/publicIPAddresses/*/read | Bir ortak IP adresinin özelliklerini okuma |
| Microsoft.Network/publicIPAddresses/join/action | Bir genel ip adresine katılır |
| Microsoft.Network/publicIPAddresses/read | Bir genel ip adresi tanımını alır. |
| Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağ birleştirir |
| Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
| Microsoft.Resources/deployments/read | Dağıtımları alır veya listeler. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |

| **NotActions** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makineyi güncelleştirmek için kullanılabilir boyutları listeler |

## <a name="dns-zone-contributor"></a>DNS Bölgesi Katkıda Bulunanı
Azure DNS'te, DNS bölgelerini ve kayıt kümelerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/dnsZones/* | DNS bölgeleri ve kayıtları oluşturma ve yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="documentdb-account-contributor"></a>DocumentDB Hesabı Katılımcısı
Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB önceden DocumentDB bilinirdi.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.DocumentDb/databaseAccounts/* | Azure Cosmos DB hesapları oluşturma ve yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="intelligent-systems-account-contributor"></a>Akıllı Sistemler Hesap Katılımcısı
Akıllı Sistemler hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.IntelligentSystems/accounts/* | Akıllı sistemler hesapları oluşturma ve yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="key-vault-contributor"></a>Key Vault Katkıda Bulunanı
Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişmenize izin vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.KeyVault/* |  |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.KeyVault/locations/deletedVaults/purge/action | Geçici silinen bir anahtar kasasını temizleyin |
| Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>Laboratuvar Oluşturucusu
Oluşturma, yönetme, Azure Laboratuvar hesaplarınızı altında yönetilen labs silme olanak sağlar.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.ManagedLab/labAccounts/createLab/action | Bir laboratuvar bir laboratuvar hesabı oluşturun. |
| Microsoft.ManagedLab/labAccounts/*/read |  |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunan
Günlük analizi katkıda bulunan tüm izleme verilerini okuma ve izleme ayarlarını düzenleyin. İzleme ayarları düzenleme, VM'ler için VM uzantısı eklemeyi içerir; Azure Storage günlüklerinden koleksiyonu yapılandırmak için depolama hesabı anahtarlarını okuma; oluşturma ve Automation hesapları yapılandırma; çözümleri ekleme; ve tüm Azure kaynaklara Azure tanılama yapılandırılıyor.

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.Automation/automationAccounts/* |  |
| Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
| Microsoft.Compute/virtualMachines/extensions/* |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/diagnosticSettings/* | Analysis Server tanılama ayarını okur, güncelleştirir veya oluşturur |
| Microsoft.OperationalInsights/* |  |
| Microsoft.OperationsManagement/* |  |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
| Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="log-analytics-reader"></a>Log Analytics Okuyucusu
Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir.

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.OperationalInsights/workspaces/analytics/query/action | Yeni altyapısını kullanarak arayın. |
| Microsoft.OperationalInsights/workspaces/search/action | Bir arama sorgusunu çalıştırır |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.OperationalInsights/workspaces/sharedKeys/read | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |

## <a name="logic-app-contributor"></a>Mantıksal Uygulama Katkıda Bulunanı
Mantıksal uygulamayı yönetmenize izin verir, ancak bunlara yönelik erişimi yönetmenize izin vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
| Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba yönelik depolama hesabını döndürün. |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/diagnosticSettings/* | Analysis Server tanılama ayarını okur, güncelleştirir veya oluşturur |
| Microsoft.Insights/logdefinitions/* | Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğü günlük kategorilerini liste. |
| Microsoft.Insights/metricDefinitions/* | Ölçüm tanımlarını (bir kaynak için kullanılabilir ölçüm türlerinin listesi) okuyun. |
| Microsoft.Logic/* | Logic Apps kaynaklarını yönetir. |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
| Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/connectionGateways/* | Oluşturma ve bir bağlantı ağ geçidi yönetir. |
| Microsoft.Web/connections/* | Oluşturma ve bir bağlantı yönetir. |
| Microsoft.Web/customApis/* | Oluşturur ve bir özel API yönetir. |
| Microsoft.Web/serverFarms/join/action |  |
| Microsoft.Web/serverFarms/read | Bir uygulama hizmeti planı üzerinde özelliklerini alma |
| Microsoft.Web/sites/functions/listSecrets/action | Liste gizli Web Apps işlevleri. |

## <a name="logic-app-operator"></a>Mantıksal Uygulama Operatörü
Mantıksal uygulamayı okumanıza, etkinleştirmenize ve devre dışı bırakmanıza izin verir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/*/read | Okuma Öngörüler uyarı kuralları |
| Microsoft.Insights/diagnosticSettings/*/read | Logic Apps için tanılama ayarlarını alır |
| Microsoft.Insights/metricDefinitions/*/read | Logic Apps için kullanılabilir ölçümleri alır. |
| Microsoft.Logic/*/read | Logic Apps kaynaklarını okur. |
| Microsoft.Logic/workflows/disable/action | İş akışını devre dışı bırakır. |
| Microsoft.Logic/workflows/enable/action | İş akışını etkinleştirir. |
| Microsoft.Logic/workflows/validate/action | İş akışını doğrular. |
| Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
| Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/connectionGateways/*/read | Bağlantı ağ geçidi okuyun. |
| Microsoft.Web/connections/*/read | Bağlantıları okuyun. |
| Microsoft.Web/customApis/*/read | Özel API okuyun. |
| Microsoft.Web/serverFarms/read | Bir uygulama hizmeti planı üzerinde özelliklerini alma |

## <a name="managed-identity-contributor"></a>Yönetilen kimlik katkıda bulunan
Oluşturma, okuma, güncelleştirme ve kullanıcı kimliği atanır silme

| **Eylemler** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="managed-identity-operator"></a>Yönetilen kimlik işleci
Okuma ve kullanıcı kimliği atanır atayın

| **Eylemler** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="monitoring-contributor"></a>İzleme Katkıda Bulunanı
Tüm izleme verileri okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.AlertsManagement/alerts/* |  |
| Microsoft.AlertsManagement/alertsSummary/* |  |
| Microsoft.Insights/AlertRules/* | Okuma/yazma/silme uyarı kuralları. |
| Microsoft.Insights/components/* | Okuma/yazma/silme Application Insights bileşenleri. |
| Microsoft.Insights/DiagnosticSettings/* | Okuma/yazma/silme tanılama ayarları. |
| Microsoft.Insights/eventtypes/* | Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, etkinlik günlüğü programlı ve portal erişimi için geçerlidir. |
| Microsoft.Insights/LogDefinitions/* | Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğü günlük kategorilerini liste. |
| Microsoft.Insights/MetricDefinitions/* | Ölçüm tanımlarını (bir kaynak için kullanılabilir ölçüm türlerinin listesi) okuyun. |
| Microsoft.Insights/Metrics/* | Bir kaynak için ölçümleri okuyun. |
| Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
| Microsoft.Insights/webtests/* | Okuma/yazma/silme Application Insights testleri web. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Okuma/yazma/silme günlük analizi çözüm paketleri. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* | Okuma/yazma/silme günlük analizi kayıtlı aramalar. |
| Microsoft.OperationalInsights/workspaces/search/action | Bir arama sorgusunu çalıştırır |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Okuma/yazma/silme günlük analizi depolama Insight yapılandırmaları. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>İzleme Okuyucusu
Tüm izleme verilerini (ölçümleri, günlükleri, vb.) okuyabilir. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.OperationalInsights/workspaces/search/action | Bir arama sorgusunu çalıştırır |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="network-contributor"></a>Ağ Katılımcısı
Ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/* | Oluşturun ve ağları yönetin |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="new-relic-apm-account-contributor"></a>New Relic APM Hesap Katılımcısı
New Relic Application Performance Management hesaplarını ve uygulamalarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| NewRelic.APM/accounts/* |  |

## <a name="redis-cache-contributor"></a>Redis Cache Katılımcısı
Redis Cache'leri yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Cache/redis/* | Oluşturma ve Redis önbellekleri yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="scheduler-job-collections-contributor"></a>Zamanlayıcı İş Koleksiyonları Katılımcısı
Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Scheduler/jobcollections/* | Oluşturma ve iş koleksiyonları yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="search-service-contributor"></a>Search Hizmeti Katılımcısı
Search hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Search/searchServices/* | Oluşturma ve arama hizmetleri yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="security-admin"></a>Güvenlik Yöneticisi
Yalnızca Güvenlik Merkezi'nde: görüntülemek güvenlik ilkeleri, güvenlik durumları görüntülemek, güvenlik ilkeleri, Uyarıları görüntüle ve öneriler düzenleme, uyarısı ve öneri yok sayın

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Authorization/policyAssignments/* | Oluşturma ve ilke atamalarını yönetme |
| Microsoft.Authorization/policyDefinitions/* | Oluşturma ve ilke tanımları yönetme |
| Microsoft.Authorization/policySetDefinitions/* | Oluşturun ve ilke kümelerini yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.operationalInsights/workspaces/*/read | Günlük analizi verilerini görüntüleme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |
| Microsoft.Security/locations/alerts/dismiss/action | Bir güvenlik uyarısını kapatmanın |
| Microsoft.Security/locations/tasks/dismiss/action | Güvenlik açısından kapatın |
| Microsoft.Security/policies/write | Güvenlik İlkesi güncelleştirmeleri |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="security-manager"></a>Güvenlik Yöneticisi
Güvenlik bileşenlerini, güvenlik ilkelerini ve sanal makineleri yönetmenizi sağlar

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.ClassicCompute/*/read | Klasik sanal makineleri yapılandırma bilgilerini okumak |
| Microsoft.ClassicCompute/virtualMachines/*/write | Klasik sanal makineler için yapılandırma yazma |
| Microsoft.ClassicNetwork/*/read | Klasik ağ hakkında yapılandırma bilgilerini okuyun |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Security/* | Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="security-reader"></a>Güvenlik Okuyucu
Yalnızca Güvenlik Merkezi'nde: önerileri ve uyarılar, güvenlik ilkeleri, güvenlik durumları görüntüleyebilir ancak değişiklik yapamaz görünümü görüntüleyebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.operationalInsights/workspaces/*/read | Günlük analizi verilerini görüntüleme |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |

## <a name="site-recovery-contributor"></a>Site Recovery Katkıda Bulunanı
Kasa oluşturma ve rol atama işlemleri dışında Site Recovery hizmetini yönetmenize imkan sağlar

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir |
| Microsoft.RecoveryServices/Vaults/certificates/write | Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve kasa ilgili genişletilmiş bilgilerini yönetme |
| Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Oluşturma veya çoğaltma uyarı ayarlarını güncelleştirme |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Oluşturma ve çoğaltma yapıları yönetme |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Çoğaltma ilkeleri oluşturun ve yönetin |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Oluşturma ve kurtarma planlarını yönetme |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Kurtarma Hizmetleri kasası, depolama yapılandırması oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür. |
| Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read |  |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="site-recovery-operator"></a>Site Recovery Operatörü
Yük devretme ve yeniden çalışma dışındaki Site Recovery yönetimi işlemlerini gerçekleştirmenize izin vermez

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
| Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm uyarı ayarlarını okuma |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Doku Tutarlılığını Denetler |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Ağ geçidi yeniden ilişkilendirin |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Yapı sertifikasını Yenile |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/okuma | Tüm ağ eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/okuma | Herhangi bir koruma kapsayıcıdaki okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Korunabilir tüm öğeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Kurtarma noktası Uygula |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Yük devretmenin yürütülmesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlanan yük devretme |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Bir çoğaltma kurtarma noktası okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Onarım çoğaltma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Korumalı öğe koruyun |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/action | Yük Devretme Sınaması |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Yük devretme sınaması temizliğini |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Yük devretme |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility hizmeti güncelleştirmesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/okuma | Koruma kapsayıcısı eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Tüm kurtarma Hizmetleri sağlayıcılarını okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider/eylem | Sağlayıcı Yenile |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/okuma | Tüm depolama sınıflandırması eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Herhangi bir işi okuma |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Yük devretmenin yürütülmesi kurtarma planı |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planlanan yük devretme kurtarma planı |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Tüm kurtarma planlarını okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Kurtarma planı koruyun |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test yük devretme kurtarma planı |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test yük devretmesi temizliğini kurtarma planı |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Yük devretme kurtarma planı |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read |  |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür. |
| Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="site-recovery-reader"></a>Site Recovery Okuyucusu
Site Recovery durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmenize izin vermez

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read |  |
| Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm uyarı ayarlarını okuma |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/okuma | Tüm ağ eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/okuma | Herhangi bir koruma kapsayıcıdaki okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Korunabilir tüm öğeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Bir çoğaltma kurtarma noktası okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/okuma | Koruma kapsayıcısı eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Tüm kurtarma Hizmetleri sağlayıcılarını okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/okuma | Tüm depolama sınıflandırması eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Herhangi bir işi okuma |
| Microsoft.RecoveryServices/vaults/replicationJobs/read | Herhangi bir işi okuma |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Tüm kurtarma planlarını okuma |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür. |
| Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="sql-db-contributor"></a>SQL DB Katılımcısı
SQL veritabanları, ancak onlara erişimi yönetmenize olanak tanır. Ayrıca, güvenlikle ilgili ilkelerini veya kendi üst SQL Server'lar yönetemezsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/databases/* | Oluşturma ve SQL veritabanlarını yönetme |
| Microsoft.Sql/servers/read | Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Denetim ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditingSettings/* | Denetim ayarlarını düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob Denetim kayıtlarını alma |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Bağlantı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Veri ilkeleri maskeleme düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Güvenlik Uyarısı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/securityMetrics/* | Güvenlik ölçümleri düzenlenemez. |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>SQL Güvenlik Yöneticisi
SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenizi sağlar ancak onlara erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma Microsoft yetkilendirme |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Sql/servers/auditingPolicies/* | SQL server denetim ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/auditingSettings/* | Oluşturma ve SQL server denetim ayarı yönetme |
| Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL server veritabanı denetim ayarlarını oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/auditRecords/read | Okuma denetim kaydı |
| Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantısı ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Oluşturma ve SQL server veritabanı veri maskeleme ilkelerini yönetme |
| Microsoft.Sql/servers/databases/read | Veritabanları veya belirtilen veritabanı özelliklerini alır listesini döndürür. |
| Microsoft.Sql/servers/databases/schemas/read | Veritabanı şemaları listesini alma |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Bir tablodaki sütunların listesini alma |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/read | Bir veritabanının tabloların listesini alma |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarısı ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/securityMetrics/* | Oluşturma ve SQL server veritabanı güvenlik ölçümleri yönetme |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/firewallRules/* |  |
| Microsoft.Sql/servers/read | Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür. |
| Microsoft.Sql/servers/securityAlertPolicies/* | SQL server güvenlik uyarısı ilkeleri oluşturun ve yönetin |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="sql-server-contributor"></a>SQL Server Katılımcısı
SQL sunucularını ve veritabanlarını yönetmenizi sağlar ancak güvenlikle ilgili ilkelerini yönetmenize izin vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/* | Oluşturun ve SQL sunucularını yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* | SQL server denetim ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/auditingSettings/* | SQL server denetim ayarları düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL server veritabanı denetim ayarları düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditRecords/read | Denetim kayıtlarının okunamıyor |
| Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server veritabanı veri ilkeleri maskeleme düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarısı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/securityMetrics/* | SQL server veritabanı güvenlik ölçümleri düzenlenemez. |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/securityAlertPolicies/* | SQL server güvenlik uyarısı ilkeleri düzenleyemezsiniz |

## <a name="storage-account-contributor"></a>Depolama Hesabı Katılımcısı
Depolama hesaplarını yönetmenize izin verir ancak bunlara yönelik erişimi yönetmenize izin vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Tüm Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/diagnosticSettings/* | Tanılama ayarlarını yönet |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="storage-account-key-operator-service-role"></a>Depolama Hesabı Anahtarı İşleci Hizmet Rolü
Depolama Hesabı Anahtarı İşleçlerine, Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
| Microsoft.Storage/storageAccounts/regeneratekey/action | Belirtilen depolama hesabının erişim anahtarlarını yeniden oluşturur. |

## <a name="support-request-contributor"></a>Destek İsteğine Katkıda Bulunan
Destek istekleri oluşturmanıza ve bunları yönetmenize olanak sağlar

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="traffic-manager-contributor"></a>Traffic Manager Katkıda Bulunanı
Traffic Manager profillerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Network/trafficManagerProfiles/* |  |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi
Azure kaynaklarına yönelik kullanıcı erişimini yönetmenizi sağlar.

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.Authorization/* | Yetkilendirme yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="virtual-machine-administrator-login"></a>Sanal makine yönetici oturum açma
-   Bu role sahip kullanıcılar oturum açmak için bir sanal makine Windows yönetici veya Linux kök kullanıcı ayrıcalıkları ile sahipsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="virtual-machine-contributor"></a>Sanal Makine Katılımcısı
Sanal makineler, ancak onlara, erişimi ve sanal ağ veya depolama hesabı bağlı oldukları yönetmenize olanak tanır.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Compute/availabilitySets/* | Oluşturun ve işlem kullanılabilirlik kümelerini yönetme |
| Microsoft.Compute/locations/* | Oluşturma ve işlem konumları yönetme |
| Microsoft.Compute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
| Microsoft.Compute/virtualMachineScaleSets/* | Oluşturma ve sanal makine ölçek kümeleri yönetme |
| Microsoft.DevTestLab/schedules/* |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Bir uygulama ağ geçidi arka uç adres havuzuna katılır |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzuna katılır |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Katıldığı bir yük dengeleyici gelen nat havuzu |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralına katılır |
| Microsoft.Network/loadBalancers/probes/join/action | Bir yük dengeleyicinin araştırmalar kullanarak sağlar. Örneğin, VM ölçek bu izni healthProbe özelliği ile kümesi araştırma başvuruda bulunabilir. |
| Microsoft.Network/loadBalancers/read | Yük Dengeleyici tanımını alır |
| Microsoft.Network/locations/* | Oluşturma ve ağ konumları yönetme |
| Microsoft.Network/networkInterfaces/* | Oluşturma ve ağ arabirimlerinin yönetme |
| Microsoft.Network/networkSecurityGroups/join/action | Ağ güvenlik grubuna katılır |
| Microsoft.Network/networkSecurityGroups/read | Ağ güvenlik grubu tanımını alır |
| Microsoft.Network/publicIPAddresses/join/action | Bir genel ip adresine katılır |
| Microsoft.Network/publicIPAddresses/read | Bir genel ip adresi tanımını alır. |
| Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
| Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağ birleştirir |
| Microsoft.RecoveryServices/locations/* |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/*/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Korumalı öğe nesne ayrıntılarını döndürür |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Bir yedekleme korumalı öğesi oluşturma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Bir yedekleme koruma hedefini oluşturma |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkeleri döndürür |
| Microsoft.RecoveryServices/Vaults/backupPolicies/write | Koruma ilkesi oluşturur |
| Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
| Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
| Microsoft.RecoveryServices/Vaults/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
| Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="virtual-machine-user-login"></a>Sanal makine kullanıcı oturum açma
Bu role sahip kullanıcılar oturum açmak için bir sanal makine için normal bir kullanıcı olarak sahipsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="web-plan-contributor"></a>Web Planı Katılımcısı
Web siteleri için web planlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/serverFarms/* | Oluşturma ve sunucu grupları yönetme |

## <a name="website-contributor"></a>Web Sitesi Katılımcısı
Web sitelerini (web planlarını değil) yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/components/* | Oluşturma ve Insights bileşenlerini yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/certificates/* | Web sitesi sertifikaları oluşturmak ve yönetmek |
| Microsoft.Web/listSitesAssignedToHostName/read | Ana bilgisayar adı için atanmış siteler adlarını alır. |
| Microsoft.Web/serverFarms/join/action |  |
| Microsoft.Web/serverFarms/read | Bir uygulama hizmeti planı üzerinde özelliklerini alma |
| Microsoft.Web/sites/* | Oluşturma ve (site oluşturma ilişkili uygulama hizmeti planı yazma izinleri de gerektirir) Web sitelerini yönetme |

## <a name="see-also"></a>Ayrıca bkz.
* [Rol tabanlı erişim denetimi](role-based-access-control-configure.md): Azure portalında RBAC ile çalışmaya başlama.
* [Azure rbac'de özel roller](role-based-access-control-custom-roles.md): erişim gereksinimlerinize uyacak şekilde özel roller oluşturma hakkında bilgi edinin.
* [Erişim değişiklik geçmişi raporu oluşturma](role-based-access-control-access-change-history-report.md): RBAC içinde rol atamalarını değiştirme izler.
* [Rol tabanlı erişim denetimi sorun giderme](role-based-access-control-troubleshooting.md): sık karşılaşılan sorunları düzeltmek için öneriler alın.
* [Azure Güvenlik Merkezi'nde izinleri](../security-center/security-center-permissions.md)
