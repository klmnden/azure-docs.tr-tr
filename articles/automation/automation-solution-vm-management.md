---
title: "Yoğun olmayan saatlerde çözüm sırasında Başlat/Durdur VM'ler | Microsoft Docs"
description: "VM Management çözümleri, Azure Resource Manager Sanal Makinelerinizi bir zamanlama dahilinde başlatıp durdurmanın yanı sıra Log Analytics aracılığıyla izler."
services: automation
documentationCenter: 
authors: eslesar
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: magoedte
ms.openlocfilehash: 2ff2208f62c24c460c9d17533e28fd007549828b
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Azure Otomasyonu yoğun olmayan saatlerde çözümde sırasında Başlat/Durdur VM'ler

Çözüm başlar ve kullanıcı tanımlı zamanlamada Azure sanal makinelerinizi durdurur, günlük analizi aracılığıyla Öngörüler sağlar ve yararlanarak isteğe bağlı bir e-posta gönderir yoğun olmayan saatlerde sırasında Başlat/Durdur VM'ler [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). Azure Resource Manager ve klasik sanal makineleri için çoğu senaryoları destekler. 

Bu çözüm sunucusuz, düşük maliyetli kaynakları yararlanarak bunların maliyetlerini azaltmak için isteyen müşteriler için bir protokoldür Otomasyon yeteneği sağlar. Şu özellikler mevcuttur:

* Başlat/Durdur zamanlama VM'ler
* VM'ler Başlat/Durdur Azure (Klasik sanal makineleri için desteklenmez) etiketleri kullanarak artan düzende zamanlama
* Düşük CPU kullanımı VM'ler tabanlı otomatik durdurma

## <a name="prerequisites"></a>Ön koşullar

- Runbook'lar, [Azure Farklı Çalıştır hesabı](automation-offering-get-started.md#authentication-methods) ile çalışır.  Süresi dolabilecek ya da sık değişebilecek olan bir parola yerine sertifika doğrulaması kullandığından Farklı Çalıştır hesabı, tercih edilen kimlik doğrulama yöntemidir.  

- Bu çözüm yalnızca Otomasyon hesabının bulunduğu olarak aynı abonelikte bulunan sanal makineleri yönetebilirsiniz.  

- Bu çözüm yalnızca aşağıdaki Azure bölgeleri için - Avustralya Güneydoğu, Kanada Merkezi, Orta Hindistan, Doğu ABD, Japonya Doğu, Güneydoğu Asya, Birleşik Krallık Güney ve Batı Avrupa dağıtır.  
    
    > [!NOTE]
    > VM zamanlama yönetme runbook'ları tüm bölgelerdeki VM'ler hedefleyebilirsiniz.  

- Azure Marketi'nden ekleme sırasında başlatma ve durdurma VM runbook'lar tamamladığınızda, e-posta bildirimleri göndermek için Seç seçin **Evet** SendGrid dağıtmak için. 

    > [!IMPORTANT]
    > SendGrid bir üçüncü taraf hizmeti, SendGrid ile ilgili destek için lütfen başvurun [SendGrid](https://sendgrid.com/contact/).  
    >
   
    SendGrid kısıtlamalarla şunlardır:

    * En fazla kullanıcı abonelik başına bir SendGrid hesabı
    * İki SendGrid hesap başına abonelik sayısı

Bu çözüm önceki bir sürümünü dağıttıysanız, önce bu sürüm dağıtmadan önce hesabınızdan silmeniz gerekir.  

## <a name="solution-components"></a>Çözüm bileşenleri

Bu çözüm önceden yapılandırılmış runbook'ları, zamanlama ve işinizi sağlayan başlatma ve kapatma sanal makinelerinizin paketine uyarlamak için günlük analizi ile tümleştirme içerir gerekiyor. 

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda, Automation hesabınız için dağıtılan runbook'ları listeler.  Runbook kodda değişiklik, ancak bunun yerine yeni işlevsellik için kendi runbook yazma önerilmez.

> [!NOTE]
> Doğrudan "Alt" buna ait adının sonuna eklenen ada sahip herhangi bir runbook çalışmıyor.
>

Tüm üst runbook'ları içeren *whatIf* parametresi, hangi olduğunda kümesine **True**, runbook alır olmadan çalıştırdığınızda tam davranışı ayrıntılı destekler *whatIf* parametre ve doğru doğrular VMs hedeflenen.  Runbook'ları yalnızca kendi tanımlanan eylemleri gerçekleştirir, *whatIf* parametrenin ayarlanmış **False**. 

|**Runbook** | **Parametreler** | **Açıklama**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Yalnızca üst runbook'tan çağrılır. Kaynak başına temelinde DISPLAYFILTER senaryosu için uyarılar oluşturur.| 
|AutoStop_CreateAlert_Parent | WhatIf: True veya False <br> VMList | Oluşturur veya vm'lerinde Azure uyarı kuralları hedeflenen abonelik veya kaynak gruplarındaki güncelleştirir. <br> VMList: VM'lerin listesini virgülle ayrılmış.  Örneğin, *vm1, vm2, vm3*| 
|AutoStop_Disable | yok | DISPLAYFILTER uyarılar ve varsayılan zamanlama devre dışı bırakın.| 
|AutoStop_StopVM_Child | WebHookData | Yalnızca üst runbook'tan çağrılır. Uyarı kuralları bu runbook'u çağırmak ve VM durdurma işlemlerini yapar.|  
|Bootstrap_Main | yok | Kurulum önyükleme yapılandırmalara genellikle Azure Kaynak Yöneticisi'nden erişilebilir olmayan webhookURI gibi bir kez kullanılır. Dağıtım başarıyla geçti, bu runbook otomatik olarak kaldırılacak.|  
|ScheduledStartStop_Child | VMName <br> Eylem: Durdurmak veya başlatmak <br> resourceGroupName | Yalnızca üst runbook'tan çağrılır. Durdurma veya başlatma zamanlanmış durdurma için gerçek yürütülmesi yapar.|  
|ScheduledStartStop_Parent | Eylem: Durdurmak veya başlatmak <br> WhatIf: True veya False | Düzenleme sürece bu Abonelikteki tüm VM'ler geçerlilik kazanacaktır **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** hangi kısıtlamak yalnızca bunlar üzerinde çalıştırmak için Kaynak gruplarını hedefleyin. Belirli sanal makineleri güncelleştirerek de hariç tutabilirsiniz **External_ExcludeVMNames** değişkeni. WhatIf diğer runbook'lar olduğu gibi aynı şekilde davranır.|  
|SequencedStartStop_Parent | Eylem: Durdurmak veya başlatmak <br> WhatIf: True veya False | Adlı bir etiket oluşturmak **SequenceStart** ve adlı başka bir etiket **SequenceStop** dizisi başına istediğiniz her bir VM üzerinde\\durdurmak için etkinlik. Etiket değeri başlatmak istiyor sipariş karşılık gelen pozitif bir tamsayı (1,2,3) olmalıdır\\artan düzende dur. WhatIf diğer runbook'lar olduğu gibi aynı şekilde davranır. <br> **Not: VM'ler gerekir kaynak grupları içinde tanımlı External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames Azure Otomasyon değişkenleri ve etkili olması eylemler için uygun etiketler olmalıdır.**|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda, Automation hesabında oluşturulan değişkenleri listesi.  Önekine sahip değişkenler yalnızca değiştirmeniz önerilir **dış**, değişkenleri değiştirme önekiyle **dahili** istenmeyen etkileri neden olur.  

|**Değişken** | **Açıklama**|
---------|------------|
|External_AutoStop_Condition | Bu, bir uyarı tetiklemeden önce koşul yapılandırmak için gerekli koşullu işlecidir. Kabul edilebilir değerler **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, **LessThanOrEqual**.|  
|External_AutoStop_Description | CPU % eşiği aşarsa VM durdurmak için uyarı.|  
|External_AutoStop_MetricName | Azure uyarı kuralı performans ölçümü için yapılandırılacak adıdır.| 
|External_AutoStop_Threshold | Eşik değişkeninde belirtilen Azure uyarı kuralı için *External_AutoStop_MetricName*. Yüzde değerleri 1 ile 100 arasında olabilir.|  
|External_AutoStop_TimeAggregationOperator | Koşulu değerlendirmek için seçilen pencere boyutu uygulanacak zaman Toplama işleci. Kabul edilebilir değerler **ortalama**, **Minimum**, **maksimum**, **toplam**, **son**.|  
|External_AutoStop_TimeWindow | Bir uyarıyı tetiklemek için seçilen ölçüm analiz eder Azure pencere boyutu. Bu parametre timespan biçim girişinde kabul eder. Olası değerler için altı saat beş dakika arasında olur.|  
|External_EmailFromAddress | Gönderenin e-postanın belirtir.|  
|External_EmailSubject | E-postanın konu satırı metnini belirtir.|  
|External_EmailToAddress | E-postanın alıcılarını belirtir. Adları virgül kullanarak ayırın.|  
|External_ExcludeVMNames | Hariç tutulacak VM adları, boşluk virgül kullanarak adları ayırarak girin.|  
|External_IsSendEmail | İşlem tamamlandıktan sonra e-posta bildirimi gönder seçeneği belirtir.  Belirtin **Evet** veya **Hayır** e-posta göndermek için.  Bu seçenek olmalıdır **Hayır** ilk dağıtım sırasında SendGrid oluşturmadıysanız.|  
|External_Start_ResourceGroupNames | Başlangıç eylemler için hedeflenen virgül kullanarak değerleri ayırarak bir veya daha fazla kaynak grupları belirtir.|  
|External_Stop_ResourceGroupNames | Bir veya değerleri Dur eylemler için hedeflenen virgülle ayırarak, daha fazla kaynak grupları belirtir.|  
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|  
|Internal_AutoSnooze_WebhookUri | Web kancası URI DISPLAYFILTER senaryo için adlı belirtir.|  
|Internal_AzureSubscriptionId | Azure abonelik kimliğini belirtir.|  
|Internal_ResourceGroupName | Azure Otomasyonu hesabı kaynak grubu adı belirtir.|  
|Internal_SendGridAccountName | SendGrid hesap adını belirtir.|  
|Internal_SendGridPassword | SendGrid hesabının parolasını belirtir.|  

<br>

Tüm senaryolar arasında **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, ve **External_ExcludeVMNames** değişkenlerdir gerekli virgül sağlama hariç olmak üzere sanal makineleri hedeflemek için ayrılmış VM'ler için listesi **AutoStop_CreateAlert_Parent** runbook. Diğer bir deyişle, Vm'leriniz gerçekleşmesi için hedef kaynak grubunda Başlat/Durdur eylemler için bulunmalıdır. Hedef abonelik veya kaynak grubu ve Eylemler sahip mantığı biraz Azure ilke gibi çalışır yeni oluşturulan VM'ler tarafından devralınır. Bu yaklaşım, her VM için ayrı bir zamanlama korumak ve Başlat/Durdur ölçeğinde yönetme gereğini ortadan kaldırır.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tabloda her Otomasyon hesabınızda oluşturulan varsayılan zamanlamalar listeler.  Bunları değiştirebilir veya kendi özel zamanlamalar oluşturabilirsiniz.  Varsayılan olarak her bu zamanlamaları devre dışı bırakılır dışında **Scheduled_StartVM** ve **zamanlanmış StopVM**.

Bu hangi zamanlama bir eylem gerçekleştiren bir çakışma olarak çok oluşturmak gibi tüm zamanlamalar, etkinleştirmek için önerilmez.  Bunun yerine, gerçekleştirmek ve buna uygun olarak değiştirmek istediğinizden hangi iyileştirmeleri belirlemek en iyi olacaktır.  Örnek senaryolar genel bakış bölümünde daha fazla açıklama için bkz.   

|**ScheduleName** | **Sıklık** | **Açıklama**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Her sekiz saatte çalışır | AutoStop_CreateAlert_Parent runbook 8 hangi sırayla VM'in Azure Automation değişkenlerine "External_Start_ResourceGroupNames", "External_Stop_ResourceGroupNames" ve "External_ExcludeVMNames" tabanlı değerleri durdurur saatte çalışır.  Alternatif olarak, virgülle ayrılmış listesini "VMList" parametresi kullanılarak VM'ler belirtebilirsiniz.|  
|Scheduled_StopVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook "Durdur" parametresiyle her gün belirtilen zamanda çalışır.  Varlık değişkenleri tanımlanan kurallara uyan tüm sanal makinenin otomatik olarak durur.  Kardeş etkinleştirilmesi önerilir zamanlaması, zamanlanmış-StartVM.|  
|Scheduled_StartVM | Kullanıcı tanımlı, her gün | Scheduled_Parent runbook bir parametreyle çalıştırır *Başlat* her gün belirtilen zamanda.  Tüm tanımlanan kurallara uyan VM'in ve uygun değişkenleri parçası otomatik olarak başlatılacak.  Kardeş etkinleştirmeniz önerilir zamanlama **zamanlanmış StopVM**.|
|Sıralı StopVM | 1: 00'da (UTC), her Cuma | Sequenced_Parent runbook bir parametreyle çalıştırır *durdurmak* anda her Cuma.  Sıralı olarak olacak tüm VM kullanıcının bir etiketle (artan) durdurma **SequenceStop** tanımlı ve uygun değişkenleri parçası.  Etiket değerlerini ve varlık değişkenleri hakkında daha fazla ayrıntı için runbook'ları bölümüne bakın.  Olan önerilir kardeş etkinleştirmek zamanlama **sıralı-StartVM**.|
|Sıralı StartVM | 1:00 PM (UTC), her Pazartesi | Sequenced_Parent runbook bir parametreyle çalıştırır *Başlat* belirtilen saatte her Pazartesi.  Sıralı olarak olacak tüm VM kullanıcının bir etiketle (Azalan) Başlangıç **SequenceStart** tanımlı ve uygun değişkenleri parçası.  Etiket değerlerini ve varlık değişkenleri hakkında daha fazla ayrıntı için runbook'ları bölümüne bakın.  Kardeş etkinleştirmeniz önerilir zamanlama **sıralı StopVM**.|

<br>

## <a name="configuration"></a>Yapılandırma

VM’leri çalışma saatleri dışında Başlatma/Durdurma [Önizleme] çözümünü Otomasyonu hesabınızı ekleyip ardından çözümü özelleştirmek üzere değişkenlerini yapılandırmak için aşağıdaki adımları uygulayın.

1. Azure portalda **Yeni**’ye tıklayın.<br> ![Azure portal](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. Market bölmesinde yazın **VM Başlat**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Başlat/Durdur VM'ler yoğun olmayan saatlerde [Önizleme] sırasında** Arama sonuçlarından.  
3. İçinde **Başlat/Durdur VM'ler yoğun olmayan saatlerde [Önizleme] sırasında** bölmesinde seçilen çözümü için özet bilgilerini inceleyin ve ardından **oluşturma**.  
4. **Çözüm Ekle** bölmesinde görünür burada Otomasyon aboneliğinizi içeri aktarmadan önce çözümünüzü yapılandırma istenir.<br><br> ![VM Management Çözüm Ekleme dikey penceresi](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  Üzerinde **Çözüm Ekle** dikey penceresinde, select **çalışma** ve burada Otomasyon hesabının bulunduğu aynı Azure aboneliğine bağlı ya da yeni bir çalışma alanı oluşturan bir OMS çalışma alanını seçin.  Bir çalışma alanı yoksa, seçebileceğiniz **yeni çalışma alanı oluştur** ve **OMS çalışma** bölmesinde aşağıdakileri gerçekleştirin: 
   - Yeni **OMS Çalışma Alanı** için bir ad belirtin.
   - Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   - **Kaynak Grubu** için, yeni bir kaynak grubu oluşturabilir veya mevcut bir kaynak grubunu seçebilirsiniz.  
   - Bir **Konum** seçin.  Geçerli seçim için sağlanan yalnızca konumlarının **Avustralya Güneydoğu**, **Kanada Merkezi**, **Orta Hindistan**, **Doğu ABD**, **Doğu Japonya**, **Güneydoğu Asya**, **Birleşik Krallık Güney**, ve **Batı Avrupa**.
   - Bir **Fiyatlandırma katmanı** seçin.  Çözüm iki katmanda sunulur: ücretsiz ve OMS ücretli katmanı.  Ücretsiz katmanında günlük toplanan veri miktarı, elde tutma süresi ve runbook işi çalışma zamanı dakika sayısına ilişkin sınırlar vardır.  OMS ücretli katmanında günlük toplanan veri miktarı için bir sınır yoktur.  

        > [!NOTE]
        > İsteğe bağlı olarak her GB (tek başına)'i Ücretli katmanı görüntülenirken, geçerli değildir.  Bu katmanı seçerek aboneliğinizde bu çözümü oluşturmaya çalışırsanız, işlem başarısız olur.  Bu çözüm resmi olarak yayımlandığında, bu sorun da ele alınacaktır.<br>Bu çözümü kullanırsanız, yalnızca otomasyon işi dakikaları ve günlük alımı kullanılır.  Çözüm, ortamınıza ek OMS düğümleri eklemez.  

6. **OMS çalışma alanı** dikey penceresinde gerekli bilgileri girdikten sonra **Oluştur**’a tıklayın.  Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.  **Çözüm Ekle** dikey penceresine döndürülürsünüz.  
7. **Çözüm Ekle** dikey penceresinde **Otomasyon Hesabı**’nı seçin.  Yeni bir OMS çalışma alanı oluşturuyorsanız Azure aboneliğiniz, kaynak grubunuz ve bölgeniz dahil olmak üzere belirtilen yeni OMS çalışma alanı ile ilişkilendirilecek olan yeni bir Otomasyon hesabı da oluşturmanız gerekir.  **Otomasyon hesabı oluştur**’u seçerek, **Otomasyon hesabı ekle** dikey penceresinde aşağıdakileri girebilirsiniz: 
  - **Ad** alanına Otomasyon hesabının adını girin.

    Tüm diğer seçenekler, seçili OMS çalışma alanına dayalı olarak otomatik doldurulur ve bu seçenekler değiştirilemez.  Bu çözüme dahil olan runbook'lar için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır.  **Tamam**’a tıkladığınızda yapılandırma seçenekleri doğrulanır ve Otomasyon hesabı oluşturulur.  Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

    Aksi takdirde, mevcut bir Otomasyon Farklı Çalıştır hesabını seçebilirsiniz.  Seçtiğiniz hesap önceden başka bir OMS çalışma alanına bağlı olamaz, böyle olması durumunda dikey pencerede bunu belirten bir ileti görürsünüz.  Önceden bağlıysa, farklı bir Otomasyon Farklı Çalıştır hesabı seçmeniz veya yeni bir hesap oluşturmanız gerekir.<br><br> ![Otomasyon Hesabı Önceden OMS Çalışma Alanına Bağlı](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Son olarak, **Çözüm Ekle** dikey penceresinde **Yapılandırma**’yı seçin. Böylece **Parametreler** dikey penceresi görüntülenir.<br><br> ![Parametreler bölmesini çözümü](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  **Parametreler** dikey penceresinde aşağıdakileri yapmanız istenir:  
   - Bu çözüm tarafından yönetilecek VM’leri içeren bir kaynak grubu adı olan **Hedef ResourceGroup Adları**’nı belirtin.  Birden fazla ad girin ve her (değerleri büyük küçük harfe duyarlı değildir) bir virgül kullanarak ayırın.  Abonelikte yer alan tüm kaynak gruplarındaki sanal makineleri hedeflemek istiyorsanız, joker karakter kullanılması desteklenir.
   - Specity **VM dışlama listesi (dize)**, üzerinde adını veya daha fazla sanal makine hedef kaynak grubu.  Birden fazla ad girin ve her (değerleri büyük küçük harfe duyarlı değildir) bir virgül kullanarak ayırın.  Bir joker karakter kullanılması desteklenir.
   - Hedef kaynak gruplarındaki VM’lerin başlatılmasına ve durdurulmasına ilişkin yinelenen bir tarih ve saat olan bir **Zamanlama** seçin.  Varsayılan olarak, zamanlama için UTC saat dilimi yapılandırılır ve başka bir bölge seçmeyi kullanılabilir değil.  Çözüm yapılandırdıktan sonra belirli saat diliminiz için zamanlama yapılandırmak istiyorsanız bkz [başlatma ve kapatma zamanlamasını değiştirme](#modifying-the-startup-and-shutdown-schedule) aşağıda.
   - Almak için **e-posta bildirimleri** SendGrid varsayılan değerini kabul **Evet** ve geçerli bir eposta adresi belirtin.  Seçerseniz **Hayır** ve daha sonra karar verirseniz, e-posta bildirimleri almak istediğiniz, çözümü yeniden Azure Marketi'nden yeniden dağıtmanız gerekir.  

10. Çözüm için gereken ilk ayarların yapılandırılmasını tamamlandığınızda **Oluştur**’u seçin.  Tüm ayarlar doğrulanır ve çözümün aboneliğinize dağıtılması denenir.  Bu işlemin tamamlanması birkaç saniye alabilir ve ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="collection-frequency"></a>Toplama sıklığı

Otomasyon iş günlüğü ve iş akışı veri günlük analizi depoya beş dakikada alınan.  

## <a name="using-the-solution"></a>Çözümü kullanma

Azure Portal'dan günlük analizi çalışma alanınızdaki VM yönetim çözümü eklediğinizde **StartStopVM Görünüm** döşeme panonuza eklenir.  Bu kutucuğu sayısı ve runbook işleri başlatıldı ve başarıyla tamamlandı çözüm için grafik gösterimi görüntüler.<br><br> ![VM Management StartStopVM Görünümü Kutucuğu](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

Otomasyon hesabınızda erişmek ve çözüm seçerek yönetmek **çalışma** seçeneğini ve günlük analizi sayfasında **çözümleri** sol bölmeden.  Üzerinde **çözümleri** sayfasında, çözümü seçin **Start-Stop-VM [çalışma]** listeden.<br><br> ![Günlük analizi çözümleri listesinde](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

Çözümü seçme görüntüler **Start-Stop-VM [çalışma]** gözden geçirebileceğiniz önemli ayrıntılar gibi çözüm dikey **StartStopVM** gibi günlük analizi çalışma alanınızda döşeme hangi sayısını ve çözüm için başlamış olması ve başarıyla tamamladınız runbook işleri grafik gösterimi görüntüler.<br><br> ![Otomasyon güncelleştirme yönetimi çözümü sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Buradan da daha fazla iş kaydı analizini halka kutucuğa tıklayarak gerçekleştirebilirsiniz ve isteğe bağlı olarak çözümü panodan iş geçmişi, önceden tanımlanmış günlük arama sorguları gösterir ve aramak için günlük Advanced Analytics portalı geçin, arama sorguları temel alan.  

Tüm üst runbook'ları içeren *whatIf* parametresi, hangi olduğunda kümesine **True**, runbook alır olmadan çalıştırdığınızda tam davranışı ayrıntılı destekler *whatIf* parametre ve doğru doğrular VMs hedeflenen.  Runbook'ları yalnızca kendi tanımlanan eylemleri gerçekleştirir, *whatIf* parametrenin ayarlanmış **False**.  


### <a name="scenario-1-daily-stopstart-vms-across-a-subscription-or-target-resource-groups"></a>Senaryo 1: Günlük Durdur/VM'ler arasında bir aboneliği Başlat veya hedef kaynak grupları 

Bu, çözümü ilk kez dağıttığınızda varsayılan yapılandırmadır.  Örneğin, tüm sanal makineleri Akşam zaman iş bırakın ve office geri olduğunda sabah başlatmak abonelikte arasında durdurulmasına yapılandırabilirsiniz. Zamanlamaları yapılandırırken ** zamanlanmış-StartVM "ve **zamanlanmış StopVM** dağıtımı sırasında bunlar durdurup başlatın hedeflenen VM'ler.
>[!NOTE]
>Zamanlama süre parametre yapılandırırken saat dilimi geçerli saat diliminizde sağlar; ancak Azure Otomasyonu UTC biçiminde depolanır.  Bu dağıtım sırasında gerçekleştirilir gibi herhangi bir saat dilimi dönüştürmeye yapmak gerekmez.

Kapsamındaki VM'ler iki yapılandırarak denetim değişkenleri - **External_Start_ResourceGroupNames**, ** External_Stop_ResourceGroupNames, ve **External_ExcludeVMNames**.  

>[!NOTE]
> Değişken değeri **hedef kaynak grubu adları**"değeri olarak her ikisi için depolanan **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenleri. Daha fazla ayrıntı için her biri farklı kaynak grupları hedeflemek için bu değişkenleri değiştirebilirsiniz.  Başlangıç eylemi kullanmak için **External_Start_ResourceGroupNames** ve durdurma eylemi kullanıma **External_Stop_ResourceGroupNames** yerine. Yeni sanal makineleri Başlat'a otomatik olarak eklenir ve zamanlamaları durdurabilirsiniz.

Test ve yapılandırmanızı doğrulamak için el ile başlatın **ScheduledStartStop_Parent** runbook ve *eylem* parametresi **Başlat** veya **Durdur**  ve *whatIf* parametresi **doğru**.<br><br> ![Runbook parametrelerini Yapılandır](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Bu, gerçekleşir ve sanal makineleri üretim karşı uygulamadan önce değişiklik gerektiği gibi eylem Önizleme olanak sağlar.  Memnun kaldıktan sonra el ile runbook kümesine parametresiyle yürütebilirsiniz **false** veya otomasyonu zamanlaması izin **zamanlama-StartVM** ve **zamanlama StopVM**otomatik olarak belirlenen zamanlamanızı aşağıdaki çalıştırın.

### <a name="scenario-2-sequence-the-stopstart-vms-across-a-subscription-using-tags"></a>Senaryo 2: Durdur/Başlat VM'ler etiketleri kullanarak bir abonelik arasında sırası
Dağıtılmış bir iş yükünü destekleme birden çok VM iki veya daha fazla bileşenlerini içeren bir ortamda, hangi sırayla başlatılan/durduruldu bileşenleri dizisi destekleme önemlidir.  Bu, şu adımları izleyerek gerçekleştirebilirsiniz.

1. Ekleme bir **SequenceStart** ve **SequenceStop** artı bir tamsayı değeri VM'ler için hedeflenen aboneliğinizi arasında etiketiyle **External_Start_ResourceGroupNames**ve  **External_Stop*ResourceGroupNames** değişkenleri.  Başlatma ve durdurma Eylemler artan düzende gerçekleştirilir.  Bir VM etiketi öğrenmek için bkz: [Azure'da Windows sanal makine etiketi](../virtual-machines/windows/tag.md) ve [Azure'da bir Linux sanal makine etiketi](../virtual-machines/linux/tag.md)
2. Zamanlamaları değiştirme **sıralı-StartVM** ve **sıralı StopVM** gereksinimlerinizi karşılamak ve zamanlamasını etkinleştirmek için saat ve tarihi için.  

Test ve yapılandırmanızı doğrulamak için el ile başlatın **SequencedStartStop_Parent** runbook ve *eylem* parametresi **Başlat** veya **Durdur**  ve *whatIf* parametresi **doğru**.<br><br> ![Runbook parametrelerini Yapılandır](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Bu, gerçekleşir ve sanal makineleri üretim karşı uygulamadan önce değişiklik gerektiği gibi eylem Önizleme olanak sağlar.  Memnun kaldıktan sonra el ile runbook kümesine parametresiyle yürütebilirsiniz **false** veya otomasyonu zamanlaması izin **sıralı-StartVM** ve **sıralı StopVM**otomatik olarak belirlenen zamanlamanızı aşağıdaki çalıştırın.  

### <a name="scenario-3-auto-stopstart-vms-across-a-subscription-based-on-cpu-utilization"></a>Senaryo 3: Otomatik Durdur/Başlat VM'ler CPU kullanımı temelinde bir abonelik arasında
Aboneliğinizde sanal makineleri çalıştıran maliyetini yönetmenize yardımcı olmak için bu çözüm x %'den küçük yoğun olmayan zamanlarda gibi saat sonra kullanılmayan ve işlemci kullanımı olması durumunda otomatik olarak onları kapatmanız Azure VM'ler değerlendirerek yardımcı olabilir.  

Varsayılan olarak, çözümü Yüzde CPU ölçüm değerlendirmek için önceden yapılandırılmış ve ortalama kullanım %5 olup olmadığını veya daha az.  Bu, aşağıdaki değişkenleri tarafından denetlenir ve varsayılan değerlerine gereksinimlerinizi karşılamazsa değiştirilebilir:

* External_AutoStop_MetricName
* External_AutoStop_Threshold
* External_AutoStop_TimeAggregationOperator
* External_AutoStop_TimeWindow

Yalnızca abonelik ve kaynak grubu ya da sanal makineleri, ancak ikisini belirli listesi karşı eylem hedefleme etkinleştirebilirsiniz.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Bir abonelik ve kaynak grubu karşı durdurma eylemi hedef

1. Yapılandırma **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** değişkenleri VM'ler hedef belirtin.  
2. Etkinleştirme ve güncelleştirme **Schedule_AutoStop_CreateAlert_Parent** zamanlama.
3. Çalıştırma **AutoStop_CreateAlert_Parent** runbook'la *eylem* parametre kümesine **Başlat** ve *whatIf* parametre için **Doğru** değişikliklerinizi önizlemek için.

#### <a name="target-stop-action-by-vm-list"></a>VM listesi tarafından hedef durdurma eylemi

1. Çalıştırma **AutoStop_CreateAlert_Parent** runbook'la *eylem* parametre kümesine **Başlat**, Vm'lerde virgülle ayrılmış listesi ekleme *VMList* parametresi ve *whatIf* parametre kümesine **True** değişikliklerinizi önizlemek için.  
2. Yapılandırma **External_ExcludeVMNames** parametresi virgül ile ayrılmış VM'ler (VM1, VM2, VM3) listesi.
3. Bu senaryo dikkate almayabilir **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** varabies.  Bu senaryo için kendi Otomasyon zamanlama oluşturmanız gerekir. Ayrıntılar için bkz [Azure Otomasyon runbook'u zamanlama](../automation/automation-schedules.md).

Birini etkinleştirmeniz gerekiyor VM'ler durdurma CPU kullanımını temel alan için bir zamanlama sahip olduğunuza göre bunları başlatmak için zamanlama aşağıda.

* Hedef eylem abonelik ve kaynak grubu tarafından başlatın.  Açıklanan adımları izleyerek [Senaryo #1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) test ve etkinleştirme için **zamanlanmış-StartVM** zamanlama.
* Hedef abonelik, kaynak grubu ve etiket eylemle başlatın.  Açıklanan adımları izleyerek [Senaryo #2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) test etme ve "Sıralı-StartVM" etkinleştirme için zamanlama.


### <a name="configuring-e-mail-notifications"></a>E-posta bildirimlerini yapılandırma

Çözüm dağıtıldıktan sonra e-posta bildirimlerini yapılandırmak için aşağıdaki üç değişkenleri değiştirebilirsiniz:

* External_EmailFromAddress - gönderenin e-posta adresi belirtin
* External_EmailToAddress - bir virgülle ayrılmış e-posta adresleri listesi (user@hotmail.com, user@outlook.com) bildirim e-postaları almak için
* External_IsSendEmail - varsa kümesine **Evet**, e-postaları ve e-posta bildirimlerini devre dışı bırakmak için kümesine değeri **Hayır**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Başlatma ve kapatma zamanlamaları değiştirme

Bu çözümdeki başlatma ve kapatma zamanlamaları yönetme izleyen adımların aynısını kısmında özetlendiği gibi [Azure Otomasyon runbook'u zamanlama](automation-schedules.md).     

## <a name="log-analytics-records"></a>Log Analytics kayıtları

Otomasyon, OMS deposunda iki tür kayıt oluşturur.

### <a name="job-logs"></a>İş günlükleri

Özellik | Açıklama|
----------|----------|
Çağıran |  İşlemi başlatandır.  Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
Kategori | Veri türü sınıflandırması.  Otomasyon için değer JobLogs olacaktır.|
CorrelationId | Runbook işinin Bağıntı Kimliği olan GUID.|
JobId | Runbook işinin Kimliği olan GUID.|
operationName | Azure’da gerçekleştirilen işlem türünü belirtir.  Otomasyon için değer İş olacaktır.|
resourceId | Azure’daki kaynak türünü belirtir.  Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir.  Otomasyon için değer, Azure Otomasyonu olacaktır.|
ResourceType | Azure’daki kaynak türünü belirtir.  Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
resultType | Runbook işinin durumudur.  Olası değerler şunlardır:<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Başarılı oldu|
resultDescription | Runbook iş sonucu durumunu açıklar.  Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı|
RunbookName | Runbook’un adını belirtir.|
SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir.  Otomasyon için değer, :OpsManager olacaktır|
StreamType | Olay türünü belirtir. Olası değerler şunlardır:<br>- Ayrıntılı<br>- Çıktı<br>- Hata<br>- Uyarı|
SubscriptionId | İşin abonelik kimliğini belirtir.
Zaman | Runbook işinin yürütüldüğü tarih ve saat.|


### <a name="job-streams"></a>İş akışları

Özellik | Açıklama|
----------|----------|
Çağıran |  İşlemi başlatandır.  Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
Kategori | Veri türü sınıflandırması.  Otomasyon için değer JobStreams olacaktır.|
JobId | Runbook işinin Kimliği olan GUID.|
operationName | Azure’da gerçekleştirilen işlem türünü belirtir.  Otomasyon için değer İş olacaktır.|
ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
resourceId | Azure’daki kaynak kimliğini belirtir.  Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir.  Otomasyon için değer, Azure Otomasyonu olacaktır.|
ResourceType | Azure’daki kaynak türünü belirtir.  Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
resultType | Runbook işinin, olayın oluşturulduğu andaki sonucu.  Olası değerler şunlardır:<br>- Devam Ediyor|
resultDescription | Runbook’un çıktı akışını içerir.|
RunbookName | Runbook’un adı.|
SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir.  Otomasyon için değer, OpsManager olacaktır|
StreamType | İş akışı türü. Olası değerler şunlardır:<br>- İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı|
Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

**JobLogs** veya **JobStreams** kategorisinde kayıtlar döndüren bir günlük araması yaptığınızda, arama tarafından döndürülen güncelleştirmeleri özetleyen bir grup kutucuk görüntüleyen **JobLogs** veya **JobStreams** görünümlerini seçebilirsiniz.

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan iş kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır. 

Sorgu | Açıklama|
----------|----------|
Runbook başarıyla tamamladınız ScheduledStartStop_Parent işleri bulma | Kategori arama "JobLogs" &#124; == Burada (RunbookName_s "ScheduledStartStop_Parent" ==) &#124; Burada (ResultType "Tamamlandı" ==) &#124; AggregatedValue özetlemek ResultType, bin (TimeGenerated, 1 h) &#124; tarafından count() = TimeGenerated desc sıralama|
Runbook başarıyla tamamladınız SequencedStartStop_Parent işleri bulma | Kategori arama "JobLogs" &#124; == Burada (RunbookName_s "SequencedStartStop_Parent" ==) &#124; Burada (ResultType "Tamamlandı" ==) &#124; AggregatedValue özetlemek ResultType, bin (TimeGenerated, 1 h) &#124; tarafından count() = TimeGenerated desc sıralama

## <a name="removing-the-solution"></a>Çözüm kaldırma

Artık daha fazla çözüm herhangi kullanmanıza gerek karar verirseniz, Otomasyon hesabı silebilirsiniz.  Çözüm silme runbook'ları yalnızca kaldırır, zamanlamaları veya çözümü eklediğinizde, oluşturulan değişkenleri silmez.  Bu varlıkları, bunları ile diğer runbook'ları kullanmıyorsanız el ile silmeniz gerekir.  

Çözümü silmek için aşağıdaki adımları gerçekleştirin:

1.  Otomasyon hesabınızdan seçin **çalışma** sol bölmeden.  
2.  Üzerinde **çözümleri** sayfasında, çözümü seçin **Start-Stop-VM [çalışma]**.  Üzerinde **VMManagementSolution [çalışma]** sayfasından menüsünü tıklatın **silmek**.<br><br> ![VM yönetimi çözümü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  İçinde **silmek çözüm** penceresinde çözümü silmek istediğinizi onaylamak.
4.  Bilgi doğrulanır ve çözüm silinmiş olsa da, altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.  İçin döndürülecek **çözümleri** Çözüm kaldırma işlemini başladıktan sonra sayfa.  

Otomasyon hesabı ve günlük analizi çalışma alanı bu işlemin bir parçası olarak silinmez.  Günlük analizi çalışma alanı korumak istemiyorsanız, el ile silmeniz gerekir.  Bu, Azure portalından da gerçekleştirilebilir.   Giriş-ekranından Azure portalında, seçin **günlük analizi** ve daha sonra **günlük analizi** dikey penceresinde, çalışma alanını seçin ve tıklatın **silmek** menüsünden çalışma ayarları dikey penceresi.  
      
## <a name="next-steps"></a>Sonraki adımlar

- Farklı arama sorguları oluşturma ve Log Analytics ile Otomasyon iş günlüklerini gözden geçirme hakkında daha fazla bilgi edinmek için bkz. [Log Analytics’te günlük aramaları](../log-analytics/log-analytics-log-searches.md)
- Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md)
- Günlük analizi ve veri toplama kaynakları hakkında daha fazla bilgi için bkz: [toplama Azure storage veri günlük analizi genel bakış](../log-analytics/log-analytics-azure-storage.md)






   

