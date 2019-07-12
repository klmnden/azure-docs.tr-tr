---
title: Azure İzleyici, Azure İlkesi'ni kullanarak VM'ler için etkinleştirme | Microsoft Docs
description: Bu makalede, VM'ler için birden fazla Azure sanal makineler için Azure İzleyicisi'ni etkinleştirme veya Azure İlkesi'ni kullanarak sanal makine ölçek kümeleri nasıl açıklar.
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
ms.openlocfilehash: cf06004c70609dbea59a47b207e3568299260a82
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594439"
---
# <a name="enable-azure-monitor-for-vms-preview-by-using-azure-policy"></a>Azure İzleyici (Önizleme) VM'ler için Azure İlkesi'ni kullanarak etkinleştirme

Azure İlkesi'ni kullanarak sanal makine ölçek kümeleri ya da bu makalede, Azure sanal makineler için Azure İzleyici (Önizleme) VM'ler için etkinleştirme açıklanmaktadır. Bu işlem sonunda başarıyla Log Analytics ve bağımlılık aracılarını etkinleştirme yapılandırdıktan ve uyumlu olmayan sanal makineler belirledik.

Bulmak, yönetmek ve tüm Azure sanal makine veya sanal makine ölçek kümeleri için sanal makineler için Azure İzleyicisi'ni etkinleştirmek için Azure İlkesi ya da Azure PowerShell kullanabilirsiniz. Azure İlkesi, çünkü etkili bir şekilde tutarlı uyumluluk sağlamak için aboneliklerinizi yönetmek için İlke tanımları yönetebilir ve Vm'leri sağlanan otomatik yeni etkinleştirme öneririz yöntemidir. Bu ilke tanımları:

* Log Analytics aracısını ve bağımlılık aracısını dağıtın.
* Uyumluluk sonuçları hakkında rapor oluşturun.
* Uyumsuz VM'ler için düzeltin.

Azure PowerShell veya Azure Resource Manager şablonu ile bu görevleri yerine getirmeye ilgileniyorsanız bkz [etkinleştirin, Azure PowerShell veya Azure Resource Manager şablonları kullanarak Vm'leri (Önizleme) için Azure İzleyici](vminsights-enable-at-scale-powershell.md).

## <a name="manage-policy-coverage-feature-overview"></a>İlke kapsamı özelliğine genel bakış yönetme

Başlangıçta, Azure VM'ler için Azure İzleyici için İlke tanımları dağıtma ve yönetme için ilke deneyimiyle yalnızca Azure İlkesi'nden yapılmadı. İlke kapsamı yönetme özelliği, daha basit ve daha kolay bulmak, yönetmek ve uygun ölçekte etkinleştirmek getirir **VM'ler için Azure İzleyici'ı etkinleştirme** daha önce bahsedilen ilke tanımlarını içeren bir girişim. Bu yeni özellikten erişebileceğiniz **Başlarken** Azure İzleyici VM'ler için sekmesinde. Seçin **yönetme ilke kapsamı** açmak için **Vm'leri ilke Kapsamınız için Azure İzleyici** sayfası.

![Azure İzleyici sekmesinden VM'ler çalışmaya başlama](./media/vminsights-enable-at-scale-policy/get-started-page-01.png)

Buradan denetleyin ve girişim kapsamı aboneliklerini ve Yönetim grupları arasında yönetme. Kaç tane Vm'niz anlamak her Yönetim gruplarını, abonelikleri ve uyumluluk durumları yok.

![Azure İzleyici Vm'leri yönetme İlkesi sayfası](./media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Bu bilgiler, planlama ve bir merkezi konumda bulunan VM'ler için Azure İzleyici için idare senaryonuz yürütme yardımcı olması yararlıdır. Azure İlkesi, bir kapsam için bir ilke veya girişim atandığında Uyumluluk Görünümü sağlasa da bu yeni bir sayfa ile burada ilke veya girişim atanmamıştır ve yerinde atama bulabilir. Tüm Eylemler, atama, görüntülemek ve yeniden yönlendirme Azure İlkesi doğrudan düzenlemek gibi. **Vm'leri ilke Kapsamınız için Azure İzleyici** sayfasıdır genişletilmiş ve tümleşik bir deneyim için girişim **VM'ler için Azure İzleyici'ı etkinleştirme**.

Bu sayfadan ayrıca Log Analytics çalışma alanınız için Azure İzleyici VM'ler için yapılandırabilirsiniz hangi:

- Hizmet eşlemesi yükleme ve altyapı öngörüleri çözümleri yükler.
- Performans grafiklerini, çalışma kitaplarını ve özel günlük sorguları ve Uyarıları tarafından kullanılan işletim sistemi performans sayaçlarını etkinleştirir.

![VM'ler için Azure İzleyici yapılandırma çalışma alanı](./media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Bu seçenek, tüm ilke eylemler için ilişkili değil. Karşılamak için kolay bir yol sağlamak kullanılabilir [önkoşulları](vminsights-enable-overview.md) VM'ler için Azure İzleyicisi'ni etkinleştirmek için gereklidir.  

### <a name="what-information-is-available-on-this-page"></a>Hangi bilgilerin bu sayfada var mı?
Aşağıdaki tabloda, ilke kapsamı sayfası ve nasıl yorumladığınıza sunulan bilgilerin dökümünü sağlar.

| İşlev | Açıklama | 
|----------|-------------| 
| **Kapsam** | Yönetim grubu ve sahip olduğunuz abonelik veya yönetim grubu hiyerarşisi aracılığıyla detaya gitme olanağı ile erişimi devralınan.|
| **Rol** | Rolünüz kapsamına, okuyucu, sahibi veya katkıda bulunan olabilir. Bazı durumlarda, bu aboneliğe erişiminiz ancak yönetim grubuna ait olmayan belirtmek için boş görünür. Diğer sütunlarındaki bilgileri rolünüze bağlı olarak değişir. Rol, hangi veri görebilirsiniz ve ilke veya girişim (sahibi) atama, bunları düzenleme veya görüntüleme uyumluluk açısından gerçekleştirebileceğiniz eylemler belirlemede anahtardır. |
| **Toplam VM sayısı** | VM sayısı, kapsamı altında. Bir yönetim grubu için abonelikler veya alt yönetim grubunda altında iç içe sanal makinelerin bir toplamıdır. |
| **Atama kapsamı** | İlke veya girişim kapsamında Vm'leri yüzdesi. |
| **Atama durumu** | İlke veya girişim atamasının durumu hakkında bilgiler. |
| **Uyumlu VM'ler** | İlke veya girişim altında uyumlu olan VM sayısı. Girişim **VM'ler için Azure İzleyici'ı etkinleştirme**, hem Log Analytics aracısını ve bağımlılık aracısını Vm'leri sayısıdır. Bazı durumlarda, bu atama bulunamadı, VM yok veya yeterli izinleri nedeniyle boş görünür. Altında sağlanan bilgiler **uyumluluk durumu**. |
| **Uyumluluk** | Genel uyumluluk sayısını tüm birbirinden ayrı kaynaklara toplamına göre bölünmüş uyumlu olan, birbirinden ayrı kaynaklara toplamıdır. |
| **Uyumluluk Durumu** | Uyumluluk durumu, ilke veya girişim ataması hakkında bilgiler.|

İlke veya girişim atadığınızda, atama seçilen kapsam listelenen kapsam ya da bir kısmını olabilir. Örneğin, bir atama için bir abonelik (ilke kapsamı) ve bir yönetim grubu (Karşılama kapsamı) değil oluşturmuş olabilir. Bu durumda, değerini **atama kapsamı** ilke veya girişim kapsam kapsamı kapsam içindeki Vm'leri bölünen sanal makineleri gösterir. Başka bir durumda, bazı VM'ler, kaynak grubu veya abonelik İlkesi kapsamının dışında bırakılan. Değer boş ise, ilke veya girişim yok veya izniniz yok gösterir. Altında sağlanan bilgiler **atama durumu**.

## <a name="enable-by-using-azure-policy"></a>Azure İlkesi'ni kullanarak etkinleştirme

Azure İzleyici VM'ler için kiracınızda Azure İlkesi kullanarak etkinleştirmek için:

- Girişimin bir kapsama atayın: Yönetim grubu, abonelik veya kaynak grubu.
- Gözden geçirin ve uyumluluk sonuçlarını düzeltin.

Azure ilkesi atama hakkında daha fazla bilgi için bkz. [Azure ilkesine genel bakış](../../governance/policy/overview.md#policy-assignment) ve gözden geçirme [yönetim gruplarına genel bakış](../../governance/management-groups/index.md) devam etmeden önce.

### <a name="policies-for-azure-vms"></a>Azure sanal makineler için ilkeleri

İlke tanımları bir Azure VM için aşağıdaki tabloda listelenmiştir.

|Ad |Açıklama |Type |
|-----|------------|-----|
|\[Önizleme\]: Sanal makineler için Azure İzleyicisi'ni etkinleştirme |Azure İzleyici, belirtilen kapsam (Yönetim grubu, abonelik veya kaynak grubu) sanal makineler için etkinleştirin. Log Analytics çalışma alanı, bir parametre olarak alır. |Girişim |
|\[Önizleme\]: Bağımlılık Aracısı dağıtımı – sanal makine görüntüsü (OS) listelenmemiş denetleme |Vm'leri uyumsuz olarak bir VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil bildirir. |İlke |
|\[Önizleme\]: Log Analytics Aracısı dağıtımı – sanal makine görüntüsü (OS) listelenmemiş denetleme |Vm'leri uyumsuz olarak bir VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil bildirir. |İlke |
|\[Önizleme\]: Linux sanal makineleri için bağımlılık Aracısı dağıtma |Bağımlılık aracısını Linux VM'ler için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |
|\[Önizleme\]: Windows sanal makineleri için bağımlılık Aracısı dağıtma |Bağımlılık Aracısı Windows Vm'leri için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |
|\[Önizleme\]: Linux VM'ler için log Analytics aracısını dağıtmayı |Log Analytics aracısını Linux VM'ler için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |
|\[Önizleme\]: Windows Vm'leri için log Analytics aracısını dağıtma |Log Analytics aracısını Windows Vm'leri için VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil dağıtın. |İlke |

### <a name="policies-for-azure-virtual-machine-scale-sets"></a>İlkeleri için Azure sanal makine ölçek kümeleri

Bir Azure sanal makine ölçek kümesi için İlke tanımları aşağıdaki tabloda listelenmiştir.

|Ad |Açıklama |Type |
|-----|------------|-----|
|\[Önizleme\]: Sanal makine ölçek kümeleri için Azure İzleyicisi'ni etkinleştirme |Belirtilen kapsamda (Yönetim grubu, abonelik veya kaynak grubu) sanal makine ölçek kümeleri için Azure İzleyici'yi etkinleştirin. Log Analytics çalışma alanı, bir parametre olarak alır. Not: Ölçek kümesinin yükseltme İlkesi el ile olarak ayarlanırsa, uzantı kümesindeki tüm sanal makineler yükseltme bunlara çağrı yaparak uygulanır. CLI, az vmss update-instances budur. |Girişim |
|\[Önizleme\]: Bağımlılık Aracısı dağıtımı'sanal makine ölçek kümelerinde – sanal makine görüntüsü (OS) listelenmemiş denetleme |Sanal makine ölçek VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil, uyumsuz olarak kümesi bildirir. |İlke |
|\[Önizleme\]: Log Analytics Aracısı dağıtımı'sanal makine ölçek kümelerinde – sanal makine görüntüsü (OS) listelenmemiş denetleme |Sanal makine ölçek VM görüntüsü (OS) listesinde tanımlı değil ve aracı yüklü değil, uyumsuz olarak kümesi bildirir. |İlke |
|\[Önizleme\]: Linux sanal makine ölçek kümeleri için bağımlılık Aracısı dağıtma |Linux VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri için bağımlılık Aracısı'nı dağıtın. |İlke |
|\[Önizleme\]: Windows sanal makine ölçek kümeleri için bağımlılık Aracısı dağıtma |Windows VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri için bağımlılık Aracısı'nı dağıtın. |İlke |
|\[Önizleme\]: Linux sanal makine ölçek kümeleri için log Analytics aracısını dağıtma |Linux VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri için log Analytics aracısını dağıtın. |İlke |
|\[Önizleme\]: Windows sanal makine ölçek kümeleri için log Analytics aracısını dağıtma |Windows VM görüntüsü (OS) listede tanımlanır ve aracı yüklü değil, sanal makine ölçek kümeleri için log Analytics aracısını dağıtın. |İlke |

Tek başına ilke (girişimle yer almaz) aşağıda açıklanmıştır:

|Ad |Açıklama |Type |
|-----|------------|-----|
|\[Önizleme\]: Log Analytics çalışma alanı VM rapor uyuşmazlığı denetleme |Vm'lere ilke veya girişim atamasını belirtilen Log Analytics çalışma alanına oturum olmayan uyumsuz olarak bildirin. |İlke |

### <a name="assign-the-azure-monitor-initiative"></a>Azure İzleyici girişim Ata
İlke oluşturmak için **Vm'leri ilke Kapsamınız için Azure İzleyici** sayfasında, aşağıdaki adımları izleyin. Bu adımları tamamlamak nasıl anlamak için bkz: [Azure portalından bir ilke ataması oluşturma](../../governance/policy/assign-policy-portal.md).

İlke veya girişim atadığınızda, atama seçilen kapsam burada listelenen kapsam ya da bir kısmını olabilir. Örneğin, abonelik (ilke kapsamı) ve (Karşılama kapsamı) değil yönetim grubu için bir atama oluşturmuş olabilir. Bu durumda, kapsamı yüzdesi kapsamı kapsam içindeki Vm'leri ilke veya girişim kapsam içindeki Vm'leri bölü gösterir. Başka bir durumda, bazı VM'ler veya kaynak grubu veya abonelik İlkesi kapsamının dışında bırakılan. Boş ise, ilke veya girişim yok veya izniniz yok gösterir. Altında sağlanan bilgiler **atama durumu**.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Azure portalında **İzleyici**. 

3. Seçin **sanal makineler (Önizleme)** içinde **Insights** bölümü.
 
4. Seçin **Başlarken** sekmesi. Sayfasında, seçin **yönetme ilke kapsamı**.

5. Tablodan bir yönetim grubu veya abonelik seçin. Seçin **kapsam** nokta (...) seçerek. Örnekte, bir kapsam bir gruplandırma için ilke ataması için zorlama sanal makinelerin sınırlar.

6. Üzerinde **Azure İlkesi ataması** sayfasında da önceden doldurulmuş girişimle **VM'ler için Azure İzleyici'ı etkinleştirme**. 
    **Atama adı** kutusu girişim adıyla otomatik olarak doldurulur, ancak bunu değiştirebilirsiniz. İsteğe bağlı bir açıklama da ekleyebilirsiniz. **Atayan** kutusu, açan göre otomatik olarak doldurulur. Bu değer isteğe bağlıdır.

7. (İsteğe bağlı) Kapsamdan bir veya daha fazla kaynakları kaldırmak için işaretleyin **dışlamaları**.

8. İçinde **Log Analytics çalışma alanı** bir çalışma alanı açılır listesinde desteklenen bir bölge için seçin.

   > [!NOTE]
   > Çalışma alanı atama kapsamı dışındadır, verme *Log Analytics katkıda bulunan* izinleri ilke atama sorumlusu kimliği Bunu yapmazsanız, bir dağıtım hatası gibi görebilirsiniz `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` erişimi vermek için gözden [el ile yönetilen kimlik yapılandırma](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  **Yönetilen kimliği** onay kutusunun seçili atanan girişim bir ilkeyle içerdiğinden *Deployıfnotexists* efekt.
    
9. İçinde **yönetme kimlik konumu** aşağı açılan listesinde, uygun bölgeyi seçin.

10. **Ata**'yı seçin.

Atama oluşturduktan sonra **Vm'leri ilke Kapsamınız için Azure İzleyici** sayfa güncelleştirme **atama kapsamı**, **atama durumu**, **uyumlu Vm'leri**, ve **uyumluluk durumu** değişiklikleri yansıtacak şekilde. 

Girişim her olası uyumluluk durumu aşağıdaki matris eşler.  

| Uyumluluk durumu | Açıklama | 
|------------------|-------------|
| **Uyumlu** | Kapsamdaki tüm VM'ler kendilerine dağıtılan Log Analytics ve bağımlılık aracılarını sahiptir.|
| **Uyumlu değil** | Log Analytics kapsamında tüm Vm'leri sahip ve bağımlılık aracıları kendilerine dağıtılan ve düzeltme gerekebilir.|
| **Başlatılmadı** | Yeni bir atama eklendi. |
| **Kilit** | Yönetim grubu için yeterli ayrıcalığınız yok. <sup>1</sup> | 
| **Boş** | İlke yok atanır. | 

<sup>1</sup> erişim sağlamak için bir sahip yönetim grubu erişimi yoksa, isteyin. Veya, uyumluluğu görüntüleyin ve atamaları alt Yönetim gruplarını veya abonelikleri aracılığıyla yönetin. 

Aşağıdaki tabloda, her olası atama durumu girişim eşler.

| Atama durumu | Açıklama | 
|------------------|-------------|
| **Başarılı** | Kapsamdaki tüm VM'ler kendilerine dağıtılan Log Analytics ve bağımlılık aracılarını sahiptir.|
| **Uyarı** | Abonelik altındaki bir yönetim grubu değil.|
| **Başlatılmadı** | Yeni bir atama eklendi. |
| **Kilit** | Yönetim grubu için yeterli ayrıcalığınız yok. <sup>1</sup> | 
| **Boş** | VM yok ya da bir ilke atanmamış. | 
| **Eylem** | İlke atama veya bir atamayı düzenleyin. | 

<sup>1</sup> erişim sağlamak için bir sahip yönetim grubu erişimi yoksa, isteyin. Veya, uyumluluğu görüntüleyin ve atamaları alt Yönetim gruplarını veya abonelikleri aracılığıyla yönetin.

## <a name="review-and-remediate-the-compliance-results"></a>Gözden geçirin ve uyumluluk sonuçlarını Düzelt

Aşağıdaki örnek, bir Azure sanal makinesi için ancak sanal makine ölçek kümeleri için de geçerlidir. Uyumluluk sonuçlarını gözden geçirmek öğrenmek için bkz: [uyumsuzluk sonuçları tanımlamak](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Üzerinde **Vm'leri ilke Kapsamınız için Azure İzleyici** sayfasında, tablodan bir yönetim grubu veya abonelik seçin. Seçin **görünümü uyumluluğu** nokta (...) seçerek.   

![Azure Vm'leri için Uyumluluk İlkesi](./media/vminsights-enable-at-scale-policy/policy-view-compliance-01.png)

Girişimle dahil ilke sonuçları temelinde, VM'ler, aşağıdaki senaryolarda uyumsuz olarak bildirilir:

* Log Analytics aracısını veya bağımlılık aracısını dağıtılan değil.  
    Bu senaryo, var olan Vm'leri bir kapsamla tipik bir durumdur. Bunu azaltmak için gereken aracıları tarafından dağıtma [düzeltme görevler oluşturma](../../governance/policy/how-to/remediate-resources.md) uyumsuz İlkesi üzerinde yapılamaz.  
    - \[Önizleme\]: Linux sanal makineleri için bağımlılık Aracısı dağıtma
    - \[Önizleme\]: Windows sanal makineleri için bağımlılık Aracısı dağıtma
    - \[Önizleme\]: Linux VM'ler için log Analytics aracısını dağıtmayı
    - \[Önizleme\]: Windows Vm'leri için log Analytics aracısını dağıtma

* Sanal makine görüntüsü (OS) ilke tanımında tanımlanan değildir.  
    Dağıtım ilkesi ölçütlerini iyi bilinen bir Azure VM görüntülerinden dağıtılan Vm'leri içerir. VM işletim sistemi desteklenip desteklenmediğini görmek için belgelere bakın. Desteklenmiyorsa, güncelleştirme ve dağıtım ilkesi yinelenen veya görüntü uyumlu hale getirmek için değiştirebilirsiniz.  
    - \[Önizleme\]: Bağımlılık Aracısı dağıtımı – sanal makine görüntüsü (OS) listelenmemiş denetleme
    - \[Önizleme\]: Log Analytics Aracısı dağıtımı – sanal makine görüntüsü (OS) listelenmemiş denetleme

* Sanal makineleri belirtilen Log Analytics çalışma alanına oturum değildir.  
    Bazı VM'ler girişim kapsamda bir ilke atamasında belirtilen başka bir Log Analytics çalışma açtığı, mümkündür. Bu ilke, VM'ler, uyumlu olmayan bir çalışma alanına raporlama yapmayan tanımlamak için kullanılan bir araçtır.  
    - \[Önizleme\]: Log Analytics çalışma alanı VM rapor uyuşmazlığı denetleme

## <a name="edit-an-initiative-assignment"></a>Bir girişim atamasını Düzenle

Bir yönetim grubuna veya aboneliğe, girişim atandıktan sonra istediğiniz zaman aşağıdaki özelliklerini değiştirmek için düzenleyebilirsiniz:

- Ödev adı
- Açıklama
- Atayan
- Log Analytics çalışma alanı
- Özel durumlar

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için izlenmesi etkin olduğunda, bu bilgileri analiz için sanal makineler için Azure İzleyici ile kullanılabilir. 

- Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). 
- Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). 
- Performans sorunlarını ve sanal makinenin performans genel kullanımı belirlemek için bkz: [görünümü Azure VM performansını](vminsights-performance.md). 
- Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).
