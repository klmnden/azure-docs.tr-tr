---
title: Ethereum iş kavram consortium çözüm şablonu
description: Çoklu üyeli bir konsorsiyum Ethereum ağ yapılandırmak ve dağıtmak Etherereum kavram iş Consortium çözüm şablonu kullanın
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/29/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: fa58ecf4607efc1d212e40b98d199756d4b987f8
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50231806"
---
# <a name="ethereum-proof-of-work-consortium-solution-template"></a>Ethereum iş kavram consortium çözüm şablonu

Ethereum kavram iş Consortium çözüm şablonu, daha kolay ve hızlı dağıtma ve Azure ve Ethereum minimum bilgi ile çok üye consortium Ethereum ağ yapılandırma olmak için tasarlanmıştır.

Birkaç kullanıcı girişleri ve Azure Portalı aracılığıyla tek tıklamayla dağıtım ile üyelerin kendi ağ kaplama alanını kullanarak Microsoft Azure işlem, ağ ve depolama hizmetleri dünya çapında sağlayabilirsiniz. Her üyenin ağ kaplama alanını yük dengeli işlem düğümlerinin bir dizi oluşur. sahip olan bir uygulama ya da kullanıcı işlemleri, bir dizi araştırma düğümü kayıt işlemleri ve VPN ağ geçidi göndermek için etkileşim kurabilirsiniz. Bir sonraki bağlantı adımı tam olarak yapılandırılmış birden çok üye blockchain ağ oluşturmak için ağ geçitleri bağlanır.

## <a name="about-blockchain"></a>Blockchain hakkında

Bu blok zinciri topluluğuna Acemi, bu çözüm kolay ve yapılandırılabilir bir şekilde azure'da teknolojisi hakkında bilgi edinmek için harika bir fırsat sürümüdür. Ancak, başlamak için önce çoklu üyeli bir konsorsiyum ağlar oluşturmaya destekli sunduğumuz daha basit tek başına Ethereum ağ topolojisi dağıtma öneririz.

### <a name="mining-node-details"></a>Araştırma düğüm ayrıntıları

İşlemler benim düğümleri aynı kaynakları iki eylem rekabet değil emin olmak için işlemleri kabul düğümlerden ayrılır.

En fazla beş bölgede yönetilen diskle, bir veya daha fazla araştırma düğümleri içeren bir konsorsiyum üye sağlayabilirsiniz. Bölgede bir veya daha fazla düğüm ağ düğümleri dinamik bulunabilirliğini desteklemek için önyükleme düğümü olarak yapılandırılır. Araştırma düğümleri, fikir birliğine varılmış için temel alınan dağıtılmış kayıt defteri durumuna gelmesini diğer araştırma düğümleri ile iletişim kurar. Uygulamanız farkında olmanız ya da bu düğümler ile iletişim kurmak için gerek yoktur. Özel ağlarda odaklanarak araştırma düğümleri ikincil bir koruma katmanı eklemek için gelen genel internet trafiğini yalıtılmış durumdadır. Giden trafiğe izin verilir, ancak Ethereum bulma bağlantı noktası.

Tüm düğümleri Ethereum gidin (Geth) istemcinin kararlı bir sürüm olması ve araştırma düğümleri olacak şekilde yapılandırılır. Özel genesis blok sağlamadı, tüm düğümler aynı Ethereum adresini ve Ethereum hesabı parola korumalı anahtar çifti kullanın. Sağladığınız Ethereum parola her araştırma düğüm için varsayılan hesap (coinbase) oluşturmak için kullanılır. Araştırma düğümleri olarak Madencilik, bunlar bu hesaba eklenen ücreti tahsil.

Toplam boyut her üyesine atanmış karma gücü miktarına ve istenen ağ consortium üyesi başına araştırma düğüm sayısını bağlıdır. Ağ ne daha büyük bir haksız avantaj elde etmek için tehlikeye gereken daha fazla düğüm. Şablon kullanarak sanal makine ölçek kümeleri sağlanan bölge başına en fazla 15 araştırma düğümleri destekler.

### <a name="transaction-node-details"></a>İşlem düğümü ayrıntıları

Yük dengeli işlem düğümleri kümesi consortium üyesi de var. Bu düğümler, uygulamaları bu düğümler işlemleri göndermek ya da nitelikli akıllı anlaşmalar blockchain ağından yürütmeye kullanabilmesi için sanal ağ dışında erişilebilir. Tüm düğümleri Ethereum gidin (Geth) istemcinin kararlı bir sürüm olması ve dağıtılmış kayıt defteri tam bir kopyasını korumak için yapılandırılır. Özel genesis blok sağlanmazsa, bu düğümler Ethereum hesabı parolayla korunuyorsa aynı Ethereum hesabını kullanın.

İşlem düğümleri, yük dengeli yüksek kullanılabilirliği sürdürmek üzere bir kullanılabilirlik kümesi içinde. Şablon kullanarak sanal makine ölçek kümeleri sağlanan en fazla beş işlem düğümleri destekler.

### <a name="log-analytics-details"></a>Log analytics ayrıntıları

Her dağıtım, ayrıca yeni bir Log Analytics örneği oluşturur veya var olan bir örneğini katılabilirsiniz. Bu, çeşitli performans ölçümleri dağıtılan ağı yaptığı tüm sanal makinelerin izlenmesini sağlar.

## <a name="deployment-architecture"></a>Dağıtım mimarisi

### <a name="description"></a>Açıklama

Bu çözüm şablonu, tek veya çok çoklu bölge tabanlı üye Ethereum Konsorsiyum ağı dağıtabilirsiniz. Her bölgenin sanal ağı başka bir bölgede VNET ağ geçitleri ile bağlantı kaynaklarını zinciri topolojisinde bağlıdır. Ayrıca, her bir bölgede dağıtılan tüm Miner ve işlem düğümlerinin gerekli bilgileri içeren bir kayıt şirketi sağlar.

### <a name="standalone-and-consortium-leader-overview"></a>Tek başına ve consortium lider genel bakış

![Tek başına ve Consortium lider genel bakış](./media/ethereum-deployment/leader-overview.png)

### <a name="joining-consortium-member-overview"></a>Katılma consortium üyelerine genel bakış

![Katılma Consortium üyelerine genel bakış](./media/ethereum-deployment/member-overview.png)

## <a name="getting-started"></a>Başlarken

Bu işlem, birkaç sanal makine ölçek kümeleri dağıtma destekleyebilir ve yönetilen diskleri olan bir Azure aboneliği gerektirir. Gerekirse, başlamak için ücretsiz bir Azure hesabı oluşturun.

Bir abonelik güvenli hale getirildikten sonra Azure portalına gidin. Seçin **+ kaynak Oluştur**, Market (bkz: tümü), araması **Ethereum kavram iş Consortium**.

Şablon dağıtımı ilk üyenin ayak izini ağ yapılandırması için size yol gösterir. Dağıtım akışı beş adımlarına ayrılmıştır: temel, Operations Management Suite, dağıtım bölgeleri, ağ boyutu ve performansı, Ethereum ayarları.

### <a name="basics"></a>Temel Bilgiler

Altında **Temelleri**, abonelik ve kaynak grubu temel sanal makine özellikleri gibi herhangi bir dağıtım için standart parametreler için değerler belirtin.

![Temel Bilgiler](./media/ethereum-deployment/sample-deployment.png)

Parametre Adı|Açıklama| İzin Verilen Değerler|Varsayılan değerler
---|---|---|---
Bir yeni veya mevcut birleşim ağı oluşturulsun mu?|Yeni bir ağ oluşturmak veya önceden mevcut olan bir konsorsiyum ağı katılın|Yeni JOIN varolan oluşturma|Yeni Oluştur
Konsorsiyumu parçası olacak bir ağ dağıtmak?|Bu ağa katılmak gelecekteki dağıtımlar bir konsorsiyum ağı sağlar (görünür *Yeni Oluştur* Yukarıda seçilen)|Tek başına Consortium|Tek Başına
Kaynak ön eki |Dize kaynakları (2-4 alfasayısal karakterler) adlandırmak için temel olarak kullanıldı. Kaynağa özgü bilgi eklenir ancak benzersiz bir karması bazı kaynaklar için dizesine eklenir.|Alfasayısal karakterlerle başlayıp uzunluğu 2 ile 4|NA
VM kullanıcı adı| Dağıtılan her VM'nin (yalnızca alfasayısal karakterler) yönetici kullanıcı adı|1 - 64 karakter |gethadmin
Kimlik doğrulaması türü|Sanal makinenin kimliğini doğrulamak için yöntem. |Parola veya SSH ortak anahtarı|Parola
Parola (kimlik doğrulaması türü = parola)|Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Parola aşağıdaki gereksinimleri 3 içermelidir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter. <br />Tüm VM'lerin aynı parolayı başlangıçta olsa da, parola sağladıktan sonra değiştirebilirsiniz.|12 - 72 karakter|NA
SSH anahtarı (kimlik doğrulaması türü ortak anahtar =)|Uzaktan oturum açma için kullanılan güvenli Kabuk anahtarı.|| NA
Abonelik| Aboneliği Konsorsiyum ağı dağıtmak için||NA
Kaynak Grubu| Hangi Konsorsiyum ağı dağıtmak kaynak grubu.||NA
Konum| Kaynak grubu için bir Azure bölgesi. ||NA



### <a name="operations-management-suite"></a>Operations Management Suite

Operations Management Suite (OMS) dikey penceresinde, ağınız için bir OMS kaynak yapılandırmanıza olanak sağlar. OMS toplar ve yüzey yararlı ölçüm ve günlükleri ağınızdan, hızlı bir şekilde hata ayıklama ve ağ durumu denetleme olanağı tanıyacak verir. Kapasite ulaşıldığında ücretsiz sunulan OMS düzgün biçimde başarısız olur.

![Yeni OMS oluşturma](./media/ethereum-deployment/new-oms.png)

Parametre Adı|Açıklama| İzin Verilen Değerler|Varsayılan değerler
---|---|---|---
Mevcut OMS'ye bağlanma|Yeni bir Log Analytics örneği oluşturabilir veya var olan bir örneğini katılın|Yeni JOIN varolan oluşturma|Yeni Log Analytics konum oluşturun|Yeni Log Analytics dağıtılacağı bölgeyi (görünür if *Yeni Oluştur* seçili)
Mevcut OMS çalışma alanı kimliği|Çalışma alanı kimliği var olan örneğinin (görünür if *katılın mevcut* seçilir) OMS hizmet katmanı|Yeni örnek için fiyatlandırma katmanını seçin. Daha fazla bilgi https://azure.microsoft.com/pricing/details/log-analytics/ (görünür if *katılın mevcut* seçili)|Düğüm başına ücretsiz tek başına|Ücretsiz
Mevcut OMS birincil anahtar|Mevcut OMS örneğine bağlanmak için kullanılan birincil anahtar (görünür if *katılın mevcut* seçili)

### <a name="deployment-regions"></a>Dağıtım bölgeleri

Sonraki altında **dağıtım bölgeleri**, belirtmek için girişler **alımımın sayısı** verilen bölge sayısına göre Azure bölgelerinin seçimi ve Konsorsiyum ağı dağıtmak için. Kullanıcı en fazla beş bölgeleri içinde dağıtabilirsiniz.

![Dağıtım bölgeleri](./media/ethereum-deployment/deployment-regions.png)

Parametre Adı| Açıklama| İzin Verilen Değerler |Varsayılan değerler
---|---|---|---
Bölgelerin sayısı| Konsorsiyum ağı dağıtmak için bölge sayısı|1, 2, 3, 4, 5| 2
İlk bölgeye| İlk bölgeye Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.| Batı ABD
İkinci bölge |İkinci bölgesi (bölge sayısı 2 olarak seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.| Doğu ABD
Üçüncü bir bölge| Üçüncü bölgesi (bölge sayısı 3'olarak seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.| Orta ABD
Dördüncü bölge| Dördüncü bölgesi (bölge sayısı 4 seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.| Doğu ABD 2
Beşinci bölge| Beşinci bölgesi (bölge sayısı 5 seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.| Batı ABD 2

### <a name="network-size-and-performance"></a>Ağ boyutu ve performansı

Sonraki altında **ağ, boyut ve performans** girişleri Konsorsiyum ağı boyutunu belirtin. Sayısı ve boyutu araştırma düğümü ve işlem düğümü gibi.

![Ağ boyutu ve performansı](./media/ethereum-deployment/network-size-performance.png)

Parametre Adı |Açıklama |İzin Verilen Değerler| Varsayılan değerler
---|---|---|---
Araştırma düğüm sayısı|Bölge başına dağıtılan araştırma düğüm sayısı|2 - 15| 2
Araştırma düğüm depolama performansı|Dağıtılan araştırma düğümler yedekleme yönetilen diskin türü.|Standart veya Premium|Standart
Araştırma düğüm sanal makine boyutu|Araştırma düğümleri için kullanılan sanal makine boyutu.|Standart A <br />Standart D <br />Standart D v2 <br />Standart F serisi <br />Standart DS <br />ve standart FS|Standart D1v2
Yük dengeli işlem düğümleri sayısı|Ağın bir parçası olarak sağlamak için işlem düğümleri sayısı.|1 - 5| 2
İşlem düğümü depolama performansı|Dağıtılmış işlem düğümlerinin her biri yedekleme yönetilen diskin türü.|Standart veya Premium|Standart
İşlem düğümü sanal makine boyutu|İşlem düğümleri için kullanılan sanal makine boyutu.|Standart A <br />Standart D <br />Standart D v2 <br />Standart F serisi <br />Standart DS <br />ve standart FS|Standart D1v2

### <a name="ethereum-settings"></a>Ethereum ayarları

Sonraki altında **Ethereum ayarları**, ağ kimliği ve Ethereum hesap parolası veya genesis bloğu gibi Ethereum ile ilgili yapılandırma ayarlarını belirtin.

![Ethereum ayarları](./media/ethereum-deployment/standalone-leader-deployment.png)

Parametre Adı |Açıklama |İzin Verilen Değerler|Varsayılan değerler
---|---|---|---
ConsortiumMember kimliği|Çakışma önlemek için IP adresi alanları yapılandırmak için kullanılan consortium ağa katılan her üye ile ilişkili kimlik. <br /><br />Üye kimliği aynı ağda farklı kuruluşlar arasında benzersiz olmalıdır. Hatta aynı kuruluşa birden fazla bölgeye dağıtırken benzersiz üye kimliği gereklidir.<br /><br />Katılan diğer üyeleriyle paylaşmak olması gerektiğinden, bu parametrenin değerini not edin.|0 - 255
Ethereum ağ kimliği|Dağıtılan consortium Ethereum ağ ağ kimliği. Her Ethereum ağ kendi ağ 1 olan ortak ağ kimliği ile kimliği vardır. Ağ erişimi için araştırma düğümleri kısıtlı olsa da, çakışmaları önlemek için çok sayıda kullanarak yine de öneririz.|5 - 999,999,999| 10101010
Özel genesis bloğu|Otomatik olarak genesis blok oluşturmak veya özel bir sağlamak için seçenek.|Evet/Hayır| Hayır
Ethereum hesap parolası (özel genesis blok = Hayır)|Her bir düğümüne içeri Ethereum hesabının güvenliğini sağlamak için kullanılan yönetici parolası. Parola şunları içermelidir: 1 büyük harf karakter, 1 küçük harf ve 1 sayı.|en az 12 karakter|NA
Ethereum özel anahtar parolası (özel genesis blok = Hayır)|Oluşturulan varsayılan Ethereum hesapla ilişkili ECC özel anahtarı oluşturmak için kullanılan parola. Önceden oluşturulan bir özel anahtarı açıkça geçirilmesi gerekmez.<br /><br />Bir parola ile güçlü bir özel anahtar ve diğer consortium üyeleriyle örtüşme emin olmak için yeterli doğrulukla göz önünde bulundurun. Parola en az şunları içermelidir: 1 büyük harf karakter, 1 küçük harf ve 1 sayı.<br /><br />Not iki üye aynı parolayı oluşturulan hesapları kullanıyorsanız, aynı olacaktır. Tek bir kuruma bölgeler arasında dağıtmak çalışıyor ve tüm düğümlere tek bir hesap (para temel) paylaşmak istiyor, parolayı yararlı olur.|en az 12 karakter|NA
Genesis engelle (özel genesis blok = Yes)|Özel genesis blok temsil eden JSON dizesi. Özel ağlar altında burada genesis bloğunun biçimi hakkında daha fazla ayrıntı bulabilirsiniz.<br /><br />Ethereum hesabınız hala özel genesis blok sağlanırken oluşturulur. Araştırma için bekleme için genesis bloğundaki bir prefunded Ethereum hesabı belirtme göz önünde bulundurun.|Geçerli bir JSON |NA
Paylaşılan anahtar bağlantısı|VNET ağ geçitleri arasında bağlantı için bir paylaşılan anahtar.| en az 12 karakter|NA
Consortium veri URL'si|Başka bir üyesinin dağıtım tarafından sağlanan ilgili consortium yapılandırma verilerini işaret eden URL. <br /><br />Bu bilgiler, bir dağıtım olan zaten bağlı bir üyesi tarafından sağlanır. Ağın geri kalanı dağıttıysanız CONSORTIUM veri adlı şablon dağıtımı çıkış URL'dir.||NA
Sanal ağa bağlanmak için ağ geçidi|VNet ağ geçidinin bağlantı kurmak için kaynak yolu.<br />Bu bilgiler, bir dağıtım olan zaten bağlı bir üyesi tarafından sağlanır. Ağın geri kalanı dağıttıysanız, şablon dağıtımı çıkış CONSORTIUM_MEMBER_GATEWAY_ID adlı URL'dir. Not: URL ve sanal ağ geçidi aynı üyenin consortium veri kaynağı kullanılması gerekir.||NA
Eş bilgileri kayıt uç noktası|Başka bir üyesinin dağıtım tarafından sağlanan eş bilgisi uç noktası|İlk üye consortium geçerli uç noktası|NA
Anahtar, eş bilgileri kayıt şirketi|Başka bir üyesinin dağıtım tarafından sağlanan eş bilgileri birincil anahtar|İlk üye consortium geçerli birincil anahtarı|NA

### <a name="summary"></a>Özet

Özet dikey penceresi aracılığıyla belirtilen girişleri inceleyin ve temel dağıtım öncesi doğrulama çalıştırmak için tıklayın.

![Özet](./media/ethereum-deployment/summary.png)

Yasal ve gizlilik koşulları gözden geçirin ve tıklayın **satın alma** dağıtılacak. Daha sonra dağıtım birden fazla bölgeye yok veya bir konsorsiyum ağı için bu şablon önceden diğer üyeleri ile ağ bağlantısı desteklemek için gerekli VPN ağ geçitleri dağıtır. Ağ geçidi dağıtımının en çok 45 50 dakika sürebilir.

## <a name="post-deployment-sanity-checks"></a>POST dağıtım sağlamlık denetimleri

### <a name="administrator-page"></a>Yönetici sayfası

Dağıtım başarıyla tamamlandıktan sonra sağlanan tüm kaynakları, blok zinciri ağınıza bir görünümünü elde etmek için yönetici sayfasına gidebilirsiniz ve sağlamlık dağıtım durumunu denetleyin. Yönetim sayfasının URL'sini yük dengeleyicinin DNS adıdır; Şablon dağıtımı ADMIN_SITE adlı çıktı bölümünde yok.

Bulmak için dağıtıldığı kaynak grubunu seçin. Ardından, **genel bakış**ve bağlantıya tıklayın hemen altında **dağıtımları** başarılı sayısını gösterir.

![Kaynak grubuna genel bakış](./media/ethereum-deployment/resource-overview.png)

Yeni ekran dağıtım geçmişini gösterir. İlk dağıtım kaynağı (örneğin, microsoft-azure-blockchain.azure-blok zinciri-Hi...) seçin ve Ara **çıkışları** sayfanın alt kısmında bölümünde. Şablon dağıtımı çıkış parametresi ADMIN_SITE olarak listelenen yönetim sayfasının URL'sini görürsünüz.

![Kaynak dağıtımları](./media/ethereum-deployment/resource-deployments.png)

![Dağıtım çıkışı](./media/ethereum-deployment/deployment-output.png)

Yönetici sayfasına ulaşmak için kopyalama **yönetici sitesi** çıktı ve başka bir sekmede açmak.

Yönetici sayfasına Ethereum düğümü durum bölümü gözden geçirerek dağıtılmış topoloji üst düzey bir genel bakış elde edebilirsiniz. Bu, tüm düğüm ana bilgisayar adları ve bunların eş sayısını görülen son blok içerir. En az bir arasında eş sayıdır her düğüm için küçüktür *toplam düğüm sayısı* ve yapılandırılmış en fazla eş sayısı. Unutmayın, eş sayısı ağ içinde dağıtılabilir düğüm sayısını kısıtlamaz.
Bazen, eş sayısı (toplam sayısı düğüm - 1) değerinden küçük olması ve dalgalanma görürsünüz. Sayım farkı, her zaman defterindeki çatalları küçük değişiklikler eş sayısı neden olabilir, düğümleri iyi durumda olmayan, bir oturum değil. Son olarak, çatallar veya aksamalar sistemde belirlemek için ağdaki her bir düğüm tarafından görülen en son blok inceleyebilirsiniz.

![Düğüm durumu](./media/ethereum-deployment/node-status.png)

Düğüm durumu 10 saniyede bir yenilenir. Bir tarayıcı aracılığıyla sayfayı yeniden yükleyin veya **yeniden** Görünümü güncelleştirmek için düğme.

### <a name="oms-portal"></a>OMS portalı

Dağıtım çıkışı (OMSPORTALURL) bağlantıyı izleyerek ya da dağıtılmış kaynak grubunuzda OMS kaynak seçerek, OMS portalında bulabilirsiniz.

![OMS URL'Sİ](./media/ethereum-deployment/oms-url.png)

![OMS kaynak](./media/ethereum-deployment/oms-resource.png)

Portala ilk üst düzey ağ istatistiklerini genel bir bakış görüntüler.

![OMS genel bakış](./media/ethereum-deployment/oms-overview.png)

Genel Bakış'ta tıklayarak düğüm başına istatistiklerini görüntülemek için bir portalına yönlendirir.

![Düğüm başına istatistikleri](./media/ethereum-deployment/per-node-stats.png)

### <a name="accessing-nodes"></a>Accessing düğümleri

Belirtilen yönetici kullanıcı adı ve parolası/SSH anahtarı ile sanal makineler için işlem düğümlerini SSH aracılığıyla uzaktan bağlanabilir. Kendi genel IP adresleri işlem düğümünü sanal makinelere sahip olduğundan, yük dengeleyici üzerinden gidin ve bağlantı noktası numarasını belirtmeniz gerekecektir. Şablon dağıtımı çıkış parametresi olarak, ilk işlem düğümüne erişmek için çalıştırmak için SSH komutunu listelenir **SSH_TO_FIRST_TX_NODE** (için örnek dağıtımı: ssh -p 4000 gethadmin@leader4vb.eastus.cloudapp.azure.com). Ek işlem düğümlerine almak için bağlantı noktası numarası bir artırmasına (örneğin, ilk işlem düğümünü 4000 numaralı bağlantı noktasında olur).

Araştırma düğümleri çalıştırdığınız sanal makineler dışarıdan erişilebilir olmadığından işlem düğümlerinden biri gitmeniz gerekiyor. Bir işlem düğümü için bir SSH oturumu açtıktan sonra işlem düğümü üzerinde özel anahtarınızı yükleyin veya hiçbir araştırma düğümlerin bir SSH oturumu başlatmak için parolanızı kullanın.

**Not**

Ana bilgisayar adları yönetim sitesinden veya Azure Portalı'ndan elde edilebilir. Azure portalında, ana bilgisayar adlarını bu sanal makine ölçek kümesi (VMSS) kaynakta mevcut düğümlerinin altında listelenen **örnekleri**, gerçek ana bilgisayar adları farklıdır. Örneğin, Azure Portalı'ndaki ana bilgisayar adı gibi görünebilir **mn asdfmv reg1_0** ancak sunucunun gerçek ana bilgisayar gibi **mn asdfmv reg**.

Örneğin:

Azure portalı ana bilgisayar adı| Gerçek ana bilgisayar adı
---|---
MN ethwvu reg1_0| MN ethwvu reg1000000
MN ethwvu reg1_1 |MN ethwvu reg1000001
MN ethwvu reg1_2 |MN ethwvu reg1000002

## <a name="adding-a-new-consortium-member"></a>Yeni consortium üye ekleme

### <a name="sharing-data"></a>Veri paylaşımı

İlk üye (veya bağlı üye) Consortium katılın ve kendi bağlantı kurmak için bazı bilgilere diğer üyeleri vermeniz gerekir. Bu avantajlar şunlardır:

1. **Yapılandırma verilerini Consortium paylaşılan**: iki üyeleri arasındaki Ethereum bağlantıyı düzenlemek için kullanılan veri kümesi yok. Genesis blok, consortium ağ kimliği ve önyükleme düğümleri dahil olmak üzere gerekli bilgileri, işlem düğümlerinde Öncü veya başka bir dağıtılan üye bir dosyaya yazılır. Bu dosyanın konumu adlı şablon dağıtımı çıkış parametresinde listelenen **CONSORTIUM veri**.
2. **Bilgileri endpoint eş**: liderleri ya da başka bir üyesinin dağıtım Ethereum ağa bağlı tüm düğümleri zaten bilgi almak için eş bilgileri kayıt uç noktası. Her düğüm ile ilgili bilgi kümesi DB depoları düğümün ana bilgisayar adı, özel gibi bilgileri ağ IP adresi vb. bağlı. Adlı şablon dağıtımı çıkış parametresi budur **PEER_INFO_ENDPOINT**.
3. **Eş birincil anahtar bilgisi**: Eş bilgisi Kaydedici birincil anahtarı öncü 's veya diğer üyenin eş bilgileri birincil anahtara erişmek için kullanılır. Adlı şablon dağıtımı çıkış parametresi budur **PEER_INFO_PRIMARY_KEY**.


4. **VNET ağ geçidi**: her üye varolan bir üye ile tüm blok zinciri ağa bir bağlantı kurar. Sanal AĞA bağlamak için VNET ağ geçidinin bağlanmakta olduğunuz üyenin kaynak yolu gerekir. Adlı şablon dağıtımı çıkış parametresi budur **CONSORTIUM_MEMBER_GATEWAY_ID**.
5. **Paylaşılan anahtar**: A önceden belirlenmiş gizli arasında bağlantı kurma iki Konsorsiyum ağı üyesi. Dağıtım bağlamı dışında üzerinde anlaşılan alfasayısal bir dize (1 ile 128 karakter arasında) budur. (Örneğin, **MySharedKeyAbc123**)

### <a name="acceptance-of-new-member"></a>Yeni üye kabulü

Katılma üye kendi ağ başarıyla dağıtıldıktan sonra bu adımın yapılması gerekir. Varolan bir üye üyesi ağına katılın ve işlem trafiği de görmenize önce son yapılandırma bağlantı kabul etmek için kendi VPN ağ geçidinizde gerçekleştirmeniz gerekir. Başka bir deyişle, bir bağlantı kurulana kadar katılma üye Ethereum düğümlerinin çalışmaz. Bu yapılandırma, PowerShell veya xPlat CLI yoluyla gerçekleştirilebilir. Bir PowerShell modülü ve xPlat CLI betiği ayrıca üzerinde depolanan consortium verilerin yanı sıra işlem düğümü. Betik konumu adlı dağıtım çıkış parametreleri olan **çifti-ağ geçidi-PS-MODULE** ve **çifti-GATEWAY-AZURE-CLISCRIPT**sırasıyla.

**PowerShell/CLI Kurulumu**

Azure PowerShell cmdlet'leri ve Azure xPlat kullanmaya başlama konusunda ek bilgi CLI, Azure belgelerinde bulunabilir.

En son sürümü yerel olarak yüklü Azure cmdlet'lerini ve bir oturum açma gerekir. Azure abonelik kimlik bilgilerinizle oturum oturum emin olun.

**PowerShell: Bağlantı**

PowerShell modülünü indirin ve yerel olarak depolayın. PowerShell modülünün konumu olarak belirtilen **çifti-ağ geçidi-PS-MODULE** şablon dağıtımı çıkış parametresi.

Zaten etkinleştirildiyse, **Set-ExecutionPolicy** işaretsiz bir modül çalışmasına izin vermek yerel oturumun cmdlet'i.

**Set-ExecutionPolicy CurrentUser sınırsız**

Lider dağıtım kullanarak dağıtılan Azure aboneliğinize sonra oturum açma

**Login-AzureRmAccount**

Ardından, modülü içeri aktarın:

**Import-Module <filepath to downloaded file>**

Son olarak, uygun girişle işlevi çalıştırın:

- **MyGatewayResourceId:** geçidinizin kaynak yolu. Adlı şablon dağıtımı çıkış parametresi budur **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId:** katılma üyenin ağ geçidinin kaynak yolu. Bu katılan bir üyesi tarafından sağlanır ve şablon dağıtımı çıktı parametresi olarak da adlandırılır **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName:** , bu ağ geçidi bağlantısı tanımlamak bir ad.
- **Paylaşılan anahtar:** önceden oluşturulmuş gizli arasında bağlantı kurma iki Konsorsiyum ağı üyesi.

**CreateConnection** -MyGatewayResourceId <resource path of your Gateway> - OtherGatewayResourceId < katılma üyenin ağ geçidinin kaynak yolu > - ConnectionName myConnection - SharedKey "MySharedKeyAbc123"

**xPlat CLI: bağlantı**

Azure CLI betiği indirip yerel olarak depolar. Azure CLI betiği konumunu adlı şablon dağıtım parametresinde belirtilen **çifti-GATEWAY-AZURE-CLI-BETİK**.

Uygun giriş ile betiği çalıştırın:

- **MyGatewayResourceId:** geçidinizin kaynak yolu. Adlı şablon dağıtımı çıkış parametresi budur **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **OtherGatewayResourceId:** katılma üyenin ağ geçidinin kaynak yolu. Bu Birleşen üye tarafından sağlanan ve şablon dağıtım parametresi olarak da adlandırılan bunların dağıtım **CONSORTIUM_MEMBER_GATEWAY_ID**.
- **ConnectionName:** , bu ağ geçidi bağlantısı tanımlamak bir ad.
- **Paylaşılan anahtar:** önceden oluşturulmuş gizli arasında bağlantı kurma iki Konsorsiyum ağı üyesi.
- **Konum:** , ağ geçidi kaynağı dağıtıldığı Azure bölgesi.

``` powershell
az network vpn-connection create --name $ConnectionName --resource-group
$MyResourceGroup --vnet-gateway1 $MyGatewayResourceId --shared-key $SharedKey --vnet-
gateway2 $OtherGatewayResourceId --enable-bgp
```

Mimari arasındaki bağlantı başarıyla yapılandırdıktan sonra şu şekilde olacaktır **lider** ve **üye** dağıtımları.

![Lider ve üye mimarisi](./media/ethereum-deployment/leader-member.png)

## <a name="fund-new-member-account"></a>Fon yeni üye hesabı

### <a name="generated-genesis-block"></a>Oluşturulan genesis bloğu

Dağıtım genesis blok oluşturursa ilk üye Ethereum hesabınız ile bir trilyon ether finanse (özel Genesis blok = Hayır). Diğer üyeleri, önceden finanse değil ve araştırma düğümlerini blokları Madencilik başladığınızda Ether ulaşıncaya kadar beklemesi gereken bir hesap gerekir. Yeni üyeler için Ether beklemek zorunda kalmamak için açıkça katılma üye Ethereum hesaplarını destekleyen shuttleworth gerekecektir.

Bunu yapmak için üyeleri birleştirme, kendi Yönetim Web sitesinde görüntülenen Ethereum hesabıyla sağlamanız gerekir. Ardından yönetici Web sitenizi Ether hesabınızdan hesabında sağlanan hesabı girerek aktarmak için de kullanabilirsiniz.

### <a name="custom-genesis-block"></a>Özel genesis bloğu

Özel genesis bloğunu belirtilen bir Ethereum hesabıyla sağlanırsa, MetaMask kullanabilir veya buradan ether aktarmak için başka bir aracı belirtilen hesaba önceden oluşturulmuş Ethereum hesabına yönetici Web sitesinde görünür. MetaMask kullanma hakkında yönergeler için bkz: [Ethereum hesabı oluşturma](#create-ethereum-account).

Özel genesis blok bir hesabı sağlanır veya önceden ayrılmış tüm hesaplara erişim izniniz yok, araştırma düğümlerinizi Ether hesabınızda (para temel) oluşturmak için benim başlayana kadar beklemeniz gerekir. Mevcut Fonu nasıl hızlı bir şekilde oluşturulan özel genesis bloğunda belirttiğiniz zorluk düzeyini bağlıdır.

## <a name="create-ethereum-account"></a>Ethereum hesabı oluşturma

Ek bir hesap oluşturmak için çeşitli çözümler kullanabilirsiniz. MetaMask, bir kimlik kasa ve genel bir Ethereum ağ bağlantısı, test veya özel sağlayan bir Chrome uzantısı, bu tür bir çözümdür. Ağ hesabını kaydetmek için bir işlem MetaMask formulates.

Diğer herhangi bir işlem gibi bu işlem işlem düğümlerinden biri olacak ve aşağıda gösterildiği gibi bir blok halinde sonunda İncelenmiş.

![İşlem düğümü](./media/ethereum-deployment/transaction-node.png)

Chrome'da uzantıyı yüklemek için Özelleştir gidin ve Google Chrome (taşma düğme) denetimi > Diğer Araçlar > uzantılar > daha uzantılarını alma ve MetaMask arayın.

![MetaMask uzantısı](./media/ethereum-deployment/metamask-extension.png)

Yüklendikten sonra MetaMask açın ve yeni bir kasa oluşturun. Varsayılan olarak, kasa Morden Test ağa bağlanır. Bu özellikle işlem düğümlerinin önündeki yük dengeleyici için dağıtılan özel consortium ağa bağlanacak şekilde değiştirin. Şablon çıktısı olarak adlandırılmış kullanıma sunulan Ethereum RPC uç nokta 8545, bağlantı noktası almak `ETHEREUM-RPC-ENDPOINT`, aşağıda gösterildiği gibi özel RPC girin.

![MetaMask ayarları](./media/ethereum-deployment/metamask-settings.png)

Kasa oluşturarak, bir hesap içeren Cüzdan oluşturun. Ek hesap oluşturmak için Seç **anahtar hesapları** ardından **+** düğmesi.

![MetaMask hesap oluştur](./media/ethereum-deployment/metamask-create-account.png)

### <a name="initiate-initial-ether-allocation"></a>İlk Ether tahsisat başlatın

Yönetici sayfası Ether önceden ayrılmış hesabından başka bir Ethereum hesabına aktarmak için bir işlem oluşturmak. Bu Ether aktarım işlem düğümü için gönderilen ve aşağıda gösterildiği gibi bir bloğu içine İncelenmiş bir işlemdir.

![İşlem düğümü](./media/ethereum-deployment/transaction-node-mined.png)

Pano simgesi aracılığıyla MetaMask Cüzdan ether aktarmak ve yönetici sayfasına geri dönmek istediğiniz Ethereum hesabı adresini kopyalayın. Kopyalanmış hesabı, 1000 ether önceden ayrılmış Ethereum hesabından yeni oluşturulan hesabınıza aktarmak için giriş alanı yapıştırın. Tıklayın **Gönder** ve hareketin bloğu içine İncelenmiş için bekleyin.

![Düğüm durumu](./media/ethereum-deployment/node-status2.png)

İşlem, araştırılan bloğuna kararlıdır sonra MetaMask hesabınız için hesap bakiyesi 1000 Ether aktarımı ücreti yansıtılır.

![MetaMask aktarımı](./media/ethereum-deployment/metamask-transfer.png)

### <a name="transfer-of-ether-between-accounts"></a>Hesapları arasında Ether aktarımı

Bu noktada, özel bir konsorsiyum ağı içinde işlemleri yürütmek hazır olursunuz. En basit Ether bir hesaptan diğerine aktarılması için bir işlemdir. Bu tür bir işlem formüle etmek için MetaMask bir kez daha, ikinci bir hesaba yukarıda kullanılan ilk hesaptan para aktarma kullanabilirsiniz.

Gelen **Cüzdan 1** MetaMask içinde Gönder'e tıklayın. Oluşturulan ikinci Cüzdan adresini kopyalayın **alıcı adresi** giriş alanını ve içindeki aktarmak için Ether miktarını **tutarı** giriş alanı. Tıklayın **Gönder** ve işlem kabul edin.

![MetaMask gönderme işlemi](./media/ethereum-deployment/metamask-send.png)

Bir kez daha, işlem olduğunda ve İncelenmiş bloğu içine kaydedilen hesabını bakiyeleri buna uygun olarak yansıtılır. Not **Cüzdan 1**ait bakiye kesinti işlem işlemek için bir araştırma ücret gerekiyordu bu yana 15'ten fazla Ether.

![Cüzdan 1](./media/ethereum-deployment/wallet1.png)

![Cüzdan 2](./media/ethereum-deployment/wallet2.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulama ve özel consortium blockchain ağınıza karşı akıllı sözleşme geliştirme odaklanmak artık hazırsınız.
