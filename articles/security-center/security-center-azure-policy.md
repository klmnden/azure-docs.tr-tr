---
title: "Azure İlkesi ile Azure Güvenlik Merkezi güvenlik ilkesi tümleştirmesi | Microsoft Docs"
description: "Bu belge, Azure İlkesi ile Azure Güvenlik Merkezi güvenlik ilkeleri tümleştirmesini yapılandırmanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 045cf83caa15cb2487b4781f3c1f42752880580c
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="integrate-security-center-security-policies-with-azure-policy"></a>Azure İlkesi ile Güvenlik Merkezi güvenlik ilkelerini tümleştirme
Bu makale, Azure İlkesi ile desteklenen Azure Güvenlik Merkezi güvenlik ilkelerini yapılandırmanıza yardımcı olur. 

## <a name="how-security-policies-work"></a>Güvenlik ilkeleri nasıl çalışır?
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. Güvenlik Merkezi'nde ilkeleri düzenleyebilir ya da [Azure İlkesi](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)’ni kullanarak aşağıdakileri yapabilirsiniz:
* Yeni ilke tanımları oluşturma.
* Bir kuruluşun tamamını veya kuruluş içindeki iş birimini temsil edebilen yönetim gruplarına ilkeler atama.
* İlke uyumluluğunu izleme.

> [!NOTE]
> Azure İlkesi sınırlı önizleme aşamasındadır. Katılmak için [Azure İlkesine kaydolma](https://aka.ms/getpolicy) bölümüne gidin. Azure İlkesi hakkında daha fazla bilgi için [Uyumluluğu zorlamak için ilke oluşturma ve yönetme](http://docs.microsoft.com/azure/azure-policy/create-manage-policy) konusunu inceleyin.

## <a name="edit-security-policies"></a>Güvenlik ilkelerini düzenleme
Güvenlik Merkezi'nde tüm Azure aboneliklerinizin varsayılan güvenlik ilkesini düzenleyebilirsiniz. Bir güvenlik ilkesini değiştirmek için abonelikte veya ilkeyi içeren yönetim grubunda sahip, katkıda bulunan veya güvenlik yöneticisi rolüne sahip olmanız gerekir. Güvenlik ilkelerinizi Güvenlik Merkezi'nde görüntülemek için aşağıdakileri yapın:

1. Azure Portal’da oturum açın.

2. **Güvenlik Merkezi** panosunun **Genel** bölümünde **Güvenlik ilkesi**'ni seçin.

    ![İlke Yönetimi bölmesi](./media/security-center-azure-policy/security-center-policies-fig10.png)

3. Güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin.  

4. **İlke Bileşenleri** bölümünde **Güvenlik ilkesi**'ni seçin.  
    **Temel Bilgiler** penceresi açılır.

    ![İlke bileşenleri](./media/security-center-azure-policy/security-center-policies-fig12.png)

5. Bir ilke tanımını silmek için **İlkeler ve Parametreler** altında, silmek istediğiniz tanımın yanındaki **Sil** öğesini seçin.

6. **Kaydet** düğmesine tıklayın.  
    Azure İlkesi üzerinden Güvenlik Merkezi’ne atanmış varsayılan ilkeyi gösteren **Kullanılabilir Tanımlar** penceresi açılır. 

7. (İsteğe bağlı) **Kullanılabilir Tanımlar** penceresinde aşağıdakilerden birini yapın:

    * Bir ilke tanımı eklemek için tanımın yanındaki artı işaretini (+) seçin.

    ![Kullanılabilir ilke tanımları](./media/security-center-azure-policy/security-center-policies-fig11.png)

    * Bir ilkenin ayrıntılı açıklaması için ilkeyi seçin.  
    Tanım **Önizleme** penceresi açılır. Tanımın bir açıklaması ve [ilke tanımı](../azure-policy/policy-definition.md) yapısını sağlayan JSON kodunun bağlantısı gösterilir.

    ![Tanımın Önizleme penceresi](./media/security-center-azure-policy/security-center-policies-fig14.png)

7. Düzenlemeyi tamamladığınızda **Kaydet**'i seçin.

## <a name="available-security-policy-definitions"></a>Kullanılabilir güvenlik ilkesi tanımları

Varsayılan güvenlik ilkesinde mevcut olan ilke tanımlarını anlamak için aşağıdaki tabloya bakın: 

| İlke | Etkin ilke ne yapar? |
| --- | --- |
| Sistem güncelleştirmeleri |Windows Update veya Windows Server Update Services kaynağından kullanılabilir güvenlik güncelleştirmelerinin ve kritik güncelleştirmelerin günlük listesini alır. Alınan liste, sanal makineleriniz için yapılandırılan hizmete bağlıdır ve eksik güncelleştirmelerin uygulanmasını önerir. Linux sistemleri için bu ilke, kullanılabilir güncelleştirmeleri olan paketleri belirlemek üzere distro ile sağlanan paket yönetim sistemini kullanır. Ayrıca, [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md) sanal makinelerinden güvenlik güncelleştirmelerini ve kritik güncelleştirmeleri denetler. |
| İşletim sistemi güvenlik açıkları |Sanal makineyi saldırılara açık hale getirebilecek sorunları belirlemek üzere işletim sistemi yapılandırmalarını günlük olarak çözümler. İlke ayrıca bu güvenlik açıklarını gidermek üzere yapılandırma değişiklikleri yapılmasını önerir. İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için [önerilen temel kurallar listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) konusunu inceleyin. (Şu an için Windows Server 2016 tam olarak desteklenmemektedir.) |
| Uç nokta koruması |Virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olmak için tüm Windows sanal makinelerine (VM) sağlamak üzere uç nokta ayarlanmasını önerir. |
| Disk şifrelemesi |Bekleyen verilerin korunmasını geliştirmek için tüm sanal makinelerde disk şifrelemesini etkinleştirmeyi önerir. |
| Ağ güvenlik grupları |Ortak uç noktalara sahip sanal makinelere gelen ve giden trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md)'nın yapılandırılmasını önerir. Aksi belirtilmediği sürece bir alt ağ için yapılandırılan ağ güvenlik grupları tüm sanal makine ağ arabirimleri tarafından devralınır. Bir ağ güvenlik grubunun yapılandırılıp yapılandırılmadığını denetlemenin yanı sıra, bu ilke gelen trafiğe izin veren kuralları tanımlamak için gelen güvenlik kurallarını değerlendirir. |
| Web uygulaması güvenlik duvarı |Aşağıdaki koşullardan biri geçerli olduğunda sanal makinelerde bir web uygulaması güvenlik duvarının ayarlanmasını önerir: <ul><li>[Örnek düzeyinde ortak IP](../virtual-network/virtual-networks-instance-level-public-ip.md) kullanılır ve ilişkili ağ güvenlik grubuna yönelik gelen güvenlik kuralları 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır.</li><li>Yük dengeli IP kullanılır ve ilişkili yük dengeleme ve gelen ağ adresi çevirisi (NAT) kuralları, 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır. Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](../load-balancer/load-balancer-arm.md).</li> |
| Yeni nesil güvenlik duvarı |Ağ korumalarını Azure’da yerleşik olan ağ güvenlik gruplarının ötesine genişletir. Güvenlik Merkezi, yeni nesil güvenlik duvarının önerildiği dağıtımları bulur ve bundan sonra bir sanal gereç ayarlayabilirsiniz. |
| SQL denetimi ve tehdit algılama |Azure Veritabanı'na erişim denetiminin, araştırma amacıyla uyumluluk ve gelişmiş algılama için etkinleştirilmesini önerir. |
| SQL şifrelemesi |Azure SQL veritabanınız, ilişkili yedeklemeler ve işlem günlük dosyaları için bekleyen şifrelemenin etkinleştirilmesini önerir. Verilerinizi ihlal edilse bile okunabilir olmaz. |
| Güvenlik açığı değerlendirmesi |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| Depolama şifrelemesi |Şu anda bu özellik Azure Blob ve Azure Dosyaları için kullanılabilir. Depolama Hizmeti Şifrelemesi’ni etkinleştirdikten sonra yalnızca yeni veriler şifrelenir ve bu depolama hesabında var olan dosyalar şifrelenmemiş olarak kalır. |
| JIT ağ erişimi |Tam zamanında ağ erişimi etkinleştirildiğinde, Güvenlik Merkezi bir ağ güvenlik grubu kuralı oluşturarak Azure VM’lere gelen trafiği kilitler. Gelen trafiğin kilitlenmesi gereken VM bağlantı noktalarını seçersiniz. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi ile ilgili tasarım konularını planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
