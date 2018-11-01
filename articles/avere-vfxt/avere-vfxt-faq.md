---
title: SSS - Avere vFXT Azure
description: Azure için Avere vFXT hakkında sık sorulan sorular
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 334b4c912c40949cbecab2173425927d46350d07
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634524"
---
# <a name="avere-vfxt-for-azure-faq"></a>Azure ile ilgili SSS için Avere vFXT

Bu makalede Avere vFXT çözüm ihtiyaçlarınız için doğru seçenek olup olmadığını karar vermenize yardımcı olabilecek sorular yanıtlanmaktadır. Avere vFXT'ın özellikleri hakkında temel bilgiler verir ve diğer Azure bileşenlerini ve dış satıcılardan ürünleri ile nasıl çalıştığı açıklanmaktadır. 

## <a name="general"></a>Genel 

### <a name="what-is-avere-vfxt-for-azure"></a>Azure için Avere vFXT nedir?

Azure için Avere vFXT kritik iş yüklerinin verimli işleme için Azure işlem içinde etkin verileri önbelleğe alan bir yüksek performanslı dosya sistemidir.

### <a name="is-the-avere-vfxt-a-storage-solution"></a>Bir depolama çözümü Avere vFXT mi?

Hayır. Avere vFXT olan bir dosya sistemi **önbellek** , EMC NetApp NAS veya bir Blob kapsayıcısı gibi depolama ortamlarına ekler. VFXT veri istemcilerden gelen istekleri verimli ve uygun ölçekte ve zaman içerisinde performansı artırmak için hizmet verileri önbelleğe alır. VFXT kendisi verileri depolamaz. Bu, arkasına depolanan veri miktarı hakkında bir bilgi bulunmaz.

### <a name="is-the-avere-vfxt-a-tiering-solution"></a>Çözüm katmanlama Avere vFXT mi?

Avere vFXT sık ve seyrek erişimli Katmanlar arasındaki katman verileri otomatik olarak yapar.  

### <a name="how-do-i-know-if-an-environment-is-right-for-the-avere-vfxt"></a>Bir ortam Avere vFXT için doğru seçenek olup olmadığını nasıl anlarım?

Bu soruyu hakkında düşünmek için en iyi yolu sormaktır, "iş yükü önbelleğe alınabilir mi?" Diğer bir deyişle, iş yükü yazma oranı - Örneğin, 80/20 veya 70/30 okumayı/yazmayı yüksek okuma sahip.

Avere vFXT Azure'a yönelik çok sayıda Azure sanal makineler üzerinde çalışan bir dosya tabanlı analiz işlem hattı varsa ve bir veya daha fazla aşağıdaki koşulları karşılayan göz önünde bulundurun:

* Genel performans uzun dosya erişim zamanları (on gereksinimlerine bağlı olarak, saniye ve milisaniye) nedeniyle yavaş veya tutarsız. Bu gecikme süresi son müşterinin kabul edilmez.

* Verilerin işlenmesi için gereken WAN ortam en sonunda bulunan ve verilerin kalıcı olarak taşınması zordur. Verileri, bir müşteri veri merkezinde veya farklı bir Azure bölgesinde olabilir.

* İstemcilerin çok sayıda veri - Örneğin, bir yüksek performanslı bilgi işlem (HPC) kümesinde isteniyor. Çok sayıda eş zamanlı istek gecikme süresini artırabilir.

* Müşteri, geçerli işlem hattı çalıştırma ister "olarak-olan" Azure sanal makineleri ve gereksinimlerini POSIX tabanlı depolama (veya önbelleğe alma) paylaşılan ölçeklenebilirlik için çözüm. Azure için Avere vFXT kullanarak Azure Blob Depolama yerel çağrı yapmak için iş işlem hattını yeniden oluşturma gerekmez.

* HPC uygulamanızı NFSv3 istemcilerde temel alır. (Bazı durumlarda, SMB 2.1 istemcisi kullanılabilir, ancak performans sınırlıdır.)

   Aşağıdaki grafik, bu sorunun yanıtı basitleştirmek çalışır. İş akışınızı üst sağa edilirse, önbelleğe alma çözümü Avere ortamınız için uygun olduğu daha yakındır.

   ![binlerce istemciden okuma ağır yüklerle daha iyi olduğunu gösteren diyagram Avere vFXT için uygun](media/avere-vfxt-fit-assessment.png)

### <a name="at-what-scale-of-clients-does-the-avere-vfxt-solution-make-the-most-sense"></a>Hangi istemcilerin ölçeğini Avere mu vFXT çözüm en anlamlı yapılsın mı?

VFXT önbellek çözümü, yüzlerce, binlerce veya on binlerce işlem çekirdeğine işlemek için oluşturulmuştur. Hafif iş çalışan birkaç makineleriniz varsa, Avere vFXT doğru çözüm değildir.

Tipik Avere vFXT müşteriler yaklaşık 1.000 CPU çekirdeği başlangıç zorlu iş yüklerini çalıştırın. Bu ortamların çekirdek 50.000 veya daha büyük olabilir. VFXT ölçeklenebilir olduğundan, daha fazla üretilen işi veya daha yüksek IOPS gerektirecek şekilde büyüdükçe, bu iş yüklerini desteklemek için düğümleri ekleyebilirsiniz.

### <a name="how-much-data-can-an-avere-vfxt-environment-store"></a>Ne kadar veri Avere vFXT ortam depolayabilir miyim?

Bir önbellek Avere vFXT, özellikle verilerini depolamaz. Önbelleğe alınmış verileri depolamak için RAM ve SSD kullanır. Veriler, bir arka uç depolama sisteminde (örneğin, NetApp NAS sistem veya bir Blob kapsayıcısı) kalıcı olarak depolanır. Avere vFXT sistem arkasına depolanan veri miktarı hakkında bilgi yok; vFXT yalnızca istemciler istemek, verilerin alt kümesini önbelleğe alır.  

### <a name="what-regions-are-supported"></a>Hangi bölgeler desteklenir?

1 Kasım 2018'den itibaren tüm bölgelerde (Çin, Almanya) bağımsız bölgeler dışında ve kamu bölgelerinde Avere vFXT Azure için desteklenir. Kullanmak istediğiniz bölgeyi Avere vFXT kümeyi oluşturmak için gereken VM örnek yanı sıra, işlem çekirdek büyük miktarlarda desteklediğinden emin olun. Bağımsız bölgeler ve kamu bulutlarında henüz desteklenmiyor.

### <a name="how-do-i-get-help-with-the-avere-vfxt"></a>Avere vFXT yardımıyla nasıl alabilirim?

Bir özel destek grubu Avere vFXT ile ilgili Yardım için Azure'ı sunar. Bölümündeki yönergeleri [sisteminizle Yardım Al](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) Azure portalından bir destek bileti açın. 

### <a name="is-the-avere-vfxt-highly-available"></a>Avere vFXT yüksek oranda kullanılabilir mi?

Evet, özel bir HA çözümü olarak Avere vFXT çalıştırır.

### <a name="does-avere-vfxt-for-azure-also-support-other-cloud-services"></a>Azure için Avere vFXT ayrıca diğer bulut hizmetlerini destekliyor mu?

Evet, müşteriler birden fazla bulut sağlayıcısı Avere vFXT kümesiyle kullanabilirsiniz. Bu, Azure Blob kapsayıcıları yanı sıra AWS S3 standart demetleri ve Google Cloud Services standart demet destekler. 

> [!NOTE] 
> Avere vFXT AWS veya Google Cloud, ancak değil Azure ile kullanmak için bir yazılım ücreti uygulanır.

## <a name="technical---compute"></a>Teknik - işlem

### <a name="can-you-describe-what-an-avere-vfxt-environment-looks-like"></a>"Benzer" hangi Avere vFXT ortam açıklayabilirsiniz?

Avere vFXT yapılan birden çok Azure sanal makineleri, kümelenmiş bir gereçtir. Python kitaplığı, küme oluşturma, silme ve değiştirme işler. Okuma [Avere vFXT Azure nedir?](avere-vfxt-overview.md) daha fazla bilgi için. 

### <a name="what-kind-of-azure-virtual-machines-does-the-avere-vfxt-run-on"></a>Ne tür bir Azure sanal makineleri vFXT çalıştıracağınız Avere mu?  

Avere vFXT Azure küme için Microsoft Azure E32s_v3 ya da D16s_v3 sanal makineler kullanır. 

### <a name="can-i-mix-and-match-virtual-machine-types-for-my-cluster"></a>Karışık ve miyim kümem için sanal makine türlerinin eşleşmesi?

Hayır, bir sanal makine türünü veya diğer seçmeniz gerekir.
    
### <a name="can-i-move-between-virtual-machine-types"></a>Sanal makine türleri arasında taşıyabilirim?

Evet, bir VM türünden diğerine taşımak için bir geçiş yolu yoktur. [Bir destek bileti açın](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) bilgi edinmek için nasıl.

### <a name="does-the-avere-vfxt-environment-scale"></a>Avere vFXT ortamınızı ölçeklendirme mu?

Avere vFXT küme üç sanal makine düğümlerinin kadar küçük veya 24 düğümleri büyüklüğünde olabilir. Azure teknik destek için Yardım'ı birden fazla dokuz düğümden oluşan bir küme gerekir düşünüyorsanız planlama başvurun. Daha fazla düğüm sayısıyla daha büyük dağıtım mimarisi gerektirir.

### <a name="does-the-avere-vfxt-environment-autoscale"></a>"Otomatik ölçeklendirme" Avere vFXT ortam mu?

Hayır. Küme boyutu yukarı ve aşağı ölçeklendirilebilir, ancak Küme düğüm ekleme veya kaldırma el ile gerçekleştirilen bir adımdır.

### <a name="can-i-run-the-vfxt-cluster-as-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi vFXT küme çalıştırabilir miyim?

Sanal makine ölçek kümesi (VMSS) dağıtımına Avere vFXT desteklemez. Çeşitli yerleşik kullanılabilirlik desteği mekanizmalar, yalnızca bir kümeye katılan atomik VM'ler için tasarlanmıştır.  

### <a name="can-i-run-the-vfxt-cluster-on-low-priority-vms"></a>Düşük öncelikli VM'ler üzerinde vFXT küme çalıştırabilir miyim?

Hayır, sistem kararlı bir temel alınan sanal makine kümesi olmasını gerektirir.

### <a name="can-i-run-the-vfxt-cluster-in-containers"></a>Kapsayıcılarda vFXT küme çalıştırabilir miyim?

Hayır, Avere vFXT bağımsız bir uygulama dağıtılması gerekir.

### <a name="do-the-avere-vfxt-vms-count-against-my-compute-quota"></a>Sanal makine sayısının karşı Bilgisayarım işlem kotası Avere vFXT musunuz?

Evet. Bölge kümesini desteklemek için yeterli kotası olduğundan emin olun.  

### <a name="can-i-run-the-avere-vfxt-cluster-machines-in-different-availability-zones"></a>Farklı kullanılabilirlik alanlarında Avere vFXT küme makineler çalıştırabilir miyim?

Hayır. Tek tek vFXT küme üyeleri farklı kullanılabilirlik alanlarında bulunan Avere vFXT içinde şu anda kullanılan yüksek kullanılabilirlik modelini desteklemiyor.

### <a name="can-i-clone-avere-vfxt-virtual-machines"></a>Avere vFXT sanal makineler kopyalayabilir miyim?

Hayır, ekleme veya düğümleri kaldırma Avere vFXT kümede için desteklenen Python betiğini kullanmanız gerekir. Daha fazla bilgi için okuma [Avere vFXT kümesini yönetme](avere-vfxt-manage-cluster.md).  

### <a name="is-there-a-vm-version-of-the-software-i-can-run-in-my-own-local-environment"></a>Kendi yerel ortamda çalıştırabilir miyim yazılım "VM" sürümü var mı?

Hayır, sistem kümelenmiş bir gereç sunulan ve belirli bir sanal makine türlerini test. Bu kısıtlama, tipik bir Avere vFXT iş akışının yüksek performans gereksinimlerini destekleyemiyorsa bir sistem oluşturmaktan kaçının müşterilere yardımcı olur. 

## <a name="technical---disks"></a>Teknik - diskler

### <a name="what-type-of-disks-are-supported-for-the-azure-vms"></a>Azure Vm'leri için ne tür disk desteklenir?

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

Veri disklere yayılarak, ancak şifrelenmez. Ancak, diskleri şifrelenebilir. Daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/virtual-machines/linux/security-policy#encryption).

## <a name="technical---networking"></a>Teknik - ağ

### <a name="what-network-is-recommended"></a>Hangi ağ önerilir?

Kullanarak Avere vFXT depolamada şirket gerekiyorsa, 1 GB/sn veya daha iyi ağ bağlantısı olması gerekir. Küçük miktarda veriniz varsa ve işleri çalıştırmadan önce verileri buluta kopyalama olursunuz, VPN bağlantısı yeterli olabilir. 

> [!TIP] 
> Yavaş ağ bağlantısı, ilk soğuk okuma daha yavaş olur. Yavaş okuma iş işlem hattı gecikme süresini artırın. 

### <a name="can-i-run-the-avere-vfxt-in-a-different-virtual-network-than-my-compute-cluster"></a>İşlem kümem farklı bir sanal ağda Avere vFXT çalıştırabilir miyim?

Evet, farklı bir sanal ağda Avere vFXT sisteminizi oluşturabilirsiniz. Okuma [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md) Ayrıntılar için.

### <a name="does-the-avere-vfxt-require-its-own-subnet"></a>Kendi alt ağına Avere vFXT gerektiriyor mu?

Evet. Avere vFXT HA kümesi olarak kesin olarak çalışır ve çalışması için birden çok IP adresi gerektirir. Küme, kendi alt ağına ise, yükleme ve normal işlem için sorunlara neden olabilir IP adresi çakışmaları riski kaçının. Çakışma hiçbir IP adresleri sürece kümenin alt var olan sanal ağ içinde olabilir.

### <a name="can-i-run-the-avere-vfxt-on-infiniband"></a>Avere vFXT InfiniBand üzerinde çalıştırabilir mi?

Hayır, yalnızca Ethernet/IP Avere vFXT kullanır.

### <a name="how-do-i-access-my-on-premises-nas-environment-from-the-avere-vfxt"></a>Şirket içi NAS ortamımın Avere vFXT nasıl erişebilirim?

Müşteri veri merkezi (ve geriye) bir ağ geçidi veya VPN üzerinden gönderilmiş erişimi gerektirir, Avere vFXT ortam herhangi bir Azure VM farklı değildir. Ortamınızda varsa, Azure ExpressRoute bağlantısı kullanmayı düşünün.

### <a name="what-are-the-bandwidth-requirements-for-the-avere-vfxt"></a>Avere vFXT için bant genişliği gereksinimleri nelerdir?

Toplam bant genişliğinin iki etkenlere bağlıdır: 

* Kaynak istenen veri miktarı 
* İlk veri yükleme sırasında istemci sistemin dayanıklılık gecikmesi  

Gecikmeye duyarlı ortamlar için 1 GB/sn ile bir minimum bağlantı hızı fiber çözüm kullanmanız gerekir. Varsa Expressroute'u kullanın.  

### <a name="can-i-run-the-vfxt-with-public-ip-addresses"></a>Genel IP adresleri ile vFXT çalıştırabilir miyim?

Hayır, en iyi yöntemler kullanılarak güvenli bir ağ ortamında yapılacak vFXT yöneliktir.  

## <a name="technical---backend-storage-core-filers"></a>Teknik - arka uç depolama (çekirdek filtrelerin)

### <a name="how-many-core-filers-does-a-single-avere-vfxt-environment-support"></a>Kaç tane çekirdek filtrelerin tek bir Avere vFXT ortam destekliyor mu?

20 adede kadar çekirdek filtrelerin bir Avere vFXT kümesini destekler. 

### <a name="how-does-the-avere-environment-store-data"></a>Nasıl Avere ortamı veri depoluyor mu?

Avere vFXT depolama değildir. Okuyan ve çekirdek filtrelerin adlı birden çok depolama hedeflerden veri yazan bir önbellektir. Avere vFXT'ın premium SSD disklerinde depolanan veriler geçici ve sonunda arka uç çekirdek dosyalayıcı depolama temizlenir.

### <a name="which-core-filers-does-avere-vfxt-support"></a>Hangi çekirdek filtrelerin Avere vFXT destekliyor mu?

Genel koşullarını Avere vFXT Azure çekirdek filtrelerin aşağıdaki sistemlerini destekler: 

* Dell EMC Isilon (OneFS 7.1, 7.2, 8.0 ve 8.1) 
* NetApp ONTAP (kümelenmiş modu 9.4 sürümünden, 9.3, 9.2, 9.1P1, 8.3 8.0) ve (7 modu 7.*, 8.3 8.0) 
* Azure Blob kapsayıcıları (yalnızca LRS) 
* AWS S3 demet 
* Google Cloud demetlerine

### <a name="why-doesnt-the-avere-vfxt-support-all-nfs-filers"></a>Neden Avere vFXT tüm NFS filtrelerin desteklemiyor?

Tüm NFS platformlar aynı IETF standartlara uyduğumuzu olsa da, uygulamada her uygulama kendi quirks sahiptir. Bu ayrıntılar Avere vFXT depolama sistemi ile nasıl etkileştiğini etkiler. En yaygın olarak kullanılan platformları Marketi'nde desteklenen sistemleridir.

### <a name="does-avere-vfxt-support-private-object-storage-such-as-swiftstack"></a>Avere vFXT özel nesne depolama (örneğin, Swiftstack) destekliyor mu?

Özel bir nesne depolama Avere vFXT desteklemez.

### <a name="how-can-i-get-a-specific-storage-product-under-support"></a>Belirli depolama ürün destek kapsamında nasıl alabilirim?

Destek, isteğe bağlı olarak alan miktarını dayanır. Belirli bir NAS çözümü desteklemek için yeterli gelir tabanlı istekler varsa, kabul edilir. Azure destek isteklerini olun.

### <a name="can-i-use-azure-blob-storage-as-a-core-filer"></a>Bir çekirdek dosyalayıcı Azure Blob Depolama kullanabilir miyim?

Evet, Azure için Avere vFXT bir blok Blob kapsayıcısı bulut çekirdek dosyalayıcı kullanabilirsiniz.  

### <a name="what-are-the-storage-account-requirements-for-a-blob-core-filer"></a>Bir Blob çekirdek dosyalayıcı için depolama hesabı gereksinimleri nelerdir?

Genel amaçlı v2 depolama hesabınızın olması gerekir (GPv2) hesabı ve yerel olarak yedekli depolama (LRS) yalnızca yapılandırılmış. GRS ve ZRS desteklenmez.

### <a name="can-i-use-archive-blob-storage"></a>Arşiv Blob Depolama kullanabilir miyim?

Hayır. Arşiv depolama SLA'sı ile gerçek zamanlı directory uyumlu değil ve dosya erişimi vFXT sistemi gerekir. 

### <a name="can-i-use-cool-blob-storage"></a>Seyrek erişimli Blob Depolama kullanabilir miyim?

Seyrek erişimli katmana kullanır, ancak işlemlerinin hızıdır. çok daha yüksek olacağını unutmayın. 

### <a name="how-do-i-encrypt-the-blob-container"></a>Blob kapsayıcısını nasıl şifreliyor mu?

Blob şifrelemesini, azure'da (tercih edilen) veya vFXT çekirdek dosyalayıcı düzeyinde ya da yapılandırabilirsiniz.  

### <a name="can-i-use-my-own-encryption-key-for-a-blob-core-filer"></a>Kendi şifreleme anahtarı için bir Blob çekirdek dosyalayıcı kullanabilir miyim?

Varsayılan olarak, veriler Azure BLOB'ları, tabloları, dosyaları ve sıraları için Microsoft yönetilen anahtarlar kullanılarak şifrelenir. Azure BLOB ve dosyalar için şifreleme için kendi anahtarınızı getirebilirsiniz. VFXT şifreleme kullanmayı seçerseniz, Avere oluşturulan anahtarı kullanın ve yerel olarak depolar. 

## <a name="purchasing"></a>Satın alma

### <a name="how-do-i-get-avere-vfxt-for-azure-licensing"></a>Azure lisanslama Avere vFXT nasıl alabilirim?

Azure lisansını Avere vFXT başlama Azure Marketi aracılığıyla kolay bir işlemdir. Bir Azure hesabı için kaydolun ve içindeki yönergeleri izleyin [vFXT kümeyi dağıtmak](avere-vfxt-deploy.md) bir Avere vFXT kümesi oluşturmak için. 

### <a name="how-much-does-the-avere-vfxt-cost"></a>Avere vFXT nin ücreti ne kadardır?

Azure'da Avere vFXT kümelerini kullanarak hiçbir ek lisans ücreti yoktur. Müşteriler, depolama ve diğer Azure kullanım ücretleri için sorumludur.

### <a name="can-avere-vfxt-vms-be-run-as-low-priority"></a>Avere vFXT VM düşük öncelikli olarak çalıştırabilir miyim?

Hayır, Avere vFXT kümeleri "her zaman açık" gerektiren hizmet. Kümeler, gerekli olduğunda kapatılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure için Avere vFXT ile çalışmaya başlamak için planlama ve kendi sisteminizi dağıtma hakkında bilgi edinmek için bu bağlantıları okuyun:

* [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md)
* [Dağıtıma genel bakış](avere-vfxt-deploy-overview.md)
* [Avere vFXT oluşturmak için hazırlık yapma](avere-vfxt-prereqs.md)
* [VFXT kümesi dağıtma](avere-vfxt-deploy.md)

Kullanım örnekleri ve Avere vFXT'ın özellikleri hakkında daha fazla bilgi edinmek için şurayı ziyaret edin [Avere vFXT (Önizleme) için](https://azure.microsoft.com/services/storage/avere-vfxt/).
