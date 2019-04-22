---
title: Azure Automation’da Rol Tabanlı Erişim Denetimi
description: Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Bu makalede, Azure Automation’da RBAC’nin nasıl ayarlanacağı açıklanmaktadır.
keywords: otomasyon rbac, rol tabanlı erişim denetimi, azure rbac
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: bcbda2464a4607aaa0b1bb96ef8f34c8713cb5f1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58918799"
---
# <a name="role-based-access-control-in-azure-automation"></a>Azure Automation’da Rol Tabanlı Erişim Denetimi

Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Kullanarak [RBAC](../role-based-access-control/overview.md), ayırabilir, takım içinde ve kullanıcıları, grupları sadece erişim miktarını vermek ve ihtiyaç duydukları uygulamaları işlerini gerçekleştirin. Kullanıcılara rol tabanlı erişim Azure portalı, Azure Komut Satırı araçları ve Azure Management API'leri kullanılarak verilebilir.

## <a name="roles-in-automation-accounts"></a>Automation hesaplarında rolleri

Azure Automation’da, otomasyon hesabı kapsamında kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atanarak erişim verilir. Aşağıda Automation hesabının desteklediği yerleşik roller bulunmaktadır:

| **Rol** | **Açıklama** |
|:--- |:--- |
| Sahip |Sahip rolü, tüm kaynaklara ve diğer kullanıcıları, grupları ve Otomasyon hesabını yönetmek üzere uygulamalar için erişim sağlamak da dahil bir Otomasyon hesabı içindeki işlemlere erişim sağlar. |
| Katılımcı |Katılımcı rolü, başka kullanıcının Otomasyon hesabına erişim izinlerini değiştirme dışında her şeyi yönetmenizi sağlar. |
| Okuyucu |Okuyucu rolü, Otomasyon hesabında tüm kaynakları görmenizi sağlar; ancak değişiklik yapamazsınız. |
| Otomasyon Operatörü |Otomasyon operatörü rolü, runbook adı ve özelliklerini görüntülemek ve oluşturmak ve bir Otomasyon hesabında tüm runbook'lar için iş yönetmenize olanak sağlar. Bu rol, kimlik bilgileri varlıkları ve runbook'ları gibi Automation hesabı kaynaklarınızın görüntülenmesini veya değiştirilmesini engellemek, ancak yine de kuruluş üyelerinin bu runbook’ları yürütmesine izin vermek istiyorsanız yararlıdır. |
|Otomasyon İşi İşleci|Otomasyon işi işleci rolü, Automation hesabı tüm runbook'lar için iş oluşturma ve yönetme sağlar.|
|Otomasyon Runbook'u İşleci|Otomasyon Runbook operatörü rolü, bir runbook'un adını ve özelliklerini görüntülemenize olanak sağlar.|
| Log Analytics Katkıda Bulunan | Log Analytics katkıda bulunan rolü, tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenlemek sağlar. İzleme ayarlarını düzenleme, oluşturma ve Otomasyon hesaplarını yapılandırma, çözüm ekleme ve Azure tanılama yapılandırma Azure Depolama'dan günlüklerin toplanmasını yapılandırma yapabilmek için depolama hesabı anahtarlarını okuma VM'ler, VM uzantısı ekleme içerir Tüm Azure kaynakları.|
| Log Analytics Okuyucusu | Log Analytics okuyucusu rolü, görüntüleme ve tüm izleme verilerini yanı sıra izleme ayarlarını görünümü arama sağlar. Bu, tüm Azure kaynaklarındaki Azure Tanılama yapılandırmasını görüntüleme içerir. |
| İzleme Katkıda Bulunanı | İzleme katılımcı rolü tüm izleme verileri ve güncelleştirme izleme ayarlarını okumanıza izin verir.|
| İzleme Okuyucusu | İzleme okuyucusu rolü tüm izleme verilerini okumanıza izin verir. |
| Kullanıcı Erişimi Yöneticisi |Kullanıcı Erişimi Yöneticisi rolü, Azure Otomasyonu hesaplarına kullanıcı erişimini yönetmenizi sağlar. |

## <a name="role-permissions"></a>Rol izinleri

Aşağıdaki tablolarda her role verilen belirli izinler açıklanmaktadır. Bu izinleri verme, eylemleri ve bunları kısıtlamak NotActions, içerebilir.

### <a name="owner"></a>Sahip

Bir sahibi erişim dahil her şeyi yönetebilir. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|Eylemler|Açıklama|
|---|---|
|Microsoft.Automation/automationAccounts/|Oluşturun ve tüm türlerdeki kaynakların yönetin.|

### <a name="contributor"></a>Katılımcı

Katkıda bulunan erişim dışında her şeyi yönetebilir. Aşağıdaki tabloda verilen ve reddedilen rol için izinlerini gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Tüm türlerin kaynak oluşturma ve yönetme|
|**Eylemleri değil**||
|Microsoft.Authorization/*/Delete| Rolleri ve rol atamaları silin.       |
|Microsoft.Authorization/*/Write     |  Rolleri ve rol ataması oluşturun.       |
|Microsoft.Authorization/elevateAccess/Action    | Kullanıcı erişimi Yöneticisi oluşturma yeteneğini engeller.       |

### <a name="reader"></a>Okuyucu

Okuyucu, Otomasyon hesabında tüm kaynakları görüntüleyebilir ancak değişiklik yapamazsınız.

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Automation hesabında tüm kaynakları görüntüleyin. |

### <a name="automation-operator"></a>Otomasyon Operatörü

Otomasyon operatörü oluşturabilmek ve işlerini yönetme ve runbook adları ve tüm runbook'ları bir Otomasyon hesabı özelliklerini okuyun.  Not: Tek tek işleci erişimi denetlemek istiyorsanız, runbook'ları sonra kullanmayın bu rolü ayarlayın ve bunun yerine 'Otomasyon işi işleci' ve 'Otomasyon Runbook'u işleci' rolleri birlikte kullanın. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Authorization/*/read|Yetkilendirme okuyun.|
|Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read|Karma Runbook çalışanı kaynaklarını okur.|
|Microsoft.Automation/automationAccounts/jobs/read|Runbook'un işlerini listeleyin.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Duraklatılmış bir işini devam ettirir.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Devam eden bir işi iptal edin.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|İş akışları ve çıkış okuyun.|
|Microsoft.Automation/automationAccounts/jobs/output/read|Bir işin çıktısını alın.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Devam eden bir işi duraklatın.|
|Microsoft.Automation/automationAccounts/jobs/write|İşleri oluşturun.|
|Microsoft.Automation/automationAccounts/jobSchedules/read|Bir Azure Otomasyonu iş zamanlaması alın.|
|Microsoft.Automation/automationAccounts/jobSchedules/write|Bir Azure Otomasyonu iş zamanlaması oluşturun.|
|Microsoft.Automation/automationAccounts/linkedWorkspace/read|Otomasyon hesabına bağlı çalışma alanı alınamadı.|
|Microsoft.Automation/automationAccounts/read|Azure Otomasyonu hesabı alın.|
|Microsoft.Automation/automationAccounts/runbooks/read|Azure Otomasyonu runbook'u alın.|
|Microsoft.Automation/automationAccounts/schedules/read|Bir Azure Otomasyonu zamanlama varlığını alır.|
|Microsoft.Automation/automationAccounts/schedules/write|Veya bir Azure Otomasyonu zamanlama varlığını güncelleştirilemiyor.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rolleri ve rol atamalarını okuyun.         |
|Microsoft.Resources/deployments/*      |Oluşturun ve kaynak grubu dağıtımı yönetin.         |
|Microsoft.Insights/alertRules/*      | Oluşturun ve uyarı kurallarını yönetin.        |
|Microsoft.Support/* |Oluşturun ve Destek biletlerini yönetebilir.|

### <a name="automation-job-operator"></a>Otomasyon İşi İşleci

Bir Otomasyon işi işleci rolü, Otomasyon hesabı kapsamında verilir. Bu hesaptaki tüm runbook'lar için işleri oluşturmak ve yönetmek operatör izinleri sağlar. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Authorization/*/read|Yetkilendirme okuyun.|
|Microsoft.Automation/automationAccounts/jobs/read|Runbook'un işlerini listeleyin.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Duraklatılmış bir işini devam ettirir.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Devam eden bir işi iptal edin.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|İş akışları ve çıkış okuyun.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Devam eden bir işi duraklatın.|
|Microsoft.Automation/automationAccounts/jobs/write|İşleri oluşturun.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Rolleri ve rol atamalarını okuyun.       |
|Microsoft.Resources/deployments/*      |Oluşturun ve kaynak grubu dağıtımı yönetin.         |
|Microsoft.Insights/alertRules/*      | Oluşturun ve uyarı kurallarını yönetin.        |
|Microsoft.Support/* |Oluşturun ve Destek biletlerini yönetebilir.|

### <a name="automation-runbook-operator"></a>Otomasyon Runbook'u İşleci

Otomasyon Runbook işletmeni rolü Runbook kapsamda verilir. Bir Otomasyon Runbook'u işleci runbook'un adını ve özelliklerini görüntüleyebilirsiniz.  İşleci ayrıca oluşturup runbook işlerini yönetmek 'Otomasyon işi işleci' rolüyle birlikte bu rolü etkinleştirir. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Runbook'ları listeler.        |
|Microsoft.Authorization/*/read      | Yetkilendirme okuyun.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rolleri ve rol atamalarını okuyun.         |
|Microsoft.Resources/deployments/*      | Oluşturun ve kaynak grubu dağıtımı yönetin.         |
|Microsoft.Insights/alertRules/*      | Oluşturun ve uyarı kurallarını yönetin.        |
|Microsoft.Support/*      | Oluşturun ve Destek biletlerini yönetebilir.        |

### <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunan

Log Analytics katkıda bulunan tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyin. İzleme ayarlarını düzenleme Vm'lere VM uzantısı ekleme içerir; Azure Depolama'dan günlüklerin toplanmasını yapılandırma yapabilmek için depolama hesabı anahtarlarını okuma; oluşturma ve Otomasyon hesapları yapılandırma; çözümler eklenerek; ve tüm Azure kaynaklarında Azure tanılamayı yapılandırma. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dizileri dışında tüm türler kaynakları okuyun.|
|Microsoft.Automation/automationAccounts/*|Otomasyon hesapları yönetin.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Oluşturun ve sanal makine uzantıları yönetin.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Klasik depolama hesabı anahtarlarını listele.|
|Microsoft.Compute/virtualMachines/extensions/*|Oluşturun ve klasik sanal makine uzantıları yönetin.|
|Microsoft.Insights/alertRules/*|Okuma/yazma/silme uyarı kuralları.|
|Microsoft.Insights/diagnosticSettings/*|Tanılama ayarlarını okuma/yazma/silme.|
|Microsoft.OperationalInsights/*|Azure İzleyici günlüklerine yönetin.|
|Microsoft.OperationsManagement/*|Çalışma alanları çözümlerinde yönetin.|
|Microsoft.Resources/deployments/*|Oluşturun ve kaynak grubu dağıtımı yönetin.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Oluşturun ve kaynak grubu dağıtımı yönetin.|
|Microsoft.Storage/storageAccounts/listKeys/action|Depolama hesabı anahtarlarını listele.|
|Microsoft.Support/*|Oluşturun ve Destek biletlerini yönetebilir.|

### <a name="log-analytics-reader"></a>Log Analytics Okuyucusu

Log Analytics okuyucusu görüntüleyebilir ve tüm izleme verilerini ve ayarları, tüm Azure kaynaklarındaki Azure Tanılama yapılandırmasını görüntüleme dahil olmak üzere izleme görünümü yanı arayın. Aşağıdaki tabloda verilen veya reddedilen rol için izinleri gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dizileri dışında tüm türler kaynakları okuyun.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Azure İzleyici günlüklerine sorgularda yönetin.|
|Microsoft.OperationalInsights/workspaces/search/action|Azure İzleyici günlük verileri arayın.|
|Microsoft.Support/*|Oluşturun ve Destek biletlerini yönetebilir.|
|**Eylemleri değil**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Paylaşılan erişim anahtarları okumak karşılaştırılamıyor.|

### <a name="monitoring-contributor"></a>İzleme Katkıda Bulunanı

İzleme katkıda bulunan tüm izleme verilerini okuyabilir ve izleme ayarlarını güncelleştirebilir. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dizileri dışında tüm türler kaynakları okuyun.|
|Microsoft.AlertsManagement/alerts/*|Uyarıları yönetme.|
|Microsoft.AlertsManagement/alertsSummary/*|Uyarı Panosu yönetin.|
|Microsoft.Insights/AlertRules/*|Uyarı kurallarını yönetin.|
|Microsoft.Insights/components/*|Application Insights bileşenlerini yönetin.|
|Microsoft.Insights/DiagnosticSettings/*|Tanılama ayarlarını yönetin.|
|Microsoft.Insights/eventtypes/*|Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, Etkinlik günlüğünü programlı ve portal erişimi için geçerlidir.|
|Microsoft.Insights/LogDefinitions/*|Bu izin, portal aracılığıyla etkinlik günlüklerine erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğünde günlük kategorileri listesi.|
|Microsoft.Insights/MetricDefinitions/*|Okunan ölçüm tanımları (bir kaynak için ölçüm kullanılabilir türler listesi).|
|Microsoft.Insights/Metrics/*|Bir kaynak için ölçüm okuyun.|
|Microsoft.Insights/Register/Action|Microsoft.Insights sağlayıcısını kaydedin.|
|Microsoft.Insights/webtests/*|Application Insights web testleri yönetin.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Azure İzleyici günlüklerine çözüm paketleri yönetin.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Azure İzleyici Günlükleri kaydedilen aramaları yönetin.|
|Microsoft.OperationalInsights/workspaces/search/action|Log Analytics çalışma alanları arayın.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Log Analytics çalışma alanı için anahtarları listesi.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Azure İzleyici günlüklerine depolama Insight yapılandırmaları yönetin.|
|Microsoft.Support/*|Oluşturun ve Destek biletlerini yönetebilir.|
|Microsoft.WorkloadMonitor/workloads/*|İş yüklerini yönetin.|

### <a name="monitoring-reader"></a>İzleme Okuyucusu

Bir izleme okuyucusu, tüm izleme verilerini okuyabilir. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dizileri dışında tüm türler kaynakları okuyun.|
|Microsoft.OperationalInsights/workspaces/search/action|Log Analytics çalışma alanları arayın.|
|Microsoft.Support/*|Oluşturma ve Destek biletlerini yönetme|

### <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi

Kullanıcı erişimi Yöneticisi, Azure kaynaklarına kullanıcı erişimini yönetebilir. Aşağıdaki tabloda, rol için verilen izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Tüm kaynakları okuyun|
|Microsoft.Authorization/*|Yetkilendirme yönetme|
|Microsoft.Support/*|Oluşturma ve Destek biletlerini yönetme|

## <a name="onboarding"></a>Ekleme

Aşağıdaki tabloda, onboarding sanal makineler için değişiklik izleme için gerekli en düşük gerekli izinleri göster veya yönetim çözümleri güncelleştirin.

### <a name="onboarding-from-a-virtual-machine"></a>Bir sanal makineden ekleme

|**Eylem**  |**İzni**  |**En düşük kapsamı**  |
|---------|---------|---------|
|Yeni dağıtım yazma      | Microsoft.Resources/deployments/*          |Abonelik          |
|Yeni kaynak grubu yazma      | Microsoft.Resources/subscriptions/resourceGroups/write        | Abonelik          |
|Yeni varsayılan çalışma alanı oluşturma      | Microsoft.OperationalInsights/workspaces/write         | Kaynak grubu         |
|Yeni hesap oluşturun      |  Microsoft.Automation/automationAccounts/write        |Kaynak grubu         |
|Bağlantı çalışma alanı ve hesabı      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Çalışma alanı</br>Otomasyon hesabı
|Çözüm oluştur      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Kaynak grubu          |
|MMA uzantısı oluşturma      | Microsoft.Compute/virtualMachines/write         | Sanal Makine         |
|Kaydedilmiş arama oluştur      | Microsoft.OperationalInsights/workspaces/write          | Çalışma alanı         |
|Kapsam yapılandırması oluşturma      | Microsoft.OperationalInsights/workspaces/write          | Çalışma alanı         |
|Kapsam yapılandırmasına bağlantı çözümü      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Çözüm         |
|Ekleme durumu denetleyin - okuma çalışma      | Microsoft.OperationalInsights/workspaces/read         | Çalışma alanı         |
|Durum denetimi ekleme - okuma bağlı hesabının çalışma özelliği     | Microsoft.Automation/automationAccounts/read      | Otomasyon hesabı        |
|Ekleme durumu denetleyin - okuma çözümü      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Çözüm         |
|Ekleme durumu denetleyin - okuma VM      | Microsoft.Compute/virtualMachines/read         | Sanal Makine         |
|Ekleme durumu denetleyin - okuma hesabı      | Microsoft.Automation/automationAccounts/read  |  Otomasyon hesabı   |
| VM ekleme çalışma denetle<sup>1</sup>       | Microsoft.OperationalInsights/workspaces/read         | Abonelik         |

<sup>1</sup> VM portal deneyimiyle eklemek için bu izni gereklidir.

### <a name="onboarding-from-automation-account"></a>Otomasyon hesabından ekleme

|**Eylem**  |**İzni** |**En düşük kapsamı**  |
|---------|---------|---------|
|Yeni bir dağıtımını oluşturun     | Microsoft.Resources/deployments/*        | Abonelik         |
|Yeni kaynak grubu oluştur     | Microsoft.Resources/subscriptions/resourceGroups/write         | Abonelik        |
|AutomationOnboarding dikey penceresi - yeni çalışma alanı oluşturma     |Microsoft.OperationalInsights/workspaces/write           | Kaynak grubu        |
|Bağlanan çalışma alanı AutomationOnboarding dikey penceresi - okuma     | Microsoft.Automation/automationAccounts/read        | Otomasyon hesabı       |
|AutomationOnboarding dikey penceresi - çözüm okuyun     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Çözüm        |
|AutomationOnboarding dikey penceresi - çalışma alanını okuma     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Çalışma alanı        |
|Çalışma alanı ve hesabı için bağlantı oluşturma     | Microsoft.OperationalInsights/workspaces/write        | Çalışma alanı        |
|Hesap ayakkabı için yazma      | Microsoft.Automation/automationAccounts/write        | Hesap        |
|Çözüm oluştur      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Kaynak Grubu         |
|Oluşturma/kayıtlı aramayı düzenleyin     | Microsoft.OperationalInsights/workspaces/write        | Çalışma alanı        |
|Kapsam yapılandırması Oluştur/Düzenle     | Microsoft.OperationalInsights/workspaces/write        | Çalışma alanı        |
|Kapsam yapılandırmasına bağlantı çözümü      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Çözüm         |
|**Adım 2 - yerleşik birden çok VM**     |         |         |
|VMOnboarding dikey penceresi - MMA oluşturma uzantısı     | Microsoft.Compute/virtualMachines/write           | Sanal Makine        |
|Oluşturma / kayıtlı aramayı düzenleyin     | Microsoft.OperationalInsights/workspaces/write           | Çalışma alanı        |
|Kapsam yapılandırmasını düzenleme / oluşturma  | Microsoft.OperationalInsights/workspaces/write   | Çalışma alanı|

## <a name="update-management"></a>Güncelleştirme yönetimi

Güncelleştirme yönetimi, hizmet sağlamak için çok hizmette ulaşır. Aşağıdaki tabloda, güncelleştirme yönetimi dağıtımlarını yönetmek için gereken izinleri gösterilmektedir:

|**Kaynak**  |**Rol**  |**Kapsam**  |
|---------|---------|---------|
|Otomasyon hesabı     | Log Analytics Katkıda Bulunan       | Otomasyon hesabı        |
|Otomasyon hesabı    | Sanal Makine Katılımcısı        | Kaynak grubu hesabı        |
|Log Analytics çalışma alanı     | Log Analytics Katkıda Bulunan| Log Analytics çalışma alanı        |
|Log Analytics çalışma alanı |Log Analytics Okuyucusu| Abonelik|
|Çözüm     |Log Analytics Katkıda Bulunan         | Çözüm|
|Sanal Makine     | Sanal Makine Katılımcısı        | Sanal Makine        |

## <a name="configure-rbac-for-your-automation-account"></a>Automation hesabınız için RBAC yapılandırma

Aşağıdaki bölümde, Otomasyon hesabınızda RBAC yapılandırmak nasıl gösterir [portalı](#configure-rbac-using-the-azure-portal) ve [PowerShell](#configure-rbac-using-powershell)

### <a name="configure-rbac-using-the-azure-portal"></a>Azure portalını kullanarak RBAC yapılandırma

1. [Azure portalında](https://portal.azure.com/) oturum açın Otomasyon Hesapları sayfasından Otomasyon hesabınızı açın.
2. Tıklayarak **erişim denetimi (IAM)** sol üst köşedeki denetimi. Bu açılır **erişim denetimi (IAM)** burada yeni kullanıcıları, grupları ekleyebilir ve Otomasyon yönetmek üzere uygulamalar, hesap ve Otomasyon hesabı için yapılandırılabilen mevcut rolleri sayfası.
3. Tıklayın **rol atamaları** sekmesi.

   ![Erişim düğmesi](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Yeni kullanıcı ekleme ve rol atama

1. Gelen **erişim denetimi (IAM)** sayfasında **+ rol ataması Ekle** açmak için **rol ataması Ekle** burada kullanıcıya, gruba veya uygulamaya ekleyebilir ve rol atama sayfası bunları.

2. Kullanılabilir roller listesinden bir rol seçin. Herhangi bir Otomasyon hesabı desteklediği yerleşik roller kullanılabilir veya sizin tanımladığınız herhangi bir özel rol seçebilirsiniz.

3. İçinde izin vermek istediğiniz kullanıcının kullanıcı adı yazın **seçin** alan. Listeden kullanıcı seçin ve tıklayın **Kaydet**.

   ![Kullanıcı ekle](media/automation-role-based-access-control/automation-04-add-users.png)

   Eklenen kullanıcı görürsünüz **kullanıcılar** sayfası ile atanan seçili rolü

   ![Kullanıcıları listele](media/automation-role-based-access-control/automation-05-list-users.png)

   Kullanıcıya **Roller** sayfasından rol atayabilirsiniz.
4. Tıklayın **rolleri** gelen **erişim denetimi (IAM)** sayfasını açmak için **rolleri** sayfası. Bu sayfadan rolün adını, bu role atanan kullanıcıların ve grupların sayısını görebilirsiniz.

    ![Kullanıcılar sayfasından rol atama](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Rol tabanlı erişim denetimi yalnızca Automation hesabı kapsamındaki ve Automation hesabı altındaki kaynaklarda ayarlayabilirsiniz.

#### <a name="remove-a-user"></a>Kullanıcıyı kaldırma

Kimin Otomasyon hesabını yönetiyor değil veya kimin artık kuruluş için çalışır bir kullanıcının erişim iznini kaldırabilirsiniz. Aşağıdaki adımları kullanarak kullanıcı kaldırabilirsiniz:

1. Gelen **erişim denetimi (IAM)** sayfasında, kaldırmak ve kullanıcı istek **Kaldır**.
2. Atama ayrıntıları sayfasında **Kaldır** düğmesine tıklayın.
3. Kaldırmayı onaylamak için **Evet**'e tıklayın.

   ![Kullanıcıları kaldır](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>PowerShell ile RBAC yapılandırma

Rol tabanlı erişim aşağıdakileri kullanarak bir Otomasyon hesabı da yapılandırılabilir [Azure PowerShell cmdlet'lerini](../role-based-access-control/role-assignments-powershell.md):

[Get-AzureRmRoleDefinition](/previous-versions/azure/mt603792(v=azure.100)) Azure Active Directory'de kullanılabilen tüm RBAC rollerini listeler. Belirli bir rol tarafından gerçekleştirilebilen tüm eylemleri listelemek için bu komutu **Ad** özelliğiyle birlikte bu komutu kullanabilirsiniz.

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
```

Örnek çıktı aşağıda verilmiştir:

```azurepowershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action,
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
```

[Get-AzureRmRoleAssignment](/previous-versions/azure/mt619413(v=azure.100)) Azure AD RBAC rolü atamalarını belirtilen kapsamda listeler. Hiçbir parametre olmadan, bu komut abonelik altında yapılan tüm rol atamalarını döndürür. Belirli kullanıcıların yanı sıra bu kullanıcıların üyesi olduğu gruplara da erişim atamalarını listelemek için **ExpandPrincipalGroups** parametresini kullanın.
    **Örnek:** Tüm kullanıcıları ve rolleri bir Otomasyon hesabı içinde listelemek için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Örnek çıktı aşağıda verilmiştir:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

[Yeni-AzureRmRoleAssignment](/previous-versions/azure/mt603580(v=azure.100)) kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda erişim atamak için.
    **Örnek:** Otomasyon hesabı kapsamındaki bir kullanıcı için "Otomasyon operatörü" rolünü atamak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Örnek çıktı aşağıda verilmiştir:

```azurepowershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

Kullanım [Remove-AzureRmRoleAssignment](/previous-versions/azure/mt603781(v=azure.100)) belirli bir kapsamda belirtilen kullanıcıya, Grup veya uygulamaya erişimini kaldırmak için.
    **Örnek:** Kullanıcıyı Otomasyon hesabı kapsamındaki "Otomasyon operatörü" rolünden kaldırmak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Yukarıdaki örneklerde değiştirin **oturum açma kimliği**, **abonelik kimliği**, **kaynak grubu adı**, ve **Otomasyon hesabı adı** ile Hesap ayrıntıları. Kullanıcı rolü atamasını kaldırmak için devam etmeden önce onaylamanız istendiğin **Evet**’i seçin.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Automation operatörü rolü - Otomasyon hesabı için kullanıcı deneyimi

Bir kullanıcı, Otomasyon hesabı kapsamında Otomasyon operatörü rolüne atanan kişi için atandıkları Otomasyon hesabı görünümleri, yalnızca runbook'ları, runbook işlerinin listesini görüntüleyebilir ve Automation'da oluşturulan zamanlamalar hesap ancak görüntüleyemezsiniz kendi tanımı. Runbook işini başlatabilir, durdurabilir, askıya alabilir, sürdürebilir veya zamanlayabilirler. Kullanıcının yapılandırmalar, karma çalışan grupları veya DSC düğümleri gibi diğer Otomasyon kaynaklarına erişimi yok.

![Kaynaklara erişim yok](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>Runbook'ları için RBAC yapılandırma

Azure Otomasyonu, RBAC için belirli runbook'lara atamanızı sağlar. Bunu yapmak için bir kullanıcı belirli bir runbook eklemek için aşağıdaki betiği çalıştırın. Aşağıdaki komut dosyasını olması çalıştıran bir Otomasyon hesabı yönetici veya Kiracı Yöneticisi tarafından

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation Account
$automationAccountName ="<Automation Account Name>" # Name of the Automation Account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation Account resource
$aa = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$automationAccountName/$rbName"

# The Automation Job Operator role only needs to be ran once per user.
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

Bir kez çalıştırdığınız, Görünüm ve Azure portalında oturum açın kullanıcının **tüm kaynakları**. Listede olarak eklendikleri Runbook göreceği bir **Otomasyon Runbook'u işleci** için.

![Portalda Runbook RBAC](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Automation operatörü rolü - Runbook için kullanıcı deneyimi

Runbook görünümlerin kapsamını belirlemek için atanmış bir Runbook Otomasyon operatörü rolüne atanan kullanıcı, bunlar yalnızca runbook'u başlatmak ve runbook işleri görüntüleyin.

![Yalnızca başlatmak için erişime sahip](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu’nda RBAC yapılandırmak için çeşitli yollar hakkında daha fazla bilgi için bkz. [Azure PowerShell ile RBAC yönetme](../role-based-access-control/role-assignments-powershell.md).
* Runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz. [runbook başlatma](automation-starting-a-runbook.md)
* Farklı runbook türleri hakkında daha fazla bilgi için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md)

