---
title: Azure Site Recovery ile olağanüstü durum kurtarma, Kurtarma planlarını kullanarak | Microsoft Docs
description: Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için kurtarma planları kullanma hakkında bilgi edinin.
author: rayne-wiselman
manager: carmonm
services: site-recovery
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 520f30b5fabebf299b5407a502b76d7d30850bfd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797404"
---
# <a name="about-recovery-plans"></a>Kurtarma planları hakkında

Bu makalede kurtarma planlarında [Azure Site Recovery](site-recovery-overview.md).

Kurtarma planında makineleri kurtarma gruplar halinde toplar. Bir planı siparişi, yönergeleri ve görevler için ekleyerek özelleştirebilirsiniz. Bir plan tanımlandıktan sonra üzerinde bir yük devretme çalıştırabilirsiniz.



## <a name="why-use-a-recovery-plan"></a>Bir kurtarma planı neden kullanmalısınız?

Bir kurtarma planı yük devretme küçük bağımsız birimleri oluşturarak sistematik bir kurtarma işlemi tanımlamanıza yardımcı olur. Bir birim, ortamınızdaki bir uygulamanın genellikle temsil eder. Bir kurtarma planı nasıl makineler yük devretme ve yük devretme sonrasında başlatmaları sırasını tanımlar. Kurtarma planlarına kullanın:

* Bir uygulama etrafında bağımlılıklarını model.
* RTO azaltmak için kurtarma görevlerini otomatikleştirin.
* Uygulamalarınızı, Kurtarma planının bir parçası olduğunu sağlayarak geçiş veya olağanüstü durum kurtarma için hazır olmanız doğrulayın.
* Olağanüstü durum kurtarma veya geçiş beklendiği gibi çalıştığından emin olmak için kurtarma planları üzerinde yük devretme testi çalıştırın.


## <a name="model-apps"></a>Model uygulamalar

Planlama ve uygulamaya özgü özellikleri yakalamak için bir kurtarma grubu oluşturun. Örneğin, bir SQL server ile tipik bir üç katmanlı uygulama arka ucu, ara yazılım ve bir web ön ucu düşünelim. Genellikle, böylece her katmandaki makineler yük devretme sonrasında doğru sırayla Başlat kurtarma planını özelleştirin.

    - SQL arka uç, ilk olarak, ara yazılım başlaması gereken sonraki ve son olarak web ön ucu.
    - Bu başlatma sırasını, uygulamanın son makine başlatıldığında tarafından çalışmasını sağlar.
    - Bu sırada, ara yazılım başlar ve SQL Server katmanına bağlanmaya çalıştığında, SQL sunucusu katmanı zaten çalışıyor sağlar. 
    - Bu sırada, ön uç sunucusu son başlatır, son kullanıcılar, tüm bileşenleri hazır olduğunuzda önce uygulama URL'sini ve çalışan ve uygulama bağlamayın. böylece istek kabul etmeye hazır olun da yardımcı olur.

Bu sıralamayı oluşturmak için kurtarma grubuna grup ekleme ve gruplara makineleri ekleyin.
- Sipariş belirtilen yerlerde, sıralama kullanılır. Eylemler, uygun olduğunda, uygulama kurtarma RTO geliştirmek paralel olarak çalışır.
- Paralel olarak tek bir grupta makine devredin.
- Grup 1'deki tüm makineler yalnızca ve yük devretme başlatıldı sonra 2. Grup makineler, yük devretme başlatın. böylece farklı gruplardaki makine grubu sırayla devredin.

    ![Örnek kurtarma planı](./media/recovery-plan-overview/rp.png)

Bu özelleştirme yerinde kurtarma planı üzerinde bir yük devretme çalıştırdığınızda şunlar olur: 

1. Kapatma adım, şirket içi makineleri kapatmanız dener. Yük devretme testi çalıştırırsanız, bu durumda birincil sitenin çalışmaya devam eder. istisnadır. 
2. Kapatma, Kurtarma planında tüm makinelerin paralel bir yük devretmeyi tetikler.
3. Yük devretme çoğaltılan verileri kullanarak sanal makine disklerini hazırlar.
4. Başlangıç grupları sırayla çalıştırın ve her grup makineleri başlatmayın. İlk olarak, 1. Grup çalıştırır, sonra Grup 2 ve son olarak, Grup 3. Herhangi bir grupta birden fazla makine varsa, tüm makineler paralel olarak başlatın.


## <a name="automate-tasks"></a>Görevleri otomatikleştirme

Büyük uygulamalar kurtarma karmaşık bir görev olabilir. İşlemi el ile yapılacak adımlar hataya açık olun ve yük devretme çalıştıran kişinin tüm uygulama ayrıntılı olarak incelenmektedir haberdar olmayabilir. Sipariş koymak için bir kurtarma planı kullanın ve Azure ya da komut dosyaları için yük devretme için Azure Otomasyonu runbook'ları kullanarak her adımda gerekli eylemleri otomatik hale getirin. Otomatik olarak yapılamayan görevler için kurtarma planlarına yönelik el ile gerçekleştirilen eylemler ekleyebilirsiniz. Birkaç tür yapılandırabileceğiniz görevleri vardır:

* **Yük devretme sonrasında Azure VM'deki görevleri**: Azure'a devretmek, genellikle yük devretmeden sonra VM'ye bağlanabilmek eylemleri gerçekleştirmek için gerekir. Örneğin: 
    * Azure VM'de genel bir IP adresi oluşturun.
    * Bir ağ güvenlik grubu, Azure sanal ağ bağdaştırıcısına atayın.
    * Bir yük dengeleyici için bir kullanılabilirlik kümesi ekleyin.
* **Yük devretme sonrasında VM'nin içindeki görevleri**: Yeni ortamda düzgün çalışmaya devam eder, bu görevleri genellikle makine üzerinde çalışan uygulama yeniden yapılandırın. Örneğin:
    * Makine içinde veritabanı bağlantı dizesini değiştirin.
    * Web sunucusu yapılandırma veya kurallarını değiştirin.


## <a name="test-failover"></a>Yük devretme testi

Bir kurtarma planı test yük devretmeyi tetiklemek için kullanabilirsiniz. Aşağıdaki en iyi yöntemleri kullanın:

- Tam bir yük devretme çalıştırmadan önce her zaman bir uygulama üzerinde bir yük devretme tamamlayın. Yük devretme testi, uygulama kurtarma sitesinde gelir olup olmadığını denetlemek için yardımcı olur.
- Bir şeyler eksik bulursanız, bir temiz yedekleme tetikleyin ve sonra Yük devretme testi yeniden çalıştırın. 
- Uygulamanın düzgün kurtarır emin oluncaya kadar birden çok kez yük devretme testi çalıştırın.
- Her uygulamanın benzersiz olduğundan her uygulama için özelleştirilmiş kurtarma planları oluşturun ve her bir yük devretme testi çalıştırmak gerekir.
- Uygulamaları ve bunların bağımlılıklarını sık sık değiştirir. Kurtarma planları güncel olduğundan emin olmak için her üç her uygulama için bir yük devretme testi çalıştırın.

    ![Örnek ekran görüntüsünde, Site Recovery, kurtarma planı test](./media/recovery-plan-overview/rptest.png)

## <a name="watch-the-video"></a>Videoyu izleme

İki katmanlı bir WordPress uygulaması için bir tıklamayla üzerinde yük devretme gösteren bir örnek hızlı videosunu izleyin.
    
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="next-steps"></a>Sonraki adımlar

- [Oluşturma](site-recovery-create-recovery-plans.md) bir kurtarma planı.
- Hakkında bilgi edinin [devretme testlerini çalıştırma](site-recovery-failover.md).  
