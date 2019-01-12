---
title: Azure Log Analytics ve OMS portalında çalışma alanlarını yönetme | Microsoft Docs
description: Kullanıcılar, hesaplar, çalışma alanları ve Azure hesapları ile ilgili çeşitli yönetim görevlerini kullanarak Azure Log Analytics’teki çalışma alanlarını ve OMS portalını yönetebilirsiniz.
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
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 6c8f48ce71e11d1de0c28b4dab5327ab03e54f28
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231793"
---
# <a name="manage-workspaces"></a>Çalışma alanlarını yönetme

Log Analytics'e erişimi yönetmek için çalışma alanları ile ilgili çeşitli yönetim görevleri gerçekleştirirsiniz. Bu makalede, önerileri ve çalışma alanlarını yönetmeye yönelik yordamları sağlar. Çalışma alanı, temelde hesap bilgilerini ve hesaba ilişkin basit yapılandırma bilgilerini içeren bir kapsayıcıdır. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Bir çalışma alanı oluşturmak için şunlar gereklidir:

1. Bir Azure aboneliğine sahip olmanız.
2. Bir çalışma alanı adı seçmeniz.
3. Çalışma alanı biri Abonelikleriniz ve kaynak grubu ile ilişkilendirin.
4. Coğrafi bir konum seçmeniz.

## <a name="determine-the-number-of-workspaces-you-need"></a>İhtiyacınız olan çalışma alanı sayısını belirleme
Çalışma alanı, bir Azure kaynağıdır. Bu alan, verilerin toplandığı, derlendiği, çözümlendiği ve Azure portalında sunulduğu bir kapsayıcıdır.

Azure aboneliği başına birden çok çalışma alanına sahip olabilir ve aralarında kolayca sorgulama yapma olanağı ile birden çok çalışma alanına erişebilirsiniz. Bu bölümde birden çok çalışma alanı oluşturmanın yararlı olabileceği durumlar açıklanır.

Günümüzde bir çalışma alanı aşağıdakileri sağlar:

* Veri depolama için coğrafi bir konum
* Farklı kullanıcı erişim haklarını tanımlamak için veri yalıtımı
* Kapsam ayarlarının yapılandırılması için ister [fiyatlandırma katmanı](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [bekletme](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) ve [veri capping](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#daily-cap) 

Tüketimi açısından, mümkün olduğunca az çalışma alanları oluşturma öneririz. Yönetim ve sorgu deneyimi daha kolay ve hızlı kolaylaştırır. Ancak, önceki özelliklere bağlı olarak, durumlarda birden çok çalışma alanı oluşturmak isteyebilirsiniz:

* Global bir şirketseniz ve veri bağımsızlığı veya uyumluluk nedenleriyle verilerin belirli bölgelerde depolanmasına gerek duyuyorsanız.
* Azure kullanıyorsanız ve çalışma alanını, yönettiği Azure kaynaklarıyla aynı bölgede bulundurarak giden veri aktarımı ücretlerini ortadan kaldırmak istiyorsanız.
* Ücretleri farklı departmanlara veya iş gruplarına kendi Azure aboneliğinde her departman veya iş grubu için bir çalışma alanı oluşturarak kendi kullanımınıza göre ayırmak istiyorsanız.
* Yönetilen bir hizmet sağlayıcısıysanız ve yönettiğiniz her bir müşteriye ilişkin Log Analytics verilerini diğer müşterilerin verilerinden yalıtmak istiyorsanız.
* Birden çok müşteriyi yönetiyorsanız ve her müşteri istediğiniz / bölüm / iş grubunun kendi verilerini, ancak veri diğerlerinden değil görmek için.

Verileri toplamak için Windows aracılarını kullanıyorsanız [her bir aracıyı, bir veya daha fazla çalışma alanına raporlama yapacak şekilde yapılandırabilirsiniz](../../azure-monitor/platform/agent-windows.md).

System Center Operations Manager'ı kullanıyorsanız her bir Operations Manager yönetim grubu yalnızca bir çalışma alanıyla bağlantılı olabilir. Operations Manager tarafından yönetilen bilgisayarlara Microsoft İzleme Aracısını yükleyebilir ve hem Operations Manager hem de farklı bir Log Analytics çalışma alanı için aracı raporu alabilirsiniz.

## <a name="workspace-information"></a>Çalışma alanı bilgileri

Azure portalında çalışma alanınızla ilgili bilgileri görüntüleyebilirsiniz. 

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın

2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.  

    ![Azure portal](media/manage-access/azure-portal-01.png)  

3. Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin.

4. Çalışma sayfası, Başlarken, yapılandırma ve ek bilgi bağlantıları hakkında ayrıntıları gösterir.  

    ![Çalışma alanı ayrıntıları](./media/manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Hesapları ve kullanıcıları yönetme
Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok hesap içerebilir ve her hesabı birden çok çalışma alanına erişim sahibi olabilir. Erişim aracılığıyla yönetilir [Azure rol tabanlı erişim](../../role-based-access-control/role-assignments-portal.md). Bu erişim hakları, Azure portal ve API erişimi geçerlidir.


Şu etkinlikler de Azure izinleri gerektirir:

| Eylem                                                          | Gereken Azure İzni | Notlar |
|-----------------------------------------------------------------|--------------------------|-------|
| Yönetim çözümlerini ekleme ve kaldırma                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Bu izinlerin kaynak grubu veya abonelik düzeyinde verilmiş olması gerekir. |
| Fiyatlandırma katmanını değiştirme                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| *Backup* ve *Site Recovery* çözüm kutucuklarındaki verileri görüntüleme | Yönetici / Ortak yönetici | Klasik dağıtım modeli kullanılarak dağıtılan kaynaklara erişir |
| Azure portalında bir çalışma alanı oluşturma                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Azure izinlerini kullanarak Log Analytics’e erişimi yönetme
Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin.

Azure Log Analytics için iki yerleşik kullanıcı rolüne sahiptir:
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

Doğru erişim denetimi sağlamak için atamaları kaynak düzeyinde (çalışma alanında) gerçekleştirmenizi öneririz.  Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../../role-based-access-control/custom-roles.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [Log Analytics Aracısı genel bakış](../../azure-monitor/platform/log-analytics-agent.md) veri merkezinizde veya diğer bulut ortamında bulunan bilgisayarlardaki verileri toplamak için.
* Azure VM’lerden veri toplamayı yapılandırmak için bkz. [Azure Sanal Makineler hakkında veri toplama](../../azure-monitor/learn/quick-collect-azurevm.md).  
* İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](../../azure-monitor/insights/solutions.md).

