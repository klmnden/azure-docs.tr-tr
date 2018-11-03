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
ms.devlang: na
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 796e10053df79f8f7106d98dd9c9be6083d9f719
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964161"
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
* Kapsam yapılandırmasına yönelik bekletme ve veri capping gibi ayarları

Tüketimi açısından, mümkün olduğunca az çalışma alanları oluşturma öneririz. Yönetim ve sorgu deneyimi daha kolay ve hızlı kolaylaştırır. Ancak, önceki özelliklere bağlı olarak, durumlarda birden çok çalışma alanı oluşturmak isteyebilirsiniz:

* Global bir şirketseniz ve veri bağımsızlığı veya uyumluluk nedenleriyle verilerin belirli bölgelerde depolanmasına gerek duyuyorsanız.
* Azure kullanıyorsanız ve çalışma alanını, yönettiği Azure kaynaklarıyla aynı bölgede bulundurarak giden veri aktarımı ücretlerini ortadan kaldırmak istiyorsanız.
* Ücretleri farklı departmanlara veya iş gruplarına kendi Azure aboneliğinde her departman veya iş grubu için bir çalışma alanı oluşturarak kendi kullanımınıza göre ayırmak istiyorsanız.
* Yönetilen bir hizmet sağlayıcısıysanız ve yönettiğiniz her bir müşteriye ilişkin Log Analytics verilerini diğer müşterilerin verilerinden yalıtmak istiyorsanız.
* Birden çok müşteriyi yönetiyorsanız ve her müşteri istediğiniz / bölüm / iş grubunun kendi verilerini, ancak veri diğerlerinden değil görmek için.

Verileri toplamak için Windows aracılarını kullanıyorsanız [her bir aracıyı, bir veya daha fazla çalışma alanına raporlama yapacak şekilde yapılandırabilirsiniz](log-analytics-agent-windows.md).

System Center Operations Manager'ı kullanıyorsanız her bir Operations Manager yönetim grubu yalnızca bir çalışma alanıyla bağlantılı olabilir. Operations Manager tarafından yönetilen bilgisayarlara Microsoft İzleme Aracısını yükleyebilir ve hem Operations Manager hem de farklı bir Log Analytics çalışma alanı için aracı raporu alabilirsiniz.

## <a name="workspace-information"></a>Çalışma alanı bilgileri

Azure portalında çalışma alanınızla ilgili bilgileri görüntüleyebilirsiniz. 

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın

2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.  

    ![Azure portal](media/log-analytics-manage-access/azure-portal-01.png)  

3. Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin.

4. Çalışma sayfası, Başlarken, yapılandırma ve ek bilgi bağlantıları hakkında ayrıntıları gösterir.  

    ![Çalışma alanı ayrıntıları](./media/log-analytics-manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Hesapları ve kullanıcıları yönetme
Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok hesap içerebilir ve her hesabı birden çok çalışma alanına erişim sahibi olabilir. Erişim aracılığıyla yönetilir [Azure rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md). Bu erişim hakları, Azure portal ve API erişimi geçerlidir.


Şu etkinlikler de Azure izinleri gerektirir:

| Eylem                                                          | Gereken Azure İzni | Notlar |
|-----------------------------------------------------------------|--------------------------|-------|
| Yönetim çözümlerini ekleme ve kaldırma                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Bu izinlerin kaynak grubu veya abonelik düzeyinde verilmiş olması gerekir. |
| Fiyatlandırma katmanını değiştirme                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| *Backup* ve *Site Recovery* çözüm kutucuklarındaki verileri görüntüleme | Yönetici / Ortak yönetici | Klasik dağıtım modeli kullanılarak dağıtılan kaynaklara erişir |
| Azure portalında bir çalışma alanı oluşturma                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Azure izinlerini kullanarak Log Analytics’e erişimi yönetme
Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin.

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

Doğru erişim denetimi sağlamak için atamaları kaynak düzeyinde (çalışma alanında) gerçekleştirmenizi öneririz.  Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../role-based-access-control/custom-roles.md) kullanın.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Mevcut bir çalışma alanını Azure aboneliğine bağlama
26 Eylül 2016'dan sonra oluşturulan tüm çalışma alanları, oluşturma zamanında bir Azure aboneliğine bağlanmalıdır. Bu tarihten önce oluşturulan çalışma alanları, oturum açtığınızda bir çalışma alanına bağlanmalıdır. Çalışma alanını Azure portalından oluşturduğunuzda veya çalışma alanınızı bir Azure aboneliğine bağladığınızda, Azure Active Directory'niz kuruluş hesabınız olarak bağlanır.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Çalışma alanını, Azure portalında bir Azure aboneliğine bağlama
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.  

2. Log Analytics abonelikleri bölmesinde tıklatın **Ekle**.  

    ![çalışma alanlarının listesi](./media/log-analytics-manage-access/workspace-link-existing-01.png)

3. Gelen **Log Analytics çalışma alanı** bölmesinde tıklayın **bağlamak varolan**.  

4. **Gerekli ayarları yapılandır**'a tıklayın.  

5. Henüz Azure hesabınıza bağlanmamış olan çalışma alanlarının listesini görürsünüz. Çalışma alanını seçin.  
   
6. Gerekirse şu öğelere ilişkin değerleri değiştirebilirsiniz:
   * Abonelik
   * Kaynak grubu
   * Konum
   * Fiyatlandırma katmanı  

7. **Tamam** düğmesine tıklayın. Çalışma alanı artık Azure hesabınıza bağlı.

> [!NOTE]
> Bağlamak istediğiniz çalışma alanını görmüyorsanız Azure aboneliğinizin, OMS portalını kullanarak oluşturduğunuz çalışma alanına erişimi yoktur.  OMS portalından bu hesaba erişim vermek için bkz. [Mevcut bir çalışma alanına kullanıcı ekleme](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Çalışma alanını ücretli plana yükseltme
OMS için üç çalışma alanı plan türü mevcuttur: **Ücretsiz**, **Tek Başına** ve **OMS**.  *Ücretsiz* plandaysanız bir günde Log Analytics’e gönderilebilecek veriler için üst sınır 500 MB’dir.  Bu miktarı aşarsanız bu sınırın üzerinde veri toplanmasını önlemek için çalışma alanınızı ücretli bir planla değiştirmeniz gerekir. Plan türünüzü istediğiniz zaman değiştirebilirsiniz.  OMS fiyatlandırması hakkında daha fazla bilgi için bkz. [Fiyatlandırma Ayrıntıları](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Bir OMS aboneliğinden gelen destek haklarını kullanma
OMS E1, OMS E2 OMS veya System Center için OMS Eklentisi satın alındıktan sonra sunulan destek haklarını kullanmak için OMS Log Analytics’in *OMS* planını seçin.

Bir OMS aboneliği satın aldığınızda, destek hakları Kurumsal Anlaşmanıza eklenir. Bu anlaşma kapsamında oluşturulan herhangi bir Azure aboneliği bu destek haklarını kullanabilir. Bu aboneliklerdeki tüm çalışma alanları OMS yetkilendirmelerini kullanır.

Çalışma alanı kullanımının, OMS aboneliğinden gelen destek haklarınıza uygulandığından emin olmak için şunları yapmanız gerekir:

1. OMS aboneliğini içeren Kurumsal Anlaşmanın parçası olan Azure aboneliğinde çalışma alanınızı oluşturma

2. Çalışma alanı için *OMS* planını seçme

> [!NOTE]
> Çalışma alanınız 26 Eylül 2016’dan önce oluşturulduysa ve Log Analytics fiyatlandırma planınız *Premium* ise, bu çalışma alanı System Center için OMS Eklentisi’nden gelen destek haklarını kullanır. Destek haklarınızı, *OMS* fiyatlandırma katmanına geçerek de kullanabilirsiniz.
>
>

OMS aboneliği destek hakları Azure Portalı'nda görünmez. Destek haklarını ve kullanımı Enterprise Portal'da görebilirsiniz.  

Çalışma alanınızın bağlı olduğu Azure aboneliğini değiştirmeniz gerekiyorsa Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet'ini kullanabilirsiniz.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Kurumsal Anlaşmadaki bir Azure Taahhüdünü Kullanma
OMS aboneliğiniz yoksa OMS’nin her bir bileşeni için ayrı olarak ödeme yaparsınız ve kullanım Azure faturanızda görünür.

Azure aboneliklerinizin bağlı olduğu kurumsal kayıt anlaşmasında bir Azure parasal taahhüdünüz varsa Log Analytics kullanımı, kalan parasal taahhüde otomatik olarak eklenir.

Çalışma alanının bağlı olduğu Azure aboneliğini değiştirmeniz gerekiyorsa Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet'ini kullanabilirsiniz.  

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-azure-portal"></a>Azure portalında çalışma alanını ücretli fiyatlandırma katmanı olarak değiştirme
1. Azure portalında Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin.

2. Çalışma Alanı bölmesinden altında **genel**seçin **fiyatlandırma katmanı**.  

3. Altında **fiyatlandırma katmanı**, bir fiyatlandırma katmanı seçin ve ardından **seçin**.  
    ![Seçili fiyatlandırma planı](./media/log-analytics-manage-access/workspace-pricing-tier-info.png)

> [!NOTE]
> Çalışma alanınız bir Otomasyon hesabıyla bağlantılıysa, *Tek Başına (GB başına)* fiyatlandırma katmanını seçebilmeniz için tüm **Otomasyon ve Denetim** çözümlerini silmeniz ve Otomasyon hesabının bağlantısını kaldırmanız gerekir. Çalışma alanı dikey penceresindeki **Genel** altında **Çözümler**’e tıklayıp çözümleri silin. Bir Otomasyon hesabının bağlantısını kaldırmak için **Fiyatlandırma katmanı** dikey penceresinde Otomasyon hesabının adına tıklayın.
>
>

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-oms-portal"></a>OMS portalında çalışma alanını ücretli fiyatlandırma katmanı olarak değiştirme

OMS portalını kullanarak fiyatlandırma katmanını değiştirmek için, bir Azure aboneliğine sahip olmanız gerekir.

1. OMS portalında **Ayarlar** kutucuğuna tıklayın.

2. **Hesaplar** sekmesine ve ardından **Azure Aboneliği ve Veri Planı** sekmesine tıklayın.

3. Kullanmak istediğiniz fiyatlandırma katmanına tıklayın.

4. **Kaydet**’e tıklayın.  

    ![abonelik ve veri planları](./media/log-analytics-manage-access/subscription-tab.png)

Yeni veri planınız, web sayfanızın üst kısmındaki OMS portalı şeridinde görüntülenir.

![OMS şeridi](./media/log-analytics-manage-access/data-plan-changed.png)

## <a name="next-steps"></a>Sonraki adımlar
* Veri merkezinizde veya diğer bulut ortamlarında bulunan bilgisayarlardan veri toplamak için bkz. [Log Analytics ile ortamınızdaki bilgisayarlardan veri toplama](log-analytics-concept-hybrid.md).
* Azure VM’lerden veri toplamayı yapılandırmak için bkz. [Azure Sanal Makineler hakkında veri toplama](log-analytics-quick-collect-azurevm.md).  
* İşlev eklemek ve veri toplamak için bkz. [Çözüm Galerisinden Log Analytics çözümleri ekleme](../monitoring/monitoring-solutions.md).

