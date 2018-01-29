---
title: "Oluşturma ve yük devretme ve kurtarma Azure Site kurtarma için kurtarma planları özelleştirme | Microsoft Docs"
description: "Oluşturma ve yük devri ve sanal makineleri ve fiziksel sunucuları kurtarmak için Azure Site Recovery kurtarma planlarında özelleştirme açıklar"
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
ms.openlocfilehash: 9839a989246b28c1a194b8d1f0e99c1bd80ac2e5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="create-recovery-plans"></a>Kurtarma planları oluşturma


Bu makalede oluşturma ve kurtarma planları özelleştirme hakkında bilgiler sağlanmaktadır [Azure Site Recovery](site-recovery-overview.md).

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

 Aşağıdakileri yapmak için kurtarma planları oluşturabilirsiniz:

* Birlikte yük devri ve birlikte başlatma makine grupları tanımlayın.
* Model bağımlılıkları birlikte kurtarma gruplandırarak tarafından makineler arasında Grup planlayın. Örneğin, yük devri ve belirli bir uygulamayı oluşturan hale getirmek için tüm VM'ler bu uygulamaya aynı kurtarma planı grup için Grup.
* Bir yük devretme çalıştırın. Bir test, planlanmış veya planlanmamış yük devretme bir kurtarma planı çalıştırabilirsiniz.

## <a name="why-use-recovery-plans"></a>Kurtarma planları neden kullanılır?

Kurtarma planları yönetebileceğiniz küçük bağımsız birimler oluşturarak sistematik kurtarma işlemi için planlamanıza yardımcı olması. Bu birimleri genellikle uygulamanın ortamınızdaki temsil eder. Kurtarma planı yalnızca, sanal makineleri Başlat, ancak ayrıca Kurtarma sırasında ortak görevleri otomatikleştirmenize yardımcı serisi tanımlamanızı sağlar.


**Buluta geçiş için hazırlanır veya olağanüstü durum kurtarma her uygulamanıza bir kurtarma planı bir parçasıdır ve her kurtarma planları kurtarma için Microsoft Azure sınanır sağlayarak olduğunu denetlemek için esas olarak, bir yolu. Bu hazırlığı ile güvenle geçirebileceğiniz veya yük devretme tam merkeziniz Microsoft Azure.**
 
Kurtarma planının üç temel değer önermeleri şunlardır:

### <a name="model-an-application-to-capture-dependencies"></a>Bağımlılıklar yakalamak için bir uygulama modeli

Bir kurtarma planı, sanal makinelerin genellikle uygulamanın birlikte bu yük devretme oluşan bir gruptur. Kurtarma planı kullanarak oluşturur, uygulamaya özgü özelliklerinizi yakalamak için bu grubu geliştirebilirsiniz.
 
Tipik bir üç katmanı uygulaması örneği bize alın

* bir SQL arka uç
* bir ara yazılım
* bir web ön uç

Kurtarma planı, sanal makinelerin doğru sırayla postasına bir yük devretme gündeme emin olmak için özelleştirilebilir. SQL arka uç ilk alınması gereken, ara yazılım sıradaki gelmelidir ve web ön uç son gelmelidir. Bu uygulamanın son sanal makine gelir zamana göre çalıştığını belirli bir sıraya. Örneğin, ara yazılım geldiğinde SQL katmana bağlanmak deneyin ve kurtarma planı SQL katmanı çalışmakta olduğunu güvence altına. Ön uç sunucuları son de gelen son kullanıcılar için uygulama URL'si yanlışlıkla tüm bileşenleri yukarı çalıştırıyorsanız ve uygulamanın isteklerini kabul etmeye hazır olmasını kadar bağlanmamanız sağlar. Bu bağımlılıklar oluşturmak için gruplar eklemek için kurtarma planını özelleştirebilirsiniz. Ardından sanal makineyi seçin ve grupları arasında taşımak için grubu değiştirin.

![Örnek kurtarma planı](./media/site-recovery-create-recovery-plans/rp.png)

Özelleştirme tamamladıktan sonra kurtarma tam adımlar görselleştirebilirsiniz. Bir kurtarma planı yük devretme sırasında yürütülen adımlarının sırasını şöyledir:

* İlk sanal makineleri şirket içi devre dışı bırakma denemesi kapatma adım yoktur (birincil site burada gereken çalıştırması devam etmek için test yük devretme kümesinde hariç)
* Sonraki paralel kurtarma planının tüm sanal makinelerin yük devretme tetikler. Yük devretme adım çoğaltılan verilerin sanal makineler disklerden hazırlar.
* Son olarak Başlangıç grupları içindeki sıralarına sanal makinelerin her grubu - 1. Grup, başlatmayı sonra Grup 2 ve son olarak Grup 3 yürütün. Varsa birden fazla sanal makine herhangi bir grubunda (örneğin, bir yük dengeli web ön uç) bunların tümünün yukarı paralel olarak önyükleme yapar.

**Sıralama grupları içindeki çeşitli uygulama katmanları arasındaki bağımlılıkları dikkate alınır ve uygulama kurtarma RTO paralellik uygun olan yerlerde artırır sağlar.**

   > [!NOTE]
   > Tek bir grubun parçası olan makinelere yük devretme paralel olur. Farklı gruplarının bir parçası olan makinelere gruplarının sıralanan yük olur. Yalnızca 1. Grup tüm makinelerin üzerinden başarısız olmuş ve önyükleme sonra Grup 2 makinelerin kendi yük devretme başlatın.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>RTO azaltmak için çoğu kurtarma görevleri otomatik hale getirme

Büyük uygulamalar kurtarma karmaşık bir görev olabilir. Yük devretme veya geçiş sonrası tam özelleştirme adımları anımsaması zordur. Bazı durumlarda, ancak başka birinin uygulama ayrıntılı olarak incelenmektedir, farkında olan yük devretme tetiklemek için gereken olmadığı. El ile çok fazla kez karmaşık dünyada adımlarda zordur anımsama ve hataya. Bir kurtarma planı Microsoft Azure Otomasyonu runbook'ları kullanarak her adımda, uygulamanız gereken gerekli eylemleri otomatikleştirmek için bir yol sağlar. Runbook'ları ile aşağıda verilen örneklerde gibi ortak kurtarma görevleri otomatik hale getirebilirsiniz. Otomatik olarak bu görevler için kurtarma planları Ayrıca, el ile gerçekleştirilen eylemleri ekleme olanağı sağlar.

* Yük devretme sonrası görevleri Azure sanal makinede – sanal makine için örneğin bağlanabilmesi için bu genellikle gereklidir:
    * Sanal makine post Yük Devretmesini genel IP oluşturun
    * Bir NSG'yi sanal makineye ait NIC başarısız atayın
    * Bir yük dengeleyicisi bir kullanılabilirlik kümesine ekleme
* Yük devretme sonrası sanal makine içinde görevleri – düzgün yeni ortamında, örneğin çalışmaya devam eder, bu uygulama yeniden yapılandırın:
    * Sanal makine içinde veritabanı bağlantı dizesini değiştirin
    * Web sunucusu yapılandırma/kuralları değiştirme

**Otomasyon runbook'ları kullanarak post kurtarma görevleri otomatikleştiren bir tam kurtarma planı ile tek tıklatmayla yük devretme elde etmek ve RTO en iyi duruma getirme.**

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Bir olağanüstü durum için hazır olması için yük devretmeyi sınama

Bir kurtarma planı yük devretme veya yük devretme testi tetiklemek için kullanılabilir. Her zaman bir yük devretme işleminden önce uygulamayı bir test yük devretmeyi tamamlamanız gerekir. Yük devretme testi, uygulama kurtarma sitesinde gelecek olup olmadığını denetlemek için yardımcı olur.  Bir şey atlandığında kolayca temizleme tetiklenemedi ve yük devretme testi yineleyin. Uygulamanın düzgün kurtarır kesin olarak bildiğiniz kadar yük devretme sınaması birden çok kez yapın.

![Test kurtarma planı](./media/site-recovery-create-recovery-plans/rptest.png)

**Her uygulama farklıdır ve her biri için özelleştirilmiş kurtarma planları yapı gerekir. Ayrıca, bu dinamik veri merkezinde dünyasında, uygulamalar ve bağımlılıklarını değiştirme tutun. Yük devretme kurtarma planı geçerli olup olmadığını denetlemek için uygulamalarınızın bir Çeyreğin sınayın.**

## <a name="how-to-create-a-recovery-plan"></a>Bir kurtarma planı oluşturma

1. Tıklatın **kurtarma planlarına** > **kurtarma planı oluşturma**.
   Kurtarma planı ve kaynak ve hedef için bir ad belirtin. Kaynak konumun sanal makineler, yük devretme ve kurtarma için etkinleştirilmiş olması gerekir. Bir kaynak ve hedef kurtarma planının bir parçası olmasını istediğiniz sanal makinelere göre seçin. 

   |Senaryo                   |Kaynak               |Hedef           |
   |---------------------------|---------------------|-----------------|
   |Azure-Azure arası             |Azure bölgesi         |Azure Bölgesi     |
   |VMware Azure            |Yapılandırma sunucusu |Azure            |
   |VMM'den Azure'a               |VMM kolay adı    |Azure            |
   |Azure'da Hyper-v sitesi      |Hyper-v sitesi adı    |Azure            |
   |Fiziksel makineleri azure'a |Yapılandırma sunucusu |Azure            |
   |VMM VMM                 |VMM kolay adı    |VMM kolay adı|

   > [!NOTE]
   > Bir kurtarma planı aynı kaynak ve hedef sanal makineler içerebilir. VMM ile VMware ve sanal makineler, aynı kurtarma planının bir parçası olamaz. VMware sanal makineleri ve fiziksel makineler olabilir ancak her ikisi için kaynak yapılandırma sunucusu olarak aynı plana eklendi.

2. İçinde **sanal makine Seç**, varsayılan grubuna (Grup 1) kurtarma planında eklemek istediğiniz sanal makineleri (veya çoğaltma grubu) seçin. Yalnızca kaynağında (olarak seçilen kurtarma planında) korunan ve hedefi (olarak seçilen kurtarma planında) korumalı sanal makineleri seçimi için izin verilir.

## <a name="how-to-customize-and-extend-recovery-plans"></a>Özelleştirme ve kurtarma planları genişletmek nasıl

Özelleştirebilir ve Site Recovery kurtarma planı kaynak dikey penceresine gitme ve Özelleştir sekmesini tıklatarak kurtarma planları genişletir.

Özelleştirme ve kurtarma planları genişletir:

- **Yeni gruplar eklemek**— ek kurtarma planı grubu (en fazla yedi) varsayılan grubuna ekleyin ve sonra kurtarma planı grupların daha fazla makine ya da çoğaltma grupları ekleyin. Grupları, bunları eklediğiniz sırayla numaralandırılır. Yalnızca bir sanal makine ya da çoğaltma grubu bir kurtarma planı grubuna eklenebilir.
- **İşlemi el ile eklemek**— önce veya sonra bir kurtarma planı Grup çalıştırmak el ile yapılan eylemler ekleyebilirsiniz. Kurtarma planı çalıştığında işlemi el ile eklenen bir noktada durdurur. Bir iletişim kutusu, el ile gerçekleştirilen işlemin tamamlandığını belirtmenizi ister.
- **Bir komut dosyası eklemek**— önce veya sonra bir kurtarma planı Grup çalışan komut dosyaları ekleyebilirsiniz. Bir komut dosyası eklediğinizde, yeni bir grup için Eylemler kümesi ekler. Örneğin, Grup 1 öncesi adımları kümesini adıyla oluşturulur: Grup 1: öncesi adımları. Tüm ön adımları bu kümesi içinde listelenir. Dağıtılan bir VMM sunucunuz varsa, birincil sitede yalnızca bir komut dosyası ekleyebilirsiniz. [Daha fazla bilgi edinin](site-recovery-how-to-add-vmmscript.md).
- **Azure runbook'ları ekleyin**— Azure runbook'ları içeren kurtarma planları genişletebilirsiniz. Örneğin, görevleri otomatikleştirmek için ya da tek adımlı kurtarma oluşturmak için. [Daha fazla bilgi edinin](site-recovery-runbook-automation.md).


## <a name="how-to-add-a-script-runbook-or-manual-action-to-a-plan"></a>Bir planı için bir komut dosyası, runbook veya el ile eylemi ekleme

VM'ler veya çoğaltma gruplarına eklenir ve plan oluşturulan sonra varsayılan kurtarma planı grup için bir komut dosyası veya işlemi el ile ekleyebilirsiniz.

1. Kurtarma planı açın.
2. Bir öğeyi tıklatın **adım** listeleyin ve ardından **betik** veya **el ile eylemi**.
3. Komut dosyası veya eylem önce veya sonra seçili öğe eklemek istediğiniz belirtin. Komut dosyası konumunu yukarı veya aşağı taşımak için kullanmak **Yukarı Taşı** ve **Aşağı Taşı** düğmeler.
4. VMM komut dosyası eklerseniz, seçin **VMM komut dosyası için yük devretme**. İçinde **betik yolu**, paylaşım göreli yolunu yazın. VMM aşağıdaki örnekte, yolunu belirtin: **\RPScripts\RPScript.PS1**.
5. Defterini çalıştır bir Azure Otomasyonu eklerseniz, runbook bulunduğu Azure Otomasyonu hesabı belirtin ve uygun Azure runbook komut dosyasını seçin.
6. Betiği beklendiği gibi çalıştığından emin olmak için bir kurtarma planı yük devretmesi yapın.

Bir yük devretme veya geri dönme yaparken betiği veya runbook seçenekleri yalnızca aşağıdaki senaryolarda kullanılabilir. İşlemi el ile yük devretme ve yeniden çalışma için kullanılabilir.


|Senaryo               |Yük devretme |Yeniden çalışma |
|-----------------------|---------|---------|
|Azure-Azure arası         |Runbook'lar |Runbook  |
|VMware Azure        |Runbook'lar |NA       | 
|VMM'den Azure'a           |Runbook'lar |Betik   |
|Azure'da Hyper-v sitesi  |Runbook'lar |NA       |
|VMM VMM             |Betik   |Betik   |


## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme işlemleri çalıştırma hakkında.

Eylem planlama kurtarma görmek için bu videoyu izleyin.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
