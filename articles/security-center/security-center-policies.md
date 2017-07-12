---
title: "Azure Güvenlik Merkezi&quot;nde güvenlik ilkelerini ayarlama | Microsoft Belgeleri"
description: "Bu belge, Azure Güvenlik Merkezi&quot;nde güvenlik ilkelerini yapılandırmanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 2593e6846c897644017083b49ad4ba8219696c6c
ms.contentlocale: tr-tr
ms.lasthandoff: 06/17/2017


---
<a id="set-security-policies-in-azure-security-center" class="xliff"></a>

# Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama
Bu belge, bu görevi gerçekleştirmeye ilişkin gerekli adımlarda size kılavuzluk ederek Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur.

>[!NOTE] 
>Haziran 2017'nin ilk günlerinden itibaren Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent'ı kullanacak. Daha fazla bilgi edinmek için [Azure Güvenlik Merkezi Platform Geçişi](security-center-platform-migration.md) makalesine bakın. Bu makaledeki bilgiler, Microsoft Monitoring Agent'a geçiş sonrasındaki Güvenlik Merkezi işlevselliğine yöneliktir.
>

<a id="what-are-security-policies" class="xliff"></a>

## Güvenlik ilkeleri nedir?
Güvenlik ilkesi, belirtilen abonelikteki kaynaklar için önerilen denetim kümesini tanımlar. Güvenlik Merkezi'nde şirketinizin güvenlik gereksinimlerine ve uygulamaların türüne ya da her abonelikteki verilerin gizliliğine göre Azure abonelikleriniz için ilkeler tanımlarsınız.

Örneğin, geliştirme veya test için kullanılan kaynaklar, üretim uygulamaları için kullanılan kaynaklardan farklı güvenlik gereksinimlerine sahip olabilir. Benzer şekilde, kişisel bilgiler gibi düzenlenen veriler kullanan uygulamalar daha yüksek bir güvenlik düzeyi gerektirebilir. Azure Güvenlik Merkezi'nde etkinleştirilen güvenlik ilkeleri, olası güvenlik açıklarını tanımlamanıza ve tehdit risklerini azaltmanıza yardımcı olmak için güvenlik önerilerini ve izlemeyi yürütür. Hangi seçeneğin size uygun olduğuna karar vermeye yönelik daha fazla bilgi için [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)’nu okuyun.

<a id="set-security-policies" class="xliff"></a>

## Güvenlik ilkeleri ayarlama
Her bir abonelik için güvenlik ilkeleri yapılandırabilirsiniz. Güvenlik ilkesini değiştirmek için o aboneliğin sahibi veya katkıda bulunanı olmanız gerekir. Azure portalında oturum açın ve aşağıdaki adımları izleyerek Güvenlik Merkezi'nde güvenlik ilkeleri yapılandırın:

1. Güvenlik Merkezi panosunda **İlke** kutucuğuna tıklayın.
2. Açılan Güvenlik İlkesi dikey penceresinde, güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin.

    ![İlke tanımlama](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Seçili abonelik için açılan **Güvenlik ilkesi** dikey penceresinde bir dizi seçenek bulunur. Bu dikey pencerede kullanılabilen seçenekler şunlardır:

   * **Önleme ilkesi**: İlkeleri abonelik veya kaynak grubuna göre yapılandırmak için bu seçeneği belirleyin.  
   * **E-posta bildirimi**: Bir uyarının gün içinde ilk kez oluşması durumunda ve yüksek önem düzeyindeki uyarılar için bir e-posta bildirimi yapılandırmak üzere bu seçeneği kullanın. E-posta tercihleri yalnızca abonelik ilkeleri için yapılandırılabilir. E-posta bildirimi yapılandırma hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik kişi ayrıntılarını sağlama](security-center-provide-security-contact-details.md) konusunu okuyun.
   * **Fiyatlandırma katmanı**: Fiyatlandırma katmanı seçiminden yükseltmek için bu seçeneği kullanın. Fiyatlandırma seçenekleri hakkında daha fazla bilgi almak için [Güvenlik Merkezi fiyatlandırması](security-center-pricing.md) bölümüne bakın.
4. **Sanal makinelerden veri toplama** seçeneğinin **Açık** olduğundan emin olun. Bu seçenek, Microsoft Monitoring Agent'ı (bu aracı aynı zamanda Operations Management Suite ve Log Analytics hizmeti tarafından da kullanılır) kullanan mevcut ve yeni kaynaklar için otomatik günlük toplamayı etkinleştirir. Bu aracıdan toplanan veriler, sanal makinenin coğrafi konumu göz önünde bulundurularak Azure aboneliğinizle ilişkili mevcut Log Analytics çalışma alanlarında veya yeni çalışma alanlarında depolanır.

5. **Güvenlik İlkesi** dikey penceresinde, kullanılabilen seçenekleri görmek için **Önleme ilkesi** seçeneğine tıklayın. Bu abonelikle ilgili güvenlik önerilerini etkinleştirmek için **Açık** seçeneğine tıklayın.

    ![Güvenlik ilkelerini seçme](./media/security-center-policies/security-center-policies-fig4-newUI.png)

Her bir seçeneği anlamak için aşağıdaki tabloyu kullanın:

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
| Depolama Şifrelemesi |Şu anda bu özellik Azure Blob'ları ve Dosyaları için kullanılabilir. Depolama Hizmeti Şifrelemesi’ni etkinleştirdikten sonra yalnızca yeni verilerin şifreleneceğini ve bu depolama hesabında var olan dosyaların şifrelenmemiş olarak kalacağını unutmayın. |

Tüm seçenekleri yapılandırdıktan sonra öneriler içeren **Güvenlik İlkesi** dikey penceresinde **Tamam**'a tıklayın ve ilk ayarları içeren **Güvenlik İlkesi** dikey penceresinde **Kaydet**'e tıklayın.

> [!NOTE]
> Fiyatlandırma katmanı, kaynak grubu düzeyi için hâlâ geçerlidir. Daha fazla bilgi için [Fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/) sayfasını ziyaret edin.
>
>

<a id="see-also" class="xliff"></a>

## Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md). Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md). Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

