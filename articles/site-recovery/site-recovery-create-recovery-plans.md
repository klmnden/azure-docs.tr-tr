---
title: "Oluşturma ve yük devretme ve kurtarma Azure Site kurtarma için bir kurtarma planı özelleştirme | Microsoft Docs"
description: "Oluşturma ve Azure Site Recovery kurtarma planlarında özelleştirme hakkında bilgi edinin. Bu makalede, yük devri ve sanal makineleri ve fiziksel sunucuları kurtarma açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/26/2018
ms.author: raynew
ms.openlocfilehash: 6ad82a8a2f8e16ab794ba90c109904bd45cb6b89
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-recovery-plan-by-using-site-recovery"></a>Site Recovery kullanarak bir kurtarma planı oluşturun

Bu makalede nasıl oluşturulacağı ve bir kurtarma planı özelleştirme [Azure Site Recovery](site-recovery-overview.md).

Aşağıdakileri yapmak için bir kurtarma planı oluşturun:

* Birlikte yük devri ve birlikte Başlat makine grupları tanımlayın.
* Bunları bir kurtarma planı grubunda gruplamak makinelerini arasında model bağımlılıklar. Örneğin, yük devri ve belirli bir uygulamayı oluşturan hale getirmek için bu uygulama için tüm sanal makineleri aynı kurtarma planı grubunda içerir.
* Bir yük devretme çalıştırın. Bir test, planlanmış veya planlanmamış yük devretme bir kurtarma planı çalıştırabilirsiniz.

## <a name="why-use-a-recovery-plan"></a>Bir kurtarma planı neden kullanılır?

Bir kurtarma planı yönetebileceğiniz küçük bağımsız birimler oluşturarak sistematik kurtarma işlemi için hazırlanmanıza yardımcı olur. Birimleri, bir uygulama, ortamınızdaki genellikle temsil eder. Bir kurtarma planı içinde sanal makineleri Başlat dizisi tanımlamanıza yardımcı olabilir. Bir kurtarma planı, Kurtarma sırasında ortak görevleri otomatik hale getirmek için de kullanabilirsiniz.

> [!TIP]
> Bulut geçiş veya olağanüstü durum kurtarma için hazır olduğunu denetlemek için bir uygulamalarınızın her birinde, Kurtarma planının bir parçası olduğundan emin olmak için yoludur. Ayrıca, her kurtarma planı kurtarma Azure için test edildiğinden emin olun. Bu hazırlığı ile güvenle geçirmek veya tam merkeziniz Azure yük devredin.
 
Bir kurtarma planı üç temel değer önermeleri sahiptir:

* Uygulama bağımlılıkları yakalamak için model.
* RTO azaltmak için çoğu kurtarma görevleri otomatik hale getirme.
* Bir olağanüstü durum için hazır olması için yük devretme sınamasını.

### <a name="model-an-application-to-capture-dependencies"></a>Bağımlılıklar yakalamak için bir uygulama modeli

Bir kurtarma planı, genellikle bir uygulamayı oluşturan ve hangi birlikte yük devri sanal makinelerin bir gruptur. Kurtarma planı kullanarak oluşturur, uygulamaya özgü özelliklerinizi yakalamak için bir kurtarma planı Grup geliştirebilirsiniz. 

Bu makalede, son, ara yazılımı ve bir web ön uç geri SQL olabilir tipik bir üç katmanlı uygulama kullanırız. Sanal makineler yük devretme sonrasında doğru sırayla Başlat sağlamaya yardımcı olmak için kurtarma planını özelleştirebilirsiniz. SQL arka uç ilk başlamalıdır, Ara sonraki başlamalıdır ve web ön uç son başlamanız gerekir. Bu sırada, uygulamanın son sanal makine başlatıldığında tarafından çalıştığını sağlar. 

Örneğin, ara yazılım başladığında SQL katmanına bağlanmaya çalışır. Kurtarma planı SQL katmanı zaten çalışıyor sağlar. Bir ön uç sunucusu başlangıç son ayrıca tüm bileşenleri yukarı önce son kullanıcılara uygulama URL'sine bağlanma sağlamaya yardımcı olur sahip ve çalıştığından ve uygulama isteklerini kabul etmeye hazır. Bu bağımlılıklar oluşturmak için grup eklemek için kurtarma planını özelleştirin ve bir sanal makineyi seçin. Bir sanal makine grupları arasında taşımak için sanal makinenin grubunu değiştirin.

![Site Recovery örnek kurtarma planında ekran görüntüsü](./media/site-recovery-create-recovery-plans/rp.png)

Özelleştirme tamamladıktan sonra kurtarma tam adımlar görselleştirebilirsiniz. Bir kurtarma planı yük devretme sırasında yürütülen adımlarının sırasını şöyledir:

1. Sanal makineleri şirket içi devre dışı bırakma kapatma adım çalışır. (Birincil site sırasında çalıştırmaya devam etmek gereken bir test yük devretme kümesinde, dışında.)
2. Kapatma denemesini paralel kurtarma planındaki tüm sanal makinelerin yük devretme tetikler. Yük devretme adım çoğaltılmış verileri kullanarak sanal makine disklerini hazırlar.
3. Başlangıç grupları içindeki sıralarına yürütün ve her grupta olan sanal makineleri başlatın. İlk olarak, Grup 1 çalıştırır, sonra Grup 2 yürütür ve son olarak, Grup 3 yürütür. Herhangi bir grubu (örneğin, bir yük dengeli web ön uç) içinde birden fazla sanal makine varsa, tüm sanal makineleri paralel olarak başlatın.

> [!TIP]
> Sıralama grupları içindeki çeşitli uygulama katmanları arasındaki bağımlılıkları dikkate alınır, sağlar. Paralellik, uygulama kurtarma RTO kullanmak uygun olan yerlerde artırır.

   > [!NOTE]
   > Paralel olarak tek bir grubun parçası olan makinelere devredin. Farklı gruplarının bir parçası olan makinelere sırasına göre grupları yük devri. Yalnızca 1. Grup tüm makinelerin üzerinden başarısız olmuş ve başlatılan sonra Grup 2 makineler, yük devretme başlatın.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>RTO azaltmak için çoğu kurtarma görevleri otomatik hale getirme

Büyük uygulamalar kurtarma karmaşık bir görev olabilir. Karmaşık dünyada kez anımsaması birçok el ile yapılacak adımlar zordur ve hataya açık hale getirir. Ayrıca, kimin gerçekten yük devretmeyi tetikler uygulama ayrıntılı olarak incelenmektedir farkında değildir biri olabilir. 

Her aşamada yapmanıza gerek gerekli eylemleri otomatikleştirmek için bir kurtarma planı kullanabilirsiniz. Azure Otomasyonu runbook'ları kullanarak gerekli eylemleri ayarlayabilirsiniz. Runbook'ları ile görevleri aşağıdaki örnekteki gibi ortak kurtarma görevleri otomatik hale getirebilirsiniz. Otomatikleştirilemez görevler için kurtarma planlarınızı el ile yapılan eylemler ekleyebilirsiniz.

* **Azure sanal makinesi yük devretme sonrasında görevlerde**: Bu görevleri genellikle sanal makineye bağlanabilmesi için gereklidir. Örnekler:
    * Bir ortak IP adresi, sanal makine yük devretme sonrasında üzerinde oluşturun.
    * NIC başarısız üzerinden sanal makineye bir ağ güvenlik grubu atayın.
    * Bir yük dengeleyicisi bir kullanılabilirlik kümesi ekleyin.
* **Sanal makine yük devretme sonrasında içinde görevleri**: Bu görevleri, böylece yeni ortamda doğru şekilde çalışmaya devam eder, uygulama yeniden yapılandırın. Örnekler:
    * Sanal makine içinde veritabanı bağlantı dizesini değiştirin.
    * Web sunucusu yapılandırma veya kurallarını değiştirin.

> [!TIP]
> Tek tıklamayla yük devretme elde etmek ve Otomasyon runbook'ları kullanarak kurtarma sonrası görevleri otomatikleştiren bir tam kurtarma planı oluşturarak RTO en iyi duruma getirme.

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Bir olağanüstü durum için hazır olması için yük devretmeyi sınama

Bir kurtarma planı yük devretme veya yük devretme testi tetiklemek için kullanabilirsiniz. Her zaman bir yük devretme işleminden önce uygulamayı bir test Yük Devretmesini Tamamla. Test yük devretmeleri Yardım uygulama kurtarma sitesinde gelir olup olmadığını denetleyin. Bir şey, kurulumda atlandığında, kolayca temizleme tetiklenemedi ve yük devretme testi yineleyin. Uygulamanın düzgün kurtarır kadar yük devretme sınaması birden çok kez yapın.

![Ekran görüntüsü bir örnek, Site Recovery kurtarma planı test](./media/site-recovery-create-recovery-plans/rptest.png)

> [!TIP]
> Her uygulama benzersiz olduğundan her uygulama için özelleştirilmiş kurtarma planları yapı gerekir. 
>
> Ayrıca, günümüzün dinamik, veri merkezi odaklı ortamında, uygulamalar ve bağımlılıklarını sık değiştirin. Kurtarma planı geçerli olduğundan emin olmak için her üç uygulamalarınız için yük devretme testi.

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma

1. Seçin **kurtarma planlarına** > **kurtarma planı oluşturma**.
   Kurtarma planı ve kaynak ve hedef için bir ad belirtin. Kaynak konumun sanal makineler, yük devretme ve kurtarma için etkinleştirilmiş olması gerekir. Bir kaynak ve hedef kurtarma planının bir parçası olmasını istediğiniz sanal makinelere göre seçin. 

   |Senaryo                   |Kaynak               |Hedef           |
   |---------------------------|---------------------|-----------------|
   |Azure-Azure arası             |Azure bölgesi         |Azure bölgesi     |
   |Vmware’den Azure’a            |Yapılandırma sunucusu |Azure            |
   |Azure Virtual Machine Manager (VMM)               |VMM görünen adı    |Azure            |
   |Azure'da Hyper-V sitesi      |Hyper-V sitesi adı    |Azure            |
   |Fiziksel makineleri azure'a |Yapılandırma sunucusu |Azure            |
   |VMM VMM                 |VMM kolay adı    |VMM görünen adı|

   > [!NOTE]
   > Bir kurtarma planı aynı kaynak ve hedef sanal makineler içerebilir. VMware ve System Center Virtual Machine Manager (VMM) sanal makineleri aynı kurtarma planında olamaz. Ancak, aynı kurtarma planına VMware sanal makineleri ve fiziksel makineler ekleyebilirsiniz. Bu durumda, her iki makine için bir yapılandırma sunucusu kaynağıdır.

2. Altında **sanal makine Seç**, varsayılan grubuna (Grup 1) kurtarma planında eklemek istediğiniz sanal makineleri (veya çoğaltma grubu) seçin. Yalnızca kaynağı (Seçilen kurtarma planında) korumalı ve hangi (olarak seçilen kurtarma planında) hedefte korunan sanal makineleri seçebilirsiniz.

## <a name="customize-and-extend-recovery-plans"></a>Özelleştirme ve kurtarma planları genişletme

Özelleştirme ve kurtarma planları genişletmek için Site Recovery kurtarma planı kaynak bölmesine gidin. Seçin **Özelleştir** sekmesi. Özelleştirme ve aşağıdaki seçenekleri kullanarak kurtarma planları genişletme:

- **Yeni gruplar eklemek**: en çok yedi ek kurtarma planı grupları varsayılan grubuna ekleyebilirsiniz. Ardından, bu kurtarma planı gruplara daha fazla makine ya da çoğaltma grupları ekleyebilirsiniz. Grupları, bunları eklediğiniz sırayla numaralandırılır. Bir sanal makine ya da çoğaltma grubu yalnızca bir kurtarma planı grubunda içerebilir.
- **İşlemi el ile eklemek**: önce veya sonra bir kurtarma planı Grup çalıştırmak el ile yapılan eylemler ekleyebilirsiniz. Kurtarma planı çalıştığında işlemi el ile eklenen bir noktada durdurur. Bir iletişim kutusu, el ile gerçekleştirilen işlemin tamamlandığını belirtmenizi ister.
- **Bir komut dosyası eklemek**: önce veya sonra bir kurtarma planı Grup çalışan komut dosyaları ekleyebilirsiniz. Bir komut dosyası eklediğinizde, yeni bir grup için Eylemler kümesi ekler. Örneğin, Grup 1 öncesi adımları kümesini adıyla oluşturulur *Grup 1: öncesi adımları*. Tüm ön adımları bu kümesi içinde listelenir. Yalnızca dağıtılan bir VMM sunucunuz varsa, birincil sitede bir komut dosyası ekleyebilirsiniz. Daha fazla bilgi için bkz: [bir kurtarma planı için bir VMM komut dosyası eklemek](site-recovery-how-to-add-vmmscript.md).
- **Azure runbook'ları ekleyin**: Azure runbook'ları kullanarak kurtarma planları genişletebilirsiniz. Örneğin, görevleri otomatikleştirmek için ya da tek adımlı kurtarma oluşturmak için bir runbook kullanabilirsiniz. Daha fazla bilgi için bkz: [kurtarma planlarına eklemek Azure Otomasyon çalışma kitabı](site-recovery-runbook-automation.md).


## <a name="add-a-script-runbook-or-manual-action-to-a-plan"></a>Bir plana bir komut dosyası, runbook veya işlemi el ile Ekle

Varsayılan kurtarma planı grubuna sanal makine ya da çoğaltma grupları ekledikten sonra kurtarma planı grup için bir komut dosyası veya işlemi el ile ekleyebilirsiniz.

1. Kurtarma planı açın.
2. İçinde **adım** listesinde, bir öğe seçin. Ardından, seçin **betik** veya **el ile eylemi**.
3. Komut dosyası veya eylem önce veya sonra seçili öğe eklemek isteyip istemediğinizi belirtin. Komut dosyası konumunu yukarı veya aşağı taşımak için seçin **Yukarı Taşı** veya **Aşağı Taşı** düğmeler.
4. VMM komut dosyası eklerseniz, seçin **VMM komut dosyası için yük devretme**. İçinde **betik yolu**, paylaşımının göreli yolunu girin. Örneğin, **\RPScripts\RPScript.PS1**.
5. Bir Otomasyon runbook'u eklerseniz, runbook bulunduğu Otomasyon hesabı belirtin. Ardından, Azure runbook komut dosyasını seçin.
6. Betiği beklendiği gibi çalıştığından emin olmak için bir kurtarma planı yük devretmesi yapın.

Komut dosyası veya runbook seçenekleri, yalnızca aşağıdaki senaryolarda ve bir yük devretme veya yeniden çalışma sırasında kullanılabilir. İşlemi el ile hem yük devretme ve yeniden çalışma için kullanılabilir.


|Senaryo               |Yük devretme |Yeniden çalışma |
|-----------------------|---------|---------|
|Azure-Azure arası         |Runbook |Runbook  |
|Vmware’den Azure’a        |Runbook |NA       | 
|VMM'den Azure'a           |Runbook |Betik   |
|Azure'da Hyper-V sitesi  |Runbook |NA       |
|VMM VMM             |Betik   |Betik   |


## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [yerine çalıştıran](site-recovery-failover.md).  
* Eylem planlama kurtarma görmek için bu videoyu bakın:
    
    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
