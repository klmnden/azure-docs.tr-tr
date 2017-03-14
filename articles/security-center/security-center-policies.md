---
title: "Azure Güvenlik Merkezi&quot;nde güvenlik ilkelerini ayarlama | Microsoft Belgeleri"
description: "Bu belge, Azure Güvenlik Merkezi&quot;nde güvenlik ilkelerini yapılandırmanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/03/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: bd2291129a1a61f69e83cb76748d00b9ede6eb6f
ms.lasthandoff: 03/04/2017


---
# <a name="set-security-policies-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama
Bu belge, bu görevi gerçekleştirmeye ilişkin gerekli adımlarda size kılavuzluk ederek Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur.

## <a name="what-are-security-policies"></a>Güvenlik ilkeleri nedir?
Güvenlik ilkeleri, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar. Güvenlik Merkezi'nde şirketinizin güvenlik gereksinimlerine veya uygulamaların türüne ya da her abonelikteki verilerin duyarlılığına göre Azure abonelikleriniz veya kaynak grubu için ilkeler tanımlarsınız.

Örneğin, geliştirme veya test için kullanılan kaynaklar, üretim uygulamaları için kullanılan kaynaklardan farklı güvenlik gereksinimlerine sahip olabilir. Benzer şekilde, kişisel bilgiler gibi düzenlenen veriler kullanan uygulamalar daha yüksek bir güvenlik düzeyi gerektirebilir. Azure Güvenlik Merkezi'nde etkinleştirilen güvenlik ilkeleri, olası güvenlik açıklarını tanımlamanıza ve tehdit risklerini azaltmanıza yardımcı olmak için güvenlik önerilerini ve izlemeyi yürütür. Hangi seçeneğin size uygun olduğuna karar vermeye yönelik daha fazla bilgi için [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)’nu okuyun.

## <a name="set-security-policies-for-subscriptions"></a>Abonelikler için güvenlik ilkelerini ayarlama
Her bir abonelik veya kaynak grubu için güvenlik ilkeleri yapılandırabilirsiniz. Güvenlik ilkesini değiştirmek için o aboneliğin sahibi veya katkıda bulunanı olmanız gerekir. Azure portalında oturum açın ve aşağıdaki adımları izleyerek Güvenlik Merkezi'nde güvenlik ilkeleri yapılandırın:

1. Güvenlik Merkezi panosunda **İlke** kutucuğuna tıklayın.
2. Açılan **Güvenlik İlkesi - Her abonelik veya kaynak grubu için ilke tanımlama** dikey penceresinde, güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin. Aboneliğin tümü yerine bir kaynak grubuna ilişkin güvenlik ilkesini etkinleştirmeyi tercih ederseniz kaynak gruplarına ilişkin güvenlik ilkelerini ayarlama hakkında bilgi verilen bir sonraki bölüme inin.

    ![İlke tanımlama](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Seçili aboneliğin **Güvenlik ilkesi** dikey penceresi, aşağıdaki ekran görüntüsüne benzeyen bir seçenekler kümesiyle açılır:

    ![Veri koleksiyonunu etkinleştirme](./media/security-center-policies/security-center-policies-fig2-ga.png)

    Bu dikey pencerede kullanılabilen seçenekler şunlardır:

   * **Önleme ilkesi**: İlkeleri abonelik veya kaynak grubuna göre yapılandırmak için bu seçeneği belirleyin.  
   * **E-posta bildirimi**: Bir uyarının gün içinde ilk kez oluşması durumunda ve yüksek önem düzeyindeki uyarılar için bir e-posta bildirimi yapılandırmak üzere bu seçeneği kullanın. E-posta tercihleri yalnızca abonelik ilkeleri için yapılandırılabilir. E-posta bildirimi yapılandırma hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik kişi ayrıntılarını sağlama](security-center-provide-security-contact-details.md) konusunu okuyun.
   * **Fiyatlandırma katmanı**: Fiyatlandırma katmanı seçiminden yükseltmek için bu seçeneği kullanın. Fiyatlandırma seçenekleri hakkında daha fazla bilgi almak için [Güvenlik Merkezi fiyatlandırması](security-center-pricing.md) bölümüne bakın.
4. **Sanal makinelerden veri toplama** seçeneğinin **Açık** olduğundan emin olun. Bu seçenek, var olan ve yeni kaynaklar için otomatik günlük koleksiyonunu etkinleştirir.

   > [!NOTE]
   > Var olan ve yeni VM'lerin hepsinde güvenlik izleme özelliğinin kullanılabilir olduğundan emin olmak için her bir aboneliğinizde veri toplamayı açmanız önerilir. Veri koleksiyonunu etkinleştirme, izleme aracısını yükler. Veri koleksiyonunu şimdi bu konumdan açmak istemiyorsanız bu işlemi daha sonra **Sistem Durumu** ve **Öneriler** görünümlerinden gerçekleştirebilirsiniz. Ayrıca, yalnızca abonelik için veya VM'leri seçmek için de veri koleksiyonunu etkinleştirebilirsiniz. Desteklenen VM'ler hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) 
   >
   >
5. Depolama hesabınız henüz yapılandırılmamışsa **Güvenlik İlkesi**'ni açtığınızda aşağıdaki ekran görüntüsüne benzeyen bir uyarı görebilirsiniz. Her bölge için bir depolama hesabı seçmezseniz sizin için oluşturulur.

    ![Storage seçimi](./media/security-center-policies/security-center-policies-fig2.png)
6. Bu uyarıyı görürseniz bu seçeneğe tıklayıp aşağıdaki ekran görüntüsünde gösterildiği gibi bölgeyi seçin:

    ![Storage seçimi](./media/security-center-policies/security-center-policies-fig3-ga.png)
7. Sanal makinelerinizin çalıştığı her bir bölge için bu sanal makinelerden toplanan verilerin depolandığı depolama hesabını seçin. Bu işlem, gizlilik ve veri egemenliği amacıyla verileri aynı coğrafi alanda tutmanızı kolaylaştırır. Kullanacağınız bölgeye karar verdikten sonra bölgeyi ve ardından depolama hesabını seçin.
8. **Depolama hesaplarını seçme** dikey penceresinde **Tamam**'a tıklayın.

   > [!NOTE]
   > İsterseniz verileri çeşitli bölgelerdeki sanal makineler için merkezi bir depolama hesabında toplayabilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md).
   >
   >
9. Bu abonelikte kullanmak istediğiniz güvenlik önerilerini etkinleştirmek için **Güvenlik İlkesi** dikey penceresinde **Açık**'a tıklayın. Aşağıdaki ekran görüntüsüne benzer seçenekleri görmek için **Önleme ilkesi** öğesine tıklayın:

    ![Güvenlik ilkelerini seçme](./media/security-center-policies/security-center-policies-fig4-ga-new.png)

Her bir seçeneği anlamak için aşağıdaki tabloyu kullanın:

| İlke | Durum açık olduğunda |
| --- | --- |
| Sistem güncelleştirmeleri |Windows Update veya Windows Server Update Services kaynağından kullanılabilir güvenlik güncelleştirmelerinin ve kritik güncelleştirmelerin günlük listesini alır. Alınan liste ilgili sanal makine için yapılandırılan hizmete bağlıdır ve eksik güncelleştirmelerin uygulanmasını önerir. Linux sistemleri için bu ilke, kullanılabilir güncelleştirmeleri olan paketleri belirlemek üzere distro ile sağlanan paket yönetim sistemini kullanır. Ayrıca, [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md) sanal makinelerinden güvenlik güncelleştirmelerini ve kritik güncelleştirmeleri denetler. |
| İşletim sistemi güvenlik açıkları |Sanal makineyi saldırılara açık hale getirebilecek sorunları belirlemek üzere işletim sistemi yapılandırmalarını günlük olarak çözümler. İlke ayrıca bu güvenlik açıklarını gidermek üzere yapılandırma değişiklikleri yapılmasını önerir. İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için bkz. [önerilen temel kurallar listesi](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (Şu an için Windows Server 2016 tam olarak desteklenmemektedir.) |
| Uç nokta koruması |Virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olmak için tüm Windows sanal makinelerine sağlamak üzere uç nokta koruması önerir. |
| Disk şifrelemesi |Bekleyen verilerin korunmasını geliştirmek için tüm sanal makinelerde disk şifrelemesini etkinleştirmeyi önerir. |
| Ağ güvenlik grupları |Ortak uç noktalara sahip sanal makinelere gelen ve giden trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md)'nın yapılandırılmasını önerir. Aksi belirtilmediği sürece bir alt ağ için yapılandırılan ağ güvenlik grupları tüm sanal makine ağ arabirimleri tarafından devralınır. Bir ağ güvenlik grubunun yapılandırılıp yapılandırılmadığını denetlemenin yanı sıra, bu ilke gelen trafiğe izin veren kuralları tanımlamak için gelen güvenlik kurallarını değerlendirir. |
| Web uygulaması güvenlik duvarı |Aşağıdaki koşullardan biri geçerli olduğunda sanal makinelerde bir web uygulaması güvenlik duvarının sağlanmasını önerir:</br></br>[Örnek Düzeyinde Ortak IP](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) kullanılır ve ilişkili ağ güvenlik grubuna yönelik gelen güvenlik kuralları 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır.</br></br>Yük dengeli IP kullanılır ve ilişkili yük dengeleme ve gelen ağ adresi çevirisi (NAT) kuralları, 80/443 numaralı bağlantı noktasına erişime izin verecek şekilde yapılandırılır. (Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](../load-balancer/load-balancer-arm.md). |
| Yeni nesil güvenlik duvarı |Ağ korumalarını Azure’da yerleşik olan ağ güvenlik gruplarının ötesine genişletir. Güvenlik Merkezi, yeni nesil güvenlik duvarının önerildiği dağıtımları bulur ve sanal gereç sağlamanıza imkan tanır. |
| SQL denetimi ve Tehdit algılama |Azure Veritabanı'na erişim denetiminin, araştırma amacıyla uyumluluk ve gelişmiş algılama için etkinleştirilmesini önerir. |
| SQL saydam veri şifrelemesi |Azure SQL Veritabanınız, ilişkili yedeklemeler ve işlem günlük dosyaları için bekleyen şifrelemenin etkinleştirilmesini önerir. Verilerinizi ihlal edilse bile okunabilir olmayacaktır. |
| Güvenlik açığı değerlendirmesi |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |

Tüm seçenekleri yapılandırdıktan sonra öneriler içeren **Güvenlik İlkesi** dikey penceresinde **Tamam**'a tıklayın ve ilk ayarları içeren **Güvenlik İlkesi** dikey penceresinde **Kaydet**'e tıklayın.

## <a name="set-security-policies-for-resource-groups"></a>Kaynak gruplar için güvenlik ilkelerini ayarlama
Her kaynak grubu için güvenlik ilkelerinizi yapılandırmak isterseniz izleyeceğiniz adımlar aboneliklere ilişkin güvenlik ilkelerini ayarlamak için kullandığınız adımlara benzerdir. İkisi arasındaki temel fark, abonelik adını genişletmenizin ve benzersiz güvenlik ilkesini yapılandırmak istediğiniz kaynak grubunu seçmenizin gerekmesidir:

![Kaynak grubu seçimi](./media/security-center-policies/security-center-policies-fig5-ga.png)

Kaynak grubunu seçtikten sonra **Güvenlik ilkesi** dikey penceresi açılır. Varsayılan olarak, **Devralma** seçeneği etkindir. Bu durum kaynak grubu için tüm güvenlik ilkelerinin abonelik düzeyinden devralındığı anlamına gelir. Her kaynak grubu için özel güvenlik ilkesi istemeniz durumunda bu yapılandırmayı değiştirebilirsiniz. Bu durumda, **Benzersiz**'i seçip **Önleme ilkesi** seçeneğinde değişiklik yapmanız gerekir.

![Her kaynak grubu için güvenlik ilkesi](./media/security-center-policies/security-center-policies-fig6-ga.png)

> [!NOTE]
> Abonelik düzeyi ilkesi ile kaynak grubu düzeyi ilkesi arasında bir çakışma olması durumunda, kaynak grubu düzeyi ilkesi önceliklidir.
>
>

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md). Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md). Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

