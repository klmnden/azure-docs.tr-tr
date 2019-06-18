---
title: SSS - Avere vFXT Azure
description: Azure için Avere vFXT hakkında sık sorulan sorular
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: v-erkell
ms.openlocfilehash: 47a4b38d39c52992b51284776ec34cb9491020e7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65595417"
---
# <a name="avere-vfxt-for-azure-faq"></a>Azure için Avere vFXT hakkında SSS

Bu makalede, Azure için Avere vFXT gereksinimleriniz için doğru seçenek olup olmadığını karar vermenize yardımcı olabilecek sorular yanıtlanmaktadır. Avere vFXT hakkında temel bilgiler verir ve diğer Azure bileşenlerini ve dış satıcılardan ürünleri ile nasıl çalıştığı açıklanmaktadır. 

## <a name="general"></a>Genel 

### <a name="what-is-avere-vfxt-for-azure"></a>Azure için Avere vFXT nedir?

Azure için Avere vFXT kritik iş yüklerinin verimli işleme için Azure işlem etkin verileri önbelleğe alan bir yüksek performanslı dosya sistemidir.

### <a name="is-avere-vfxt-a-storage-solution"></a>Bir depolama çözümü Avere vFXT mi?

Hayır. Avere vFXT olan bir dosya sistemi *önbellek* , EMC NetApp NAS veya bir Azure blob kapsayıcısı gibi depolama ortamlarına ekler. Avere vFXT veri istemcilerden gelen istekleri verimli ve uygun ölçekte ve zaman içerisinde performansı artırmak için hizmet verileri önbelleğe alır. Avere vFXT kendisi verileri depolamaz. Bu, arkasına depolanan veri miktarı hakkında bir bilgi bulunmaz.

### <a name="is-avere-vfxt-a-tiering-solution"></a>Çözüm katmanlama Avere vFXT mi?

Avere vFXT sık ve seyrek erişimli Katmanlar arasındaki katman verileri otomatik olarak yapar.  

### <a name="how-do-i-know-if-an-environment-is-right-for-avere-vfxt"></a>Bir ortam Avere vFXT için doğru seçenek olup olmadığını nasıl anlarım?

Bu soruyu hakkında düşünmek için en iyi yolu sormaktır, "iş yükü önbelleğe alınabilir mi?" Diğer bir deyişle, iş yüküne yüksek okuma ve yazma oranı var mı? 80/20 veya 70/30 okumayı/yazmayı buna bir örnektir.

Avere vFXT Azure'a yönelik çok sayıda Azure sanal makineler üzerinde çalışan bir dosya tabanlı analiz işlem hattı varsa ve bir veya daha fazla aşağıdaki koşulları karşılayan göz önünde bulundurun:

* Genel performans uzun dosya erişim zamanları (on gereksinimlerine bağlı olarak, saniye ve milisaniye) nedeniyle yavaş veya tutarsız. Bu gecikme süresi için müşteri tarafından kabul edilmez.

* Verilerin işlenmesi için gereken WAN ortam en sonunda bulunan ve verilerin kalıcı olarak taşınması zordur. Verileri, bir müşteri veri merkezinde veya farklı bir Azure bölgesinde olabilir.

* İstemcilerin çok sayıda veri - Örneğin, bir yüksek performanslı bilgi işlem (HPC) kümesinde isteniyor. Çok sayıda eş zamanlı istek gecikme süresini artırabilir.

* Müşteri, geçerli işlem hattı çalıştırma ister "olduğu gibi" Azure sanal makinelerinde ve ihtiyaçlarını POSIX tabanlı depolama (veya önbelleğe alma) paylaşılan ölçeklenebilirlik için çözüm. Azure için Avere vFXT kullanarak Azure Blob Depolama yerel çağrı yapmak için iş işlem hattını yeniden oluşturma gerekmez.

* HPC uygulamanızı NFSv3 istemcilerde temel alır. (Bazı durumlarda, SMB 2.1 istemcileri kullanabilirsiniz, ancak performans sınırlıdır.)

Aşağıdaki diyagramda, bu sorunun yanıtı basitleştirir. İş akışınızı üst sağa edilirse, önbelleğe alma çözümü Avere ortamınız için uygun olduğu daha yakındır.

![binlerce istemciden okuma ağır yüklerle daha iyi olduğunu gösteren diyagram Avere vFXT için uygun](media/avere-vfxt-fit-assessment.png)

### <a name="at-what-scale-of-clients-does-the-avere-vfxt-solution-make-the-most-sense"></a>Hangi istemcilerin ölçeğini Avere mu vFXT çözüm en anlamlı yapılsın mı?

Avere vFXT önbellek çözümü, yüzlerce, binlerce veya on binlerce işlem çekirdeğine işlemek için oluşturulmuştur. Hafif iş çalışan birkaç makineleriniz varsa, Avere vFXT doğru çözüm değildir.

Tipik Avere vFXT müşteriler yaklaşık 1.000 CPU çekirdeği başlangıç zorlu iş yüklerini çalıştırın. Bu ortamların çekirdek 50.000 veya daha büyük olabilir. Avere vFXT ölçeklenebilir olduğundan, daha fazla üretilen işi veya daha yüksek IOPS gerektirecek şekilde büyüdükçe, bu iş yüklerini desteklemek için düğümleri ekleyebilirsiniz.

### <a name="how-much-data-can-an-avere-vfxt-environment-store"></a>Ne kadar veri Avere vFXT ortam depolayabilir miyim?

Avere vFXT bir önbellektir. Özellikle veri depolama değil. Önbelleğe alınmış verileri depolamak için RAM ve SSD kullanır. Veriler, bir arka uç depolama sisteminde (örneğin, NetApp NAS sistem veya bir blob kapsayıcısı) kalıcı olarak depolanır. Avere vFXT sistem arkasına depolanan veri miktarı hakkında bilgi yok. Avere vFXT yalnızca istemciler istemek, verilerin alt kümesini önbelleğe alır.  

### <a name="what-regions-are-supported"></a>Hangi bölgeler desteklenir?

Azure için Avere vFXT bağımsız bölgelerde (Çin, Almanya) dışındaki tüm bölgelerde desteklenir. Kullanmak istediğiniz bölgeyi işlem Çekirdeği ve Avere vFXT küme oluşturmak için gereken sanal makine örneklerinin büyük miktarlarda desteklediğinden emin olun.

### <a name="how-do-i-get-help-with-avere-vfxt"></a>Avere vFXT yardımıyla nasıl alabilirim?

Bir özel destek grubu Avere vFXT ile ilgili Yardım için Azure'ı sunar. Bölümündeki yönergeleri [sisteminizle Yardım Al](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) Azure portalından bir destek bileti açın. 

### <a name="is-avere-vfxt-highly-available"></a>Avere vFXT yüksek oranda kullanılabilir mi?

Evet, özel bir HA çözümü olarak Avere vFXT çalıştırır.

### <a name="does-avere-vfxt-for-azure-also-support-other-cloud-services"></a>Azure için Avere vFXT ayrıca diğer bulut hizmetlerini destekliyor mu?

Evet, müşteriler birden fazla bulut sağlayıcısı Avere vFXT kümesiyle kullanabilirsiniz. Bu, AWS S3 standart demetleri, Google bulut Hizmetleri standart demetleri ve Azure blob kapsayıcıları destekler. 

> [!NOTE] 
> Avere vFXT AWS veya Google Cloud, ancak değil Azure ile kullanmak için bir yazılım ücreti uygulanır.

## <a name="technical-compute"></a>Teknik: İşlem

### <a name="can-you-describe-what-an-avere-vfxt-environment-looks-like"></a>"Benzer" hangi Avere vFXT ortam açıklayabilirsiniz?

Avere vFXT yapılan birden çok Azure sanal makineleri, kümelenmiş bir gereçtir. Python kitaplığı, küme oluşturma, silme ve değiştirme işler. Okuma [Avere vFXT Azure nedir?](avere-vfxt-overview.md) daha fazla bilgi için. 

### <a name="what-kind-of-azure-virtual-machines-does-avere-vfxt-run-on"></a>Ne tür bir Azure sanal makineleri Avere vFXT çalışıyor mu?  

Bir Avere vFXT Azure küme için Microsoft Azure E32s_v3 sanal makineler kullanır. 

<!-- ### Can I mix and match virtual machine types for my cluster?

No, you must choose one virtual machine type or the other.
    
### Can I move between virtual machine types?

Yes, there is a migration path to move from one VM type to the other. [Open a support ticket](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) to learn how.

-->

### <a name="does-the-avere-vfxt-environment-scale"></a>Avere vFXT ortamınızı ölçeklendirme mu?

Avere vFXT küme üç sanal makine düğümlerinin kadar küçük veya 24 düğümleri büyüklüğünde olabilir. Birden fazla dokuz düğümden oluşan bir küme gerekir düşünüyorsanız planlama konusunda yardım için Azure teknik desteğine başvurun. Daha fazla düğüm sayısıyla daha büyük bir dağıtım mimarisi gerektirir.

### <a name="does-the-avere-vfxt-environment-autoscale"></a>"Otomatik ölçeklendirme" Avere vFXT ortam mu?

Hayır. Küme boyutu yukarı ve aşağı ölçeklendirilebilir, ancak Küme düğüm ekleme veya kaldırma el ile gerçekleştirilen bir adımdır.

### <a name="can-i-run-the-avere-vfxt-cluster-as-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi Avere vFXT küme çalıştırabilir miyim?

Bir sanal makine ölçek kümesi dağıtımı Avere vFXT desteklemez. Çeşitli yerleşik kullanılabilirlik desteği mekanizmalar, yalnızca bir kümeye katılan atomik VM'ler için tasarlanmıştır.  

### <a name="can-i-run-the-avere-vfxt-cluster-on-low-priority-vms"></a>Düşük öncelikli VM'ler üzerinde Avere vFXT küme çalıştırabilir miyim?

Hayır, sistem kararlı bir temel alınan sanal makine kümesi olmasını gerektirir.

### <a name="can-i-run-the-avere-vfxt-cluster-in-containers"></a>Kapsayıcılarda Avere vFXT küme çalıştırabilir miyim?

Hayır, Avere vFXT bağımsız bir uygulama dağıtılması gerekir.

### <a name="do-the-avere-vfxt-vms-count-against-my-compute-quota"></a>Sanal makine sayısının karşı Bilgisayarım işlem kotası Avere vFXT musunuz?

Evet. Bölge kümesini desteklemek için yeterli kotası olduğundan emin olun.  

### <a name="can-i-run-the-avere-vfxt-cluster-machines-in-different-availability-zones"></a>Farklı kullanılabilirlik alanlarında Avere vFXT küme makineler çalıştırabilir miyim?

Hayır. Şu anda Avere vFXT yüksek kullanılabilirlik modelinde farklı kullanılabilirlik alanlarında bulunan tek Avere vFXT küme üyelerini desteklemez.

### <a name="can-i-clone-avere-vfxt-virtual-machines"></a>Avere vFXT sanal makineler kopyalayabilir miyim?

Hayır, ekleme veya düğümleri kaldırma Avere vFXT kümede için desteklenen Python betiğini kullanmanız gerekir. Daha fazla bilgi için okuma [Avere vFXT kümesini yönetme](avere-vfxt-manage-cluster.md).  

### <a name="is-there-a-vm-version-of-the-software-i-can-run-in-my-own-local-environment"></a>Kendi yerel ortamda çalıştırabilir miyim yazılım "VM" sürümü var mı?

Hayır, sistem kümelenmiş bir gereç sunulan ve belirli bir sanal makine türlerini test. Bu kısıtlama, tipik bir Avere vFXT iş akışının yüksek performanslı gereksinimlerini destekleyemiyorsa bir sistem oluşturmaktan kaçının müşterilere yardımcı olur. 

## <a name="technical-disks"></a>Teknik: Diskler

### <a name="what-types-of-disks-are-supported-for-the-azure-vms"></a>Azure Vm'leri için hangi disk türleri desteklenir?

Azure için Avere vFXT 1 TB veya 4 TB premium SSD yapılandırmalar kullanabilirsiniz. Premium SSD yapılandırma, birden fazla yönetilen disk dağıtılabilir.

### <a name="does-the-cluster-support-unmanaged-disks"></a>Küme, yönetilmeyen diskler destekliyor mu?

Hayır, küme, yönetilen diskler gerekir.

### <a name="does-the-system-support-local-attached-ssds"></a>Sistem yerel (ekli) SSD destekliyor mu?

Azure için Avere vFXT yerel SSD şu anda desteklemiyor. Avere vFXT için kullanılan disk kapatın ve yeniden başlatmanız gerekir, ancak yerel ekli Ssd'lerde bu yapılandırma yalnızca sonlandırılabilir.

### <a name="does-the-system-support-ultra-ssds"></a>Sistem ultra SSD destekliyor mu?

Hayır, sistem yalnızca premium SSD yapılandırmaları destekler.

### <a name="can-i-detach-my-premium-ssds-and-reattach-them-later-to-preserve-cache-contents-between-use"></a>My premium SSD ayırma ve miyim bunları daha sonra kullanım arasında önbellek içeriği korumak için yeniden?

Ayırma ve SSD yeniden bağlanması desteklenmiyor. Veri bütünlüğü sorunları neden olabilecek kullanımlar arasında kaynak meta verileri veya dosya içeriği değişmiş olabilir.

### <a name="does-the-system-encrypt-the-cache"></a>Sistem önbelleği şifreleyebilir mi?

Veri disklere yayılarak, ancak şifrelenmez. Ancak, diskleri şifrelenebilir. Daha fazla bilgi için [Azure içindeki sanal makinelerde güvenli ve kullanım ilkeleri](https://docs.microsoft.com/azure/virtual-machines/linux/security-policy#encryption).

## <a name="technical-networking"></a>Teknik: Ağ

### <a name="what-network-is-recommended"></a>Hangi ağ önerilir?

Şirket içi depolama ile Avere vFXT kullanıyorsanız, 1 GB/sn veya daha iyi ağ bağlantısı olmalıdır. Küçük miktarda veriniz varsa ve işleri çalıştırmadan önce verileri buluta kopyalama istediğiniz VPN bağlantısı yeterli olabilir. 

> [!TIP] 
> Yavaş ağ bağlantısı, ilk soğuk okuma daha yavaş olur. Yavaş okuma iş işlem hattı gecikme süresini artırın. 

### <a name="can-i-run-avere-vfxt-in-a-different-virtual-network-than-my-compute-cluster"></a>İşlem kümem farklı bir sanal ağda Avere vFXT çalıştırabilir miyim?

Evet, farklı bir sanal ağda Avere vFXT sisteminizi oluşturabilirsiniz. Okuma [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md) Ayrıntılar için.

### <a name="does-avere-vfxt-require-its-own-subnet"></a>Kendi alt ağına Avere vFXT gerektiriyor mu?

Evet. Avere vFXT yüksek kullanılabilirlik (HA) kümesi olarak kesin olarak çalışır ve çalışması için birden çok IP adresi gerektirir. Küme, kendi alt ağına ise, yükleme ve normal işlem için sorunlara neden olabilir IP adresi çakışmaları riski kaçının. Çakışma hiçbir IP adresleri sürece kümenin alt var olan sanal ağ içinde olabilir.

### <a name="can-i-run-avere-vfxt-on-infiniband"></a>Avere vFXT InfiniBand üzerinde çalıştırabilir mi?

Hayır, yalnızca Ethernet/IP Avere vFXT kullanır.

### <a name="how-do-i-access-my-on-premises-nas-environment-from-avere-vfxt"></a>Şirket içi NAS ortamımın Avere vFXT nasıl erişebilirim?

Müşteri veri merkezi (ve geriye) bir ağ geçidi veya VPN üzerinden gönderilmiş erişimi gerektirir, Avere vFXT herhangi bir Azure VM gibi ortamıdır. Ortamınızda varsa, Azure ExpressRoute bağlantısı kullanmayı düşünün.

### <a name="what-are-the-bandwidth-requirements-for-avere-vfxt"></a>Avere vFXT için bant genişliği gereksinimleri nelerdir?

Toplam bant genişliğinin iki etkenlere bağlıdır: 

* Kaynak istenen veri miktarı 
* İlk veri yükleme sırasında istemci sistemin dayanıklılık gecikmesi  

Gecikmeye duyarlı ortamlar için 1 GB/sn ile bir minimum bağlantı hızı fiber çözüm kullanmanız gerekir. Varsa Expressroute'u kullanın.  

### <a name="can-i-run-avere-vfxt-with-public-ip-addresses"></a>Genel IP adresleri ile Avere vFXT çalıştırabilir miyim?

Hayır, en iyi yöntemler güvenli bir ağ ortamında yapılacak Avere vFXT yöneliktir.  

### <a name="can-i-restrict-internet-access-from-my-clusters-virtual-network"></a>My kümenin sanal ağdan internet erişiminin kısıtlandığı? 

Genel olarak, ek güvenlik gerektiğinde sanal ağınızda yapılandırabilirsiniz, ancak bazı kısıtlamalar ile küme işlemi engelleyebilir.

Açıkça AzureCloud erişmesine izin veren bir kuralı da eklemediğiniz sürece, ağınızdan giden internet erişimi kısıtlama sorunlara küme için neden olur. Bu durumda açıklanan [ek belgeleri github'da](https://github.com/Azure/Avere/tree/master/src/vfxt/internet_access.md).

Bölümünde anlatıldığı gibi özelleştirilmiş güvenlik konusunda yardım için desteğe başvurun [sisteminizle Yardım Al](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt).

## <a name="technical-back-end-storage-core-filers"></a>Teknik: Arka uç depolama (çekirdek filtrelerin)

### <a name="how-many-core-filers-does-a-single-avere-vfxt-environment-support"></a>Kaç tane çekirdek filtrelerin tek bir Avere vFXT ortam destekliyor mu?

20 adede kadar çekirdek filtrelerin bir Avere vFXT kümesini destekler. 

### <a name="how-does-the-avere-vfxt-environment-store-data"></a>Nasıl Avere vFXT ortam veri depoluyor mu?

Avere vFXT depolama değildir. Okuyan ve çekirdek filtrelerin adlı birden çok depolama hedeflerden veri yazan bir önbellektir. Premium SSD diskleri Avere vFXT içinde depolanan veriler geçici ve sonunda arka uç çekirdek dosyalayıcı depolama temizlenir.

### <a name="which-core-filers-does-avere-vfxt-support"></a>Hangi çekirdek filtrelerin Avere vFXT destekliyor mu?

Genel koşullarını Avere vFXT Azure çekirdek filtrelerin aşağıdaki sistemlerini destekler: 

* Dell EMC Isilon (OneFS 7.1, 7.2, 8.0 ve 8.1) 
* NetApp ONTAP (kümelenmiş modu 9.4 sürümünden, 9.3, 9.2, 9.1P1, 8.3 8.0) ve (7 modu 7.*, 8.3 8.0) 

  > [!NOTE] 
  > NetApp Azure dosyaları şu anda desteklenmiyor. 

* Azure blob kapsayıcıları (yalnızca yerel olarak yedekli depolama) 
* AWS S3 demet 
* Google Cloud demetlerine

### <a name="why-doesnt-avere-vfxt-support-all-nfs-filers"></a>Neden Avere vFXT tüm NFS filtrelerin desteklemiyor?

Tüm NFS platformlar aynı IETF standartlara uyduğumuzu olsa da, uygulamada her uygulama kendi quirks sahiptir. Bu ayrıntılar Avere vFXT depolama sistemi ile nasıl etkileştiğini etkiler. En yaygın olarak kullanılan platformları Marketi'nde desteklenen sistemleridir.

### <a name="does-avere-vfxt-support-private-object-storage-such-as-swiftstack"></a>Avere vFXT özel nesne depolama (örneğin, SwiftStack) destekliyor mu?

Özel bir nesne depolama Avere vFXT desteklemez.

### <a name="how-can-i-get-a-specific-storage-product-under-support"></a>Belirli depolama ürün destek kapsamında nasıl alabilirim?

Destek, isteğe bağlı olarak alan miktarını dayanır. Bir NAS çözümü desteklemek için yeterli gelir tabanlı istekler varsa, onu ele alacağız. Azure destek isteklerini olun.

### <a name="can-i-use-azure-blob-storage-as-a-core-filer"></a>Bir çekirdek dosyalayıcı Azure Blob Depolama kullanabilir miyim?

Evet, Avere vFXT Azure blok blob kapsayıcısına bir bulut çekirdek dosyalayıcı kullanabilirsiniz.  

### <a name="what-are-the-storage-account-requirements-for-a-blob-core-filer"></a>Bir blob çekirdek dosyalayıcı için depolama hesabı gereksinimleri nelerdir?

Genel amaçlı v2 depolama hesabınızın olması gerekir (GPv2) hesabı ve yalnızca yerel olarak yedekli depolama için yapılandırılmış. Coğrafi olarak yedekli depolama ve bölgesel olarak yedekli depolama desteklenmez.

### <a name="can-i-use-archive-blob-storage"></a>Arşiv blob depolama kullanabilir miyim?

Hayır. Arşiv depolama için hizmet düzeyi sözleşmesi (SLA) gerçek zamanlı dizin ve dosya erişim gereksinimlerini Avere vFXT sistem ile uyumlu değil. 

### <a name="can-i-use-cool-blob-storage"></a>Seyrek erişimli blob depolama kullanabilir miyim?

Seyrek erişimli katmana kullanır, ancak işlemlerinin hızıdır. çok daha yüksek olacağını unutmayın. 

### <a name="how-do-i-encrypt-the-blob-container"></a>Blob kapsayıcısını nasıl şifreliyor mu?

Blob şifrelemesi (tercih edilir) Azure veya Avere vFXT çekirdek dosyalayıcı düzeyinde yapılandırabilirsiniz.  

### <a name="can-i-use-my-own-encryption-key-for-a-blob-core-filer"></a>Kendi şifreleme anahtarı için bir blob çekirdek dosyalayıcı kullanabilir miyim?

Varsayılan olarak, Azure Blob, tablo ve kuyruk depolamanın yanı sıra, Azure dosyaları için Microsoft tarafından yönetilen anahtarlar aracılığıyla veriler şifrelenir. Blob Depolama ve Azure dosyaları için şifreleme için kendi anahtarınızı getirebilirsiniz. Avere vFXT şifreleme kullanmayı seçerseniz, Avere oluşturulan anahtarı kullanın ve yerel olarak depolar. 

## <a name="purchasing"></a>Satın alma

### <a name="how-do-i-get-avere-vfxt-for-azure-licensing"></a>Azure lisanslama Avere vFXT nasıl alabilirim?

Azure lisansını Avere vFXT başlama Azure Marketi aracılığıyla kolay bir işlemdir. Bir Azure hesabı için kaydolun ve içindeki yönergeleri izleyin [Avere vFXT kümeyi dağıtmak](avere-vfxt-deploy.md) bir Avere vFXT kümesi oluşturmak için. 

### <a name="how-much-does-avere-vfxt-cost"></a>Avere vFXT maliyeti ne kadar?

Azure'da Avere vFXT kümelerini kullanarak hiçbir ek lisans ücreti yoktur. Müşteriler, depolama ve diğer Azure kullanım ücretleri için sorumludur.

### <a name="can-avere-vfxt-vms-be-run-as-low-priority"></a>Avere vFXT VM düşük öncelikli olarak çalıştırabilir miyim?

Hayır, Avere vFXT kümeleri "her zaman açık" gerektiren hizmet. Kümeler, gerekli olduğunda kapatılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure için Avere vFXT ile çalışmaya başlamak için planlama ve kendi sisteminizi dağıtma hakkında bilgi edinmek için bu makaleleri okuyun:

* [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md)
* [Dağıtıma genel bakış](avere-vfxt-deploy-overview.md)
* [Bir Avere vFXT kümesi oluşturmak hazırlama](avere-vfxt-prereqs.md)
* [Avere vFXT kümesini dağıtma](avere-vfxt-deploy.md)

Avere vFXT için kullanım örnekleri ve özellikleri hakkında daha fazla bilgi edinmek için şurayı ziyaret edin [Avere vFXT Azure](https://azure.microsoft.com/services/storage/avere-vfxt/).
