---
title: Azure Güvenlik Merkezi'nde veri toplamayı | Microsoft Docs
description: " Azure Güvenlik Merkezi'nde veri toplamayı etkinleştirmeyi öğrenin. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 8fed3ce98b23c5ac1cc97b88a278c5946f06af8e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968795"
---
# <a name="data-collection-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde veri toplamayı
Güvenlik Merkezi, Azure sanal makineleri (VM'ler), sanal makine ölçek kümeleri, Iaas kapsayıcılarınızdaki ve Azure olmayan (dahil, şirket içi) bilgisayarlar güvenlik açıklarını ve tehditleri izlemek için veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır. Bu tür verilerin örnekleri şunlardır: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri) çalışan işlemler, makine adı, IP adresleri ve kullanıcı oturum. Microsoft Monitoring Agent aracı, ayrıca kilitlenme bilgi dökümü dosyalarını çalışma alanınıza kopyalar.

Veri toplama, güncelleştirmeleri, yanlış yapılandırılmış işletim sistemi güvenlik ayarları, endpoint protection etkinleştirme ve sistem durumu ve tehdit algılama eksik içine görünürlük sağlamak için gereklidir. 

Bu makalede, Microsoft Monitoring Agent'ı yükleme ve toplanan verilerin depolanacağı bir Log Analytics çalışma alanını ayarlama hakkında yönergeler sağlanır. Her iki işlem, veri toplamayı etkinleştirmek için gereklidir. 

> [!NOTE]
> - Veri toplama, yalnızca işlem kaynakları (VM'ler, sanal makine ölçek kümeleri, Iaas kapsayıcıları ve Azure olmayan bilgisayarlar) gereklidir. Aracıları sağlama yoksa bile Azure Güvenlik Merkezi'nden yararlanabilir; Ancak, güvenlik kısıtlı olarak erişebilir ve yukarıda listelenen özellikleri desteklenmez.  
> - Desteklenen platformlar listesi için bkz: [desteklenen platformlar Azure Güvenlik Merkezi'nde](security-center-os-coverage.md).
> - Sanal makine ölçek kümesi için veri koleksiyonu şu anda desteklenmiyor.
> - Yeni veya mevcut bir çalışma kullanıp Log Analytics'te veri depolama, veri depolama için ek ücret, daha fazla ayrıntı için fiyatlandırma sayfasına bakın.

## Microsoft Monitoring Agent'ın otomatik sağlamayı etkinleştirme <a name="auto-provision-mma"></a>

Makinelerden verileri toplamak için Microsoft izleme aracısı yüklü olmalıdır.  Aracı yüklemesi otomatik olarak olabilir (önerilir) veya aracıyı el ile yüklemeyi tercih edebilirsiniz.  

>[!NOTE]
> Otomatik sağlama varsayılan olarak kapalıdır. Güvenlik Merkezi, varsayılan olarak otomatik sağlama yüklemek için ayarlanacak ayarlayın **üzerinde**.
>

Otomatik sağlama açık olduğunda Güvenlik Merkezi desteklenen tüm Azure Vm'lere ve oluşturulan tüm yeni vm'lere Microsoft Monitoring agent sağlar. Otomatik sağlama önemle tavsiye edilir ancak el ile aracı yüklemelerini da kullanılabilir. [Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz öğrenin](#manualagent).



Microsoft Monitoring Agent için otomatik sağlamayı etkinleştirmek üzere:
1. Güvenlik Merkezi ana menüsünde seçin **Güvenlik İlkesi**.
2. Tıklayın **ayarlarını Düzenle** listesinde istediğiniz Abonelik ayarları sütununda.

   ![Abonelik seçme][7]

3. **Güvenlik ilkesi** bölümünde **Veri Toplama**’yı seçin.
4. Altında **otomatik sağlama**seçin **üzerinde** otomatik sağlamayı etkinleştirmek için.
5. **Kaydet**’i seçin.

   ![Otomatik sağlamayı etkinleştirme][1]

>[!NOTE]
> - Önceden var olan bir yükleme sağlama konusunda yönergeler için bkz. [önceden var olan bir aracı yüklemesi durumlarda otomatik sağlama](#preexisting).
> - El ile sağlama ile ilgili yönergeler için bkz: [Microsoft Monitoring Agent uzantısını el ile yükleme](#manualagent).
> - Otomatik sağlama kapatma hakkında yönergeler için bkz: [otomatik sağlamayı etkinleştirmek](#offprovisioning).
> - Yönergeler için PowerShell kullanarak yerleşik güvenlik Merkezi'ne nasıl görürüm [ekleme PowerShell kullanarak Azure Güvenlik Merkezi'nin otomatik hale getirmek](security-center-powershell-onboarding.md).
>

## <a name="workspace-configuration"></a>Çalışma alanı yapılandırması
Güvenlik Merkezi tarafından toplanan veriler, Log Analytics çalışma Alanlarınızda depolanır.  Azure Güvenlik Merkezi tarafından oluşturulan çalışma alanlarında veya kendi oluşturduğunuz mevcut bir çalışma depolanan vm'lerden toplanan verileri seçebilirsiniz. 

Çalışma alanı yapılandırması, abonelik başına ayarlanır ve çok abonelik aynı çalışma alanını kullanabilir.

### <a name="using-a-workspace-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan bir çalışma alanını kullanma

Güvenlik Merkezi otomatik olarak veri depolamak için varsayılan çalışma alanı oluşturabilirsiniz. 

Güvenlik Merkezi tarafından oluşturulan bir çalışma alanı seçmek için:

1. Altında **varsayılan çalışma alanı yapılandırması**, select, Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını kullanın.
   ![Fiyatlandırma katmanı seçin][10] 

1. **Kaydet**’e tıklayın.<br>
    Güvenlik Merkezi, coğrafi konum içinde yeni bir kaynak grubu ve varsayılan çalışma alanı oluşturur ve aracıyı bu çalışma alanına bağlar. Çalışma alanını ve kaynak grubu için adlandırma kuralı aşağıdaki gibidir:<br>
   **Çalışma alanı: DefaultWorkspace-[abonelik-kimliği]-[Bölge]<br> kaynak grubu: DefaultResourceGroup-[Bölge]**

   Bir abonelik birden çok geolocations Vm'leri içeriyorsa, Güvenlik Merkezi birden çok çalışma alanı oluşturur. Birden çok çalışma alanı, veri gizlilik kuralları korumak için oluşturulur.
1. Güvenlik Merkezi bir Güvenlik Merkezi çözüm çalışma alanının fiyatlandırma katmanını ayarlamak için abonelik başına otomatik olarak etkinleştirir. 

> [!NOTE]
> Fiyatlandırma katmanı Güvenlik Merkezi tarafından oluşturulan çalışma alanları, Log Analytics, Güvenlik Merkezi faturalandırma etkilemez. Güvenlik Merkezi her zaman, Güvenlik Merkezi güvenlik ilkesi ve bir çalışma alanına yüklenmiş çözümlere göre faturalandırılır. Güvenlik Merkezi'nin ücretsiz katmanı için etkinleştirir *SecurityCenterFree* çözüm için varsayılan çalışma alanı. Güvenlik Merkezi, Standart katmanda varsayılan çalışma alanında *Security* çözümünü etkinleştirir.
> Log Analytics'te veri depolama, veri depolama için ek ücret, daha fazla ayrıntı için fiyatlandırma sayfasına bakın.

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/).

Mevcut log analytics hesabı hakkında daha fazla bilgi için bkz: [mevcut log analytics müşterileri](security-center-faq.md#existingloganalyticscust).

### <a name="using-an-existing-workspace"></a>Mevcut bir çalışma alanını kullanma

Mevcut bir Log Analytics çalışma alanı zaten varsa, aynı çalışma alanını kullanmak isteyebilirsiniz.

Mevcut bir Log Analytics çalışma alanınızı kullanmak için olmalıdır okuma ve yazma izinleri için çalışma alanı.

> [!NOTE]
> Mevcut çalışma alanınızda etkin çözüm, kendisine bağlı Azure vm'lerine uygulanır. Ücretli çözümler için bu ek ücretlere neden olabilir. İçin veri gizlilik konuları, seçili çalışma alanının sağ coğrafi bölgede olduğundan emin olun.
> Log analytics'te veri depolama, veri depolama için ek ücret, daha fazla ayrıntı için fiyatlandırma sayfasına bakın.

Mevcut bir Log Analytics çalışma alanı seçmek için:

1. Altında **varsayılan çalışma alanı yapılandırması**seçin **başka bir çalışma alanını kullanabilmesini**.

   ![Mevcut bir çalışma alanı seçin][2]

2. Aşağı açılır menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

   > [!NOTE]
   > Tüm aboneliklerinizi tüm çalışma alanları açılır menü, kullanılabilir. Bkz: [çapraz abonelik çalışma alanı seçimi](security-center-enable-data-collection.md#cross-subscription-workspace-selection) daha fazla bilgi için. Çalışma alanına erişim izni olması gerekir.
   >
   >

3. **Kaydet**’i seçin.
4. Seçtikten sonra **Kaydet**, daha önce varsayılan çalışma alanına bağlı izlenen Vm'leri yeniden istiyorsanız istenir.

   - Seçin **Hayır** yalnızca yeni Vm'lere uygulamak için yeni çalışma alanı ayarlarını istiyorsanız. Yeni çalışma alanı ayarları, yalnızca yeni aracı yüklemelerini için geçerlidir; Microsoft Monitoring Agent yüklüyse olmayan yeni bulunmuş VM'ler.
   - Seçin **Evet** tüm sanal makinelere uygulamak için yeni çalışma alanı ayarlarını istiyorsanız. Ayrıca, çalışma alanı oluşturulduğunda bir güvenlik Merkezi'ne bağlı her bir VM yeni hedef çalışma alanına bağlanır.

   > [!NOTE]
   > Evet'i seçerseniz, tüm sanal makineler yeni hedef çalışma alanına bağlandı kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmemelisiniz. Bir çalışma alanı çok erken silinirse bu işlem başarısız olur.
   >
   >

   - Seçin **iptal** işlemi iptal etme.

     ![Mevcut bir çalışma alanı seçin][3]

5. Microsoft Monitoring agent'ı ayarlamayı planladığınız istenen çalışma alanı için fiyatlandırma katmanını seçin. <br>Mevcut bir çalışma alanını kullanmak için çalışma alanı için fiyatlandırma katmanını ayarlayın. Zaten mevcut değilse bu çalışma alanında bir Güvenlik Merkezi çözümü yükler.

    a.  Güvenlik Merkezi ana menüsünde seçin **Güvenlik İlkesi**.
     
    b.  Hangi düşündüğünüz aracı tıklayarak bağlanmak istediğiniz çalışma alanını seçin **ayarlarını Düzenle** listesinde istediğiniz Abonelik ayarları sütununda.
        ![Çalışma alanı seçin][8] c. Fiyatlandırma katmanını ayarlayın.
        ![Fiyatlandırma katmanı seçin][9] 
   
   >[!NOTE]
   >Çalışma alanı zaten varsa, bir **güvenlik** veya **SecurityCenterFree** etkin çözüm, fiyatlandırma ayarlanacak otomatik olarak. 

## <a name="cross-subscription-workspace-selection"></a>Çapraz abonelik çalışma alanı seçimi
Tüm aboneliklerinizdeki tüm çalışma alanları, verilerinizin depolanacağı bir çalışma alanı seçtiğinizde kullanılabilir. Çapraz abonelik çalışma alanı seçimi, farklı Aboneliklerde çalışan sanal makinelerden veri toplama ve tercih ettiğiniz çalışma alanında depolamak sağlar. Bu seçim, kuruluşunuzdaki merkezi bir çalışma alanı kullanıyorsanız ve güvenlik verileri toplanmasını için kullanmak istediğiniz yararlı olur. Çalışma alanlarını yönetme hakkında daha fazla bilgi için bkz. [çalışma alanı erişimi yönetme](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access).


## <a name="data-collection-tier"></a>Veri koleksiyonu katmanı
Azure Güvenlik Merkezi'nde veri koleksiyonunu katmanı seçerek yalnızca Log Analytics çalışma alanınızda, güvenlik olaylarının depolama etkiler. Log Analytics aracısını hala toplamak ve Log Analytics çalışma alanınızda (varsa) depolamak seçtiğiniz hangi güvenlik olayları katmanı bağımsız olarak Azure Güvenlik Merkezi'nin tehdit algılamaları için gereken güvenlik olayları analiz edin. Çalışma alanınızda güvenlik olayları depolamak seçme araştırma, arama ve çalışma alanınızda bu olayların denetimini etkinleştirir. 
> [!NOTE]
> Log analytics'te veri depolama, veri depolama için ek ücret, daha fazla ayrıntı için fiyatlandırma sayfasına bakın.
> 
> Çalışma alanınızda depolanmış Abonelikleriniz ve çalışma alanlarından olayların dört kümesi için ilke filtreleme hakkı seçebilirsiniz: 

- **Hiçbiri** – güvenlik olay depolama devre dışı bırakın. Bu varsayılan ayardır.
- **En az** – olayları olay birimi en aza indirmek isteyen müşteriler için daha küçük bir dizi.
- **Ortak** – tam denetim kaydı sağlar ve müşterilerin çoğu karşılayan bir dizi olay budur.
- **Tüm olaylar** – tüm olayları emin olmak isteyen müşterilerin depolanır.


> [!NOTE]
> Bu güvenlik olayları kümeleri yalnızca Güvenlik Merkezi'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
Bu ayarlar, tipik senaryoları için tasarlanmıştır. Uygulamadan önce gereksinimlerinize hangisinin uyduğunu değerlendirmek üzere emin olun.
>
>

Ait olayları belirlemek üzere **ortak** ve **Minimal** olay kümeleri çalıştık filtrelenmemiş sıklığını her olay ve kullanımları hakkında bilgi edinmek için müşteriler ve endüstri standartları. Bu işlem aşağıdaki yönergeleri kullandık:

- **En az** -bu küme yalnızca başarılı ihlal gösterebilir olayları ve çok düşük bir birime sahip önemli olayları kapsadığından emin olun. Örneğin, bu kullanıcının başarılı ve başarısız oturum açma (olay kimliği 4624 4625) içerir, ancak denetim için önemlidir, ancak algılama için anlamlı ve görece yüksek hacimli olan oturum kapatma içermiyor. Bu veri hacmini çoğunu, oturum açma olayları ve işlem oluşturma olayı (olay kimliği 4688) olur.
- **Ortak** -bu kümesindeki bir tam kullanıcı denetim kaydı sağlar. Örneğin, bu ayarla, hem kullanıcı oturum açma bilgileri ve kullanıcı oturum kapatma (olay kimliği 4634) içerir. Güvenlik grubu değişikliklerini, anahtar etki alanı denetleyicisi Kerberos işlemleri ve sektör kuruluşlar tarafından önerilen diğer olaylar gibi eylemleri denetimi ekliyoruz.

Tüm olayları üzerine miktarının azaltılmasını ve belirli olay filtre için olduğunu seçin için ana motivasyon olarak ortak çok düşük bir birime sahip olayları dahil edilmişti.

Güvenlik ve App Locker olay kimlikleri her küme için tam bir dökümü aşağıda verilmiştir:

| Veri katmanı | Toplanan olay göstergeleri |
| --- | --- |
| En az | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Ortak | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Grup İlkesi nesnesi (GPO) kullanıyorsanız, denetim ilkeleri işlem oluşturma olay 4688 etkinleştirmeniz önerilir ve *CommandLine* olay 4688 içindeki alan. İşlem oluşturma olay 4688 hakkında daha fazla bilgi için bkz: Güvenlik Merkezi'nin [SSS](security-center-faq.md#what-happens-when-data-collection-is-enabled). Denetim ilkeleri bunlar hakkında daha fazla bilgi için bkz: [Denetim İlkesi önerileri](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  İçin veri toplamayı etkinleştirmek için [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md), Güvenlik Merkezi, tüm uygulamalara izin vermek üzere Denetim modunda yerel bir AppLocker ilkesini yapılandırır. Bu, daha sonra toplanan ve Güvenlik Merkezi tarafından kullanılabilir olaylar oluşturmak AppLocker neden olur. Bu ilke, üzerinde zaten var. yapılandırılmış bir AppLocker İlkesi tüm makinelerde yapılandırılmaz dikkat edin önemlidir. 
> - Windows Filtre Platformu toplanacak [olay kimliği 5156](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156), etkinleştirmeniz gerekiyor [denetim Platform bağlantısı filtrelemeyi](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) (Auditpol/kümesi subcategory: "Platform bağlantısı filtrelemeyi" /Success:Enable)
>

Filtreleme ilkenizi seçmek için:
1. Üzerinde **güvenlik ilkesinde veri toplamayı** dikey penceresinde, filtreleme ilkesini altında seçin **güvenlik olaylarını**.
2. **Kaydet**’i seçin.

   ![Filtreleme ilkesi seçin][5]

### Önceden var olan bir aracı yüklemesi durumlarda otomatik sağlama <a name="preexisting"></a> 

Otomatik sağlama çalışır durumda olduğunda zaten bir aracı veya uzantı yüklü aşağıdaki kullanım örneklerini belirtin. 

- Microsoft Monitoring Agent makinede, ancak uzantı (doğrudan aracı) olarak yüklenir.<br>
Microsoft Monitoring Agent (olarak değil bir Azure uzantısı) doğrudan VM'de yüklü değilse, Güvenlik Merkezi Microsoft Monitoring Agent uzantısını yükleyecek ve Microsoft Monitoring agent, en son sürüme yükseltebilirsiniz.
Yüklü aracı için zaten yapılandırılmış kendi çalışma alanlarında bildirmeye devam eder ve ayrıca Güvenlik Merkezi'nde yapılandırılmış çalışma alanına rapor eder (birden çok giriş desteklenir).
Yapılandırılmış çalışma alanı olan bir kullanıcı çalışma (Güvenlik Merkezi'nin varsayılan çalışma değil) ve ardından yüklemeniz gerekir, "güvenlik /"securityFree"çözümü için Güvenlik Merkezi, Vm'leri ve Bilgisayarları'ndan olayları işlemeyi başlatmak bu çalışma alanına raporlama.<br>
<br>
Güvenlik Merkezi'ne abonelikleri eklenmedi 2019-03-mevcut bir aracının ne zaman algılanır, 17 önce mevcut makinelerde Microsoft Monitoring Agent uzantısını yüklenmez ve makine etkilenmez. Bu makineler için bu makineler üzerinde aracı yükleme sorunlarını gidermek için "Çözümle makinelerinizde aracı sistem durumu sorunlarını izleme" öneri için bkz.

  
- System Center Operations Manager aracısının makinede yüklü<br>
Güvenlik Merkezi, mevcut Operations Manager için Microsoft Monitoring Agent uzantısı yan yana yüklenir. Mevcut Operations Manager aracısının Operations Manager sunucusuna normalde bildirmeye devam eder. Operations Manager aracısı ve Microsoft Monitoring Agent bu işlem sırasında en son sürüme güncelleştirilir ortak çalışma zamanı kitaplıkları paylaşmak unutmayın.
Operations Manager 2012 aracı sürümü yüklü değilse, - Not **olmayan** üzerinde sağlama otomatik kapatma.<br>

- Önceden var olan VM uzantısı yok<br>
    - İzleme Aracısı, uzantı olarak yüklendikten sonra uzantı yapılandırması yalnızca tek bir çalışma alanına raporlama sağlar. Güvenlik Merkezi, kullanıcı çalışma alanları için varolan bağlantılar kılmaz. Güvenlik Merkezi şartıyla "güvenlik" veya "securityFree" çözüm üzerinde yüklü VM Güvenlik verileri zaten bağlı çalışma alanında depolar. Güvenlik Merkezi, uzantı sürümü, bu işlem en son sürüme yükseltebilirsiniz.  
    - Hangi çalışma alanına mevcut uzantı test çalıştırmak için veri gönderdiğini görmek için [Azure Güvenlik Merkezi ile bağlantısı doğrulama](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/). Alternatif olarak, Log Analytics çalışma alanları'nı açın, bir çalışma alanı seçin, sanal Makineyi seçin ve Log Analytics aracısını bağlantısını arayın. 
    - Bir ortamınız varsa, burada Log Analytics aracısını istemci iş istasyonları üzerinde yüklü olduğundan ve mevcut bir Log Analytics çalışma alanına raporlama listesini gözden geçirin [Azure Güvenlik Merkezi tarafından desteklenen işletim sistemleri](security-center-os-coverage.md) emin olmak için işletim sisteminiz desteklenir ve bkz [mevcut log analytics müşterileri](security-center-faq.md#existingloganalyticscust) daha fazla bilgi için.
 
### Otomatik sağlamayı etkinleştirmek <a name="offprovisioning"></a>
Otomatik kaynaklardan herhangi bir zamanda bu güvenlik ilkesi ayarı devre dışı bırakarak sağlamayı kapatın kapatabilirsiniz. 


1. Güvenlik Merkezi ana menüsüne geri dönün ve Güvenlik İlkesi'ni seçin.
2. Tıklayın **ayarlarını Düzenle** satırında otomatik sağlamayı devre dışı bırakmak istediğiniz abonelik.
3. Üzerinde **güvenlik ilkesi – veri toplama** dikey altında **otomatik sağlama** seçin **kapalı**.
4. **Kaydet**’i seçin.

   ![Otomatik sağlamayı devre dışı bırak][6]

Otomatik sağlama (Kapalı) devre dışı bırakıldığında, varsayılan çalışma alanı yapılandırma bölümü görüntülenmez.

Otomatik sağlama sonra kapalı geçiş yaparsanız daha önce şirket şöyleydi:
-   Aracıları yeni Vm'lere sağlanmadı.
-   Güvenlik Merkezi, varsayılan çalışma alanından veri toplamayı durdurur.
 
> [!NOTE]
>  Otomatik sağlama devre dışı bırakıldığında Microsoft Monitoring Agent aracı burada sağlanan Azure Vm'lerinden kaldırmaz. OMS uzantısını kaldırma hakkında daha fazla bilgi için bkz: [nasıl Güvenlik Merkezi tarafından yüklü OMS uzantılarını kaldırabilirim](security-center-faq.md#remove-oms).
>
    
## El ile aracı sağlama <a name="manualagent"></a>
 
Microsoft Monitoring Agent'ı el ile yüklemek için birkaç yolu vardır. El ile yükleme sırasında otomatik sağlamayı devre dışı bırakıldığından emin olun.

### <a name="operations-management-suite-vm-extension-deployment"></a>Operations Management Suite VM uzantısı dağıtımı 

Güvenlik Merkezi, Vm'lerinizden güvenlik verilerini toplamak ve öneriler ve uyarılar sağlamak için Microsoft Monitoring Agent, el ile yükleyebilirsiniz.
1. Otomatik sağlama – Kapalı'i seçin.
2. Çalışma alanı oluşturma ve Microsoft Monitoring agent'ı ayarlamayı planladığınız çalışma alanı için fiyatlandırma katmanını ayarlayın:

   a.  Güvenlik Merkezi ana menüsünde seçin **Güvenlik İlkesi**.
     
   b.  Aracısını bağlamak istediğiniz çalışma alanını seçin. Çalışma alanı aynı abonelikte Güvenlik Merkezi'nde kullanın ve çalışma alanı okuma/yazma izinlerine sahip olduğundan emin olun.
       ![Çalışma alanı seçin][8]
3. Fiyatlandırma katmanını ayarlayın.
   ![Fiyatlandırma katmanı seçin][9] 
   >[!NOTE]
   >Çalışma alanı zaten varsa, bir **güvenlik** veya **SecurityCenterFree** etkin çözüm, fiyatlandırma ayarlanacak otomatik olarak. 
   > 

4. Resource Manager şablonu kullanılarak yeni Vm'lere aracılarda dağıtmak istiyorsanız, OMS sanal makine uzantısını yükleyin:

   a.  [Windows için OMS sanal makine uzantısını yükleme](../virtual-machines/extensions/oms-windows.md)
    
   b.  [Linux için OMS sanal makine uzantısını yükleme](../virtual-machines/extensions/oms-linux.md)
5. Uzantıları mevcut Vm'lere dağıtmak için yönergeleri izleyin. [Azure sanal makineler hakkında veri toplama](../azure-monitor/learn/quick-collect-azurevm.md).

   > [!NOTE]
   > Bölüm **olay ve performans verileri toplama** isteğe bağlıdır.
   >
6. Uzantıyı dağıtmak için PowerShell kullanmak için aşağıdaki PowerShell örneği kullanın:
   
   [!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
   
   1. Git **Log Analytics** tıklayın **Gelişmiş ayarlar**.
    
      ![Log Analytics'i ayarlama][11]

   2. Dışı değerleri kopyalayın **Workspaceıd** ve **birincil anahtar**.
  
      ![Değerleri kopyalayın][12]

   3. Genel yapılandırma ve özel yapılandırma aşağıdaki değerlerle doldurun:
     
           $PublicConf = '{
               "workspaceId": "WorkspaceID value"
           }' 
 
           $PrivateConf = '{
               "workspaceKey": "<Primary key value>”
           }' 

      - Bir Windows VM'de yüklerken:
        
            Set-AzVMExtension -ResourceGroupName $vm.ResourceGroupName -VMName $vm.Name -Name "MicrosoftMonitoringAgent" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "MicrosoftMonitoringAgent" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True 
    
      - Bir Linux VM'de yüklerken:
        
            Set-AzVMExtension -ResourceGroupName $vm1.ResourceGroupName -VMName $vm1.Name -Name "OmsAgentForLinux" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "OmsAgentForLinux" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True`

> [!NOTE]
> Yönergeler için PowerShell kullanarak yerleşik güvenlik Merkezi'ne nasıl görürüm [ekleme PowerShell kullanarak Azure Güvenlik Merkezi'nin otomatik hale getirmek](security-center-powershell-onboarding.md).

## <a name="troubleshooting"></a>Sorun giderme

-   Otomatik sağlama yükleme sorunlarını belirlemek için bkz. [aracı sistem durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).

-  Monitoring agent ağ gereksinimi tanımlamak için bkz: [sorun giderme monitoring agent ağ gereksinimleri](security-center-troubleshooting-guide.md#mon-network-req).
-   El ile ekleme sorunlarını belirlemek için bkz. [Operations Management Suite ekleme sorunlarını giderme](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).

- İzlenmeyen VM'ler ve bilgisayarların sorunlarını belirlemek için bkz. [izlenmeyen VM'ler ve bilgisayarlar](security-center-virtual-machine-protection.md#unmonitored-vms-and-computers).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede gösterilen, nasıl veri toplama ve otomatik sağlama Güvenlik Merkezi çalışır. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.



<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
[8]: ./media/security-center-enable-data-collection/manual-provision.png
[9]: ./media/security-center-enable-data-collection/pricing-tier.png
[10]: ./media/security-center-enable-data-collection/workspace-selection.png
[11]: ./media/security-center-enable-data-collection/log-analytics.png
[12]: ./media/security-center-enable-data-collection/log-analytics2.png
