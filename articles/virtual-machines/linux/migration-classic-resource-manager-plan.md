---
title: Iaas Klasik kaynaklardan Azure Resource Manager'a geçişi planlama | Microsoft Docs
description: Iaas Klasik kaynaklardan Azure Resource Manager'a geçişi planlama
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 78492a2c-2694-4023-a7b8-c97d3708dcb7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/01/2017
ms.author: kasing
ms.openlocfilehash: 586a5590c88ef4124543c47389f62eaa864d2d18
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Iaas Klasik kaynaklardan Azure Resource Manager'a geçişi planlama
Azure Resource Manager birçok şaşırtıcı özellik sunarken, sorunsuz şeyler emin olmak için geçiş Yolculuğunuzun planlama önemlidir. Planlama zaman harcama geçiş etkinliklerini yürütülürken sorunlarla değil olduğunu güvence altına alır. 

> [!NOTE] 
> Aşağıdaki kılavuz yoğun müşterilerle geçirme büyük enviornments üzerinde çalışan bulut çözümü mimarları ve Azure Müşteri danışma ekibi tarafından katkısı. Bu nedenle bu tür bu belge başarı yeni desenler ortaya çıkan olarak güncelleştirilmesi devam ederken, geri zamandan yeni herhangi bir önerimiz olup olmadığını görmek için süre denetleyin.

Geçiş gezisine dört genel aşamaları şunlardır:

![Yükseltme aşamaları](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Planlama

### <a name="technical-considerations-and-tradeoffs"></a>Teknik konuları ve bileşim

Teknik gereksinimleri boyutu, coğrafyalara ve işletimsel yöntemler bağlı olarak, düşünmek isteyebilirsiniz:

1. Neden Azure Resource Manager, kuruluşunuz için istendiğini?  Bir geçiş için iş nedenleri nelerdir?
2. Azure Resource Manager için teknik nedeni nedir?  Hangi (varsa) ek Azure Hizmetleri yararlanan istediğinizden emin misiniz?
3. Hangi uygulama (veya sanal makineler kümesi) geçiş işlemine dahil edildi?
4. Hangi senaryoları ile API geçiş desteklenir?  Gözden geçirme [desteklenmeyen özellikler ve yapılandırmalar](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations).
5. İşletimsel ekipleriniz artık hem Klasik hem de Azure Resource Manager uygulamalar/VMs destekleyecek mi?
6. Nasıl (varsa) Azure Resource Manager VM Dağıtım, yönetim, izleme ve raporlama işlemleri değişiyor mu?  Dağıtım betikleri güncelleştirilmesi gerekiyor mu?
7. İletişimleri nedir Paydaşlar (son kullanıcılar, uygulama sahipleri ve altyapı sahipleri) uyarı planlıyor?
8. Ortam karmaşıklığına bağlı olarak, uygulamanın son kullanıcılara ve uygulama sahipleri kullanılamaz olduğu bir bakım süresine var olması gerekiyor mu?  Öyleyse, ne kadar süreyle?
9. Paydaşlar bilgili ve Azure Kaynak Yöneticisi'nde yeterli olduğundan emin olmak için eğitim planı nedir?
10. Program yönetimi veya geçiş için proje yönetim planı nedir?
11. Ne zaman çizelgeleri Azure Resource Manager geçiş ve diğer teknoloji yol haritalarına ilişkilidir?  Bunlar en iyi şekilde hizalanmış mı?

### <a name="patterns-of-success"></a>Başarı desenleri

Başarılı müşteriler burada yukarıdaki soruları ele alınan, belgelenen kapsamındadır ve plan ayrıntılı.  Geçiş planlarını sponsorlar ve Paydaşlar için kapsamlı açıkça emin olun.  Geçiş seçenekleri hakkında bilgi Donatı; Bu geçiş belge aşağıda kümesi aracılığıyla okuma kullanmamanız önerilir.

* [Platform desteklenen geçişi Iaas Klasik kaynaklardan Azure Resource Manager'a genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas kaynaklarına Klasikten Azure Resource Manager geçirmek için PowerShell kullanma](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarına Klasikten Azure Resource Manager geçirmek için CLI kullanın](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas Klasik kaynaklardan Azure Resource Manager için geçiş ile Yardım için topluluk araçları](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Gözden geçirme Iaas kaynaklardan Klasik Azure Kaynak Yöneticisi hakkında en sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

- Plana hatası.  Bu geçiş teknolojisi adımları kanıtlanmış ve sonucu tahmin edilebilir.
- Desteklenen geçiş API platform varsayım tüm senaryoları için hesap. Okuma [desteklenmeyen özellikler ve yapılandırmalar](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) hangi senaryoları desteklenir anlamak için.
- Olası uygulama kesinti son kullanıcılar için planlama değil.  Son kullanıcılar olası kullanılamaz uygulama süre yeterli uyarmak için yeterli arabellek planlayın.


## <a name="lab-test"></a>Lab Test 

**Enviornment çoğaltabilir ve bir test geçişi yapın**
  > [!NOTE]
  > Resmi Microsoft Support tarafından desteklenmeyen bir topluluk katkıda bulunan aracı kullanarak mevcut ortamınızın tam çoğaltma yürütülür. Bu nedenle, bir **isteğe bağlı** adım ancak üretim ortamınızı dokunmadan sorunları bulmak için en iyi yolu değil. Topluluğa katkıda bulunan bir araç kullanarak bir seçenek değilse, aşağıdaki hazırlama/doğrula/durdurma kuru Çalıştır öneri hakkında okuyun.
  >
  
  Laboratuvar test (işlem, ağ ve depolama), tam senaryo, yönetme, sorunsuz bir geçiş sağlamak için en iyi yoludur. Bu, olmanıza yardımcı olur:

  - Tamamen ayrı bir laboratuvar veya test etmek için var olan bir üretim dışı ortamı. Sürekli olarak geçirilebilir ve kalıcı olmayacak şekilde değiştirilebilir tamamen ayrı bir laboratuvar öneririz.  Gerçek abonelikleri meta verilerini toplama/hydrate için komut dosyaları, aşağıda listelenmiştir.
  - Laboratuvar ayrı bir abonelik oluşturmak için iyi bir fikirdir. Laboratuvar art arda bozulur ve ayrı bir sahip, yalıtılmış abonelik olasılığını azaltır nedeni gerçek bir şey yanlışlıkla alırsınız emin silindi.

  Bu AsmMetadataParser aracı kullanılarak gerçekleştirilebilir. [Burada bu araç hakkında daha fazla bilgi](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Başarı desenleri

Birçok büyük geçişler bulunan sorunları oluştu. Bu kapsamlı bir liste değildir ve için başvurmalıdır [desteklenmeyen özellikler ve yapılandırmalar](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) daha fazla ayrıntı için. Bu teknik sorunlar yaşayabilirsiniz değil veya olabilir ancak bunu yaparsanız, geçiş işlemini denemeden önce bunları çözme daha sorunsuz bir deneyim garanti eder.

- **Prepare/doğrula/durdurma kuru Çalıştır yapmak** -bu belki de klasik Azure Resource Manager geçiş başarılı durumuna emin olmak için en önemli bir adımdır. API geçiş üç ana adım vardır: doğrulamak için hazırlama ve tamamlama. Doğrulamak Klasik ortamınızın durumunu okuma ve tüm sorunların bir sonuç döndürür. Ancak, bazı sorunlar Azure Kaynak Yöneticisi yığınında olabileceğinden doğrula her şeyi yakalamaz. Geçiş sürecinde sonraki adım, bu sorunları kullanıma hazırlama yardımcı olur. Hazırlama meta verileri Klasikten Azure Resource Manager için taşıma ancak değil taşıma tamamlama ve kaldırmaz veya Klasik tarafında değişikliği. Kuru Çalıştır geçişi hazırlama sonra durduruluyor içerir (**değil yürüten**) geçiş hazırlayın. Doğrula/hazırlama/durdurma kuru Çalıştır hedefidir tüm Azure Kaynak Yöneticisi yığınında meta verileri görmek için onu inceleyin (*program aracılığıyla veya Portal*), her şeyin doğru şekilde geçirir olduğunu doğrulayın ve iş teknik sorunları.  Kapalı kalma süresi için uygun planı yapabilmesi için Ayrıca, geçiş süresi bir fikir verir.  Bir doğrula/hazırlama/durdurma kullanıcı kapalı kalma süresi neden olmaz; Bu nedenle, uygulama kullanımı için benzer.
  - Aşağıdaki öğeler kuru çalıştırmadan önce çözülmesi gerekir, ancak bunlar eksik, test kuru çalıştırma hazırlık adımları da güvenli bir şekilde temizler. Kurumsal geçiş sırasında biz geçiş hazırlığı emin olmak için güvenli ve çok değerli bir kaynak yolu için kuru Çalıştır buldunuz.
  - Ne zaman hazırlama çalışıyor, Denetim düzlemi (Azure yönetim işlemlerini) herhangi bir değişiklik VM meta verileri doğrula/hazırlama/durdurma sırasında böylece tüm sanal ağ için kilitlenir.  Ancak Aksi durumda herhangi bir uygulama işlevini (RD, VM kullanımı, vb.) etkilenmeyecek.  Sanal makineleri kullanıcılarının kuru Çalıştır yürütülmekte olan bilmez.

- **Rota devreler ve VPN express**. Şu anda Express rota ağ geçitleri yetkilendirme bağlantılarla kapalı kalma süresi olmadan geçirilemez. Geçici çözüm için bkz: [geçirmek ExpressRoute bağlantı hattına ve Resource Manager dağıtım modeli için Klasik sanal ağlar ilişkili](../../expressroute/expressroute-migration-classic-resource-manager.md).

- **VM uzantıları** -sanal makine uzantıları olası en büyük roadblocks geçirme çalışan sanal makineleri için biri. Düzeltme VM uzantıları, çalınıyor 1-2 gün sürebilir, bu nedenle uygun şekilde planlamanız.  Çalışan bir Azure aracısını geri VM'ler çalıştıran VM uzantısı durumu raporlamak için gereklidir. Durumu kötü olarak çalışan bir VM için geliyorsa, bu geçiş durdurulur. Aracının kendisi geçiş etkinleştirmek için çalışma sırada olması gerekmez, ancak uzantıları VM varsa, daha sonra hem bir çalışma aracısı ve giden internet bağlantısı (DNS ile) ilerlemek için geçiş için gereklidir.
  - Bir DNS sunucusuna bağlantı geçiş sırasında Bgınfo v1 dışındaki tüm VM uzantıları kaybolursa. \* ilk her geçiş hazırlamadan önce VM ve daha sonra yeniden eklenen arka VM'ye Azure Resource Manager geçişten sonra kaldırılmaları gerekir.  **Çalıştırmakta olan VM'ler için budur.**  VM'ler deallocated durdurulmuşsa VM uzantıları kaldırılması gerekmez. **Not:** izleme edecek Azure tanılama ve Güvenlik Merkezi gibi birçok uzantıları kendilerini yeniden geçişten sonra bu nedenle bunları kaldırma bir sorun değildir.
  - Ayrıca, ağ güvenlik grupları olmadığından emin olun giden internet erişimi kısıtlama. Bu, bazı ağ güvenlik grupları yapılandırmalarla meydana gelebilir. Giden internet erişimi (ve DNS), Azure Resource Manager geçirilecek VM uzantıları için gereklidir. 
  - Bgınfo uzantısını iki sürümü vardır: v1 ve v2.  VM Azure portal veya PowerShell kullanarak oluşturulmuşsa VM olasılıkla üzerinde v1 uzantısı vardır. Bu uzantı kaldırılması gerekmez ve (geçiş) atlanacak geçiş API tarafından. Klasik VM yeni Azure portal ile oluşturulmuşsa, ancak, büyük olasılıkla JSON tabanlı olması için Azure Resource Manager geçirilebilecek Bgınfo v2 sürümünü sağlanan aracı çalıştığından ve giden internet erişimi (ve DNS) vardır. 
  - **Düzeltme seçeneği 1**. Vm'leriniz erişim, çalışma DNS hizmeti ve Azure aracılarını sanal makinelerin çalışıyoruz. tüm VM uzantıları hazırlama önce geçişin parçası olarak kaldırın giden internet olmaz biliyorsanız, VM uzantıları geçişten sonra yeniden yükleyin. 
  - **Düzeltme seçeneği 2**. VM uzantıları hurdle çok büyük olduğunda, başka bir seçenek serbest/bırakma geçişten önce tüm VM'ler olur. Deallocated Vm'leri geçirme sonra bunları Azure Resource Manager tarafında yeniden başlatın. Burada VM uzantıları geçiş yapacağınız avantajdır. Tüm genel sanal IP'ye kullanıma yönelik kaybolacak dezavantajı olduğundan (Bu Başlatıcı olmayan olabilir) ve çok daha büyük etkisi üzerinde çalışan uygulamalar neden aşağı VM'ler açıkça kapanır.

    > [!NOTE] 
    > Azure Güvenlik Merkezi ilke Geçirilmekte olan çalışan sanal makinelerini karşı yapılandırılmışsa, Güvenlik İlkesi uzantıları kaldırmadan önce durdurulması gerekir, aksi takdirde izleme uzantısı güvenlik otomatik olarak VM kaldırdıktan sonra yeniden yüklenecek.

- **Kullanılabilirlik kümeleri** - için Azure Resource Manager, içerilen VM'ler tüm olmalıdır bir kullanılabilirlik kümesinde Klasik dağıtım (yani, bulut hizmeti) ya da sanal makineleri için geçirilecek sanal ağ (vNet) tüm herhangi bir kullanılabilirlik kümesi olmamalıdır. Birden fazla kullanılabilirlik kümesi bulut hizmetinde sahip Azure Resource Manager ile uyumlu değildir ve geçiş durdurulur.  Ayrıca, olamaz bazı sanal makineleri bir kullanılabilirlik kümesinde ve bazı sanal makineleri bir kullanılabilirlik kümesinde değil. Bu sorunu çözmek için düzeltmek veya Bulut hizmetiniz sırasını yeniden ayarlaması gerekir.  Plan uygun şekilde gibi bu zaman alıcı olabilir. 

- **Web/çalışan rolü dağıtımları** -web ve çalışan rolleri içeren Cloud Services, Azure Resource Manager geçirilemiyor. Geçiş başlamadan önce web/çalışan rolleri sanal ağdan kaldırılmalıdır.  Tipik bir çözüm ayrıca bir expressroute bağlantı hattı bağlantılı ayrı Klasik sanal ağda yalnızca web/çalışan rolü örnekleri taşımayı veya kod (Bu tartışma bu belgenin kapsamında değildir) yeni PaaS uygulama hizmetleri geçirmek için değil. Eski durum dağıtmanız, yeni bir Klasik sanal ağ oluşturma, taşıma/web/çalışan rolleri için yeni bir sanal ağ dağıtın ve ardından taşınan sanal ağdan dağıtımları silin. Kod değişiklikleri gerekli. Yeni [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md) özelliği, web/çalışan rolleri ve diğer sanal ağlar aynı Azure bölgesinde Geçirilmekte olan sanal ağ gibi içeren klasik sanal ağda birlikte eş için kullanılabilir (**eşlenmiş sanal ağlar geçirilemez gibi sanal ağ geçişi tamamlandıktan sonra**), bu nedenle aynı yetenekleri performans kaybı ve gecikme/bant genişliği cezaları sağlama. Eklenmesi verilen [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md), web/çalışan rolü dağıtımları şimdi kolayca azaltılabilir ve Azure Resource Manager için geçiş engelleme değil.

- **Azure Kaynak Yöneticisi kotaları** -Azure bölgeleri hem Klasik hem de Azure Resource Manager için ayrı kota sınırları vardır. Bir geçiş senaryosunda yeni donanım tüketilen değil olsa bile *(biz VM'ler Klasikten Azure Resource Manager takas)*, Azure Kaynak Yöneticisi kotaları hala gereksinim yeterli kapasiteye sahip yerinde geçiş başlamadan önce olmalıdır. Aşağıda, gördük ana sınırları sorunlara neden yer alır.  Sınırları artırmak için kota destek bileti açın. 

    > [!NOTE]
    > Bu sınırların geçirilmesi için geçerli enviornment ile aynı bölgede oluşturulması gerekir.
    >

    - Ağ Arabirimleri
    - Yük Dengeleyiciler
    - Ortak IP'ler
    - Statik genel IP'ler
    - Çekirdek
    - Ağ Güvenlik Grupları
    - Yönlendirme Tabloları

    En son Azure CLI 2.0 sürümü ile aşağıdaki komutları kullanarak, geçerli Azure Kaynak Yöneticisi kotaları kontrol edebilirsiniz.

    **İşlem** *(çekirdek, Avaiability kümeleri)*

    ```bash
    az vm list-usage -l <azure-region> -o jsonc 
    ```

    **Ağ** *(sanal ağlar, statik genel IP'ler, genel IP'ler, ağ güvenlik grupları, ağ arabirimleri, yük Dengeleyiciler, yol tablolarını)*
    
    ```bash
    az network list-usages -l <azure-region> -o jsonc
    ```

    **Depolama** *(depolama hesabı)*
    
    ```bash
    az storage account show-usage
    ```

- **Azure Resource Manager azaltma sınırları API** - (ör. yeterli büyüklükte bir ortamınız varsa > Azaltma sınırları yazmalar için varsayılan API isabet 400 bir VNET içindeki Vm'leri), (şu anda **1200 yazma/saat**) Azure Kaynak Yöneticisi'nde. Geçiş işlemini başlatmadan önce bu abonelik sınırınızı artırmak üzere bir destek bileti yükseltmeniz.

- **VM durumu zaman aşımına sağlama** - VM durumunu **sağlama zaman aşımına uğradı**, bu geçiş öncesi çözümlenmiş olması gerekir. Bunu yapmak için tek kapalı kalma süresi ile sağlamayı / (silme, disk tutun ve VM yeniden) VM sağlama işleminin tarafından yoludur. 

- **RoleStateUnknown VM durumu** - nedeniyle geçiş işlemindeki bir **rol durumu bilinmeyen** hata iletisi, portal kullanarak VM inceleyebilir ve çalıştığından emin olun. Bu hata genellikle kaybolur, birkaç dakika sonra (düzeltme gerekli) sahibi ve geçici bir türü sırasında sanal makine genellikle sık görülen **Başlat**, **durdurmak**, **yeniden** işlemleri. **Önerilen yöntem:** birkaç dakika sonra tekrar geçiş işlemini yeniden deneyin. 

- **Fabric kümesi yok** - bazı durumlarda, bazı sanal makineleri tek çeşitli nedenlerle geçirilemez. Bu bilinen durumların birini VM ise son (geçen hafta ya da bunu içinde) oluşturulur ve Azure Resource Manager iş yükleri için henüz donatılmış olmayan bir Azure küme güden oldu.  Bildiren bir hata iletisiyle karşılaşırsınız **fabric kümesi yok** ve VM geçirilemez. Küme yakında Azure Kaynak Yöneticisi etkin alırsınız gibi birkaç gün bekleyen genellikle belirli bu sorunu giderir. Ancak, için bir hemen geçici bir çözüm değildir `stop-deallocate` VM ardından İleri geçirme işlemine devam etmek ve taşıdıktan sonra Azure Kaynak Yöneticisi'nde VM'yi başlatın yedekleyin.

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

- Kısayollar ele değil ve Doğrula/hazırlama/durdurma kuru Çalıştır geçişler atlayabilirsiniz.
- En değilse, olası sorunları, doğrula/hazırlama/durdurma adımları sırasında belirir.

## <a name="migration"></a>Geçiş

### <a name="technical-considerations-and-tradeoffs"></a>Teknik konuları ve bileşim

Ortamınız ile bilinen sorunlar çalışılan çünkü artık hazırsınız.

Gerçek geçişler için düşünmek isteyebilirsiniz:

1. Planlama ve öncelik ile sanal ağ (geçiş en küçük birim) zamanlayabilirsiniz.  Daha karmaşık sanal ağlarla ilerleme ve basit sanal ağlar ilk yapabilirsiniz.
2. Müşterilerin çoğu, üretim dışı ve üretim ortamlarını sahip olur.  Üretim son zamanlayın.
3. **(İSTEĞE BAĞLI)**  Beklenmeyen sorunlar çıkması durumunda bir bakım kapalı kalma arabellek bolca zamanlayın.
4. İle iletişim kurmasını ve sorunlar çıkması durumunda destek ekipleriniz hizalayın.

### <a name="patterns-of-success"></a>Başarı desenleri

Yukarıdaki Laboratuvar Test bölümünden teknik kılavuz olarak kabul ve gerçek geçiş öncesinde azaltıldığından.  Uygun testleri ile olay dışı gerçekte bir geçiş değil.  Üretim ortamları için güvenilen bir Microsoft iş ortağı veya Microsoft Premier Hizmetleri gibi ek destek sağlamak faydalı olabilir.

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

Tam olarak test sorunlara neden ve geçişte gecikme olabilir.  

## <a name="beyond-migration"></a>Geçiş dışında

### <a name="technical-considerations-and-tradeoffs"></a>Teknik konuları ve bileşim

Azure Kaynak Yöneticisi'nde olduğunuz, platform ekranı kaplamasını sağlayın.  Okuma [genel bakış Azure Kaynak Yöneticisi'nin](../../azure-resource-manager/resource-group-overview.md) ek yararları hakkında bilgi almak için.

Göz önünde bulundurmanız gerekenler:

- Diğer etkinlikleri ile geçiş paketleme.  Müşterilerin çoğu, bir uygulama bakım penceresi için kabul et.  Bu durumda, şifreleme ve yönetilen disklere geçişi gibi diğer Azure Resource Manager işlevlerini etkinleştirmek için bu kesinti kullanmak isteyebilirsiniz.
- Teknik ve iş nedenleri Azure kaynak yöneticisi için yeniden ziyaret; ortamınız için geçerli olan ek hizmetleri yalnızca Azure Kaynak Yöneticisi kullanılabilir etkinleştirin.
- Ortamınızı PaaS hizmetleriyle modernize.

### <a name="patterns-of-success"></a>Başarı desenleri

Artık Azure Kaynak Yöneticisi'nde etkinleştirmek istediğiniz hangi hizmetlerin üzerinde amaca olabilir.  Birçok müşteri bulmak için Azure ortamlarını çekici altında:

- [Rol tabanlı erişim denetimi](../../azure-resource-manager/resource-group-overview.md#access-control).
- [Daha kolay ve daha denetimli dağıtımı için Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md#template-deployment).
- [Etiketleri](../../azure-resource-manager/resource-group-using-tags.md).
- [Etkinlik denetimi](../../azure-resource-manager/resource-group-audit.md)
- [Azure ilkeleri](../../azure-policy/azure-policy-introduction.md)

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

Neden bu Klasik Azure Resource Manager geçiş gezisine olarak başlatılan unutmayın.  Ne özgün iş nedenleri muydunuz? İş neden elde?


## <a name="next-steps"></a>Sonraki adımlar

* [Platform desteklenen geçişi Iaas Klasik kaynaklardan Azure Resource Manager'a genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas kaynaklarına Klasikten Azure Resource Manager geçirmek için PowerShell kullanma](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas Klasik kaynaklardan Azure Resource Manager için geçiş ile Yardım için topluluk araçları](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Gözden geçirme Iaas kaynaklardan Klasik Azure Kaynak Yöneticisi hakkında en sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
