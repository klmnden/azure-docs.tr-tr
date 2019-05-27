---
title: Ethereum kavram yetki Consortium - Azure
description: Çoklu üyeli bir konsorsiyum Ethereum ağ yapılandırmak ve dağıtmak Ethereum kavram yetki Consortium çözümü kullanın
services: azure-blockchain
keywords: ''
author: CodyBorn
ms.author: coborn
ms.date: 04/08/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: vamelech
ms.openlocfilehash: 3531b43e6aee1eedef811e81e192873c5b5ed561
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66126805"
---
# <a name="ethereum-proof-of-authority-consortium"></a>Ethereum yetkilisi kavram consortium

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
[Bu çözüm](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) dağıtmak, yapılandırmak ve Azure ve Ethereum minimum bilgi ile çok üye consortium yetkilisi kavram Ethereum ağ yöneten kolaylaştırmak için tasarlanmıştır.

Birkaç kullanıcı girişleri ve Azure Portalı aracılığıyla tek tıklamayla dağıtım ile her üyenin ağ kaplama alanını kullanarak Microsoft Azure işlem, ağ ve depolama hizmetleri dünya çapında sağlayabilirsiniz. Her üyenin ağ kaplama alanını bir dizi yük dengeleme olanağı sunan bir doğrulayıcı düğümden oluşur. sahip olan bir uygulama veya kullanıcıya Ethereum işlemleri göndermek için etkileşim kurabilirsiniz.

## <a name="concepts"></a>Kavramlar

### <a name="terminology"></a>Terminoloji

-   **Fikir birliğine varılmış** -blok doğrulama ve oluşturma aracılığıyla dağıtılmış ağ üzerinden veri eşitleme işlemi.

-   **Consortium üye** - bir fikir birliğine varılmış Blockchain ağdaki katılan varlık.

-   **Yönetici** -verilen consortium üyesi için katılım yönetmek için kullanılan bir Ethereum hesabı.

-   **Doğrulayıcı** -adına bir yönetici fikir birliğine varılmış katıldığı bir Ethereum hesabı ile ilişkili bir makine

### <a name="proof-of-authority"></a>Yetki başlangıcı kavram

Bu blok zinciri topluluğuna Acemi, bu çözüm kolay ve yapılandırılabilir bir şekilde azure'da teknolojisi hakkında bilgi edinmek için harika bir fırsat sürümüdür. İş kavram ağ Self düzenleyen ve adil katılımını sağlamak için maliyetlerini hesaplama yararlanan bir Sybil Direnci mekanizmadır. Bu harika burada cryptocurrency yarışmaya ağdaki güvenlik yükseltir blockchain anonim ve açık ağlarda çalışır. Ancak, özel/consortium ağlarda temel alınan Ether değeri yok. Bir alternatif, kavram yetki başlangıcı, daha uygun izin verilen ağları burada tüm fikir birliğine varılmış katılımcıları bilinen ve güvenilir protokolüdür. Araştırma için gerek olmadan hala Byzantine hata toleransı korurken, yetki başlangıcı kavram daha verimli olur.

### <a name="consortium-governance"></a>Consortium idare

Yetki başlangıcı kavram izin verilen ağ durumunun iyi kalmasını sağlamak için ağ yetkilileri listesi üzerinde kullandığından, bu izin listesine değişiklikler yapmasını adil bir mekanizma sağlamanız önemlidir. Her dağıtım, akıllı sözleşmeler ve izin verilen bu listenin zincir İdaresi için portal kümesi ile birlikte gelir. Önerilen bir değişikliğin consortium üyeleri tarafından bir Çoğunluk oyu ulaştığında, değişiklik geçirilmeden. Bu yeni fikir birliğine varılmış katılımcıları olmasını sağlar eklendi veya dürüst bir ağ teşvik eder saydam bir şekilde kaldırılacak katılımcıları gizliliğinin ihlal edilmemesini.

### <a name="admin-account"></a>Yönetici hesabı

Yetki başlangıcı kavram düğümleri dağıtımı sırasında bir yönetici Ethereum adresi sorulur. Birkaç farklı sistemler oluşturabilir ve bu Ethereum hesabının güvenliğini sağlamak için kullanabilirsiniz. Bu adres, ağ üzerinde bir yetkilisi olarak eklendikten sonra idaresinde katılmak için bu hesabı kullanabilirsiniz. Bu yönetici hesabı, aynı zamanda fikir birliğine varılmış katılım bu dağıtımın bir parçası oluşturulan Doğrulayıcı düğümleri için temsilci seçmek için kullanılır. Yalnızca genel Ethereum adresi kullanıldığından her yönetici, istenen güvenlik modeli izleyen bir şekilde özel anahtarlarını güvenli hale getirmek için esnekliğine sahiptir.

### <a name="validator-node"></a>Doğrulayıcı düğümü

Yetki başlangıcı kavram protokolünde Doğrulayıcı düğümleri geleneksel miner düğümleri gerçekleşir. Her bir doğrulayıcı akıllı sözleşme izin listesine eklenen benzersiz bir Ethereum kimliği vardır. Bu listede bir doğrulayıcı hale geldikten sonra blok oluşturma işleminde yer alabilir. Bu işlem hakkında daha fazla bilgi için ilgili eşlik'ın belgelere bakın [yetkilisi yuvarlak fikir birliğine varılmış](https://wiki.parity.io/Aura). Her consortium üye beş bölgede coğrafi yedeklilik için iki veya daha fazla Doğrulayıcı düğümleri sağlayabilirsiniz. Doğrulayıcı düğümleri, fikir birliğine varılmış için temel alınan dağıtılmış kayıt defteri durumuna gelmesini diğer Doğrulayıcı düğümleri ile iletişim kurar.
Ağdaki adil katılımını sağlamak için consortium üyelerin ağda ilk üye değerinden daha fazla doğrulayıcıları kullanmaları yasaktır (ilk üye üç doğrulayıcıları dağıtır, üyelerin yalnızca en fazla üç doğrulayıcıya sahip olabilen).

### <a name="identity-store"></a>Kimlik deposu

Her üye aynı anda çalışan birden çok doğrulayıcı düğüm varsa ve her düğümü izin verilen bir kimlik olmalıdır doğrulayıcıları benzersiz bir etkin kimliği ağda güvenli bir şekilde edinebilir önemlidir. Bunu kolaylaştırmak için güvenli bir şekilde oluşturulan Ethereum kimlikleri tutan her üyenin abonelikte dağıtılmış bir kimlik Store oluşturduk. Dağıtımdan sonra orchestration kapsayıcı Ethereum özel anahtarı için her bir doğrulayıcı oluşturun ve Azure anahtar Kasası'nda depolayın. Eşlik düğüm başlatıldığında önce öncelikle bir kira kimliği başka bir düğüm tarafından toplanmış olmadığından emin olmak için kullanılmayan bir kimlik alır. Kimlik blokları oluşturmaya başlamak için yetki veren istemciye sağlanır. Barındırma VM bir kesinti oluşursa, bir değiştirme düğüm kimliğini gelecekte sürdürmek izin verme kimlik kira yayınlanacaktır.

### <a name="bootnode-registrar"></a>Bootnode kayıt şirketi

Bağlantı kolaylığı sağlamak için her üye bir dizi bağlantı bilgilerini barındıracak [veri API uç noktası](#data-api). Bu veriler, eşleme bir düğüm olarak katılan üyesi için sağlanan bootnodes listesini içerir. Bu veriler API işleminin bir parçası olarak, biz bu bootnode liste güncel tutun

### <a name="bring-your-own-operator"></a>Kendi işleci Getir

Genellikle consortium üyesi ağ idaresinde katılmak istiyor ancak çalıştırıp koruyacağınızı altyapılarını istemiyorsanız. Geleneksel sistemlerinden farklı ağ works blok zinciri sistemleri merkezi olmayan modeline karşı arasında tek bir işleci sahip. Bir ağ çalışması için merkezi bir aracıyı işe yerine PKI'nizin altyapı Yönetimi işleci consortium üyelerin devredebilirsiniz. Bu, üyelerin kendi altyapısı çalışmaz veya işlemi farklı bir iş ortağı için temsilci seçmek için seçebileceğiniz bir karma model sağlar. Temsilci işlemi iş akışı aşağıdaki gibi çalışır:

1.  **Consortium üye** Ethereum adresin (özel anahtarı içeren) oluşturur.

2.  **Consortium üye** genel Ethereum adresine sağlar **işleci**

3.  **İşleç** dağıtır ve Azure Resource Manager Çözümümüzü kullanarak PoA Doğrulayıcı düğümleri yapılandırır

4.  **İşleç** RPC ve yönetim uç noktası sağlar **Consortium üyesi**

5.  **Consortium üye** Doğrulayıcı düğümleri kabul eden bir isteğini imzalamak için kendi özel anahtarı kullandığı **işleci** kendi adınıza katılmak dağıtıldığını

### <a name="azure-monitor"></a>Azure İzleyici

Bu çözüm, ayrıca düğüm ve ağ istatistiklerini izlemek için Azure İzleyici ile birlikte gelir. Uygulama geliştiriciler için bu blok nesil istatistikleri izlemek için temel alınan blok zinciri görünürlük sağlar. Ağ İşletmenleri Azure İzleyici, hızlı bir şekilde algılayın ve altyapı istatistikleri ve sorgulanabilir günlükleri aracılığıyla ağ kesintileri önlemek için kullanabilirsiniz. Daha fazla bilgi için [hizmet izleme](#service-monitoring).

### <a name="deployment-architecture"></a>Dağıtım mimarisi

#### <a name="description"></a>Açıklama

Çoklu üyeli Ethereum Konsorsiyum ağı çok bölgeli tabanlı veya bu çözüm, tek bir dağıtabilirsiniz. Varsayılan olarak, RPC ve eşleme uç noktaları abonelikleri ve bulut basit bağlantısını etkinleştirmek için genel IP üzerinden erişilebilir. Yararlanarak öneririz [eşlik'ın kullanarak sözleşmeler](https://wiki.parity.io/Permissioning) erişim denetimleri için uygulama düzeyi. Abonelikler arası bağlantı için sanal ağ geçitleri yararlanarak VPN, arkasında dağıtılmış ağlar da destekliyoruz. Bu dağıtımları daha karmaşık olduğundan genel IP modeliyle başlatmaya önerilir.

#### <a name="consortium-member-overview"></a>Consortium üyelerine genel bakış

Her consortium üye dağıtım içerir:

-   Sanal makineler, PoA doğrulayıcıları çalıştırmak için

-   Azure Load Balancer RPC dağıtma, eşleme ve idare DApp istekleri için

-   Doğrulayıcı kimlik güvenliğini sağlamak için Azure anahtar kasası

-   Barındırma kalıcı ağ bilgilerini ve kiralama koordine için azure depolama

-   Günlükleri ve performans istatistikleri toplamak için azure İzleyici

-   Sanal ağ özel sanal ağlar arasında VPN bağlantılarına izin vermek için ağ geçidi (isteğe bağlı)

![Dağıtım mimarisi](./media/ethereum-poa-deployment/deployment-architecture.png)

Biz, Docker kapsayıcıları için güvenilirlik ve modülerlik yararlanın. Azure Container Registry barındırabilir ve her dağıtımın bir parçası olarak tutulan görüntüleri hizmet için kullanırız. Kapsayıcı görüntülerini oluşur:

-   Orchestrator

    -   Dağıtım sırasında bir kez çalışır

    -   Kimlikleri ve idare oluşturur sözleşmeleri

    -   Kimlik Store depoları kimlikleri

-   Eşlik istemci

    -   Kimlik Store kimlikten kiralar

    -   Bulur ve eşlere bağlanır

-   EthStats aracı

    -   Azure İzleyicisi'ne gönderir ve yerel günlüklerinin ve istatistiklerinin RPC üzerinden toplar

-   İdare DApp

    -   İdare sözleşmeleri ile etkileşim kurmak için web arabirimi

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları
### <a name="governance-dapp"></a>İdare DApp

Yetki başlangıcı kavram merkezi olmayan idare kalbidir. DApp olan bir dizi idare önceden dağıtılan [akıllı sözleşmeleri](https://github.com/Azure-Samples/blockchain/tree/master/ethereum-on-azure/) ve ağ üzerinde yetkilileri yönetmek için kullanılan bir web uygulaması.
Yetkilileri, yönetici kimlikleri ve Doğrulayıcı düğümleri ayrılır.
Yöneticiler, fikir birliğine varılmış katılım Doğrulayıcı düğümleri kümesi için temsilci seçmek için güce sahipsiniz. Yöneticiler ayrıca içine veya dışına ağ diğer yöneticileri oy.

![idare dapp](./media/ethereum-poa-deployment/governance-dapp.png)

-   **İdare - merkezi olmayan tercihinizden** ağ yetkilileri değişiklikleri yönetilen zincir select yöneticileri tarafından oylama.

-   **Doğrulayıcı temsilci -** yetkilileri her PoA dağıtımda ayarlanan Doğrulayıcı düğümlerini yönetebilirsiniz.

-   **Denetlenebilir değişiklik geçmişi -** her değişiklik, saydamlık ve denetlenebilirlik sağlama üzerinde blok zinciri kaydedilir.

#### <a name="getting-started-with-governance"></a>İdare ile çalışmaya başlama
Herhangi bir türden idare DApp aracılığıyla işlemlerini gerçekleştirmek için bir Ethereum Cüzdan yararlanarak gerekecektir.  Gibi tarayıcı içi Cüzdan kullanmak için en basit yaklaşımdır [MetaMask](https://metamask.io); ancak, bu ağda dağıtılan nitelikli akıllı anlaşmalar olduğundan etkileşimlerinizi idare sözleşmeye de otomatik hale.

MetaMask yükledikten sonra tarayıcıda idare DApp gidin.  URL, dağıtım onay e-posta veya dağıtım çıktıda Azure Portalı aracılığıyla bulabilirsiniz.  Yüklü bir tarayıcı içi Cüzdan yoksa, herhangi bir eylem; gerçekleştirmek mümkün olacaktır değil Ancak, yine yönetici durumu okuyabilirsiniz.  

#### <a name="becoming-an-admin"></a>Bir yönetici olma
Ağda dağıtılan ilk üye değilseniz, ardından otomatik olarak bir yönetici hale gelirler ve eşlik düğümlerinizi doğrulayıcıları listelenir.  Ağ birleştirdiğimiz, yönetici olarak Çoğunluk (50 %'den büyük) oylayan gerekir Mevcut yönetim kümesi.  Düğümlerinizi hala eşitleme ve blok zinciri doğrulama bir yönetici olmayan bir duruma seçerseniz; ancak blok oluşturma işleminde katılmayacaktır. Bir yönetici olun oylama işlemini başlatmak için tıklatın __Nominate__ Ethereum adresi ve diğer ad girin.

![Belirle](./media/ethereum-poa-deployment/governance-dapp-nominate.png)

#### <a name="candidates"></a>Aday
Seçme __adayları__ sekmesi, geçerli bir aday Yöneticisi gösterilir.  Bir aday kendisine yapılan bir Çoğunluk oyu geçerli yöneticiler tarafından ulaştığında, aday bir yöneticiye yükseltilen  Bir aday üzerinde oylamak için satırı seçin ve "en üstünde oy"'a tıklayın.  Oy üzerinde fikrinizi değiştirirseniz, aday seçin ve "Rescind oy"'a tıklayın.

![Aday](./media/ethereum-poa-deployment/governance-dapp-candidates.png)


#### <a name="admins"></a>Yöneticiler
__Yöneticileri__ sekmesi yöneticilerin geçerli kümesini Göster ve karşı oy olanağı sunar.  Daha fazla yönetici kaybeder sonra % 50 desteği'ne kıyasla, bunların ağ üzerindeki bir yönetici olarak kaldırılması.  Bu yönetici sahip herhangi bir doğrulayıcı düğüm Doğrulayıcı durumunu kaybederekten ve ağ üzerinde işlem düğümler haline gelir.  Bir yönetici, bir dizi nedenden ötürü Kaldırılabilir; Ancak, bu ilke önceden kabul etmek için consortium aittir.

![Yöneticiler](./media/ethereum-poa-deployment/governance-dapp-admins.png)

#### <a name="validators"></a>Doğrulayıcıları
Seçme __doğrulayıcıları__ sekmesi soldaki menüde, bu örnek ve bunların geçerli durumunu (düğüm türü) için geçerli dağıtılan eşlik düğümleri görüntülenir.  Bu görünümde geçerli dağıtılan consortium üye temsil ettiği her consortium üye bu listede doğrulayıcıları farklı bir dizi gerekir.  Bu yeni dağıtılan bir örneği ve kendi doğrulayıcılarının henüz eklemediniz seçeneği 'Ekle doğrulayıcısına' gösterilen.  Bu seçeneğin seçilmesi otomatik olarak bölgesel dengeli bir dizi eşlik düğümü seçin ve bunları Doğrulayıcı kümenize atayın.  Daha fazla düğüm izin verilen kapasiteden dağıttıysanız, kalan düğümleri ağ üzerinde işlem düğümleri olacak.

Her bir doğrulayıcı adresi aracılığıyla otomatik olarak atanır [kimlik deposu](#identity-store) azure'da.  Bir düğüm arıza yaparsa, başka bir düğüme yerini alacak, dağıtımınızdaki izin vererek kimliğini teknisyene.  Bu, fikir birliğine varılmış katılımınızı yüksek oranda kullanılabilir olmasını sağlar.

![Doğrulayıcıları](./media/ethereum-poa-deployment/governance-dapp-validators.png)

#### <a name="consortium-name"></a>Consortium adı
Herhangi bir yönetici, sayfanın üst kısmında görüntülenen Consortium adı güncelleştirebilir.  Consortium adı güncelleştirmek için sol üst köşedeki dişli simgesini seçin.

#### <a name="account-menu"></a>Hesap menüsü
Sağ üst köşesinde identicon ve Ethereum hesap diğer adı ' dir.  Bir yönetici değilseniz, diğer imkanına sahip olursunuz.

![Hesap](./media/ethereum-poa-deployment/governance-dapp-account.png)

### <a name="deploy-ethereum-proof-of-authority"></a>Ethereum kavram yetki dağıtma

Çok taraflı dağıtım akışı, bir örnek aşağıda verilmiştir:

1.  Üç üye MetaMask kullanarak Ethereum hesabı oluştur

2.  *Üyesi* kendi Ethereum genel adres sağlayarak Ethereum PoA dağıtır

3.  *Üyesi* consortium URL'sine sağlar *üye B* ve *üye C*

4.  *B üyesi* ve *üye C* Ethereum PoA kendi Ethereum genel adresi sağlama, dağıtma ve *üyesi*'s consortium URL'si

5.  *Üyesi* oyları *üye B* yönetici olarak

6.  *Üyesi* ve *üye B* hem oy *üye C* yönetici olarak

Bu işlem, birkaç sanal makine dağıtmaya destekleyebilir ve yönetilen diskleri olan bir Azure aboneliği gerektirir. Gerekirse, [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/) başlamak için.

Bir abonelik güvenli hale getirildikten sonra Azure portalına gidin. Seç '+', ('bkz. tüm'), Market ve Ethereum PoA Consortium arayın.

Aşağıdaki bölümde ilk üyenin ayak izini ağ yapılandırması için size yol gösterir. Dağıtım akışı beş adımlarına ayrılmıştır: Temel bilgiler, dağıtım bölgeleri, ağ boyutu ve performansı, Ethereum ayarları, Azure İzleyici.

#### <a name="basics"></a>Temel

Altında **Temelleri**, abonelik, kaynak grubu ve temel sanal makine özellikleri gibi herhangi bir dağıtım için standart parametreler için değerler belirtin.

Her parametre için ayrıntılı bir açıklaması aşağıdaki gibidir:

Parametre adı|Açıklama|İzin verilen değerler|Varsayılan değerler
---|---|---|---
Bir yeni veya mevcut birleşim ağı oluşturulsun mu?|Yeni bir ağ oluşturmak veya önceden mevcut olan bir konsorsiyum ağı katılın|Yeni JOIN varolan oluşturma|Yeni Oluştur
E-posta adresiniz (isteğe bağlı)|Dağıtımınız hakkında bilgi içeren dağıtımınız tamamlandığında bir e-posta bildirimi alırsınız.|Geçerli bir e-posta adresi|NA
VM kullanıcı adı|Dağıtılan her VM'nin (yalnızca alfasayısal karakterler) yönetici kullanıcı adı|1-64 karakter|NA
Kimlik doğrulaması türü|Sanal makinenin kimliğini doğrulamak için yöntem.|Parola veya SSH ortak anahtarı|Parola
Parola (kimlik doğrulaması türü = parola)|Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası.  Parola 3 birini içermelidir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter. Tüm VM'lerin aynı parolayı başlangıçta olsa da, parola sağladıktan sonra değiştirebilirsiniz.|12-72 karakter|NA
SSH anahtarı (kimlik doğrulaması türü ortak anahtar =)|Uzaktan oturum açma için kullanılan güvenli Kabuk anahtarı.||NA
Abonelik|Aboneliği Konsorsiyum ağı dağıtmak için||NA
Kaynak Grubu|Hangi Konsorsiyum ağı dağıtmak kaynak grubu.||NA
Location|Kaynak grubu için bir Azure bölgesi.||NA

Aşağıda bir örnek dağıtımı: ![temel dikey penceresi](./media/ethereum-poa-deployment/basic-blade.png)

#### <a name="deployment-regions"></a>Dağıtım bölgeleri

Ardından, dağıtım bölgeleri altında girdiler için verilen bölge sayısına göre Azure bölgelerinin seçimi ve Konsorsiyum ağı dağıtmak için alımımın sayısını belirtin. Kullanıcı, maksimum 5 bölgelerin dağıtabilirsiniz. Kaynak grubu konumunu temel bilgileri bölümünden eşleştirilecek ilk bölgeye seçme öneririz. Geliştirme veya test ağlar için tek bir bölge üyesi başına önerilir. Üretim için yüksek kullanılabilirlik için iki veya daha fazla bölgede dağıtma öneririz.

Her parametre için ayrıntılı bir açıklaması aşağıdaki gibidir:

  Parametre adı|Açıklama|İzin verilen değerler|Varsayılan değerler
  ---|---|---|---
  Bölgelerin sayısı|Konsorsiyum ağı dağıtmak için bölge sayısı|1, 2, 3, 4, 5|1
  İlk bölgeye|İlk bölgeye Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.|NA
  İkinci bölge|İkinci bölgesi (bölge sayısı 2 olarak seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.|NA
  Üçüncü bir bölge|Üçüncü bölgesi (bölge sayısı 3'olarak seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.|NA
  Dördüncü bölge|Dördüncü bölgesi (bölge sayısı 4 seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.|NA
  Beşinci bölge|Beşinci bölgesi (bölge sayısı 5 seçildiğinde görünür) Konsorsiyum ağı dağıtmak için|Tüm Azure bölgelerinde de izin verilir.|NA

Aşağıda bir örnek dağıtımı: ![dağıtım bölgeleri](./media/ethereum-poa-deployment/deployment-regions.png)

#### <a name="network-size-and-performance"></a>Ağ boyutu ve performansı

Ardından, sayı ve Doğrulayıcı düğümlerinin boyutu gibi bir konsorsiyum ağı boyutu için girişler 'ağ boyut ve performans altında' belirtin.
Doğrulayıcı düğüm depolama boyutu, blok zinciri olası boyutunu gösterecektir. Bu dağıtımdan sonra değiştirilebilir.

Her parametre için ayrıntılı bir açıklaması aşağıdaki gibidir:

  Parametre adı|Açıklama|İzin verilen değerler|Varsayılan değerler
  ---|---|---|---
  Yük dengeli Doğrulayıcı düğüm sayısı|Ağın bir parçası olarak sağlamak için Doğrulayıcı düğüm sayısı|2-15|2
  Doğrulayıcı düğüm depolama performansı|Dağıtılan Doğrulayıcı düğümler yedekleme yönetilen diskin türü.|Standart SSD veya Premium|Standart SSD
  Doğrulayıcı düğüm sanal makine boyutu|Doğrulayıcı düğümleri için kullanılan sanal makine boyutu.|Standart bir standart D, standart D-v2, standart F serisi, standart DS ve standart FS|Standart D1 v2

[Depolama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/managed-disks/)

[Sanal makine fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)

Sanal makine ve depolama katmanı, ağ performansını etkiler.  İstenen maliyet verimliliği göre aşağıdaki SKU'ları öneririz:

  Sanal makine SKU'su|Depolama katmanı|Fiyat|Performans|Gecikme
  ---|---|---|---|---
  F1|Standart SSD|düşük|düşük|Yüksek
  D2_v3|Standart SSD|orta|orta|orta
  F16s|Premium SSD|Yüksek|Yüksek|düşük

Aşağıda bir örnek dağıtımı: ![ağ, boyut ve performans](./media/ethereum-poa-deployment/network-size-and-performance.png)

#### <a name="ethereum-settings"></a>Ethereum ayarları

Ardından, Ethereum Ayarları'nın altında ağ kimliği ve Ethereum hesap parolası veya genesis bloğu gibi Ethereum ile ilgili yapılandırma ayarlarını belirtin.

Her parametre için ayrıntılı bir açıklaması aşağıdaki gibidir:

  Parametre adı|Açıklama|İzin verilen değerler|Varsayılan değerler
  ---|---|---|---
Consortium üye kimliği|Çakışma önlemek için IP adresi alanları yapılandırmak için kullanılan consortium ağa katılan her üye ile ilişkili kimlik. Özel bir ağ söz konusu olduğunda, üye kimliği aynı ağda farklı kuruluşlar arasında benzersiz olması gerekir.  Hatta aynı kuruluşa birden fazla bölgeye dağıtırken benzersiz üye kimliği gereklidir. Hiçbir çakışma olduğundan emin olmak için katılan diğer üyeleriyle paylaşmak ihtiyaç duyacağınız bu parametrenin değerini not edin.|0-255|NA
Ağ kimliği|Dağıtılan consortium Ethereum ağ ağ kimliği.  Her Ethereum ağ kendi ağ 1 olan ortak ağ kimliği ile kimliği vardır.|5 - 999,999,999|10101010
Yönetici Ethereum adresi|PoA idaresinde katıldığınız için kullanılan hesap adresi Ethereum.  MetaMask Ethereum adresi oluşturmak için kullanmanızı öneririz.|0 x'ile başlayan 42 alfasayısal karakterler|NA
Gelişmiş Seçenekler|Ethereum ayarları için Gelişmiş Seçenekleri|Etkinleştirmek veya devre dışı|Devre dışı bırak
Genel IP (Gelişmiş Seçenekler = etkin)|VNet ağ geçidinin arkasında ağ dağıtır ve eşleme erişim kaldırır. Bu seçenek belirlenirse, tüm üyeleri uyumlu olacak şekilde sanal ağ geçidi bağlantısı için kullanmanız gerekir.|Genel IP özel VNet|Genel IP
Block gaz sınırı (Gelişmiş Seçenekler = etkin)|Ağ başlatma bloğu gaz sınırı|Herhangi bir sayısal|50000000
Blok yeniden mühürleme süresi (sn)|Ağ üzerinde hiçbir işlem olduğunda, boş bir blok oluşturulur sıklığı. Yüksek sıklık, artan depolama maliyetlerine ancak daha hızlı finality sahip olur.|Herhangi bir sayısal|15
İşlem izin anlaşması (Gelişmiş Seçenekler = etkin)|İşlem adının sözleşmesinin ByteCode. Akıllı sözleşme dağıtım ve yürütme için izin verilen liste Ethereum hesapları kısıtlar.|Sözleşme bayt|NA

Aşağıda bir örnek dağıtımı: ![ethereum ayarları](./media/ethereum-poa-deployment/ethereum-settings.png)

#### <a name="monitoring"></a>İzleme

İzleme dikey penceresinde, ağınız için bir Azure İzleyici günlüklerine kaynak yapılandırmanıza olanak sağlar. İzleme Aracısı'nı toplar ve yüzey yararlı ölçüm ve günlükleri ağınızdan, hızlı bir şekilde hata ayıklama ve ağ durumu denetleme olanağı tanıyacak verir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

  Parametre adı|Açıklama|İzin verilen değerler|Varsayılan değerler
  ---|---|---|---
İzleme|İzleme özelliğini etkinleştirme seçeneği|Etkinleştirmek veya devre dışı|Etkinleştirme
Mevcut Azure İzleyici günlüklerine bağlanma|Yeni bir Azure İzleyici günlüklerine örneği oluşturabilir veya var olan bir örneğini katılın|Yeni veya mevcut katılın|Yeni oluştur
İzleme konumu (var olan Azure İzleyici günlüklerine Bağlan = Yeni Oluştur)|Yeni Azure İzleyici örneği burada oturum bölgeye dağıtılır|Bölgeleri tüm Azure izleme günlükleri|NA
Mevcut log analytics çalışma alanı kimliği (var olan Azure İzleyici günlüklerine Bağlan katılın mevcut =)|Çalışma alanı kimliği mevcut Azure İzleyicisi'nin örneği günlüğe kaydeder.||NA
Mevcut log analytics birincil anahtar (var olan Azure İzleyici günlüklerine Bağlan katılın mevcut =)|Mevcut Azure İzleyici günlüklerine örneğine bağlanmak için kullanılan birincil anahtar||NA


Aşağıda bir örnek dağıtımı: ![azure İzleyici](./media/ethereum-poa-deployment/azure-monitor.png)

#### <a name="summary"></a>Özet

Özet dikey penceresi aracılığıyla belirtilen girişleri inceleyin ve temel dağıtım öncesi doğrulama çalıştırmak için tıklayın. Dağıtmadan önce şablonu ve parametreleri indirebilirsiniz.

Yasal ve gizlilik koşullarını gözden geçirin ve dağıtmak için ' Satın Al' öğesini tıklatın. Dağıtım VNet ağ Geçitleriniz içeriyorsa, dağıtım 45 ' için 50 dakika sürer.

#### <a name="post-deployment"></a>Dağıtım sonrası

##### <a name="deployment-output"></a>Dağıtım çıkışı

Dağıtım tamamlandıktan sonra gerekli parametreleri onay e-posta aracılığıyla veya Azure portalı üzerinden erişebilirsiniz. Bu parametreleri bölümünde bulabilirsiniz:

-   Ethereum RPC endpoint

-   İdare Pano URL'si

-   Azure İzleyici URL'si

-   Veri URL'si

-   VNet ağ geçidi kaynak kimliği (isteğe bağlı)

##### <a name="confirmation-email"></a>Onay e-postası

Bir e-posta adresi belirtirseniz ([temel bilgileri bölümüne](#basics)), dağıtım çıkış bilgileri e-posta adresine bir e-posta gönderilir.

![Dağıtım e-posta](./media/ethereum-poa-deployment/deployment-email.png)

##### <a name="portal"></a>Portal

Dağıtım başarıyla tamamlandıktan sonra sağlanan tüm kaynakları kaynak grubunuzda çıkış parametreleri görüntüleyebilirsiniz.

1.  Portalda kaynak grubunuzu bulun

2.  Gidin *dağıtımları*

3.  Aynı ada sahip üst dağıtımı kaynak grubunuzu seçin

4.  Seçin *çıkışları*

### <a name="growing-the-consortium"></a>Consortium büyüyen

Consortium genişletmek için önce fiziksel ağ bağlanmanız gerekir.
Genel IP tabanlı bir dağıtım kullanarak bu ilk adım sorunsuz bağlıdır. VPN dağıtma bölümüne bakın [bağlanan sanal ağ geçidi](#connecting-vnet-gateways) yeni üye dağıtımının parçası olarak ağ bağlantısını yapma.  Kullanımı dağıtımınız tamamlandıktan sonra [idare DApp](#governance-dapp) bir ağ yöneticisi olmak için

#### <a name="new-member-deployment"></a>Yeni üye dağıtım

1.  Aşağıdaki bilgileri birleşme üyesi ile paylaşın. Bu bilgiler, dağıtım sonrası e-postanıza veya portal dağıtım çıktıda bulunabilir.

    -  Consortium veri URL'si

    -  Dağıttığınız düğüm sayısı

    -  Sanal ağ, ağ geçidi kaynak (VPN kullanıyorsanız) kimliği

2.  Dağıtma üye kullanması gereken [aynı çözüm](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) aşağıdakileri göz önünde bulundurarak ile ağ varlıklarını dağıtırken:

    -  Seçin *varolan katılın*

    -  Kalan adil gösterimi emin olmak için ağ üzerinde üyeler aynı sayıda Doğrulayıcı düğümleri seçin

    -  Önceki adımda sağlanan aynı Ethereum adresini kullanın

    -  Sağlanan geçirmek *Consortium veri URL'si* üzerinde *Ethereum ayarları* sekmesi

    -  VPN ağın geri kalanı ise seçin *özel VNet* Gelişmiş bölümünde

#### <a name="connecting-vnet-gateways"></a>VNet ağ geçitlerini bağlama

Varsayılan genel IP ayarları kullanarak dağıttıysanız bu adımı yoksayabilirsiniz. Özel bir ağ söz konusu olduğunda, farklı üyeleri VNet ağ geçidi bağlantıları bağlanır. Varolan bir üye, üye ağına katılın ve işlem trafiği de görmenize önce son bir bağlantı kabul etmek için kendi VPN ağ geçidi yapılandırmasına yapmanız gerekir. Bu, bir bağlantı kurulana kadar katılma üye Ethereum düğümlerinin çalışmadığından anlamına gelir. Bir tek hata noktası olasılığını azaltmak için consortium içine boş ağ bağlantılarından (kafes) oluşturmak için önerilir.

Yeni üye dağıttıktan sonra varolan bir üye yeni üye bir VNet ağ geçidi bağlantısı kurarak çift yönlü bağlantı tamamlamanız gerekir. Bunu başarmak için varolan bir üye gerekir:

1.  VNet ağ geçidinin bağlantı üyenin ResourceId (bkz. dağıtım çıkış)

2.  Paylaşılan bağlantı anahtarı

Varolan bir üye, bağlantıyı tamamlamak için aşağıdaki PowerShell betiğini çalıştırmanız gerekir. Azure Cloud Shell kullanarak portalın sağ üst gezinti çubuğunda bulunan öneririz.

![cloud shell](./media/ethereum-poa-deployment/cloud-shell.png)

```Powershell
$MyGatewayResourceId = "<EXISTING_MEMBER_RESOURCEID>"
$OtherGatewayResourceId = "<NEW_MEMBER_RESOURCEID]"
$ConnectionName = "Leader2Member"
$SharedKey = "<NEW_MEMBER_KEY>"

## $myGatewayResourceId tells me what subscription I am in, what ResourceGroup and the VNetGatewayName
$splitValue = $MyGatewayResourceId.Split('/')
$MySubscriptionid = $splitValue[2]
$MyResourceGroup = $splitValue[4]
$MyGatewayName = $splitValue[8]

## $otherGatewayResourceid tells me what the subscription and VNet GatewayName are
$OtherGatewayName = $OtherGatewayResourceId.Split('/')[8]
$Subscription=Select-AzSubscription -SubscriptionId $MySubscriptionid

## create a PSVirtualNetworkGateway instance for the gateway I want to connect to
$OtherGateway=New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
$OtherGateway.Name = $OtherGatewayName
$OtherGateway.Id = $OtherGatewayResourceId
$OtherGateway.GatewayType = "Vpn"
$OtherGateway.VpnType = "RouteBased"

## get a PSVirtualNetworkGateway instance for my gateway
$MyGateway = Get-AzVirtualNetworkGateway -Name $MyGatewayName -ResourceGroupName $MyResourceGroup

## create the connection
New-AzVirtualNetworkGatewayConnection -Name $ConnectionName -ResourceGroupName $MyResourceGroup -VirtualNetworkGateway1 $MyGateway -VirtualNetworkGateway2 $OtherGateway -Location $MyGateway.Location -ConnectionType Vnet2Vnet -SharedKey $SharedKey -EnableBgp $True
```

### <a name="service-monitoring"></a>Hizmet izleme

Azure İzleyici portalınız aşağıdaki dağıtım e-postadaki bağlantıya veya dağıtım çıkış parametresi bulma bulabilirsiniz \[OMS\_portalı\_URL\].

Portalda ilk önce üst düzey ağ istatistiklerini ve düğüm genel bakış görüntüler.

![İzleme kategorileri](./media/ethereum-poa-deployment/monitor-categories.png)

Seçme **düğüm genel bakış** düğüm başına altyapı istatistiklerini görüntülemek için bir portalına yönlendirir.

![düğüm istatistikleri](./media/ethereum-poa-deployment/node-stats.png)

Seçme **ağ istatistikleri** Ethereum ağ istatistiklerini görüntülemek için yönlendirir.

![Ağ istatistikleri](./media/ethereum-poa-deployment/network-stats.png)

#### <a name="sample-kusto-queries"></a>Kusto sorgu örneği

Bu panolar sorgulanabilir ham günlükleri kümesidir. Panoları özelleştirin, hataları araştırmak ve uyarı verme eşiği kurulumu için bu ham günlükleri'ni kullanabilirsiniz. Aşağıda günlük arama aracında olabilecek örnek sorguları bir dizi çalıştırıldı bulabilirsiniz:

##### <a name="lists-blocks-that-have-been-reported-by-more-than-one-validator-useful-to-help-find-chain-forks"></a>Birden fazla Doğrulayıcısı tarafından bildirilen blokları listeler. Zincir çatalları bulmak yararlıdır.

```sql
MinedBlock_CL
| summarize DistinctMiners = dcount(BlockMiner_s) by BlockNumber_d, BlockMiner_s
| where DistinctMiners > 1
```

##### <a name="get-average-peer-count-for-a-specified-validator-node-averaged-over-5-minute-buckets"></a>Belirtilen Doğrulayıcı düğümü 5 dakika içinde demetler ortalaması için ortalama eş sayısı alınamadı.

```sql
let PeerCountRegex = @"Syncing with peers: (\d+) active, (\d+) confirmed, (\d+)";
ParityLog_CL
| where Computer == "vl-devn3lgdm-reg1000001"
| project RawData, TimeGenerated
| where RawData matches regex PeerCountRegex
| extend ActivePeers = extract(PeerCountRegex, 1, RawData, typeof(int))
| summarize avg(ActivePeers) by bin(TimeGenerated, 5m)
```

### <a name="ssh-access"></a>SSH erişimini

Güvenlik nedeniyle, SSH bağlantı noktası erişim varsayılan olarak bir ağ grubu güvenlik kuralı tarafından reddedildi. Sanal makine örnekleri PoA ağdaki erişmek için bu kural için değiştirmeniz gerekir \"izin ver\"

1.  Azure portalından dağıtılan kaynak grubunun genel bakış bölümünde başlatın.

    ![SSH genel bakış](./media/ethereum-poa-deployment/ssh-overview.png)

2.  Bölge erişmek isteyen VM için ağ güvenlik grubu seçin

    ![SSH nsg](./media/ethereum-poa-deployment/ssh-nsg.png)

3.  Seçin \"izin ver-ssh\" kuralı

    ![SSH-izin ver](./media/ethereum-poa-deployment/ssh-allow.png)

4.  Değişiklik \"eylem\" izin vermek için

    ![SSH etkinleştir izin ver](./media/ethereum-poa-deployment/ssh-enable-allow.png)

5.  Tıklayın \"Kaydet\" (değişiklikleri uygulamak için birkaç dakika sürebilir)

Artık uzaktan SSH aracılığıyla Doğrulayıcı düğümleri için sanal makineler için sağlanan yönetici kullanıcı adı ve parolası/SSH anahtarı ile bağlanabilirsiniz.
SSH komutu ilk Doğrulayıcı düğümüne erişmek için çalışacak şekilde şablon dağıtımı çıkış parametresi olarak, listelenen ' SSH\_Kime\_ilk\_VL\_düğüm\_Bölge1 ' (örnek dağıtımı için: ssh -p 4000 poaadmin\@leader4vb.eastus.cloudapp.azure.com). Ek işlem düğümlerine almak için bağlantı noktası numarası bir artırmasına (örneğin, ilk işlem düğümünü 4000 numaralı bağlantı noktasında olur).

Birden fazla bölgeye dağıttıysanız, DNS adı veya IP adresini yük dengeleyici bu bölgede yukarıdaki komutu değiştirin. Kaynak adlandırma kuralları ile DNS adı veya diğer bölgeler IP adresini bulmak için bulma \* \* \* \* \*- lbpip reg\#ve onun DNS adı ve IP adresi özelliklerini görüntüleyin.

### <a name="azure-traffic-manager-load-balancing"></a>Azure Traffic Manager Yük Dengeleme

Azure Traffic Manager, farklı bölgelerdeki birden fazla dağıtımın gelen trafik tarafından PoA ağ yanıt verme hızını artırmak ve kapalı kalma süresinin azaltılmasına yardımcı olabilir. Yerleşik sistem durumu denetimleri ve otomatik yeniden yönlendirilmesini yüksek RPC uç noktaları ve idare DApp sağlanmasına yardımcı olur. Bu özellik, birden fazla bölgeye dağıttıysanız ve üretime hazır yararlıdır.

Traffic Manager'ı aşağıdaki amaçlarla kullanabilirsiniz:

-   Otomatik Yük devretme ile PoA ağ kullanılabilirliğini artırın.

-   En düşük ağ gecikme süresi olan Azure konumuna yönlendirme son kullanıcılar tarafından ağları yanıt hızını artırın.

Traffic Manager profili oluşturmaya karar verirseniz, ağa erişmek için profil DNS adı'nı kullanabilirsiniz. Bir kez diğer consortium üyeleri eklenmiştir ağa Traffic Manager dağıtılan kendi doğrulayıcılarının Yük Dengelemesi için de kullanılabilir.

#### <a name="creating-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Arayın ve seçin \"Traffic Manager profili\" tıkladıktan sonra \"kaynak Oluştur\" düğme Azure portalında.

![Azure traffic manager için arama yapın](./media/ethereum-poa-deployment/traffic-manager-search.png)

Profil benzersiz bir ad verin ve PoA dağıtımı sırasında oluşturduğunuz kaynak grubunu seçin. Dağıtmak için "Oluştur" düğmesine tıklayın.

![trafik Yöneticisi oluşturma](./media/ethereum-poa-deployment/traffic-manager-create.png)

Dağıtıldıktan sonra örneğinin kaynak grubunu seçin. Traffic manager erişmek için DNS adı genel bakış sekmesinde bulunabilir.

![Traffic manager DNS bulun](./media/ethereum-poa-deployment/traffic-manager-dns.png)

Uç noktalar sekmesini seçin ve Ekle düğmesine tıklayın. Uç nokta benzersiz bir ad verin. Hedef kaynak türü, genel IP adresine değiştirin. İlk bölgeye genel IP adresini seçip\'s yük dengeleyici.

![Traffic manager yönlendirme](./media/ethereum-poa-deployment/traffic-manager-routing.png)

Her bir bölgede dağıtılan ağ için yineleyin. Uç noktaları kez \"etkin\" durumu, bunlar olması otomatik olarak yüklemek ve bölge dengeli traffic manager DNS adını. Artık bu DNS adı yerine kullanabileceğiniz \[CONSORTIUM\_veri\_URL\] belgenin diğer adımları parametresi.

### <a name="data-api"></a>Veri API'si

Her consortium üye başkalarının ağa bağlanmak gerekli bilgileri barındırır. Varolan bir üye üyenin dağıtımdan önce [CONSORTIUM_DATA_URL] sağlar. Dağıtımdan sonra katılma üyesi JSON arabirimi aşağıdaki uç noktasında bilgi alır:

`<CONSORTIUM_DATA_URL>/networkinfo`

Yanıt üyeleri (Genesis blok, Doğrulayıcı ayarlandı. sözleşme abı, bootnodes) ve bilgi yararlı var olan bir üyeye (Doğrulayıcı adresleri) katılmak için faydalı bilgiler içerir. Bulut sağlayıcıları arasında consortium genişletmek için bu Standardizasyon kullanılmasını öneririz. Bu API, aşağıdaki yapıya sahip bir biçimlendirilmiş JSON yanıtı döndürür:
```json
{
  "$id": "",
  "type": "object",
  "definitions": {},
  "$schema": "https://json-schema.org/draft-07/schema#",
  "properties": {
    "majorVersion": {
      "$id": "/properties/majorVersion",
      "type": "integer",
      "title": "This schema’s major version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "minorVersion": {
      "$id": "/properties/minorVersion",
      "type": "integer",
      "title": "This schema’s minor version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "bootnodes": {
      "$id": "/properties/bootnodes",
      "type": "array",
      "items": {
        "$id": "/properties/bootnodes/items",
        "type": "string",
        "title": "This member’s bootnodes",
        "default": "",
        "examples": [
          "enode://a348586f0fb0516c19de75bf54ca930a08f1594b7202020810b72c5f8d90635189d72d8b96f306f08761d576836a6bfce112cfb6ae6a3330588260f79a3d0ecb@10.1.17.5:30300",
          "enode://2d8474289af0bb38e3600a7a481734b2ab19d4eaf719f698fe885fb239f5d33faf217a860b170e2763b67c2f18d91c41272de37ac67386f80d1de57a3d58ddf2@10.1.17.4:30300"
        ]
      }
    },
    "valSetContract": {
      "$id": "/properties/valSetContract",
      "type": "string",
      "title": "The ValidatorSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\n\nimport \"./SafeMath.sol\";\nimport \"./Utils.sol\";\n\ncontract ValidatorSet …"
      ]
    },
    "adminContract": {
      "$id": "/properties/adminContract",
      "type": "string",
      "title": "The AdminSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\nimport \"./SafeMath.sol\";\nimport \"./SimpleValidatorSet.sol\";\nimport \"./Admin.sol\";\n\ncontract AdminValidatorSet is SimpleValidatorSet { …"
      ]
    },
    "adminContractABI": {
      "$id": "/properties/adminContractABI",
      "type": "string",
      "title": "The Admin Contract ABI",
      "default": "",
      "examples": [
        "[{\"constant\":false,\"inputs\":[{\"name\":\"proposedAdminAddress\",\"type\":\"address\"},…"
      ]
    },
    "paritySpec": {
      "$id": "/properties/paritySpec",
      "type": "string",
      "title": "The Parity client spec file",
      "default": "",
      "examples": [
        "\n{\n \"name\": \"PoA\",\n \"engine\": {\n \"authorityRound\": {\n \"params\": {\n \"stepDuration\": \"2\",\n \"validators\" : {\n \"safeContract\": \"0x0000000000000000000000000000000000000006\"\n },\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\"\n }\n }\n },\n \"params\": {\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\",\n \"wasmActivationTransition\": \"0x0\"\n },\n \"genesis\": {\n \"seal\": {\n \"authorityRound\": {\n \"step\": \"0x0\",\n \"signature\": \"0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\"\n }\n },\n \"difficulty\": \"0x20000\",\n \"gasLimit\": \"0x2FAF080\"\n },\n \"accounts\": {\n \"0x0000000000000000000000000000000000000001\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ecrecover\", \"pricing\": { \"linear\": { \"base\": 3000, \"word\": 0 } } } },\n \"0x0000000000000000000000000000000000000002\": { \"balance\": \"1\", \"builtin\": { \"name\": \"sha256\", \"pricing\": { \"linear\": { \"base\": 60, \"word\": 12 } } } },\n \"0x0000000000000000000000000000000000000003\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ripemd160\", \"pricing\": { \"linear\": { \"base\": 600, \"word\": 120 } } } },\n \"0x0000000000000000000000000000000000000004\": { \"balance\": \"1\", \"builtin\": { \"name\": \"identity\", \"pricing\": { \"linear\": { \"base\": 15, \"word\": 3 } } } },\n \"0x0000000000000000000000000000000000000006\": { \"balance\": \"0\", \"constructor\" : \"…\" }\n }\n}"
      ]
    },
    "errorMessage": {
      "$id": "/properties/errorMessage",
      "type": "string",
      "title": "Error message",
      "default": "",
      "examples": [
        ""
      ]
    },
    "addressList": {
      "$id": "/properties/addressList",
      "type": "object",
      "properties": {
        "addresses": {
          "$id": "/properties/addressList/properties/addresses",
          "type": "array",
          "items": {
            "$id": "/properties/addressList/properties/addresses/items",
            "type": "string",
            "title": "This member’s validator addresses",
            "default": "",
            "examples": [
              "0x00a3cff0dccc0ecb6ae0461045e0e467cff4805f",
              "0x009ce13a7b2532cbd89b2d28cecd75f7cc8c0727"
            ]
          }
        }
      }
    }
  }
}

```
## <a name="tutorials"></a>Öğreticiler

### <a name="programmatically-interacting-with-a-smart-contract"></a>Program aracılığıyla akıllı bir sözleşme ile etkileşim kurma

> [!WARNING]
> Hiçbir zaman Ethereum özel anahtarınızı ağ üzerinden gönderin! Her işlem önce yerel olarak imzalanır ve imzalı işlem, ağ üzerinden gönderilen emin olun.

Aşağıdaki örnekte *ethereumjs Cüzdan* Ethereum adresin oluşturulacak *ethereumjs tx* yerel olarak oturum açmak için ve *web3* ham işlem göndermek için Ethereum RPC uç nokta.

Bu örnek için bu basit Hello-World akıllı sözleşme kullanacağız:

```javascript
pragma solidity ^0.4.11;
contract postBox {
    string message;
    function postMsg(string text) public {
        message = text;
    }
    function getMsg() public view returns (string) {
        return message;
    }
}
```

Bu örnek, sözleşme zaten dağıtılmış varsayar. Kullanabileceğiniz *solc* ve *web3* sözleşme programlı bir şekilde dağıtmak için. Öncelikle aşağıdaki düğüm modüllerini yükleyin:
```
sudo npm install web3@0.20.2
sudo npm install ethereumjs-tx@1.3.6
sudo npm install ethereumjs-wallet@0.6.1
```
Bu nodeJS betik aşağıdakileri gerçekleştirin:

-   Ham bir işlem oluşturun: postMsg

-   Oluşturulan özel anahtarı kullanarak işlemi oturum

-   İmzalı işlem Ethereum ağ gönderin

```javascript
var ethereumjs = require('ethereumjs-tx')
var wallet = require('ethereumjs-wallet')
var Web3 = require('web3')

// TODO Replace with your contract address
var address = "0xfe53559f5f7a77125039a993e8d5d9c2901edc58";
var abi = [{"constant": false,"inputs": [{"name": "text","type": "string"}],"name": "postMsg","outputs": [],"payable": false,"stateMutability": "nonpayable","type": "function"},{"constant": true,"inputs": [],"name": "getMsg","outputs": [{"name": "","type": "string"}],"payable": false,"stateMutability": "view","type": "function"}];

// Generate a new Ethereum account
var account = wallet.generate();
var accountAddress = account.getAddressString()
var privateKey = account.getPrivateKey();

// TODO Replace with your RPC endpoint
var web3 = new Web3(new Web3.providers.HttpProvider(
    "http://testzvdky-dns-reg1.eastus.cloudapp.azure.com:8545"));

// Get the current nonce of the account
web3.eth.getTransactionCount(accountAddress, function (err, nonce) {
   var data = web3.eth.contract(abi).at(address).postMsg.getData("Hello World");
   var rawTx = {
     nonce: nonce,
     gasPrice: '0x00',
     gasLimit: '0x2FAF080',
     to: address,
     value: '0x00',
     data: data
   }
   var tx = new ethereumjs(rawTx);

   tx.sign(privateKey);

   var raw = '0x' + tx.serialize().toString('hex');
   web3.eth.sendRawTransaction(raw, function (txErr, transactionHash) {
     console.log("TX Hash: " + transactionHash);
     console.log("Error: " + txErr);
   });
 });
```

### <a name="deploy-smart-contract-with-truffle"></a>Akıllı sözleşme Truffle ile dağıtma

-   Gerekli kitaplıkları yükleme

```javascript
npm init

npm install truffle-hdwallet-provider --save
```
-   Kod MetaMask hesabınızın kilidini açın ve PoA düğümü girdisi olarak yapılandırmak için aşağıdaki truffle.js içinde ekleyin noktası anımsatıcı ifade sağlayarak (MetaMask / ayarları / çekirdek sözcükleri Göster)

```javascript
var HDWalletProvider = require("truffle-hdwallet-provider");

var rpc_endpoint = "XXXXXX";
var mnemonic = "twelve words you can find in metamask/settings/reveal seed words";

module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*" // Match any network id
    },
    poa: {
      provider: new HDWalletProvider(mnemonic, rpc_endpoint),
      network_id: 3,
      gasPrice : 0
    }
  }
};

```

-   PoA ağa dağıtma

```javascript
$ truffle migrate --network poa
```

### <a name="debug-smart-contract-with-truffle"></a>Akıllı sözleşme Truffle ile hata ayıklama

Truffle akıllı sözleşme hata ayıklama için kullanılabilir olan bir yerel geliştirme ağı sahiptir. Tam öğretici bulabilirsiniz [burada](https://truffleframework.com/tutorials/debugging-a-smart-contract).

### <a name="webassembly-wasm-support"></a>WebAssembly (WASM) desteği

WebAssembly destek zaten sizin için yeni dağıtılan PoA ağlarda etkindir. Web-Derleme (C++ Rust, C) söz konusu transpiles herhangi bir dilde akıllı sözleşme geliştirme sağlar. Ek bilgi için aşağıdaki bağlantıları inceleyin

-   Eşlik WebAssembly bakış- <https://wiki.parity.io/WebAssembly-Home>

-   Eşlik teknik öğreticinin- <https://github.com/paritytech/pwasm-tutorial>

## <a name="reference"></a>Başvuru

### <a name="faq"></a>SSS

#### <a name="i-notice-there-are-many-transactions-on-the-network-that-i-didnt-send-where-are-these-coming-from"></a>Ağ üzerinde birçok işlem fark ederim, ı etmedi\'t Gönder. Burada bu nereden geldiğini?

Kilidini açmak için güvenli değil [kişisel API](https://web3js.readthedocs.io/en/1.0/web3-eth-personal.html). Botlar, kilidi Ethereum hesapları için dinleme ve fon boşaltma dener. Bot bu hesapların gerçek ether içerir ve ilk Bakiye siphon olmaya varsayar. Ağ üzerindeki kişisel API etkinleştirmeyin. Bunun yerine ya da bir Cüzdan MetaMask gibi veya programlama yoluyla bölümde açıklandığı gibi el ile kullanarak işlem önceden oturum [program aracılığıyla etkileşim akıllı sözleşme ile](#programmatically-interacting-with-a-smart-contract).

#### <a name="how-to-ssh-onto-a-vm"></a>Bir VM üzerinde SSH nasıl?

SSH bağlantı noktası, güvenlik nedenleriyle gösterilmez. İzleyin [SSH bağlantı noktasını etkinleştirmek için bu Kılavuzu](#ssh-access).

#### <a name="how-do-i-set-up-an-audit-member-or-transaction-nodes"></a>Bir denetim üyesi veya işlem düğümlerini nasıl ayarlayabilirim?

İşlem düğümleri, fikir birliğine varılmış içinde yer almayan ancak ağla eşlendikten eşlik istemciler kümesidir. Bu düğümler, Ethereum işlemleri gönderin ve akıllı Sözleşme durumu okumak için hala kullanılabilir.
Bu, ağ üzerinde yetkili olmayan consortium üyelerine denetlenebilirlik sağlamak için iyi bir mekanizma olarak çalışır. Elde etmek için yalnızca izleyin 2. adım Consortium büyümesini.

#### <a name="why-are-metamask-transactions-taking-a-long-time"></a>Neden MetaMask işlemleri uzun sürüyor?

İşlemler doğru sırayla alınan emin olmak için her Ethereum işlem artan bir nonce ile birlikte gelir. Farklı bir ağda MetaMask içinde bir hesap kullandıysanız, nonce değeri sıfırlamak gerekir. Ayarlar simgesine (3-çubuk) ayarları, hesap sıfırlama tıklayın. İşlem Geçmişi silinecek ve artık işlem yeniden gönderebilirsiniz.

#### <a name="do-i-need-to-specify-gas-fee-in-metamask"></a>İçinde MetaMask gaz ücret belirtmek gerekiyor mu?

Ether yetkilisi kavram consortium içinde bir amaca hizmet değil. Bu nedenle gaz ücret MetaMask işlemlerde gönderirken belirtmek için gerek yoktur.

#### <a name="what-should-i-do-if-my-deployment-fails-due-to-failure-to-provision-azure-oms"></a>Azure OMS sağlama hatası nedeniyle dağıtımım başarısız olursa ne yapmalıyım?

İzleme isteğe bağlı bir özelliktir. Bazı nadir durumlarda burada bağlanamama başarıyla Azure İzleyici kaynak sağlama nedeniyle dağıtım başarısız Azure İzleyici yeniden dağıtabilirsiniz.

#### <a name="are-public-ip-deployments-compatible-with-private-network-deployments"></a>Genel IP dağıtımları özel ağ dağıtımları ile uyumludur?

Hayır, eşleme tüm ağ ya da genel veya özel olmalıdır böylece iki yönlü iletişim gerektirir.

#### <a name="what-is-the-expected-transaction-throughput-of-proof-of-authority"></a>Beklenen işlem aktarım hızı yetkilisi kavram nedir?

İşlem aktarım hızı, işlem ve ağ topolojisi türleri üzerinde yüksek oranda bağımlı olacaktır.  Basit işlemler kullanarak, biz 400 saniyede ortalama birden fazla bölgede dağıtılan bir ağla benchmarked.

#### <a name="how-do-i-subscribe-to-smart-contract-events"></a>Akıllı sözleşme olaylarına nasıl abone olabilirim?

Ethereum kavram yetki artık web yuvalarını destekler.  Dağıtım e-posta veya dağıtım web yuvası URL ve bağlantı noktasını bulmak için çıkışı denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [Ethereum kavram yetki Consortium](https://portal.azure.com/?pub_source=email&pub_status=success#create/microsoft-azure-blockchain.azure-blockchain-ethereumethereum-poa-consortium) çözüm.
