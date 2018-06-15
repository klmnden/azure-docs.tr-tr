---
title: Azure Automation’da Rol Tabanlı Erişim Denetimi
description: Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Bu makalede, Azure Automation’da RBAC’nin nasıl ayarlanacağı açıklanmaktadır.
keywords: otomasyon rbac, rol tabanlı erişim denetimi, azure rbac
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fd96a6cfebe44bd02e3f44a44d91119ad1c2c5a9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598761"
---
# <a name="role-based-access-control-in-azure-automation"></a>Azure Automation’da Rol Tabanlı Erişim Denetimi

Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için erişim yönetimi sağlar. Kullanarak [RBAC](../role-based-access-control/overview.md), ekibiniz içinde görevleri kurabilmeleri ve kullanıcıları, grupları sadece erişim miktarını vermek ve işlerini gerçekleştirmek için gereksinim duydukları uygulamaları. Kullanıcılara rol tabanlı erişim Azure portalı, Azure Komut Satırı araçları ve Azure Management API'leri kullanılarak verilebilir.

## <a name="roles-in-automation-accounts"></a>Automation hesapları rollerinde

Azure Automation’da, otomasyon hesabı kapsamında kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atanarak erişim verilir. Aşağıda Automation hesabının desteklediği yerleşik roller bulunmaktadır:

| **Rol** | **Açıklama** |
|:--- |:--- |
| Sahip |Sahip rolü tüm kaynaklara ve diğer kullanıcılar, gruplar ve Automation hesabını yönetmek üzere uygulamalar için erişim sağlama dahil olmak üzere bir Otomasyon hesabı içinde eylemler erişmesini sağlar. |
| Katılımcı |Katılımcı rolü, başka kullanıcının Otomasyon hesabına erişim izinlerini değiştirme dışında her şeyi yönetmenizi sağlar. |
| Okuyucu |Okuyucu rolü, Otomasyon hesabında tüm kaynakları görmenizi sağlar; ancak değişiklik yapamazsınız. |
| Otomasyon Operatörü |Automation operatörü rolü, runbook adı ve özelliklerini görüntülemek ve oluşturmak ve bir Otomasyon hesabı içinde tüm runbook'lar için iş yönetmenize olanak sağlar. Bu rol, kimlik bilgileri varlıkları ve runbook'ları gibi Automation hesabı kaynaklarınızın görüntülenmesini veya değiştirilmesini engellemek, ancak yine de kuruluş üyelerinin bu runbook’ları yürütmesine izin vermek istiyorsanız yararlıdır. |
|Otomasyon İşi İşleci|Otomasyon iş işleci rolü, Automation hesabı tüm runbook'lar için işleri oluşturmak ve yönetmek sağlar.|
|Otomasyon Runbook'u İşleci|Otomasyon Runbook işletmeni rolü, bir runbook'un adını ve özelliklerini görüntülemenize olanak sağlar.|
| Log Analytics Katkıda Bulunan | Günlük analizi katılımcı rolü tüm izleme verilerini okuma ve izleme ayarları düzenlemenize olanak sağlar. İzleme ayarları düzenleme, oluşturma ve Automation hesapları yapılandırma, çözümleri ekleme ve Azure tanılama yapılandırma Azure depolama günlüklerinden koleksiyonu yapılandırmak için depolama hesabı anahtarlarını okuma VM'ler için VM uzantısı eklemeyi içerir Tüm Azure kaynakları.|
| Log Analytics Okuyucusu | Günlük analizi okuyucu rolü, görüntüleme ve tüm izleme verilerini yanı sıra izleme ayarlarını görünümü arama sağlar. Bu, tüm Azure kaynakları üzerinde Azure tanılama yapılandırması görüntüleme içerir. |
| İzleme Katkıda Bulunanı | İzleme katılımcı rolü, tüm izleme verileri ve izleme ayarlarını güncelleştirme okumanızı sağlar.|
| İzleme Okuyucusu | İzleme okuyucu rolüne tüm izleme verilerini okumasına izin verir. |
| Kullanıcı Erişimi Yöneticisi |Kullanıcı Erişimi Yöneticisi rolü, Azure Otomasyonu hesaplarına kullanıcı erişimini yönetmenizi sağlar. |

## <a name="role-permissions"></a>Rol izinleri

Aşağıdaki tablolarda her rol için verilen özel izinler açıklanmaktadır. Bu izinleri verin, Eylemler ve bunların kısıtlamak NotActions içerebilir.

### <a name="owner"></a>Sahip

Bir sahibi erişim dahil her şeyi yönetebilir. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|Eylemler|Açıklama|
|---|---|
|Microsoft.Automation/automationAccounts/|Oluşturun ve tüm türlerinin kaynakları yönetin.|

### <a name="contributor"></a>Katılımcı

Katılımcı erişim dışında her şeyi yönetebilir. Aşağıdaki tabloda verilen ve rol için reddedildi izinleri gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Oluşturma ve tüm türlerinin kaynakları yönetme|
|**Değil Eylemler**||
|Microsoft.Authorization/*/Delete| Rolleri ve rol atamalarını silin.       |
|Microsoft.Authorization/*/Write     |  Rolleri ve rol atamalarını oluşturun.       |
|Microsoft.Authorization/elevateAccess/Action    | Kullanıcı erişimi Yöneticisi oluşturma olanağı reddeder.       |

### <a name="reader"></a>Okuyucu

Bir okuyucu Automation hesabında tüm kaynakları görüntüleyebilir ancak değişiklik yapamazsınız.

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Automation hesabında tüm kaynakları görüntüleyin. |

### <a name="automation-operator"></a>Otomasyon Operatörü

Automation operatörü oluşturabilir ve işlerini yönetme ve runbook adları ve tüm runbook'lar bir Otomasyon hesabı için özellikler okuyun.  Not: tek tek işleci erişimi denetlemek istiyorsanız, runbook'ları sonra yok Bu rolü ayarlayın ve 'Otomasyon iş işleci' ve 'Otomasyon Runbook işletmeni' rolleri birlikte kullanın. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Authorization/*/read|Yetkilendirme okuyun.|
|Microsoft.Automation/automationAccounts/jobs/read|Runbook işlerini listeleyin.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Duraklatılmış bir işini devam ettirir.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Devam eden işi iptal edin.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|İş akışları ve çıktı okuyun.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Devam eden bir iş duraklatılamadı.|
|Microsoft.Automation/automationAccounts/jobs/write|İşlerini oluşturun.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rolleri ve rol atamalarını okuyun.         |
|Microsoft.Resources/deployments/*      |Oluşturun ve kaynak grubu dağıtımı yönetin.         |
|Microsoft.Insights/alertRules/*      | Oluşturun ve uyarı kuralları yönetin.        |
|Microsoft.Support/* |Oluşturun ve Destek biletlerini yönetme.|

### <a name="automation-job-operator"></a>Otomasyon İşi İşleci

Bir Otomasyon iş operatörü rolü Automation hesabı kapsamda verilir. Bu hesaptaki tüm runbook'lar işleri oluşturmak ve yönetmek operatör izinleri sağlar. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Authorization/*/read|Yetkilendirme okuyun.|
|Microsoft.Automation/automationAccounts/jobs/read|Runbook işlerini listeleyin.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Duraklatılmış bir işini devam ettirir.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Devam eden işi iptal edin.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|İş akışları ve çıktı okuyun.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Devam eden bir iş duraklatılamadı.|
|Microsoft.Automation/automationAccounts/jobs/write|İşlerini oluşturun.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Rolleri ve rol atamalarını okuyun.       |
|Microsoft.Resources/deployments/*      |Oluşturun ve kaynak grubu dağıtımı yönetin.         |
|Microsoft.Insights/alertRules/*      | Oluşturun ve uyarı kuralları yönetin.        |
|Microsoft.Support/* |Oluşturun ve Destek biletlerini yönetme.|

### <a name="automation-runbook-operator"></a>Otomasyon Runbook'u İşleci

Otomasyon Runbook işletmeni rol Runbook kapsamda verilir. Bir Otomasyon Runbook işleç runbook'un adını ve özelliklerini görüntüleyebilirsiniz.  'Otomasyon iş işleci' rolüyle birlikte bu rolü de oluşturmak ve runbook işleri yönetmek işleci sağlar. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Runbook'ları listeler.        |
|Microsoft.Authorization/*/read      | Yetkilendirme okuyun.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rolleri ve rol atamalarını okuyun.         |
|Microsoft.Resources/deployments/*      | Oluşturun ve kaynak grubu dağıtımı yönetin.         |
|Microsoft.Insights/alertRules/*      | Oluşturun ve uyarı kuralları yönetin.        |
|Microsoft.Support/*      | Oluşturun ve Destek biletlerini yönetme.        |

### <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunan

Günlük analizi katkıda bulunan tüm izleme verilerini okuma ve izleme ayarlarını düzenleyin. İzleme ayarları düzenleme, VM'ler için VM uzantısı eklemeyi içerir; Azure Storage günlüklerinden koleksiyonu yapılandırmak için depolama hesabı anahtarlarını okuma; oluşturma ve Automation hesapları yapılandırma; çözümleri ekleme; ve tüm Azure kaynaklara Azure tanılama yapılandırılıyor. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dışındaki tüm türlerinin kaynakları okuyun.|
|Microsoft.Automation/automationAccounts/*|Automation hesapları yönetin.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Oluşturun ve sanal makine uzantıları yönetin.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Klasik depolama hesabı anahtarlarını listele.|
|Microsoft.Compute/virtualMachines/extensions/*|Oluşturun ve klasik sanal makine uzantıları yönetin.|
|Microsoft.Insights/alertRules/*|Okuma/yazma/silme uyarı kuralları.|
|Microsoft.Insights/diagnosticSettings/*|Okuma/yazma/silme tanılama ayarları.|
|Microsoft.OperationalInsights/*|Günlük analizi yönetin.|
|Microsoft.OperationsManagement/*|Çalışma alanları çözümlerinde yönetin.|
|Microsoft.Resources/deployments/*|Oluşturun ve kaynak grubu dağıtımı yönetin.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Oluşturun ve kaynak grubu dağıtımı yönetin.|
|Microsoft.Storage/storageAccounts/listKeys/action|Depolama hesabı anahtarlarını listele.|
|Microsoft.Support/*|Oluşturun ve Destek biletlerini yönetme.|

### <a name="log-analytics-reader"></a>Log Analytics Okuyucusu

Günlük analizi Okuyucu, görüntüleyebilir ve tüm izleme verilerini izleme ayarları üzerinde tüm Azure kaynakları Azure tanılama yapılandırması görüntüleme dahil olmak üzere, Görünüm ve yanı arayabilirsiniz. Aşağıdaki tabloda verilen veya reddedilen rolü için izinleri göstermektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dışındaki tüm türlerinin kaynakları okuyun.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Günlük analizi sorgularda yönetin.|
|Microsoft.OperationalInsights/workspaces/search/action|Günlük analizi veri arayın.|
|Microsoft.Support/*|Oluşturun ve Destek biletlerini yönetme.|
|**Değil Eylemler**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Paylaşılan erişim anahtarları okumak erişilemiyor.|

### <a name="monitoring-contributor"></a>İzleme Katkıda Bulunanı

İzleme katkıda bulunan tüm izleme verilerini okuma ve izleme ayarlarını güncelleştirin. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dışındaki tüm türlerinin kaynakları okuyun.|
|Microsoft.AlertsManagement/alerts/*|Uyarıları yönetme.|
|Microsoft.AlertsManagement/alertsSummary/*|Uyarı Pano yönetin.|
|Microsoft.Insights/AlertRules/*|Uyarı kurallarını yönet.|
|Microsoft.Insights/components/*|Application Insights bileşenlerini yönetin.|
|Microsoft.Insights/DiagnosticSettings/*|Tanılama ayarlarını yönetin.|
|Microsoft.Insights/eventtypes/*|Bir abonelikte etkinlik günlüğü olayları (Yönetim olayları) listeler. Bu izin, etkinlik günlüğü programlı ve portal erişimi için geçerlidir.|
|Microsoft.Insights/LogDefinitions/*|Bu izin, etkinlik günlükleri için portal aracılığıyla erişmek isteyen kullanıcılar için gereklidir. Etkinlik günlüğü günlük kategorilerini liste.|
|Microsoft.Insights/MetricDefinitions/*|Ölçüm tanımlarını (bir kaynak için kullanılabilir ölçüm türlerinin listesi) okuyun.|
|Microsoft.Insights/Metrics/*|Bir kaynak için ölçümleri okuyun.|
|Microsoft.Insights/Register/Action|Microsoft.ınsights sağlayıcıyı kaydedin.|
|Microsoft.Insights/webtests/*|Application Insights web testleri yönetin.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Günlük analizi çözüm paketlerini yönetin.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Günlük analizi kayıtlı aramaları yönetin.|
|Microsoft.OperationalInsights/workspaces/search/action|Günlük analizi çalışma alanları arayın.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Günlük analizi çalışma alanı için anahtarları listeler.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Günlük analizi depolama Insight yapılandırmalarını yönetme.|
|Microsoft.Support/*|Oluşturun ve Destek biletlerini yönetme.|
|Microsoft.WorkloadMonitor/workloads/*|İş yükleri yönetin.|

### <a name="monitoring-reader"></a>İzleme Okuyucusu

Bir izleme Okuyucusu tüm izleme verileri okuyabilir. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Gizli dışındaki tüm türlerinin kaynakları okuyun.|
|Microsoft.OperationalInsights/workspaces/search/action|Günlük analizi çalışma alanları arayın.|
|Microsoft.Support/*|Oluşturma ve Destek biletlerini yönetme|

### <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi

Kullanıcı erişimi Yöneticisi Azure kaynaklarına kullanıcı erişimi yönetebilirsiniz. Aşağıdaki tabloda, rol için izinler gösterilmektedir:

|**Eylemler**  |**Açıklama**  |
|---------|---------|
|* / Okuma|Tüm kaynaklara okuma|
|Microsoft.Authorization/*|Yetkilendirme yönetme|
|Microsoft.Support/*|Oluşturma ve Destek biletlerini yönetme|

## <a name="onboarding"></a>Ekleme

Aşağıdaki tablolarda değişiklik izleme ekleme sanal makineler için gereken en düşük gerekli izinleri göster veya yönetim çözümleri güncelleştirin.

### <a name="onboarding-from-a-virtual-machine"></a>Bir sanal makineden ekleme

|**Eylem**  |**İzni**  |**En küçük kapsam**  |
|---------|---------|---------|
|Yeni dağıtım yazma      | Microsoft.Resources/deployments/*          |Abonelik          |
|Yeni kaynak grubu yazma      | Microsoft.Resources/subscriptions/resourceGroups/write        | Abonelik          |
|Yeni varsayılan çalışma alanı oluşturma      | Microsoft.OperationalInsights/workspaces/write         | Kaynak grubu         |
|Yeni hesap oluştur      |  Microsoft.Automation/automationAccounts/write        |Kaynak grubu         |
|Bağlantı çalışma ve hesabı      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Çalışma alanı</br>Otomasyon hesabı
|Çözüm oluştur      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Kaynak grubu          |
|MMA uzantısı oluşturma      | Microsoft.Compute/virtualMachines/write         | Sanal Makine         |
|Kaydedilen arama oluşturma      | Microsoft.OperationalInsights/workspaces/write          | Çalışma alanı         |
|Kapsam yapılandırma oluşturma      | Microsoft.OperationalInsights/workspaces/write          | Çalışma alanı         |
|Kapsam yapılandırma bağlantı çözümü      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Çözüm         |
|Ekleme durumu denetleme - okunur çalışma      | Microsoft.OperationalInsights/workspaces/read         | Çalışma alanı         |
|Ekleme durumu denetimi - okunur bağlı hesabının çalışma özelliği     | Microsoft.Automation/automationAccounts/read      | Otomasyon hesabı        |
|Ekleme durumu denetleme - okunur çözümü      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Çözüm         |
|Ekleme durumu denetimi - okunur VM      | Microsoft.Compute/virtualMachines/read         | Sanal Makine         |
|Ekleme durumu denetleme - okuma hesabı      | Microsoft.Automation/automationAccounts/read  |  Otomasyon hesabı   |

### <a name="onboarding-from-automation-account"></a>Automation hesabı ekleme

|**Eylem**  |**İzni** |**En küçük kapsam**  |
|---------|---------|---------|
|Yeni bir dağıtımını oluşturun     | Microsoft.Resources/deployments/*        | Abonelik         |
|Yeni kaynak grubu oluştur     | Microsoft.Resources/subscriptions/resourceGroups/write         | Abonelik        |
|AutomationOnboarding dikey penceresi - yeni çalışma alanı oluştur     |Microsoft.OperationalInsights/workspaces/write           | Kaynak grubu        |
|AutomationOnboarding dikey penceresi - bağlantılı çalışma okuma     | Microsoft.Automation/automationAccounts/read        | Otomasyon hesabı       |
|AutomationOnboarding dikey penceresi - çözüm okuma     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Çözüm        |
|AutomationOnboarding dikey penceresi - çalışma okuma     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Çalışma alanı        |
|Bağlantı çalışma ve hesap oluşturma     | Microsoft.OperationalInsights/workspaces/write        | Çalışma alanı        |
|Hesap shoebox için yazma      | Microsoft.Automation/automationAccounts/write        | Hesap        |
|Çözüm oluştur      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Kaynak Grubu         |
|Oluşturma/kaydedilen Düzenle arama     | Microsoft.OperationalInsights/workspaces/write        | Çalışma alanı        |
|Kapsam yapılandırma Oluştur/Düzenle     | Microsoft.OperationalInsights/workspaces/write        | Çalışma alanı        |
|Kapsam yapılandırma bağlantı çözümü      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Çözüm         |
|**Adım 2 - yerleşik birden çok VM**     |         |         |
|VMOnboarding dikey penceresi - MMA oluşturma uzantısı     | Microsoft.Compute/virtualMachines/write           | Sanal Makine        |
|Oluştur / Düzenle kaydedilmiş arama     | Microsoft.OperationalInsights/workspaces/write           | Çalışma alanı        |
|Oluştur / kapsam yapılandırma Düzenle  | Microsoft.OperationalInsights/workspaces/write   | Çalışma alanı|

## <a name="update-management"></a>Güncelleştirme yönetimi

Güncelleştirme yönetimi, hizmet sağlamak için birden fazla hizmet ulaşır. Aşağıdaki tabloda, güncelleştirme yönetimi dağıtımları yönetmek için gereken izinler gösterilmektedir:

|**Kaynak**  |**Rol**  |**Kapsam**  |
|---------|---------|---------|
|Otomasyon hesabı     | Log Analytics Katkıda Bulunan       | Otomasyon hesabı        |
|Otomasyon hesabı    | Sanal Makine Katılımcısı        | Kaynak grubu hesabı        |
|Log Analytics çalışma alanı     | Log Analytics Katkıda Bulunan| Log Analytics çalışma alanı        |
|Log Analytics çalışma alanı |Log Analytics Okuyucusu| Abonelik|
|Çözüm     |Log Analytics Katkıda Bulunan         | Çözüm|
|Sanal Makine     | Sanal Makine Katılımcısı        | Sanal Makine        |

## <a name="configure-rbac-for-your-automation-account"></a>Automation hesabınız için RBAC yapılandırma

Aşağıdaki bölümde Otomasyon hesabınızda RBAC yapılandırmak gösterilmiştir [portal](#configure-rbac-using-the-azure-portal) ve [PowerShell](#configure-rbac-using-powershell)

### <a name="configure-rbac-using-the-azure-portal"></a>Azure portalını kullanarak RBAC yapılandırma

1. [Azure portalında](https://portal.azure.com/) oturum açın Otomasyon Hesapları sayfasından Otomasyon hesabınızı açın.
2. Tıklayın **erişim denetimi (IAM)** sol üst köşede denetimi. Bu açılır **erişim denetimi (IAM)** burada yeni kullanıcıları, grupları ekleyebilir, Automation'ınızı yönetmek üzere uygulamalar hesap ve Otomasyon hesabı için yapılandırılabilen mevcut rolleri görüntüleyebildiğiniz sayfa.

   ![Erişim düğmesi](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Yeni kullanıcı ekleme ve rol atama

1. Gelen **erişim denetimi (IAM)** sayfasında, **+ Ekle** açmak için **izinleri eklemek** sayfa olduğu bir kullanıcı, Grup veya uygulama ekleyebileceğiniz ve rol atayabilirsiniz.

2. Kullanılabilir roller listesinden bir rol seçin. Herhangi bir Otomasyon hesabı destekleyen herhangi bir rolü veya tanımlamış olabileceğiniz herhangi bir özel rolü seçebilirsiniz.

3. İçinde izin vermek istediğiniz kullanıcının kullanıcı adı yazın **seçin** alan. Listeden kullanıcıyı seçin ve'ı tıklatın **kaydetmek**.

   ![Kullanıcı ekle](media/automation-role-based-access-control/automation-04-add-users.png)

   Eklenen kullanıcı görmelisiniz artık **kullanıcılar** atanan seçili rolü sayfası

   ![Kullanıcıları listele](media/automation-role-based-access-control/automation-05-list-users.png)

   Kullanıcıya **Roller** sayfasından rol atayabilirsiniz.
4. Tıklatın **rolleri** gelen **erişim denetimi (IAM)** sayfasını açmak için **rolleri** sayfası. Bu sayfadan rolün adını, bu role atanan kullanıcıların ve grupların sayısını görebilirsiniz.

    ![Kullanıcılar sayfasından rol atama](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Rol tabanlı erişim denetimi yalnızca Automation hesabı kapsamındaki ve Automation hesabı altındaki kaynaklarda ayarlayabilirsiniz.

#### <a name="remove-a-user"></a>Kullanıcıyı kaldırma

Kimin Automation hesabını yönetiyor değil ya da kimin artık kuruluş için çalışmayan bir kullanıcı için erişim izni kaldırabilirsiniz. Aşağıdaki adımları kullanarak kullanıcı kaldırabilirsiniz:

1. Gelen **erişim denetimi (IAM)** sayfasında, kaldırmak ve kullanıcı istek seçin **kaldırmak**.
2. Atama ayrıntıları sayfasında **Kaldır** düğmesine tıklayın.
3. Kaldırmayı onaylamak için **Evet**'e tıklayın.

   ![Kullanıcıları kaldır](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>PowerShell kullanarak RBAC yapılandırma

Rol tabanlı erişim aşağıdaki kullanarak bir Otomasyon hesabı için de yapılandırılabilir [Azure PowerShell cmdlet'lerini](../role-based-access-control/role-assignments-powershell.md):

[Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) Azure Active Directory'de kullanılabilen tüm RBAC rolleri listeler. Belirli bir rol tarafından gerçekleştirilebilen tüm eylemleri listelemek için bu komutu **Ad** özelliğiyle birlikte bu komutu kullanabilirsiniz.

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
```

Örnek çıktı verilmiştir:

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

[Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) Azure AD RBAC rolü atamalarını belirtilen kapsamda listeler. Hiçbir parametre olmadan, bu komut abonelik altında yapılan tüm rol atamalarını döndürür. Belirli kullanıcıların yanı sıra bu kullanıcıların üyesi olduğu gruplara da erişim atamalarını listelemek için **ExpandPrincipalGroups** parametresini kullanın.
    **Örnek:** Otomasyon hesabı içinde tüm kullanıcıları ve rollerini listelemek için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Örnek çıktı verilmiştir:

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

[Yeni-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) kullanıcılar, gruplar ve uygulamalar belirli bir kapsam için erişim atamak için.
    **Örnek:** "Automation operatör" rolü Automation hesabı kapsamındaki bir kullanıcı için atamak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Örnek çıktı verilmiştir:

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

Kullanım [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) belirli kapsamdan belirtilen kullanıcı, Grup veya uygulama erişimi kaldırmak için.
    **Örnek:** kullanıcı Otomasyon hesabı kapsamında "Automation operatör" rolü kaldırmak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Yukarıdaki örneklerde Değiştir **kimliğinde oturum**, **abonelik kimliği**, **kaynak grubu adı**, ve **Automation hesabı adını** ile Hesap ayrıntıları. Kullanıcı rolü atamasını kaldırmak için devam etmeden önce onaylamanız istendiğin **Evet**’i seçin.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Automation operatörü rolü - Otomasyon hesabı için kullanıcı deneyimi

Bir kullanıcı, kimin Otomasyon hesabı kapsamında Automation operatörü rolü atanmış oldukları atanır Otomasyon hesabı görünümleri, yalnızca runbook'ları, runbook işlerinin listesini görüntüleyebilirsiniz ve Otomasyon oluşturulan zamanlamalar hesap ancak görüntüleyemez kendi tanımı. Runbook işini başlatabilir, durdurabilir, askıya alabilir, sürdürebilir veya zamanlayabilirler. Kullanıcının yapılandırmalar, karma çalışan grupları veya DSC düğümleri gibi diğer Automation kaynaklarına erişimi yok.

![Kaynaklara erişim yok](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>Runbook'lar için RBAC yapılandırma

Azure otomasyonu için belirli runbook'lara RBAC atamanızı sağlar. Bunu yapmak için belirli bir runbook bir kullanıcı eklemek için aşağıdaki betiği çalıştırın. Aşağıdaki komut dosyasını olması çalıştıran bir Otomasyon hesabı yönetici veya Kiracı Yöneticisi tarafından

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

Bir kez çalıştırılmadan görebilir ve Azure portalında oturum kullanıcı **tüm kaynakları**. Listede gördükleri olarak eklendikleri Runbook bir **Otomasyon Runbook işletmeni** için.

![Portalda Runbook RBAC](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Kullanıcı deneyimi Automation operatörü rolü - Runbook

Runbook kapsam görünümleri için atanan bir Runbook'un Automation operatörü rolüne atanmış bir kullanıcı, bunlar yalnızca runbook'u başlatmak ve runbook işleri görüntüleyin.

![Erişimi yalnızca Başlat](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu’nda RBAC yapılandırmak için çeşitli yollar hakkında daha fazla bilgi için bkz. [Azure PowerShell ile RBAC yönetme](../role-based-access-control/role-assignments-powershell.md).
* Runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz. [runbook başlatma](automation-starting-a-runbook.md)
* Farklı runbook türleri hakkında daha fazla bilgi için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md)
