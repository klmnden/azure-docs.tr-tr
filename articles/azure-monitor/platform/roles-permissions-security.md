---
title: Azure İzleyici ile güvenlik rolleri ve izinleri ile çalışmaya başlama
description: Azure İzleyici'nın yerleşik rolleri ve izinleri kaynakları izlemek için erişimi kısıtlamak için kullanmayı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 4949391aded58f27ba8acd5c9ec437e8933f9843
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243434"
---
# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Azure İzleyici ile güvenlik rolleri ve izinleri ile çalışmaya başlama

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Birçok ekip verilerini ve ayarlarını izlemeye erişim kesinlikle düzenleyen gerekir. Özel İzleme (destek mühendisleri, DevOps mühendislerine) üzerinde çalışan takım üyeleri sahipseniz veya yönetilen hizmet sağlayıcısı kullanıyorsanız, bunları oluşturmak için kendi yeteneği sınırlandırırken yalnızca izleme verilerine erişimi vermek isteyebilirsiniz, örneğin, değiştirme, veya kaynakları silin. Bu makalede, azure'da bir kullanıcı için bir yerleşik izleme RBAC rolü uygulamak veya izleme sınırlı izinlere ihtiyaç duyan bir kullanıcı için kendi özel rol oluşturma gösterilmektedir. Ardından, Azure İzleyici ile ilgili kaynaklarınızı ve içerdikleri verilere erişimi nasıl sınırlamak için güvenlik konuları açıklanmaktadır.

## <a name="built-in-monitoring-roles"></a>İzleme yerleşik roller
Azure İzleyicisi'nin yerleşik roller yardımcı olmak için tasarlanmıştır almak ve ihtiyaç duydukları verilere yapılandırmak için altyapı izleme için sorumlu bir etkinleştirme sırasında bir abonelik kaynaklarına erişimi sınırlayın. Azure İzleyici, iki Giden kutusu rolünü sağlar: İzleme okuyucusu ve izleme Katılımcısı için.

### <a name="monitoring-reader"></a>İzleme okuyucusu
İzleme okuyucu rolüne atanan kişi bir Abonelikteki tüm izleme verilerini görüntülemek, ancak herhangi bir kaynağa değiştiremez veya kaynaklarını izleme ile ilgili herhangi bir ayarı düzenlemek. Bu rol, oluşturabilmek için gereken destek veya işlem mühendisleri gibi bir kuruluştaki kullanıcılar için uygundur:

* Portalda izleme panoları görüntüleme ve kendi özel izleme panolar oluşturun.
* Tanımlanan uyarı kuralları görüntüleyebiliriz [Azure uyarıları](alerts-overview.md)
* Ölçümleri kullanarak için sorgu [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell cmdlet'leri](powershell-quickstart-samples.md), veya [platformlar arası CLI](cli-samples.md).
* Portal, Azure İzleyici REST API, PowerShell cmdlet'leri veya platformlar arası CLI kullanarak Etkinlik günlüğü sorgulayın.
* Görünüm [tanılama ayarları](diagnostic-logs-overview.md#diagnostic-settings) bir kaynak için.
* Görünüm [günlük profili](activity-log-export.md) aboneliği.
* Otomatik ölçeklendirme ayarlarını görüntüleyin.
* Uyarı etkinlikleri görüntüle ve ayarlar.
* Application Insights veri erişimi ve yapay ZEKA Analytics'te verileri görüntüleme.
* Çalışma alanı için kullanım verileri dahil olmak üzere Log Analytics çalışma alanı veri arayın.
* Log Analytics Yönetim grupları görüntüleyin.
* Log Analytics çalışma alanında arama şemasını alır.
* Log Analytics çalışma alanında izleme paketlerini listeler.
* Almak ve Log Analytics çalışma alanında kayıtlı aramalar yürütün.
* Log Analytics çalışma alanı depolama yapılandırması alır.

> [!NOTE]
> Bu rol, bir olay hub'ına akış veya bir depolama hesabında depolanan günlük verilerine okuma erişimi sağlamaz. [Aşağıya bakın](#security-considerations-for-monitoring-data) bu kaynaklara erişimini yapılandırma hakkında bilgi için.
> 
> 

### <a name="monitoring-contributor"></a>İzleme katkıda bulunanı
Kişilerin izleme katılımcı rolü, bir Abonelikteki tüm izleme verilerini görüntüleyebilir ve oluşturma veya izleme ayarlarını değiştirebilir, ancak diğer tüm kaynakları değiştiremezsiniz. Bu rolü izleme okuyucusu rolü bir üst kümesidir ve bir kuruluşun izleme takım ya da yukarıdaki izinlere ek olarak, oluşturabilmek için ayrıca ihtiyaç duyan yönetilen hizmet sağlayıcıları için uygundur:

* İzleme panoları, paylaşılan bir panoyu yayımlayın.
* Ayarlama [tanılama ayarları](diagnostic-logs-overview.md#diagnostic-settings) bir kaynak için.\*
* Ayarlama [günlük profili](activity-log-export.md) aboneliği.\*
* Ayarlama etkinliği uyarı kuralları ve ayarları aracılığıyla [Azure uyarıları](alerts-overview.md).
* Application Insights web testleri ve bileşenler oluşturun.
* Log Analytics çalışma alanı paylaşılan anahtarlarını listele.
* Etkinleştirmek veya Log Analytics çalışma alanında izleme paketlerini devre dışı bırakın.
* Oluşturun ve silin ve Log Analytics çalışma alanında kayıtlı aramalar yürütün.
* Oluşturma ve Log Analytics çalışma alanı depolama yapılandırması Sil.

\*Kullanıcı ayrıca bir günlük profilini veya tanılama ayarını belirlemek için hedef kaynak (depolama hesabına veya olay hub'ı ad alanı) Listkeys'i izni verilmesi gerekir.

> [!NOTE]
> Bu rol, bir olay hub'ına akış veya bir depolama hesabında depolanan günlük verilerine okuma erişimi sağlamaz. [Aşağıya bakın](#security-considerations-for-monitoring-data) bu kaynaklara erişimini yapılandırma hakkında bilgi için.
> 
> 

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>İzinler ve özel bir RBAC rollerini izleme
Yukarıdaki yerleşik roller tam ekibinizin ihtiyaçlarını karşılamıyorsa, aşağıdakileri yapabilirsiniz [özel bir RBAC rolü oluşturmak](../../role-based-access-control/custom-roles.md) daha ayrıntılı izinlerine sahip. Açıklamalarının ile yaygın Azure İzleyici RBAC işlemlerini aşağıda verilmiştir.

| İşlem | Açıklama |
| --- | --- |
| Microsoft.Insights/ActionGroups/[Read, yazma, silme] |Okuma/yazma/silme Eylem grupları. |
| Microsoft.Insights/ActivityLogAlerts/[Read, yazma, silme] |Okuma/yazma/silme etkinlik günlüğü uyarıları. |
| Microsoft.Insights/AlertRules/[Read, yazma, silme] |Okuma/yazma/silme uyarı kuralları (Klasik uyarılar). |
| Microsoft.Insights/AlertRules/Incidents/Read |Olaylar (tetiklenen uyarı kuralının geçmişi) için uyarı kuralları listesi. Bu, yalnızca portala geçerlidir. |
| Microsoft.Insights/AutoscaleSettings/[Read, yazma, silme] |Okuma/yazma/silme otomatik ölçeklendirme ayarları. |
| Microsoft.Insights/DiagnosticSettings/[Read, yazma, silme] |Tanılama ayarlarını okuma/yazma/silme. |
| Microsoft.Insights/EventCategories/Read |Etkinlik günlüğünde olası tüm kategorileri sıralar. Azure portal tarafından kullanılır. |
| Microsoft.Insights/eventtypes/digestevents/Read |Bu izin, portal aracılığıyla etkinlik günlüklerine erişmek isteyen kullanıcılar için gereklidir. |
| Microsoft.Insights/eventtypes/values/Read |Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, Etkinlik günlüğünü programlı ve portal erişimi için geçerlidir. |
| Microsoft.Insights/ExtendedDiagnosticSettings/[Read, yazma, silme] | Tanılama ayarları ağ akış günlüklerini okuma/yazma/silme. |
| Microsoft.Insights/LogDefinitions/Read |Bu izin, portal aracılığıyla etkinlik günlüklerine erişmek isteyen kullanıcılar için gereklidir. |
| Microsoft.Insights/LogProfiles/[Read, yazma, silme] |Okuma/yazma/silme günlük profilleri (Etkinlik günlüğü olay hub'ı ya da depolama hesabına akışı). |
| Microsoft.Insights/MetricAlerts/[Read, yazma, silme] |Gerçek zamanlıya yakın ölçüm uyarıları okuma/yazma/silme |
| Microsoft.Insights/MetricDefinitions/Read |Okunan ölçüm tanımları (bir kaynak için ölçüm kullanılabilir türler listesi). |
| Microsoft.Insights/Metrics/Read |Bir kaynak için ölçüm okuyun. |
| Microsoft.Insights/Register/Action |Azure İzleyici kaynak sağlayıcısını kaydedin. |
| Microsoft.Insights/ScheduledQueryRules/[Read, yazma, silme] |Azure İzleyici'de okuma/yazma/silme günlük uyarıları. |



> [!NOTE]
> Kullanıcının kaynak türüne ve kapsamına bu kaynağın okuma erişimi olan bir kaynak gerektirir, uyarılar, tanılama ayarları ve ölçümleri erişin. ("Yazma") oluşturma da hedef kaynak üzerindeki Listkeys'i iznine sahip kullanıcı için bir depolama hesabına veya olay hub'ları akışlara arşivleri tanılama bir ayar veya günlük profili gerektirir.
> 
> 

Örneğin, bir "etkinlik günlük okuyucusu" şunun gibi özel bir RBAC rolü oluşturabilirsiniz yukarıdaki tabloyu kullanarak:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Veri izleme güvenlik konuları
İzleme verilerini — özellikle günlük dosyaları — IP adresleri veya kullanıcı adları gibi gizli bilgiler içerebilir. İzleme verilerini azure'dan üç temel formlarında gelir:

1. Etkinlik, Azure aboneliğinizde tüm denetim düzlemi işlemleri açıklayan günlük.
2. Tanılama günlükleri, kaynak tarafından gösterilen günlüklerdir.
3. Ölçümler, kaynaklara göre gönderilir.

Bu veri türlerini üç bir depolama hesabında depolanmış veya olay ikisi için de genel amaçlı Azure kaynaklarıdır Hub'ına akış. Bunlar genel amaçlı kaynaklar olduğundan, oluşturma, silme ve bunlara erişmek için yönetici ayrılmış ayrıcalıklı bir işlemdir. Kötüye kullanımı önlemesi için izleme ile ilgili kaynaklar için aşağıdaki yöntemleri kullanmanızı öneririz:

* Verileri izlemek için tek ve özel depolama hesabı kullanın. Birden çok depolama hesabında oturum izleme verilerini ayırmanız gerekiyorsa, hiçbir zaman bir depolama hesabı izleme arasında kullanımını paylaşın ve bu olarak olmayan izleme verileri yanlışlıkla İzleme verilerine (örneğin, bir üçüncü taraf SIEM) erişimi yalnızca ihtiyacınız olanlar verebilir İzleme erişim veri.
* Tek ve özel bir Service Bus veya olay hub'ı ad, yukarıdakilerle aynı nedenle tüm tanılama ayarlarını kullanın.
* Ayrı bir kaynak grubu içinde tutarak izleme ile ilgili depolama hesapları veya olay hub'ları erişimi sınırlamak ve [kapsamı kullanan](../../role-based-access-control/overview.md#scope) yalnızca o kaynak grubunun erişimi sınırlamak için izleme roller.
* Hiçbir zaman bir kullanıcının yalnızca izleme verilerine erişim sağlaması gerektiğinde depolama hesaplarını veya abonelik kapsamda event hubs'ı Listkeys'i izni verin. (Ayrılmış bir izleme kaynak grubunuz varsa) bunun yerine, bu kullanıcı bir kaynağa veya kaynak grubu için izin kapsamı.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>İzleme ile ilgili depolama hesaplarına erişimi sınırlandırma
Bir kullanıcı veya uygulama izleme verileri bir depolama hesabındaki erişim gerektiğinde, aşağıdakileri yapmalısınız [hesap SAS oluşturma](https://msdn.microsoft.com/library/azure/mt584140.aspx) izleme verileri blob depolama için hizmet düzeyi salt okunur erişimli bir depolama hesabına. PowerShell'de aşağıdaki gibi görünebilir:

```powershell
$context = New-AzStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Ardından belirteci, depolama alanından okuma hesabı, liste ve o depolama hesabındaki tüm bloblar okuma, varlığa verebilirsiniz.

Alternatif olarak, RBAC bu izinle denetlemek gerekiyorsa, bu varlık, belirli bir depolama hesabına Microsoft.Storage/storageAccounts/listkeys/action izin verebilirsiniz. Bu, bir tanılama ayarı veya bir depolama hesabına arşivleme profili oturum açabilmesi için gereken kullanıcılar için gereklidir. Örneğin, aşağıdaki özel RBAC rolü için kullanıcı veya yalnızca bir depolama hesabından diğerine okumak için gereken bir uygulama oluşturabilirsiniz:

```powershell
$role = Get-AzRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzRoleDefinition -Role $role 
```

> [!WARNING]
> Kullanıcının birincil ve ikincil depolama hesabı anahtarlarını Listele Listkeys'i izni sağlar. Bu anahtarları kullanıcı tüm imzalı izinleri vermek (okuma, yazma, BLOB'ları oluşturmak, silmek BLOB'lar vb.) tamamında (blob, kuyruk, tablo, dosya) Hizmetleri, depolama hesabında oturum. Mümkün olduğunda yukarıda açıklanan bir hesap SAS kullanmanızı öneririz.
> 
> 

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>İzleme ile ilgili olay hub'ları erişimi sınırlandırma
Event hubs ile benzer bir desen gelebilir, ancak öncelikle bir adanmış dinleme yetkilendirme kuralı oluşturmanız gerekir. Vermek istiyorsanız, yalnızca izleme ile ilgili olay hub'larına dinlemek için gereken bir uygulama için erişim aşağıdakileri yapın:

1. Akış yalnızca dinleme talepleri ile izleme verileri için oluşturulan olay hub(ları) bir paylaşılan erişim ilkesi oluşturun. Bu portalda yapılabilir. Örneğin, "monitoringReadOnly." çağırabilirsiniz Mümkünse, tüketiciye doğrudan bu anahtar verin ve bir sonraki adımı atlayın isteyeceksiniz.
2. Tüketici anahtarı geçici hale getirebileceksiniz gerekiyorsa, kullanıcının bu olay hub'ı Listkeys'i eylemi verin. Bu, aynı zamanda tanılama ayarı veya profili event hubs'a akış oturum açabilmesi için gereken kullanıcılar için de gereklidir. Örneğin, bir RBAC kuralı oluşturabilirsiniz:
   
   ```powershell
   $role = Get-AzRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzRoleDefinition -Role $role 
   ```

## <a name="monitoring-within-a-secured-virtual-network"></a>Güvenli bir sanal ağ içindeki izleme

Azure İzleyicisi'ni etkinleştirmeniz hizmetleri sağlamak için Azure kaynaklarına erişimi gerekir. Azure kaynaklarınızı yine de bunları erişimden genel Internet'e korurken izlemek istiyorsanız, aşağıdaki ayarları etkinleştirebilirsiniz.

### <a name="secured-storage-accounts"></a>Güvenli Depolama hesapları 

İzleme verileri, genellikle bir depolama hesabına yazılır. Bir depolama hesabına kopyalanan verileri yetkisiz kullanıcılar tarafından erişilemez olduğundan emin olmak isteyebilirsiniz. Ek güvenlik için yalnızca yetkili kaynaklarınızı izin vermek için ağ erişimi ve bir depolama hesabına güvenilen Microsoft Hizmetleri erişim tuşunu "Seçili ağlar" kullanmak üzere bir depolama hesabı kısıtlayarak kilitleyebilirsiniz.
![Azure depolama Ayarları iletişim kutusu](./media/roles-permissions-security/secured-storage-example.png) Azure İzleyici değerlendirilir bunlardan birini "Güvenilen Microsoft Hizmetleri" güvenli depolama alanına erişmek, güvenilen Microsoft hizmetlerinin izin verirseniz, güvenli depolama hesabınıza erişim; Azure İzleyici olacaktır etkinleştirme Depolama hesabınıza korumalı bu koşullar altında Azure İzleyici tanılama günlükleri, etkinlik günlüğü ve ölçümleri yazma. Bu da Log Analytics'i güvenli depolama alanından günlükleri okuyacak şekilde etkinleştirir.   


Daha fazla bilgi için [ağ güvenlik ve Azure depolama](../../storage/common/storage-network-security.md)

## <a name="next-steps"></a>Sonraki adımlar
* [RBAC ve izinler de Resource Manager hakkında bilgi edinin](../../role-based-access-control/overview.md)
* [Azure'da izlemeye genel bakış okuyun](../../azure-monitor/overview.md)


