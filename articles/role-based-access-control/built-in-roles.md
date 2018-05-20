---
title: Azure rol tabanlı erişim denetimi (RBAC) için yerleşik roller | Microsoft Docs
description: Azure rol tabanlı erişim denetimi (RBAC) için yerleşik roller açıklanmıştır. NotActions ve eylemleri listeler.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/11/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 91f721f5508191c7530e57b6dd96cad3301542a7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi için yerleşik roller
[Rol tabanlı erişim denetimi (RBAC)](overview.md) kullanıcıları, grupları ve hizmet asıl adı atayabilirsiniz birkaç yerleşik rol tanımı yok. Rol atamalarını azure'daki kaynaklara erişimi denetlemek yoludur. Yerleşik roller değiştiremez, ancak kendi oluşturabilirsiniz [özel roller](custom-roles.md) , kuruluşunuzun belirli gereksinimlerine uyacak şekilde.

Yerleşik roller her zaman artmaktadır. Son rol tanımları almak için [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) veya [az rol tanımı listesi](/cli/azure/role/definition#az-role-definition-list).

## <a name="built-in-role-descriptions"></a>Yerleşik rol açıklamaları
Aşağıdaki tabloda yerleşik roller kısa açıklamaları sağlar. Rol adı listesini görmek için tıklatın `actions` ve `notActions` her rol için.


| Yerleşik rolü | Açıklama |
| --- | --- |
| [Sahibi](#owner) | Kaynaklara erişim dahil olmak üzere her şeyi yönetmenizi sağlar. |
| [Katkıda bulunan](#contributor) | Kaynaklara erişim hariç olmak üzere her şeyi yönetmenizi sağlar. |
| [Okuyucu](#reader) | Her şeyi görüntülemenizi sağlar ancak değişiklik yapmanıza izin vermez. |
| [AcrImageSigner](#acrimagesigner) | acr görüntüsü imzalayan |
| [AcrQuarantineReader](#acrquarantinereader) | acr karantina veri okuyucu |
| [AcrQuarantineWriter](#acrquarantinewriter) | acr karantina veri yazıcı |
| [API Management hizmeti katkıda bulunan](#api-management-service-contributor) | Hizmeti ve API'leri yönetebilir |
| [API Management hizmet işleci rolü](#api-management-service-operator-role) | Hizmeti yönetebilir, ancak API'leri yönetemez |
| [API Management hizmet okuyucu rolü](#api-management-service-reader-role) | Hizmet ve API'lere salt okunur erişim |
| [Uygulama Öngörüler bileşen katkıda bulunan](#application-insights-component-contributor) | Application Insights bileşenlerini yönetebilir |
| [Uygulama Öngörüler anlık görüntü hata ayıklayıcı](#application-insights-snapshot-debugger) | Kullanıcıya Application Insights Snapshot Debugger özelliklerini kullanma izni verir |
| [Otomasyon iş işleci](#automation-job-operator) | Otomasyon Runbook'larını kullanarak İş oluşturun ve yönetin. |
| [Automation operatörü](#automation-operator) | Otomasyon Operatörleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir |
| [Otomasyon Runbook işleci](#automation-runbook-operator) | Runbook'un İşlerini oluşturabilmek için Runbook özelliklerini okuyun. |
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
| [Klasik sanal makine Katılımcısı](#classic-virtual-machine-contributor) | Klasik sanal makineleri yönetmenizi sağlar ancak bunlara veya bağlı oldukları sanal ağ ya da depolama hesaplarına yönelik erişimi yönetme izni vermez. |
| [ClearDB MySQL DB katkıda bulunan](#cleardb-mysql-db-contributor) | ClearDB MySQL veritabanlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Cosmos DB hesap okuyucu rolü](#cosmos-db-account-reader-role) | Azure Cosmos DB hesap verileri okuyabilir. Bkz: [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetmek için. |
| [Veri Fabrikası katkıda bulunan](#data-factory-contributor) | Veri fabrikaları ile birlikte bunların alt kaynaklarını oluşturup yönetin. |
| [Data Lake Analytics Geliştirici](#data-lake-analytics-developer) | İşlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics hesabı oluşturmanıza veya silmenize izin vermez. |
| [Veri Purger](#data-purger) | Analytics verilerini temizle |
| [DevTest Labs kullanıcı](#devtest-labs-user) | Azure DevTest Labs'teki tüm sanal makinelerinize bağlanmanıza, bu makineleri başlatmanıza, yeniden başlatmanıza ve kapatmanıza izin verir. |
| [DNS bölgesi katkıda bulunan](#dns-zone-contributor) | Azure DNS'te, DNS bölgelerini ve kayıt kümelerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
| [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) | Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB önceden DocumentDB bilinirdi. |
| [Akıllı sistemler hesap katkıda bulunan](#intelligent-systems-account-contributor) | Akıllı Sistemler hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Anahtar kasası katkıda bulunan](#key-vault-contributor) | Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişmenize izin vermez. |
| [Laboratuvar Oluşturucusu](#lab-creator) | Azure Laboratuvar Hesaplarınız altında yönetilen laboratuvarları oluşturmanıza, yönetmenize ve silmenize olanak sağlar. |
| [Günlük analizi katkıda bulunan](#log-analytics-contributor) | Günlük analizi katkıda bulunan tüm izleme verilerini okuma ve izleme ayarlarını düzenleyin. İzleme ayarları düzenleme, VM'ler için VM uzantısı eklemeyi içerir; Azure Storage günlüklerinden koleksiyonu yapılandırmak için depolama hesabı anahtarlarını okuma; oluşturma ve Automation hesapları yapılandırma; çözümleri ekleme; ve tüm Azure kaynaklara Azure tanılama yapılandırılıyor. |
| [Günlük analizi okuyucu](#log-analytics-reader) | Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir. |
| [Mantığı uygulamasını katkıda bulunan](#logic-app-contributor) | Mantıksal uygulamayı yönetmenize izin verir, ancak bunlara yönelik erişimi yönetmenize izin vermez. |
| [Mantıksal uygulama işleci](#logic-app-operator) | Mantıksal uygulamayı okumanıza, etkinleştirmenize ve devre dışı bırakmanıza izin verir. |
| [Yönetilen kimlik katkıda bulunan](#managed-identity-contributor) | Kullanıcı Tarafından Atanan Kimliği Oluşturma, Okuma, Güncelleştirme ve Silme |
| [Yönetilen kimlik işleci](#managed-identity-operator) | Kullanıcı Tarafından Atanan Kimliği Oku ve Ata |
| [Katkıda bulunan izleme](#monitoring-contributor) | Tüm izleme verileri okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Okuyucu izleme](#monitoring-reader) | Tüm izleme verilerini (ölçümleri, günlükleri, vb.) okuyabilir. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Ağ Katılımcısı](#network-contributor) | Ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Yeni Relic APM hesap katkıda bulunan](#new-relic-apm-account-contributor) | New Relic Application Performance Management hesaplarını ve uygulamalarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Okuyucu ve veri erişimi](#reader-and-data-access) | Sağlayan her şeyi görüntüleyebilir ancak silin veya bir depolama hesabı veya içerilen kaynağın oluşturma izin vermez. Ayrıca bir depolama hesabı depolama hesabı anahtarlarını erişimi aracılığıyla bulunan tüm verileri için okuma/yazma erişimine izin verir. |
| [Redis önbelleği katkıda bulunan](#redis-cache-contributor) | Redis Cache'leri yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Zamanlayıcı İş koleksiyonları katkıda bulunan](#scheduler-job-collections-contributor) | Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Arama hizmeti katkıda bulunan](#search-service-contributor) | Search hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Güvenlik Yöneticisi](#security-admin) | Yalnızca Güvenlik Merkezi'nde: görüntülemek güvenlik ilkeleri, güvenlik durumları görüntülemek, güvenlik ilkeleri, Uyarıları görüntüle ve öneriler düzenleme, uyarısı ve öneri yok sayın |
| [Güvenlik Yöneticisi'ni (eski)](#security-manager-legacy) | Bu eski bir roldür. Lütfen Güvenlik Yöneticisi kullanın |
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
| [Sanal makine yönetici oturum açma](#virtual-machine-administrator-login) | -  Bu role sahip kullanıcılar, bir sanal makinede Windows yöneticisi veya Linux kök kullanıcı ayrıcalıkları ile oturum açabilir. |
| [Sanal makine Katılımcısı](#virtual-machine-contributor) | Sanal makineler, ancak onlara, erişimi ve sanal ağ veya depolama hesabı bağlı oldukları yönetmenize olanak tanır. |
| [Sanal makine kullanıcı oturum açma](#virtual-machine-user-login) | Bu role sahip kullanıcılar, bir sanal makinede normal kullanıcı olarak oturum açabilir. |
| [Web planı katkıda bulunan](#web-plan-contributor) | Web siteleri için web planlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
| [Web sitesi katkıda bulunan](#website-contributor) | Web sitelerini (web planlarını değil) yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |


## <a name="owner"></a>Sahibi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kaynaklara erişim dahil olmak üzere her şeyi yönetmenizi sağlar. |
> | **Kimlik** | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | **Eylemler** |  |
> | * | Oluşturma ve tüm türlerinin kaynakları yönetme |

## <a name="contributor"></a>Katılımcı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kaynaklara erişim hariç olmak üzere her şeyi yönetmenizi sağlar. |
> | **Kimlik** | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | **Eylemler** |  |
> | * | Oluşturma ve tüm türlerinin kaynakları yönetme |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Rolleri ve rol atamalarını silinemiyor |
> | Microsoft.Authorization/*/Write | Rolleri ve rol atamalarını oluşturulamıyor |
> | Microsoft.Authorization/elevateAccess/Action | Çağırana kiracı kapsamında Kullanıcı Erişim Yöneticisi erişimi verir |

## <a name="reader"></a>Okuyucu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Her şeyi görüntülemenizi sağlar ancak değişiklik yapmanıza izin vermez. |
> | **Kimlik** | acdd72a7-3385-48EF-bd42-f606fba81ae7 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |

## <a name="acrimagesigner"></a>AcrImageSigner
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | acr görüntüsü imzalayan |
> | **Kimlik** | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |

## <a name="acrquarantinereader"></a>AcrQuarantineReader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | acr karantina veri okuyucu |
> | **Kimlik** | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="acrquarantinewriter"></a>AcrQuarantineWriter
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | acr karantina veri yazıcı |
> | **Kimlik** | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="api-management-service-contributor"></a>API Yönetimi Hizmeti Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hizmeti ve API'leri yönetebilir |
> | **Kimlik** | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/* | Oluşturma ve API Management hizmeti yönetme |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="api-management-service-operator-role"></a>API Management Hizmet Operatörü Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hizmeti yönetebilir, ancak API'leri yönetemez |
> | **Kimlik** | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
> | Microsoft.ApiManagement/service/backup/action | Bir kullanıcı belirtilen kapsayıcıda yedekleme API Management hizmetine sağlanan depolama hesabı |
> | Microsoft.ApiManagement/service/delete | API Management hizmeti örneği Sil |
> | Microsoft.ApiManagement/service/managedeployments/action | SKU/birimlerini, API Management Hizmet Ekle/Kaldır bölgesel dağıtımları değiştirme |
> | Microsoft.ApiManagement/service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
> | Microsoft.ApiManagement/service/restore/action | API Management hizmeti belirtilen bir kullanıcı tarafından sağlanan depolama hesabı kapsayıcısında geri yükleme |
> | Microsoft.ApiManagement/service/updatecertificate/action | Bir API Management hizmeti için SSL sertifikasını karşıya yükle |
> | Microsoft.ApiManagement/service/updatehostname/action | Kurulum, güncelleştirmeniz ya da bir API Management hizmeti için özel etki alanı adlarını kaldırın |
> | Microsoft.ApiManagement/service/write | API Management hizmeti yeni bir örneğini oluşturma |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini al |

## <a name="api-management-service-reader-role"></a>API Management Hizmet Okuyucusu Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Hizmet ve API'lere salt okunur erişim |
> | **Kimlik** | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/*/read | Okuma API Management hizmeti örnekleri |
> | Microsoft.ApiManagement/service/read | API Management hizmeti örneği ile ilgili meta verilerini okuma |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Kullanıcı anahtarları listesini al |

## <a name="application-insights-component-contributor"></a>Application Insights Bileşeni Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Application Insights bileşenlerini yönetebilir |
> | **Kimlik** | ae349356-3a1b-4a5e-921d-050484c6347e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.Insights/components/* | Oluşturma ve Insights bileşenlerini yönetme |
> | Microsoft.Insights/webtests/* | Oluşturma ve web testleri yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="application-insights-snapshot-debugger"></a>Application Insights Snapshot Debugger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kullanıcıya Application Insights Snapshot Debugger özelliklerini kullanma izni verir |
> | **Kimlik** | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="automation-job-operator"></a>Otomasyon İşi İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Otomasyon Runbook'larını kullanarak İş oluşturun ve yönetin. |
> | **Kimlik** | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışını alır |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Bir Azure Otomasyonu işini askıya alır |
> | Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="automation-operator"></a>Otomasyon Operatörü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Otomasyon Operatörleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir |
> | **Kimlik** | d3881f73-407a-4167-8283-e981cbba0404 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Karma Runbook çalışanı kaynaklarını okur |
> | Microsoft.Automation/automationAccounts/jobs/read | Bir Azure Otomasyonu işini alır |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Bir Azure Otomasyonu işini sürdürür |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Bir Azure Otomasyonu işini durdurur |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Bir Azure Otomasyonu iş akışını alır |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Bir Azure Otomasyonu işini askıya alır |
> | Microsoft.Automation/automationAccounts/jobs/write | Bir Azure Otomasyonu işi oluşturur |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Bir Azure Otomasyonu iş zamanlaması alır |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Bir Azure Otomasyonu iş zamanlaması oluşturur |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Otomasyon hesabı bağlı çalışma alır |
> | Microsoft.Automation/automationAccounts/read | Bir Azure Otomasyonu hesabını alır |
> | Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
> | Microsoft.Automation/automationAccounts/schedules/read | Bir Azure Otomasyonu zamanlama varlığını alır |
> | Microsoft.Automation/automationAccounts/schedules/write | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Bir işin çıktısını alır |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="automation-runbook-operator"></a>Otomasyon Runbook'u İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Runbook'un İşlerini oluşturabilmek için Runbook özelliklerini okuyun. |
> | **Kimlik** | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Automation/automationAccounts/runbooks/read | Bir Azure Otomasyonu runbook'unu alır |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="azure-stack-registration-owner"></a>Azure Stack Kayıt Sahibi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Stack kayıtlarını yönetmenize imkan sağlar. |
> | **Kimlik** | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | **Eylemler** |  |
> | Microsoft.AzureStack/registrations/products/listDetails/action | Bir Azure yığın Market ürün ayrıntılarını genişletilmiş alır |
> | Microsoft.AzureStack/registrations/products/read | Bir Azure yığın Market ürün özelliklerini alır |
> | Microsoft.AzureStack/registrations/read | Bir Azure yığın kaydı özelliklerini alır |

## <a name="backup-contributor"></a>Yedekleme Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yedekleme hizmetini yönetmenize olanak sağlar ancak kasa oluşturma ve diğer kullanıcılara erişim verme izni sağlamaz |
> | **Kimlik** | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Yedekleme Yönetimi işleminin sonuçlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Oluşturma ve kurtarma Hizmetleri kasasına yedekleme yapılarındaki yedekleme kapsayıcılarda yönetme |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturmak ve yönetmek |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Oluşturma ve yedekleme yönetimiyle ilgili meta verileri yönetme |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme Yönetimi işlemlerinin sonuçlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Yedekleme ilkeleri oluşturun ve yönetin |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilir öğelerini oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Yedeklenen öğelerini oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Oluşturma ve yedekleme öğelerini tutan kapsayıcı yönetme |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Kurtarma Hizmetleri kasasına yedekleme ilgili sertifikaları oluşturmak ve yönetmek |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve kasa ilgili genişletilmiş bilgilerini yönetme |
> | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/* | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
> | Microsoft.RecoveryServices/Vaults/usages/* | Oluşturma ve kurtarma Hizmetleri kasası kullanımını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="backup-operator"></a>Yedekleme İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yedekleme kaldırma, kasa oluşturma ve diğer kullanıcılara erişim verme dışındaki yedekleme hizmetlerini yönetmenize olanak sağlar |
> | **Kimlik** | 00c29273-979b-4161-815C-10b084fb9324 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Korumalı Öğe için Yedekleme gerçekleştirir. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğe nesne ayrıntılarını döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Korumalı Öğeler için Kurtarma Noktalarını geri yükleyin. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Bir yedekleme korumalı öğesi oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturmak ve yönetmek |
> | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | İşi iptal |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm iş nesneleri döndürür |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Kurtarma Hizmetleri Kasası için Yedekleme Yönetimi Meta Verilerini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Oluşturma ve yedekleme Yönetimi işlemlerinin sonuçlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkeleri döndürür |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilir öğelerini oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Tüm Korunabilir Öğelerin listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğine ait tüm kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/* | Yeni kapsayıcıları oluşturulan getirme için bulma işlemini yönetme |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Hizmet kapsayıcısı kaydetme işlemi, bir kapsayıcı kurtarma Hizmeti'ne kaydolmak için kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Korumalı öğe için sağlama anlık öğe kurtarma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Anlık öğe kurtarma için korumalı öğe iptal etme |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="backup-reader"></a>Yedekleme Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yedekleme hizmetlerini görüntüleyebilir ancak değişiklik yapamaz |
> | **Kimlik** | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğe nesne ayrıntılarını döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm iş nesneleri döndürür |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Dışarı aktarma işleri |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Kurtarma Hizmetleri Kasası için Yedekleme Yönetimi Meta Verilerini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Kurtarma Hizmetleri Kasası için Yedekleme işleminin Sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminin Sonuçlarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkeleri döndürür |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğine ait tüm kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Özetleri korunan öğeler ve korunan sunucular için bir kurtarma Hizmetleri döndürür. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktalarını alın. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Dışa aktarma işi işleminin sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |

## <a name="billing-reader"></a>Faturalandırma Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Faturalandırma verilerine okuma erişimi verir |
> | **Kimlik** | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Billing/*/read | Faturalama bilgileri okuyun |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Management/managementGroups/read | Kimliği doğrulanmış kullanıcı için Yönetim grupları listesi. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="biztalk-contributor"></a>BizTalk Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | BizTalk hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.BizTalkServices/BizTalk/* | Oluşturma ve BizTalk services yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-endpoint-contributor"></a>CDN Uç Noktası Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
> | **Kimlik** | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-endpoint-reader"></a>CDN Uç Nokta Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
> | **Kimlik** | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-profile-contributor"></a>CDN Profili Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. |
> | **Kimlik** | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cdn-profile-reader"></a>CDN Profil Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. |
> | **Kimlik** | 8f96442b-4075-438f-813d-ad51ab4019af |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-network-contributor"></a>Klasik Ağ Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.ClassicNetwork/* | Oluşturun ve klasik ağları yönetin |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-storage-account-contributor"></a>Klasik Depolama Hesabı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik depolama hesaplarını yönetmenize izin verir ancak bu hesaplara erişim hakkı vermez. |
> | **Kimlik** | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.ClassicStorage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="classic-storage-account-key-operator-service-role"></a>Klasik Depolama Hesabı Anahtarı İşleci Hizmet Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik Depolama Hesabı Anahtarı İşleçlerine, Klasik Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
> | **Kimlik** | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | **Eylemler** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Depolama hesabı için var olan erişim anahtarlarını yeniden oluşturur. |

## <a name="classic-virtual-machine-contributor"></a>Klasik Sanal Makine Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Klasik sanal makineleri yönetmenizi sağlar ancak bunlara veya bağlı oldukları sanal ağ ya da depolama hesaplarına yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.ClassicCompute/domainNames/* | Oluşturma ve klasik hesaplama etki alanı adlarını yönetme |
> | Microsoft.ClassicCompute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir IP'ye bağlantı oluşturun |
> | Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış IP'leri alır |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağa katılır. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Sanal ağı alın. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskini döndürür. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü döndürür. |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba yönelik depolama hesabını döndürün. |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | ClearDB MySQL veritabanlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 9106cda0-8a86-4E81-b686-29a22c54effe |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | successbricks.cleardb/Databases/* | Oluşturma ve ClearDB MySQL veritabanları yönetme |

## <a name="cosmos-db-account-reader-role"></a>Cosmos DB Hesabı Okuyucusu Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Cosmos DB hesap verileri okuyabilir. Bkz: [DocumentDB hesabı katkıda bulunan](#documentdb-account-contributor) Azure Cosmos DB hesapları yönetmek için. |
> | **Kimlik** | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları, her bir kullanıcıya verilen izinler okuyabilir |
> | Microsoft.DocumentDB/*/read | Herhangi bir koleksiyonu okuma |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Veritabanı hesabı readonly anahtarları okur. |
> | Microsoft.Insights/MetricDefinitions/read | Ölçüm tanımlarını oku |
> | Microsoft.Insights/Metrics/read | Ölçümleri okuma |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="data-factory-contributor"></a>Data Factory Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Veri fabrikaları ile birlikte bunların alt kaynaklarını oluşturup yönetin. |
> | **Kimlik** | 673868aa-7521-48A0-acc6-0f60742d39f5 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.DataFactory/dataFactories/* | Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin. |
> | Microsoft.DataFactory/factories/* | Oluşturun ve veri fabrikaları ve bunların içindeki alt kaynakları yönetin. |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="data-lake-analytics-developer"></a>Data Lake Analytics Geliştiricisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | İşlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics hesabı oluşturmanıza veya silmenize izin vermez. |
> | **Kimlik** | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | Bir DataLakeAnalytics hesabı silin. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen işleri iptal etme izni verin. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Oluşturun veya bir DataLakeAnalytics hesabı güncelleştirin. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Oluşturun veya bir bağlantılı DataLakeStore hesabı DataLakeAnalytics hesabının güncelleştirin. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | DataLakeAnalytics hesap DataLakeStore hesabından bağlantısını. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Oluşturun veya bağlı bir depolama hesabında DataLakeAnalytics hesabının güncelleştirin. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Bir depolama hesabı DataLakeAnalytics hesabından bağlantısını. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Oluşturun veya bir güvenlik duvarı kuralı güncelleştirin. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını siler. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Oluşturun veya bir işlem İlkesi güncelleştirin. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Bir işlem ilkeyi silin. |

## <a name="data-purger"></a>Veri Purger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Analytics verilerini temizle |
> | **Kimlik** | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | **Eylemler** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action |  |
> | Microsoft.OperationalInsights/workspaces/*/read |  |
> | Microsoft.OperationalInsights/workspaces/purge/action | Belirtilen veri çalışma alanından Sil |

## <a name="devtest-labs-user"></a>DevTest Labs Kullanıcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure DevTest Labs'teki tüm sanal makinelerinize bağlanmanıza, bu makineleri başlatmanıza, yeniden başlatmanıza ve kapatmanıza izin verir. |
> | **Kimlik** | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Compute/availabilitySets/read | Bir kullanılabilirlik kümesinin özelliklerini al |
> | Microsoft.Compute/virtualMachines/*/read | Bir sanal makine (VM boyutları, çalışma zamanı durumu, VM uzantıları, vb.) özelliklerini okuma |
> | Microsoft.Compute/virtualMachines/deallocate/action | Sanal makineyi kapatır ve işlem kaynaklarını serbest bırakır |
> | Microsoft.Compute/virtualMachines/read | Bir sanal makinenin özelliklerini alır |
> | Microsoft.Compute/virtualMachines/restart/action | Sanal makineyi yeniden başlatır |
> | Microsoft.Compute/virtualMachines/start/action | Sanal makineyi başlatır |
> | Microsoft.DevTestLab/*/read | Bir laboratuvar özelliklerini okuma |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Sanal makineler bir laboratuar ortamında oluşturun. |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Laboratuvar rastgele claimable sanal makinede talep. |
> | Microsoft.DevTestLab/labs/formulas/delete | Formülleri silin. |
> | Microsoft.DevTestLab/labs/formulas/read | Formülleri okuyun. |
> | Microsoft.DevTestLab/labs/formulas/write | Ekleyin veya formüller değiştirin. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Laboratuvar ilkeyi değerlendirir. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Var olan bir sanal makine sahipliğini alın |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzuna katılır |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralına katılır |
> | Microsoft.Network/networkInterfaces/*/read | Bir ağ arabirimi (ağ arabirimi bir parçası olan Örneğin, tüm yük dengeleyicileri) özelliklerini okuma |
> | Microsoft.Network/networkInterfaces/join/action | Bir sanal makine bir ağ arabirimiyle birleştirir |
> | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımını alır.  |
> | Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya mevcut bir ağ arabirimini güncelleştirir.  |
> | Microsoft.Network/publicIPAddresses/*/read | Bir ortak IP adresinin özelliklerini okuma |
> | Microsoft.Network/publicIPAddresses/join/action | Bir genel ip adresine katılır |
> | Microsoft.Network/publicIPAddresses/read | Bir genel ip adresi tanımını alır. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağ birleştirir |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/deployments/read | Dağıtımları alır veya listeler. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makineyi güncelleştirmek için kullanılabilir boyutları listeler |

## <a name="dns-zone-contributor"></a>DNS Bölgesi Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure DNS'te, DNS bölgelerini ve kayıt kümelerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
> | **Kimlik** | befefa01-2a29-4197-83a8-272ff33ce314 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.Network/dnsZones/* | DNS bölgeleri ve kayıtları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="documentdb-account-contributor"></a>DocumentDB Hesabı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Cosmos DB hesapları yönetebilirsiniz. Azure Cosmos DB önceden DocumentDB bilinirdi. |
> | **Kimlik** | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.DocumentDb/databaseAccounts/* | Azure Cosmos DB hesapları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="intelligent-systems-account-contributor"></a>Akıllı Sistemler Hesap Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Akıllı Sistemler hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.IntelligentSystems/accounts/* | Akıllı sistemler hesapları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="key-vault-contributor"></a>Key Vault Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişmenize izin vermez. |
> | **Kimlik** | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Geçici silinen bir anahtar kasasını temizleyin |
> | Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>Laboratuvar Oluşturan
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure Laboratuvar Hesaplarınız altında yönetilen laboratuvarları oluşturmanıza, yönetmenize ve silmenize olanak sağlar. |
> | **Kimlik** | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Bir laboratuvar bir laboratuvar hesabı oluşturun. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Günlük analizi katkıda bulunan tüm izleme verilerini okuma ve izleme ayarlarını düzenleyin. İzleme ayarları düzenleme, VM'ler için VM uzantısı eklemeyi içerir; Azure Storage günlüklerinden koleksiyonu yapılandırmak için depolama hesabı anahtarlarını okuma; oluşturma ve Automation hesapları yapılandırma; çözümleri ekleme; ve tüm Azure kaynaklara Azure tanılama yapılandırılıyor. |
> | **Kimlik** | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server tanılama ayarını okur, güncelleştirir veya oluşturur |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="log-analytics-reader"></a>Log Analytics Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Log Analytics Okuyucusu, tüm izleme verilerinin görüntüleme ve aramanın yanı sıra izleme ayarlarını da (tüm Azure kaynaklarındaki Azure tanılama yapılandırmalarını görüntüleme dahil) görüntüleyebilir. |
> | **Kimlik** | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Yeni altyapısını kullanarak arayın. |
> | Microsoft.OperationalInsights/workspaces/search/action | Bir arama sorgusunu çalıştırır |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |

## <a name="logic-app-contributor"></a>Mantıksal Uygulama Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Mantıksal uygulamayı yönetmenize izin verir, ancak bunlara yönelik erişimi yönetmenize izin vermez. |
> | **Kimlik** | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/read | Belirli bir hesaba yönelik depolama hesabını döndürün. |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server tanılama ayarını okur, güncelleştirir veya oluşturur |
> | Microsoft.Insights/logdefinitions/* | Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğü günlük kategorilerini liste. |
> | Microsoft.Insights/metricDefinitions/* | Ölçüm tanımlarını (bir kaynak için kullanılabilir ölçüm türlerinin listesi) okuyun. |
> | Microsoft.Logic/* | Logic Apps kaynaklarını yönetir. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/connectionGateways/* | Oluşturma ve bir bağlantı ağ geçidi yönetir. |
> | Microsoft.Web/connections/* | Oluşturma ve bir bağlantı yönetir. |
> | Microsoft.Web/customApis/* | Oluşturur ve bir özel API yönetir. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Bir uygulama hizmeti planı üzerinde özelliklerini alma |
> | Microsoft.Web/sites/functions/listSecrets/action | Liste gizli Web Apps işlevleri. |

## <a name="logic-app-operator"></a>Mantıksal Uygulama Operatörü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Mantıksal uygulamayı okumanıza, etkinleştirmenize ve devre dışı bırakmanıza izin verir. |
> | **Kimlik** | 515c2055-d9d4-4321-B1B9-bd0c9a0f79fe |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/*/read | Okuma Öngörüler uyarı kuralları |
> | Microsoft.Insights/diagnosticSettings/*/read | Logic Apps için tanılama ayarlarını alır |
> | Microsoft.Insights/metricDefinitions/*/read | Logic Apps için kullanılabilir ölçümleri alır. |
> | Microsoft.Logic/*/read | Logic Apps kaynaklarını okur. |
> | Microsoft.Logic/workflows/disable/action | İş akışını devre dışı bırakır. |
> | Microsoft.Logic/workflows/enable/action | İş akışını etkinleştirir. |
> | Microsoft.Logic/workflows/validate/action | İş akışını doğrular. |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlem sonuçlarını alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/connectionGateways/*/read | Bağlantı ağ geçidi okuyun. |
> | Microsoft.Web/connections/*/read | Bağlantıları okuyun. |
> | Microsoft.Web/customApis/*/read | Özel API okuyun. |
> | Microsoft.Web/serverFarms/read | Bir uygulama hizmeti planı üzerinde özelliklerini alma |

## <a name="managed-identity-contributor"></a>Yönetilen Kimlik Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kullanıcı Tarafından Atanan Kimliği Oluşturma, Okuma, Güncelleştirme ve Silme |
> | **Kimlik** | e40ec5ca-96e0-45A2-b4ff-59039f2c2b59 |
> | **Eylemler** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="managed-identity-operator"></a>Yönetilen Kimlik İşleci
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kullanıcı Tarafından Atanan Kimliği Oku ve Ata |
> | **Kimlik** | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Eylemler** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="monitoring-contributor"></a>İzleme Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Tüm izleme verileri okuyabilir ve izleme ayarlarını düzenleyin. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Kimlik** | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | **Eylemler** |  |
> | * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/AlertRules/* | Okuma/yazma/silme uyarı kuralları. |
> | Microsoft.Insights/components/* | Okuma/yazma/silme Application Insights bileşenleri. |
> | Microsoft.Insights/DiagnosticSettings/* | Okuma/yazma/silme tanılama ayarları. |
> | Microsoft.Insights/eventtypes/* | Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, etkinlik günlüğü programlı ve portal erişimi için geçerlidir. |
> | Microsoft.Insights/LogDefinitions/* | Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğü günlük kategorilerini liste. |
> | Microsoft.Insights/MetricDefinitions/* | Ölçüm tanımlarını (bir kaynak için kullanılabilir ölçüm türlerinin listesi) okuyun. |
> | Microsoft.Insights/Metrics/* | Bir kaynak için ölçümleri okuyun. |
> | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Microsoft.Insights/webtests/* | Okuma/yazma/silme Application Insights testleri web. |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Okuma/yazma/silme günlük analizi çözüm paketleri. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Okuma/yazma/silme günlük analizi kayıtlı aramalar. |
> | Microsoft.OperationalInsights/workspaces/search/action | Bir arama sorgusunu çalıştırır |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarları Microsoft operasyonel Öngörüler aracıları için çalışma alanına bağlanmak için kullanılır. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Okuma/yazma/silme günlük analizi depolama Insight yapılandırmaları. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>İzleme Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Tüm izleme verilerini (ölçümleri, günlükleri, vb.) okuyabilir. Ayrıca bkz. [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Kimlik** | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
> | Microsoft.OperationalInsights/workspaces/search/action | Bir arama sorgusunu çalıştırır |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="network-contributor"></a>Ağ Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Ağları yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.Network/* | Oluşturun ve ağları yönetin |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="new-relic-apm-account-contributor"></a>New Relic APM Hesap Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | New Relic Application Performance Management hesaplarını ve uygulamalarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 5d28c62d-5b37-4476-8438-e587778df237 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | NewRelic.APM/accounts/* |  |

## <a name="reader-and-data-access"></a>Okuyucu ve Veri Erişimi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Sağlayan her şeyi görüntüleyebilir ancak silin veya bir depolama hesabı veya içerilen kaynağın oluşturma izin vermez. Ayrıca bir depolama hesabı depolama hesabı anahtarlarını erişimi aracılığıyla bulunan tüm verileri için okuma/yazma erişimine izin verir. |
> | **Kimlik** | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |

## <a name="redis-cache-contributor"></a>Redis Cache Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Redis Cache'leri yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Cache/redis/* | Oluşturma ve Redis önbellekleri yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="scheduler-job-collections-contributor"></a>Zamanlayıcı İş Koleksiyonları Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Scheduler/jobcollections/* | Oluşturma ve iş koleksiyonları yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="search-service-contributor"></a>Search Hizmeti Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Search hizmetlerini yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Search/searchServices/* | Oluşturma ve arama hizmetleri yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="security-admin"></a>Güvenlik Yöneticisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yalnızca Güvenlik Merkezi'nde: görüntülemek güvenlik ilkeleri, güvenlik durumları görüntülemek, güvenlik ilkeleri, Uyarıları görüntüle ve öneriler düzenleme, uyarısı ve öneri yok sayın |
> | **Kimlik** | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Authorization/policyAssignments/* | Oluşturma ve ilke atamalarını yönetme |
> | Microsoft.Authorization/policyDefinitions/* | Oluşturma ve ilke tanımları yönetme |
> | Microsoft.Authorization/policySetDefinitions/* | Oluşturun ve ilke kümelerini yönetme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.operationalInsights/workspaces/*/read | Günlük analizi verilerini görüntüleme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |
> | Microsoft.Security/locations/alerts/dismiss/action | Bir güvenlik uyarısını kapatmanın |
> | Microsoft.Security/locations/alerts/activate/action | Bir güvenlik uyarısı etkinleştir |
> | Microsoft.Security/locations/tasks/dismiss/action | Güvenlik açısından kapatın |
> | Microsoft.Security/locations/tasks/activate/action | Güvenlik açısından etkinleştir |
> | Microsoft.Security/policies/write | Güvenlik İlkesi güncelleştirmeleri |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="security-manager-legacy"></a>Güvenlik Yöneticisi (Eski)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bu eski bir roldür. Lütfen Güvenlik Yöneticisi kullanın |
> | **Kimlik** | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.ClassicCompute/*/read | Klasik sanal makineleri yapılandırma bilgilerini okumak |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Klasik sanal makineler için yapılandırma yazma |
> | Microsoft.ClassicNetwork/*/read | Klasik ağ hakkında yapılandırma bilgilerini okuyun |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/* | Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="security-reader"></a>Güvenlik Okuyucu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yalnızca Güvenlik Merkezi'nde: önerileri ve uyarılar, güvenlik ilkeleri, güvenlik durumları görüntüleyebilir ancak değişiklik yapamaz görünümü görüntüleyebilirsiniz |
> | **Kimlik** | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **Eylemler** |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.operationalInsights/workspaces/*/read | Günlük analizi verilerini görüntüleme |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/*/read | Okuma güvenlik bileşenleri ve ilkeleri |

## <a name="site-recovery-contributor"></a>Site Recovery Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Kasa oluşturma ve rol atama işlemleri dışında Site Recovery hizmetini yönetmenize imkan sağlar |
> | **Kimlik** | 6670b86e-a3f7-4917-AC9B-5d6ab1be4567 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Güncelleştirme kaynağı sertifika işlemi kaynak/kasa kimlik bilgileri sertifikası güncelleştirir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Oluşturma ve kasa ilgili genişletilmiş bilgilerini yönetme |
> | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Oluşturma ve kayıtlı kimlikleri yönetme |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Oluşturma veya çoğaltma uyarı ayarlarını güncelleştirme |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Oluşturma ve çoğaltma yapıları yönetme |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturmak ve yönetmek |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Çoğaltma ilkeleri oluşturun ve yönetin |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Oluşturma ve kurtarma planlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Kurtarma Hizmetleri kasası, depolama yapılandırması oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="site-recovery-operator"></a>Site Recovery Operatörü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Yük devretme ve yeniden çalışma dışındaki Site Recovery yönetimi işlemlerini gerçekleştirmenize izin vermez |
> | **Kimlik** | 494ae006-db33-4328-BF46-533a6560a3ca |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmeti tarafından kullanılan iç bir işlemdir |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm uyarı ayarlarını okuma |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Doku Tutarlılığını Denetler |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Ağ geçidi yeniden ilişkilendirin |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Yapı sertifikasını Yenile |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Herhangi bir koruma kapsayıcıdaki okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Korunabilir tüm öğeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Kurtarma noktası Uygula |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Yük devretmenin yürütülmesi |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlanan yük devretme |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Bir çoğaltma kurtarma noktası okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Onarım çoğaltma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Korumalı öğe koruyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Test Yük Devretmesi |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Yük devretme sınaması temizliğini |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Yük devret |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility hizmeti güncelleştirmesi |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Koruma kapsayıcısı eşlemeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Tüm kurtarma Hizmetleri sağlayıcılarını okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Sağlayıcı Yenile |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Tüm depolama sınıflandırması eşlemeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Herhangi bir işi okuma |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturmak ve yönetmek |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Yük devretmenin yürütülmesi kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planlanan yük devretme kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Tüm kurtarma planlarını okuma |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Kurtarma planı koruyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test yük devretme kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test yük devretmesi temizliğini kurtarma planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Yük devretme kurtarma planı |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri kasası için uyarıları okuma |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="site-recovery-reader"></a>Site Recovery Okuyucusu
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Site Recovery durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmenize izin vermez |
> | **Kimlik** | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Uyarılar için kurtarma Hizmetleri kasası alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Zaman uyumsuz olarak gönderilen işlemi için sonuç ve işlem durumunu alma işlemi işlemi kullanılabilir sonuçlar elde |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Alma işlemi kullanılabilir kapsayıcıları için bir kaynak kayıtlı kapsayıcıları alın. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm uyarı ayarlarını okuma |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Herhangi bir ağa okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Tüm ağ eşlemeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Herhangi bir koruma kapsayıcıdaki okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Korunabilir tüm öğeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Tüm korumalı öğeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Bir çoğaltma kurtarma noktası okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Koruma kapsayıcısı eşlemeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Tüm kurtarma Hizmetleri sağlayıcılarını okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Tüm depolama sınıflandırmaları okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Tüm depolama sınıflandırması eşlemeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Herhangi bir işi okuma |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Herhangi bir işi okuma |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm ilkeleri okuma |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Tüm kurtarma planlarını okuma |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Kurtarma Hizmetleri kasası için belirteç bilgileri döndürür. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa belirteci işlemi kasası düzeyi arka uç işlemleri için kasa belirtecini almak için kullanılabilir. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="sql-db-contributor"></a>SQL DB Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | SQL veritabanları, ancak onlara erişimi yönetmenize olanak tanır. Ayrıca, güvenlikle ilgili ilkelerini veya kendi üst SQL Server'lar yönetemezsiniz. |
> | **Kimlik** | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | Oluşturma ve SQL veritabanlarını yönetme |
> | Microsoft.Sql/servers/read | Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Denetim ilkeleri düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Denetim ayarlarını düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob Denetim kayıtlarını alma |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Bağlantı ilkeleri düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Veri ilkeleri maskeleme düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Güvenlik Uyarısı ilkeleri düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Güvenlik ölçümleri düzenlenemez. |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>SQL Güvenlik Yöneticisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenizi sağlar ancak onlara erişimi yönetme izni vermez. |
> | **Kimlik** | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma Microsoft yetkilendirme |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server denetim ilkeleri oluşturun ve yönetin |
> | Microsoft.Sql/servers/auditingSettings/* | Oluşturma ve SQL server denetim ayarı yönetme |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkeleri oluşturun ve yönetin |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL server veritabanı denetim ayarlarını oluşturun ve yönetin |
> | Microsoft.Sql/servers/databases/auditRecords/read | Okuma denetim kaydı |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantısı ilkeleri oluşturun ve yönetin |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Oluşturma ve SQL server veritabanı veri maskeleme ilkelerini yönetme |
> | Microsoft.Sql/servers/databases/read | Veritabanları veya belirtilen veritabanı özelliklerini alır listesini döndürür. |
> | Microsoft.Sql/servers/databases/schemas/read | Veritabanı şemaları listesini alma |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Bir tablodaki sütunların listesini alma |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Bir veritabanının tabloların listesini alma |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarısı ilkeleri oluşturun ve yönetin |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Oluşturma ve SQL server veritabanı güvenlik ölçümleri yönetme |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Sunucuları veya alır belirtilen sunucu için özellikler listesini döndürür. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL server güvenlik uyarısı ilkeleri oluşturun ve yönetin |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="sql-server-contributor"></a>SQL Server Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | SQL sunucularını ve veritabanlarını yönetmenizi sağlar ancak güvenlikle ilgili ilkelerini yönetmenize izin vermez. |
> | **Kimlik** | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Oluşturun ve SQL sunucularını yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | **NotActions** |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server denetim ilkeleri düzenleyemezsiniz |
> | Microsoft.Sql/servers/auditingSettings/* | SQL server denetim ayarları düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkeleri düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL server veritabanı denetim ayarları düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/auditRecords/read | Denetim kayıtlarının okunamıyor |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantı ilkeleri düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server veritabanı veri ilkeleri maskeleme düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarısı ilkeleri düzenleyemezsiniz |
> | Microsoft.Sql/servers/databases/securityMetrics/* | SQL server veritabanı güvenlik ölçümleri düzenlenemez. |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL server güvenlik uyarısı ilkeleri düzenleyemezsiniz |

## <a name="storage-account-contributor"></a>Depolama Hesabı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Depolama hesaplarını yönetmenize izin verir ancak bunlara yönelik erişimi yönetmenize izin vermez. |
> | **Kimlik** | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Tüm Yetkilendirme okuma |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Insights/diagnosticSettings/* | Tanılama ayarlarını yönet |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Kaynak depolama hesabı veya SQL veritabanı gibi bir alt ağa birleştirir. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="storage-account-key-operator-service-role"></a>Depolama Hesabı Anahtarı İşleci Hizmet Rolü
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Depolama Hesabı Anahtarı İşleçlerine, Depolama Hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir |
> | **Kimlik** | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Belirtilen depolama hesabının erişim anahtarlarını yeniden oluşturur. |

## <a name="support-request-contributor"></a>Destek İsteğine Katkıda Bulunan
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Destek istekleri oluşturmanıza ve bunları yönetmenize olanak sağlar |
> | **Kimlik** | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="traffic-manager-contributor"></a>Traffic Manager Katkıda Bulunanı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Traffic Manager profillerini yönetmenize izin verir, ancak bunlara kimlerin erişebildiğini denetlemenize izin vermez. |
> | **Kimlik** | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Okuma rolleri ve rol atamaları |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Azure kaynaklarına yönelik kullanıcı erişimini yönetmenizi sağlar. |
> | **Kimlik** | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Eylemler** |  |
> | * / Okuma | Gizli dışındaki tüm türlerinin kaynakları okuyun. |
> | Microsoft.Authorization/* | Yetkilendirme yönetme |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="virtual-machine-administrator-login"></a>Sanal Makine Yöneticisi Oturum Açma
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | -  Bu role sahip kullanıcılar, bir sanal makinede Windows yöneticisi veya Linux kök kullanıcı ayrıcalıkları ile oturum açabilir. |
> | **Kimlik** | 1c0163c0-47E6-4577-8991-ea5c82e286e4 |
> | **Eylemler** |  |
> | Microsoft.Network/publicIPAddresses/read | Bir genel ip adresi tanımını alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Microsoft.Network/loadBalancers/read | Yük Dengeleyici tanımını alır |
> | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımını alır.  |
> | Microsoft.Compute/virtualMachines/*/read |  |

## <a name="virtual-machine-contributor"></a>Sanal Makine Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Sanal makineler, ancak onlara, erişimi ve sanal ağ veya depolama hesabı bağlı oldukları yönetmenize olanak tanır. |
> | **Kimlik** | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.Compute/availabilitySets/* | Oluşturun ve işlem kullanılabilirlik kümelerini yönetme |
> | Microsoft.Compute/locations/* | Oluşturma ve işlem konumları yönetme |
> | Microsoft.Compute/virtualMachines/* | Sanal makineler oluşturun ve yönetin |
> | Microsoft.Compute/virtualMachineScaleSets/* | Oluşturma ve sanal makine ölçek kümeleri yönetme |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Bir uygulama ağ geçidi arka uç adres havuzuna katılır |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Bir yük dengeleyici arka uç adres havuzuna katılır |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Katıldığı bir yük dengeleyici gelen nat havuzu |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Bir yük dengeleyici gelen nat kuralına katılır |
> | Microsoft.Network/loadBalancers/probes/join/action | Bir yük dengeleyicinin araştırmalar kullanarak sağlar. Örneğin, VM ölçek bu izni healthProbe özelliği ile kümesi araştırma başvuruda bulunabilir. |
> | Microsoft.Network/loadBalancers/read | Yük Dengeleyici tanımını alır |
> | Microsoft.Network/locations/* | Oluşturma ve ağ konumları yönetme |
> | Microsoft.Network/networkInterfaces/* | Oluşturma ve ağ arabirimlerinin yönetme |
> | Microsoft.Network/networkSecurityGroups/join/action | Ağ güvenlik grubuna katılır |
> | Microsoft.Network/networkSecurityGroups/read | Ağ güvenlik grubu tanımını alır |
> | Microsoft.Network/publicIPAddresses/join/action | Bir genel ip adresine katılır |
> | Microsoft.Network/publicIPAddresses/read | Bir genel ip adresi tanımını alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Bir sanal ağ birleştirir |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı öğe nesne ayrıntılarını döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Bir yedekleme korumalı öğesi oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Bir yedekleme koruma hedefini oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma ilkeleri döndürür |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Koruma ilkesi oluşturur |
> | Microsoft.RecoveryServices/Vaults/read | Kasa alma işlemi Azure kaynak türü 'Kasası' temsil eden bir nesneyi alır |
> | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft.RecoveryServices/Vaults/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |

## <a name="virtual-machine-user-login"></a>Sanal Makine Kullanıcı Oturum Açma
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Bu role sahip kullanıcılar, bir sanal makinede normal kullanıcı olarak oturum açabilir. |
> | **Kimlik** | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Eylemler** |  |
> | Microsoft.Network/publicIPAddresses/read | Bir genel ip adresi tanımını alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını Al |
> | Microsoft.Network/loadBalancers/read | Yük Dengeleyici tanımını alır |
> | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımını alır.  |
> | Microsoft.Compute/virtualMachines/*/read |  |

## <a name="web-plan-contributor"></a>Web Planı Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Web siteleri için web planlarını yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/serverFarms/* | Oluşturma ve sunucu grupları yönetme |

## <a name="website-contributor"></a>Web Sitesi Katılımcısı
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Açıklama** | Web sitelerini (web planlarını değil) yönetmenizi sağlar ancak onlara yönelik erişimi yönetme izni vermez. |
> | **Kimlik** | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/read | Yetkilendirme okuma |
> | Microsoft.Insights/alertRules/* | Oluşturma ve Öngörüler uyarı kurallarını yönetme |
> | Microsoft.Insights/components/* | Oluşturma ve Insights bileşenlerini yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynaklar için kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Oluşturma ve kaynak grubu dağıtımı yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Oluşturma ve Destek biletlerini yönetme |
> | Microsoft.Web/certificates/* | Web sitesi sertifikaları oluşturmak ve yönetmek |
> | Microsoft.Web/listSitesAssignedToHostName/read | Ana bilgisayar adı için atanmış siteler adlarını alır. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Bir uygulama hizmeti planı üzerinde özelliklerini alma |
> | Microsoft.Web/sites/* | Oluşturma ve (site oluşturma ilişkili uygulama hizmeti planı yazma izinleri de gerektirir) Web sitelerini yönetme |

## <a name="next-steps"></a>Sonraki adımlar

- [Özel roller](custom-roles.md)
- [Azure Portalı'nı kullanarak rol atamalarını yönetme](role-assignments-portal.md)
- [Azure Güvenlik Merkezi'nde izinleri](../security-center/security-center-permissions.md)
