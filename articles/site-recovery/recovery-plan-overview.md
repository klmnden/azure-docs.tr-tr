---
title: Azure Site Recovery kurtarma planları kullanarak | Microsoft Docs
description: Azure Site Recovery kurtarma planlarında hakkında bilgi edinin.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: raynew
ms.openlocfilehash: 871c9e8404438f966cab2fc5ab782e254295569e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30181986"
---
# <a name="about-recovery-plans"></a>Kurtarma planları hakkında

Bu makalede kurtarma planları [Azure Site Recovery](site-recovery-overview.md).

Bir kurtarma planı makineler kurtarma gruplar halinde toplar. Bir planı sırası, yönergeler ve görevler ekleyerek özelleştirebilirsiniz. Bir planı tanımlandıktan sonra bir yük devretme üzerinde çalıştırabilirsiniz.





## <a name="why-use-a-recovery-plan"></a>Bir kurtarma planı neden kullanılır?

Bir kurtarma planı yük devretme küçük bağımsız birimler oluşturarak sistematik kurtarma işlemi tanımlamak için yardımcı olur. Bir birim, genellikle bir uygulama, ortamınızdaki temsil eder. Bir kurtarma planı nasıl makineleri yük devri ve yük devretme sonrasında başlatmaları sırasını tanımlar. Kurtarma planları için kullanın:

* Bir uygulama bağımlılıklarını geçici model.
* RTO azaltmak için kurtarma görevleri otomatik hale getirme.
- Uygulamalarınızı, Kurtarma planının bir parçası olduğunu sağlayarak geçiş veya olağanüstü durum kurtarma için hazır doğrulayın.
* Yük devretme testi olağanüstü durum kurtarma veya geçiş beklendiği gibi çalıştığından emin olmak için kurtarma planları üzerinde çalıştırın.


## <a name="model-apps"></a>Model uygulamalar

Planlama ve uygulamaya özgü özellikleri yakalamak için bir kurtarma grubu oluşturun. Örnek olarak, şirketinizdeki bir genel üç katmanlı uygulama SQL server ile arka uç, ara yazılımı ve bir web ön uç göz önünde bulundurun. Genellikle, böylece her katmanındaki makinelerin yük devretme sonrasında doğru sırayla Başlat kurtarma planını özelleştirin.

    - SQL arka uç ilk olarak, ara yazılım başlaması gereken sonraki ve son olarak web ön uç.
    - Bu başlangıç sıra uygulama son makine başlatıldığında tarafından çalışmasını sağlar.
    - Bu sırada, ara yazılım başlar ve SQL Server katmana bağlanmak çalışır, SQL Server katmanı zaten çalışıyor sağlar. 
    - Bu sırada, ön uç sunucusu son başlatır ve böylece son kullanıcıların tüm bileşenleri yukarı önce uygulama URL'sini ve çalıştığından ve uygulama bağlanma istekleri kabul etmeye hazır olduğundan emin da yardımcı olur.

Bu sırada oluşturmak için kurtarma grubuna grupları ekleyin ve makineler grubuna ekleyin. 
    - Sipariş belirtilen yerlerde, sıralama kullanılır. Eylemler, uygun olan yerlerde, uygulama kurtarma RTO artırmak paralel olarak çalışır.
    - Tek bir grup makinelerde paralel olarak yük devri.
    - Böylece yalnızca Grup 1'deki tüm makineler üzerinden başarısız olmuş ve başlatılan sonra Grup 2 makineleri, kendi yük devretme başlatın. farklı gruplardaki makineler Grup sırayla yük devri.

    ![Örnek kurtarma planı](./media/recovery-plan-overview/rp.png)

Yerinde bu özelleştirme ile kurtarma planı üzerinde bir yük devretme çalıştırdığınızda şunlar olur: 

1. Kapatma adım, şirket içi makineleri kapatmanız dener. Yük devretme testi çalıştırmak, bu durumda birincil site çalışmaya devam eder istisnadır. 
2. Kapatma paralel bir yük devretme, tüm makineler kurtarma planında tetikler.
3. Yük devretme çoğaltılan veriler kullanılarak sanal makine disklerini hazırlar.
4. Başlangıç grupları sırada çalıştırın ve her bir grubu makineleri başlatın. İlk olarak, Grup 1 çalıştırır, sonra Grup 2 ve son olarak, Grup 3. Herhangi bir grubunda birden fazla makine varsa, tüm makinelerde paralel olarak başlatın.


## <a name="automate-tasks"></a>Görevleri otomatik hale getirme

Büyük uygulamalar kurtarma karmaşık bir görev olabilir. İşlemi el ile yapılacak adımlar hataya yatkın olun ve yük devretme çalıştıran kişinin tüm uygulama ayrıntılı olarak incelenmektedir uyumlu olmayabilir. Azure ya da komut dosyaları için yük devretme için Azure Otomasyonu runbook'ları kullanarak her adımda gerekli eylemleri otomatikleştirmek ve bir kurtarma planı sipariş koymak için kullanın. Otomatikleştirilemez görevler için kurtarma planları el ile gerçekleştirilen eylemleri duraklar ekleyebilirsiniz. Birkaç yapılandırabileceğiniz görevleri türleri şunlardır:

* **Yük devretme sonrasında Azure VM'de görevlerde**: Azure'a üzerinden çalıştırma işlemini gerçekleştiriyorsanız, genelde VM'ye yük devretme sonrasında bağlanabilmesi eylemleri gerçekleştirmek gerekir. Örneğin: 
    * Bir ortak IP adresi Azure VM oluşturun.
    * Bir ağ güvenlik grubu Azure VM ağ bağdaştırıcısına atayın.
    * Bir yük dengeleyicisi bir kullanılabilirlik kümesi ekleyin.
* **Yük devretme sonrasında VM içinde görevleri**: yeni ortamda düzgün çalışmaya devam şekilde bu görevleri genellikle makine üzerinde çalışan uygulama yeniden yapılandırın. Örneğin:
    * Makine içinde veritabanı bağlantı dizesini değiştirin.
    * Web sunucusu yapılandırma veya kurallarını değiştirin.


## <a name="test-failover"></a>Yük devretme sınaması

Bir kurtarma planı yük devretme testi tetiklemek için kullanabilirsiniz. Aşağıdaki en iyi yöntemleri kullanın:

- Tam bir yük devretme çalıştırmadan önce her zaman bir uygulama bir test Yük Devretmesini Tamamla. Yük devretme sınaması işlemlerini uygulama kurtarma sitesinde gelir olup olmadığını denetlemek için Yardım.
- Bir şey eksik bulursanız, Yukarı temiz tetikler ve yük devretme sınamasını yeniden çalıştırın. 
- Uygulama sorunsuz kurtarır emin kadar bir yük devretme testi birden çok kez çalıştırın.
- Her bir uygulama benzersiz olduğundan her uygulama için özelleştirilmiş kurtarma planları oluşturun ve her bir yük devretme testi çalıştırmak gerekir.
- Uygulamalar ve bağımlılıklarını sık değiştirin. Kurtarma planlarının güncel olduğundan emin olmak için yük devretme sınaması her uygulama için her üç çalıştırın.

    ![Ekran görüntüsü bir örnek, Site Recovery kurtarma planı test](./media/recovery-plan-overview/rptest.png)

## <a name="watch-the-video"></a>Videoyu izleme

İki katmanlı WordPress uygulaması için tıklatın üzerinde yük devretme gösteren bir örnek videoyu izleyin.
    
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="next-steps"></a>Sonraki adımlar

- [Oluşturma](site-recovery-create-recovery-plans.md) bir kurtarma planı.
* Hakkında bilgi edinin [yerine çalıştıran](site-recovery-failover.md).  
