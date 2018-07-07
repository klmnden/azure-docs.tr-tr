---
title: Başlat/Durdur Vm'leri çalışma saatleri çözümü (Önizleme)
description: Bu VM management çözüm başlar ve Azure Resource Manager sanal makinelerinizi bir zamanlamaya göre durdurur ve Log Analytics'ten proaktif olarak izler.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8675223162527cc5b2bc45dc5521aac07edaf36c
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37908248"
---
# <a name="startstop-vms-during-off-hours-solution-preview-in-azure-automation"></a>Sırasında Azure Otomasyonu (Önizleme) çözümde yoğun olmayan saatlerde Vm'leri başlatma/durdurma

Çözüm başlar ve Azure sanal makinelerinizi kullanıcı tanımlı zamanlamalarda durdurur, Azure Log Analytics aracılığıyla Öngörüler sağlar ve kullanarak isteğe bağlı bir e-postaları gönderen çalışma saatleri dışında Vm'leri başlatma/durdurma [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md). Bu, çoğu senaryo için hem Azure Resource Manager ve klasik Vm'leri destekler.

Bu çözüm, sunucusuz, düşük maliyetli kaynakları kullanarak maliyetlerini azaltmak için isteyen kullanıcılar için bir merkezi olmayan Otomasyon seçeneği sunar. Bu çözüm ile şunları yapabilirsiniz:

* Vm'leri başlatma ve durdurma zamanlayın.
* VM'ler (Klasik VM'ler için desteklenmez) Azure etiketleri kullanarak artan düzende durdurmak ve başlatmak zamanlayın.
* Düşük CPU kullanımına göre Vm'leri otomatik durdurma.

## <a name="prerequisites"></a>Önkoşullar

* Runbook'lar, [Azure Farklı Çalıştır hesabı](automation-create-runas-account.md) ile çalışır. Sona ya da sık değişebilecek bir parola yerine sertifika doğrulaması kullandığından farklı çalıştır hesabı tercih edilen kimlik doğrulama yöntemidir.
* Bu çözüm, Azure Otomasyonu hesabı ile aynı abonelikte olan sanal makineleri yönetir.
* Bu çözüm yalnızca şu Azure bölgelerine dağıtılır: Avustralya Güneydoğu, Kanada Orta, Orta Hindistan, Doğu ABD, Doğu Japonya, Güneydoğu Asya, UK Güney ve Batı Avrupa.

  > [!NOTE]
  > VM zamanlamasını yöneten bir runbook'ların herhangi bir bölgedeki Vm'leri hedefleyebilir.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Vm'leri başlatma/durdurma sırasında yoğun olmayan saatlerde çözüm Otomasyon hesabınıza eklemek için aşağıdaki adımları uygulayın ve ardından çözümü özelleştirmek üzere değişkenlerini yapılandırmak.

1. Azure portalında **Kaynak oluştur**’a tıklayın.
1. Market sayfasını içinde bir anahtar sözcüğü gibi yazın **Başlat** veya **Başlat/Durdur**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Alternatif olarak, bir veya daha fazla sözcüklerden çözüm tam adını yazın ve Enter tuşuna basın. Seçin **Vm'leri başlatma/durdurma [Önizleme] çalışma saatleri dışında** Arama sonuçlarından.
1. İçinde **Vm'leri başlatma/durdurma [Önizleme] çalışma saatleri dışında** sayfasında seçilen çözüm için özet bilgilerini gözden geçirin ve ardından **Oluştur**.

   ![Azure portalına](media/automation-solution-vm-management/azure-portal-01.png)

1. **Çözüm Ekle** sayfası görüntülenir. Çözümü Otomasyon aboneliğinize aktarmadan önce yapılandırmanız istenir.

   ![VM Management Çözüm Ekle sayfası](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

1. Üzerinde **Çözüm Ekle** sayfasında **çalışma**. Otomasyon hesabının bulunduğu Azure aboneliğine bağlı bir Log Analytics çalışma alanını seçin. Bir çalışma alanınız yoksa, seçin **yeni çalışma alanı oluştur**. Üzerinde **OMS çalışma alanı** sayfasında, şunları yapın:
   * Yeni **OMS Çalışma Alanı** için bir ad belirtin.
   * Seçin bir **abonelik** varsayılan seçili uygun değilse açılan listeden seçerek bağlamak için.
   * İçin **kaynak grubu**, yeni bir kaynak grubu oluşturun veya varolan bir tanesini seçin.
   * Bir **Konum** seçin. Şu anda yalnızca mevcut konumlarının **Avustralya Güneydoğu**, **Kanada orta**, **Orta Hindistan**, **Doğu ABD**, **Doğu Japonya**, **Güneydoğu Asya**, **UK Güney**, ve **Batı Avrupa**.
   * Bir **Fiyatlandırma katmanı** seçin. Seçin **GB başına (tek başına)** seçeneği. Log Analytics'e güncelleştirdi [fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/) ve GB başına katman tek seçenektir.

1. Gerekli bilgileri girdikten sonra **OMS çalışma alanı** sayfasında **Oluştur**. Altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüden döndüren size **Çözüm Ekle** işiniz bittiğinde sayfa.
1. Üzerinde **Çözüm Ekle** sayfasında **Otomasyon hesabı**. Yeni bir Log Analytics çalışma alanı oluşturuyorsanız, ilişkili yeni bir Otomasyon hesabı da oluşturmanız gerekir. Seçin **Otomasyon hesabı oluşturma**ve **Otomasyon hesabı Ekle** sayfasında, şu bilgileri sağlayın:
   * **Ad** alanına Otomasyon hesabının adını girin.

    Diğer tüm seçenekler seçili Log Analytics çalışma alanı otomatik olarak doldurulur. Bu seçenekler değiştirilemez. Bu çözüme dahil olan runbook'lar için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır. Tıkladıktan sonra **Tamam**, yapılandırma seçenekleri doğrulanır ve Otomasyon hesabı oluşturulur. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

1. Son olarak, üzerinde **Çözüm Ekle** sayfasında **yapılandırma**. **Parametreleri** sayfası görüntülenir.

   ![Çözüm için parametreleri sayfası](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Burada, sizden istenir:
   * Belirtin **hedef ResourceGroup adları**. Bu çözüm tarafından yönetilecek Vm'leri içeren kaynak grubu adları şunlardır. Birden fazla ad girin ve her (Bu değerler büyük küçük harfe duyarlı değildir) virgül kullanarak ayırın. Abonelikte yer alan tüm kaynak gruplarındaki sanal makineleri hedeflemek istiyorsanız, joker karakter kullanılması desteklenir. Bu değer depolanan **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenleri.
   * Belirtin **VM çıkarma listesini (dize)**. Hedef kaynak grubu bir veya daha fazla sanal makinelerden adıdır. Birden fazla ad girin ve her (Bu değerler büyük küçük harfe duyarlı değildir) virgül kullanarak ayırın. Joker karakter kullanılması desteklenir. Bu değer depolanan **External_ExcludeVMNames** değişkeni.
   * Seçin bir **zamanlama**. Bir yinelenen tarih ve saat için başlatma ve durdurma hedef kaynak gruplarındaki VM'lerin budur. Varsayılan olarak, zamanlama, 30 dakika sonra için yapılandırılır. Başka bir bölge seçmeyi kullanılabilir değil. Çözüm yapılandırdıktan sonra belirli bir saat dilimi için zamanlama yapılandırmak için bkz [başlatma ve kapatma zamanlamasını değiştirme](#modify-the-startup-and-shutdown-schedule).
   * Almaya **e-posta bildirimleri** bir eylem grubundan varsayılan değerini kabul **Evet** ve geçerli bir eposta adresi belirtin. Seçerseniz **Hayır** ancak daha sonraki bir tarihte karar e-posta bildirimleri almak istediğinizi, güncelleştirebilirsiniz [eylem grubu](../monitoring-and-diagnostics/monitoring-action-groups.md) virgülle ayrılmış geçerli e-posta adresi ile oluşturulur.

    > [!IMPORTANT]
    > İçin varsayılan değer **hedef ResourceGroup adları** olduğu bir **&ast;**. Bu, bir Abonelikteki tüm sanal makineler hedefler. Aboneliğinizdeki tüm sanal makineleri hedeflemek için çözüm istemiyorsanız bu değeri zamanlamaları etkinleştirilmeden önce kaynak grubu adları listesine güncelleştirilmesi gerekiyor.

1. Çözüm için gereken ve ilk ayarları yapılandırdıktan sonra tıklayın **Tamam** kapatmak için **parametreleri** sayfasından seçim yapıp **Oluştur**. Tüm ayarlar doğrulandıktan sonra çözümün aboneliğinize dağıtılır. Bu işlemin tamamlanması birkaç saniye sürebilir ve altında ilerleme durumunu izleyebilir **bildirimleri** menüsünde.

## <a name="scenarios"></a>Senaryolar

Çözüm, üç farklı senaryolar içerir. Bu senaryolar şunlardır:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>1. Senaryo: Bir zamanlamaya göre Vm'leri başlatma/durdurma

Bu, çözüm ilk kez dağıttığınızda varsayılan yapılandırmadır. Örneğin, akşam iş çıkıp office geri olduğunuzda sabah başlatmak, bir Abonelikteki tüm sanal makineleri durdurmak için yapılandırabilirsiniz. Zamanlama yapılandırdığınızda **zamanlanmış StartVM** ve **zamanlanmış StopVM** dağıtımı sırasında bunlar başlatıp hedeflenen VM'ler. Bu çözüm, yalnızca sanal makineleri durdurmak için yapılandırma desteklenir, bkz: [başlatma ve kapatma zamanlamalarını değiştirmek](#modify-the-startup-and-shutdown-schedules) özel bir zamanlama yapılandırmak hakkında bilgi edinmek için.

>[!NOTE]
>Zamanlama süre parametresini yapılandırdığınızda saat dilimi geçerli saat diliminizde alınır. Ancak, Azure automation'da UTC biçiminde depolanır. Bu dağıtım sırasında işlendiğinden hiçbir saat dilimi dönüştürmesi yapmak gerekmez.

Aşağıdaki değişkenleri yapılandırarak Vm'leri kapsamda olan denetim: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, ve **External_ ExcludeVMNames**.

Abonelik ve kaynak grubu karşı önlem hedefleme veya belirli bir liste VM'ler, ancak ikisini birden hedefleyen etkinleştirebilirsiniz.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Hedef abonelik ve kaynak grubunda karşı başlatma ve durdurma eylemleri

1. Yapılandırma **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** VM'ler hedef belirtmek için değişkenleri.
2. Etkinleştirme ve güncelleştirme **zamanlanmış StartVM** ve **zamanlanmış StopVM** zamanlamalar.
3. Çalıştırma **ScheduledStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listesi tarafından hedef

1. Çalıştırma **ScheduledStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat**, virgülle ayrılmış bir liste VM ekleme *VMList* parametresi ve ardından WHATIF parametresi **True**. Değişikliklerinizi önizleyin.
2. Yapılandırma **External_ExcludeVMNames** Vm'leri (VM1, VM2 ve VM3) virgülle ayrılmış listesiyle birlikte parametresi.
3. Bu senaryo değil dikkate almaz **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyonu zamanlaması oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyonu'nda runbook zamanlama](../automation/automation-schedules.md).

>[!NOTE]
> Değeri **hedef ResourceGroup adları** değeri olarak hem depolanan **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames**. Daha fazla ayrıntı için her biri farklı kaynak gruplarında hedeflemek için bu değişkenleri değiştirebilir. Kullanmak için başlatma eylemini, **External_Start_ResourceGroupNames**ve durdurma eylemi için **External_Stop_ResourceGroupNames**. VM'ler, başına otomatik olarak eklenir ve zamanlamaları durdurabilirsiniz.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Senaryo 2: Başlat/Durdur VM'leri sıradaki etiketleri kullanarak

Dağıtılmış bir iş yükü destekleyen birden çok VM'de iki veya daha fazla bileşen içeren bir ortamda, sırayla bileşenleri başlatıldığında ve durdurulduğunda dizisi destekleyen önemlidir. Bunu, aşağıdaki adımları uygulayarak gerçekleştirebilirsiniz:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Hedef abonelik ve kaynak grubunda karşı başlatma ve durdurma eylemleri

1. Ekleme bir **SequenceStart** ve **SequenceStop** olarak hedeflenen VM'ler için bir pozitif tamsayı değeri olan etiketi **External_Start_ResourceGroupNames** ve  **External_Stop_ResourceGroupNames** değişkenleri. Başlatma ve durdurma eylemleri, artan sırada gerçekleştirilir. Bir VM'yi etiketleme öğrenmek için bkz. [Azure'da Windows sanal makine etiketleme](../virtual-machines/windows/tag.md) ve [Azure'da bir Linux sanal makinesi etiketleme](../virtual-machines/linux/tag.md).
2. Zamanlamalarını değiştirmek **sıralı StartVM** ve **sıralı StopVM** tarih ve saat gereksinimlerinizi karşılayan ve zamanlamasını etkinleştirmek için.
3. Çalıştırma **SequencedStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.
4. Eylem Önizleme ve üretime yönelik Vm'leri uygulamadan önce gerekli değişiklikleri yapın. Ne zaman hazır, el ile runbook ayarlanmış parametreye yürütülmesinin **False**, veya Otomasyon zamanlama **sıralı StartVM** ve **sıralı StopVM** çalıştırın otomatik olarak belirlenen zamanlamanızı izleyerek.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listesi tarafından hedef

1. Ekleme bir **SequenceStart** ve **SequenceStop** için eklemeyi planladığınız VM'ler için bir pozitif tamsayı değeri olan etiketi **VMList** değişkeni. 
2. Çalıştırma **SequencedStartStop_Parent** kümesine eylem parametresi ile runbook **Başlat**, virgülle ayrılmış bir liste VM ekleme *VMList* parametresi ve ardından WHATIF parametresi **True**. Değişikliklerinizi önizleyin.
3. Yapılandırma **External_ExcludeVMNames** Vm'leri (VM1, VM2 ve VM3) virgülle ayrılmış listesiyle birlikte parametresi.
4. Bu senaryo değil dikkate almaz **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyonu zamanlaması oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyonu'nda runbook zamanlama](../automation/automation-schedules.md).
5. Eylem Önizleme ve üretime yönelik Vm'leri uygulamadan önce gerekli değişiklikleri yapın. Ne zaman hazır, el ile izleme-ve-Tanılama/izleme-işlem-groupsrunbook ayarlanmış parametreye yürütülmesinin **False**, veya Otomasyon zamanlama **sıralı StartVM** ve **Sıralı StopVM** otomatik olarak belirlenen zamanlamanızı aşağıdaki çalıştırın.

### <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a>Senaryo 3: Başlat/Durdur otomatik olarak CPU kullanım oranlarına dayalı

Bu çözüm, Azure VM'ler, yoğun olmayan dönemlerde gibi saat sonra kullanılmayan değerlendirmek ve işlemci kullanımı olması durumunda otomatik olarak onları kapatılıyor küçüktür %x aboneliğinizde sanal makineleri çalıştırmanın maliyeti yönetmenize yardımcı olabilir.

Varsayılan olarak, önceden yapılandırılmış yüzdesi CPU ölçüm ortalama kullanım %5 olup olmadığını değerlendirmek için ya da daha az çözümüdür. Bu, aşağıdaki değişkenleri tarafından denetlenir ve varsayılan değerlerin gereksinimlerinizi karşılamıyorsa değiştirilebilir:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Abonelik ve kaynak grubu karşı önlem hedefleme veya belirli bir liste VM'ler, ancak ikisini birden hedefleyen etkinleştirebilirsiniz.

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Hedef abonelik ve kaynak grubunda karşı durdurma eylemi

1. Yapılandırma **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** VM'ler hedef belirtmek için değişkenleri.
2. Etkinleştirme ve güncelleştirme **Schedule_AutoStop_CreateAlert_Parent** zamanlama.
3. Çalıştırma **AutoStop_CreateAlert_Parent** kümesine eylem parametresi ile runbook **Başlat** ve kümesine WHATIF parametresi **True** değişikliklerinizi önizlemek için.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Başlatma ve durdurma eylemi VM listesi tarafından hedef

1. Çalıştırma **AutoStop_CreateAlert_Parent** kümesine eylem parametresi ile runbook **Başlat**, virgülle ayrılmış bir liste VM ekleme *VMList* parametresi ve ardından WHATIF parametresi **True**. Değişikliklerinizi önizleyin.
2. Yapılandırma **External_ExcludeVMNames** Vm'leri (VM1, VM2 ve VM3) virgülle ayrılmış listesiyle birlikte parametresi.
3. Bu senaryo değil dikkate almaz **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri. Bu senaryo için kendi Otomasyonu zamanlaması oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyonu'nda runbook zamanlama](../automation/automation-schedules.md).

CPU kullanımı temelinde sanal makineleri durdurmak için bir zamanlamaya sahip olduğunuza göre bunları başlatmak için aşağıdaki zamanlamalardan birinin etkinleştirmeniz gerekir.

* Hedef abonelik ve kaynak grubu tarafından eylem başlatın. Açıklanan adımları izleyerek [Senaryo 1](#scenario-1-startstop-vms-on-a-schedule) test etmek ve etkinleştirmek için **zamanlanmış StartVM** zamanlamalar.
* Hedef abonelik, kaynak grubu ve etiket eylemi başlatın. Açıklanan adımları izleyerek [Senaryo 2](#scenario-2-startstop-vms-in-sequence-by-using-tags) test etmek ve etkinleştirmek için **sıralı StartVM** zamanlamalar.

## <a name="solution-components"></a>Çözüm bileşenleri

Bu çözüm, başlatma ve kapatma sanal makinelerinizin, iş gereksinimlerinize uyacak şekilde uyarlamak için önceden yapılandırılmış bir runbook'ları, tabloları ve Log Analytics ile tümleştirme içerir.

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda bu çözüm tarafından Otomasyon hesabınıza dağıtılan runbook'ları listeler. Runbook koda değişiklik yapmamalısınız. Bunun yerine, yeni işlevsellik için kendi runbook yazma.

> [!IMPORTANT]
> Doğrudan herhangi bir runbook adının "alt" ile çalışmaz.

Tüm üst runbook'ları dahil *WhatIf* parametresi. Ayarlandığında **True**, *WhatIf* olmadan çalıştırdığınızda gerçekleşen davranış tam destekleyen runbook alır *WhatIf* parametresi ve doğru doğrular Vm'leri güncellenmekte Hedeflenen. Bir runbook yalnızca tanımlanmış eylemlerini gerçekleştirir, *WhatIf* parametrenin ayarlanmış **False**.

|**Runbook** | **Parametreler** | **Açıklama**|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Üst runbook'tan çağrılır. Bu runbook DISPLAYFILTER senaryosu için kaynak başına temelinde uyarılar oluşturur.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: True veya False  | Oluşturur veya hedef abonelik veya kaynak gruplarındaki VM'lerin Azure uyarı kuralları güncelleştirir. <br> VMList: Virgülle ayrılmış listesi VM'ler. Örneğin, *vm1, vm2, vm3*.<br> *WhatIf* çalıştırmadan runbook mantığının doğrular.|
|AutoStop_Disable | yok | DISPLAYFILTER uyarıları ve varsayılan zamanlama devre dışı bırakır.|
|AutoStop_StopVM_Child | WebHookData | Üst runbook'tan çağrılır. Uyarı kuralları, bu runbook için VM'yi durdurmak çağırın.|
|Bootstrap_Main | yok | Bir kez webhookURI gibi genellikle Azure Kaynak Yöneticisi'nden erişilebilir olmayan önyükleme yapılandırmaları ayarlamak için kullanılır. Bu runbook başarılı dağıtımdan sonra otomatik olarak kaldırılır.|
|ScheduledStartStop_Child | VMName <br> Eylem: Başlatma veya durdurma <br> ResourceGroupName | Üst runbook'tan çağrılır. Zamanlanmış Durdur için bir başlatma veya durdurma eylemi yürütür.|
|ScheduledStartStop_Parent | Eylem: Başlatma veya durdurma <br>VMList <br> WhatIf: True veya False | Bu Abonelikteki tüm sanal makineleri etkiler. Düzen **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** yalnızca bunları yürütmek için kaynak gruplarını hedeflenen. Belirli sanal makineler güncelleştirerek de hariç tutabilirsiniz **External_ExcludeVMNames** değişkeni.<br> VMList: Virgülle ayrılmış listesi VM'ler. Örneğin, *vm1, vm2, vm3*.<br> *WhatIf* çalıştırmadan runbook mantığının doğrular.|
|SequencedStartStop_Parent | Eylem: Başlatma veya durdurma <br> WhatIf: True veya False<br>VMList| Adlı etiketler oluşturma **SequenceStart** ve **SequenceStop** dizisi Başlat/Durdur etkinliğinin istediğiniz her VM'de. Etiket değeri başlatmak veya durdurmak istediğiniz siparişin karşılık gelen bir pozitif tamsayı (1, 2, 3) olmalıdır. <br> VMList: Virgülle ayrılmış listesi VM'ler. Örneğin, *vm1, vm2, vm3*. <br> *WhatIf* çalıştırmadan runbook mantığının doğrular. <br> **Not**: Vm'leri Azure Otomasyonu değişken External_Start_ResourceGroupNames External_Stop_ResourceGroupNames ve External_ExcludeVMNames tanımlanmış kaynak grubu içinde olmalıdır. Uygun etiketleri etkili olması eylemler için olmaları gerekir.|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda, Otomasyon hesabınızda oluşturduğunuz değişkenleri listeler. Yalnızca ön ekine sahip değişken değiştirmelisiniz **dış**. Değişkenleri değiştirme önekiyle **dahili** istenmeyen etkilere neden oluyor.

|**Değişkeni** | **Açıklama**|
---------|------------|
|External_AutoStop_Condition | Koşullu işleç bir uyarı tetiklemeden önce koşul yapılandırmak için gereklidir. Kabul edilebilir değerler **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, ve **LessThanOrEqual**.|
|External_AutoStop_Description | CPU yüzdesi eşiği aşarsa bir VM'yi durdurmaya yönelik uyarı.|
|External_AutoStop_MetricName | Azure uyarı kuralının yapılandırılması için olduğu performans ölçüm adı.|
|External_AutoStop_Threshold | Eşik değişkeni içinde belirtilen Azure uyarı kuralının *External_AutoStop_MetricName*. Yüzde değerleri 1 ile 100 aralığında değişebilir.|
|External_AutoStop_TimeAggregationOperator | Koşulu değerlendirmek için seçilen pencere boyutuna uygulandığı zaman Toplama işleci. Kabul edilebilir değerler **ortalama**, **Minimum**, **maksimum**, **toplam**, ve **son**.|
|External_AutoStop_TimeWindow | Pencere boyutu bu sırada Azure uyarı tetiklemek için seçilen ölçümleri analiz eder. Bu parametre, giriş timespan biçim olarak kabul eder. Olası değerler şunlardır: 6 saat için 5 dakika.|
|External_ExcludeVMNames | Hariç tutulacak VM adlarını adları boşluk virgül kullanarak ayırarak girin.|
|External_Start_ResourceGroupNames | Değerleri başlatma eylemleri için hedeflenen virgülle ayırarak bir veya daha fazla kaynak grupları belirtir.|
|External_Stop_ResourceGroupNames | Değerleri durdurma eylemler için hedeflenen virgülle ayırarak bir veya daha fazla kaynak grubunu belirtir.|
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|
|Internal_AutoSnooze_WebhookUri | Web kancası URI DISPLAYFILTER senaryosu adı verilen belirtir.|
|Internal_AzureSubscriptionId | Azure abonelik kimliğini belirtir.|
|Internal_ResourceGroupName | Otomasyon hesabı kaynak grubu adını belirtir.|

Tüm senaryolar arasında **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, ve **External_ExcludeVMNames** değişkenlerdir gerekli VM'ler için virgülle ayrılmış bir listesini sağlayan hariç olmak üzere sanal makineleri hedeflemek için **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent**, ve  **ScheduledStartStop_Parent** runbook'ları. Diğer bir deyişle, sanal makinelerinizin başlangıç için hedef kaynak grubu içinde bulunan ve bu Durdur eylemlerinin gerçekleşmesi gerekir. Azure İlkesi, hedef abonelik veya kaynak grubu ve eylemlerinde benzer mantıksal çalışır, yeni oluşturulan VM'ler tarafından devralınan. Bu yaklaşım, her VM için ayrı bir zamanlama korumak ve başlar yönetme zorunluluğunu ortadan ve ölçek durdurur.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tabloda her Otomasyon hesabınızda oluşturduğunuz varsayılan zamanlaması listelenmiştir. Bunları değiştirebilir veya kendi özel bir zamanlama oluşturun. Varsayılan olarak, her birini devre dışı bırakıldı dışında **Scheduled_StartVM** ve **Scheduled_StopVM**.

Bu çakışan eylemleri zamanlama oluşturmak için tüm zamanlamalar etkinleştirmemeniz gerekir. Hangi iyileştirmeler gerçekleştirin ve uygun şekilde değiştirmek için istediğiniz belirlemek idealdir. Örnek senaryolar genel bakış bölümünde daha ayrıntılı açıklaması için bkz.

|**Zamanlama adı** | **Sıklık** | **Açıklama**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | 8 saatte bir | Azure Otomasyonu değişken External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames sanal makine tabanlı değerleri sırayla durdurur AutoStop_CreateAlert_Parent runbook her 8 saatte çalışır. Alternatif olarak, VMList parametresini kullanarak VM'lerin virgülle ayrılmış bir listesini belirtebilirsiniz.|
|Scheduled_StopVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook bir parametreyle çalıştırır *Durdur* her gün belirtilen zamanda. Varlık değişkenleri tarafından tanımlanmış kurallara uygun olan tüm VM'ler otomatik olarak durdurur. İlgili bir zamanlama etkinleştirmelisiniz **zamanlanmış StartVM**.|
|Scheduled_StartVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook bir parametreyle çalıştırır *Başlat* her gün belirtilen zamanda. Uygun değişkenleri tarafından tanımlanmış kurallara uygun olan tüm VM'ler otomatik olarak başlar. İlgili bir zamanlama etkinleştirmelisiniz **zamanlanmış StopVM**.|
|Sıralı StopVM | 1: 00'da (UTC), her Cuma | Sequenced_Parent runbook bir parametreyle çalıştırır *Durdur* her Cuma belirtilen zamanda. Sıralı olarak (artan) durdurur bir etiketi bulunduğu tüm VM'ler **SequenceStop** uygun değişkenleri tarafından tanımlanmış. Etiket değerlerini ve varlık değişkenleri hakkında daha fazla ayrıntı için runbook'ları bölümüne bakın. İlgili bir zamanlama etkinleştirmelisiniz **sıralı StartVM**.|
|Sıralı StartVM | 13: 00'te (UTC), her Pazartesi | Sequenced_Parent runbook bir parametreyle çalıştırır *Başlat* her Pazartesi belirtilen zamanda. Ardışık olarak bir etiketle başlayan tüm sanal makineler (Azalan) **SequenceStart** uygun değişkenleri tarafından tanımlanmış. Etiket değerlerini ve varlık değişkenleri hakkında daha fazla ayrıntı için runbook'ları bölümüne bakın. İlgili bir zamanlama etkinleştirmelisiniz **sıralı StopVM**.|

## <a name="log-analytics-records"></a>Log Analytics kayıtları

Otomasyon, Log Analytics çalışma alanında iki tür kayıt oluşturur: İş günlükleri ve iş akışları.

### <a name="job-logs"></a>İş günlükleri

Özellik | Açıklama|
----------|----------|
Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır.|
CorrelationId | Runbook işinin bağıntı kimliği olan GUID.|
JobId | Runbook işinin kimliği olan GUID.|
operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer iş olacaktır.|
resourceId | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
resultType | Runbook işinin durumudur. Olası değerler şunlardır:<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Başarılı oldu|
resultDescription | Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı|
RunbookName | Runbook’un adını belirtir.|
SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer, OpsManager olacaktır|
StreamType | Olay türünü belirtir. Olası değerler şunlardır:<br>- Ayrıntılı<br>- Çıktı<br>- Hata<br>- Uyarı|
SubscriptionId | İşin abonelik kimliğini belirtir.
Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

### <a name="job-streams"></a>İş akışları

Özellik | Açıklama|
----------|----------|
Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır.|
JobId | Runbook işinin kimliği olan GUID.|
operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer iş olacaktır.|
ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
resourceId | Azure'daki kaynak Kimliğini belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
resultType | Runbook işinin, olayın oluşturulduğu andaki sonucu. Olası bir değerdir:<br>- Devam Ediyor|
resultDescription | Runbook’un çıktı akışını içerir.|
RunbookName | Runbook’un adı.|
SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer OpsManager olacaktır.|
StreamType | İş akışı türü. Olası değerler şunlardır:<br>-İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı|
Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

Kategori kayıtlarını döndüren bir günlük araması yaptığınızda **JobLogs** veya **JobStreams**, seçebileceğiniz **JobLogs** veya **JobStreams**görünümü arama tarafından döndürülen güncelleştirmeleri özetleyen bir kutucuk kümesi görüntüler.

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan iş kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

Sorgu | Açıklama|
----------|----------|
Runbook'una ilişkin başarıyla tamamlanmış ScheduledStartStop_Parent Bul | Kategori arama "JobLogs" == &#124; nerede (RunbookName_s "ScheduledStartStop_Parent" ==) &#124; nerede (resulttype'ı "Completed" ==) &#124; Summarize aggregatedvalue = count() resulttype'ı, bin (TimeGenerated, 1 saat) tarafından &#124; TimeGenerated göre sırala desc|
Runbook'una ilişkin başarıyla tamamlanmış SequencedStartStop_Parent Bul | Kategori arama "JobLogs" == &#124; nerede (RunbookName_s "SequencedStartStop_Parent" ==) &#124; nerede (resulttype'ı "Completed" ==) &#124; Summarize aggregatedvalue = count() resulttype'ı, bin (TimeGenerated, 1 saat) tarafından &#124; TimeGenerated göre sırala desc

## <a name="viewing-the-solution"></a>Çözümü görüntüleme

Çözüm erişmek için seçin, bunları Otomasyon hesabınıza gidin **çalışma** altında **ilgili kaynakları**. Log Analytics sayfasında **çözümleri** altında **genel**. Üzerinde **çözümleri** sayfasında, çözümü seçin **Başlat-Durdur-VM [çalışma alanı]** listeden.

Çözüme görüntüler **Başlat-Durdur-VM [çalışma alanı]** çözüm sayfasında. Önemli ayrıntıları gibi buradan inceleyebilirsiniz **StartStopVM** Döşe. Log Analytics çalışma olduğu gibi bir sayı ve bir grafik gösterimi başlatılmış ve başarıyla tamamlanmış runbook işlerinin çözümü bu kutucuk görüntüler.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Buradan, halka kutucuğa tıklayarak daha fazla iş kayıtlarıyla analiz gerçekleştirebilirsiniz. Çözüm Panosu, iş geçmişini gösterir ve günlük arama sorguları önceden tanımlanmış. Aramak için günlük Advanced Analytics portalına geçiş, arama sorgularına dayalı.

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Çözüm dağıtıldıktan sonra e-posta bildirimlerini değiştirmek için dağıtım sırasında oluşturulan eylem grubunu değiştirin.  

Azure portalında izleyicisine gidin -> Eylem grupları. Adlı eylem grubu seçin **StartStop_VM_Notication**.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/azure-monitor.png)

Üzerinde **StartStop_VM_Notification** sayfasında **Ayrıntıları Düzenle** altında **ayrıntıları**. Bu açılır **e-posta/SMS/anında iletme/ses** sayfası. E-posta adresini güncelleştirin ve tıklayın **Tamam** yaptığınız değişiklikleri kaydedin.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/change-email.png)

Alternatif olarak, Eylem grupları hakkında daha fazla bilgi edinmek için bir eylem grubu için ek eylemler bkz ekleyebilirsiniz [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md)

Sanal makineleri çözümü kapatır, gönderilen örnek e-posta verilmiştir.

![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/email.png)

## <a name="modify-the-startup-and-shutdown-schedules"></a>Başlatma ve kapatma zamanlamalarını değiştirme

Bu çözümde başlatma ve kapatma zamanlamalarını yönetme izleyen aynı adımları açıklandığı şekilde [Azure Otomasyonu'nda runbook zamanlama](automation-schedules.md).

Yalnızca belirli bir süre sonunda sanal makineleri durdurmak için çözümü yapılandırma desteklenir. Bunu yapmanız için gerekenler:

1. İçinde kapatılacak şekilde sanal makineler için kaynak gruplarını eklediğinizden emin olun **External_Start_ResourceGroupNames** değişkeni.
2. Kendi zamanlamada Vm'lerini kapatmak istediğiniz zamanı oluşturun.
3. Gidin **ScheduledStartStop_Parent** runbook tıklatıp **zamanlama**. Bu, önceki adımda oluşturduğunuz zamanlamayı seçmenize olanak sağlar.
4. Seçin **parametreler ve çalıştırma ayarları** ve "Durdur" için eylem parametresini ayarlayın.
5. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="update-the-solution"></a>Güncelleştirme çözümü

Bu çözümün önceki bir sürümünü dağıttıysanız, bu hesabınızdan güncelleştirilmiş bir sürümünü dağıtmadan önce silmelisiniz. Adımlarını izleyin [çözümünü Kaldır](#remove-the-solution) ve ardından yukarıdaki adımlarını izleyin [çözümü dağıtmak](#deploy-the-solution).

## <a name="remove-the-solution"></a>Çözümü Kaldır

Artık çözümü kullanmak gereken karar verirseniz, Otomasyon hesabı silebilirsiniz. Çözüm silindiğinde, yalnızca runbook'ları kaldırır. Çözüm eklendiğinde, oluşturulan değişkenleri ve zamanlamaları silmez. Bu varlıklar, bunları ile diğer runbook'ları kullanmıyorsanız el ile silmeniz gerekir.

Çözümü silmek için aşağıdaki adımları gerçekleştirin:

1. Otomasyon hesabınızdan seçin **çalışma** sol sayfasından.
1. Üzerinde **çözümleri** sayfasında, çözümü seçin **Başlat-Durdur-VM [çalışma alanı]**. Üzerinde **VMManagementSolution [çalışma alanı]** sayfasından menüsünde, select **Sil**.<br><br> ![VM yönetimi çözümü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. İçinde **Sil çözüm** penceresinde çözümü silmek istediğinizi onaylayın.
1. Bilgiler doğrulanır ve çözümün silinmiş, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde. Döndürülürsünüz **çözümleri** Çözüm kaldırma işlemini başladıktan sonra sayfa.

Bu işlemin bir parçası olarak, Log Analytics çalışma alanını ve Otomasyon hesabı silinmez. Log Analytics çalışma alanı korumak istemiyorsanız el ile silmeniz gerekir. Bu, Azure portalından gerçekleştirilebilir:

1. Azure portal giriş ekranından, seçin **Log Analytics**.
1. Üzerinde **Log Analytics** sayfasında, çalışma alanını seçin.
1. Seçin **Sil** menüsünden çalışma alanı ayarları sayfasında.

## <a name="next-steps"></a>Sonraki adımlar

* Farklı arama sorguları oluşturma ve Log Analytics ile Otomasyon iş günlüklerini gözden geçirme hakkında daha fazla bilgi için bkz. [Log Analytics'te günlük aramaları](../log-analytics/log-analytics-log-searches.md).
* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md).
* Log Analytics ve veri toplama kaynakları hakkında daha fazla bilgi için bkz: [Azure depolama verileri toplamaya Log Analytics'e genel bakış](../log-analytics/log-analytics-azure-storage.md).
