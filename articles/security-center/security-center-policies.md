---
title: Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama | Microsoft Belgeleri
description: Bu makale Azure Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: c68b55beba445b7f5d30efe7155a47e7f6f76690
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161297"
---
# <a name="set-security-policies-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama
Bu makale Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur.

PowerShell kullanarak ilkeler ayarlama konusunda yönergeler için bkz: [hızlı başlangıç: Azure RM PowerShell modülünü kullanarak uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturma](../azure-policy/assign-policy-definition-ps.md).

## <a name="how-security-policies-work"></a>Güvenlik ilkeleri nasıl çalışır?
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. Güvenlik Merkezi'nde ilkeleri düzenleyebilir ve ilke uyumluluğunu izleyebilirsiniz.

> [!NOTE]
> Artık Güvenlik Merkezi ilkelerini [Azure İlkesi](../azure-policy/azure-policy-introduction.md)'ni kullanarak genişletebilirsiniz. Daha fazla bilgi için bkz. [Azure İlkesi ile Güvenlik Merkezi güvenlik ilkelerini tümleştirme](security-center-azure-policy.md).

Geliştirme veya test için kullanılan kaynaklara yönelik güvenlik gereksinimleri, üretim uygulamaları için kullanılan kaynaklardan farklı gereksinimlere sahip olabilir. Kişisel bilgiler gibi düzenlenen veriler kullanan uygulamalar daha yüksek bir güvenlik düzeyi gerektirebilir. Azure Güvenlik Merkezi'nde etkinleştirilen güvenlik ilkeleri, olası güvenlik açıklarını tanımlamanıza ve tehdit risklerini azaltmanıza yardımcı olmak için güvenlik önerilerini ve izlemeyi yürütür. Hangi seçeneğin size uygun olduğuna karar vermeye yönelik daha fazla bilgi için bkz. [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md).

## <a name="edit-security-policies"></a>Güvenlik ilkelerini düzenleme
Güvenlik Merkezi'nde tüm Azure aboneliklerinizin varsayılan güvenlik ilkesini düzenleyebilirsiniz. Güvenlik ilkesini değiştirmek için aboneliğin sahibi, katkıda bulunanı veya güvenlik yöneticisi olmanız gerekir. Güvenlik Merkezi'nde güvenlik ilkelerinizi yapılandırmak için aşağıdakileri yapın:

1. Azure Portal’da oturum açın.

2. **Güvenlik Merkezi** panosunun **İLKE VE UYUMLULUK** bölümünde **Güvenlik ilkesi**'ni seçin.

3. Güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin.

4. Abonelik için etkinleştirmek istediğiniz ilkelerini etkinleştirin. Seçtiğiniz her ilkesine göre öneriler alırsınız. 
  ![ilke listesi](./media/security-center-policies/policies.png)
5. Düzenlemeyi tamamladığınızda **Kaydet**'i seçin.

## <a name="available-security-policy-definitions"></a>Kullanılabilir güvenlik ilkesi tanımları

Varsayılan güvenlik ilkesinde mevcut olan ilke tanımlarını anlamak için aşağıdaki tabloya bakın:

| İlke | İlkenin yaptığı |
| --- | --- |
| Sistem güncelleştirmeleri |Windows Update veya Windows Server Update Services kaynağından kullanılabilir güvenlik güncelleştirmelerinin ve kritik güncelleştirmelerin günlük listesini alır. Alınan liste, sanal makineleriniz için yapılandırılan hizmete bağlıdır ve eksik güncelleştirmelerin uygulanmasını önerir. Linux sistemleri için bu ilke, kullanılabilir güncelleştirmeleri olan paketleri belirlemek üzere distro ile sağlanan paket yönetim sistemini kullanır. Ayrıca, [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md) sanal makinelerinden güvenlik güncelleştirmelerini ve kritik güncelleştirmeleri denetler. |
| Güvenlik yapılandırmaları |Sanal makineyi saldırılara açık hale getirebilecek sorunları belirlemek üzere işletim sistemi yapılandırmalarını günlük olarak çözümler. İlke ayrıca bu güvenlik açıklarını gidermek üzere yapılandırma değişiklikleri yapılmasını önerir. İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için [önerilen temel kurallar listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) konusunu inceleyin. (Şu an için Windows Server 2016 tam olarak desteklenmemektedir.) |
| Uç nokta koruması |Virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olmak için tüm Windows sanal makinelerine (VM) sağlamak üzere uç nokta ayarlanmasını önerir. |
| Disk şifrelemesi |Bekleyen verilerin korunmasını geliştirmek için tüm sanal makinelerde disk şifrelemesini etkinleştirmeyi önerir. |
| Ağ güvenlik grupları |Ortak uç noktalara sahip sanal makinelere gelen ve giden trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/security-overview.md)'nın yapılandırılmasını önerir. Aksi belirtilmediği sürece bir alt ağ için yapılandırılan ağ güvenlik grupları tüm sanal makine ağ arabirimleri tarafından devralınır. Bir ağ güvenlik grubunun yapılandırılıp yapılandırılmadığını denetlemenin yanı sıra, bu ilke gelen trafiğe izin veren kuralları tanımlamak için gelen güvenlik kurallarını değerlendirir. |
| Web uygulaması güvenlik duvarı |Aşağıdaki koşullardan biri geçerli olduğunda sanal makinelerde bir web uygulaması güvenlik duvarının ayarlanmasını önerir: <ul><li>[Örnek düzeyinde ortak IP](../virtual-network/virtual-networks-instance-level-public-ip.md) kullanılır ve ilişkili ağ güvenlik grubuna yönelik gelen güvenlik kuralları 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır.</li><li>Yük dengeli IP kullanılır ve ilişkili yük dengeleme ve gelen ağ adresi çevirisi (NAT) kuralları, 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır. Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](../load-balancer/load-balancer-arm.md).</li> |
| Yeni nesil güvenlik duvarı |Ağ korumalarını Azure’da yerleşik olan ağ güvenlik gruplarının ötesine genişletir. Güvenlik Merkezi, yeni nesil güvenlik duvarının önerildiği dağıtımları bulur ve bundan sonra bir sanal gereç ayarlayabilirsiniz. |
| SQL denetimi ve tehdit algılama |SQL veritabanınıza erişim denetiminin, araştırma amacıyla uyumluluk ve gelişmiş algılama için etkinleştirilmesini önerir. |
| SQL şifrelemesi |SQL veritabanınız, ilişkili yedeklemeler ve işlem günlük dosyaları için bekleyen şifrelemenin etkinleştirilmesini önerir. Verilerinizi ihlal edilse bile okunabilir olmaz. |
| Güvenlik açığı değerlendirmesi |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| Depolama şifrelemesi |Şu anda bu özellik bloblar ve Azure Dosyaları için kullanılabilir. Depolama Hizmeti Şifrelemesi’ni etkinleştirdikten sonra yalnızca yeni veriler şifrelenir ve bu depolama hesabında var olan dosyalar şifrelenmemiş olarak kalır. |
| JIT ağ erişimi |Tam zamanında ağ erişimi etkinleştirildiğinde, Güvenlik Merkezi bir ağ güvenlik grubu kuralı oluşturarak Azure VM’lere gelen trafiği kilitler. Gelen trafiğin kilitlenmesi gereken VM bağlantı noktalarını seçersiniz. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi ile ilgili tasarım konularını planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

Azure İlkesi hakkında daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)
