---
title: Tam zamanında sanal makine, Azure Güvenlik Merkezi'nde erişim | Microsoft Docs
description: Bu belgenin nasıl tam zamanında VM erişimi, Azure Güvenlik Merkezi'nde yardımcı olan erişimi denetlemek için Azure sanal makinelerinizin gösterir.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2018
ms.author: terrylan
ms.openlocfilehash: 288524e58efd64670df098f249f3ad0b1cca464c
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990587"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme

Tam zamanında sanal makine içinde (VM) erişimi, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır, Azure Vm'lere gelen trafiği kilitlemek için kullanılabilir.

> [!NOTE]
> Tam zamanında özelliği Güvenlik Merkezi'nin standart katmanında kullanılabilir.  Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
>

## <a name="attack-scenario"></a>Saldırı senaryosu

Deneme yanılma hedef yönetim noktaları bir sanal makineye erişmek için bir yol olarak genellikle saldırıları. Başarılı olursa, bir saldırganın VM yükü denetimini ve ortamınıza bir köprübaşı oluşturmak.

Bir deneme yanılma saldırısı maruz kalma riskinizi azaltmak için bir bağlantı noktası açık olduğu süre miktarını sınırlamak için yoludur. Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Bunların yalnızca VM’ye bağlı olduğunuzda (örneğin, yönetim veya bakım görevleri gerçekleştirmek için) açık olması gerekir. Tam zamanında etkinleştirildiğinde Güvenlik Merkezi kullanan [ağ güvenlik grubu](../virtual-network/security-overview.md#security-rules) saldırganlar tarafından hedeflenemez yönetim bağlantı noktalarına erişimi kısıtlama (NSG) kuralları.

![Yalnızca zaman senaryoda][1]

## <a name="how-does-just-in-time-access-work"></a>Nasıl tam zamanında erişim çalışır mı?

Tam zamanında etkinleştirildiğinde, Güvenlik Merkezi bir NSG kuralı oluşturarak Azure VM’lere gelen trafiği kilitler. Sizin için gelen trafiği kilitlenir VM bağlantı noktalarını seçin. Bu bağlantı noktaları yalnızca tarafından denetlenen çözümünü.

Bir kullanıcı bir sanal makine erişimine izin istediğinde, Güvenlik Merkezi kullanıcı olup olmadığını denetler [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) VM için yazma erişimi sağlayan izinleri. Belirtilen yazma iznine sahip oldukları isteğin onaylanacağını ve Güvenlik Merkezi, ağ güvenlik süre Seçili bağlantı noktalarına gelen trafiğe izin veren grupları (Nsg'ler) otomatik olarak yapılandırır. Süresi dolduktan sonra Güvenlik Merkezi Nsg'ler önceki durumlarına geri yükler. Önceden oluşturulmuş bu bağlantılar, ancak kesilmez.

> [!NOTE]
> Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager üzerinden dağıtılan Vm'leri desteklemektedir. Klasik ve Resource Manager dağıtım modelleri hakkında daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Tam zamanında erişim kullanma

1. **Güvenlik Merkezi** panosunu açın.

2. Sol bölmede seçin **tam zamanında VM erişimi**.

![Tam zamanında VM erişimi Döşe][2]

**Tam zamanında VM erişimi** penceresi açılır.

![Tam zamanında VM erişimi Döşe][10]

**Tam zamanında VM erişimi**, VM’lerinizin durumu hakkında bilgiler sağlar:

- **Yapılandırılan** - Tam zamanında VM erişimini destekleyecek şekilde yapılandırılmış VM’lerdir. Sunulan veriler geçen haftaya aittir ve her VM için onaylanan istekler, son erişim tarihi ve saati ve son kullanıcı sayısını içerir.
- **Önerilen** - Tam zamanında VM erişimini destekleyebilen ancak bunun için yapılandırılmamış VM’lerdir. Bu VM'ler için yalnızca zaman VM erişim denetimi etkinleştirmenizi öneririz. Bkz [yapılandırma tam zamanında erişim ilkesi](#configuring-a-just-in-time-access-policy).
- **Öneri olmayan** - Bir VM’nin önerilmemesinin olası nedenleri şunlardır:
  - NSG yok - Tam zamanında erişim çözümü için NSG’nin mevcut olması gerekir.
  - Klasik VM - Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager üzerinden dağıtılan VM’leri desteklemektedir. Klasik dağıtım yalnızca tarafından desteklenmeyen çözümünü.
  - Diğer - Aboneliğin veya kaynak grubunun güvenlik ilkesinde tam zamanında erişim çözümü kapatılmışsa VM bu kategoridedir ya da bu kategorideki bir VM’de genel IP adresi eksik olabilir ve bir NSG mevcut olmayabilir.

## <a name="configuring-a-just-in-time-access-policy"></a>Yapılandırma tam zamanında erişim ilkesi

Etkinleştirmek istediğiniz Vm'leri seçmek için:

1. Altında **tam zamanında VM erişimi**seçin **önerilen** sekmesi.

  ![Tam zamanında erişim etkinleştir][3]

2. Altında **sanal makine**, etkinleştirmek istediğiniz Vm'leri seçin. Bu, bir VM yanında bir onay işareti koyar.
3. Seçin **etkinleştirme vm'lerde JIT**.
4. **Kaydet**’i seçin.

### <a name="default-ports"></a>Varsayılan bağlantı noktaları

Güvenlik Merkezi tam zamanında etkinleştirme önerdiği varsayılan bağlantı noktalarını görebilirsiniz.

1. Altında **tam zamanında VM erişimi**seçin **önerilen** sekmesi.

  ![Varsayılan bağlantı noktalarını görüntüle][6]

2. Altında **Vm'leri**, bir sanal Makineyi seçin. Bu sanal Makinenin yanında bir onay işareti koyar ve açılır **JIT VM erişimi Yapılandırması**. Bu dikey pencere varsayılan bağlantı noktalarını görüntüler.

### <a name="add-ports"></a>Bağlantı noktaları ekleme

Altında **JIT VM erişimi Yapılandırması**, ayrıca ekleyebilir ve tam olarak etkinleştirmek istediğiniz yeni bir bağlantı noktasını yapılandıran çözümünü.

1. Altında **JIT VM erişimi Yapılandırması**seçin **Ekle**. Bu açılır **bağlantı noktası yapılandırması Ekle**.

  ![Bağlantı noktası yapılandırması][7]

2. Altında **bağlantı noktası yapılandırması Ekle**, bağlantı noktası, protokol türü, izin verilen kaynak IP'leri ve en fazla istek süresi tanımlayın.

  Onaylanan bir isteğin ardından erişim elde etmesine izin verilen IP aralıkları izin verilen kaynak IP'leri var.

  En fazla istek süresi, belirli bir bağlantı noktası açılabilir en uzun süreyi penceredir.

3. **Tamam**’ı seçin.

## <a name="requesting-access-to-a-vm"></a>Bir VM için erişim isteği

VM'ye erişime izin istemek için:

1. Altında **tam zamanında VM erişimi**seçin **yapılandırıldı** sekmesi.
2. Altında **Vm'leri**, erişim sağlamak istediğiniz Vm'leri seçin. Bu, bir VM yanında bir onay işareti koyar.
3. Seçin **erişim isteği**. Bu açılır **erişim isteği**.

  ![VM'ye erişime izin iste][4]

4. Altında **erişim isteği**, her VM için bağlantı noktası için açık kaynak IP ve bağlantı noktası açıldığı zaman penceresi ile birlikte açmak için bağlantı noktalarını yapılandırın. Yalnızca yapılandırılan bağlantı noktaları için erişim isteğinde bulunabileceği zamanında ilkesi. Her bağlantı noktası yalnızca türetilmiş süresi izin verilen en fazla olan zamanında ilkesi.
5. Seçin **bağlantı noktalarını açmak**.

> [!NOTE]
> Bir kullanıcı bir sanal makine erişimine izin istediğinde, Güvenlik Merkezi kullanıcı olup olmadığını denetler [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) VM için yazma erişimi sağlayan izinleri. Yazma izinleriniz varsa, isteği onaylandı.
>
>

> [!NOTE]
> Erişim isteyen bir kullanıcı bir proxy'nin arkasındayken, "IP'mi" seçeneği çalışmayabilir. Kuruluş tam aralığını tanımlamak için bir gereksinim olabilir.
>
>

## <a name="editing-a-just-in-time-access-policy"></a>Düzenleme tam zamanında erişim ilkesi

Bir sanal makinenin yalnızca zaman ilkesinde varolan ekleyip bu VM için açmak için yeni bir bağlantı noktası yapılandırma ya da zaten korumalı bir bağlantı noktasına ilgili herhangi bir parametre değiştirerek değiştirebilirsiniz.

Yalnızca bir sanal makinenin saat İlkesi içinde varolan bir düzen için **yapılandırıldı** sekmesi kullanılır:

1. Altında **Vm'leri**, o sanal makine için satır içinde üç noktaya tıklayarak bir bağlantı eklemek için bir sanal Makineyi seçin. Bu, bir menü açılır.
2. Seçin **Düzenle** menüsünde. Bu açılır **JIT VM erişimi Yapılandırması**.

  ![İlkeyi düzenleme][8]

3. Altında **JIT VM erişimi Yapılandırması**ya da kendi bağlantı noktasına tıklayarak zaten korumalı olan bir bağlantı noktası var olan ayarları düzenleyebilirsiniz veya seçebilirsiniz **Ekle**. Bu açılır **bağlantı noktası yapılandırması Ekle**.

  ![Bağlantı Noktası Ekle][7]

4. Altında **bağlantı noktası yapılandırması Ekle**, bağlantı noktasını belirlemek, protokol türü, izin verilen kaynak IP'leri ve en fazla istek süresi.
5. **Tamam**’ı seçin.
6. **Kaydet**’i seçin.

## <a name="auditing-just-in-time-access-activity"></a>Yalnızca zaman erişim etkinliğini denetleme

Günlük araması'nı kullanarak VM etkinlikleri hakkında Öngörüler elde edebilirsiniz. Günlükleri görüntülemek için:

1. Altında **tam zamanında VM erişimi**seçin **yapılandırıldı** sekmesi.
2. Altında **Vm'leri**, o sanal makine için satır içinde üç noktaya tıklayarak ilgili bilgileri görüntülemek için bir sanal Makineyi seçin. Bu, bir menü açılır.
3. Seçin **etkinlik günlüğü** menüsünde. Bu açılır **etkinlik günlüğü**.

  ![Etkinlik günlüğü seçin][9]

  **Etkinlik günlüğü** önceki işlem saati, tarih ve abonelik yanı sıra bu VM için filtrelenmiş bir görünüm sağlar.

  ![Etkinlik Günlüğü Görüntüle][5]

Günlük bilgilerini seçerek indirebilirsiniz **tüm öğeleri CSV olarak indirmek için buraya tıklayın**.

Seç ve filtreleri değiştirmek **Uygula** arama ve günlük oluşturmak için.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Zamanında VM erişimi PowerShell aracılığıyla kullanarak

Yalnızca kullanmak için PowerShell aracılığıyla zaman çözümde olduğundan emin olun [son](/powershell/azure/install-azurerm-ps) Azure PowerShell sürümü.
Bunu yaptığınızda, yüklemeniz gerekir [son](https://aka.ms/asc-psgallery) Azure Güvenlik Merkezi PowerShell Galerisi'ndeki.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Yapılandırma tam zamanında ilkesi için bir VM

Yalnızca bir yapılandırma İlkesi zaman belirli bir VM'de PowerShell oturumunuzda bu komutu çalıştırmanız gerekir: Set-ASCJITAccessPolicy.
Daha fazla bilgi için cmdlet belgeleri izleyin.

### <a name="requesting-access-to-a-vm"></a>Bir VM için erişim isteği

Yalnızca korunan belirli bir sanal Makineye erişmek için PowerShell oturumunuzda bu komutu çalıştırmak gereken zaman çözümde: çağırma ASCJITAccess.
Daha fazla bilgi için cmdlet belgeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, nasıl yalnızca zamanında VM erişimi, Güvenlik Merkezi yardımcı olur, Azure sanal makineleri için erişim denetim öğrendiniz.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Güvenlik ilkelerini ayarlama](security-center-policies.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
- [Güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
- [Güvenlik durumunu izleme](security-center-monitoring.md) — Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
- [Yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
- [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) — iş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
- [Güvenlik Merkezi SSS](security-center-faq.md) — hizmet kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[10]: ./media/security-center-just-in-time/just-in-time-access.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
