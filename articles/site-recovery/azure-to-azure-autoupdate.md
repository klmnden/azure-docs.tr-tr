---
title: Mobility hizmetinin otomatik güncelleştirme için Azure Azure olağanüstü durum kurtarma | Microsoft Docs
description: Otomatik güncelleştirme Azure Azure Site Recovery kullanarak sanal makineleri çoğaltma için kullanılan Mobility hizmetinin genel bir bakış sağlar.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: rajanaki
ms.openlocfilehash: d9b653e4766746d2142a7e1040d6d60ec2aacc44
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="automatic-update-of-mobility-service-extension-in-azure-to-azure-replication"></a>Azure için Azure çoğaltma Mobility hizmeti uzantı otomatik güncelleştirilmesi

Azure Site Recovery, var olan özellikleri iyileştirme veya yenilerini eklenir ve bilinen sorunlar varsa sabit bir aylık bir yayın tempoyla sahiptir. Bu hizmetle geçerli kalmasını sağlamak için dağıtımı bu düzeltme ekleri için aylık plan gerektiğini anlamına gelir. Yükseltme işlemine ilişkili üzerinden head önlemek için kullanıcıların bunun yerine güncelleştirmeleri bileşenlerini yönetmek Site Recovery izin vermeyi seçebilirsiniz. İçinde ayrıntılı olarak [mimarisi referans](azure-to-azure-architecture.md) Azure için Azure olağanüstü durum kurtarma için Mobility hizmeti için çoğaltma etkin bir Azure sanal makineleri çoğaltılırken tüm Azure sanal makinelerinde yüklü başka bir bölge. Otomatik güncelleştirme etkinleştirdiğinizde, Mobility hizmeti uzantı her yeni sürümde güncelleştirilir. Bu belge aşağıdaki Ayrıntılar verilmiştir:

- Otomatik Güncelleştirme nasıl çalışır?
- Otomatik güncelleştirmeleri etkinleştirmek
- Ortak sorunlar ve sorun giderme
 
## <a name="how-does-automatic-update-work"></a>Otomatik Güncelleştirme nasıl çalışır

Güncelleştirmelerini yönetmek Site Recovery izin sonra (hangi Azure Hizmetleri tarafından kullanılır) genel bir runbook kasa ile aynı abonelikte oluşturulan bir Otomasyon hesabı aracılığıyla dağıtılır. Bir Otomasyon hesabı için belirli bir kasa kullanılır. Runbook her VM için Otomatik Güncelleştirmeler açık bir kasa içinde olup olmadığını denetler ve yeni bir sürümü varsa Mobility hizmeti uzantı yükseltmesini başlatır. Varsayılan zamanlama her gün şu saatte 12: 00'da çoğaltılan sanal makinenin coğrafi saat dilimini göredir runbook recurrs olan. Runbook zamanlama da Otomasyon hesabı kullanıcı tarafından gerekirse değiştirilebilir. 

> [!NOTE]
> Otomatik güncelleştirmeler etkinleştirildiğinde Azure Vm'leriniz yeniden gerektirmez ve devam eden çoğaltma etkilemez.

## <a name="enable-automatic-updates"></a>Otomatik güncelleştirmeleri etkinleştirmek

Aşağıdaki yollarla güncelleştirmeleri yönetmek Site Recovery izin vermeyi seçebilirsiniz:-

- [Etkinleştirme çoğaltma adımının bir parçası](#as-part-of-the-enable-replication-step)
- [İki durumlu uzantısı güncelleştirme kasa içinde ayarları](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Etkinleştirme çoğaltma adımının bir parçası:

Etkinleştirdiğinizde çoğaltma bir sanal makine için başlangıç ya da [sanal makine görünümünden](azure-to-azure-quickstart.md), veya [kurtarma Hizmetleri Kasası'ndan](azure-to-azure-how-to-enable-replication.md), ya da Site RECOVERY'yi izin vermek seçmek için bir seçenek alırsınız Site Recovery uzantısı veya el ile aynı yönetmek için güncelleştirmeleri yönetin.

![Çoğaltma otomatik güncelleştirmeyi etkinleştir](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>İki durumlu uzantısı güncelleştirme kasa içinde ayarları

1. Kasa içinde gitmek **Yönet**-> **Site Recovery altyapısı**
2. Altında **için Azure sanal makineleri**-> **uzantı güncelleştirme ayarları**, izin vermek isteyip istemediğinizi seçmek için Değiştir'i tıklatın *güncelleştirmeleri yönetmek için ASR* veya *el ile yönetme*. **Kaydet**’e tıklayın.

![iki durumlu autuo güncelleştirme kasası](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Seçtiğinizde *ASR yönetmek için izin*, ayar karşılık gelen kasayı tüm sanal makinelerin uygulanır.


> [!Note] 
> Her iki seçenek, güncelleştirmeleri yönetmek için kullanılan Otomasyon hesabının size bildirir. Bir kasadaki ilk kez bu özelliği etkinleştirmek, yeni bir Otomasyon hesabı oluşturulacak. Tüm sonraki etkinleştir çoğaltmalar aynı kasadaki önceden oluşturulmuş bir kullanır.

## <a name="common-issues--troubleshooting"></a>Ortak sorunlar ve sorun giderme

Otomatik Güncelleştirmeler ile ilgili bir sorun varsa, aynı kasa panosunda 'yapılandırma sorunları' altında bildirilir. 

Otomatik güncelleştirmeleri etkinleştirmek çalıştı ve başarısız durumunda, aşağıda sorun giderme için bkz.

**Hata**: bir Azure farklı çalıştır hesabı (hizmet sorumlusu) oluşturma ve hizmet sorumlusuna katkıda bulunan rolü verme izniniz yok. 
- Önerilen eylem: oturum açmış hesabı 'Katılımcısı' atandığından emin olup işlemi yeniden deneyin.
 
Otomatik Güncelleştirmeler açık sonra sorunların çoğu Site Recovery hizmeti tarafından gösteriyor ve tıklayarak gerektiriyor '**onarım**' düğmesi.

![Onarım düğmesi](./media/azure-to-azure-autoupdate/repair.png)

Onarım düğmesi kullanılamaz durumda uzantısı ayarları bölmesi altında görüntülenen hata iletisini bakın.

 - **Hata**: Farklı Çalıştır hesabı kurtarma Hizmetleri kaynağa erişmek için izni yok.

    **Önerilen eylem**: silin ve ardından [farklı çalıştır hesabını yeniden oluşturun](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account) veya Automation farklı çalıştır hesabının Azure Active Directory Uygulama Kurtarma Hizmetleri kaynağa erişim izni olduğundan emin olun.

- **Hata**: Farklı Çalıştır hesabı bulunamadı. Ya da bunlardan birini silinmiş veya oluşturulmamış - Azure Active Directory Uygulama, hizmet sorumlusu, rolü, Automation sertifikası varlığı, Otomasyon bağlantı varlığı - veya parmak izi sertifika ve bağlantısı arasında aynı değil. 

    **Önerilen eylem**: silin ve [farklı çalıştır hesabını yeniden oluşturun](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account).
