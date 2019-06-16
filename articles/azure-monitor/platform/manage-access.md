---
title: Azure İzleyici'de log Analytics çalışma alanlarını yönetme | Microsoft Docs
description: Azure İzleyici'nın kullanıcılar, hesaplar, çalışma alanları ve Azure hesapları çeşitli yönetim görevlerini kullanarak Log Analytics çalışma alanlarını yönetebilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: magoedte
ms.openlocfilehash: 27db27d79a05f24461e63242c0395cfd81315432
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60782811"
---
# <a name="manage-log-data-and-workspaces-in-azure-monitor"></a>Günlük verilerini ve Azure İzleyici'de çalışma alanlarını yönetme
Azure İzleyici depoları, temelde verileri ve yapılandırma bilgilerini içeren bir kapsayıcı ve Log Analytics çalışma alanında verilerini günlüğe kaydedebilirsiniz. Verileri günlüğe kaydetmek için erişimi yönetmek için çalışma alanları ile ilgili çeşitli yönetim görevlerini gerçekleştirin. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Bu makalede, günlükleri erişimi yönetmek için ve bunları içeren çalışma alanlarını yönetmek için nasıl açıklar. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Bir Log Analytics çalışma alanı oluşturmak için şunları yapmanız:

1. Bir Azure aboneliğine sahip olmanız.
2. Bir çalışma alanı adı seçmeniz.
3. Çalışma alanı biri Abonelikleriniz ve kaynak grubu ile ilişkilendirin.
4. Coğrafi bir konum seçmeniz.

Bir çalışma alanı oluşturma hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure portalında Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md)
- [Azure CLI 2.0 ile Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace-cli.md)
- [Azure PowerShell ile bir Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace-posh.md)

## <a name="determine-the-number-of-workspaces-you-need"></a>İhtiyacınız olan çalışma alanı sayısını belirleme
Bir Log Analytics çalışma alanı, bir Azure kaynağıdır ve verilerin toplanan, toplu, analiz ve Azure İzleyici'de sunulan bir kapsayıcıdır. Azure aboneliği başına birden çok çalışma alanına sahip olabilir ve bunların arasında kolayca sorgulama olanağı ile birden fazla çalışma alanına erişim sahibi olabilir. Bu bölümde birden çok çalışma alanı oluşturmanın yararlı olabileceği durumlar açıklanır.

Bir Log Analytics çalışma alanı sağlar:

* Veri depolama için coğrafi bir konum.
* Çalışma alanı merkezli modda farklı kullanıcı erişim haklarını tanımlamak için veri yalıtımı'nı tıklatın. Kaynak odaklı modunda çalışırken ilgili değildir.
* Kapsam ayarlarının yapılandırılması için ister [fiyatlandırma katmanı](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [bekletme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) ve [veri capping](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#daily-cap).
* Veri alımı ve bekletme ilgili ücretler çalışma alanı kaynak üzerinde gerçekleştirilir.

Tüketimi açısından, mümkün olduğunca az çalışma alanları oluşturma öneririz. Yönetim ve sorgu deneyimi daha kolay ve hızlı kolaylaştırır. Ancak, önceki özelliklere bağlı olarak, durumlarda birden çok çalışma alanı oluşturmak isteyebilirsiniz:

* Global bir şirketseniz ve veri bağımsızlığı veya uyumluluk nedenleriyle verilerin belirli bölgelerde depolanan oturum açmanız gerekir.
* Azure kullanıyorsanız ve çalışma alanını, yönettiği Azure kaynaklarıyla aynı bölgede bulundurarak giden veri aktarımı ücretlerini ortadan kaldırmak istiyorsanız.
* Yönetilen bir hizmet sağlayıcısıysanız ve yönettiğiniz her bir müşteriye ilişkin Log Analytics verilerini diğer müşterilerin verilerinden yalıtmak istiyorsanız.
* Birden çok müşteriyi yönetiyorsanız ve her müşteri istediğiniz / bölüm / iş grubunun kendi verilerini, ancak değil, diğerlerinin verileri görmek için ve birleştirilmiş bir çapraz müşteri için iş gerek yoktur / bölüm / iş grubunun görüntüle. ".

Verileri toplamak için Windows aracılarını kullanıyorsanız [her bir aracıyı, bir veya daha fazla çalışma alanına raporlama yapacak şekilde yapılandırabilirsiniz](../../azure-monitor/platform/agent-windows.md).

System Center Operations Manager'ı kullanıyorsanız her bir Operations Manager yönetim grubu yalnızca bir çalışma alanıyla bağlantılı olabilir. Operations Manager tarafından yönetilen bilgisayarlara Microsoft İzleme Aracısını yükleyebilir ve hem Operations Manager hem de farklı bir Log Analytics çalışma alanı için aracı raporu alabilirsiniz.

Çalışma alanı mimarisi tanımlandıktan sonra bu ilke Azure kaynaklarıyla üzerinde uygulamalıdır [Azure İlkesi](../../governance/policy/overview.md). Bu, tüm Azure kaynakları için otomatik olarak uygulanacak bir yerleşik tanımı sağlayabilir. Örneğin, belirli bir bölgede tüm Azure kaynakları için belirli bir çalışma alanı, tanılama günlükleri gönderilen emin olmak için bir ilke ayarlayabilirsiniz.

## <a name="view-workspace-details"></a>Çalışma alanı ayrıntılarını görüntüle
Log Analytics çalışma alanınızdan veri analiz ederken **Azure İzleyici** menüsünde Azure portalında, oluşturabilir ve çalışma alanlarını yönetebilirsiniz **Log Analytics çalışma alanları** menüsü.
 

1. Oturum [Azure portalında](https://portal.azure.com) tıklatıp **tüm hizmetleri**. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Log Analytics** çalışma alanları.  

    ![Azure portal](media/manage-access/azure-portal-01.png)  

3. Çalışma alanınızı listeden seçin.

4. Çalışma alanı sayfasında çalışma alanı, Başlarken, yapılandırma ve ek bilgi bağlantıları hakkında ayrıntıları görüntüler.  

    ![Çalışma alanı ayrıntıları](./media/manage-access/workspace-overview-page.png)  


## <a name="workspace-permissions-and-scope"></a>Çalışma alanı izinlerini ve kapsamı
Bir kullanıcının erişimi olan veri, aşağıdaki tabloda listelenen çoklu faktörlerle tarafından belirlenir. Her işlem, aşağıdaki bölümlerde açıklanmıştır.

| faktörü | Açıklama |
|:---|:---|
| [Erişim modu](#access-modes) | Kullanıcının kullandığı yöntem, çalışma alanına erişir.  Kullanılabilir verilerin kapsamını ve uygulanan erişim denetimi modu tanımlar. |
| [Erişim denetim modu](#access-control-mode) | İzinler çalışma alanı ya da kaynak düzeyinde uygulanır tanımlar için çalışma alanı ayarlama. |
| [İzinler](#manage-accounts-and-users) | İzinler, tek tek veya çalışma alanı veya kaynak için kullanıcı gruplarına uygulanır. Hangi verilerin kullanıcı erişimi olacaktır tanımlar. |
| [Tablo düzeyi RBAC](#table-level-rbac) | İsteğe bağlı ayrıntılı izinler, kullanıcıların erişim modu veya erişim denetim modu bağımsız olarak tüm kullanıcılara uygulanır. Bir kullanıcının hangi veri türlerini tanımlar. |



## <a name="access-modes"></a>Erişim modu
_Erişim modu_ bir kullanıcı bir Log Analytics çalışma alanı nasıl eriştiğini için ifade eder ve kullanıcıların verilere erişebileceği kapsamını tanımlar. 

**Çalışma alanı merkezli**: Bu modda, bir kullanıcı izinlerine sahip oldukları çalışma alanındaki tüm günlükleri görüntüleyebilirsiniz. Bu modda sorgular çalışma alanındaki tüm tablolardaki tüm verileri kapsayan. Bu günlükleri gibi seçtiğinizde, kapsam olarak çalışma alanı ile erişildiğinde kullanılan erişim moddur **günlükleri** gelen **Azure İzleyici** Azure portalındaki menü.

**Kaynak odaklı**: Çalışma alanı seçtiğinizde, gibi belirli bir kaynak için eriştiğinizde **günlükleri** Azure portalında bir kaynak menüsünden erişiminiz olan tüm tablolarda yalnızca o kaynak için günlükleri görüntüleyebilirsiniz. Bu modda sorgular yalnızca bu kaynakla ilişkilendirilmiş veri kapsamına eklenir. Bu mod ayrıca ayrıntılı rol tabanlı erişim denetimi (RBAC) sağlar. 

> [!NOTE]
> Yalnızca ilgili kaynak düzgün bir şekilde ilişkili günlükleri kaynak odaklı sorgular için kullanılabilir. Şu anda aşağıdaki kaynaklar sınırlamalara sahiptir: 
> - Azure dışındaki bilgisayarlar
> - Service Fabric
> - Application Insights
> - Kapsayıcılar
>
> Bir sorguyu çalıştırarak günlükleri, kaynak ile düzgün bir şekilde ilişkili ve kayıtları inceleyerek ilgilendiğiniz test edebilirsiniz. Doğru kaynak kimliği ise [_ResourceId](log-standard-properties.md#_resourceid) özelliği, ardından veri kaynağı merkezli sorgular için kullanılabilir.

### <a name="comparing-access-modes"></a>Erişim modu karşılaştırması

Erişim modu aşağıdaki tabloda özetlenmiştir:

| | Çalışma alanı merkezli | Kaynak odaklı |
|:---|:---|:---|
| Her model kimin için tasarlanmıştır? | Merkezi Yönetim. Çok çeşitli kaynaklara erişmesi gereken veri toplama ve kullanıcıları yapılandırmak için gereken yöneticileri. Ayrıca Azure dışındaki kaynaklar için günlüklerine erişmek için sahip kullanıcılar için şu anda gereklidir. | Uygulama ekipler. İzlenmekte olan Azure kaynak yöneticileri. |
| Günlükleri görüntülemek için ne bir kullanıcı gerektiriyor mu? | Çalışma alanına izinleri. Bkz: **çalışma alanı izinlerini** içinde [hesapları ve kullanıcıları yönetme](#manage-accounts-and-users). | Kaynak yönelik okuma erişimi. Bkz: **kaynağı izinlerini** içinde [hesapları ve kullanıcıları yönetme](#manage-accounts-and-users). İzinleri olabilir (örneğin içeren kaynak grubunu'ye kadar) devralınan veya doğrudan kaynağa atanmış. Günlükleri kaynak için izni otomatik olarak atanır. |
| İzinleri kapsamı nedir? | Çalışma alanı. Çalışma alanına erişimi olan kullanıcılar sahip oldukları izinleri, çalışma alanını tablodan tüm günlükleri sorgulayabilir. Bkz: [tablo erişim denetimi](#table-level-rbac) | Azure kaynak. Kullanıcı, günlükleri sorgulayabilir kaynaklar için bunların herhangi bir çalışma alanından erişiminiz ancak günlükler için diğer kaynaklar sorgulanamıyor. |
| Kullanıcı erişim günlükleri nasıl kullanabilir? | Başlangıç **günlükleri** gelen **Azure İzleyici** menüsü veya **Log Analytics çalışma alanları**. | Başlangıç **günlükleri** Azure kaynak menüsünden. |


## <a name="access-control-mode"></a>Erişim denetim modu
_Erişim denetim modu_ bu çalışma alanı için izinleri nasıl belirlendiğini tanımlayan bir ayar her çalışma alanı üzerinde.

**Çalışma alanı izinleri gerektiren**:  Bu denetim modu ayrıntılı RBAC izin vermez. Bir kullanıcı çalışma alanına erişmek, çalışma alanına veya özel tablolara izinleri verilmelidir. 

Kullanıcı merkezli çalışma modu çalışma erişirse, tüm verilerine erişim için erişim verilmiş herhangi bir tablo gerekir. Bir kullanıcı çalışma alanında kaynak odaklı modu erişirse, bunlar yalnızca veri bu kaynak için erişim verilmiş tüm tablolarda erişebilir.

Mart 2019 önce oluşturulan tüm çalışma alanları için varsayılan ayar budur.

**Kaynak veya çalışma alanı izinlerini kullanın**: Bu denetim modu ayrıntılı RBAC sağlar. Kullanıcılara yalnızca bunlar Azure izinleri sahip oldukları kaynakları görüntüleyebilir kaynaklarla ilişkili verilere erişim izni olan `read` izni. 

Kullanıcı merkezli çalışma modu çalışma eriştiğinde, çalışma alanı izinlerini uygulanır. Bir kullanıcı çalışma alanında kaynak odaklı modu eriştiğinde, yalnızca kaynağı izinlerini doğrulanır ve çalışma alanı izinleri göz ardı edilir. RBAC, bunları çalışma alanı izinlerini kaldırma ve tanınması kendi kaynak izinleri vererek bir kullanıcı için etkinleştirin.

Mart 2019 sonra oluşturulan tüm çalışma alanları için varsayılan ayar budur.

> [!NOTE]
> Bir kullanıcı çalışma alanına yalnızca kaynak izinleri varsa, bunlar yalnızca çalışma alanını kullanarak erişmeye erişebilir [kaynak odaklı modu](#access-modes).


### <a name="define-access-control-mode-in-azure-portal"></a>Azure portalında erişim denetim modu tanımlayın
Geçerli çalışma alanına erişim denetim modu görüntüleyebileceğiniz **genel bakış** çalışma sayfası **Log Analytics çalışma alanı** menüsü.

![Görünüm çalışma alanına erişim denetim modu](media/manage-access/view-access-control-mode.png)

Bu ayarı değiştirebilirsiniz **özellikleri** çalışma sayfası. Çalışma alanını yapılandırmak için izinleri yoksa, ayarı değiştirmeyi devre dışı bırakılır.

![Çalışma alanı erişimi modunu Değiştir](media/manage-access/change-access-control-mode.png)

### <a name="define-access-control-mode-in-powershell"></a>Erişim denetimi modu PowerShell'de tanımlayın

Abonelikteki tüm çalışma alanları için erişim denetim modu incelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions} 
```

Belirli bir çalışma alanı için erişim denetim modu ayarlamak için aşağıdaki betiği kullanın:

```powershell
$WSName = "my-workspace"
$Workspace = Get-AzResource -Name $WSName -ExpandProperties
if ($Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $Workspace.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $Workspace.ResourceId -Properties $Workspace.Properties -Force
```

Abonelikteki tüm çalışma alanları için erişim denetim modu ayarlamak için aşağıdaki betiği kullanın.

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {
if ($_.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $_.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $_.ResourceId -Properties $_.Properties -Force
```

### <a name="define-access-mode-in-resource-manager-template"></a>Erişim modu Resource Manager şablonunda tanımlama
Bir Azure Resource Manager şablonunda erişim modu yapılandırmak için ayarlanmış **enableLogAccessUsingOnlyResourcePermissions** özellik bayrağı için çalışma alanı için aşağıdaki değerlerden biri.

- **False**: Çalışma alanı için çalışma alanına odaklı izinlerini ayarlayın. Bayrağı ayarlanmamışsa varsayılan ayar budur.
- **True**: Çalışma alanı, kaynak odaklı izinleri ayarlayın.


## <a name="manage-accounts-and-users"></a>Hesapları ve kullanıcıları yönetme
Belirli bir kullanıcıya uygulanan izinleri çalışma alanına erişim modlarını tarafından tanımlanır ve [erişim denetim modu](#access-control-mode) çalışma alanının. **Çalışma alanı izinlerini** herhangi bir çalışma alanı kullanarak bir kullanıcının eriştiği uygulanır **çalışma merkezli** içinde [merkezli çalışma modu](#access-modes). **Kaynak izinleri** bir çalışma alanı ile bir kullanıcının eriştiği uygulanır **kaynak veya çalışma alanı izinlerini kullanın** [erişim denetim modu](#access-control-mode) kullanarak [kaynak odaklı modu ](#access-modes).

### <a name="workspace-permissions"></a>Çalışma alanı izinleri
Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok hesap içerebilir ve her hesabı birden çok çalışma alanına erişim sahibi olabilir. Erişim aracılığıyla yönetilir [Azure rol tabanlı erişim](../../role-based-access-control/role-assignments-portal.md). 


Şu etkinlikler de Azure izinleri gerektirir:

| Eylem                                                          | Gereken Azure İzni | Notlar |
|-----------------------------------------------------------------|--------------------------|-------|
| Ekleme ve kaldırma izleme çözümleri                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Bu izinlerin kaynak grubu veya abonelik düzeyinde verilmiş olması gerekir. |
| Fiyatlandırma katmanını değiştirme                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| *Backup* ve *Site Recovery* çözüm kutucuklarındaki verileri görüntüleme | Yönetici / Ortak yönetici | Klasik dağıtım modeli kullanılarak dağıtılan kaynaklara erişir |
| Azure portalında bir çalışma alanı oluşturma                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


#### <a name="manage-access-to-log-analytics-workspace-using-azure-permissions"></a>Azure izinlerini kullanarak Log Analytics'e erişimi yönetme 
Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin.

Azure Log Analytics çalışma alanları için iki yerleşik kullanıcı rolüne sahiptir:
- Log Analytics Okuyucusu
- Log Analytics Katkıda Bulunan

*Log Analytics Okuyucusu* rolünün üyeleri aşağıdakileri yapabilir:
- Tüm izleme verilerini görüntüleme ve arama 
- Tüm Azure kaynakları üzerinde Azure tanılama yapılandırmasını görüntüleme dahil olmak üzere, izleme ayarlarını görüntüleyin.

Log Analytics okuyucusu rolü, aşağıdaki Azure eylemleri içerir:

| Tür    | İzin | Açıklama |
| ------- | ---------- | ----------- |
| Eylem | `*/read`   | Tüm Azure kaynaklarını ve kaynak yapılandırması görüntüleme olanağı. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları |
| Eylem | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Günlük araması v2 sorguları gerçekleştirme becerisi |
| Eylem | `Microsoft.OperationalInsights/workspaces/search/action` | Günlük araması v1 sorguları gerçekleştirme becerisi |
| Eylem | `Microsoft.Support/*` | Destek olayları açma özelliği |
|Eylem Dışı | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Çalışma alanının engeller veri koleksiyonu API'sini kullanmak ve aracıları yüklemek için gereken anahtarı. Bu kullanıcının yeni kaynaklar çalışma alanına eklemesini engeller |


*Log Analytics Katkıda Bulunan* rolünün üyeleri aşağıdakileri yapabilir:
- Log Analytics okuyucusu gibi tüm izleme verilerini okuma  
- Otomasyon hesaplarını oluşturma ve yapılandırma  
- Yönetim çözümlerini ekleme ve kaldırma    
    > [!NOTE] 
    > Başarıyla son iki eylemleri gerçekleştirmek için bu izin kaynak grubu veya abonelik düzeyinde verilmesi gerekir.  

- Depolama hesabı anahtarlarını okuma   
- Azure Depolama’dan günlüklerin toplanmasını yapılandırma  
- Azure kaynaklarına ilişkin izleme ayarlarını düzenleme:
  - VM'lere VM uzantısı ekleme
  - Tüm Azure kaynaklarında Azure tanılamayı yapılandırma

> [!NOTE] 
> Özelliği, bir sanal makine üzerinde tam denetim kazanmak için bir sanal makineye sanal makine uzantısı eklemek için kullanabilirsiniz.

Log Analytics katkıda bulunan rolü, aşağıdaki Azure eylemleri içerir:

| İzin | Açıklama |
| ---------- | ----------- |
| `*/read`     | Tüm kaynakların ve kaynak yapılandırmalarının görüntülenmesine imkan sağlar. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları |
| `Microsoft.Automation/automationAccounts/*` | Runbook'ları ekleme ve düzenleme dahil olmak üzere Azure Otomasyonu hesapları oluşturma ve yapılandırma olanağı |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Linux uzantısı için OMS aracısı ve Microsoft Monitoring Agent gibi sanal makine uzantılarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Depolama hesabı anahtarını görüntüleyin. Log Analytics’i Azure depolama hesaplarındaki günlükleri okuyacak şekilde yapılandırmak için gereklidir |
| `Microsoft.Insights/alertRules/*` | Uyarı kurallarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.Insights/diagnosticSettings/*` | Azure kaynaklarında tanılama ayarlarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationalInsights/*` | Log Analytics çalışma alanları için yapılandırmaları ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationsManagement/*` | Yönetim çözümlerini ekleme ve kaldırma |
| `Microsoft.Resources/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |

Kullanıcıları bir kullanıcı rolüne eklemek ve bu rolden kaldırmak için `Microsoft.Authorization/*/Delete` ve `Microsoft.Authorization/*/Write` izinleri gereklidir.

Bu rolleri, kullanıcılara farklı kapsamlarda erişim vermek için kullanın:
- Abonelik - Abonelikteki tüm çalışma alanlarına erişim
- Kaynak grubu - Kaynak grubundaki tüm çalışma alanına erişim
- Kaynak - Yalnızca belirtilen çalışma alanına erişim

Atamalar doğru erişim denetimi sağlamak için kaynak düzeyinde (çalışma alanına) gerçekleştirmeniz gerekir.  Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../../role-based-access-control/custom-roles.md) kullanın.

### <a name="resource-permissions"></a>Kaynak izinleri 
Kullanıcılar sorgu kaynak odaklı erişimi kullanarak bir çalışma alanından açtığında, aşağıdaki izinler kaynak gerekir:

| İzin | Açıklama |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Örnekler:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | Tüm günlük veri kaynağı için görüntüleme olanağı.  |


Bu izin, genellikle içeren rolden verilir  _\*/okuma veya_ _\*_ izinleri gibi yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) ve [ Katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) rolleri. Bu izin, belirli eylemleri içeren özel rolleri veya adanmış yerleşik roller içermeyebilir unutmayın.

Bkz: [tablo başına erişim denetimini tanımlama](#table-level-rbac) üstündeyse farklı erişim denetimi için farklı tablolar oluşturmak istiyorsunuz.


## <a name="table-level-rbac"></a>Tablo düzeyi RBAC
**Tablo düzeyi RBAC** , diğer izinlere ek olarak bir Log Analytics çalışma alanında verilerine daha ayrıntılı bir denetim sağlamanıza olanak verir. Bu denetim, yalnızca belirli bir kullanıcı kümesine erişilebilir olan belirli veri türlerini tanımlamanızı sağlar.

Tablo erişim denetimi ile uygulama [Azure özel roller](../../role-based-access-control/custom-roles.md) vermek veya erişimini belirli [tabloları](../log-query/log-query-overview.md#how-azure-monitor-log-data-is-organized) çalışma. Bu roller çalışma alanlarıyla çalışma merkezli ya da kaynak odaklı uygulanır [erişim denetimi modları](#access-control-mode) kullanıcının bakılmaksızın [erişim modu](#access-modes).

Oluşturma bir [özel rol](../../role-based-access-control/custom-roles.md) erişim tablosu için erişim denetimi tanımlamak için aşağıdaki eylemler ile.

- Bir tabloya erişim vermek için de dahil **eylemleri** Rol tanımının bölümü.
- Bir tablo erişimini engellemek için de dahil **NotActions** Rol tanımının bölümü.
- Kullanım * tüm tabloları belirtmek için.

Örneğin, erişimi olan bir rol oluşturmak için _sinyal_ ve _AzureActivity_ tablolar, aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
              "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
              "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Yalnızca erişimi olan bir rol oluşturmak için _SecurityBaseline_ ve başka bir tablo yoktur, aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
    ],
    "NotActions":  [
        "Microsoft.OperationalInsights/workspaces/query/*/read"
    ],
```

### <a name="custom-logs"></a>Özel günlükler
 Özel günlükler, özel günlükleri ve HTTP veri toplayıcı API'sini gibi veri kaynakları tarafından oluşturulur. Günlük türü tanımlamak için en kolay yolu altında listelenen tablolar denetleyerek olan [özel günlükleri günlük şemada](../log-query/get-started-portal.md#understand-the-schema).

 Şu anda vermek alamaz veya tek tek özel Günlükleri erişimini ancak vermek veya erişimini tüm özel günlükleri. Tüm özel günlükleri erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
    ],
```

### <a name="considerations"></a>Dikkat edilmesi gerekenler

- Bir kullanıcı, genel atanırsa üzerinde okuma izni içeren standart okuyucu veya katkıda bulunan rollerine sahip  _\*/okuma_ eylemi geçersiz kılar tablo başına erişim denetimi ve bunları tüm günlük verilerine erişim izni verebilirsiniz.
- Bir kullanıcı diğer herhangi bir izin vermeden tablo başına erişim izni verildiyse API'sinden ancak Azure Portalı'ndan günlük verilerine erişme olanağına olacaktır. Azure portalına erişim sağlamak için Log Analytics okuyucusu rolüne temel kullanın.
- Abonelik için yöneticileri diğer izin ayarlarından bağımsız olarak tüm veri türlerine erişebilir.
- Çalışma alanı sahipleri tablo başına erişim denetimi için herhangi bir kullanıcı gibi kabul edilir.
- Rol atamaları sayısını azaltmak için tek tek kullanıcılar yerine gruplara güvenlik atamanız gerekir. Bu da, yapılandırmak ve erişimi doğrulamak için mevcut Grup Yönetimi araçları kullanmanıza yardımcı olur.




## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [Log Analytics Aracısı genel bakış](../../azure-monitor/platform/log-analytics-agent.md) veri merkezinizde veya diğer bulut ortamında bulunan bilgisayarlardaki verileri toplamak için.
* Azure VM’lerden veri toplamayı yapılandırmak için bkz. [Azure Sanal Makineler hakkında veri toplama](../../azure-monitor/learn/quick-collect-azurevm.md).  

