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
ms.openlocfilehash: 5e07cd6891a5ab04012f819b5f6b9379312e530d
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="set-security-policies-in-security-center-powered-by-azure-policy"></a>Güvenlik Merkezi'nde Azure İlkesi destekli güvenlik ilkeleri belirleme
Bu belge, bu görevi gerçekleştirmeye ilişkin gerekli adımlarda size kılavuzluk ederek Güvenlik Merkezi'nde Azure İlkesi destekli güvenlik ilkeleri yapılandırmanıza yardımcı olur. 


## <a name="how-security-policies-work"></a>Güvenlik ilkeleri nasıl çalışır?
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. İlkeyi Güvenlik Merkezi'nde düzenleyebilir veya [Azure İlkesi](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)'ni kullanarak yeni ilke tanımları oluşturabilir, ilkeleri farklı Yönetim Gruplarına (kuruluşun tamamı, tek bir iş birimi vs. olabilir) atayabilir ve ilkelere uyum sağlanıp sağlanmadığını izleyebilirsiniz.

> [!NOTE]
> Azure İlkesi sınırlı önizleme aşamasındadır. Katılmak için [buraya](https://aka.ms/getpolicy) tıklayın. Azure İlkeleri hakkında daha fazla bilgi için bkz. [Uyumluluğu zorlamak için ilke oluşturma ve yönetme](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy).

## <a name="edit-security-policies"></a>Güvenlik ilkelerini düzenleme
Güvenlik Merkezi'nde tüm Azure aboneliklerinizin varsayılan güvenlik ilkesini düzenleyebilirsiniz. Bir güvenlik ilkesini değiştirmek için abonelikte veya ilkeyi içeren Yönetim Grubunda sahip, katkıda bulunan veya Güvenlik Yöneticisi rolüne sahip olmanız gerekir. Azure portalında oturum açın ve aşağıdaki adımları izleyerek Güvenlik Merkezi'nde güvenlik ilkelerini görüntüleyin:

1. **Güvenlik Merkezi** panosunun **Genel** bölümünde **Güvenlik İlkesi**'ne tıklayın.
2. Güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin.

    ![İlke Yönetimi](./media/security-center-policies/security-center-policies-fig10.png)

3. **İLKE BİLEŞENLERİ** bölümünde **Güvenlik ilkesi**'ne tıklayın.

    ![İlke bileşenleri](./media/security-center-policies/security-center-policies-fig12.png)

4. Bu, Azure İlkesi aracılığıyla Güvenlik Merkezi'ne atanmış olan varsayılan ilkedir. **İLKELER VE PARAMETRELER** altındaki öğeleri silebilir veya **KULLANILABİLİR SEÇENEKLER** altına başka ilke tanımları ekleyebilirsiniz. Bunun için tanım adının yanındaki artı işaretine tıklamanız yeterlidir.

    ![İlke tanımları](./media/security-center-policies/security-center-policies-fig11.png)

5. İlke hakkında daha ayrıntılı bir açıklamaya ihtiyacınız varsa üzerine tıklayarak ayrıntıları ve [ilke tanımı](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy/#policy-definition-structure) yapısını içeren JSON kodunun bulunduğu sayfanın açılmasını sağlayabilirsiniz:

    ![Json](./media/security-center-policies/security-center-policies-fig14.png)

6. Düzenlemeyi tamamladığınızda **Kaydet**'e tıklayın.


## <a name="available-security-policy-definitions"></a>Kullanılabilir güvenlik ilkesi tanımları

Varsayılan güvenlik ilkesinde mevcut olan ilke tanımlarını anlamak için aşağıdaki tabloyu kullanabilirsiniz: 

| İlke | Durum açık olduğunda |
| --- | --- |
| Sistem güncelleştirmeleri |Windows Update veya Windows Server Update Services kaynağından kullanılabilir güvenlik güncelleştirmelerinin ve kritik güncelleştirmelerin günlük listesini alır. Alınan liste ilgili sanal makine için yapılandırılan hizmete bağlıdır ve eksik güncelleştirmelerin uygulanmasını önerir. Linux sistemleri için bu ilke, kullanılabilir güncelleştirmeleri olan paketleri belirlemek üzere distro ile sağlanan paket yönetim sistemini kullanır. Ayrıca, [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md) sanal makinelerinden güvenlik güncelleştirmelerini ve kritik güncelleştirmeleri denetler. |
| İşletim sistemi güvenlik açıkları |Sanal makineyi saldırılara açık hale getirebilecek sorunları belirlemek üzere işletim sistemi yapılandırmalarını günlük olarak çözümler. İlke ayrıca bu güvenlik açıklarını gidermek üzere yapılandırma değişiklikleri yapılmasını önerir. İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için bkz. [önerilen temel kurallar listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (Şu an için Windows Server 2016 tam olarak desteklenmemektedir.) |
| Uç nokta koruması |Virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olmak için tüm Windows sanal makinelerine sağlamak üzere uç nokta koruması önerir. |
| Disk şifrelemesi |Bekleyen verilerin korunmasını geliştirmek için tüm sanal makinelerde disk şifrelemesini etkinleştirmeyi önerir. |
| Ağ güvenlik grupları |Ortak uç noktalara sahip sanal makinelere gelen ve giden trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md)'nın yapılandırılmasını önerir. Aksi belirtilmediği sürece bir alt ağ için yapılandırılan ağ güvenlik grupları tüm sanal makine ağ arabirimleri tarafından devralınır. Bir ağ güvenlik grubunun yapılandırılıp yapılandırılmadığını denetlemenin yanı sıra, bu ilke gelen trafiğe izin veren kuralları tanımlamak için gelen güvenlik kurallarını değerlendirir. |
| Web uygulaması güvenlik duvarı |Aşağıdaki koşullardan biri geçerli olduğunda sanal makinelerde bir web uygulaması güvenlik duvarının sağlanmasını önerir: </br></br>[Örnek Düzeyinde Ortak IP](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) kullanılır ve ilişkili ağ güvenlik grubuna yönelik gelen güvenlik kuralları 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır.</br></br>Yük dengeli IP kullanılır ve ilişkili yük dengeleme ve gelen ağ adresi çevirisi (NAT) kuralları, 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır. (Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](../load-balancer/load-balancer-arm.md). |
| Yeni nesil güvenlik duvarı |Ağ korumalarını Azure’da yerleşik olan ağ güvenlik gruplarının ötesine genişletir. Güvenlik Merkezi, yeni nesil güvenlik duvarının önerildiği dağıtımları bulur ve sanal gereç sağlamanıza imkan tanır. |
| SQL denetimi ve Tehdit algılama |Azure Veritabanı'na erişim denetiminin, araştırma amacıyla uyumluluk ve gelişmiş algılama için etkinleştirilmesini önerir. |
| SQL Şifrelemesi |Azure SQL Veritabanınız, ilişkili yedeklemeler ve işlem günlük dosyaları için bekleyen şifrelemenin etkinleştirilmesini önerir. Verilerinizi ihlal edilse bile okunabilir olmayacaktır. |
| Güvenlik açığı değerlendirmesi |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| Depolama Şifrelemesi |Şu anda bu özellik Azure Blob'ları ve Dosyaları için kullanılabilir. Depolama Hizmeti Şifrelemesi’ni etkinleştirdikten sonra yalnızca yeni veriler şifrelenir ve bu depolama hesabında var olan dosyalar şifrelenmemiş olarak kalır. |
| JIT Ağ Erişimi |Tam zamanında etkinleştirildiğinde, Güvenlik Merkezi bir NSG kuralı oluşturarak Azure VM’lere gelen trafiği kilitler. Gelen trafiğin kilitlenmesi gereken VM bağlantı noktalarını seçersiniz. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |


## <a name="next-step"></a>Sonraki adım
Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md). Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md). Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
