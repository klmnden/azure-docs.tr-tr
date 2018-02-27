---
title: "Eylemler ve NotActions - Azure rol tabanlı erişim denetimi (RBAC) | Microsoft Docs"
description: "Bu konu için rol tabanlı erişim denetimi (RBAC) rollerdeki yerleşik açıklar. Rolleri sürekli olarak eklenir, belgeleri yenilik kontrol edin."
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 02/23/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: e49f555b2ae972cd3a0437fc44d2331aaeb5e955
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi için yerleşik roller
Azure rol tabanlı erişim denetimi (RBAC), kullanıcılar, gruplar ve hizmetlere atanmış aşağıdaki yerleşik rolleri ile birlikte gelir. Yerleşik rol tanımlarını değiştiremezsiniz. Ancak, oluşturabileceğiniz [Azure rbac'de özel roller](role-based-access-control-custom-roles.md) , kuruluşunuzun belirli gereksinimlerine uyacak şekilde.

## <a name="roles-in-azure"></a>Azure rollerinde
Aşağıdaki tabloda yerleşik roller kısa açıklamaları sağlar. Ayrıntılı bir listesi görmek için rol adını tıklatın **Eylemler** ve **notactions** rolü. **Eylemler** özelliği, Azure kaynakları izin verilen eylemleri belirtir. Eylem dizeleri joker karakterleri kullanabilirsiniz. **Notactions** özelliği, izin verilen eylemler hariç tutulan eylemleri belirtir.

Belirtilen kaynak türü üzerinde gerçekleştirebileceğiniz ne tür bir operations eylemi tanımlar. Örneğin:
- **Yazma** PUT, POST, düzeltme eki ve silme işlemleri yapmanıza olanak sağlar.
- **Okuma** GET işlemleri yapmanıza olanak sağlar.

Bu makalede yalnızca bugün mevcut farklı rolleri giderir. Ancak, bir kullanıcıya rol atadığınızda, kapsam tanımlayarak daha fazla izin verilen eylemleri sınırlayabilirsiniz. Bu, birisi bir Web sitesi katkıda bulunan, ancak yalnızca bir kaynak grubu için yapmak istiyorsanız yararlıdır.

> [!NOTE]
> Azure rol tanımlarını sürekli olarak artmaktadır. Bu makalede olarak olabildiğince güncel tutulur, ancak her zaman en son rol tanımlarını Azure PowerShell'de bulabilirsiniz. Kullanım [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) cmdlet'ini tüm geçerli rolleri listeler. Belirli bir rol kullanmaya başlayabilirsiniz `(get-azurermroledefinition "<role name>").actions` veya `(get-azurermroledefinition "<role name>").notactions` olarak uygulanabilir. Kullanım [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) listeleme işlemleri belirli bir Azure kaynak sağlayıcıları için.


| Rol adı | Açıklama |
| --- | --- |
| [API Management hizmeti katkıda bulunan](#api-management-service-contributor) |API Management hizmeti ve API'leri Yönet |
| [API Management hizmet işleci rolü](#api-management-service-operator-role) | API Management hizmeti, ancak API kendilerini yönetebilirsiniz. |
| [API Management hizmet okuyucu rolü](#api-management-service-reader-role) | API Management hizmeti ve API'ler için salt okunur erişim |
| [Uygulama Öngörüler bileşen katkıda bulunan](#application-insights-component-contributor) |Application Insights bileşenlerini yönetebilir |
| [Automation operatörü](#automation-operator) |Başlatma, durdurma, askıya alma ve işlerini sürdürmek için |
| [Yedekleme katkıda bulunan](#backup-contributor) | Kurtarma Hizmetleri kasasına yedekleme yönetebilirsiniz. |
| [Yedekleme işletmeni](#backup-operator) | Kurtarma Hizmetleri kasasına yedekleme kaldırma dışında yedekleme yönetebilirsiniz. |
| [Yedekleme okuyucusu](#backup-reader) | Tüm yedekleme Yönetimi Hizmetleri görüntüleyebilirsiniz  |
| [Faturalama okuyucusu](#billing-reader) | Tüm fatura bilgilerini görüntüleyebilirsiniz  |
| [BizTalk katkıda bulunan](#biztalk-contributor) |BizTalk Hizmetleri yönetebilir. |
| [ClearDB MySQL DB katkıda bulunan](#cleardb-mysql-db-contributor) |ClearDB MySQL veritabanları yönetebilirsiniz |
| [Katkıda bulunan](#contributor) |Erişim dışında her şeyi yönetebilir. |
| [Cosmos DB hesap okuyucu rolü](#cosmos-db-account-reader-role) |Azure Cosmos DB hesap verileri okuyabilir |
| [Veri Fabrikası katkıda bulunan](#data-factory-contributor) |Oluşturabilir ve veri fabrikaları ve bunların içindeki alt kaynakları yönetebilirsiniz. |
| [DevTest Labs kullanıcı](#devtest-labs-user) |Her şeyi görüntüleyebilir ve bağlanmak, Başlat, yeniden başlatma ve kapatma sanal makineler |
| [DNS bölgesi katkıda bulunan](#dns-zone-contributor) |DNS bölgeleri ve kayıtları yönetebilir |
| [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) |Azure Cosmos DB hesaplarını yönetme |
| [Akıllı sistemler hesap katkıda bulunan](#intelligent-systems-account-contributor) |Akıllı sistemler hesaplarını yönetme |
| Mantıksal Uygulama Katkıda Bulunanı | Bir mantıksal uygulama tüm yönlerini yönetmek, ancak yeni bir tane oluşturun değil. |
| Mantıksal Uygulama Operatörü |Başlangıç ve bir mantıksal uygulama içinde tanımlanan iş akışlarını durdurun kullanabilirsiniz. |
| [Okuyucu izleme](#monitoring-reader) |Tüm izleme verileri okuyabilir |
| [Katkıda bulunan izleme](#monitoring-contributor) |İzleme verileri okuyabilir ve izleme ayarlarını Düzenle |
| [Ağ Katılımcısı](#network-contributor) |Tüm ağ kaynakları yönetebilir |
| [Sahibi](#owner) |Erişim dahil her şeyi yönetebilir |
| [Reader](#reader) |Her şeyi görüntüleyebilir ancak değişiklik yapamaz |
| [Redis önbelleği katkıda bulunan](#redis-cache-contributor) |Redis önbellekleri yönetebilirsiniz. |
| [Zamanlayıcı İş koleksiyonları katkıda bulunan](#scheduler-job-collections-contributor) |Zamanlayıcı İş koleksiyonları yönetebilir |
| [Arama hizmeti katkıda bulunan](#search-service-contributor) |Arama Hizmetleri yönetebilir. |
| [Güvenlik Yöneticisi](#security-administrator) | Yalnızca Güvenlik Merkezi'nde: görüntülemek güvenlik ilkeleri, güvenlik durumları görüntülemek, güvenlik ilkeleri, Uyarıları görüntüle ve öneriler düzenleme, uyarısı ve öneri yok sayın |
| [Güvenlik Yöneticisi](#security-manager) | Güvenlik bileşenleri, güvenlik ilkeleri ve sanal makineleri yönetebilirsiniz |
| [Güvenlik okuyucusu](#security-reader) | Yalnızca Güvenlik Merkezi'nde: önerileri ve uyarılar, güvenlik ilkeleri, güvenlik durumları görüntüleyebilir ancak değişiklik yapamaz görünümü görüntüleyebilirsiniz |
| [Site kurtarma katkıda bulunan](#site-recovery-contributor) | Site Recovery kurtarma Hizmetleri kasasına yönetebilirsiniz |
| [Site Recovery Operator](#site-recovery-operator) | Yük devretme ve yeniden çalışma işlemlerini Site Recovery kurtarma Hizmetleri kasasına yönetebilirsiniz |
| [Site Recovery Reader](#site-recovery-reader) | Tüm Site Recovery yönetim işlemlerinin görüntüleyebilirsiniz  |
| [SQL DB Katılımcısı](#sql-db-contributor) |SQL veritabanları, ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz. |
| [SQL Güvenlik Yöneticisi](#sql-security-manager) |SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetebilirsiniz |
| [SQL Server katkıda bulunan](#sql-server-contributor) |SQL sunucuları ve veritabanları, ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz. |
| [Klasik depolama hesabı katkıda bulunan](#classic-storage-account-contributor) |Klasik depolama hesaplarını yönetme |
| [Depolama hesabı katkıda bulunan](#storage-account-contributor) |Depolama hesaplarını yönetme |
| [Destek isteği katkıda bulunan](#support-request-contributor) | Oluşturabilir ve Destek isteklerini yönet |
| [Kullanıcı erişimi Yöneticisi](#user-access-administrator) |Azure kaynakları için kullanıcı erişimini yönetebilirsiniz |
| [Klasik sanal makine Katılımcısı](#classic-virtual-machine-contributor) |Klasik sanal makineler, ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz. |
| [Sanal makine Katılımcısı](#virtual-machine-contributor) |Sanal makineler, ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz. |
| [Klasik Ağ Katılımcısı](#classic-network-contributor) |Klasik sanal ağlar ve ayrılmış IP yönetebilir |
| [Web planı katkıda bulunan](#web-plan-contributor) |Web planlarını yönetme |
| [Web sitesi katkıda bulunan](#website-contributor) |Web siteleri, ancak olmayan bağlı web planlarını yönetme |

## <a name="role-permissions"></a>Rol izinleri
Aşağıdaki tablolarda her rol için verilen özel izinler açıklanmaktadır. Bu içerebilir **Eylemler**, izinleri verin ve **NotActions**, hangi kısıtlamak bunları.

### <a name="api-management-service-contributor"></a>API Yönetimi Hizmeti Katılımcısı
API Management Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/* |Oluşturma ve API Management hizmeti yönetme |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Okuma rolleri ve rol atamaları |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="api-management-service-operator-role"></a>API Management Hizmet Operatörü Rolü
API Management Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Okuma API Management hizmeti örnekleri |
| Microsoft.ApiManagement/Service/backup/action | Bir kullanıcı tarafından sağlanan depolama hesabı belirtilen kapsayıcıda API Management hizmeti dön |
| Microsoft.ApiManagement/Service/delete | API Management hizmeti örneği Sil |
| Microsoft.ApiManagement/Service/managedeployments/action | Değişiklik SKU/birimleri; API Management hizmet bölgesel dağıtımları Ekle Kaldır |
| Microsoft.ApiManagement/Service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
| Microsoft.ApiManagement/Service/restore/action | API Management hizmeti belirtilen bir kullanıcı tarafından sağlanan depolama hesabı kapsayıcısında geri yükleme |
| Microsoft.ApiManagement/Service/updatehostname/action | Ayarlama, güncelleştirmek veya bir API Management hizmeti için özel etki alanı adlarını kaldırın |
| Microsoft.ApiManagement/Service/write | API Management hizmeti yeni bir örneğini oluşturma |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Okuma rolleri ve rol atamaları |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="api-management-service-reader-role"></a>API Management Hizmet Okuyucusu Rolü
API Management Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Okuma API Management hizmeti örnekleri |
| Microsoft.ApiManagement/Service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Okuma rolleri ve rol atamaları |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="application-insights-component-contributor"></a>Application Insights Bileşeni Katılımcısı
Application Insights bileşenlerini yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Insights/components/* |Oluşturma ve Insights bileşenlerini yönetme |
| Microsoft.Insights/webtests/* |Oluşturma ve web testleri yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="automation-operator"></a>Otomasyon Operatörü
Başlatma, durdurma, askıya alma ve işlerini sürdürmek için

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Automation/automationAccounts/jobs/read |Otomasyon hesabı işleri okuma |
| Microsoft.Automation/automationAccounts/jobs/resume/action |Bir Otomasyon hesabı işini sürdürme |
| Microsoft.Automation/automationAccounts/jobs/stop/action |Bir Otomasyon hesabı işini durdurma |
| Microsoft.Automation/automationAccounts/jobs/streams/read |Otomasyon hesabı iş akışları okuma |
| Microsoft.Automation/automationAccounts/jobs/suspend/action |Bir Otomasyon hesabı işini askıya alma |
| Microsoft.Automation/automationAccounts/jobs/write |Otomasyon hesabı işleri yazma |
| Microsoft.Automation/automationAccounts/jobSchedules/read |Bir Otomasyon hesabı iş zamanlaması okuma |
| Microsoft.Automation/automationAccounts/jobSchedules/write |Bir Otomasyon hesabı iş zamanlaması okuma |
| Microsoft.Automation/automationAccounts/read |Automation hesapları okuma |
| Microsoft.Automation/automationAccounts/runbooks/read |Otomasyon runbook'ları okuma |
| Microsoft.Automation/automationAccounts/schedules/read |Otomasyon hesabı zamanlamaları okuma |
| Microsoft.Automation/automationAccounts/schedules/write |Otomasyon hesabı zamanlamaları yazma |
| Microsoft.Insights/components/* |Oluşturma ve Insights bileşenlerini yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="backup-contributor"></a>Yedekleme Katılımcısı
Kurtarma Hizmetleri kasası oluşturmaya ve erişim başkalarına verip dışındaki tüm yedekleme yönetimi eylemleri yönetebilirsiniz

| **Eylemler** | |
| --- | --- |
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
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="backup-operator"></a>Yedekleme İşleci
Yedekleme ve vermiş erişim başkalarına kaldırma kasalarını oluşturma dışındaki tüm yedekleme yönetimi eylemleri yönetebilirsiniz

| **Eylemler** | |
| --- | --- |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Yedekleme Yönetimi işlemi sonuçlarını okuyun |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | İşlem sonuçları koruma kapsayıcılarında okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Yedeklenen öğesi talep üzerine yedekleme işlemi gerçekleştirme |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Madde üzerinde gerçekleştirilen işleminin okuma sonucu yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read | Madde üzerinde gerçekleştirilen işlem okuma durumu yedeği |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Kurtarma noktası yedeklenen öğesinin okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Yedeklenen bir öğenin bir kurtarma noktasını kullanarak bir geri yükleme işlemi gerçekleştirme |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Bir yedekleme öğesi oluşturma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Yedekleme öğesi tutan kapsayıcılar okuma |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Bir Excel'e yedekleme işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Yedekleme yönetimiyle ilgili meta verilerini okuma |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme Yönetimi işlemlerinin sonuçlarını yönetme |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Yedekleme ilkeleri üzerinde gerçekleştirilen işlemler okuma sonuçları |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Yedekleme ilkeleri okuma |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilir öğelerini oluşturma ve yönetme |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Yedekleme öğelerini tutan kapsayıcı okuma yedeklenen |
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
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

### <a name="backup-reader"></a>Yedekleme Okuyucusu
Kurtarma Hizmetleri kasasına yedekleme yönetimini izleyebilirsiniz

| **Eylemler** | |
| --- | --- |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read  | Yedekleme Yönetimi işlemi sonuçlarını okuyun |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read  | İşlem sonuçları koruma kapsayıcılarında okuma |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read  | Madde üzerinde gerçekleştirilen işleminin okuma sonucu yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read  | Madde üzerinde gerçekleştirilen işlem okuma durumu yedeği |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read  | Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read  | Yedekleme öğesi tutan kapsayıcılar okuma |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read  | Yedekleme işleri sonuçlarını okuyun |
| Microsoft.RecoveryServices/Vaults/backupJobs/read  | Yedekleme işleri okuma |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Bir Excel'e yedekleme işleri |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read  | Yedekleme yönetimiyle ilgili meta verilerini okuma |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read  | Yedekleme Yönetimi işlem sonuçları okuma |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read  | Yedekleme ilkeleri üzerinde gerçekleştirilen işlemler okuma sonuçları |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read  | Yedekleme ilkeleri okuma |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read  |  Öğeleri okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read  | Yedekleme öğelerini tutan kapsayıcı okuma yedeklenen |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Kasa için ilgili bilgileri Genişletilmiş okuma |
| Microsoft.RecoveryServices/Vaults/read  | Kurtarma Hizmetleri kasaları okuma |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Yeni kapsayıcıları oluşturulan getirme için bulma işleminin sonucu okuma |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | Kasaya kayıtlı öğeler üzerinde gerçekleştirilen işlemin okuma sonuçları |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Kasa, kayıtlı öğeleri okuma |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Kurtarma Hizmetleri kasası kullanımını okuma |

### <a name="billing-reader"></a>Faturalandırma Okuyucusu
Tüm faturalama bilgileri görüntüleyebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Billing/*/read |Faturalama bilgileri okuyun |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="biztalk-contributor"></a>BizTalk Katılımcısı
BizTalk Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.BizTalkServices/BizTalk/* |Oluşturma ve BizTalk services yönetme |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Katılımcısı
ClearDB MySQL veritabanları yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |
| successbricks.cleardb/databases/* |Oluşturma ve ClearDB MySQL veritabanları yönetme |

### <a name="contributor"></a>Katılımcı
Erişim dışında her şeyi yönetebilir

| **Eylemler** |  |
| --- | --- |
| * |Oluşturma ve tüm türlerinin kaynakları yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete |Rolleri ve rol atamalarını silinemiyor |
| Microsoft.Authorization/*/Write |Rolleri ve rol atamalarını oluşturulamıyor |

### <a name="cosmos-db-account-reader-role"></a>Cosmos DB Hesabı Okuyucusu Rolü
Azure Cosmos DB hesap verileri okuyabilir. Bkz: [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetmek için.

| **Eylemler** |  |
| --- | --- |
|Microsoft.Authorization/*/read|Okuma rolleri ve rol atamaları, her bir kullanıcıya verilen izinler okuyabilir|
|Microsoft.DocumentDB/*/read|Herhangi bir koleksiyonu okuma|
|Microsoft.DocumentDB/databaseAccounts/readonlykeys/action|Salt okunur anahtarları bölmesinde okuma|
|Microsoft.Insights/Metrics/read|Hesap ölçümleri|
|Microsoft.Insights/MetricDefinitions/read|Ölçüm tanımlarını oku|
|Microsoft.Resources/subscriptions/resourceGroups/read|Kaynak gruplarını oku|
|Microsoft.Support/*|Oluşturma ve Destek biletlerini yönetme|

### <a name="data-factory-contributor"></a>Data Factory Katılımcısı
Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.DataFactory/dataFactories/* |Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin. |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="devtest-labs-user"></a>DevTest Labs Kullanıcısı
Her şeyi görüntüleyebilir ve bağlanmak, Başlat, yeniden başlatma ve kapatma sanal makineler

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Compute/availabilitySets/read |Kullanılabilirlik kümeleri özelliklerini okuma |
| Microsoft.Compute/virtualMachines/*/read |Bir sanal makine (VM boyutları, çalışma zamanı durumu, VM uzantıları, vb.) özelliklerini okuma |
| Microsoft.Compute/virtualMachines/deallocate/action |Sanal makineler serbest bırakma |
| Microsoft.Compute/virtualMachines/read |Bir sanal makinenin özelliklerini okuma |
| Microsoft.Compute/virtualMachines/restart/action |Sanal makineleri yeniden başlatın |
| Microsoft.Compute/virtualMachines/start/action |Sanal makineleri Başlat |
| Microsoft.DevTestLab/*/read |Bir laboratuvar özelliklerini okuma |
| Microsoft.DevTestLab/labs/createEnvironment/action |Bir laboratuvar ortamı oluşturma |
| Microsoft.DevTestLab/labs/formulas/delete |Formülleri silin |
| Microsoft.DevTestLab/labs/formulas/read |Formülleri okuma |
| Microsoft.DevTestLab/labs/formulas/write |Ekleme veya formüller değiştirme |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action |Laboratuvar ilkeleri değerlendir |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Bir yük dengeleyici arka uç adres havuzuna Katıl |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Bir yük dengeleyici katılma gelen NAT kuralı |
| Microsoft.Network/networkInterfaces/*/read |Bir ağ arabirimi (ağ arabirimi bir parçası olan Örneğin, tüm yük dengeleyicileri) özelliklerini okuma |
| Microsoft.Network/networkInterfaces/join/action |Bir sanal makine bir ağ arabirimiyle Birleştir |
| Microsoft.Network/networkInterfaces/read |Ağ arabirimleri okuma |
| Microsoft.Network/networkInterfaces/write |Ağ arabirimleri yazma |
| Microsoft.Network/publicIPAddresses/*/read |Bir ortak IP adresinin özelliklerini okuma |
| Microsoft.Network/publicIPAddresses/join/action |Genel IP adresine Katıl |
| Microsoft.Network/publicIPAddresses/read |Ağ ortak IP adresleri okuyun |
| Microsoft.Network/virtualNetworks/subnets/join/action |Sanal bir ağa |
| Microsoft.Resources/deployments/operations/read |Dağıtım işlemlerini okuma |
| Microsoft.Resources/deployments/read |Okuma dağıtımları |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/listKeys/action |Depolama hesabı anahtarlarını Listele |

### <a name="dns-zone-contributor"></a>DNS Bölgesi Katkıda Bulunanı
DNS bölgeleri ve kayıtları yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/ \* /okuma |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/\* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/dnsZones/\* |DNS bölgeleri ve kayıtları oluşturma ve yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/\* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/\* |Oluşturma ve Destek biletlerini yönetme |

### <a name="documentdb-account-contributor"></a>DocumentDB Hesabı Katılımcısı
Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB önceden DocumentDB bilinirdi.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.DocumentDb/databaseAccounts/* |Azure Cosmos DB hesapları oluşturma ve yönetme |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="intelligent-systems-account-contributor"></a>Akıllı Sistemler Hesap Katılımcısı
Akıllı sistemler hesaplarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.IntelligentSystems/accounts/* |Akıllı sistemler hesapları oluşturma ve yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="monitoring-reader"></a>İzleme Okuyucusu
Tüm izleme verilerini (ölçümleri, günlükleri, vb.) okuyabilir. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Eylemler** |  |
| --- | --- |
| * / Okuma |Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.OperationalInsights/workspaces/search/action |Günlük analizi veri arama |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="monitoring-contributor"></a>İzleme Katkıda Bulunanı
Tüm izleme verileri okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Eylemler** |  |
| --- | --- |
| * / Okuma |Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.Insights/AlertRules/* |Okuma/yazma/silme uyarı kuralları. |
| Microsoft.Insights/components/* |Okuma/yazma/silme Application Insights bileşenleri. |
| Microsoft.Insights/DiagnosticSettings/* |Okuma/yazma/silme tanılama ayarları. |
| Microsoft.Insights/eventtypes/* |Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, etkinlik günlüğü programlı ve portal erişimi için geçerlidir. |
| Microsoft.Insights/LogDefinitions/* |Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğü günlük kategorilerini liste. |
| Microsoft.Insights/MetricDefinitions/* |Ölçüm tanımlarını (bir kaynak için kullanılabilir ölçüm türlerinin listesi) okuyun. |
| Microsoft.Insights/Metrics/* |Bir kaynak için ölçümleri okuyun. |
| Microsoft.Insights/Register/Action |Microsoft.ınsights sağlayıcıyı kaydedin. |
| Microsoft.Insights/webtests/* |Okuma/yazma/silme Application Insights testleri web. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* |Okuma/yazma/silme günlük analizi çözüm paketleri. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* |Okuma/yazma/silme günlük analizi kayıtlı aramalar. |
| Microsoft.OperationalInsights/workspaces/search/action |Günlük analizi çalışma alanları arayın. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action |Günlük analizi çalışma alanı için anahtarları listeler. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* |Okuma/yazma/silme günlük analizi depolama Insight yapılandırmaları. |

### <a name="network-contributor"></a>Ağ Katılımcısı
Tüm ağ kaynakları yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/* |Oluşturun ve ağları yönetin |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="owner"></a>Sahip
Erişim dahil her şeyi yönetebilir

| **Eylemler** |  |
| --- | --- |
| * |Oluşturma ve tüm türlerinin kaynakları yönetme |

### <a name="reader"></a>Okuyucu
Her şeyi görüntüleyebilir ancak değişiklik yapamaz

| **Eylemler** |  |
| --- | --- |
| * / Okuma |Gizli dışındaki tüm türlerinin kaynakları okuyun. |

### <a name="redis-cache-contributor"></a>Redis Cache Katılımcısı
Redis önbellekleri yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Cache/redis/* |Oluşturma ve Redis önbellekleri yönetme |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="scheduler-job-collections-contributor"></a>Zamanlayıcı İş Koleksiyonları Katılımcısı
Zamanlayıcı İş koleksiyonları yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Scheduler/jobcollections/* |Oluşturma ve iş koleksiyonları yönetme |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="search-service-contributor"></a>Search Hizmeti Katılımcısı
Arama Hizmetleri yönetebilir.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Search/searchServices/* |Oluşturma ve arama hizmetleri yönetme |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="security-administrator"></a>Güvenlik Yöneticisi
Yalnızca Güvenlik Merkezi'nde: görüntülemek güvenlik ilkeleri, güvenlik durumları görüntülemek, güvenlik ilkeleri, Uyarıları görüntüle ve öneriler düzenleme, uyarısı ve öneri yok sayın

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Authorization/policyAssignments/* | Oluşturma ve ilke atamalarını yönetme |
| Microsoft.Authorization/policySetDefinitions/* | Oluşturun ve ilke kümelerini yönetme |
| Microsoft.Authorization/policyDefinitions/* | Oluşturma ve ilke tanımları yönetme |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.operationalInsights/workspaces/*/read | Günlük analizi verilerini görüntüleme |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="security-manager"></a>Güvenlik Yöneticisi
Güvenlik bileşenleri, güvenlik ilkeleri ve sanal makineleri yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.ClassicCompute/*/read |Klasik sanal makineleri yapılandırma bilgilerini okumak |
| Microsoft.ClassicCompute/virtualMachines/*/write |Klasik sanal makineler için yapılandırma yazma |
| Microsoft.ClassicNetwork/*/read |Klasik ağ hakkında yapılandırma bilgilerini okuyun |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Security/* |Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="security-reader"></a>Güvenlik okuyucusu
Yalnızca Güvenlik Merkezi'nde: önerileri ve uyarılar, güvenlik ilkeleri, güvenlik durumları görüntüleyebilir ancak değişiklik yapamaz görünümü görüntüleyebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.operationalInsights/workspaces/*/read | Günlük analizi verilerini görüntüleme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |

### <a name="site-recovery-contributor"></a>Site Recovery Katkıda Bulunanı
Kurtarma Hizmetleri kasası oluşturma ve diğer kullanıcılara erişim hakları atama hariç tüm Site Recovery yönetim eylemleri, yönetebilirsiniz

| **Eylemler** | |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.RecoveryServices/Vaults/certificates/write | Kasa kimlik bilgileri sertifikası güncelleştirir |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve kasa ilgili genişletilmiş bilgilerini yönetme |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read  | Okuma kurtarma Hizmetleri kasası bildirim yapılandırması |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="site-recovery-operator"></a>Site Recovery Operatörü
Yük devretme ve yeniden çalışma ancak diğer Site Recovery yönetim eylemleri gerçekleştirmek veya diğer kullanıcılara erişimi atayın

| **Eylemler** | |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.Network/virtualNetworks/read | Sanal ağlar okuma |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Kasa için ilgili bilgileri Genişletilmiş okuma |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read  | Okuma kurtarma Hizmetleri kasası bildirim yapılandırması |
| Microsoft.RecoveryServices/Vaults/read | Okuma kurtarma Hizmetleri kasaları |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | İşlem durumunu ve gönderilen bir işlemin sonucu okuyun |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Bir kaynak için kayıtlı okuma kapsayıcıları |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Çoğaltma uyarı ayarlarını okuma |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Çoğaltma olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Yapılar, tutarlılık denetimi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Okuma çoğaltma yapıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ reassociateGateway/action | Çoğaltma ağ geçidi yeniden ilişkilendirin |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Çoğaltma yapı sertifikasını Yenile |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Çoğaltma doku ağları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Okuma çoğaltma doku ağ eşlemesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Koruma kapsayıcıları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Tüm korunabilir öğe listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ applyRecoveryPoint/action | Belirli bir kurtarma noktasını Uygula |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ failoverCommit/action | Yük devretme başarısız için Yürüt öğe üzerinde |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ plannedFailover/action | Korumalı bir öğe için planlanan yük devretme başlatın |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğelerin listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Kullanılabilir kurtarma noktaları listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ repairReplication/action | Korumalı bir öğe için onarım çoğaltma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Yeniden korumak için korumalı bir öğe Başlat|
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/action | Korumalı bir öğe yük devretme Testi Başlat |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ testFailoverCleanup/action | Yük devretme testi temizlenmesi Başlat |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ unplannedFailover/action | Korumalı bir öğe planlanmamış yük devretme başlatın |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/ updateMobilityService/action | Mobility hizmeti güncelleştirmesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Koruma kapsayıcısı eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read | Okuma kurtarma Hizmetleri sağlayıcıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/refreshProvider/action | Kurtarma Hizmetleri sağlayıcısını yenileyin |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Çoğaltma yapıları için depolama sınıfların okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Depolama sınıflandırma eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | VCenter bilgileri okuma kayıtlı |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturmak ve yönetmek |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Çoğaltma İlkesi okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ failoverCommit/action | Kurtarma planı yük devretme için yük devretme Yürüt |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ plannedFailover/action | Bir kurtarma planı yük devretme başlatın |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma planları okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Yeniden koruma, Kurtarma planının Başlat |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Bir kurtarma planı yük devretme Testi Başlat |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ testFailoverCleanup/action | Bir kurtarma planı yük devretme testi temizlenmesi Başlat |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ unplannedFailover/action | Bir kurtarma planı planlanmamış yük devretme başlatın |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Kurtarma Hizmetleri kasası depolama yapılandırmasını okuma |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Okuma kurtarma Hizmetleri kasası belirteç bilgileri |
| Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri kasası kullanım ayrıntılarını okuyun |
| Microsoft.ResourceHealth/availabilityStatuses/read | Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/read | Depolama hesapları okuma |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

### <a name="site-recovery-reader"></a>Site Recovery Okuyucusu
Kurtarma Hizmetleri kasası Site kurtarma durumunu izleyebilir ve Destek biletlerini Yükselt

| **Eylemler** | |
| --- | --- |
| Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Kasa için ilgili bilgileri Genişletilmiş okuma |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read  | Kurtarma Hizmetleri kasası için uyarıları okuma |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration/read  | Okuma kurtarma Hizmetleri kasası bildirim yapılandırması |
| Microsoft.RecoveryServices/Vaults/read  | Okuma kurtarma Hizmetleri kasaları |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | İşlem durumunu ve gönderilen bir işlemin sonucu okuyun |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Bir kaynak için kayıtlı okuma kapsayıcıları |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Çoğaltma uyarı ayarlarını okuma |
| Microsoft.RecoveryServices/vaults/replicationEvents/read  | Çoğaltma olayları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read  | Okuma çoğaltma yapıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read  | Çoğaltma doku ağları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read  | Okuma çoğaltma doku ağ eşlemesi |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read  |  Koruma kapsayıcıları okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read  | Tüm korunabilir öğe listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read  | Tüm korumalı öğelerin listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read  | Kullanılabilir kurtarma noktaları listesini al |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read  | Koruma kapsayıcısı eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders/read  | Okuma kurtarma Hizmetleri sağlayıcıları |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read  | Çoğaltma yapıları için depolama sınıfların okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read  |  Depolama sınıflandırma eşlemeleri okuma |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read  |  VCenter bilgileri okuma kayıtlı |
| Microsoft.RecoveryServices/vaults/replicationJobs/read  |  Çoğaltma işlerin durumunu okuma |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read  |  Çoğaltma İlkesi okuma |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read  |  Kurtarma planları okuma |
| Microsoft.RecoveryServices/Vaults/storageConfig/read  |  Kurtarma Hizmetleri kasası depolama yapılandırmasını okuma |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read  |  Okuma kurtarma Hizmetleri kasası belirteç bilgileri |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Kurtarma Hizmetleri kasası kullanım ayrıntılarını okuyun |
| Microsoft.Support/*  |  Oluşturma ve Destek biletlerini yönetme |

### <a name="sql-db-contributor"></a>SQL DB Katılımcısı
SQL veritabanları ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Sql/servers/databases/* |Oluşturma ve SQL veritabanlarını yönetme |
| Microsoft.Sql/servers/read |SQL Server'lar okuma |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Denetim ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditingSettings/* |Denetim ayarlarını düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditRecords/read |Denetim kayıtlarının okunamıyor |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Bağlantı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Veri ilkeleri maskeleme düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Güvenlik Uyarısı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/securityMetrics/* |Güvenlik ölçümleri düzenlenemez. |

### <a name="sql-security-manager"></a>SQL Güvenlik Yöneticisi
SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma Microsoft yetkilendirme |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Sql/servers/auditingPolicies/* |SQL server denetim ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/auditingSettings/* |Oluşturma ve SQL server denetim ayarı yönetme |
| Microsoft.Sql/servers/databases/auditingPolicies/* |SQL server veritabanı denetim ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/auditingSettings/* |SQL server veritabanı denetim ayarlarını oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/auditRecords/read |Okuma denetim kaydı |
| Microsoft.Sql/servers/databases/connectionPolicies/* |SQL server veritabanı bağlantısı ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Oluşturma ve SQL server veritabanı veri maskeleme ilkelerini yönetme |
| Microsoft.Sql/servers/databases/read |Okuma SQL veritabanları |
| Microsoft.Sql/servers/databases/schemas/read |Okuma SQL server veritabanı şemaları |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read |Okuma SQL server veritabanı tablo sütunları |
| Microsoft.Sql/servers/databases/schemas/tables/read |Okuma SQL server veritabanı tabloları |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |SQL server veritabanı güvenlik uyarısı ilkeleri oluşturun ve yönetin |
| Microsoft.Sql/servers/databases/securityMetrics/* |Oluşturma ve SQL server veritabanı güvenlik ölçümleri yönetme |
| Microsoft.Sql/servers/read |SQL Server'lar okuma |
| Microsoft.Sql/servers/securityAlertPolicies/* |SQL server güvenlik uyarısı ilkeleri oluşturun ve yönetin |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="sql-server-contributor"></a>SQL Server Katılımcısı
SQL sunucuları ve veritabanları ancak değil güvenlikle ilgili ilkelerini yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Okuma rolleri ve rol atamaları |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Sql/servers/* |Oluşturun ve SQL sunucularını yönetme |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* |SQL server denetim ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/auditingSettings/* |SQL server denetim ayarları düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditingPolicies/* |SQL server veritabanı denetim ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditingSettings/* |SQL server veritabanı denetim ayarları düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/auditRecords/read |Denetim kayıtlarının okunamıyor |
| Microsoft.Sql/servers/databases/connectionPolicies/* |SQL server veritabanı bağlantı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |SQL server veritabanı veri ilkeleri maskeleme düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |SQL server veritabanı güvenlik uyarısı ilkeleri düzenleyemezsiniz |
| Microsoft.Sql/servers/databases/securityMetrics/* |SQL server veritabanı güvenlik ölçümleri düzenlenemez. |
| Microsoft.Sql/servers/securityAlertPolicies/* |SQL server güvenlik uyarısı ilkeleri düzenleyemezsiniz |

### <a name="classic-storage-account-contributor"></a>Klasik Depolama Hesabı Katılımcısı
Klasik depolama hesaplarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.ClassicStorage/storageAccounts/* |Depolama hesapları oluşturma ve yönetme |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="storage-account-contributor"></a>Depolama Hesabı Katılımcısı
Depolama hesaplarını yönetme, ancak onlara erişimi yok.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Tüm Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/diagnosticSettings/* |Tanılama ayarlarını yönet |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/* |Depolama hesapları oluşturma ve yönetme |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="support-request-contributor"></a>Destek İsteğine Katkıda Bulunan
Oluşturabilir ve abonelik kapsamında destek biletlerini yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Yetkilendirme okuma |
| Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read | Okuma rolleri ve rol atamaları |

### <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi
Azure kaynakları için kullanıcı erişimini yönetebilirsiniz

| **Eylemler** |  |
| --- | --- |
| * / Okuma |Gizli dışındaki tüm türlerinin kaynakları okuyun. |
| Microsoft.Authorization/* |Yetkilendirme yönetme |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="classic-virtual-machine-contributor"></a>Klasik Sanal Makine Katılımcısı
Klasik sanal makineleri ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.ClassicCompute/domainNames/* |Oluşturma ve klasik hesaplama etki alanı adlarını yönetme |
| Microsoft.ClassicCompute/virtualMachines/* |Sanal makineler oluşturun ve yönetin |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action |Ağ güvenlik grupları katılma |
| Microsoft.ClassicNetwork/reservedIps/link/action |Ayrılmış IP bağlantı |
| Microsoft.ClassicNetwork/reservedIps/read |Okuma ayrılmış IP adresleri |
| Microsoft.ClassicNetwork/virtualNetworks/join/action |Sanal ağlar katılma |
| Microsoft.ClassicNetwork/virtualNetworks/read |Sanal ağlar okuma |
| Microsoft.ClassicStorage/storageAccounts/disks/read |Depolama hesabı diskleri okuma |
| Microsoft.ClassicStorage/storageAccounts/images/read |Depolama hesabı görüntüleri okuma |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action |Depolama hesabı anahtarlarını Listele |
| Microsoft.ClassicStorage/storageAccounts/read |Klasik depolama hesaplarını okuma |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="virtual-machine-contributor"></a>Sanal Makine Katılımcısı
Sanal makineler ancak bağlı sanal ağ veya depolama hesabı değil yönetebilirsiniz.

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.Compute/availabilitySets/* |Oluşturun ve işlem kullanılabilirlik kümelerini yönetme |
| Microsoft.Compute/locations/* |Oluşturma ve işlem konumları yönetme |
| Microsoft.Compute/virtualMachines/* |Sanal makineler oluşturun ve yönetin |
| Microsoft.Compute/virtualMachineScaleSets/* |Oluşturma ve sanal makine ölçek kümeleri yönetme |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action |Ağ uygulama ağ geçidi arka uç adres havuzlarında katılma |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Yük Dengeleyici arka uç adres havuzlarında katılma |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action |Yük Dengeleyici katılma gelen NAT havuzları |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Yük Dengeleyici katılma gelen NAT kuralları |
| Microsoft.Network/loadBalancers/read |Okuma yük Dengeleyiciler |
| Microsoft.Network/locations/* |Oluşturma ve ağ konumları yönetme |
| Microsoft.Network/networkInterfaces/* |Oluşturma ve ağ arabirimlerinin yönetme |
| Microsoft.Network/networkSecurityGroups/join/action |Ağ güvenlik grupları katılma |
| Microsoft.Network/networkSecurityGroups/read |Ağ güvenlik grupları okuma |
| Microsoft.Network/publicIPAddresses/join/action |Ağ ortak IP adresleri katılma |
| Microsoft.Network/publicIPAddresses/read |Ağ ortak IP adresleri okuyun |
| Microsoft.Network/virtualNetworks/read |Sanal ağlar okuma |
| Microsoft.Network/virtualNetworks/subnets/join/action |Sanal ağ alt ağları katılma |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Storage/storageAccounts/listKeys/action |Depolama hesabı anahtarlarını Listele |
| Microsoft.Storage/storageAccounts/read |Depolama hesapları okuma |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="classic-network-contributor"></a>Klasik Ağ Katılımcısı
Klasik sanal ağlar ve ayrılmış IP yönetebilir

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.ClassicNetwork/* |Oluşturun ve klasik ağları yönetin |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |

### <a name="web-plan-contributor"></a>Web Planı Katılımcısı
Web planlarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/serverFarms/* |Oluşturma ve sunucu grupları yönetme |

### <a name="website-contributor"></a>Web Sitesi Katılımcısı
Web siteleri ancak olmayan bağlı web planlarını yönetme

| **Eylemler** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Yetkilendirme okuma |
| Microsoft.Insights/alertRules/* |Oluşturma ve Öngörüler uyarı kurallarını yönetme |
| Microsoft.Insights/components/* |Oluşturma ve Insights bileşenlerini yönetme |
| Microsoft.ResourceHealth/availabilityStatuses/read |Kaynakların durumunu okuma |
| Microsoft.Resources/deployments/* |Oluşturma ve kaynak grubu dağıtımı yönetme |
| Microsoft.Resources/subscriptions/resourceGroups/read |Kaynak gruplarını oku |
| Microsoft.Support/* |Oluşturma ve Destek biletlerini yönetme |
| Microsoft.Web/certificates/* |Web sitesi sertifikaları oluşturmak ve yönetmek |
| Microsoft.Web/listSitesAssignedToHostName/read |Bir ana bilgisayar adı atanmış siteler okuma |
| Microsoft.Web/serverFarms/join/action |Sunucu grupları katılma |
| Microsoft.Web/serverFarms/read |Sunucu grupları okuma |
| Microsoft.Web/sites/* |Oluşturma ve (site oluşturma ilişkili uygulama hizmeti planı yazma izinleri de gerektirir) Web sitelerini yönetme |

## <a name="see-also"></a>Ayrıca bkz.
* [Rol tabanlı erişim denetimi](role-based-access-control-configure.md): Azure portalında RBAC ile çalışmaya başlama.
* [Azure rbac'de özel roller](role-based-access-control-custom-roles.md): erişim gereksinimlerinize uyacak şekilde özel roller oluşturma hakkında bilgi edinin.
* [Erişim değişiklik geçmişi raporu oluşturma](role-based-access-control-access-change-history-report.md): RBAC içinde rol atamalarını değiştirme izler.
* [Rol tabanlı erişim denetimi sorun giderme](role-based-access-control-troubleshooting.md): sık karşılaşılan sorunları düzeltmek için öneriler alın.
* [Azure Güvenlik Merkezi'nde izinleri](../security-center/security-center-permissions.md)
