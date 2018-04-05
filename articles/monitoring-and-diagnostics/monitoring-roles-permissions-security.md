---
title: Roller, izinleri ve güvenlik Azure İzleyicisi ile kullanmaya başlama | Microsoft Docs
description: Azure monitörün yerleşik rolleri ve izinleri kaynakları izlemek için erişimi kısıtlamak için nasıl kullanılacağını öğrenin.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2686e53b-72f0-4312-bcd3-3dc1b4a9b912
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: johnkem
ms.openlocfilehash: 81f083b799e359f69605de22c30d3adc4480e44b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Roller, izinleri ve güvenlik Azure İzleyicisi ile kullanmaya başlama
İzleme verilerini ve ayarları için erişimi kesinlikle yönetmenin birçok ekip gerekir. Örneğin, özel olarak (destek mühendisleri, devops mühendisleri) izleme üzerinde çalışan takım üyeleri sahipseniz veya bir yönetilen hizmet sağlayıcısı kullanıyorsanız, bunları oluşturmak için kendi yeteneği kısıtlama sırasında yalnızca izleme verilerine erişim vermek isteyebilirsiniz, değiştirmek veya kaynakları silin. Bu makalede, hızlı bir şekilde bir kullanıcı Azure içinde yerleşik bir izleme RBAC rolü uygulamak veya kendi özel rol sınırlı izleme izinleri gereken kullanıcılar için yapı gösterilmektedir. Ardından Azure İzleyicisi ile ilgili kaynaklarınızın ve içerdikleri verilere erişimin nasıl sınırlandırmak için güvenlik konuları ele alınmıştır.

## <a name="built-in-monitoring-roles"></a>Yerleşik izleme roller
Azure monitörün yerleşik roller yardımcı olmak için tasarlanmıştır hala elde edilir ve ihtiyaç duydukları verilere yapılandırmak için altyapı izleme için sorumlu etkinleştirme sırasında bir abonelik kaynaklara erişimi sınırlayabilir. Azure İzleyicisi, iki Giden kutusu rolleri sağlar: bir izleme okuyucu ve izleme katılımcı.

### <a name="monitoring-reader"></a>İzleme Okuyucusu
İzleme okuyucu rolüne atanan kişi bir Abonelikteki tüm izleme verilerini görüntüleme ancak herhangi bir kaynağa değiştiremez veya kaynakları izleme ile ilgili ayarları düzenleyin. Bu rol için gerekir desteği veya işlem mühendisleri gibi bir kuruluştaki kullanıcılar için uygundur:

* İzleme panoları portalda görüntüleyebilir ve kendi özel izleme panolar oluşturun.
* Tanımlanan uyarı kuralları [Azure uyarıları](monitoring-overview-unified-alerts.md)
* Ölçümleri kullanarak sorgu [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell cmdlet'leri](insights-powershell-samples.md), veya [platformlar arası CLI](insights-cli-samples.md).
* Portal, Azure İzleyici REST API'si, PowerShell cmdlet'lerini veya platformlar arası CLI kullanarak Etkinlik günlüğü sorgu.
* Görünüm [tanılama ayarlarını](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) bir kaynak için.
* Görünüm [oturum profili](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) aboneliği.
* Otomatik ölçeklendirme ayarlarını görüntüleyin.
* Uyarı etkinliği görüntüle ve ayarlar.
* Application Insights verilere erişmek ve veri AI analizleri görüntüleyin.
* Çalışma alanı için kullanım verilerini de dahil olmak üzere günlük analizi çalışma alanı veri arayın.
* Günlük analizi Yönetim grupları görüntüleyin.
* Günlük analizi arama şemasını alın.
* Günlük analizi Intelligence paketlerini listeler.
* Almak ve günlük kayıtlı aramalar analizi yürütün.
* Günlük analizi depolama yapılandırması alın.

> [!NOTE]
> Bu rol, bir olay hub'ına akışı veya bir depolama hesabında depolanan günlük verileri için okuma erişimi sağlamaz. [Aşağıya bakın](#security-considerations-for-monitoring-data) bu kaynaklara erişim yapılandırma hakkında bilgi için.
> 
> 

### <a name="monitoring-contributor"></a>İzleme Katkıda Bulunanı
Kişilerin izleme katılımcı rolü atanmış bir abonelikte tüm izleme verilerini görüntüleyebilir ve oluşturmak veya izleme ayarlarını değiştirmek, ancak diğer kaynakları değiştiremezsiniz. Bu rolü izleme okuyucu rolüne bir üst kümesidir ve kuruluşun izleme takım veya isteyen, yukarıdaki izinleri yanı sıra de erişebilmeleri için Yönetilen hizmet sağlayıcıları üyeleri için uygundur:

* İzleme panoları paylaşılan Pano olarak yayımlayın.
* Ayarlama [tanılama ayarlarını](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings) bir resource.* için
* Ayarlama [oturum profili](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile) bir subscription.* için
* Uyarı kuralları etkinliği ve ayarları aracılığıyla ayarlama [Azure uyarıları](monitoring-overview-unified-alerts.md).
* Application Insights web testleri ve bileşenleri oluşturun.
* Günlük analizi çalışma alanı paylaşılan anahtarlar listeleyin.
* Etkinleştirmek veya devre dışı günlük analizi Intelligence paketleri.
* Oluşturma ve silme ve günlük kayıtlı aramalar analizi yürütme.
* Oluşturun ve günlük analizi depolama yapılandırması silin.

* Kullanıcı ayrıca günlük profil veya tanılama ayarını ayarlamak için ListKeys (depolama hesabı veya olay hub'ad alanı) hedef kaynak üzerindeki izni verilmesi gerekir.

> [!NOTE]
> Bu rol, bir olay hub'ına akışı veya bir depolama hesabında depolanan günlük verileri için okuma erişimi sağlamaz. [Aşağıya bakın](#security-considerations-for-monitoring-data) bu kaynaklara erişim yapılandırma hakkında bilgi için.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>İzinler ve özel RBAC rolleri izleme
Yukarıdaki yerleşik roller ekibinizin tam gereksinimlerini karşılamıyorsa, aşağıdakileri yapabilirsiniz [özel RBAC rolü oluşturma](../active-directory/role-based-access-control-custom-roles.md) daha ayrıntılı izinlere sahip. Açıklamaları ortak Azure İzleyici RBAC işlemleriyle aşağıda verilmiştir.

| İşlem | Açıklama |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Read, yazma, silme] |Okuma/yazma/silme Eylem grupları. |
| Microsoft.Insights/ActivityLogAlerts/[Read, yazma, silme] |Okuma/yazma/silme etkinlik günlüğü uyarıları. |
| Microsoft.Insights/AlertRules/[Read, Write, Delete] |Okuma/yazma/silme uyarı kurallarını (Uyarılar Klasik). |
| Microsoft.Insights/AlertRules/Incidents/Read |Olaylar (tetiklenen uyarı kuralı geçmişini) için uyarı kuralları listesi. Bu, yalnızca portalına geçerlidir. |
| Microsoft.Insights/AutoscaleSettings/[Read, yazma, silme] |Okuma/yazma/silme otomatik ölçeklendirme ayarları. |
| Microsoft.Insights/DiagnosticSettings/[Read, Write, Delete] |Okuma/yazma/silme tanılama ayarları. |
| Microsoft.Insights/EventCategories/Read |Etkinlik günlüğünde olası tüm kategorileri numaralandırır. Azure portal tarafından kullanılır. |
| Microsoft.Insights/eventtypes/digestevents/Read |Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. |
| Microsoft.Insights/eventtypes/values/Read |Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, etkinlik günlüğü programlı ve portal erişimi için geçerlidir. |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Read, Write, Delete] | Okuma/yazma/silme ağ akış günlükleri için tanılama ayarları. |
| Microsoft.Insights/LogDefinitions/Read |Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. |
| Microsoft.Insights/LogProfiles/[Read, Write, Delete] |Okuma/yazma/silme günlük profilleri (Etkinlik günlüğü olay hub'ı ya da depolama hesabınıza akış). |
| Microsoft.Insights/MetricAlerts/[Read, yazma, silme] |Yakın gerçek zamanlı ölçüm uyarıları okuma/yazma/silme |
| Microsoft.Insights/MetricDefinitions/Read |Ölçüm tanımlarını (bir kaynak için kullanılabilir ölçüm türlerinin listesi) okuyun. |
| Microsoft.Insights/Metrics/Read |Bir kaynak için ölçümleri okuyun. |
| Microsoft.Insights/Register/Action |Azure İzleyicisi kaynak sağlayıcısı kaydedin. |
| Microsoft.Insights/ScheduledQueryRules/[Read, Write, Delete] |Application Insights için okuma/yazma/silme günlük uyarıları. |



> [!NOTE]
> Kullanıcının kaynak türü ve bu kaynağın kapsamını okuma erişimi olan bir kaynak gerektirir uyarıları, tanılama ayarlarını ve ölçümleri erişin. ("Yazma") oluşturma kullanıcıya, aynı zamanda hedef kaynak üzerindeki ListKeys izne sahip bir depolama hesabı veya olay hub'ları akışlara arşivler tanılama bir ayarı veya günlük profili gerektirir.
> 
> 

Örneğin, bir "etkinlik günlüğü okuyucusu" şöyle için özel bir RBAC rolü oluşturabilir yukarıdaki tabloyu kullanarak:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Verilerin izlenmesi için güvenlik konuları
İzleme verilerini — özellikle günlük dosyalarını — IP adresleri veya kullanıcı adları gibi gizli bilgiler içerebilir. İzleme verilerini Azure üç temel formlarında gelir:

1. Etkinlik, Azure aboneliğinizde tüm denetim düzlemi eylemleri açıklayan günlük.
2. Tanılama günlükleri, bir kaynak tarafından gösterilen günlükleri.
3. Kaynaklar tarafından gösterilen ölçümleri.

Bu veri türlerini üç bir depolama hesabında depolanan veya olay her ikisi de genel amaçlı Azure kaynaklardır Hub'ına akışı. Bu genel amaçlı kaynaklar olduğundan, oluşturma, silme ve bunlara erişmek için bir yönetici ayrılmış ayrıcalıklı bir işlemdir. Kötüye kullanımı önlemek için izleme ile ilgili kaynaklar için aşağıdaki yöntemleri kullanmanızı öneririz:

* Bir tek, özel bir depolama hesabı verileri izlemek için kullanın. İzleme verilerini birden çok depolama hesabı ayırmak gerekiyorsa, hiçbir zaman bir depolama hesabı izleme arasında kullanımı paylaşabilir ve bu olarak izleme olmayan veri yanlışlıkla yalnızca izleme verilerine (örneğin, bir üçüncü taraf SIEM) erişmesi gereken olanlar verebilir İzleme erişimi veri.
* Tek ve özel bir hizmet veri yolu veya olay hub'ın ad yukarıdaki gibi aynı nedenden dolayı tüm tanılama ayarlarını kullanın.
* Farklı bir kaynak grubunda tutarak izleme ile ilgili depolama hesapları veya olay hub'ları erişimi sınırlayabilir ve [kapsamı kullan](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) yalnızca o kaynak grubu erişimi sınırlamak için izleme rolleri.
* Hiçbir zaman ListKeys ya da depolama hesapları için izni veya olay hub'ları abonelik kapsamında bir kullanıcı yalnızca izleme verilerine erişimi olması gerekir. (Ayrılmış bir izleme kaynak grubu varsa) bunun yerine, bu izinleri bir kaynak veya kaynak grubu kullanıcıya vermek kapsamı.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>İzleme ile ilgili depolama hesaplarına erişimi sınırlandırma
Bir kullanıcı veya uygulama İzleme verilerine bir depolama hesabındaki erişim sağlaması gerektiğinde, gereken [bir hesap SAS oluşturmak](https://msdn.microsoft.com/library/azure/mt584140.aspx) depolama hesabındaki blob depolama hizmet düzeyi salt okunur erişimi olan izleme verilerini içerir. PowerShell'de, bunu aşağıdaki gibi görünmelidir:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Ardından belirteç varlığa o depolama alanından okunmasını hesabı ve listelemek ve o depolama hesabındaki tüm BLOB'lar okuma olduğunu verebilirsiniz.

Alternatif olarak, bu izne sahip RBAC denetlemeye ihtiyacınız varsa, o varlık bu belirli depolama hesabı Microsoft.Storage/storageAccounts/listkeys/action izni verebilirsiniz. Bu, tanılama ayarını ayarlayın veya bir depolama hesabına arşivlemek için profil oturum açabilmesi için gereken kullanıcılar için gereklidir. Örneğin, bir kullanıcı veya yalnızca bir depolama hesabından okumak için gereken uygulama için aşağıdaki özel RBAC rolü oluşturabilirsiniz:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [!WARNING]
> ListKeys izni kullanıcının birincil ve ikincil depolama hesabı anahtarlarını Listele sağlar. Bu anahtarları kullanıcı tüm imzalı izin ver (okuma, yazma, BLOB'ları oluşturmak, silmek BLOB'lar vb.) tüm hizmetleri (blob, kuyruk, tablo, dosya), depolama hesabındaki imzalanmış. Mümkün olduğunda yukarıda açıklanan bir hesap SAS kullanılmasını öneririz.
> 
> 

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>İzleme ile ilgili olay hub'ları erişimi sınırlandırma
Event hubs ile benzer bir desen izlenebilir, ancak önce adanmış bir dinleme yetkilendirme kuralı oluşturmanız gerekir. İzleme ile ilgili event hubs'a dinlemek için yalnızca gereken bir uygulamaya erişim vermek istiyorsanız, aşağıdakileri yapın:

1. Yalnızca dinleme talepleri ile izleme verilerini akış için oluşturulan olay ölçeklendirecek bir paylaşılan erişim ilkesi oluşturun. Bu portalda yapılabilir. Örneğin, "monitoringReadOnly." çağırabilirsiniz Mümkünse, bu anahtarı doğrudan tüketiciye verin ve bir sonraki adımı atlayın isteyeceksiniz.
2. Tüketici anahtarı geçici almak mümkün olması gerekiyorsa, kullanıcı bu olay hub'ın ListKeys eylem verin. Bu, aynı zamanda bir tanılama ayarını ayarlayın veya olay hub'ları akışına profili oturum açabilmesi için gereken kullanıcılar için de gereklidir. Örneğin, bir RBAC kuralı oluşturabilirsiniz:
   
   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```

## <a name="next-steps"></a>Sonraki adımlar
* [RBAC ve izinlerini Kaynak Yöneticisi'ni okuyun](../active-directory/role-based-access-control-what-is.md)
* [Azure'da izleme genel bakış bilgileri okuyun](monitoring-overview.md)

