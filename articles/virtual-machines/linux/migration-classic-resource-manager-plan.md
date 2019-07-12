---
title: Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçişini planlama | Microsoft Docs
description: Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçişini planlama
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: gwallace
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
ms.openlocfilehash: 713e814478f88def2559dad79aafeb6fa2737d7f
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667349"
---
# <a name="planning-for-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçişini planlama
Azure Resource Manager çok sayıda harika özellik sunarken, geçiş yolculuğunuza emin sorunsuz şeyler yapmak için planlamak için önemlidir. Harcadığınız zamanı planlama, sorunları geçiş etkinliklerini yürütülürken karşılaşmayacağınızdan emin olursunuz. 

> [!NOTE] 
> Aşağıdaki yönergeler yoğun bir şekilde geçirme büyük ortamlarda müşterilerle birlikte çalışarak bulut çözüm mimarları ve Azure Müşteri danışma ekibi tarafından katkıda. Bu nedenle bu tür bir belge başarı yeni desenler çıktıkça güncelleştirilmesi devam ederken, geri zamandan yeni önerisi olup olmadığını görmek için için zamanını kontrol edin.

Geçiş yolculuğu dört genel aşamaları şunlardır:

![Geçiş aşamaları](../media/virtual-machines-windows-migration-classic-resource-manager/plan-labtest-migrate-beyond.png)

## <a name="plan"></a>Planlama

### <a name="technical-considerations-and-tradeoffs"></a>Teknik konular ve avantajsız

Teknik gereksinimler boyutu, coğrafyaları ve işletimsel uygulamalar bağlı olarak, düşünmek isteyebilirsiniz:

1. Neden Azure Resource Manager, kuruluşunuz için istendiğini?  Geçiş için iş nedenleri nelerdir?
2. Azure Resource Manager için teknik nedenleri nelerdir?  Hangi (varsa) diğer Azure hizmetlerini ister yararlanarak?
3. Geçiş işleminde, hangi uygulama (veya sanal makine kümeleri) dahildir?
4. Hangi senaryoları ile API geçiş destekleniyor mu?  Gözden geçirme [desteklenmeyen özellikleri ve yapılandırmalar](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations).
5. Operasyon Takımlarınız, artık hem Klasik hem de Azure Resource Manager uygulamalar/VM'ler desteklenecek?
6. Nasıl (varsa) Azure Resource Manager VM dağıtımı, yönetim, izleme ve raporlama işlemlerinin değişiyor mu?  Dağıtım betiklerinizi güncelleştirilmesi gerekiyor mu?
7. İletişim nedir (son kullanıcılar, uygulama sahipleri ve altyapı sahipleri) katılımcıları planlıyor?
8. Ortamı karmaşıklığına bağlı olarak, uygulamanın son kullanıcılara ve uygulama sahipleri kullanılamaz olduğu bir bakım süresine var olmalıdır?  Öyleyse ne kadar süreyle kullanmıştı?
9. Proje katılımcıları bilgili ve Azure Resource Manager'daki yeterli olduğundan emin olmak için eğitim planı nedir?
10. Geçiş proje yönetimi planlaması ve program yönetimi nedir?
11. Azure Resource Manager'a geçiş ve diğer zaman çizelgeleri nelerdir teknoloji yol haritalarını ilgili?  Bunlar en uygun şekilde hizalanmış mı?

### <a name="patterns-of-success"></a>Başarı desenleri

Başarılı müşteriler burada yukarıdaki soruları ele alınan, belgelenen kapsamındadır ve planları ayrıntılı.  Geçiş planları problem sponsor ve proje katılımcılarına iletildiği emin olun.  Geçiş seçenekleri hakkında bilgi Donatı; Bu geçiş belge kümesi altına aracılığıyla okuma kesinlikle önerilir.

* [Iaas kaynaklarının Klasik modelden Azure Resource Manager'a platform destekli geçişe genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas kaynaklarının Klasik'ten Azure Resource Manager'a geçiş için PowerShell kullanma](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarının Klasik'ten Azure Resource Manager'a geçiş için CLI kullanma](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçişini ile Yardım için topluluk araçları](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Gözden geçirme Iaas kaynaklarını Klasik modelden Azure Resource Manager'a hakkında sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

- Plan hatası.  Bu geçiş teknolojisi adımları kanıtlanmış ve sonucu tahmin edilebilir.
- Platform geçişi API'sini desteklediği varsayımına tüm senaryolar için hesaba katacaktır. Okuma [desteklenmeyen özellikleri ve yapılandırmalar](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) anlamak hangi senaryolar desteklenir.
- Olası uygulama kesintisi son kullanıcılar için planlama değil.  Yeterince büyük olasılıkla kullanılabilir uygulama süresi son kullanıcıları uyarmak için yeterli arabellek planlayın.


## <a name="lab-test"></a>Laboratuvar testi 

**Ortamınızın çoğaltılması ve bir test geçiş yapın**
  > [!NOTE]
  > Topluluk tarafından katkıda bulunulan resmi olarak Microsoft Support desteklenmeyen bir araç kullanarak mevcut ortamınıza tam çoğaltma yürütülür. Bu nedenle, bir **isteğe bağlı** adım ancak, üretim ortamlarınızı dokunmadan sorunları bulmak için en iyi yoludur. Ardından bir topluluk tarafından katkıda bulunulan aracını kullanarak bir seçenek değilse, aşağıdaki doğrulama/hazırlama/iptal prova öneri hakkında okuyun.
  >
  
  Gerçek senaryonuza (işlem, ağ ve depolama) bir laboratuvar testi yürütmek, yumuşak bir geçiş sağlamak için en iyi bir yoludur. Bu, olmanıza yardımcı olur:

- Tamamen ayrı bir laboratuvar veya test etmek için var olan bir üretim dışı ortamda. Sürekli olarak geçirilebilir ve kalıcı olmayacak şekilde değiştirilebilir tamamen ayrı bir laboratuvar öneririz.  Betiklerin gerçek aboneliklerinden gelen meta veri toplama/hydrate aşağıda listelenmiştir.
- Laboratuvar içinde ayrı bir abonelik oluşturmak için iyi bir fikirdir. Laboratuvar art arda bozulur ve ayrı bir sahip, yalıtılmış abonelik bir şey gerçek yanlışlıkla silinecek, olasılığını azaltır nedenidir.

  Bu işlem AsmMetadataParser aracı kullanılarak gerçekleştirilebilir. [Buradaki aracı hakkında daha fazla bilgi](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

### <a name="patterns-of-success"></a>Başarı desenleri

Birçok büyük geçişlerin bulunan sorunları yoktu. Bu kapsamlı bir liste değildir ve başvurmanız gerekir [desteklenmeyen özellikleri ve yapılandırmalar](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#unsupported-features-and-configurations) daha fazla ayrıntı için. Olabilir veya bu teknik sorunlarla karşılaşabilirsiniz değildir ancak bunu yaparsanız geçişi denemeden önce bunları çözme daha sorunsuz bir deneyim sağlayacaktır.

- **Doğrulama/hazırlama/iptal prova yapmak** -bu belki de klasik Azure Resource Manager için geçiş başarı sağlamak için en önemli adımdır. Geçiş API'si üç ana adım vardır: Doğrulama, hazırlama ve tamamlama. Doğrulama Klasik ortamınızın durumunu okuma ve tüm sorunların bir sonuç döndürür. Ancak, bazı sorunlar, Azure Resource Manager yığınında olabileceğinden, doğrulama her şeyi yakalayamaz. Geçiş işlemi sonraki adımda, bu sorunların kullanıma hazırlama yardımcı olur. Hazırlama meta verilerin Klasikten Azure Resource Manager'a taşıma ancak taşıma işlemi, yürütme ve değil kaldırın veya Klasik tarafında herhangi bir ayarı değiştirmek. Geçiş için hazırlama ve ardından iptal ediliyor prova içerir (**değil yürüten**) geçiş hazırlama. Doğrulama/hazırlama/iptal prova hedefidir tüm meta veriler Azure Resource Manager yığınında görmek için onu inceleyin (*programlama yoluyla veya Portal*), her şeyin doğru şekilde geçirir olduğunu doğrulayın ve teknik ile çalışma sorun.  Kapalı kalma süresi için uygun şekilde planlamak için aynı zamanda size bir fikir geçiş süresi sunar.  Bir doğrulama/hazırlama/iptal kullanıcı kapalı kalma süresi neden olmaz; Bu nedenle, uygulama kullanımını kesintiye neden olmayan gereklidir.
  - Aşağıdaki öğeler prova önce çözülmesi gerekir, ancak bunlar kaçırdıysanız prova test hazırlık adımları da güvenli bir şekilde temizler. Kurumsal geçiş sırasında biz geçiş hazırlığı emin olmak için güvenli ve her bir yolu olarak prova buldunuz.
  - Ne zaman hazırlama çalışıyor, Denetim düzlemi (Azure yönetim işlemleri) kilitlenmiş olabilir tüm sanal ağ için bu nedenle herhangi bir değişiklik için VM meta veri doğrulama/hazırlama/durdurma sırasında sağlanabilir.  Ancak Aksi takdirde herhangi bir uygulama işlevi (RD, VM kullanımı, vb.) etkilenmez.  Kullanıcılar sanal makinelerin prova yürütülmekte olan bilmez.

- **Express route bağlantı hatları ve VPN**. Şu anda kapalı kalma süresi olmadan Express Route ağ geçidi yetkilendirme bağlantıları olan geçirilemez. Geçici çözüm için bkz. [geçirme ExpressRoute devrelerini ve ilişkili sanal ağları Klasikten Resource Manager dağıtım modeline](../../expressroute/expressroute-migration-classic-resource-manager.md).

- **VM uzantıları** -sanal makine uzantıları, büyük olasılıkla bir büyük bariyerler geçirme çalışan VM'ler. Düzeltme VM uzantılarının, 1-2 gün çalınıyor alın, böylece buna göre planlayın.  Çalışan bir Azure Aracısı geri çalışan VM uzantı durumu raporlamak için gereklidir. Durum hatalı olarak çalışan bir VM için geliyorsa, bu geçiş durdurulur. Aracı geçişi etkinleştirmek için çalışma sırada olması gerekmez, ancak VM uzantıları varsa, sonra hem bir çalışan aracısı ve giden internet bağlantısı (DNS ile) ileri taşımak geçiş için gereklidir.
  - Bgınfo v1 dışındaki tüm VM uzantıları geçiş sırasında bir DNS sunucusu bağlantısı kaybolsa bile. \* önce her geçiş hazırlamadan önce VM ve daha sonra yeniden ilave VM'ye geçişten sonra Azure Resource Manager kaldırılması gerekir.  **Çalıştıran VM'ler için budur.**  VM serbest bırakıldığında durdurulursa, kaldırılacak VM uzantıları gerekmez. **Not:** İzleme edecek Azure tanılama ve Güvenlik Merkezi gibi birçok uzantı kendilerini yeniden geçişten sonra bu nedenle bunları kaldırma bir sorun değildir.
  - Ayrıca, ağ güvenlik grupları olmadığından emin olun giden internet erişimi sınırlandırma. Bu, bazı ağ güvenlik gruplarının yapılandırmasıyla oluşabilir. Giden internet erişimi (ve DNS), VM uzantıları, Azure Resource Manager'a geçirilmesi için gereklidir. 
  - Bgınfo uzantısını iki sürümü vardır: v1 ve v2.  Azure portal veya PowerShell kullanarak VM oluşturulmuş olsa bile, VM olasılıkla üzerinde v1 uzantısı vardır. Bu uzantı kaldırılacak gerekmez ve (geçiş) atlanacak geçiş API'si tarafından. Klasik VM'yi yeni Azure portalı ile oluşturulmuş olsa bile, ancak büyük olasılıkla JSON tabanlı olması için Azure Resource Manager geçirilebilecek Bgınfo v2 sürümünü sağlanan aracının çalıştığını ve giden internet erişimi (ve DNS). 
  - **Düzeltme seçeneği 1**. Sanal makinelerinizin erişim, bir çalışma DNS hizmeti ve Azure aracıları Vm'lerde çalışan, tüm VM uzantıları hazırlama önce geçişin bir parçası olarak kaldırın giden internet olmaz biliyorsanız, VM uzantılarını geçişten sonra yeniden yükleyin. 
  - **Düzeltme 2. seçenek**. VM uzantıları bir hurdle çok büyük olduğunda, başka bir seçenek kapatma/serbest geçişten önce tüm sanal makineler olur. Serbest bırakıldığında sanal makineleri geçirme ve ardından Azure Kaynak Yöneticisi taraftaki yeniden başlatın. Burada VM uzantıları geçiş yapacağınız avantajdır. Tüm genel sanal IP'ye yönelik kaybolacak dezavantajı olduğundan (Bu, başlangıç olmayan olabilir), ve Vm'leri üzerinde çalışan uygulamalar bir çok büyük bir etkiye neden aşağı kuşkusuz kapatacak.

    > [!NOTE] 
    > Geçirilmekte olan karşı çalışan sanal makinelerini Azure Güvenlik Merkezi İlkesi yapılandırdıysanız, Güvenlik İlkesi uzantıları kaldırmadan önce durdurulması gerekir, aksi durumda güvenlik izleme uzantısı otomatik olarak VM kaldırdıktan sonra yüklenir.

- **Kullanılabilirlik kümeleri** - Azure Resource Manager, Klasik dağıtım (yani, bulut hizmeti) içindeki Vm'leri tüm olmalıdır bir kullanılabilirlik kümesinde veya sanal makinelerin geçirilmesi için bir sanal ağ (vNet) tüm herhangi bir kullanılabilirlik kümesinde olmamalıdır. Bulut hizmetinde birden fazla kullanılabilirlik sahip Azure Resource Manager ile uyumlu değil ve geçiş durdurulur.  Ayrıca, olamaz bazı Vm'leri bir kullanılabilirlik kümesinde ve bir kullanılabilirlik kümesindeki bazı VM'ler. Bu sorunu çözmek için düzeltme veya Bulut hizmetinizi sırasını yeniden ayarlaması gerekir.  Plan buna uygun olarak bu, zaman alıcı olabilir. 

- **Web/çalışan rolü dağıtımları** -web ve çalışan rollerini içeren bulut Hizmetleri, Azure Resource Manager'a geçirme olamaz. Geçişi başlatmadan önce web/çalışan rollerini ilk sanal ağdan kaldırılmalıdır.  Tipik bir çözüm, ayrıca bir ExpressRoute bağlantı hattına bağlı olan ayrı bir Klasik sanal ağ web/çalışan rolü örnekleri yalnızca taşıyın ya da (Bu belgenin kapsamı dışındadır bu tartışma olduğu) daha yeni PaaS uygulama hizmetleri için geçiş kodu ' dir. Eski durum yeniden dağıtın, yeni bir Klasik sanal ağ oluşturma, taşıma/web/çalışan rollerini yeni bir sanal ağ için yeniden dağıtın ve ardından taşınan sanal ağdan dağıtımları silin. Gerekli bir kod değişikliği olmadan. Yeni [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md) birlikte web/çalışan rollerini içeren klasik sanal ağ ile eşleyebilme özelliği kullanılabilir ve diğer sanal ağlara olan sanal ağ gibi aynı Azure bölgesinde (geçişi**olarak eşlenmiş sanal ağlarda geçirilemiyor sanal ağ geçişi tamamlandıktan sonra**), bu nedenle performans kaybı olmadan ve herhangi bir gecikme süresi/bant genişliği yaptırımlara ile aynı özellikleri sağlama. Ayrıca, verilen [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md), web/çalışan rolü dağıtımları artık bir kolayca azaltılabilir ve Azure Resource Manager'a geçiş engellemediğinizden.

- **Azure Resource Manager kotaları** -Azure bölgeleri hem Klasik hem de Azure Resource Manager için ayrı kota sınırları vardır. Bir geçiş senaryosunda, yeni donanım tüketilen değil olsa bile *(biz varolan Vm'leri Klasik modelden Azure Resource Manager'a takas)* , Azure Resource Manager kotalar hala yerinde önce yeterli kapasiteye sahip olması gerekir geçiş başlatabilirsiniz. Aşağıda listelenen gördük ana sınırları sorunlara neden olan.  Sınırları artırmak için bir kota destek bileti açın. 

    > [!NOTE]
    > Bu sınırların geçirilmesi Geçerli ortamınız ile aynı bölgede oluşturulması gerekir.
    >

  - Ağ Arabirimleri
  - Yük Dengeleyiciler
  - Genel IP'ler
  - Statik genel IP'ler
  - Çekirdek
  - Ağ Güvenlik Grupları
  - Yönlendirme Tabloları

    Azure CLI'ın en son sürüm ile aşağıdaki komutları kullanarak geçerli Azure Resource Manager kotanızı kontrol edebilirsiniz.

    **İşlem** *(çekirdek, kullanılabilirlik kümeleri)*

    ```bash
    az vm list-usage -l <azure-region> -o jsonc 
    ```

    **Ağ** *(sanal ağlar, statik genel IP'ler, genel IP'ler, ağ güvenlik grupları, ağ arabirimleri, yük Dengeleyiciler, rota tabloları)*
    
    ```bash
    az network list-usages -l <azure-region> -o jsonc
    ```

    **Depolama** *(depolama hesabı)*
    
    ```bash
    az storage account show-usage
    ```

- **Azure Resource Manager API'si azaltma sınırları** - (örn. büyük bir ortamı varsa > Sınırları yazma işlemleri için varsayılan API isabet 400 bir vnet'teki VM'ler), (şu anda **1200 yazma/saat**) Azure Resource Manager'daki. Geçişi başlatmadan önce bu abonelik sınırınızı artırmak amacıyla bir destek bileti tetiklemelidir.

- **Sağlama zaman aşımına uğradı VM durumu** - herhangi bir VM durumunu **sağlama zaman aşımına uğradı**, bu geçiş öncesi çözülmüş olması gerekir. Bunu yapmak için tek kapalı kalma süresi ile sağlamayı kaldırma/VM (, disk tutun ve VM yeniden Sil) çıkış tarafından yoludur. 

- **RoleStateUnknown VM durumu** - geçiş nedeniyle durduran bir **bilinmeyen rol durumu** hata iletisi, portalı kullanarak VM inceleyin ve çalıştığından emin olun. Bu hata genellikle kaybolur, birkaç dakika sonra (düzeltme gerekli) aittir ve genellikle geçici bir türü sırasında bir sanal makine sıklıkla görülür **Başlat**, **Durdur**, **yenidenbaşlatın** işlemleri. **Önerilen yöntem:** yeniden geçiş birkaç dakika sonra yeniden deneyin. 

- **Fabric küme yok** - bazı durumlarda, belirli Vm'leri tek çeşitli nedenlerden dolayı geçirilemez. Bu bilinen durumlarından biri VM ise kısa süre önce (geçen hafta veya bunu içinde) oluşturulur ve henüz Azure Resource Manager iş yükleri için donatılmış değil bir Azure kümesine yerleşmesi oldu.  Bildiren bir hata alırsınız **fabric küme yok** ve bu VM geçirilemez. Birkaç gün bekleyen küme yakında Azure Resource Manager özellikli alacağınız genellikle belirli bu sorunu çözer. Anında çözüm kullanmaktır ancak `stop-deallocate` VM ardından İleri geçirme işlemine devam etmek ve geçişten sonra Azure Resource Manager'da VM Başlat yedekleyin.

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

- Kısayolları olması değil ve doğrulama/hazırlama/iptal prova geçişlerin atlayabilirsiniz.
- En çok, aksi takdirde, olası sorunları doğrulama/hazırlama/iptal adımları sırasında belirir.

## <a name="migration"></a>Geçiş

### <a name="technical-considerations-and-tradeoffs"></a>Teknik konular ve avantajsız

Ortamınızla bilinen sorunlar hakkında deneyimli olduğunuzu için artık hazırsınız.

Gerçek geçiş işleminde, düşünmek isteyebilirsiniz:

1. Planlama ve öncelik artan sanal ağ (en küçük birim geçiş) planlayın.  Öncelikle basit sanal ağlar ve daha karmaşık sanal ağlar ile ilerleme.
2. Çoğu müşteri, üretim dışı ve üretim ortamlarına sahip olur.  Üretim son zamanlayın.
3. **(İSTEĞE BAĞLI)**  Beklenmeyen sorunlar çıkması durumunda bakım kapalı kalma süresinin çok fazla arabellek zamanlayın.
4. İle iletişim kurmak ve sorunlar çıkması durumunda, destek ekipleri ile Hizala.

### <a name="patterns-of-success"></a>Başarı desenleri

Yukarıdaki Laboratuvar Test bölümü aracılığıyla teknik rehberlik kabul ve bir gerçek geçişten önce azaltılabilir.  Yeterli testiyle geçiş gerçekten dışı bir olay değil.  Üretim ortamları için bir güvenilen Microsoft iş ortağı veya Microsoft Premier services gibi ek destek yararlı olabilir.

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

Tam test sorunlarına neden ve geçişin gecikme.  

## <a name="beyond-migration"></a>Geçiş

### <a name="technical-considerations-and-tradeoffs"></a>Teknik konular ve avantajsız

Azure Resource Manager'da olduğuna göre platform en üst düzeye çıkarın.  Okuma [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md) ek avantajları hakkında bilgi edinmek için.

Göz önünde bulundurulması gerekenler:

- Diğer etkinlikler ile geçiş paketleme.  Çoğu müşteri, bir uygulama bakım penceresi için kabul et.  Bu durumda, şifreleme ve yönetilen Diskler'e geçiş gibi diğer Azure Resource Manager özelliklerini etkinleştirmek için bu kapalı kalma süresi kullanmak isteyebilirsiniz.
- Azure Resource Manager için teknik ve işletmeye nedeniyle yeniden ziyaret; ortamınız için geçerli sağlanan ek hizmetler yalnızca Azure Resource Manager'ı etkinleştirin.
- PaaS Hizmetleri ile ortamınızı modernleştirin.

### <a name="patterns-of-success"></a>Başarı desenleri

Artık Azure Resource Manager'da etkinleştirmek istediğiniz hangi Hizmetleri amaca yönelik olabilir.  Birçok müşteri için Azure ortamlarını cazip aşağıda:

- [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md).
- [Daha kolay ve daha denetimli dağıtımı için Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-overview.md#template-deployment).
- [Etiketleri](../../azure-resource-manager/resource-group-using-tags.md).
- [Etkinlik denetimi](../../azure-resource-manager/resource-group-audit.md)
- [Azure ilkeleri](../../governance/policy/overview.md)

### <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

Neden bu Klasik'ten Azure Resource Manager'a geçiş yolculuğu başlattığınız unutmayın.  Özgün iş nedenleri were İş nedeni ulaşmak mı?


## <a name="next-steps"></a>Sonraki adımlar

* [Iaas kaynaklarının Klasik modelden Azure Resource Manager'a platform destekli geçişe genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Iaas kaynaklarının Klasik'ten Azure Resource Manager'a geçiş için PowerShell kullanma](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Iaas kaynaklarını Klasik modelden Azure Resource Manager'a geçişini ile Yardım için topluluk araçları](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Gözden geçirme Iaas kaynaklarını Klasik modelden Azure Resource Manager'a hakkında sık sorulan sorular](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
