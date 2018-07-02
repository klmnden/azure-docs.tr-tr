---
title: Azure İlkesi ile Azure Güvenlik Merkezi güvenlik ilkesi tümleştirmesi | Microsoft Docs
description: Bu belge, Azure İlkesi ile Azure Güvenlik Merkezi güvenlik ilkeleri tümleştirmesini yapılandırmanıza yardımcı olur.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2018
ms.author: terrylan
ms.openlocfilehash: b3d6d15d41fece613290deb2c77e980caa5dcfef
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37018572"
---
# <a name="integrate-security-center-security-policies-with-azure-policy"></a>Azure İlkesi ile Güvenlik Merkezi güvenlik ilkelerini tümleştirme
Bu makale, [Azure İlkesi](../azure-policy/azure-policy-introduction.md) ile desteklenen Azure Güvenlik Merkezi güvenlik ilkelerini yapılandırmanıza yardımcı olur.

## <a name="how-security-policies-work"></a>Güvenlik ilkeleri nasıl çalışır?
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. Güvenlik Merkezi'nde ilkeleri düzenleyebilir ya da Azure İlkesi’ni kullanarak aşağıdakileri yapabilirsiniz:
- Yeni ilke tanımları oluşturma.
- Bir kuruluşun tamamını veya kuruluş içindeki iş birimini temsil edebilen yönetim gruplarına ve aboneliklere ilkeler atama.
- İlke uyumluluğunu izleme.

Azure İlkesi hakkında daha fazla bilgi için [Uyumluluğu zorlamak için ilke oluşturma ve yönetme](../azure-policy/create-manage-policy.md) konusunu inceleyin.

Bir Azure ilkesi aşağıdaki bileşenlerden oluşur:

- **İlke**: Bir kuraldır.
- **Girişim**: İlkelerden oluşan bir koleksiyondur.
- **Atama**: Bir girişimin veya ilkenin belirli bir kapsama (yönetim grubu, abonelik veya kaynak grubu) uygulanmasıdır.

Bir kaynak, kendisine atanmış olan ilkelere göre değerlendirilir ve kaynağın uyumlu olduğu ilke sayısına göre bir uyumluluk oranına sahip olur.

## <a name="edit-security-policies"></a>Güvenlik ilkelerini düzenleme
Güvenlik Merkezi'nde tüm Azure aboneliklerinizin ve yönetim gruplarınızın varsayılan güvenlik ilkesini düzenleyebilirsiniz. Bir güvenlik ilkesini değiştirmek için abonelikte veya ilkeyi içeren yönetim grubunda sahip, katkıda bulunan veya güvenlik yöneticisi rolüne sahip olmanız gerekir. Güvenlik Merkezi'nde güvenlik ilkelerinizi görüntüleme:

1. **Güvenlik Merkezi** panosunun **İLKE VE UYUMLULUK** bölümünde **Güvenlik ilkesi**'ni seçin. **İlke Yönetimi** bölmesi açılır.

    ![İlke Yönetimi bölmesi](./media/security-center-azure-policy/security-center-policies-fig10.png)

  İlke Yönetimi bölmesinde yönetim grubu, abonelik ve çalışma alanı sayısının yanı sıra yönetim grubunuzun yapısı görüntülenir.

  > [!NOTE]
  > Güvenlik Merkezi panosunun **Abonelik kapsamı** bölümünde, **İlke Yönetimi** bölmesinde gösterilenden daha fazla abonelik görünebilir. Abonelik kapsamı Standart, Ücretsiz ve “kapsanmayan” aboneliklerin sayısını gösterir. “Kapsanmayan” aboneliklerde Güvenlik Merkezi etkin değildir ve bu abonelikler **İlke Yönetimi** bölmesinde görüntülenmez.
  >
  >

  Tablodaki sütunlar şunları gösterir:

 - İlke Girişimi Ataması: Bir aboneliğe veya yönetim grubuna atanmış olan yerleşik Güvenlik Merkezi ilkeleri ve girişimleridir.
 - Uyumluluk: Bir yönetim grubu, abonelik veya çalışma alanının genel uyumluluk puanını gösterir. Puan, atamaların ağırlıklı ortalamasıdır. Ağırlıklı ortalama, tek bir atamadaki ilke sayısını ve atamanın uygulandığı kaynak sayısını etkiler.

 Örneğin aboneliğinizde iki VM ve atanmış beş ilkeli bir girişim varsa aboneliğinizde 10 atama olur. VM'lerin biri ilkelerin ikisiyle uyumlu değilse aboneliğinizin genel uyumluluk puanı %80 olur.

 - Kapsam: Yönetim grubu, abonelik veya çalışma alanının üzerinde çalıştığı fiyatlandırma katmanını (Ücretsiz veya Standart) tanımlar.  Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
 - Ayarlar: Aboneliklerde **Ayarları düzenle** (bağlantısı bulunur). **Ayarları düzenle**'yi seçtiğinizde aboneliğinizin veri koleksiyonu, fiyatlandırma katmanı ve e-posta bildirimleri gibi ayarları güncelleştirebilirsiniz.

2. Güvenlik ilkesini etkinleştirmek istediğiniz aboneliği veya yönetim grubunu seçin. **Güvenlik ilkesi** açılır.

3.  **Güvenlik ilkesi** bölümünde **Açık**'ı seçerek Güvenlik Merkezi'nin izlemesini ve öneri sunmasını istediğiniz denetimleri seçin.  Güvenlik Merkezi'nin izlemesini istemediğiniz denetimler için **Kapalı**'yı seçin.

    ![İlke bileşenleri](./media/security-center-azure-policy/security-policy.png)

4. **Kaydet**’i seçin.

## <a name="available-security-policy-definitions"></a>Kullanılabilir güvenlik ilkesi tanımları

Varsayılan güvenlik ilkesinde mevcut olan ilke tanımlarını anlamak için aşağıdaki tabloya bakın:

| İlke | Etkin ilke ne yapar? |
| --- | --- |
| Sistem güncelleştirmeleri |Windows Update veya Windows Server Update Services kaynağından kullanılabilir güvenlik güncelleştirmelerinin ve kritik güncelleştirmelerin günlük listesini alır. Alınan liste, sanal makineleriniz için yapılandırılan hizmete bağlıdır ve eksik güncelleştirmelerin uygulanmasını önerir. Linux sistemleri için bu ilke, kullanılabilir güncelleştirmeleri olan paketleri belirlemek üzere distro ile sağlanan paket yönetim sistemini kullanır. Ayrıca, [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md) sanal makinelerinden güvenlik güncelleştirmelerini ve kritik güncelleştirmeleri denetler. |
| Güvenlik yapılandırmaları |Sanal makineyi saldırılara açık hale getirebilecek sorunları belirlemek üzere işletim sistemi yapılandırmalarını günlük olarak çözümler. İlke ayrıca bu güvenlik açıklarını gidermek üzere yapılandırma değişiklikleri yapılmasını önerir. İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için [önerilen temel kurallar listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) konusunu inceleyin. (Şu an için Windows Server 2016 tam olarak desteklenmemektedir.) |
| Uç nokta koruması |Virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olmak için tüm Windows sanal makinelerine (VM) sağlamak üzere uç nokta ayarlanmasını önerir. |
| Disk şifrelemesi |Bekleyen verilerin korunmasını geliştirmek için tüm sanal makinelerde disk şifrelemesini etkinleştirmeyi önerir. |
| Ağ güvenlik grupları |Ortak uç noktalara sahip sanal makinelere gelen ve giden trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/security-overview.md)'nın yapılandırılmasını önerir. Aksi belirtilmediği sürece bir alt ağ için yapılandırılan ağ güvenlik grupları tüm sanal makine ağ arabirimleri tarafından devralınır. Bir ağ güvenlik grubunun yapılandırılıp yapılandırılmadığını denetlemenin yanı sıra, bu ilke gelen trafiğe izin veren kuralları tanımlamak için gelen güvenlik kurallarını değerlendirir. |
| Web uygulaması güvenlik duvarı |Aşağıdaki durumlardan biri geçerli olduğunda sanal makinelerde bir web uygulaması güvenlik duvarının ayarlanmasını önerir: <ul><li>[Örnek düzeyinde ortak IP](../virtual-network/virtual-networks-instance-level-public-ip.md) kullanılır ve ilişkili ağ güvenlik grubuna yönelik gelen güvenlik kuralları 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır.</li><li>Yük dengeli IP kullanılır ve ilişkili yük dengeleme ve gelen ağ adresi çevirisi (NAT) kuralları, 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır. Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](../load-balancer/load-balancer-arm.md).</li> |
| Yeni nesil güvenlik duvarı |Ağ korumalarını Azure’da yerleşik olan ağ güvenlik gruplarının ötesine genişletir. Güvenlik Merkezi, yeni nesil güvenlik duvarının önerildiği dağıtımları bulur ve bundan sonra bir sanal gereç ayarlayabilirsiniz. |
| SQL denetimi ve tehdit algılama |Azure Veritabanı'na erişim denetiminin, araştırma amacıyla uyumluluk ve gelişmiş algılama için etkinleştirilmesini önerir. |
| SQL şifrelemesi |Azure SQL veritabanınız, ilişkili yedeklemeler ve işlem günlük dosyaları için bekleyen şifrelemenin etkinleştirilmesini önerir. Verilerinizi ihlal edilse bile okunabilir olmaz. |
| Güvenlik açığı değerlendirmesi |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| Depolama şifrelemesi |Şu anda bu özellik Azure Blob ve Azure Dosyaları için kullanılabilir. Depolama Hizmeti Şifrelemesi’ni etkinleştirdikten sonra yalnızca yeni veriler şifrelenir ve bu depolama hesabında var olan dosyalar şifrelenmemiş olarak kalır. |
| JIT ağ erişimi |Tam zamanında ağ erişimi etkinleştirildiğinde, Güvenlik Merkezi bir ağ güvenlik grubu kuralı oluşturarak Azure VM’lere gelen trafiği kilitler. Gelen trafiğin kilitlenmesi gereken VM bağlantı noktalarını seçersiniz. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |

## <a name="management-groups"></a>Yönetim grupları
Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure Yönetim Grupları, aboneliklerin üzerinde bir kapsam sunar. Abonelikleri "yönetim grupları" adlı kapsayıcılarla düzenler ve yönetim ilkelerinizi bu yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan ilkeleri devralır. Her dizinde "kök" yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu bulunur. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu kök yönetim grubu, genel ilkelerin ve RBAC atamalarının dizin düzeyinde uygulanmasını sağlar. Yönetim gruplarını Azure Güvenlik Merkezi'yle birlikte kullanacak şekilde ayarlamak için [Azure Güvenlik Merkezi için kiracı genelinde görünürlük kazanma](security-center-management-groups.md). 

> [!NOTE]
> Yönetim gruplarının ve aboneliklerin hiyerarşisini anlamanız önemlidir. Yönetim grupları, kök yönetimi ve yönetim grubu erişimi hakkında daha fazla bilgi edinmek için bkz. [Kaynaklarınızı Azure Yönetim Gruplarıyla düzenleme](../azure-resource-manager/management-groups-overview.md#root-management-group-for-each-directory).
>
>


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi ile ilgili tasarım konularını planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi için kiracı genelinde görünürlük kazanma](security-center-management-groups.md): Yönetim gruplarını Azure Güvenlik Merkezi için ayarlamayı öğrenin. 
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

Azure İlkesi hakkında daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)
