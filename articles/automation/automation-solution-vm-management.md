---
title: Başlat/Durdur Vm'leri çalışma saatleri çözümü
description: Bu VM management çözüm başlar ve Azure Resource Manager sanal makinelerinizi bir zamanlamaya göre durdurur ve Azure İzleyici günlüklerinden proaktif olarak izler.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 05/21/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 39ba577580424bf8283d64198bb3068b82869c51
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476883"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Sırasında Azure Otomasyonu çözümde yoğun olmayan saatlerde Vm'leri başlatma/durdurma

Çözüm başlar ve Azure sanal makinelerinizi kullanıcı tanımlı zamanlamalarda durdurur, Azure İzleyici günlüklerine aracılığıyla Öngörüler sağlar ve kullanarak isteğe bağlı bir e-postaları gönderen çalışma saatleri dışında Vm'leri başlatma/durdurma [Eylem grupları](../azure-monitor/platform/action-groups.md). Bu, çoğu senaryo için hem Azure Resource Manager ve klasik Vm'leri destekler.

> [!NOTE]
> Çözüm çözümünü dağıttığınızda, Otomasyon hesabınızda içeri Azure modülleri ile test edilmiştir çalışma saatleri dışında Vm'leri başlatma/durdurma. Çözüm, şu anda Azure modülü daha yeni sürümleriyle çalışmaz. Bu, yalnızca Otomasyon Vm'leri başlatma/durdurma çalışma sırasında yeniden hedeflenen çözümü çalıştırmak için kullandığınız hesabın etkiler. Daha yeni sürümleri, Azure modülü, diğer Automation hesaplarında, açıklanan şekilde kullanmaya devam edebilirsiniz [Azure automation'da Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)

Bu çözüm, VM maliyetlerini en iyi hale getirmek isteyen kullanıcılar için bir merkezi olmayan düşük maliyetli Otomasyon seçeneği sunar. Bu çözüm ile şunları yapabilirsiniz:

- Vm'leri başlatma ve durdurma zamanlayın.
- VM'ler (Klasik VM'ler için desteklenmez) Azure etiketleri kullanarak artan düzende durdurmak ve başlatmak zamanlayın.
- Düşük CPU kullanımına göre DISPLAYFILTER VM'ler.

Geçerli çözümdeki sınırlamalar aşağıda verilmiştir:

- Bu çözüm, herhangi bir bölgedeki Vm'leri yönetir, ancak yalnızca, Azure Otomasyonu hesabı ile aynı abonelikte kullanılabilir.
- Bu çözüm, Log Analytics çalışma alanı, bir Azure Otomasyonu hesabını ve Uyarıları destekleyen herhangi bir bölgesine AzureGov ve Azure ile kullanılabilir. E-posta işlevselliği AzureGov bölgeler şu anda desteklemez.

> [!NOTE]
> Klasik VM'ler için çözümü kullanıyorsanız, tüm Vm'leriniz sıralı olarak bulut hizmeti başına işlenir. Sanal makine, farklı bulut hizmetleri arasında yine de paralel olarak işlenir.
>
> Azure bulut çözümü sağlayıcısı (Azure CSP) abonelikleri yalnızca Azure Resource Manager modeline destek, Azure Resource Manager - program kullanılamıyor. Başlat/Durdur çözümü çalıştığında Klasik kaynakları yönetmek için cmdlet'ler olduğu gibi hatalar alabilirsiniz. CSP hakkında daha fazla bilgi için bkz: [CSP aboneliklerinde kullanılabilir hizmetler](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments). Bir CSP aboneliği kullanıyorsanız değiştirmelisiniz [ **External_EnableClassicVMs** ](#variables) değişkenini **False** dağıtımdan sonra.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu çözüm için runbook'ları ile çalışır bir [Azure farklı çalıştır hesabı](automation-create-runas-account.md). Sona ya da sık değişebilecek bir parola yerine sertifika doğrulaması kullandığından farklı çalıştır hesabı tercih edilen kimlik doğrulama yöntemidir.

VM Başlat/Durdur çözümü için ayrı bir Otomasyon hesabı kullanmak için önerilir. Azure modülü sürümler sık yükseltilir ve kendi parametrelerini değişebilir olduğundan budur. VM başlatma/durdurma çözüm kullandığı cmdlet'lerin daha yeni sürümleriyle çalışmayabilir şekilde aynı tempoyla yükseltilmez. Otomasyon hesabı, üretim aktarmadan önce modülü güncelleştirmeleri bir Otomasyon hesabı testinde test etmek için önerilir.

### <a name="permissions-needed-to-deploy"></a>Dağıtmak için gereken izinler

Bir kullanıcı, Vm'leri başlatma/durdurma sırasında saat çözümü Kapat'ı dağıtmak için gereken belirli izinleri vardır. Bu izinleri bir önceden oluşturulmuş Otomasyon hesabının ve Log Analytics çalışma alanı kullanıyorsanız farklı veya dağıtım sırasında yenilerini oluşturma. Abonelik üzerinde katkıda bulunan ve genel yönetici Azure Active Directory kiracınız varsa, aşağıdaki izinleri yapılandırmanız gerekmez. Aşağıdaki gerekli izinleri, bu haklara sahip bir ya da özel bir rol yapılandırmanız gerekir, bkz.

#### <a name="pre-existing-automation-account-and-log-analytics-account"></a>Önceden var olan Otomasyon hesabının ve Log Analytics hesabı

Vm'leri başlatma/durdurma sırasında saat çözüm kapalı bir Otomasyon hesabının ve Log Analytics çözümünü dağıtma kullanıcı üzerinde aşağıdaki izinleri gerektirir dağıtmak için **kaynak grubu**. Rolleri hakkında daha fazla bilgi edinmek için [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md).

| İzin | `Scope`|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/variables/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/schedules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/runbooks/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/connections/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/certificates/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/modules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/modules/read | Kaynak Grubu |
| Microsoft.automation/automationAccounts/jobSchedules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/jobs/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/jobs/read | Kaynak Grubu |
| Microsoft.OperationsManagement/solutions/write | Kaynak Grubu |
| Microsoft.OperationalInsights/workspaces/* | Kaynak Grubu |
| Microsoft.Insights/diagnosticSettings/write | Kaynak Grubu |
| Microsoft.Insights/ActionGroups/Write | Kaynak Grubu |
| Microsoft.Insights/ActionGroups/read | Kaynak Grubu |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak Grubu |
| Microsoft.Resources/deployments/* | Kaynak Grubu |

#### <a name="new-automation-account-and-a-new-log-analytics-workspace"></a>Yeni Otomasyon hesabı ve yeni bir Log Analytics çalışma alanı

Mesai saatleri dışında başlatma/durdurma Vm'leri dağıtmak için aşağıdaki izinlerin yanı sıra önceki bölümde tanımlanan izinleri çözüme yeni bir Otomasyon hesabının ve Log Analytics çalışma alanı çözümü dağıtma kullanıcı gerekir:

- Ortak yönetici abonelikte - bunu yalnızca klasik farklı çalıştır hesabı oluşturmak için gereklidir
- Parçası olarak [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) **uygulama geliştiricisi** rol. Farklı Çalıştır hesaplarını yapılandırma hakkında ayrıntılı bilgi için bkz: [farklı çalıştır hesaplarını yapılandırmak için izinleri](manage-runas-account.md#permissions).
- Abonelik veya aşağıdaki izinlere katkıda bulunan.

| İzin |`Scope`|
| --- | --- |
| Microsoft.Authorization/Operations/read | Abonelik|
| Microsoft.Authorization/permissions/read |Abonelik|
| Microsoft.Authorization/roleAssignments/read | Abonelik |
| Microsoft.Authorization/roleAssignments/write | Abonelik |
| Microsoft.Authorization/roleAssignments/delete | Abonelik |
| Microsoft.Automation/automationAccounts/connections/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/certificates/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/write | Kaynak Grubu |
| Microsoft.OperationalInsights/workspaces/write | Kaynak Grubu |

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Vm'leri başlatma/durdurma sırasında yoğun olmayan saatlerde çözüm Otomasyon hesabınıza eklemek için aşağıdaki adımları uygulayın ve ardından çözümü özelleştirmek üzere değişkenlerini yapılandırmak.

1. Bir Otomasyon hesap sayfasında **VM başlatma/durdurma** altında **ilgili kaynakları**. Buradan, tıkladığınız **daha fazla bilgi edinin ve çözümü etkinleştirmek**. Dağıtılan VM başlatma/durdurma Çözüm zaten varsa, bunu tıklayarak seçebilirsiniz **çözümü yönetmek** ve listede bulunuyor.

   ![Otomasyon hesabı etkinleştir](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Ayrıca, her yerden oluşturabileceğiniz tıklayarak Azure portalında **kaynak Oluştur**. Market sayfasını içinde bir anahtar sözcüğü gibi yazın **Başlat** veya **Başlat/Durdur**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Alternatif olarak, bir veya daha fazla sözcüklerden çözüm tam adını yazın ve Enter tuşuna basın. Seçin **Vm'leri çalışma saatleri dışında başlatma/durdurma** Arama sonuçlarından.

2. İçinde **Vm'leri çalışma saatleri dışında başlatma/durdurma** sayfasında seçilen çözüm için özet bilgilerini gözden geçirin ve ardından **Oluştur**.

   ![Azure portal](media/automation-solution-vm-management/azure-portal-01.png)

3. **Çözüm Ekle** sayfası görüntülenir. Çözümü Otomasyon aboneliğinize aktarmadan önce yapılandırmanız istenir.

   ![VM Management Çözüm Ekle sayfası](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

4. Üzerinde **Çözüm Ekle** sayfasında **çalışma**. Otomasyon hesabının bulunduğu Azure aboneliğine bağlı bir Log Analytics çalışma alanını seçin. Bir çalışma alanınız yoksa, seçin **yeni çalışma alanı oluştur**. Üzerinde **Log Analytics çalışma alanı** sayfasında, aşağıdaki adımları gerçekleştirin:
   - Yeni bir ad belirtin **Log Analytics çalışma alanı**, "ContosoLAWorkspace" gibi.
   - Seçin bir **abonelik** varsayılan seçili uygun değilse açılan listeden seçerek bağlamak için.
   - İçin **kaynak grubu**, yeni bir kaynak grubu oluşturun veya varolan bir tanesini seçin.
   - Bir **Konum** seçin. Şu anda yalnızca mevcut konumlarının **Avustralya Güneydoğu**, **Kanada orta**, **Orta Hindistan**, **Doğu ABD**, **Doğu Japonya**, **Güneydoğu Asya**, **UK Güney**, **Batı Avrupa**, ve **Batı ABD 2**.
   - Bir **Fiyatlandırma katmanı** seçin. Seçin **GB başına (tek başına)** seçeneği. Azure İzleyici günlüklerine güncelleştirdi [fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/) ve GB başına katman tek seçenektir.

   > [!NOTE]
   > Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
   >
   > Desteklenen eşleme çiftlerine bir listesi için bkz. [Otomasyon hesabının ve Log Analytics çalışma alanı için bölge eşleme](how-to/region-mappings.md).

5. Gerekli bilgileri girdikten sonra **Log Analytics çalışma alanı** sayfasında **Oluştur**. Altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüden döndüren size **Çözüm Ekle** işiniz bittiğinde sayfa.
6. Üzerinde **Çözüm Ekle** sayfasında **Otomasyon hesabı**. Yeni bir Log Analytics çalışma alanı oluşturuyorsanız, kendisiyle ilişkilendirilmiş olması için yeni bir Otomasyon hesabı oluşturun veya bir Log Analytics çalışma alanına bağlı olmayan bir Otomasyon hesabı seçin. Mevcut bir Otomasyon hesabı seçin veya **Otomasyon hesabı oluşturma**ve **Otomasyon hesabı Ekle** sayfasında, aşağıdaki bilgileri sağlayın:
   - **Ad** alanına Otomasyon hesabının adını girin.

     Diğer tüm seçenekler seçili Log Analytics çalışma alanı otomatik olarak doldurulur. Bu seçenekler değiştirilemez. Bu çözüme dahil olan runbook'lar için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır. Tıkladıktan sonra **Tamam**, yapılandırma seçenekleri doğrulanır ve Otomasyon hesabı oluşturulur. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

7. Son olarak, üzerinde **Çözüm Ekle** sayfasında **yapılandırma**. **Parametreleri** sayfası görüntülenir.

   ![Çözüm için parametreleri sayfası](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Burada, sizden istenir:
   - Belirtin **hedef ResourceGroup adları**. Bu değerler, bu çözüm tarafından yönetilecek Vm'leri içeren kaynak grubu adları. Birden fazla ad girin ve her (Bu değerler büyük küçük harfe duyarlı değildir) virgül kullanarak ayırın. Abonelikte yer alan tüm kaynak gruplarındaki sanal makineleri hedeflemek istiyorsanız, joker karakter kullanılması desteklenir. Bu değer depolanan **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenleri.
   - Belirtin **VM çıkarma listesini (dize)** . Bu değer, hedef kaynak grubu bir veya daha fazla sanal makinelerden adıdır. Birden fazla ad girin ve her (Bu değerler büyük küçük harfe duyarlı değildir) virgül kullanarak ayırın. Joker karakter kullanılması desteklenir. Bu değer depolanan **External_ExcludeVMNames** değişkeni.
   - Seçin bir **zamanlama**. Bir tarih ve saat zamanlamanızı için seçin. Yinelenen bir günlük zamanlama, seçtiğiniz saat ile başlayarak oluşturulur. Başka bir bölge seçmeyi kullanılabilir değil. Çözüm yapılandırdıktan sonra belirli bir saat dilimi için zamanlama yapılandırmak için bkz [başlatma ve kapatma zamanlamasını değiştirme](#modify-the-startup-and-shutdown-schedules).
   - Almaya **e-posta bildirimleri** bir eylem grubundan varsayılan değerini kabul **Evet** ve geçerli bir eposta adresi belirtin. Seçerseniz **Hayır** ancak daha sonraki bir tarihte karar e-posta bildirimleri almak istediğinizi, güncelleştirebilirsiniz [eylem grubu](../azure-monitor/platform/action-groups.md) virgülle ayrılmış geçerli e-posta adresi ile oluşturulur. Aşağıdaki uyarı kurallarını etkinleştirmeniz gerekir:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > İçin varsayılan değer **hedef ResourceGroup adları** olduğu bir **&ast;** . Bu, bir Abonelikteki tüm sanal makineler hedefler. Aboneliğinizdeki tüm sanal makineleri hedeflemek için çözüm istemiyorsanız bu değeri zamanlamaları etkinleştirilmeden önce kaynak grubu adları listesine güncelleştirilmesi gerekiyor.

8. Çözüm için gereken ve ilk ayarları yapılandırdıktan sonra tıklayın **Tamam** kapatmak için **parametreleri** sayfasından seçim yapıp **Oluştur**. Tüm ayarlar doğrulandıktan sonra çözümün aboneliğinize dağıtılır. Bu işlemin tamamlanması birkaç saniye sürebilir ve altında ilerleme durumunu izleyebilir **bildirimleri** menüsünde.

> [!NOTE]
> Otomasyon hesabınızda, dağıtım tamamlandıktan sonra bir Azure bulut çözümü sağlayıcısı (Azure CSP) aboneliğiniz varsa, Git **değişkenleri** altında **paylaşılan kaynakları** ayarlayıp [ **External_EnableClassicVMs** ](#variables) değişkenini **False**. Bu, Klasik VM kaynak mı arıyorsunuz gelen çözüm durdurur.

## <a name="scenarios"></a>Senaryolar

Çözüm, üç farklı senaryolar içerir. Bu senaryolar şunlardır:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Senaryo 1: Bir zamanlamaya göre Vm'leri başlatma/durdurma

Çözümü ilk kez dağıttığınızda bu senaryo varsayılan yapılandırmadır. Örneğin, akşam iş çıkıp office geri olduğunuzda sabah başlatmak, bir Abonelikteki tüm sanal makineleri durdurmak için yapılandırabilirsiniz. Zamanlama yapılandırdığınızda **zamanlanmış StartVM** ve **zamanlanmış StopVM** dağıtımı sırasında bunlar başlatıp hedeflenen VM'ler. Bu çözüm, yalnızca sanal makineleri durdurmak için yapılandırma desteklenir, bkz: [başlatma ve kapatma zamanlamalarını değiştirmek](#modify-the-startup-and-shutdown-schedules) özel bir zamanlama yapılandırmak hakkında bilgi edinmek için.

> [!NOTE]
> Zamanlama süre parametresini yapılandırdığınızda saat dilimi geçerli saat diliminizde alınır. Ancak, Azure automation'da UTC biçiminde depolanır. Bu dağıtım sırasında işlendiğinden hiçbir saat dilimi dönüştürmesi yapmak gerekmez.

Olan Vm'leri kapsamda aşağıdaki değişkenleri yapılandırarak, denetimi: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, ve **External_ExcludeVMNames**.

Abonelik ve kaynak grubu karşı önlem hedefleme veya belirli bir liste VM'ler, ancak ikisini birden hedefleyen etkinleştirebilirsiniz.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Hedef abonelik ve kaynak grubunda karşı başlatma ve durdurma eylemleri

1. Yapılandırma **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** VM'ler hedef belirtmek için değişkenleri.
2. Etkinleştirme ve güncelleştirme **zamanlanmış StartVM** ve **zamanlanmış StopVM** zamanlamalar.
3. Çalıştırma **ScheduledStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listesi tarafından hedef

1. Çalıştırma **ScheduledStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat**, virgülle ayrılmış bir liste VM ekleme *VMList* parametresi ve ardından WHATIF parametresi **True**. Değişikliklerinizi önizleyin.
1. Yapılandırma **External_ExcludeVMNames** Vm'leri (VM1, VM2 ve VM3) virgülle ayrılmış listesiyle birlikte parametresi.
1. Bu senaryo değil dikkate almaz **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyonu zamanlaması oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyonu'nda runbook zamanlama](../automation/automation-schedules.md).

> [!NOTE]
> Değeri **hedef ResourceGroup adları** değeri olarak hem depolanan **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames**. Daha fazla ayrıntı için her biri farklı kaynak gruplarında hedeflemek için bu değişkenleri değiştirebilir. Kullanmak için başlatma eylemini, **External_Start_ResourceGroupNames**ve durdurma eylemi için **External_Stop_ResourceGroupNames**. VM'ler, başına otomatik olarak eklenir ve zamanlamaları durdurabilirsiniz.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Senaryo 2: Etiketleri kullanarak dizideki VM'leri başlatma/durdurma

Dağıtılmış bir iş yükü destekleyen birden çok VM'de iki veya daha fazla bileşen içeren bir ortamda, sırayla bileşenleri başlatıldığında ve durdurulduğunda dizisi destekleyen önemlidir. Bu senaryo, aşağıdaki adımları uygulayarak gerçekleştirebilirsiniz:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Hedef abonelik ve kaynak grubunda karşı başlatma ve durdurma eylemleri

1. Ekleme bir **sequencestart** ve **sequencestop** olarak hedeflenen VM'ler için bir pozitif tamsayı değeri olan etiketi **External_Start_ResourceGroupNames** ve  **External_Stop_ResourceGroupNames** değişkenleri. Başlatma ve durdurma eylemleri, artan sırada gerçekleştirilir. Bir VM'yi etiketleme öğrenmek için bkz. [Azure'da Windows sanal makine etiketleme](../virtual-machines/windows/tag.md) ve [Azure'da bir Linux sanal makinesi etiketleme](../virtual-machines/linux/tag.md).
1. Zamanlamalarını değiştirmek **sıralı StartVM** ve **sıralı StopVM** tarih ve saat gereksinimlerinizi karşılayan ve zamanlamasını etkinleştirmek için.
1. Çalıştırma **SequencedStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.
1. Eylem Önizleme ve üretime yönelik Vm'leri uygulamadan önce gerekli değişiklikleri yapın. Ne zaman hazır, el ile runbook ayarlanmış parametreye yürütülmesinin **False**, veya Otomasyon zamanlama **sıralı StartVM** ve **sıralı StopVM** çalıştırın otomatik olarak belirlenen zamanlamanızı izleyerek.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listesi tarafından hedef

1. Ekleme bir **sequencestart** ve **sequencestop** için eklemeyi planladığınız VM'ler için bir pozitif tamsayı değeri olan etiketi **VMList** parametresi.
1. Çalıştırma **SequencedStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat**, virgülle ayrılmış bir liste VM ekleme *VMList* parametresi ve ardından WHATIF parametresi **True**. Değişikliklerinizi önizleyin.
1. Yapılandırma **External_ExcludeVMNames** Vm'leri (VM1, VM2 ve VM3) virgülle ayrılmış listesiyle birlikte parametresi.
1. Bu senaryo değil dikkate almaz **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyonu zamanlaması oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyonu'nda runbook zamanlama](../automation/automation-schedules.md).
1. Eylem Önizleme ve üretime yönelik Vm'leri uygulamadan önce gerekli değişiklikleri yapın. Ne zaman hazır, el ile izleme-ve-Tanılama/izleme-işlem-groupsrunbook ayarlanmış parametreye yürütülmesinin **False**, veya Otomasyon zamanlama **sıralı StartVM** ve **Sıralı StopVM** otomatik olarak belirlenen zamanlamanızı aşağıdaki çalıştırın.

### <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a>Senaryo 3: CPU kullanımı otomatik olarak temelinde başlatma/durdurma

Bu çözüm, Azure VM'ler, yoğun olmayan dönemlerde gibi saat sonra kullanılmayan değerlendirmek ve işlemci kullanımı olması durumunda otomatik olarak onları kapatılıyor küçüktür %x aboneliğinizde sanal makineleri çalıştırmanın maliyeti yönetmenize yardımcı olabilir.

Varsayılan olarak, önceden yapılandırılmış yüzdesi CPU ölçüm ortalama kullanım %5 olup olmadığını değerlendirmek için ya da daha az çözümüdür. Bu senaryo, aşağıdaki değişkenleri tarafından denetlenir ve varsayılan değerlerin gereksinimlerinizi karşılamıyorsa değiştirilebilir:

- External_AutoStop_MetricName
- External_AutoStop_Threshold
- External_AutoStop_TimeAggregationOperator
- External_AutoStop_TimeWindow

Abonelik ve kaynak grubu karşı önlem hedefleme veya belirli bir liste VM'ler, ancak ikisini birden hedefleyen etkinleştirebilirsiniz.

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Hedef abonelik ve kaynak grubunda karşı durdurma eylemi

1. Yapılandırma **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** VM'ler hedef belirtmek için değişkenleri.
1. Etkinleştirme ve güncelleştirme **Schedule_AutoStop_CreateAlert_Parent** zamanlama.
1. Çalıştırma **AutoStop_CreateAlert_Parent** kümesine eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listesi tarafından hedef

1. Çalıştırma **AutoStop_CreateAlert_Parent** kümesine eylem parametresi ile runbook **Başlat**, virgülle ayrılmış bir liste VM ekleme *VMList* parametresi ve ardından WHATIF parametresi **True**. Değişikliklerinizi önizleyin.
1. Yapılandırma **External_ExcludeVMNames** Vm'leri (VM1, VM2 ve VM3) virgülle ayrılmış listesiyle birlikte parametresi.
1. Bu senaryo değil dikkate almaz **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyonu zamanlaması oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyonu'nda runbook zamanlama](../automation/automation-schedules.md).

CPU kullanımı temelinde sanal makineleri durdurmak için bir zamanlamaya sahip olduğunuza göre bunları başlatmak için aşağıdaki zamanlamalardan birinin etkinleştirmeniz gerekir.

- Hedef abonelik ve kaynak grubu tarafından eylem başlatın. Açıklanan adımları izleyerek [Senaryo 1](#scenario-1-startstop-vms-on-a-schedule) test etmek ve etkinleştirmek için **zamanlanmış StartVM** zamanlamalar.
- Hedef abonelik, kaynak grubu ve etiket eylemi başlatın. Açıklanan adımları izleyerek [Senaryo 2](#scenario-2-startstop-vms-in-sequence-by-using-tags) test etmek ve etkinleştirmek için **sıralı StartVM** zamanlamalar.

## <a name="solution-components"></a>Çözüm bileşenleri

Bu çözüm, başlatma ve kapatma sanal makinelerinizin, iş gereksinimlerinize uyacak şekilde uyarlamak için önceden yapılandırılmış bir runbook'ları, çizelgeler ve günlükleri Azure İzleyici ile tümleştirmesi içerir.

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda bu çözüm tarafından Otomasyon hesabınıza dağıtılan runbook'ları listeler. Runbook koda değişiklik yapmayın. Bunun yerine, yeni işlevsellik için kendi runbook yazma.

> [!IMPORTANT]
> Doğrudan herhangi bir runbook adının "alt" ile çalışmaz.

Tüm üst runbook'ları dahil _WhatIf_ parametresi. Ayarlandığında **True**, _WhatIf_ olmadan çalıştırdığınızda gerçekleşen davranış tam destekleyen runbook alır _WhatIf_ parametresi ve doğru doğrular Vm'leri güncellenmekte Hedeflenen. Bir runbook yalnızca tanımlanmış eylemlerini gerçekleştirir, _WhatIf_ parametrenin ayarlanmış **False**.

|Runbook | Parametreler | Açıklama|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Üst runbook'tan çağrılır. Bu runbook DISPLAYFILTER senaryosu için kaynak başına temelinde uyarılar oluşturur.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: TRUE veya False  | Oluşturur veya hedef abonelik veya kaynak gruplarındaki VM'lerin Azure uyarı kuralları güncelleştirir. <br> VMList: Vm'leri virgülle ayrılmış listesi. Örneğin, _vm1, vm2, vm3_.<br> *WhatIf* çalıştırmadan runbook mantığının doğrular.|
|AutoStop_Disable | Yok | DISPLAYFILTER uyarıları ve varsayılan zamanlama devre dışı bırakır.|
|AutoStop_StopVM_Child | WebHookData | Üst runbook'tan çağrılır. Uyarı kuralları, bu runbook için VM'yi durdurmak çağırın.|
|Bootstrap_Main | Yok | Bir kez webhookURI gibi genellikle Azure Kaynak Yöneticisi'nden erişilebilir olmayan önyükleme yapılandırmaları ayarlamak için kullanılır. Bu runbook başarılı dağıtımdan sonra otomatik olarak kaldırılır.|
|ScheduledStartStop_Child | VMName <br> Eylem: Başlatma veya durdurma <br> ResourceGroupName | Üst runbook'tan çağrılır. Zamanlanmış Durdur için bir başlatma veya durdurma eylemi yürütür.|
|ScheduledStartStop_Parent | Eylem: Başlatma veya durdurma <br>VMList <br> WhatIf: TRUE veya False | Bu ayar, abonelik içindeki tüm sanal makineleri etkiler. Düzen **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** yalnızca bunları yürütmek için kaynak gruplarını hedeflenen. Belirli sanal makineler güncelleştirerek de hariç tutabilirsiniz **External_ExcludeVMNames** değişkeni.<br> VMList: Vm'leri virgülle ayrılmış listesi. Örneğin, _vm1, vm2, vm3_.<br> _WhatIf_ çalıştırmadan runbook mantığının doğrular.|
|SequencedStartStop_Parent | Eylem: Başlatma veya durdurma <br> WhatIf: TRUE veya False<br>VMList| Adlı etiketler oluşturma **sequencestart** ve **sequencestop** dizisi Başlat/Durdur etkinliğinin istediğiniz her VM'de. Bu etiket adları büyük/küçük harfe duyarlıdır. Etiket değeri başlatmak veya durdurmak istediğiniz siparişin karşılık gelen bir pozitif tamsayı (1, 2, 3) olmalıdır. <br> VMList: Vm'leri virgülle ayrılmış listesi. Örneğin, _vm1, vm2, vm3_. <br> _WhatIf_ çalıştırmadan runbook mantığının doğrular. <br> **Not**: VM'ler, Azure Otomasyonu değişken External_Start_ResourceGroupNames External_Stop_ResourceGroupNames ve External_ExcludeVMNames tanımlanmış kaynak grubu içinde olmalıdır. Uygun etiketleri etkili olması eylemler için olmaları gerekir.|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda, Otomasyon hesabınızda oluşturduğunuz değişkenleri listeler. Yalnızca ön ekine sahip değişkenleri değiştirin **dış**. Değişkenleri değiştirme önekiyle **dahili** istenmeyen etkilere neden oluyor.

|Değişken | Açıklama|
|---------|------------|
|External_AutoStop_Condition | Koşullu işleç bir uyarı tetiklemeden önce koşul yapılandırmak için gereklidir. Kabul edilebilir değerler **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, ve **LessThanOrEqual**.|
|External_AutoStop_Description | CPU yüzdesi eşiği aşarsa bir VM'yi durdurmaya yönelik uyarı.|
|External_AutoStop_MetricName | Azure uyarı kuralının yapılandırılması için olduğu performans ölçüm adı.|
|External_AutoStop_Threshold | Eşik değişkeni içinde belirtilen Azure uyarı kuralının _External_AutoStop_MetricName_. Yüzde değerleri 1 ile 100 aralığında değişebilir.|
|External_AutoStop_TimeAggregationOperator | Koşulu değerlendirmek için seçilen pencere boyutuna uygulandığı zaman Toplama işleci. Kabul edilebilir değerler **ortalama**, **Minimum**, **maksimum**, **toplam**, ve **son**.|
|External_AutoStop_TimeWindow | Pencere boyutu bu sırada Azure uyarı tetiklemek için seçilen ölçümleri analiz eder. Bu parametre, giriş timespan biçim olarak kabul eder. Olası değerler şunlardır: 6 saat için 5 dakika.|
|External_EnableClassicVMs| Klasik sanal makineleri çözüm tarafından hedeflenen olup olmadığını belirtir. Varsayılan değer True'dur. Bu müşterilere CSP abonelikleri False olarak ayarlanmalıdır.|
|External_ExcludeVMNames | Hariç tutulacak VM adlarını adları boşluk virgül kullanarak ayırarak girin. Bu, 140 Vm'lere sınırlıdır. 140'tan fazla VM'ler için bu virgülle ayrılmış liste eklerseniz, hariç tutulacak ayarlanmış Vm'leri yanlışlıkla başlatılması veya durdurulmasına.|
|External_Start_ResourceGroupNames | Değerleri başlatma eylemleri için hedeflenen virgülle ayırarak bir veya daha fazla kaynak grupları belirtir.|
|External_Stop_ResourceGroupNames | Değerleri durdurma eylemler için hedeflenen virgülle ayırarak bir veya daha fazla kaynak grubunu belirtir.|
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|
|Internal_AutoSnooze_WebhookUri | Web kancası URI DISPLAYFILTER senaryosu adı verilen belirtir.|
|Internal_AzureSubscriptionId | Azure abonelik kimliğini belirtir.|
|Internal_ResourceGroupName | Otomasyon hesabı kaynak grubu adını belirtir.|

Tüm senaryolar arasında **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, ve **External_ExcludeVMNames** değişkenlerdir gerekli VM'ler için virgülle ayrılmış bir listesini sağlayan hariç olmak üzere sanal makineleri hedeflemek için **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent**, ve  **ScheduledStartStop_Parent** runbook'ları. Diğer bir deyişle, sanal makinelerinizin başlangıç için hedef kaynak grubu içinde bulunan ve bu Durdur eylemlerinin gerçekleşmesi gerekir. Azure İlkesi, hedef abonelik veya kaynak grubu ve eylemlerinde benzer mantıksal çalışır, yeni oluşturulan VM'ler tarafından devralınan. Bu yaklaşım, her VM için ayrı bir zamanlama korumak ve başlar yönetme zorunluluğunu ortadan ve ölçek durdurur.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tabloda her Otomasyon hesabınızda oluşturduğunuz varsayılan zamanlaması listelenmiştir. Bunları değiştirebilir veya kendi özel bir zamanlama oluşturun. Varsayılan olarak, tüm zamanlamalar dışında devre dışıdır **Scheduled_StartVM** ve **Scheduled_StopVM**.

Bu çakışan eylemleri zamanlama oluşturmak için tüm zamanlamalar etkinleştirmemeniz gerekir. Hangi iyileştirmeler gerçekleştirin ve uygun şekilde değiştirmek için istediğiniz belirlemek idealdir. Örnek senaryolar genel bakış bölümünde daha ayrıntılı açıklaması için bkz.

|Zamanlama adı | Sıklık | Açıklama|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | 8 saatte bir | Azure Otomasyonu değişken External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames sanal makine tabanlı değerleri sırayla durdurur AutoStop_CreateAlert_Parent runbook her 8 saatte çalışır. Alternatif olarak, VMList parametresini kullanarak VM'lerin virgülle ayrılmış bir listesini belirtebilirsiniz.|
|Scheduled_StopVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook bir parametreyle çalıştırır _Durdur_ her gün belirtilen zamanda. Varlık değişkenleri tarafından tanımlanmış kurallara uygun olan tüm VM'ler otomatik olarak durdurur. İlgili zamanlamayı etkinleştir **zamanlanmış StartVM**.|
|Scheduled_StartVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook bir parametreyle çalıştırır _Başlat_ her gün belirtilen zamanda. Uygun değişkenleri tarafından tanımlanmış kurallara uygun olan tüm VM'ler otomatik olarak başlar. İlgili zamanlamayı etkinleştir **zamanlanmış StopVM**.|
|Sıralı StopVM | 1: 00'da (UTC), her Cuma | Sequenced_Parent runbook bir parametreyle çalıştırır _Durdur_ her Cuma belirtilen zamanda. Sıralı olarak (artan) durdurur bir etiketi bulunduğu tüm VM'ler **SequenceStop** uygun değişkenleri tarafından tanımlanmış. Etiketi değer ve varlık değişkenleri hakkında daha fazla bilgi için runbook'ları bölümüne bakın. İlgili zamanlamayı etkinleştir **sıralı StartVM**.|
|Sıralı StartVM | 13: 00'te (UTC), her Pazartesi | Sequenced_Parent runbook bir parametreyle çalıştırır _Başlat_ her Pazartesi belirtilen zamanda. Ardışık olarak bir etiketle başlayan tüm sanal makineler (Azalan) **SequenceStart** uygun değişkenleri tarafından tanımlanmış. Etiketi değer ve varlık değişkenleri hakkında daha fazla bilgi için runbook'ları bölümüne bakın. İlgili zamanlamayı etkinleştir **sıralı StopVM**.|

## <a name="azure-monitor-logs-records"></a>Azure İzleyici kayıtları günlüğe kaydeder.

Otomasyon, Log Analytics çalışma alanında iki tür kayıt oluşturur: İş günlükleri ve iş akışları.

### <a name="job-logs"></a>İş günlükleri

|Özellik | Açıklama|
|----------|----------|
|Caller |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Category | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır.|
|CorrelationId | Runbook işinin bağıntı kimliği olan GUID.|
|JobId | Runbook işinin kimliği olan GUID.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer iş olacaktır.|
|resourceId | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin durumudur. Olası değerler şunlardır:<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Başarılı oldu|
|resultDescription | Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı|
|RunbookName | Runbook’un adını belirtir.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer, OpsManager olacaktır|
|StreamType | Olay türünü belirtir. Olası değerler şunlardır:<br>- Ayrıntılı<br>- Çıktı<br>- Hata<br>- Uyarı|
|SubscriptionId | İşin abonelik kimliğini belirtir.
|Time | Runbook işinin yürütüldüğü tarih ve saat.|

### <a name="job-streams"></a>İş akışları

|Özellik | Açıklama|
|----------|----------|
|Caller |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Category | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır.|
|JobId | Runbook işinin kimliği olan GUID.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer iş olacaktır.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|resourceId | Azure'daki kaynak Kimliğini belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin, olayın oluşturulduğu andaki sonucu. Olası bir değerdir:<br>- Devam Ediyor|
|resultDescription | Runbook’un çıktı akışını içerir.|
|RunbookName | Runbook’un adı.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer OpsManager olacaktır.|
|StreamType | İş akışı türü. Olası değerler şunlardır:<br>-İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı|
|Time | Runbook işinin yürütüldüğü tarih ve saat.|

Kategori kayıtlarını döndüren bir günlük araması yaptığınızda **JobLogs** veya **JobStreams**, seçebileceğiniz **JobLogs** veya **JobStreams**görünümü arama tarafından döndürülen güncelleştirmeleri özetleyen bir kutucuk kümesi görüntüler.

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan iş kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

|Sorgu | Açıklama|
|----------|----------|
|Runbook'una ilişkin başarıyla tamamlanmış ScheduledStartStop_Parent Bul | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Runbook'una ilişkin başarıyla tamamlanmış SequencedStartStop_Parent Bul | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="viewing-the-solution"></a>Çözümü görüntüleme

Çözüm erişmek için seçin, bunları Otomasyon hesabınıza gidin **çalışma** altında **ilgili kaynakları**. Log analytics sayfasında **çözümleri** altında **genel**. Üzerinde **çözümleri** sayfasında, çözümü seçin **Başlat-Durdur-VM [çalışma alanı]** listeden.

Çözüme görüntüler **Başlat-Durdur-VM [çalışma alanı]** çözüm sayfasında. Önemli ayrıntıları gibi buradan inceleyebilirsiniz **StartStopVM** Döşe. Log Analytics çalışma olduğu gibi bir sayı ve bir grafik gösterimi başlatılmış ve başarıyla tamamlanmış runbook işlerinin çözümü bu kutucuk görüntüler.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Buradan, halka kutucuğa tıklayarak daha fazla iş kayıtlarıyla analiz gerçekleştirebilirsiniz. Çözüm Panosu, iş geçmişini gösterir ve günlük arama sorguları önceden tanımlanmış. Aramak için log analytics Gelişmiş portalı geçin, arama sorgularına dayalı.

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Çözüm dağıtıldıktan sonra e-posta bildirimlerini değiştirmek için dağıtım sırasında oluşturulan eylem grubunu değiştirin.  

> [!NOTE]
> Azure kamu bulutunda abonelikler, e-posta işlevselliği bu çözümün desteklemez.

Azure portalında izleyicisine gidin -> Eylem grupları. Adlı eylem grubu seçin **StartStop_VM_Notication**.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/azure-monitor.png)

Üzerinde **StartStop_VM_Notification** sayfasında **Ayrıntıları Düzenle** altında **ayrıntıları**. Bu açılır **e-posta/SMS/anında iletme/ses** sayfası. E-posta adresini güncelleştirin ve tıklayın **Tamam** yaptığınız değişiklikleri kaydedin.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/change-email.png)

Alternatif olarak, Eylem grupları hakkında daha fazla bilgi edinmek için bir eylem grubu için ek eylemler bkz ekleyebilirsiniz [Eylem grupları](../azure-monitor/platform/action-groups.md)

Sanal makineleri çözümü kapatır, gönderilen örnek e-posta verilmiştir.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/email.png)

## <a name="add-exclude-vms"></a>Vm'leri ekleme/çıkarma

Çözüm, çözüm tarafından hedeflenen veya özellikle makineler çözümünden çıkarmak için Vm'leri ekleme olanağı sağlar.

### <a name="add-a-vm"></a>Bir VM ekleme

Çalıştığında bir VM başlatma/durdurma çözümde yer alan emin olmak için kullanabileceğiniz birkaç seçenek vardır.

* Her üst [runbook'ları](#runbooks) çözüm sahip bir **VMList** parametresi. Çözüm çalıştığında durumunuzu ve bu VM'ler için uygun üst runbook zamanlama dahil edilecek zaman, bu parametre için VM adlarının virgülle ayrılmış listesini geçirebilirsiniz.

* Birden çok VM seçmek üzere ayarlamak **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** başlatmak veya durdurmak istediğiniz Vm'leri içeren kaynak grubu adları ile. De bu değeri ayarlayabilirsiniz `*`, Abonelikteki tüm kaynak gruplarını çalıştırmanızı çözüm sağlamak için.

### <a name="exclude-a-vm"></a>Bir VM Dışla

Bir VM çözümünden çıkarmak için ona ekleyebilirsiniz **External_ExcludeVMNames** değişkeni. Bu değişken Başlat/Durdur çözümü dışlanacak belirli sanal makineler bir virgülle ayrılmış listesidir. Bu liste, 140 Vm'lere sınırlıdır. 140'tan fazla VM'ler için bu virgülle ayrılmış liste eklerseniz, hariç tutulacak ayarlanmış Vm'leri yanlışlıkla başlatılması veya durdurulmasına.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Başlatma ve kapatma zamanlamalarını değiştirme

Bu çözümde başlatma ve kapatma zamanlamalarını yönetme izleyen aynı adımları açıklandığı şekilde [Azure Otomasyonu'nda runbook zamanlama](automation-schedules.md). Var. başlatmak ve durdurmak için zamanlama ayrı olması gerekir.

Yalnızca belirli bir süre sonunda sanal makineleri durdurmak için çözümü yapılandırma desteklenir. Bu senaryoda, az önce oluşturduğunuz bir **Durdur** zamanlama ve karşılık gelen **Başlat** zamanlanmış. Bunu yapmanız için gerekenler:

1. İçinde kapatılacak şekilde sanal makineler için kaynak gruplarını eklediğinizden emin olun **External_Stop_ResourceGroupNames** değişkeni.
2. Kendi zamanlamada Vm'lerini kapatmak istediğiniz zamanı oluşturun.
3. Gidin **ScheduledStartStop_Parent** runbook tıklatıp **zamanlama**. Bu, önceki adımda oluşturduğunuz zamanlamayı seçmenize olanak sağlar.
4. Seçin **parametreler ve çalıştırma ayarları** ve "Durdur" için eylem parametresini ayarlayın.
5. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="update-the-solution"></a>Güncelleştirme çözümü

Bu çözümün önceki bir sürümünü dağıttıysanız, bu hesabınızdan güncelleştirilmiş bir sürümünü dağıtmadan önce silmelisiniz. Adımlarını izleyin [çözümünü Kaldır](#remove-the-solution) ve ardından yukarıdaki adımlarını izleyin [çözümü dağıtmak](#deploy-the-solution).

## <a name="remove-the-solution"></a>Çözümü Kaldır

Artık çözümü kullanmak gereken karar verirseniz, Otomasyon hesabı silebilirsiniz. Çözüm silindiğinde, yalnızca runbook'ları kaldırır. Çözüm eklendiğinde, oluşturulan değişkenleri ve zamanlamaları silmez. Bu varlıklar, bunları ile diğer runbook'ları kullanmıyorsanız el ile silmeniz gerekir.

Çözümü silmek için aşağıdaki adımları gerçekleştirin:

1. Otomasyon hesabınızdan altında **ilgili kaynakları**seçin **bağlantılı çalışma**.
1. Seçin **çalışma alanına gidin**.
1. Altında **genel**seçin **çözümleri**. 
1. Üzerinde **çözümleri** sayfasında, çözümü seçin **Başlat-Durdur-VM [çalışma alanı]** . Üzerinde **VMManagementSolution [çalışma alanı]** sayfasından menüsünde, select **Sil**.<br><br> ![VM yönetimi çözümü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. İçinde **Sil çözüm** penceresinde çözümü silmek istediğinizi onaylayın.
1. Bilgiler doğrulanır ve çözümün silinmiş, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde. Döndürülürsünüz **çözümleri** Çözüm kaldırma işlemini başladıktan sonra sayfa.

Bu işlemin bir parçası olarak, Log Analytics çalışma alanını ve Otomasyon hesabı silinmez. Log Analytics çalışma alanı korumak istemiyorsanız el ile silmeniz gerekir. Bu, Azure portalından gerçekleştirilebilir:

1. Azure portal giriş ekranından, seçin **Log Analytics çalışma alanları**.
1. Üzerinde **Log Analytics çalışma alanları** sayfasında, çalışma alanını seçin.
1. Seçin **Sil** menüsünden çalışma alanı ayarları sayfasında.

Azure Otomasyonu hesabı bileşenleri korumak istemiyorsanız her el ile silebilirsiniz. Runbook'ları, değişkenler ve çözüm tarafından oluşturulan zamanlamalar listesi için bkz. [çözüm bileşenlerini](#solution-components).

## <a name="next-steps"></a>Sonraki adımlar

- Azure İzleyici günlüklerine ile Otomasyon iş günlüklerini gözden geçirin ve farklı arama sorguları oluşturma hakkında daha fazla bilgi için bkz. [günlük aramaları Azure İzleyici günlüklerine](../log-analytics/log-analytics-log-searches.md).
- Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md).
- Azure İzleyici günlüklerine ve veri toplama kaynakları hakkında daha fazla bilgi için bkz: [Azure depolama verileri toplamaya Azure İzleyici'de genel bakış oturumu](../azure-monitor/platform/collect-azure-metrics-logs.md).
