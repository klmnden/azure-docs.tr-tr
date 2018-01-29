---
title: "Yalnızca zaman sanal makineye erişim Azure Güvenlik Merkezi'nde | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi'nde VM erişim yardımcı nasıl zamanında Azure sanal makinelerinizi erişimi denetleme gösterir."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/26/2018
ms.author: terrylan
ms.openlocfilehash: a15857f0df5c967031aed00d89e71b3199eed0c4
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="manage-virtual-machine-access-using-just-in-time-preview"></a>Tam zamanında (Önizleme) kullanarak sanal makine erişimini yönetme

Yalnızca zaman sanal makine (VM) erişim gerektiğinde VM'ler bağlamak için kolay erişim sağlarken saldırılara maruz kalma azaltma, Azure vm'lerine gelen trafik kilitlemek için kullanılabilir.

> [!NOTE]
> Özellik önizlemededir zaman yalnızca ve Güvenlik Merkezi'nin standart katmanında kullanılabilir.  Bkz: [fiyatlandırma](security-center-pricing.md) Güvenlik Merkezi hakkında daha fazla katmanları fiyatlandırma öğrenin.
>
>

## <a name="attack-scenario"></a>Saldırı senaryosu

Deneme yanılma saldırısı hedef yönetim noktaları VM erişmek için bir yol olarak sık saldırıları. Başarılı olursa, bir saldırganın VM üzerinden denetimini ele geçirmek ve bir açısından ortamınıza oluşturun.

Saldırılarına maruz azaltmak için tek bir bağlantı noktasının açık olduğundan süre miktarını sınırlamak için bir yoludur. Yönetim bağlantı noktalarını her zaman açık olması gerekmez. Bunlar yalnızca VM Yönetimi veya bakım görevleri gerçekleştirmek örnek için bağlıyken açık olması gerekir. Tam zamanında etkinleştirilmişse, Güvenlik Merkezi kullanır [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) saldırganlar tarafından hedeflenemez, yönetim bağlantı noktalarına erişimi sınırlayan (NSG) kuralları.

![Yalnızca zaman senaryoda][1]

## <a name="how-does-just-in-time-access-work"></a>Nasıl yalnızca süresi erişimi çalışıyor mu?

Tam zamanında etkinleştirildiğinde, Güvenlik Merkezi bir NSG kuralı oluşturarak Azure VM’lere gelen trafiği kilitler. Aşağı gelen trafik için kilitlenir VM bağlantı noktalarını seçin. Bu bağlantı noktaları yalnızca tarafından denetlenen zaman çözümde.

Bir kullanıcı bir VM erişim istediğinde, Güvenlik Merkezi kullanıcının sahip olduğunu denetler [rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md) VM için yazma erişimi sağlayan izinler. Belirtilen yazma izinlerine sahip oldukları isteğini onayladı ve Güvenlik Merkezi ağ güvenlik zaman miktarı yönetim bağlantı noktalarına gelen trafiğe izin verecek şekilde grupları (Nsg'ler) otomatik olarak yapılandırır. Güvenlik Merkezi Nsg'ler süresi dolduktan sonra önceki durumlarına geri yükler.

> [!NOTE]
> Güvenlik Merkezi'nde yalnızca zaman VM erişim şu anda yalnızca Azure Resource Manager aracılığıyla dağıtılan VM'ler destekler. Klasik ve Resource Manager dağıtım modelleri hakkında daha fazla bilgi için [Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Yalnızca süresi erişimi kullanma

1. **Güvenlik Merkezi** panosunu açın.

2. Sol bölmede seçin **saat VM erişimi hemen**.

![Tam zamanında VM erişim döşeme][2]

**Saat VM erişimi hemen** penceresi açılır.

![Tam zamanında VM erişim döşeme][10]

**Zaman VM Access'te yalnızca** Vm'leriniz durumu hakkında bilgi sağlar:

- **Yapılandırılmış** -yalnızca zaman VM erişimini desteklemek üzere yapılandırılmış VM'ler. Sunulan veriler son hafta ve her VM için onaylanan istekleri, son erişim tarihi ve saati ve son kullanıcı sayısını içerir.
- **Önerilen** -Vm'lerini yalnızca süresi VM erişimi destekler ancak için yapılandırılmamış. Yalnızca zaman VM erişim denetimi bu VM'ler için etkinleştirmenizi öneririz. Bkz: [yalnızca bir yapılandırma saat erişim ilkesinde](#configuring-a-just-in-time-access-policy).
- **Öneri yok** -önerilmez için bir VM neden nedenleri şunlardır:
  - NSG - yalnızca eksik zamanında çözüm bir NSG yerinde olmasını gerektirir.
  - Klasik VM - Güvenlik Merkezi'nde yalnızca zaman VM erişim şu anda yalnızca Azure Resource Manager aracılığıyla dağıtılan VM'ler destekler. Klasik dağıtım yalnızca tarafından desteklenmeyen zaman çözümde.
  - Bu kategorideki diğer - bir VM olan ise yalnızca çözüm kapalı abonelik veya kaynak grubu ya da, güvenlik ilkesi zaman VM, bir ortak IP eksik ve bir NSG yerinde sahip değil.

## <a name="configuring-a-just-in-time-access-policy"></a>Yalnızca bir yapılandırma saat erişim ilkesinde

Etkinleştirmek istediğiniz sanal makineleri seçmek için:

1. Altında **saat VM erişimi hemen**seçin **önerilen** sekmesi.

  ![Yalnızca süresi erişimi etkinleştir][3]

2. Altında **sanal makine**, etkinleştirmek istediğiniz sanal makineleri seçin. Bir VM yanında bir onay işareti koyar.
3. Seçin **etkinleştirmek JIT vm'lerde**.
4. **Kaydet**’i seçin.

### <a name="default-ports"></a>Varsayılan bağlantı noktaları

Güvenlik Merkezi tam zamanında etkinleştirme önerir varsayılan bağlantı noktalarını görebilirsiniz.

1. Altında **saat VM erişimi hemen**seçin **önerilen** sekmesi.

  ![Varsayılan bağlantı noktalarını görüntüle][6]

2. Altında **VM'ler**, bir VM seçin. Bu VM yanında bir onay işareti koyar ve açılır **JIT VM erişim yapılandırması**. Bu dikey varsayılan bağlantı noktalarını görüntüler.

### <a name="add-ports"></a>Bağlantı noktaları ekleme

Altında **JIT VM erişim yapılandırması**, ayrıca eklemek ve yapılandırmak istediğiniz yalnızca etkinleştirmek yeni bir bağlantı noktası zaman çözümde.

1. Altında **JIT VM erişim yapılandırması**seçin **Ekle**. Bu açılır **Ekle bağlantı noktası yapılandırmasını**.

  ![Bağlantı noktası yapılandırması][7]

2. Altında **Ekle bağlantı noktası yapılandırmasını**, bağlantı noktası, protokol türü, izin verilen kaynak IP'leri ve en fazla istek süresi tanımlayın.

  İzin verilen IP kaynağıdır onaylanmış bir istek üzerine erişmek için izin verilen IP aralıkları.

  En fazla istek süresi, belirli bir bağlantı noktasını açılabilir en uzun süre penceredir.

3. **Tamam**’ı seçin.

## <a name="requesting-access-to-a-vm"></a>Bir VM erişim isteyen

Bir VM erişim istemek için:

1. Altında **saat VM erişimi hemen**seçin **yapılandırıldı** sekmesi.
2. Altında **VM'ler**, erişimini etkinleştirmek istediğiniz sanal makineleri seçin. Bir VM yanında bir onay işareti koyar.
3. Seçin **erişim isteği**. Bu açılır **erişim isteği**.

  ![Erişim isteğinde bulunmak için bir VM][4]

4. Altında **erişim isteği**, her VM için bağlantı noktası için açılmış kaynak IP ve bağlantı noktası açıldığı zaman penceresi birlikte açmak için bağlantı noktalarını yapılandırın. Yalnızca içinde yapılandırılmış olan bağlantı noktalarına erişim isteğinde bulunabileceği zaman ilkesi. Her bağlantı noktası yalnızca türetilen süresi izin verilen maksimum olan zaman ilkesi.
5. Seçin **bağlantı noktalarını açmak**.

## <a name="editing-a-just-in-time-access-policy"></a>Yalnızca bir düzenleme zaman erişim ilkesinde

VM'in yalnızca zaman ilkesinde varolan ekleyerek ve o VM için açmak için yeni bir bağlantı noktası yapılandırma ya da zaten korumalı bir bağlantı noktasına ilgili herhangi bir parametre değiştirerek değiştirebilirsiniz.

Varolan yalnızca bir VM zaman İlkesi'nde düzenlemek için **yapılandırıldı** sekmesi kullanılır:

1. Altında **VM'ler**, o VM için üç nokta satırdaki tıklayarak bir bağlantı noktası eklemek için VM seçin. Bir menüdeki açılır.
2. Seçin **Düzenle** menüde. Bu açılır **JIT VM erişim yapılandırması**.

  ![İlkeyi düzenle][8]

3. Altında **JIT VM erişim yapılandırması**, kendi bağlantı noktasında tıklayarak ya da zaten korumalı olan bir bağlantı noktası var olan ayarları düzenleyebilirsiniz veya seçebileceğiniz **Ekle**. Bu açılır **Ekle bağlantı noktası yapılandırmasını**.

  ![Bir bağlantı noktası ekleme][7]

4. Altında **Ekle bağlantı noktası yapılandırmasını**, bağlantı noktasını belirlemek, protokol türü, izin verilen kaynak IP'leri ve en fazla istek süresi.
5. **Tamam**’ı seçin.
6. **Kaydet**’i seçin.

## <a name="auditing-just-in-time-access-activity"></a>Yalnızca zaman erişim etkinliğinde denetleme

Günlük arama kullanarak VM etkinlikleri Öngörüler elde edebilirsiniz. Günlükleri görüntülemek için:

1. Altında **saat VM erişimi hemen**seçin **yapılandırıldı** sekmesi.
2. Altında **VM'ler**, o VM için üç nokta satırdaki tıklayarak ilgili bilgileri görüntülemek için VM seçin. Bir menüdeki açılır.
3. Seçin **etkinlik günlüğü** menüde. Bu açılır **etkinlik günlüğü**.

  ![Etkinlik günlüğü seçin][9]

  **Etkinlik günlüğü** önceki işlem saati, tarih ve abonelik ile birlikte bu VM için filtre uygulanmış bir görünümünü sağlar.

  ![Etkinlik Günlüğü Görüntüle][5]

Günlük bilgilerini seçerek yükleyebilirsiniz **tüm öğeler CSV olarak indirmek için buraya tıklayın**.

Seç ve filtreleri değiştirmek **Uygula** bir arama ve günlük oluşturmak için.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Tam zamanında PowerShell aracılığıyla VM erişim kullanma

Yalnızca kullanmak için PowerShell aracılığıyla zaman çözümde olduğundan emin olun [son](/powershell/azure/install-azurerm-ps) Azure PowerShell sürümü.
Bunu yaptığınızda, yüklemek gereken [son](https://aka.ms/asc-psgallery) Azure Güvenlik Merkezi PowerShell Galerisi'nden.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Yalnızca bir yapılandırma bir VM için zaman İlkesi

Bir yalnızca yapılandırmak için PowerShell oturumunda bu komutu çalıştırmak gereken belirli bir VM'de zaman ilkesinde: Set-ASCJITAccessPolicy.
Daha fazla bilgi için cmdlet belgeleri izleyin.

### <a name="requesting-access-to-a-vm"></a>Bir VM erişim isteyen

Yalnızca ile korunan belirli bir VM'yi erişmek için PowerShell oturumunda bu komutu çalıştırmak gereken zaman çözümde: çağırma ASCJITAccess.
Daha fazla bilgi için cmdlet belgeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, nasıl tam zamanında VM erişim Güvenlik Merkezi yardımcı olur, Azure sanal makineleriniz için erişim denetim öğrendiniz.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Güvenlik ilkelerini ayarlama](security-center-policies.md) — Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
- [Güvenlik önerilerini yönetme](security-center-recommendations.md) — nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
- [Güvenlik durumunu izleme](security-center-monitoring.md) — Azure kaynaklarınızı sağlığını izlemek öğrenin.
- [Yönetme ve güvenlik uyarılarını yanıt](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
- [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) — iş ortağı çözümlerinizin sistem durumunu öğrenin.
- [Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) — hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
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
