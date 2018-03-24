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
ms.openlocfilehash: 0e8f1d97f51fbfedbca1619ef5e7f28a3a0570b9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
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
| [Sahibi](#owner) | Erişim dahil her şeyi yönetebilir |
| [Katkıda bulunan](#contributor) | Erişim dışında her şeyi yönetebilir |
| [Reader](#reader) | Her şeyi görüntüleyebilir ancak değişiklik yapamaz |
| [API Management hizmeti katkıda bulunan](#api-management-service-contributor) | API Management Hizmetleri yönetebilir. |
| [API Management hizmet işleci rolü](#api-management-service-operator-role) | API Management Hizmetleri yönetebilir. |
| [API Management hizmet okuyucu rolü](#api-management-service-reader-role) | API Management Hizmetleri yönetebilir. |
| [Uygulama Öngörüler bileşen katkıda bulunan](#application-insights-component-contributor) | Application Insights bileşenlerini yönetebilir |
| [Uygulama Öngörüler anlık görüntü hata ayıklayıcı](#application-insights-snapshot-debugger) | Kullanıcıya Application Insights Anlık Görüntü Hata Ayıklayıcısı özelliklerini kullanma izni verir |
| [Otomasyon iş işleci](#automation-job-operator) | Otomasyon Runbook'larını kullanarak İş oluşturun ve yönetin. |
| [Automation operatörü](#automation-operator) | Başlatma, durdurma, askıya alma ve işlerini sürdürmek için |
| [Otomasyon Runbook işleci](#automation-runbook-operator) | Runbook'un İşlerini oluşturabilmek için Runbook özelliklerini okuyun. |
| [Azure yığın kayıt sahibi](#azure-stack-registration-owner) | Azure Stack kayıtlarını yönetmenize imkan sağlar. |
| [Yedekleme katkıda bulunan](#backup-contributor) | Kurtarma Hizmetleri kasası oluşturmaya ve erişim başkalarına verip dışındaki tüm yedekleme yönetimi eylemleri yönetebilirsiniz |
| [Yedekleme işletmeni](#backup-operator) | Yedekleme ve vermiş erişim başkalarına kaldırma kasalarını oluşturma dışındaki tüm yedekleme yönetimi eylemleri yönetebilirsiniz |
| [Yedekleme okuyucusu](#backup-reader) | Kurtarma Hizmetleri kasasına yedekleme yönetimini izleyebilirsiniz |
| [Faturalama okuyucusu](#billing-reader) | Tüm faturalama bilgileri görüntüleyebilirsiniz |
| [BizTalk katkıda bulunan](#biztalk-contributor) | BizTalk Hizmetleri yönetebilir. |
| [CDN uç noktası katkıda bulunan](#cdn-endpoint-contributor) | CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
| [CDN Endpoint Reader](#cdn-endpoint-reader) | CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
| [CDN profili katkıda bulunan](#cdn-profile-contributor) | CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
| [CDN profili okuyucusu](#cdn-profile-reader) | CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
| [Klasik Ağ Katılımcısı](#classic-network-contributor) | Klasik sanal ağlar ve ayrılmış IP yönetebilir |
| [Klasik depolama hesabı katkıda bulunan](#classic-storage-account-contributor) | Klasik depolama hesaplarını yönetme |
| [Klasik depolama hesabı anahtar işleci hizmet rolü](#classic-storage-account-key-operator-service-role) | Klasik Depolama Hesabı Anahtarı İşleçlerine, Klasik Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
| [Klasik sanal makine Katılımcısı](#classic-virtual-machine-contributor) | Klasik sanal makineleri ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz. |
| [ClearDB MySQL DB katkıda bulunan](#cleardb-mysql-db-contributor) | ClearDB MySQL veritabanları yönetebilirsiniz |
| [Cosmos DB hesap okuyucu rolü](#cosmos-db-account-reader-role) | Azure Cosmos DB hesap verileri okuyabilir. Bkz: [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetmek için. |
| [Veri Fabrikası katkıda bulunan](#data-factory-contributor) | Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin. |
| [Data Lake Analytics Geliştirici](#data-lake-analytics-developer) | İşlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics hesabı oluşturmanıza veya silmenize izin vermez. |
| [DevTest Labs kullanıcı](#devtest-labs-user) | Her şeyi görüntüleyebilir ve bağlanmak, Başlat, yeniden başlatma ve kapatma sanal makineler |
| [DNS bölgesi katkıda bulunan](#dns-zone-contributor) | DNS bölgeleri ve kayıtları yönetebilirsiniz. |
| [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) | Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB önceden DocumentDB bilinirdi. |
| [Akıllı sistemler hesap katkıda bulunan](#intelligent-systems-account-contributor) | Akıllı sistemler hesaplarını yönetme |
| [Anahtar kasası katkıda bulunan](#key-vault-contributor) | Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişmenize izin vermez. |
| [Laboratuvar Oluşturucusu](#lab-creator) | Oluşturma, yönetme, Azure Laboratuvar hesaplarınızı altında yönetilen labs silme olanak sağlar. |
| [Günlük analizi katkıda bulunan](#log-analytics-contributor) | Günlük analizi katkıda bulunan tüm izleme verilerini okuma ve izleme ayarlarını düzenleyin. İzleme ayarları düzenleme, VM'ler için VM uzantısı eklemeyi içerir; Azure Storage günlüklerinden koleksiyonu yapılandırmak için depolama hesabı anahtarlarını okuma; oluşturma ve Automation hesapları yapılandırma; çözümleri ekleme; ve tüm Azure kaynaklara Azure tanılama yapılandırılıyor. |
| [Log Analytics Reader](#log-analytics-reader) | Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir. |
| [Mantığı uygulamasını katkıda bulunan](#logic-app-contributor) | Mantıksal uygulamayı yönetmenize izin verir, ancak bunlara yönelik erişimi yönetmenize izin vermez. |
| [Mantıksal uygulama işleci](#logic-app-operator) | Mantıksal uygulamayı okumanıza, etkinleştirmenize ve devre dışı bırakmanıza izin verir. |
| [Yönetilen kimlik katkıda bulunan](#managed-identity-contributor) | Oluşturma, okuma, güncelleştirme ve kullanıcı kimliği atanır silme |
| [Yönetilen kimlik işleci](#managed-identity-operator) | Okuma ve kullanıcı kimliği atanır atayın |
| [Katkıda bulunan izleme](#monitoring-contributor) | Tüm izleme verileri okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Okuyucu izleme](#monitoring-reader) | Tüm izleme verilerini (ölçümleri, günlükleri, vb.) okuyabilir. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Ağ Katılımcısı](#network-contributor) | Tüm ağ kaynakları yönetebilir |
| [Yeni Relic APM hesap katkıda bulunan](#new-relic-apm-account-contributor) | New Relic Application Performance Management hesaplarını ve uygulamalarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Redis önbelleği katkıda bulunan](#redis-cache-contributor) | Redis önbellekleri yönetebilirsiniz. |
| [Zamanlayıcı İş koleksiyonları katkıda bulunan](#scheduler-job-collections-contributor) | Zamanlayıcı İş koleksiyonları yönetebilir |
| [Arama hizmeti katkıda bulunan](#search-service-contributor) | Arama Hizmetleri yönetebilir. |
| [Güvenlik Yöneticisi](#security-admin) | Yalnızca Güvenlik Merkezi'nde: görüntülemek güvenlik ilkeleri, güvenlik durumları görüntülemek, güvenlik ilkeleri, Uyarıları görüntüle ve öneriler düzenleme, uyarısı ve öneri yok sayın |
| [Güvenlik Yöneticisi](#security-manager) | Güvenlik bileşenleri, güvenlik ilkeleri ve sanal makineleri yönetebilirsiniz |
| [Güvenlik okuyucusu](#security-reader) | Yalnızca Güvenlik Merkezi'nde: önerileri ve uyarılar, güvenlik ilkeleri, güvenlik durumları görüntüleyebilir ancak değişiklik yapamaz görünümü görüntüleyebilirsiniz |
| [Site kurtarma katkıda bulunan](#site-recovery-contributor) | Kurtarma Hizmetleri kasası oluşturma ve diğer kullanıcılara erişim hakları atama hariç tüm Site Recovery yönetim eylemleri, yönetebilirsiniz |
| [Site Recovery Operator](#site-recovery-operator) | Yük devretme ve yeniden çalışma ancak diğer Site Recovery yönetim eylemleri gerçekleştirmek veya diğer kullanıcılara erişimi atayın |
| [Site Recovery Reader](#site-recovery-reader) | Kurtarma Hizmetleri kasası Site kurtarma durumunu izleyebilir ve Destek biletlerini Yükselt |
| [SQL DB Katılımcısı](#sql-db-contributor) | SQL veritabanları ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz. |
| [SQL Güvenlik Yöneticisi](#sql-security-manager) | SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetebilirsiniz |
| [SQL Server katkıda bulunan](#sql-server-contributor) | SQL sunucuları ve veritabanları ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz. |
| [Depolama hesabı katkıda bulunan](#storage-account-contributor) | Depolama hesaplarını yönetme, ancak onlara erişimi yok. |
| [Depolama hesabı anahtar işleci hizmeti rolü](#storage-account-key-operator-service-role) | Depolama Hesabı Anahtarı İşleçlerine, Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
| [Destek isteği katkıda bulunan](#support-request-contributor) | Oluşturabilir ve abonelik kapsamında destek biletlerini yönetme |
| [Trafik Yöneticisi katkıda bulunan](#traffic-manager-contributor) | Traffic Manager profillerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
| [Kullanıcı erişimi Yöneticisi](#user-access-administrator) | Azure kaynakları için kullanıcı erişimini yönetebilirsiniz |
| [Sanal makine yönetici oturum açma](#virtual-machine-administrator-login) | -Bu rol kullanıcılarla Windows yönetici veya Linux kök kullanıcı ayrıcalıkları bir sanal makineye oturum açma yeteneğine sahip. |
| [Sanal makine Katılımcısı](#virtual-machine-contributor) | Sanal makineler ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz. |
| [Sanal makine kullanıcı oturum açma](#virtual-machine-user-login) | Bu role sahip kullanıcılar oturum açmak için bir sanal makine için normal bir kullanıcı olarak sahipsiniz. |
| [Web planı katkıda bulunan](#web-plan-contributor) | Web planlarını yönetme |
| [Web sitesi katkıda bulunan](#website-contributor) | Web siteleri ancak olmayan bağlı web planlarını yönetme |

Aşağıdaki tablolarda her rol için verilen özel izinler açıklanmaktadır. Bu içerebilir **Eylemler**, izinleri verin ve **NotActions**, hangi kısıtlamak bunları.

## <a name="owner"></a>Sahip
Erişim dahil her şeyi yönetebilir

| **Eylemler** |  |
| --- | --- |
| * | Oluşturma ve tüm türlerinin kaynakları yönetme |

## <a name="contributor"></a>Katılımcı
Erişim dışında her şeyi yönetebilir

| **Eylemler** |  |
| --- | --- |
| * | Oluşturma ve tüm türlerinin kaynakları yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete | Rolleri ve rol atamalarını silinemiyor |
| Microsoft.Authorization/*/Write | Rolleri ve rol atamalarını oluşturulamıyor |
| Microsoft.Authorization/elevateAccess/Action | Çağırana kiracı kapsamında Kullanıcı Erişim Yöneticisi erişimi verir |

## <a name="reader"></a>Okuyucu
Her şeyi görüntüleyebilir ancak değişiklik yapamaz

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |

## <a name="api-management-service-contributor"></a>API Yönetimi Hizmeti Katılımcısı
API Management Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/service/* | Oluşturma ve API Management hizmeti yönetme |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Okuma rolleri ve rol atamaları |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="api-management-service-operator-role"></a>API Management Hizmet Operatörü Rolü
API Management Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
| Microsoft.ApiManagement/service/backup/action | Bir kullanıcı tarafından sağlanan depolama hesabı belirtilen kapsayıcıda API Management hizmeti dön |
| Microsoft.ApiManagement/service/delete | API Management hizmeti örneği Sil |
| Microsoft.ApiManagement/service/managedeployments/action | Değişiklik SKU/birimleri; API Management hizmet bölgesel dağıtımları Ekle Kaldır |
| Microsoft.ApiManagement/service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
| Microsoft.ApiManagement/service/restore/action | API Management hizmeti belirtilen bir kullanıcı tarafından sağlanan depolama hesabı kapsayıcısında geri yükleme |
| Microsoft.ApiManagement/service/updatecertificate/action | Bir API Management hizmeti için SSL sertifikasını karşıya yükle |
| Microsoft.ApiManagement/service/updatehostname/action | Ayarlama, güncelleştirmek veya bir API Management hizmeti için özel etki alanı adlarını kaldırın |
| Microsoft.ApiManagement/service/write | API Management hizmeti yeni bir örneğini oluşturma |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Okuma rolleri ve rol atamaları |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini al |

## <a name="api-management-service-reader-role"></a>API Management Hizmet Okuyucusu Rolü
API Management Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
| Microsoft.ApiManagement/service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Okuma rolleri ve rol atamaları |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
Başlatma, durdurma, askıya alma ve işlerini sürdürmek için

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Automation/automationAccounts/jobs/read | Otomasyon hesabı işleri okuma |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Otomasyon hesabı işini sürdürme |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Otomasyon hesabı işini durdurma |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Otomasyon hesabı iş akışları okuma |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Bir Otomasyon hesabı işini askıya alma |
| Microsoft.Automation/automationAccounts/jobs/write | Otomasyon hesabı işleri yazma |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Bir Otomasyon hesabı iş zamanlaması okuma |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Bir Otomasyon hesabı iş zamanlaması okuma |
| Microsoft.Automation/automationAccounts/linkedWorkspace/read | Otomasyon hesabı bağlı çalışma alır |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
| Microsoft.Automation/automationAccounts/read | Automation hesapları okuma |
| Microsoft.Automation/automationAccounts/runbooks/read | Otomasyon runbook'ları okuma |
| Microsoft.Automation/automationAccounts/schedules/read | Otomasyon hesabı zamanlamaları okuma |
| Microsoft.Automation/automationAccounts/schedules/write | Otomasyon hesabı zamanlamaları yazma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
Kurtarma Hizmetleri kasası oluşturmaya ve erişim başkalarına verip dışındaki tüm yedekleme yönetimi eylemleri yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Yedekleme Yönetimi işleminin sonuçlarını yönetme |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Oluşturma ve kurtarma Hizmetleri kasasına yedekleme yapılarındaki yedekleme kapsayıcılarda yönetme |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Bir Excel'e yedekleme işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Oluşturma ve yedekleme yönetimiyle ilgili meta verileri yönetme |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme Yönetimi işlemlerinin sonuçlarını yönetme |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Yedekleme ilkeleri oluşturun ve yönetin |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilir öğelerini oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Yedeklenen öğelerini oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Oluşturma ve yedekleme öğelerini tutan kapsayıcı yönetme |
| Microsoft.RecoveryServices/Vaults/certificates/* | Kurtarma Hizmetleri kasasına yedekleme ilgili sertifikaları oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve kasa ilgili genişletilmiş bilgilerini yönetme |
| Microsoft.RecoveryServices/Vaults/read | Kurtarma Hizmetleri kasaları okuma |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
| Microsoft.RecoveryServices/Vaults/usages/* | Oluşturma ve kurtarma Hizmetleri kasası kullanımını yönetme |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="backup-operator"></a>Yedekleme İşleci
Yedekleme ve vermiş erişim başkalarına kaldırma kasalarını oluşturma dışındaki tüm yedekleme yönetimi eylemleri yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Yedekleme Yönetimi işlemi sonuçlarını okuyun |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | İşlem sonuçları koruma kapsayıcılarında okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/backup/action | Yedeklenen öğesi talep üzerine yedekleme işlemi gerçekleştirme |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Madde üzerinde gerçekleştirilen işleminin okuma sonucu yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Kurtarma noktası yedeklenen öğesinin okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/restore/action | Yedeklenen bir öğenin bir kurtarma noktasını kullanarak bir geri yükleme işlemi gerçekleştirme |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/write | Bir yedekleme öğesi oluşturma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Yedekleme öğesi tutan kapsayıcılar okuma |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | İşi iptal |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm iş nesneleri döndürür |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Bir Excel'e yedekleme işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Yedekleme yönetimiyle ilgili meta verilerini okuma |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme Yönetimi işlemlerinin sonuçlarını yönetme |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Yedekleme ilkeleri üzerinde gerçekleştirilen işlemler okuma sonuçları |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Yedekleme ilkeleri okuma |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilir öğelerini oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Tüm Korunabilir Öğelerin listesini döndürür. |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Yedekleme öğelerini tutan kapsayıcı okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Kasa için ilgili bilgileri Genişletilmiş okuma |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Kasa için ilgili genişletilmiş bilgileri yazma |
| Microsoft.RecoveryServices/Vaults/read | Kurtarma Hizmetleri kasaları okuma |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Kasaya kayıtlı öğeler üzerinde gerçekleştirilen işlemin okuma sonuçları |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kasa, kayıtlı öğeleri okuma |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Kasa için kayıtlı öğeler yazma |
| Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri kasası kullanımını okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
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
Kurtarma Hizmetleri kasasına yedekleme yönetimini izleyebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Yedekleme Yönetimi işlemi sonuçlarını okuyun |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults/read | İşlem sonuçları koruma kapsayıcılarında okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationResults/read | Madde üzerinde gerçekleştirilen işleminin okuma sonucu yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/read | Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Yedekleme öğesi tutan kapsayıcılar okuma |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Yedekleme işleri sonuçlarını okuyun |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Yedekleme işleri okuma |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Bir Excel'e yedekleme işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Yedekleme yönetimiyle ilgili meta verilerini okuma |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Yedekleme Yönetimi işlem sonuçları okuma |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Yedekleme ilkeleri üzerinde gerçekleştirilen işlemler okuma sonuçları |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Yedekleme ilkeleri okuma |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Yedekleme öğelerini tutan kapsayıcı okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Kasa için ilgili bilgileri Genişletilmiş okuma |
| Microsoft.RecoveryServices/Vaults/read | Kurtarma Hizmetleri kasaları okuma |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Yeni kapsayıcıları oluşturulan getirme için bulma işleminin sonucu okuma |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Kasaya kayıtlı öğeler üzerinde gerçekleştirilen işlemin okuma sonuçları |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kasa, kayıtlı öğeleri okuma |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
| Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri kasası kullanımını okuma |

## <a name="billing-reader"></a>Faturalandırma Okuyucusu
Tüm faturalama bilgileri görüntüleyebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Billing/*/read | Faturalama bilgileri okuyun |
| Microsoft.Consumption/*/read |  |
| Microsoft.Commerce/*/read |  |
| Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="biztalk-contributor"></a>BizTalk Katılımcısı
BizTalk Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.BizTalkServices/BizTalk/* | Oluşturma ve BizTalk services yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
Klasik sanal ağlar ve ayrılmış IP yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.ClassicNetwork/* | Oluşturun ve klasik ağları yönetin |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-storage-account-contributor"></a>Klasik Depolama Hesabı Katılımcısı
Klasik depolama hesaplarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.ClassicStorage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-storage-account-key-operator-service-role"></a>Klasik Depolama Hesabı Anahtarı İşleci Hizmet Rolü
Klasik Depolama Hesabı Anahtarı İşleçlerine, Klasik Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.ClassicStorage/storageAccounts/listkeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
| Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Depolama hesabı için var olan erişim anahtarlarını yeniden oluşturur. |

## <a name="classic-virtual-machine-contributor"></a>Klasik Sanal Makine Katılımcısı
Klasik sanal makineleri ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.ClassicCompute/domainNames/* | Oluşturma ve klasik hesaplama etki alanı adlarını yönetme |
| Microsoft.ClassicCompute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Ağ güvenlik grupları katılma |
| Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış IP bağlantı |
| Microsoft.ClassicNetwork/reservedIps/read | Okuma ayrılmış IP adresleri |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağlar katılma |
| Microsoft.ClassicNetwork/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskleri okuma |
| Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüleri okuma |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesabı anahtarlarını Listele |
| Microsoft.ClassicStorage/storageAccounts/read | Klasik depolama hesaplarını okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Katılımcısı
ClearDB MySQL veritabanları yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| successbricks.cleardb/databases/* | Oluşturma ve ClearDB MySQL veritabanları yönetme |

## <a name="cosmos-db-account-reader-role"></a>Cosmos DB Hesabı Okuyucusu Rolü
Azure Cosmos DB hesap verileri okuyabilir. Bkz: [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetmek için.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları, her bir kullanıcıya verilen izinler okuyabilir |
| Microsoft.DocumentDB/*/read | Herhangi bir koleksiyonu okuma |
| Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Salt okunur anahtarları bölmesinde okuma |
| Microsoft.Insights/MetricDefinitions/read | Ölçüm tanımlarını oku |
| Microsoft.Insights/Metrics/read | Hesap ölçümleri |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="data-factory-contributor"></a>Data Factory Katılımcısı
Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.DataFactory/dataFactories/* | Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin. |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
Her şeyi görüntüleyebilir ve bağlanmak, Başlat, yeniden başlatma ve kapatma sanal makineler

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Compute/availabilitySets/read | Kullanılabilirlik kümeleri özelliklerini okuma |
| Microsoft.Compute/virtualMachines/*/read | Bir sanal makine (VM boyutları, çalışma zamanı durumu, VM uzantıları, vb.) özelliklerini okuma |
| Microsoft.Compute/virtualMachines/deallocate/action | Sanal makineler serbest bırakma |
| Microsoft.Compute/virtualMachines/read | Bir sanal makinenin özelliklerini okuma |
| Microsoft.Compute/virtualMachines/restart/action | Sanal makineleri yeniden başlatın |
| Microsoft.Compute/virtualMachines/start/action | Sanal makineleri Başlat |
| Microsoft.DevTestLab/*/read | Bir laboratuvar özelliklerini okuma |
| Microsoft.DevTestLab/labs/createEnvironment/action | Bir laboratuvar ortamı oluşturma |
| Microsoft.DevTestLab/labs/claimAnyVm/action | Laboratuvar rastgele claimable sanal makinede talep. |
| Microsoft.DevTestLab/labs/formulas/delete | Formülleri silin |
| Microsoft.DevTestLab/labs/formulas/read | Formülleri okuma |
| Microsoft.DevTestLab/labs/formulas/write | Ekleme veya formüller değiştirme |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Laboratuvar ilkeleri değerlendir |
| Microsoft.DevTestLab/labs/virtualMachines/claim/action | Var olan bir sanal makine sahipliğini alın |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzuna Katıl |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici katılma gelen NAT kuralı |
| Microsoft.Network/networkInterfaces/*/read | Bir ağ arabirimi (ağ arabirimi bir parçası olan Örneğin, tüm yük dengeleyicileri) özelliklerini okuma |
| Microsoft.Network/networkInterfaces/join/action | Bir sanal makine bir ağ arabirimiyle Birleştir |
| Microsoft.Network/networkInterfaces/read | Ağ arabirimleri okuma |
| Microsoft.Network/networkInterfaces/write | Ağ arabirimleri yazma |
| Microsoft.Network/publicIPAddresses/*/read | Bir ortak IP adresinin özelliklerini okuma |
| Microsoft.Network/publicIPAddresses/join/action | Genel IP adresine Katıl |
| Microsoft.Network/publicIPAddresses/read | Ağ ortak IP adresleri okuyun |
| Microsoft.Network/virtualNetworks/subnets/join/action | Sanal bir ağa |
| Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini okuma |
| Microsoft.Resources/deployments/read | Okuma dağıtımları |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/listKeys/action | Depolama hesabı anahtarlarını Listele |

| **NotActions** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makineyi güncelleştirmek için kullanılabilir boyutları listeler |

## <a name="dns-zone-contributor"></a>DNS Bölgesi Katkıda Bulunanı
DNS bölgeleri ve kayıtları yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/dnsZones/* | DNS bölgeleri ve kayıtları oluşturma ve yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="documentdb-account-contributor"></a>DocumentDB Hesabı Katılımcısı
Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB önceden DocumentDB bilinirdi.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.DocumentDb/databaseAccounts/* | Azure Cosmos DB hesapları oluşturma ve yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="intelligent-systems-account-contributor"></a>Akıllı Sistemler Hesap Katılımcısı
Akıllı sistemler hesaplarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.IntelligentSystems/accounts/* | Akıllı sistemler hesapları oluşturma ve yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
| Microsoft.Insights/Register/Action | Microsoft.ınsights sağlayıcıyı kaydedin. |
| Microsoft.Insights/webtests/* | Okuma/yazma/silme Application Insights testleri web. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Okuma/yazma/silme günlük analizi çözüm paketleri. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* | Okuma/yazma/silme günlük analizi kayıtlı aramalar. |
| Microsoft.OperationalInsights/workspaces/search/action | Günlük analizi çalışma alanları arayın. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action | Günlük analizi çalışma alanı için anahtarları listeler. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Okuma/yazma/silme günlük analizi depolama Insight yapılandırmaları. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>İzleme Okuyucusu
Tüm izleme verilerini (ölçümleri, günlükleri, vb.) okuyabilir. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Eylemler** |  |
| --- | --- |
| * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.OperationalInsights/workspaces/search/action | Günlük analizi veri arama |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="network-contributor"></a>Ağ Katılımcısı
Tüm ağ kaynakları yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/* | Oluşturun ve ağları yönetin |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
Redis önbellekleri yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Cache/redis/* | Oluşturma ve Redis önbellekleri yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="scheduler-job-collections-contributor"></a>Zamanlayıcı İş Koleksiyonları Katılımcısı
Zamanlayıcı İş koleksiyonları yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Scheduler/jobcollections/* | Oluşturma ve iş koleksiyonları yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="search-service-contributor"></a>Search Hizmeti Katılımcısı
Arama Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |
| Microsoft.Security/locations/alerts/dismiss/action | Bir güvenlik uyarısını kapatmanın |
| Microsoft.Security/locations/tasks/dismiss/action | Güvenlik açısından kapatın |
| Microsoft.Security/policies/write | Güvenlik İlkesi güncelleştirmeleri |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="security-manager"></a>Güvenlik Yöneticisi
Güvenlik bileşenleri, güvenlik ilkeleri ve sanal makineleri yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.ClassicCompute/*/read | Klasik sanal makineleri yapılandırma bilgilerini okumak |
| Microsoft.ClassicCompute/virtualMachines/*/write | Klasik sanal makineler için yapılandırma yazma |
| Microsoft.ClassicNetwork/*/read | Klasik ağ hakkında yapılandırma bilgilerini okuyun |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |

## <a name="site-recovery-contributor"></a>Site Recovery Katkıda Bulunanı
Kurtarma Hizmetleri kasası oluşturma ve diğer kullanıcılara erişim hakları atama hariç tüm Site Recovery yönetim eylemleri, yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir |
| Microsoft.RecoveryServices/Vaults/certificates/write | Kasa kimlik bilgileri sertifikası güncelleştirir |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve kasa ilgili genişletilmiş bilgilerini yönetme |
| Microsoft.RecoveryServices/Vaults/read | Okuma kurtarma Hizmetleri kasaları |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Oluşturma veya çoğaltma uyarı ayarlarını güncelleştirme |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Çoğaltma olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Oluşturma ve çoğaltma yapıları yönetme |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Çoğaltma ilkeleri oluşturun ve yönetin |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Oluşturma ve kurtarma planlarını yönetme |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Kurtarma Hizmetleri kasası, depolama yapılandırması oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Okuma kurtarma Hizmetleri kasası belirteç bilgileri |
| Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri kasası kullanım ayrıntılarını okuyun |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read | Okuma kurtarma Hizmetleri kasası bildirim yapılandırması |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="site-recovery-operator"></a>Site Recovery Operatörü
Yük devretme ve yeniden çalışma ancak diğer Site Recovery yönetim eylemleri gerçekleştirmek veya diğer kullanıcılara erişimi atayın

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Kasa için ilgili bilgileri Genişletilmiş okuma |
| Microsoft.RecoveryServices/Vaults/read | Okuma kurtarma Hizmetleri kasaları |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | İşlem durumunu ve gönderilen bir işlemin sonucu okuyun |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Bir kaynak için kayıtlı okuma kapsayıcıları |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Çoğaltma uyarı ayarlarını okuma |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Çoğaltma olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Yapılar, tutarlılık denetimi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Okuma çoğaltma yapıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Çoğaltma ağ geçidi yeniden ilişkilendirin |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Çoğaltma yapı sertifikasını Yenile |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Çoğaltma doku ağları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Okuma çoğaltma doku ağ eşlemesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Koruma kapsayıcıları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Tüm korunabilir öğe listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Kurtarma noktası Uygula |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Yük devretmenin yürütülmesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlanan yük devretme |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğelerin listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Kullanılabilir kurtarma noktaları listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Onarım çoğaltma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Yeniden korumak için korumalı bir öğe Başlat |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/action | Korumalı bir öğe yük devretme Testi Başlat |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Yük devretme sınaması temizliğini |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Yük devretme |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility hizmeti güncelleştirmesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Koruma kapsayıcısı eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Okuma kurtarma Hizmetleri sağlayıcıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider/action | Kurtarma Hizmetleri sağlayıcısını yenileyin |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Çoğaltma yapıları için depolama sınıfların okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Depolama sınıflandırma eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | VCenter bilgileri okuma kayıtlı |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Çoğaltma İlkesi okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Kurtarma planı yük devretme için yük devretme Yürüt |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Bir kurtarma planı yük devretme başlatın |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma planları okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Yeniden koruma, Kurtarma planının Başlat |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Bir kurtarma planı yük devretme Testi Başlat |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Bir kurtarma planı yük devretme testi temizlenmesi Başlat |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Bir kurtarma planı planlanmamış yük devretme başlatın |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read | Okuma kurtarma Hizmetleri kasası bildirim yapılandırması |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Kurtarma Hizmetleri kasası depolama yapılandırmasını okuma |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Okuma kurtarma Hizmetleri kasası belirteç bilgileri |
| Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri kasası kullanım ayrıntılarını okuyun |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="site-recovery-reader"></a>Site Recovery Okuyucusu
Kurtarma Hizmetleri kasası Site kurtarma durumunu izleyebilir ve Destek biletlerini Yükselt

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Kasa için ilgili bilgileri Genişletilmiş okuma |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read | Okuma kurtarma Hizmetleri kasası bildirim yapılandırması |
| Microsoft.RecoveryServices/Vaults/read | Okuma kurtarma Hizmetleri kasaları |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | İşlem durumunu ve gönderilen bir işlemin sonucu okuyun |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Bir kaynak için kayıtlı okuma kapsayıcıları |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Çoğaltma uyarı ayarlarını okuma |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Çoğaltma olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Okuma çoğaltma yapıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Çoğaltma doku ağları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Okuma çoğaltma doku ağ eşlemesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Koruma kapsayıcıları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Tüm korunabilir öğe listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğelerin listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Kullanılabilir kurtarma noktaları listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Koruma kapsayıcısı eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Okuma kurtarma Hizmetleri sağlayıcıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Çoğaltma yapıları için depolama sınıfların okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Depolama sınıflandırma eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | VCenter bilgileri okuma kayıtlı |
| Microsoft.RecoveryServices/vaults/replicationJobs/read | Çoğaltma işlerin durumunu okuma |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Çoğaltma İlkesi okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma planları okuma |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Kurtarma Hizmetleri kasası depolama yapılandırmasını okuma |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Okuma kurtarma Hizmetleri kasası belirteç bilgileri |
| Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri kasası kullanım ayrıntılarını okuyun |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="sql-db-contributor"></a>SQL DB Katılımcısı
SQL veritabanları ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/databases/* | Oluşturma ve SQL veritabanlarını yönetme |
| Microsoft.Sql/servers/read | SQL Server'lar okuma |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Denetim ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditingSettings/* | Denetim ayarlarını düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditRecords/read | Denetim kayıtlarının okunamıyor |
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
SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma Microsoft yetkilendirme |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Sql/servers/auditingPolicies/* | SQL server denetim ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/auditingSettings/* | Oluşturma ve SQL server denetim ayarı yönetme |
| Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL server veritabanı denetim ayarlarını oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/auditRecords/read | Okuma denetim kaydı |
| Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantısı ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Oluşturma ve SQL server veritabanı veri maskeleme ilkelerini yönetme |
| Microsoft.Sql/servers/databases/read | Okuma SQL veritabanları |
| Microsoft.Sql/servers/databases/schemas/read | Okuma SQL server veritabanı şemaları |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Okuma SQL server veritabanı tablo sütunları |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/read | Okuma SQL server veritabanı tabloları |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarısı ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/securityMetrics/* | Oluşturma ve SQL server veritabanı güvenlik ölçümleri yönetme |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/firewallRules/* |  |
| Microsoft.Sql/servers/read | SQL Server'lar okuma |
| Microsoft.Sql/servers/securityAlertPolicies/* | SQL server güvenlik uyarısı ilkeleri oluşturun ve yönetin |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="sql-server-contributor"></a>SQL Server Katılımcısı
SQL sunucuları ve veritabanları ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
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
Depolama hesaplarını yönetme, ancak onlara erişimi yok.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Tüm Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/diagnosticSettings/* | Tanılama ayarlarını yönet |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="storage-account-key-operator-service-role"></a>Depolama Hesabı Anahtarı İşleci Hizmet Rolü
Depolama Hesabı Anahtarı İşleçlerine, Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
| Microsoft.Storage/storageAccounts/regeneratekey/action | Belirtilen depolama hesabının erişim anahtarlarını yeniden oluşturur. |

## <a name="support-request-contributor"></a>Destek İsteğine Katkıda Bulunan
Oluşturabilir ve abonelik kapsamında destek biletlerini yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Resources/subscriptions/resourceGroups/read | Okuma rolleri ve rol atamaları |
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
Azure kaynakları için kullanıcı erişimini yönetebilirsiniz

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
Sanal makineler ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Compute/availabilitySets/* | Oluşturun ve işlem kullanılabilirlik kümelerini yönetme |
| Microsoft.Compute/locations/* | Oluşturma ve işlem konumları yönetme |
| Microsoft.Compute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
| Microsoft.Compute/virtualMachineScaleSets/* | Oluşturma ve sanal makine ölçek kümeleri yönetme |
| Microsoft.DevTestLab/schedules/* |  |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Ağ uygulama ağ geçidi arka uç adres havuzlarında katılma |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Yük Dengeleyici arka uç adres havuzlarında katılma |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Yük Dengeleyici katılma gelen NAT havuzları |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Yük Dengeleyici katılma gelen NAT kuralları |
| Microsoft.Network/loadBalancers/probes/join/action | Bir yük dengeleyicinin araştırmalar kullanarak sağlar. Örneğin, VM ölçek bu izni healthProbe özelliği ile kümesi araştırma başvuruda bulunabilir. |
| Microsoft.Network/loadBalancers/read | Okuma yük Dengeleyiciler |
| Microsoft.Network/locations/* | Oluşturma ve ağ konumları yönetme |
| Microsoft.Network/networkInterfaces/* | Oluşturma ve ağ arabirimlerinin yönetme |
| Microsoft.Network/networkSecurityGroups/join/action | Ağ güvenlik grupları katılma |
| Microsoft.Network/networkSecurityGroups/read | Ağ güvenlik grupları okuma |
| Microsoft.Network/publicIPAddresses/join/action | Ağ ortak IP adresleri katılma |
| Microsoft.Network/publicIPAddresses/read | Ağ ortak IP adresleri okuyun |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.Network/virtualNetworks/subnets/join/action | Sanal ağ alt ağları katılma |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/listKeys/action | Depolama hesabı anahtarlarını Listele |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="virtual-machine-user-login"></a>Sanal makine kullanıcı oturum açma
Bu role sahip kullanıcılar oturum açmak için bir sanal makine için normal bir kullanıcı olarak sahipsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="web-plan-contributor"></a>Web Planı Katılımcısı
Web planlarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/serverFarms/* | Oluşturma ve sunucu grupları yönetme |

## <a name="website-contributor"></a>Web Sitesi Katılımcısı
Web siteleri ancak olmayan bağlı web planlarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/components/* | Oluşturma ve Insights bileşenlerini yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/certificates/* | Web sitesi sertifikaları oluşturmak ve yönetmek |
| Microsoft.Web/listSitesAssignedToHostName/read | Bir ana bilgisayar adı atanmış siteler okuma |
| Microsoft.Web/serverFarms/join/action | Sunucu grupları katılma |
| Microsoft.Web/serverFarms/read | Sunucu grupları okuma |
| Microsoft.Web/sites/* | Oluşturma ve (site oluşturma ilişkili uygulama hizmeti planı yazma izinleri de gerektirir) Web sitelerini yönetme |

## <a name="see-also"></a>Ayrıca bkz.
* [Rol tabanlı erişim denetimi](role-based-access-control-configure.md): Azure portalında RBAC ile çalışmaya başlama.
* [Azure rbac'de özel roller](role-based-access-control-custom-roles.md): erişim gereksinimlerinize uyacak şekilde özel roller oluşturma hakkında bilgi edinin.
* [Erişim değişiklik geçmişi raporu oluşturma](role-based-access-control-access-change-history-report.md): RBAC içinde rol atamalarını değiştirme izler.
* [Rol tabanlı erişim denetimi sorun giderme](role-based-access-control-troubleshooting.md): sık karşılaşılan sorunları düzeltmek için öneriler alın.
* [Azure Güvenlik Merkezi'nde izinleri](../security-center/security-center-permissions.md)
