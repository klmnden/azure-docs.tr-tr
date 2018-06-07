---
title: Ethereum iş kanıt Konsorsiyumu çözüm şablonu
description: Çoklu üyeli Konsorsiyumu Ethereum ağ yapılandırmak ve dağıtmak Etherereum kanıtı çalışma Konsorsiyumu çözüm şablonu kullanın
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 0ea9bd2c7d23aa227e62858983e7170b771751da
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655470"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Ethereum iş kanıt Konsorsiyumu çözüm şablonu

Ethereum kanıtı çalışma Konsorsiyumu çözüm şablonu, kolay ve hızlı bir çok üye Konsorsiyumu Ethereum ağ ile minimum Azure ve Ethereum bilgi yapılandırmak ve dağıtmak için tasarlanmıştır.

Kullanıcı girişleri ve Azure portalı üzerinden bir tek tıklamalı dağıtım sayıda ile üyelerin kendi ağ kaplama alanını, Microsoft Azure işlem kullanılarak, ağ ve depolama hizmetleri dünya çapında sağlayabilirsiniz. Yük dengeli işlem düğümleri kümesi her üyenin ağ kaplama alanını oluşur, bir uygulama ya da kullanıcı işlemleri, kayıt işlemleri için araştırma düğümleri kümesi ve bir VPN ağ geçidi göndermek için etkileşim kurabilen ile. Bir sonraki bağlantı adımı tam olarak yapılandırılmış birden çok üye blockchain ağ oluşturmak üzere ağ geçitleri bağlanır.

## <a name="about-blockchain"></a>Blockchain hakkında

Bu blockchain topluluğuna yeni size bu çözüm kolay ve yapılandırılabilir bir şekilde Azure üzerinde teknolojisi hakkında bilgi edinmek için harika bir fırsat sürümüdür. Ancak, başlamak için daha basit tek başına Ethereum ağ topolojisi çok üye Konsorsiyumu ağları çıkışı oluşturulmadan önce bu Destekli kılavuza dağıtma öneririz.

### <a name="mining-node-details"></a>Araştırma düğümü ayrıntıları

İşlemler benim düğümleri aynı kaynakları iki eylem rekabet değil emin olmak için işlemleri kabul düğümlerinden ayrılır.

Yönetilen bir diskle bir veya daha fazla araştırma düğümleri içeren en fazla beş bölgeler Konsorsiyumu üyesi sağlayabilirsiniz. Bir veya daha fazla düğüm bölgede ağ düğümlerinin dinamik bulunabilirliği desteklemek için önyükleme düğümü olarak yapılandırılır. Araştırma düğümleri anlaşma için temel dağıtılmış defter durumuna gelmesini diğer araştırma düğümlerle iletişim kurar. Farkında olması veya bu düğümler ile iletişim kurmak, uygulamanız için gerek yoktur. Özel ağlarda Odaklanıldığında, araştırma düğümleri ikincil bir koruma katmanı eklemek için gelen ortak Internet trafiğinden yalıtılır. Giden trafiğe izin verilir, ancak Ethereum bulma bağlantı noktası.

Tüm düğümleri Git Ethereum (Geth) istemci kararlı bir sürümüne sahip ve araştırma düğümleri olarak yapılandırılmış. Bir özel genesis bloğu sağlamadı, tüm düğümler aynı Ethereum adresini ve Ethereum hesabı parola korumalı anahtar çifti kullanın. Sağladığınız Ethereum parola her araştırma düğüm için varsayılan hesap (coinbase) oluşturmak için kullanılır. Araştırma düğümleri olarak benim, bu hesaba eklediğiniz ücretlerini toplar.

Araştırma Konsorsiyumu üye başına en fazla düğüm sayısı her üyesine ayrılmış karma gücü miktarına ve istenen ağ genel boyutuna bağlıdır. Daha büyük ağ, haksız avantajlı kazanmak için tehlikeye gerekiyorsa daha fazla düğüm. Şablonu, sanal makine ölçek kümeleri kullanılarak sağlanan bölge başına en fazla 15 araştırma düğüm destekler.

### <a name="transaction-node-details"></a>İşlem düğümü ayrıntıları

Konsorsiyumu üyesi aynı zamanda yük dengeli işlem düğümleri kümesi vardır. Uygulamalar bu düğümler işlemleri göndermek ya da akıllı sözleşmeleri blockchain ağından çalıştırılmasında kullanabilmesi için bu düğümler, sanal ağ dışında erişilebilir. Tüm düğümleri Git Ethereum (Geth) istemci kararlı bir sürümüne sahip ve dağıtılmış defter tam bir kopyasını korumak için yapılandırılır. Bir özel genesis bloğu sağlanmazsa, bu düğümler Ethereum hesabı parola korumalı aynı Ethereum hesabı kullanın.

İşlem düğümleri, yük dengeli yüksek kullanılabilirliği sürdürmek üzere bir kullanılabilirlik kümesi içinde. Şablonu, sanal makine ölçek kümeleri kullanılarak sağlanan en fazla beş işlem düğümleri destekler.

### <a name="log-analytics-details"></a>Günlük analizi ayrıntıları

Her dağıtım ayrıca yeni bir günlük analizi örneği oluşturur veya var olan bir örneği katılabilirsiniz. Bu, çeşitli performans ölçümleri, dağıtılan ağı yapar her bir sanal makinenin izlenmesini sağlar.

## <a name="deployment-architecture"></a>Dağıtım mimarisi

### <a name="description"></a>Açıklama

Bu çözüm şablonu tek veya birden çok bölgede tabanlı çoklu üye Ethereum Konsorsiyumu ağ dağıtabilirsiniz. Her bölgenin sanal ağ bağlantı kaynaklarına ve sanal ağ geçitleri kullanarak zinciri topolojisinde diğer bölge bağlı. Ayrıca her bölgeye dağıtılmış tüm araştırıcısı ve işlem düğümlerinin gerekli bilgileri içeren bir kayıt sağlamasını yapar.

### <a name="standalone-and-consortium-leader-overview"></a>Tek başına ve Konsorsiyumu öncü genel bakış

![Tek başına ve Konsorsiyumu öncü genel bakış](./media/ethereum-deployment-guide/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>Katılma Konsorsiyumu üye genel bakış

![Katılma Konsorsiyumu üye genel bakış](./media/ethereum-deployment-guide/member-overview.png)

## <a name="getting-started"></a>Başlarken

Bu işlem birkaç sanal makine ölçek kümeleri dağıtma destekleyebilir ve diskleri yönetilen bir Azure aboneliği gerektirir. Gerekirse, başlamak için ücretsiz bir Azure hesabı oluşturun.

Bir abonelik güvenli hale getirildikten sonra Azure portalına gidin. Seçin **+ kaynak oluşturma**, Market (bkz. tüm), arayın ve **Ethereum kanıtı çalışma Konsorsiyumu**.

Şablon dağıtımı ilk üyenin ayak izini ağ yapılandırması için size yol gösterir. Dağıtım akışı beş adımı ayrılmıştır: temel bilgileri, Operations Management Suite, dağıtım bölgeler, ağ boyutu ve performans, Ethereum ayarlar.

### <a name="basics"></a>Temel Bilgiler

Altında **Temelleri**, abonelik, kaynak grubu ve temel sanal makine özellikleri gibi herhangi bir dağıtım için standart parametrelerin değerlerini belirtin.

![Temel Bilgiler](./media/ethereum-deployment-guide/sample-deployment.png)

Parametre Adı|Açıklama| İzin Verilen Değerler|Varsayılan değerler
---|---|---|---
Bir yeni veya mevcut birleşim ağı oluşturulsun mu?|Yeni bir ağ oluşturun veya varolan bir Konsorsiyumu ağa|Yeni birleştirme varolan oluşturma|Yeni Oluştur
Konsorsiyumu parçası olacak bir ağ dağıtmak?|Bu ağa bağlanma gelecekteki dağıtımlar Konsorsiyumu ağ verir (görünür *Yeni Oluştur* Yukarıda seçilen)|Tek başına Konsorsiyumu|Tek Başına
Kaynak öneki |Dize kaynakları (2 ila 4 alfasayısal karakterler) adlandırma için temel olarak kullanılır. Kaynak özgü bilgiler eklenir ancak benzersiz bir karması için bazı kaynaklar dizesine eklenir.|Alfasayısal karakter uzunluğu 2 ile 4 ile|NA
VM kullanıcı adı| Dağıtılan her VM'in (yalnızca alfasayısal karakterler) yönetici kullanıcı adı|1 - 64 karakter |gethadmin
Kimlik doğrulaması türü|Sanal makine kimliğini doğrulamak için yöntem. |Parola veya SSH ortak anahtarı|Parola
Parola (kimlik doğrulama türü = parola)|Dağıtılan sanal makinelerin her biri için yönetici hesabı için parola. Parola aşağıdaki gereksinimleri 3 içermelidir: 1 büyük harf, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm sanal makineleri aynı parolayı başlangıçta olmakla birlikte, sağlama işlemi sonrası parolasını değiştirebilirsiniz.|12 - 72 karakter|NA
SSH anahtarı (kimlik doğrulama türü = ortak anahtar)|Uzak oturum açma için kullanılan güvenli Kabuk anahtarı.|| NA
Abonelik| Abonelik Konsorsiyumu ağ dağıtılacağı||NA
Kaynak Grubu| Kaynak grubu Konsorsiyumu ağ dağıtılacağı.||NA
Konum| Kaynak grubu için Azure bölgesi. ||NA



### <a name="operations-management-suite"></a>Operations Management Suite

Operations Management Suite (OMS) dikey penceresinde, ağınız için bir OMS kaynağı yapılandırmanıza olanak sağlar. OMS toplar ve hızlı bir şekilde ağ durumu ya da hata ayıklama denetleme olanağı sağlayarak yüzey yararlı Ölçümler ve ağınızdan günlükleri, sorunlar. Kapasite ulaşıldığında OMS ücretsiz sunulması düzgün biçimde başarısız olur.

![Yeni OMS oluşturma](./media/ethereum-deployment-guide/new-oms.png)

Parametre Adı|Açıklama| İzin Verilen Değerler|Varsayılan değerler
---|---|---|---
Varolan OMS Bağlan|Yeni bir günlük analizi örneği oluşturun veya var olan bir örneği katılın|Yeni birleştirme varolan oluşturma|Yeni günlük analizi konumu oluşturun|Yeni günlük analizi dağıtılacağı bölge (görünür IF *Yeni Oluştur* seçili)
Varolan OMS çalışma alanı kimliği|Var olan örneğinin çalışma alanı kimliği (görünür IF *katılma varolan* seçilidir) OMS hizmet katmanı|Yeni örnek için fiyatlandırma katmanı seçin. Konumundaki daha fazla bilgi https://azure.microsoft.com/pricing/details/log-analytics/ (görünür IF *katılma varolan* seçili)|Düğüm başına ücretsiz tek başına|Ücretsiz
Varolan OMS birincil anahtar|Varolan OMS örneğine bağlanmak için kullanılan birincil anahtarı (görünür IF *katılma varolan* seçili)

### <a name="deployment-regions"></a>Dağıtım bölgeleri

Sonraki altında **dağıtım bölgeleri**, girdileri belirtin **bilgiler sayısı** Konsorsiyumu ağ ve verilen bölge sayısına göre Azure bölgelerinin seçimi dağıtmak için. Kullanıcı en fazla beş bölgeler dağıtabilirsiniz.

![Dağıtım bölgeleri](./media/ethereum-deployment-guide/deployment-regions.png)

Parametre Adı| Açıklama| İzin Verilen Değerler |Varsayılan değerler
---|---|---|---
Bilgiler sayısı| Konsorsiyumu ağ dağıtmak için bölge sayısı|1, 2, 3, 4, 5| 2
İlk bölge| İlk bölgeye Konsorsiyumu ağ dağıtmak için|Tüm Azure bölgeleri izin| Batı ABD
İkinci bölge |(Yalnızca bölgeleri sayısını 2 olarak seçildiğinde görünür) Konsorsiyumu ağ dağıtmak için ikinci bölge|Tüm Azure bölgeleri izin| Doğu ABD
Üçüncü bölge| (Yalnızca bölgeleri sayısı 3 olarak seçildiğinde görünür) Konsorsiyumu ağ dağıtımı için üçüncü bölge|Tüm Azure bölgeleri izin| Orta ABD
Dördüncü bölge| Dördüncü bölge Konsorsiyumu ağ (yalnızca bölgeleri sayısı 4 seçildiğinde görünür) dağıtmak için|Tüm Azure bölgeleri izin| Doğu ABD 2
Beşinci bölge| Beşinci bölge Konsorsiyumu ağ (yalnızca bölgeleri sayısı 5 seçildiğinde görünür) dağıtmak için|Tüm Azure bölgeleri izin| Batı ABD 2

### <a name="network-size-and-performance"></a>Ağ boyutu ve performans

Sonraki altında **ağ boyutu ve performans** girişleri sayısını ve boyutunu araştırma düğümleri ve işlem düğümleri gibi Konsorsiyumu ağ boyutunu belirtin.

![Ağ boyutu ve performans](./media/ethereum-deployment-guide/network-size-performance.png)

Parametre Adı |Açıklama |İzin Verilen Değerler| Varsayılan değerler
---|---|---|---
Araştırma düğüm sayısı|Bölge başına dağıtılan araştırma düğüm sayısı|2 - 15| 2
Araştırma düğümü depolama performansı|Yönetilen disk dağıtılan araştırma düğümlerin her birinde yedekleme türü.|Standart veya Premium|Standart
Araştırma düğüm sanal makine boyutu|Araştırma düğümleri için kullanılan sanal makine boyutu.|Standart bir <br />Standart D <br />Standart D v2 <br />Standart F serisi <br />Standart DS <br />ve standart FS|Standart D1v2
Yük dengeli işlem düğüm sayısı|Ağın bir parçası olarak sağlamak için işlem düğüm sayısı.|1 - 5| 2
İşlem düğümü depolama performansı|Dağıtılmış işlem düğümlerinin her biri yedekleme yönetilen disk türü.|Standart veya Premium|Standart
İşlem düğümü sanal makine boyutu|İşlem düğümleri için kullanılan sanal makine boyutu.|Standart bir <br />Standart D <br />Standart D v2 <br />Standart F serisi <br />Standart DS <br />ve standart FS|Standart D1v2

### <a name="ethereum-settings"></a>Ethereum ayarları

Sonraki altında **Ethereum ayarları**, ağ kimliği ve Ethereum hesap parolası veya genesis bloğu gibi Ethereum ilgili yapılandırma ayarlarını belirtin.

![Ethereum ayarları](./media/ethereum-deployment-guide/standalone-leader-deployment.png)

Parametre Adı |Açıklama |İzin Verilen Değerler|Varsayılan değerler
---|---|---|---
ConsortiumMember kimliği|Çakışma önlemek için IP adresi alanları yapılandırmak için kullanılan Konsorsiyumu ağa katılan her üye ile ilişkili kimliği. <br /><br />Üye kimliği aynı ağda farklı kuruluşlar arasında benzersiz olmalıdır. Bile aynı kuruluş için birden çok bölgeye dağıtırken benzersiz üye kimliği gereklidir.<br /><br />Katılan diğer üyeleriyle paylaşmak gerekli olduğundan bu parametrenin değerini not edin.|0 - 255
Ethereum ağ kimliği|Dağıtılan Konsorsiyumu Ethereum ağ için ağ kimliği. Her Ethereum ağ 1 ortak ağ kimliği olan ile kendi ağ kimliği vardır. Ağ erişimi için araştırma düğümleri kısıtlı olsa da, çakışmaları önlemek için çok sayıda kullanmayı hala öneririz.|5 - 999,999,999| 10101010
Özel genesis bloğu|Otomatik olarak bir genesis bloğu oluşturmak veya özel bir sağlamak için seçeneği.|Evet/Hayır| Hayır
Ethereum hesabı parolasını (özel genesis blok = Hayır)|Her düğümüne içeri Ethereum hesabını güvenli hale getirmek için kullanılan yönetici parolası. Parola şunları içermelidir: 1 büyük harf, 1 küçük harf ve 1 sayı.|12 veya daha fazla karakter|NA
Ethereum özel anahtar parolası (özel genesis blok = Hayır)|Oluşturulan varsayılan Ethereum hesabıyla ilişkili ECC özel anahtarı oluşturmak için kullanılan parola. Önceden oluşturulmuş bir özel anahtarı açıkça geçirilmesi gerekmez.<br /><br />Bir parola güçlü bir özel anahtar ve diğer Konsorsiyumu üyeleriyle örtüşmez emin olmak için yeterli rastgele göz önünde bulundurun. Parola en az şunları içermelidir: 1 büyük harf, 1 küçük harf ve 1 sayı.<br /><br />İki üye aynı parolayı, oluşturulan hesapların Not aynı olacaktır. Tek bir kurumun bölgeler arasında dağıtmak çalışıyor ve tüm düğümlere tek bir hesap (para temel) paylaşmak isterse aynı parolayı yararlıdır.|12 veya daha fazla karakter|NA
Genesis engelle (özel genesis blok = Yes)|Özel genesis bloğu temsil eden JSON dize. Özel ağlar altında genesis bloğun biçimi hakkında daha fazla ayrıntı bulabilirsiniz.<br /><br />Bir Ethereum hesap hala özel genesis blok sağlanırken oluşturulur. Araştırma için bekleme için genesis bloktaki bir prefunded Ethereum hesabı belirtme göz önünde bulundurun.|Geçerli bir JSON |NA
Paylaşılan anahtar bağlantı|SANAL ağ geçitleri arasında bir bağlantı için bir paylaşılan anahtar.| 12 veya daha fazla karakter|NA
Konsorsiyumu veri URL'si|Başka bir üyenin dağıtım tarafından sağlanan ilgili Konsorsiyumu yapılandırma verilerini işaret eden URL. <br /><br />Bu bilgiler bir dağıtıma sahip zaten bağlı bir üyesi tarafından sağlanır. Ağın geri kalanı dağıttıysanız KONSORSİYUMU veri adlı şablon dağıtımı çıktısı URL'dir.||NA
VNet bağlanmak için ağ geçidi|Sanal ağ geçidine bağlanmak için kaynak yolu.<br />Bu bilgiler bir dağıtıma sahip zaten bağlı bir üyesi tarafından sağlanır. Ağın geri kalanı dağıttıysanız, şablon dağıtımı çıktısı CONSORTIUM_MEMBER_GATEWAY_ID adlı URL'dir. Not: URL ve sanal ağ geçidi aynı üyenin Konsorsiyumu veri kaynağı kullanılması gerekir.||NA
Eş bilgi kayıt uç noktası|Başka bir üyenin dağıtım tarafından sağlanan eş bilgisi bitiş noktası|Geçerli uç nokta Konsorsiyumu ilk üyesi|NA
Anahtar, eş bilgi kayıt|Eş Info birincil anahtarı başka bir üyenin dağıtım tarafından sağlanan|Geçerli birincil anahtar Konsorsiyumu ilk üyesi|NA

### <a name="summary"></a>Özet

Özet dikey belirtilen girişleri gözden geçirmek ve temel dağıtım öncesi doğrulama çalıştırmak için tıklayın.

![Özet](./media/ethereum-deployment-guide/summary.png)

Yasal ve gizlilik koşullarını gözden geçirin ve tıklatın **satın alma** dağıtmak için. Ardından dağıtım birden fazla bölgeye veya Konsorsiyumu ağ için varsa, bu şablonu önceden diğer üyeleriyle ağ bağlantısı desteklemek için gerekli VPN ağ geçitleri dağıtır. Ağ geçidi dağıtımına kadar 45 50 dakika sürebilir.

## <a name="post-deployment-sanity-checks"></a>POST dağıtım sağlamlık denetimleri

### <a name="administrator-page"></a>Yönetici sayfası

Dağıtım başarıyla tamamlandı ve tüm kaynakları sağlanan sonra blockchain ağınızdaki bir görünümünü almak için yönetici sayfasına gidebilirsiniz ve sağlamlık dağıtım durumunu denetleyin. Yönetim sayfasının URL'sini yük dengeleyici DNS adıdır; ADMIN_SITE adlandırılan şablon dağıtımı çıktı bölümünde yok.

Bulmak için dağıtılan kaynak grubu seçin. Ardından seçin **genel bakış**, bağlantıyı tıklatıp hemen altında **dağıtımları** başarılı sayısını gösterir.

![Kaynak grubu genel bakış](./media/ethereum-deployment-guide/resource-overview.png)

Yeni ekran dağıtım geçmişini gösterir. İlk dağıtım kaynağı (örneğin, microsoft-azure-blockchain.azure-blockchain-Hi...) seçin ve Ara **çıkışları** sayfanın alt kısmında bölümünde. Şablon dağıtımı çıkış parametresi ADMIN_SITE olarak listelenen yönetim sayfasının URL'sini görürsünüz.

![Kaynak dağıtımları](./media/ethereum-deployment-guide/resource-deployments.png)

![Dağıtım çıktı](./media/ethereum-deployment-guide/deployment-output.png)

Yönetim sayfasına ulaşmak için kopyalama **yönetici sitesi** çıkış ve başka bir sekmede aç.

Yönetim sayfasında Ethereum düğüm durumu bölümü gözden geçirerek dağıtılmış topoloji üst düzey bir genel bakış elde edebilirsiniz. Tüm düğüm ana bilgisayar adları, bunların eş sayısını ve görülen son blok içerir. Her düğüm için eş sayısı en az biri arasında küçük *toplam düğüm sayısı* ve yapılandırılmış en fazla eş sayısı. Not, eş sayısı ağ içinde dağıtılabilir düğüm sayısını kısıtlamaz.
Bazen, dalgalanma ve (sayısı toplam düğüm - 1) değerinden küçük olması eş sayısı görürsünüz. Sayı fark, her zaman defterindeki çatallarını eş sayıma küçük değişiklikler neden bu yana düğümlerin sağlıksız, bir oturum değil. Son olarak, sistemde çatallarını veya gecikmelere belirlemek için ağdaki her bir düğüm tarafından görülen en son blok inceleyebilirsiniz.

![Düğüm durumu](./media/ethereum-deployment-guide/node-status.png)

Düğüm durumu 10 dakikada bir yenilenir. Tarayıcı aracılığıyla sayfayı yeniden yükleyin veya **yeniden** Görünümü güncelleştirmek için düğmesi.

### <a name="oms-portal"></a>OMS portalı

Dağıtım çıkış (OMSPORTALURL) bağlantıyı izleyerek veya OMS kaynak dağıtılan kaynak grubunuzdaki seçerek, OMS portalında bulabilirsiniz.

![OMS URL'Sİ](./media/ethereum-deployment-guide/oms-url.png)

![OMS kaynak](./media/ethereum-deployment-guide/oms-resource.png)

Portal önce üst düzey ağ istatistikleri genel bir bakış görüntüler.

![OMS genel bakış](./media/ethereum-deployment-guide/oms-overview.png)

Bir genel bakış tıklatarak düğüm başına istatistiklerini görüntülemek için bir portalına yönlendirir.

![Düğüm başına istatistikleri](./media/ethereum-deployment-guide/per-node-stats.png)

### <a name="accessing-nodes"></a>Düğümlere erişme

İle sağlanan yönetici kullanıcı adı ve parola/SSH anahtarı SSH aracılığıyla işlem düğümleri için sanal makineleri uzaktan bağlanabilir. İşlem düğümü VM'ler değil sahip olduğundan, kendi ortak IP adresleri, yük dengeleyici üzerinden gidin ve bağlantı noktası numarasını belirtmeniz gerekir. İlk işlem düğümü erişmek için çalıştırılacak SSH komutu şablon dağıtımı çıktı parametresi olarak listelenen **SSH_TO_FIRST_TX_NODE** (örnek dağıtımı: ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com). Ek işlem düğümlerine almak için bağlantı noktası numarası bir artırmasına (örneğin, ilk işlem düğümü üzerindeki 4000 bağlantı noktasıdır).

Araştırma düğümleri çalıştığı sanal makineler dışarıdan erişilebilir olmayan olduğundan, işlem düğümlerinden biri gitmeniz gerekir. Bir SSH oturumu işlem düğüme sahip olduğunda, bir işlem düğümüne özel anahtarınızı yükleyin veya araştırma düğümleri hiçbirine bir SSH oturumu başlatmak için parolanızı kullanın.

**Not**

Ana bilgisayar adları yönetim sitesinden veya Azure portalından elde edilebilir. Azure Portalı'nda, sanal makine ölçek kümesi (VMSS) kaynak mevcut düğümlerinin ana bilgisayar adı altında listelenen **örnekleri**, gerçek ana bilgisayar adları farklıdır. Örneğin, Azure portalında hostname neye benzediğini **mn asdfmv reg1_0** ancak gerçek ana bilgisayar adı gibi olacaktır **mn asdfmv reg**.

Örneğin:

Azure portal ana bilgisayar adı| Gerçek ana bilgisayar adı
---|---
MN ethwvu reg1_0| MN ethwvu reg1000000
MN ethwvu reg1_1 |MN ethwvu reg1000001
MN ethwvu reg1_2 |MN ethwvu reg1000002

## <a name="adding-a-new-consortium-member"></a>Yeni Konsorsiyumu üye ekleme

### <a name="sharing-data"></a>Veri paylaşımı

İlk üyeden (veya bağlı üyesi) Konsorsiyumu, birleştirme ve bunların bağlantısı kurmak için diğer üyeleri birkaç parça bilgi sağlamanız gerekir. Bu avantajlar şunlardır:

1. **Konsorsiyumu yapılandırma verilerini paylaşılan**: iki üyeler arasında Ethereum bağlantı düzenlemek için kullanılan veri kümesi yok. Genesis bloğu, Konsorsiyumu ağ kimliği ve önyükleme düğümleri de dahil olmak üzere gerekli bilgileri Öncü veya başka bir dağıtılan üye işlem düğümlerinin bir dosyaya yazılır. Bu dosyanın konumunu adlı şablon dağıtımı çıkış parametresinde listelenen **KONSORSİYUMU veri**.
2. **Bilgi endpoint eş**: tüm düğümlerin zaten bilgi almak için eş bilgisi Kaydedici endpoint kılavuzları veya başka bir üyenin dağıtım Ethereum ağına bağlı. Her düğüm ile ilgili bilgileri kümesi bir DB depoları düğümün ana bilgisayar adı, özel gibi bilgileri ağdaki IP adresi vb. bağlı. Bu adlı şablon dağıtımı çıktı parametresi olduğu **PEER_INFO_ENDPOINT**.
3. **Eş Info birincil anahtarı**: Eş Info Kaydedici birincil anahtarı öncü 's veya diğer üyenin eş Info birincil anahtarı erişmek için kullanılır. Bu adlı şablon dağıtımı çıktı parametresi olduğu **PEER_INFO_PRIMARY_KEY**.


4. **SANAL ağ geçidi**: her üye olan bir üye aracılığıyla tüm blockchain ağa bir bağlantı kurar. VNET bağlanmak için sanal ağ geçidi bağlanmakta olduğunuz üye için kaynak yoluna gerekir. Bu adlı şablon dağıtımı çıktı parametresi olduğu **CONSORTIUM_MEMBER_GATEWAY_ID**.
5. **Paylaşılan anahtar**: A önceden belirlenmiş bir bağlantı kurarak iki Konsorsiyumu ağ üyeleri arasında gizli. Dağıtım bağlamı dışında üzerinde anlaşılan alfasayısal bir dize (1 ila 128 karakter arasında) budur. (Örneğin, **MySharedKeyAbc123**)

### <a name="acceptance-of-new-member"></a>Yeni üye kabulü

Katılma üye ağlarındaki başarıyla dağıtıldıktan sonra bu adımın yapılması gerekir. Var olan bir üye üyesi ağa ve işlem trafiği görmek için önce son yapılandırma kendi VPN bağlantısı kabul etmek için ağ geçidinde gerçekleştirmeniz gerekir. Başka bir deyişle, bağlantı kurulana kadar katılma üye Ethereum düğümlerinin çalışmaz. Bu yapılandırma, PowerShell veya xPlat CLI yapılabilir. Bir PowerShell modülü ve xPlat CLI betiği ayrıca üzerinde depolanan Konsorsiyumu verilerin yanında işlem düğümü. Betik konumuna adlı dağıtım çıkış parametreleri olan **ağ geçidi PS MODÜL çifti** ve **çifti-GATEWAY-AZURE-CLISCRIPT**sırasıyla.

**PowerShell/CLI Kurulumu**

Azure PowerShell cmdlet'leri ve Azure xPlat kullanmaya başlama hakkında ek bilgi CLI Azure belgelerinde bulunabilir.

En son sürümünü yerel olarak yüklenmiş Azure cmdlet'lerini ve bir oturum açma gerekir. Azure aboneliği kimlik bilgilerinizle oturum içine oturum emin olun.

**PowerShell: Bağlantı**

PowerShell modülünü indirin ve yerel olarak saklanır. PowerShell modülü konumu olarak belirtilen **ağ geçidi PS MODÜL çifti** şablon dağıtımı çıktı parametresi.

Önceden etkinleştirilmemişse kullanmak **Set-ExecutionPolicy** cmdlet'i imzasız modülü çalıştırmaya izin vermek yerel oturum için.

**Set-ExecutionPolicy Unrestricted Currentuser'a**

Öncü dağıtım kullanılarak dağıtılan Azure aboneliğiniz için daha sonra oturum açma

**Login-AzureRmAccount**

Ardından, modülü içeri aktarın:

**Import-Module <filepath to downloaded file>**

Son olarak, uygun girişle işlevi çalıştırın:

- **MyGatewayResourceId** : ağ geçidinizin kaynak yolu. Bu adlı şablon dağıtımı çıktı parametresi olduğu **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId** : katılma üyenin ağ geçidi kaynak yolu. Bu birleşme üyesi tarafından sağlanır ve şablon dağıtımı çıktı parametresi olarak da adlandırılır **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName** : Bu ağ geçidi bağlantıyı tanımlamak bir ad.
- **Paylaşılan anahtar** : önceden oluşturulmuş gizli bir bağlantı kurarak iki Konsorsiyumu ağ üyeleri arasında.

**CreateConnection** -MyGatewayResourceId <resource path of your Gateway> - OtherGatewayResourceId < katılma üyenin ağ geçidi kaynak yolu > - ConnectionName myConnection - SharedKey "MySharedKeyAbc123"

**xPlat CLI: bağlantı**

Azure CLI komut dosyasını indirin ve yerel olarak saklanır. Azure CLI komut dosyasının konumunu adlı şablon dağıtım parametresinde belirtilen **çifti-GATEWAY-AZURE-CLI-BETİĞİ**.

Komut dosyası, uygun girişle çalıştırın:

- **MyGatewayResourceId** : ağ geçidinizin kaynak yolu. Bu adlı şablon dağıtımı çıktı parametresi olduğu **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId** : katılma üyenin ağ geçidi kaynak yolu. Bu birleşme üyesi tarafından sağlanır ve şablon dağıtım parametresi olarak da adlandırılan bunların dağıtım **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName** : Bu ağ geçidi bağlantıyı tanımlamak bir ad.
- **Paylaşılan anahtar** : önceden oluşturulmuş gizli bir bağlantı kurarak iki Konsorsiyumu ağ üyeleri arasında.
- **Konum** : ağ geçidi kaynağı dağıtıldığı Azure bölgesi.

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

Mimari arasındaki bağlantı başarıyla yapılandırdıktan sonra şu şekilde olacaktır **öncü** ve **üye** dağıtımları.

![Öncüsü ve üye mimarisi](./media/ethereum-deployment-guide/leader-member.png)

## <a name="fund-new-member-account"></a>Yeni üye Yen.

### <a name="generated-genesis-block"></a>Oluşturulan genesis bloğu

Dağıtım genesis blok oluşturursa ilk üye olarak Ethereum hesabınız ile bir trilyon ether finanse (özel Genesis blok = Hayır). Diğer üyeleri, önceden finanse değil ve araştırma düğümlerini blokları benim başladığınızda Ether birikmesine beklemesi gereken bir hesap gerekir. Yeni üyeler için Ether beklemek zorunda kalmamak için açıkça katılma üye Ethereum hesaplarını Bağış gerekecektir.

Bunu yapmak için üyeleri birleştirme, kendi Yönetim Web sitesindeki görüntülenen Ethereum hesabıyla sağlamanız gerekir. Ardından yönetici Web sitenizi Ether hesabınızdan hesaplarında sağlanan hesap girerek aktarmak için kullanabilirsiniz.

### <a name="custom-genesis-block"></a>Özel genesis bloğu

Bir özel genesis bloğu belirtilen Ethereum hesabıyla sağlanırsa, MetaMask kullanabilir veya ether değerinden aktarmak için başka bir araç belirtilen hesaba önceden oluşturulmuş Ethereum hesabı yönetici Web sitesi görünür. MetaMask kullanma hakkında daha fazla yönerge için bkz: [Ethereum hesabı oluşturma](#create-ethereum-account).

Bir özel genesis bloğu bir hesabı sağlanır veya önceden ayrılmış bir hesaplarına erişim yok Ether hesabınızda (para temel) oluşturmak için benim araştırma düğümleriniz başlayana kadar beklemeniz gerekir. Ne kadar hızlı fon oluşturulan özel genesis bloğunda belirttiğiniz zorluk düzeyini bağlıdır.

## <a name="create-ethereum-account"></a>Ethereum hesabı oluşturma

Ek hesap oluşturmak için çeşitli çözümler kullanabilirsiniz. Bu tür bir çözüm MetaMask, kimlik kasası ve ortak bir Ethereum ağ bağlantısı, test ya da özel sağlayan bir Chrome uzantısı olur. MetaMask ağ hesabını kaydetmek için bir işlem formulates.

Bu işlem, diğer herhangi bir işlem gibi işlem düğümleri birine gidin ve aşağıda gösterildiği gibi bir bloğuna sonunda tıklatmaktansa.

![İşlem düğümü](./media/ethereum-deployment-guide/transaction-node.png)

Chrome'uzantıyı yüklemek için Özelleştir gidin ve Google Chrome (taşma düğmesi) Kontrol > Diğer Araçlar > uzantılar > fazla alma uzantıları ve MetaMask arayın.

![MetaMask uzantısı](./media/ethereum-deployment-guide/metamask-extension.png)

Bir kez yüklendikten sonra MetaMask açın ve yeni bir kasa oluşturun. Varsayılan olarak, kasaya Morden Test ağa bağlanır. Bu işlem düğümleri önünde yük dengeleyici için özel olarak dağıtılan özel Konsorsiyumu ağını bağlamak için değiştirmeniz gerekecektir. Şablon çıktısı olarak adlı gösterilen Ethereum RPC uç nokta bağlantı noktası 8545, almak `ETHEREUM-RPC-ENDPOINT`, aşağıda gösterildiği gibi özel RPC girin.

![MetaMask ayarları](./media/ethereum-deployment-guide/metamask-settings.png)

Kasa oluşturarak, bir hesap içeren Cüzdan oluşturun. Ek hesapları oluşturmak için seçin **anahtar hesapları** ve ardından **+** düğmesi.

![MetaMask hesabı oluşturma](./media/ethereum-deployment-guide/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>İlk Ether ayırma başlatmak

Yönetici sayfası Ether önceden ayrılmış hesabından başka bir Ethereum hesabına aktarmak için bir işlem formüle. Bu Ether aktarım işlem düğüme gönderilen ve aşağıda gösterildiği gibi bir bloğuna tıklatmaktansa bir işlemdir.

![İşlem düğümü](./media/ethereum-deployment-guide/transaction-node-mined.png)

Pano simgesine MetaMask Cüzdan ether aktarım ve yönetici sayfasına geri dönmek istediğiniz Ethereum hesabının adresini kopyalayın. Yeni oluşturulan hesabınıza önceden ayrılmış Ethereum hesabından 1000 ether aktarmak için giriş alanını kopyalanmış hesabı yapıştırın. Tıklatın **gönderme** ve işlem bir bloğuna tıklatmaktansa bekleyin.

![Düğüm durumu](./media/ethereum-deployment-guide/node-status2.png)

İşlem mined bloğu içine gerçekleştirilir sonra MetaMask hesabınız için hesap bakiyesi 1000 Ether aktarımını yansıtır.

![MetaMask aktarımı](./media/ethereum-deployment-guide/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>Hesapları arasında Ether aktarımı

Bu noktada, özel Konsorsiyumu ağınızdaki işlemleri yürütmek hazır olursunuz. Basit bir hesaptan Ether aktarmak için bir işlemdir. Böyle bir işlem formüle için MetaMask bir kez daha, yukarıda ikinci bir hesap için kullanılan ilk hesap para transfer kullanabilirsiniz.

Gelen **Cüzdan 1** MetaMask içinde Gönder'i tıklatın. Oluşturulan ikinci Cüzdan adresini kopyalayın **alıcı adresi** giriş alanını ve içinde aktarmak için Ether miktarını **tutar** giriş alanı. Tıklatın **Gönder** ve işlem kabul edin.

![MetaMask gönderme işlemi](./media/ethereum-deployment-guide/metamask-send.png)

Bir kez daha, işlem tıklatmaktansa ve bir bloğuna kaydedilen hesap bakiyelerini buna uygun olarak yansıtılır. Not **Cüzdan 1**ait bakiye kesilen işlem işlemek için bir araştırma ücret ödemeniz gerekiyordu bu yana 15'ten fazla Ether.

![Cüzdan 1](./media/ethereum-deployment-guide/wallet1.png)

![Cüzdan 2](./media/ethereum-deployment-guide/wallet2.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık uygulama ve akıllı sözleşme geliştirme özel Konsorsiyumu blockchain ağınıza karşı odaklanmak hazırsınız.
