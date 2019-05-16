---
title: Azure İlkesi kullanan VM'ler için Azure İzleyicisi'ni etkinleştirme | Microsoft Docs
description: Bu makalede, birden fazla Azure sanal makineler veya sanal makine ölçek kümeleri Azure İlkesi'ni kullanarak Azure İzleyici Vm'leri için nasıl olanak açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: magoedte
ms.openlocfilehash: 9664ad5272ffeb55bd85e3d4c4f4b70d05e97702
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524153"
---
# <a name="enable-azure-monitor-for-vms-preview-using-azure-policy"></a>Azure İzleyicisi'ni (Önizleme) VM'ler için Azure İlkesi'ni kullanarak etkinleştirme

Bu makalede, Azure İzleyici (Önizleme) VM'ler için Azure sanal makineler veya sanal makine ölçek kümeleri Azure İlkesi kullanarak etkinleştirmek açıklanmaktadır. Bu işlemin sonunda, başarıyla yapılandırılmış Log Analytics ve bağımlılık aracıları'nı etkinleştirme ve uyumlu olmayan sanal makineler belirledik.

Bulmak, yönetmek ve tüm Azure sanal makine veya sanal makine ölçek kümeleri için sanal makineler için Azure İzleyicisi'ni etkinleştirmek için Azure İlkesi ya da Azure PowerShell kullanabilirsiniz. Azure İlkesi, etkili bir şekilde tutarlı uyumluluk sağlamak için aboneliklerinizi yönetmek için İlke tanımları yönetebilir ve otomatik yeni etkinleştirme sağlanan Vm'leri için önerilen yöntem aşamasındadır. Bu ilke tanımları:

* Log Analytics aracısını ve bağımlılık aracısını dağıtın.
* Uyumluluk sonuçları hakkında rapor oluşturun.
* Uyumlu olmayan VM'ler için düzeltin.

Bu Azure PowerShell veya Azure Resource Manager şablonu ile işlemlerle ilgileniyorsanız bkz [Azure PowerShell veya Resource Manager şablonu kullanarak etkinleştir](vminsights-enable-at-scale-powershell.md). 

## <a name="manage-policy-coverage-feature-overview"></a>İlke kapsamı özelliğine genel bakış yönetme

İlk olarak deneyimiyle yönetme ve VM'ler için Azure İzleyici için İlke tanımları dağıtmak için Azure ilkesi yalnızca Azure İlkesi'nden gerçekleştirildi. İle **yönetme ilke kapsamı** özelliği kolaylaştırır, daha basit ve kolay bir şekilde bulmak, yönetmek ve uygun ölçekte etkinleştirmek **VM'ler için Azure İzleyici'ı etkinleştirme** ilke tanımlarını içeren bir girişim daha önce bahsedilen. Bu yeni özellikten erişebileceğiniz **başlama** seçerek Azure İzleyici VM'ler için sekmesinde **yönetme ilke kapsamı**. Bu VM'ler ilke kapsamı sayfası açılır. 

![Azure İzleyici sekmesinden VM'ler çalışmaya başlama](./media/vminsights-enable-at-scale-policy/get-started-page-01.png)

Buradan, denetleyin ve girişim kapsamı aboneliklerini ve Yönetim grupları arasında yönetme, yapabilir kaç tane Vm'niz anlamak her Yönetim gruplarını, abonelikleri ve uyumluluk durumları yok.   

![Azure İzleyici Vm'leri yönetme İlkesi sayfası](./media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Bu bilgiler planlamak ve bir merkezi konumda bulunan VM'ler için Azure İzleyici için idare senaryonuz yürütmek yararlıdır. Bu yeni bir sayfa ile burada İlkesi/girişimi değil atanır ve yerinde atama İlkesi/girişimi bir kapsama atanmış durumdayken Azure İlkesi Uyumluluk Görünümü sağlasa da bulabilir. Tüm Eylemler (ata, görüntüleme, düzenleme) doğrudan yönlendirme için Azure ilkesi. Azure için Vm'leri ilke kapsamı sayfası izleyicisidir genişletilmiş ve tümleşik bir deneyim için girişim **VM'ler için Azure İzleyici'ı etkinleştirme**. 

Bu sayfadan ayrıca Log Analytics çalışma alanınız için Azure İzleyici VM'ler için aşağıdakileri gerçekleştiren yapılandırabilirsiniz:

- Hizmet eşlemesi yükleme ve altyapı öngörüleri çözümleri yükler
- Performans grafiklerini, çalışma kitaplarını ve özel günlük sorguları ve Uyarıları tarafından kullanılan işletim sistemi performans sayaçlarını etkinleştirir.

![VM'ler için Azure İzleyici yapılandırma çalışma alanı](./media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Bu seçenek, tüm ilke eylemleriyle ilgili değildir ve karşılamak için kolay bir yol sağlamak üzere kullanılabilir [önkoşulları](vminsights-enable-overview.md) VM'ler için Azure İzleyicisi'ni etkinleştirmek için gereklidir.  

### <a name="what-information-is-available-on-this-page"></a>Hangi bilgilerin bu sayfada var mı?
Aşağıdaki tabloda, hangi bilgilerin ilke kapsamı sayfasında sunulur ve nasıl yorumladığınıza dökümünü sağlar.

| İşlev | Açıklama | 
|----------|-------------| 
| **Kapsam** | Yönetim grubu ve sahip olduğunuz abonelikleri veya devralınan erişim grubu hiyerarşisi aracılığıyla yönetimi Detaya gitme olanağı.|
| **Rol** | Okuyucu sahibi veya katkıda bulunan bir kapsama rolünüz olabilir. Bazı durumlarda, aboneliğe erişiminiz olmayabilir ancak yönetim grubuna ait olmayan belirtmek için boş görünebilir. Hangi veri görebilirsiniz ve girişim/ilke (sahibi) atama, bunları düzenleme veya görüntüleme uyumluluk açısından gerçekleştirebileceğiniz eylemler belirlemede anahtarı olduğu gibi diğer sütunlarındaki bilgileri rolünüze bağlı olarak değişir. |
| **Toplam VM sayısı** | VM sayısı, kapsamı altında. Bir yönetim grubu için abonelikler ve/veya alt yönetim grubunda altında iç içe sanal makinelerin bir toplamıdır. |
| **Atama kapsamı** | Yüzde girişimin/ilke tarafından kapsanan bir VM. |
| **Atama durumu** | Bu sütunu altında ilkenizin durumu hakkında bilgi bulabilirsiniz / girişim ataması. |
| **Uyumlu VM'ler** | İlke/girişimin altında uyumlu olan VM sayısı. Girişim **VM'ler için Azure İzleyici'ı etkinleştirme** Log Analytics aracısını hem bağımlılık aracısını Vm'leri sayısıdır. Bazı durumlarda, hiçbir atama veya VM yok veya yetersiz izinler nedeniyle boş görünebilir. Bilgiler, uyumluluk durumu altında sağlanır. |
| **Uyumluluk** | Genel uyumluluk sayısını bölünmüş uyumlu tüm birbirinden ayrı kaynaklara toplamına göre farklı kaynaklar toplamıdır. |
| **Uyumluluk Durumu** | İlkeniz uyumluluk durumu hakkında bilgi / girişim ataması.|

Girişim/ilkeyi atadığınızda, atama seçilen kapsam listelenen kapsam ya da bir kısmını olabilir. Örneğin, bir atama için bir abonelik (ilke kapsamı) ve bir yönetim grubu (Karşılama kapsamı) değil oluşturmuş olabilirsiniz. Bu durumda, değerini **atama kapsamı** kapsamı kapsam içindeki Vm'leri bölü ilke (veya girişim) Vm'leri kapsam gösterir. Başka bir durumda, bazı VM'ler, kaynak grubu veya abonelik İlkesi kapsamının dışında bırakılan. Değer boş ise, ilke/girişim yok veya izniniz yok, gösterir (atama durumu bilgileri sağlanır).

## <a name="enable-using-azure-policy"></a>Azure İlkesi’ni kullanarak etkinleştirme

Azure İzleyici VM'ler için kiracınızda Azure İlkesi kullanarak etkinleştirmek için:

- Girişimin bir kapsama atayın: Yönetim grubu, abonelik veya kaynak grubu
- Gözden geçirin ve uyumluluk sonuçlarını Düzelt

Azure ilkesi atama hakkında daha fazla bilgi için bkz. [Azure ilkesine genel bakış](../../governance/policy/overview.md#policy-assignment) ve gözden geçirme [yönetim gruplarına genel bakış](../../governance/management-groups/index.md) devam etmeden önce.

### <a name="policies-for-azure-vms"></a>Azure sanal makineler için ilkeleri

İlke tanımları bir Azure VM için aşağıdaki tabloda listelenmiştir:

|Ad |Açıklama |Tür |
|-----|------------|-----|
|[Önizleme]: Sanal makineler için Azure İzleyicisi'ni etkinleştirme |Azure İzleyici, belirtilen kapsam (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler (VM) için etkinleştirin. Log Analytics çalışma alanı, bir parametre olarak alır. |Girişim |
|[Önizleme]: Denetim bağımlılık Aracısı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı |Vm'leri uyumsuz olarak VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil bildirir. |İlke |
|[Önizleme]: Denetim günlüğü analiz aracı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı |Vm'leri uyumsuz olarak VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil bildirir. |İlke |
|[Önizleme]: Linux sanal makineleri için bağımlılık Aracısı dağıtma |Bağımlılık aracısını Linux Vm'leri için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |
|[Önizleme]: Windows sanal makineleri için bağımlılık Aracısı dağıtma |Windows Vm'leri için bağımlılık Aracısı, aracı yüklü değil ve (OS) VM görüntü listesinde tanımlanan dağıtın. |İlke |
|[Önizleme]: Linux Vm'leri için log Analytics aracısını dağıtmayı |Log Analytics aracısını Linux Vm'leri için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |
|[Önizleme]: Windows Vm'leri için log Analytics aracısını dağıtmayı |VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil Windows Vm'leri için log Analytics aracısını dağıtın. |İlke |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>İlkeleri için Azure sanal makine ölçek kümeleri

Bir Azure sanal makine ölçek kümesi için İlke tanımları aşağıdaki tabloda listelenmiştir:

|Ad |Açıklama |Tür |
|-----|------------|-----|
|[Önizleme]: VM ölçek kümeleri (VMSS) için Azure İzleyicisi'ni etkinleştirme |Belirtilen kapsamda (Yönetim grubu, abonelik veya kaynak grubu) sanal makine ölçek kümeleri için Azure İzleyici'yi etkinleştirin. Log Analytics çalışma alanı, parametre olarak alır. Not:, Ölçek kümesi upgradePolicy el ile olarak ayarlarsanız, yükseltme bunlara çağrı yaparak kümedeki tüm sanal makineler için uzantının uygulanması gerekir. CLI'daki bu az vmss update-instances olacaktır. |Girişim |
|[Önizleme]: Bağımlılık Aracısı VMSS – sanal makine görüntüsü (OS) listelenmemiş dağıtımda denetleme |Uyumlu değil olarak (OS) VM görüntüsü listesi ve aracıyı tanımlanmadı, raporları sanal makine ölçek yüklü değil. |İlke |
|[Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS – sanal makine görüntüsü (OS) listeden kaldırıldı |Uyumlu değil olarak (OS) VM görüntüsü listesi ve aracıyı tanımlanmadı, raporları sanal makine ölçek yüklü değil. |İlke |
|[Önizleme]: Linux VM ölçek kümeleri (VMSS) için bağımlılık Aracısı dağıtma |Linux VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri için bağımlılık aracısını dağıtın. |İlke |
|[Önizleme]: Windows VM ölçek kümeleri (VMSS) için bağımlılık Aracısı dağıtma |Bağımlılık aracısı için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri Windows dağıtın. |İlke |
|[Önizleme]: Linux VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı |Linux VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri için log Analytics aracısını dağıtın. |İlke |
|[Önizleme]: Windows VM ölçek kümeleri (VMSS) için log Analytics aracısını dağıtmayı |Log Analytics aracısı için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri Windows dağıtın. |İlke |

Tek başına ilke (girişimle yer almaz) aşağıda açıklanmıştır:

|Ad |Açıklama |Tür |
|-----|------------|-----|
|[Önizleme]: VM - rapor uyuşmazlığı denetim Log Analytics çalışma alanı |Vm'lere ilke/girişim atamasını belirtilen Log Analytics çalışma alanına oturum olmayan uyumsuz olarak bildirin. |İlke |

### <a name="assign-the-azure-monitor-initiative"></a>Azure İzleyici girişim Ata
Azure İzleyici Vm'leri ilke kapsamı sayfası için ilke ataması oluşturmak için aşağıdaki adımları gerçekleştirin. Bu adımları tamamlamak nasıl anlamak için bkz: [Azure portalından bir ilke ataması oluşturma](../../governance/policy/assign-policy-portal.md).

Girişim/ilkeyi atadığınızda, atama seçilen kapsam burada listelenen kapsam ya da bir kısmını olabilir. Örneğin, atama için abonelik (ilke kapsamı) ve büyük/küçük harf kapsamı % kapsamı kapsam içindeki Vm'leri bölü ilke (veya girişim) Vm'leri kapsam belirtmek değil yönetim grubu (Karşılama kapsamı) oluşturmuş olabilir. Diğer bazı durumlarda, bazı VM'ler veya RGs ya da abonelik İlkesi kapsamının dışında bırakılan.  Boş olması durumunda gösterir kurallardan / girişim yok veya izinleri (atama durumu içinde sağlanan bilgileri) sahip değil  

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Azure portalında **İzleyici**. 

3. Seçin **sanal makineler (Önizleme)** içinde **Insights** bölümü.
 
4. Seçin **Başlarken** sekmesine tıklayın ve sayfanın **yönetme ilke kapsamı**.

5. Bir yönetim grubu veya abonelik tablodan seçip **kapsam** üzerindeki üç nokta (…) tıklatarak.      Bizim örneğimizde, bir kapsam bir gruplandırma için ilke ataması için zorlama sanal makinelerin sınırlar.

6. Üzerinde **Azure İlkesi ataması** sayfasında, önceden doldurulur girişimle **VM'ler için Azure İzleyici'ı etkinleştirme**. 
    **Atama adı** kutusu girişim adıyla otomatik olarak doldurulur, ancak bunu değiştirebilirsiniz. İsteğe bağlı bir açıklama da ekleyebilirsiniz. **Atayan** kutusu, açan göre otomatik olarak doldurulur ve bu değer isteğe bağlıdır.

7. (İsteğe bağlı) Kapsamdan bir veya daha fazla kaynakları kaldırmak için işaretleyin **dışlamaları**.

8. İçinde **Log Analytics çalışma alanı** bir çalışma alanı açılır listesinde desteklenen bir bölge için seçin.

   > [!NOTE]
   > Çalışma alanı atama kapsamı dışındadır, verme *Log Analytics katkıda bulunan* izinleri ilke atama sorumlusu kimliği Bunu yapmazsanız, aşağıdaki gibi bir dağıtım hatası görebilirsiniz: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` Erişim vermek için gözden [el ile yönetilen kimlik yapılandırma](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  **Yönetilen kimliği** onay kutusu seçiliyse, atanan girişim bir ilkeyle içerdiğinden *Deployıfnotexists* efekt.
    
9. İçinde **yönetme kimlik konumu** aşağı açılan listesinde, uygun bölgeyi seçin.

10. **Ata**'yı seçin.

Atama oluşturduktan sonra Azure İzleyici Vm'leri ilke kapsamı sayfası için atama kapsamı, atama durumu, uyumluluk Vm'leri ve uyumluluk durumu değişiklikleri yansıtacak şekilde güncelleştirin. 

Girişim her olası uyumluluk durumu aşağıdaki matris eşler.  

| Uyumluluk Durumu | Açıklama | 
|------------------|-------------|
| **Uyumlu** | Kapsamdaki tüm VM'ler kendilerine dağıtılan Log Analytics ve bağımlılık aracılarını sahiptir.|
| **Uyumlu değil** | Log Analytics kapsamında tüm Vm'leri sahip ve bağımlılık aracıları kendilerine dağıtılan ve düzeltme gerektirebilir.|
| **Başlatılmadı** | Yeni bir atama eklendi. |
| **Kilit** | Yönetim grubu için yeterli ayrıcalıklara sahip değilsiniz. <sup>1</sup> | 
| **Boş** | Atanan ilke yok. | 

<sup>1</sup> erişim sağlamak veya uyumluluk görüntüleyip atamaları alt Yönetim gruplarını veya abonelikleri aracılığıyla yönetmek için bir sahip yönetim grubu erişimi yoksa, isteyin. 

Aşağıdaki tabloda, her olası atama durumu girişim eşler.

| Atama Durumu | Açıklama | 
|------------------|-------------|
| **Başarılı** | Kapsamdaki tüm VM'ler kendilerine dağıtılan Log Analytics ve bağımlılık aracılarını sahiptir.|
| **Uyarı** | Abonelik altındaki bir yönetim grubu değil.|
| **Başlatılmadı** | Yeni bir atama eklendi. |
| **Kilit** | Yönetim grubu için yeterli ayrıcalıklara sahip değilsiniz. <sup>1</sup> | 
| **Boş** | Hiçbir VM'lerin mevcut veya ilke atanmadı. | 
| **Eylem** | Atama İlkesi veya düzenleme atama | 

<sup>1</sup> erişim sağlamak veya uyumluluk görüntüleyip atamaları alt Yönetim gruplarını veya abonelikleri aracılığıyla yönetmek için bir sahip yönetim grubu erişimi yoksa, isteyin. 

## <a name="review-and-remediate-the-compliance-results"></a>Gözden geçirin ve uyumluluk sonuçlarını Düzelt

Aşağıdaki örnek bir Azure VM için ancak sanal makine ölçek kümeleri için de geçerlidir. Okuyarak uyumluluk sonuçlarını gözden geçirmek öğrenebilirsiniz [uyumsuzluk sonuçları tanımlamak](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Azure İzleyici Vm'leri ilke kapsamı sayfası için tablodan bir yönetim grubu veya abonelik seçin ve seçin **görünümü uyumluluğu** üzerindeki üç nokta (…) tıklatarak.   

![Azure Vm'leri için Uyumluluk İlkesi](./media/vminsights-enable-at-scale-policy/policy-view-compliance-01.png)

Girişimle dahil ilke sonuçları temelinde, uyumlu değil olarak aşağıdaki senaryolarda Vm'leri bildirilir:

* Log Analytics veya bağımlılık aracısını dağıtılabilir değil.  
    Bu senaryo, var olan Vm'leri bir kapsamla tipik bir durumdur. Bunu azaltmak için gereken aracıları tarafından dağıtma [düzeltme görevler oluşturma](../../governance/policy/how-to/remediate-resources.md) uyumlu olmayan bir ilkesi üzerinde yapılamaz.  
    - [Önizleme]: Deploy Dependency Agent for Linux VMs
    - [Önizleme]: Deploy Dependency Agent for Windows VMs
    - [Önizleme]: Deploy Log Analytics Agent for Linux VMs
    - [Önizleme]: Deploy Log Analytics Agent for Windows VMs

* VM görüntüsü (OS) ilke tanımında tanımlanan değildir.  
    Dağıtım ilkesi ölçütlerini iyi bilinen bir Azure VM görüntülerinden dağıtılan Vm'leri içerir. VM işletim sistemi desteklenip desteklenmediğini görmek için belgelere bakın. Desteklenmiyorsa, güncelleştirme ve dağıtım ilkesi yinelenen veya görüntü uyumlu hale getirmek için değiştirebilirsiniz.  
    - [Önizleme]: Denetim bağımlılık Aracısı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı
    - [Önizleme]: Denetim günlüğü analiz aracı dağıtımı – sanal makine görüntüsü (OS) listeden kaldırıldı

* Sanal makineleri belirtilen Log Analytics çalışma alanına oturum değildir.  
    Bazı VM'ler girişim kapsamda bir ilke atamasında belirtilen başka bir Log Analytics çalışma açtığı, mümkündür. Bu ilke, VM'ler, uyumlu olmayan bir çalışma alanına raporlama yapmayan tanımlamak için kullanılan bir araçtır.  
    - [Önizleme]: Audit Log Analytics Workspace for VM - Report Mismatch

## <a name="edit-initiative-assignment"></a>Girişim atamasını Düzenle

Bir yönetim grubuna veya aboneliğe bir girişimin atamasını sonra herhangi bir zamanda aşağıdaki özellikleri değiştirmek için düzenleyebilirsiniz:

- Atama adı
- Açıklama
- Atayan
- Log Analytics çalışma alanı
- Özel Durumlar

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için izlenmesi etkin olduğunda, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir. Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). Performans sorunlarını ve Vm'leri performansınızı ile genel kullanımı belirlemek için bkz: [Azure VM performansını görüntüleme](vminsights-performance.md), ya da bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).