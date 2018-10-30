---
title: Hyperledger Yapı Konsorsiyumu
description: Dağıtma ve yapılandırma bir tek üyeye ağ Hyperledger Fabric Consortium çözüm şablonu kullanın
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/29/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: coborn
manager: femila
ms.openlocfilehash: c08557156848d4e7fcf0b1adbe6c8faa4ee00c82
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50231381"
---
# <a name="hyperledger-fabric-single-member-network"></a>Hyperledger Fabric tek üye ağ

Bir Hyperledger Fabric tek üyesi (çok düğümlü) ağ yapılandırmak ve dağıtmak Hyperledger Fabric Consortium çözüm şablonu kullanabilirsiniz.

Bu makaleyi okuduktan sonra şunları yapabilir olacaksınız:

- Blok zinciri, Hyperledger Fabric ve daha karmaşık bir konsorsiyum ağı mimarileri elde
- Tek üyeli Hyperledger Fabric consortium ağdan Azure Portal'ı yapılandırmak ve dağıtmak hakkında bilgi edinin

## <a name="about-blockchain"></a>Blockchain hakkında

Blok zinciri topluluğuna yeniyseniz, bu çözüm şablonu kolay ve yapılandırılabilir bir şekilde azure'da teknolojisi hakkında bilgi edinmek için harika bir fırsattır. Blok zinciri Bitcoin arkasındaki temel teknolojidir; Ancak, sanal bir para birimi Etkinleştirici çok daha fazlasına olur. Bu, güvenli çok taraflı hesaplama saldırılara karşı değiştirilemezlik, doğrulanabilirliğini denetler, denetlenebilirlik ve dayanıklılık Garantisi ile sağlayan bileşik bir var olan veritabanı, dağıtılmış sisteme ve şifreleme teknolojileri olur. Farklı protokollere bu öznitelikler sağlamak için farklı mekanizmaları. [Hyperledger Fabric](https://github.com/hyperledger/fabric) böyle bir protokoldür.

## <a name="consortium-architecture-on-azure"></a>Azure'da Consortium mimarisi

Bu şablon test ve üretim için tek bir kullanıcı benzetimini yardımcı olmak için topoloji dağıtır kuruluş (tek üyesi). Bu dağıtım, birden fazla bölgeye yakında genişletilecek tek bir bölgede bir çok düğümlü ağ oluşur.

Ağ düğüm üç tür oluşur:

1. **Üye düğümünü**: kaydeder ve ağ üyeleri yöneten Fabric üyelik hizmeti çalıştıran bir düğüm. Bu düğüm, ölçeklenebilirlik ve yüksek kullanılabilirlik için kümelenmiş; Ancak bu laboratuvarda, bir tek üyeye düğümü kullanılır.
2. **Sipariş eden düğümler**: toplam gibi bir teslim garantisi uygulama iletişim hizmetini çalıştıran bir düğüm sipariş yayın veya atomik işlemler.
3. **Eş düğümleri**: hareketlerini tamamlar ve durumu ve dağıtılmış kayıt defteri bir kopyasını tutan bir düğümü.

## <a name="getting-started"></a>Başlarken

Başlamak için birkaç sanal makineler ve standart depolama hesapları dağıtımı destekleyen bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/).

Varsayılan olarak, Kotayı artırmak gerek kalmadan, küçük dağıtım topolojisi çoğu abonelik türlerini destekler. Bir üye için olası en küçük dağıtım gerekir:

- 5 sanal makineler (5 çekirdek)
- 1 VNet
- 1 yük dengeleyici
- genel IP adresi 1

Abonelik aldıktan sonra Git [Azure portalında](https://portal.azure.com). Seçin **+** seçin **blok zinciri**seçip **Hyperledger Fabric tek üye Blockchain**.

![Hyperledger Fabric tek üye Blockchain Market şablonu](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Dağıtım

Başlatmak için **Hyperledger Fabric tek üye Blockchain** tıklatıp **Oluştur** açmak için **Temelleri** dikey Sihirbazı'nda.

Şablon dağıtımı birden çok düğümlü ağ yapılandırması için size yol gösterir. Dağıtım akışı üç adımlamayla ayrılmıştır: temel ağ yapılandırması ve yapı yapılandırması.

### <a name="basics"></a>Temel Bilgiler

İçinde **Temelleri** dikey penceresinde herhangi bir dağıtım için standart parametreler için değerler belirtin. Özellikleri gibi abonelik, kaynak grubu ve temel sanal makine.

![Temel Bilgiler](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Parametre Adı| Açıklama| İzin Verilen Değerler|Varsayılan Değer
---|---|---|---
**Kaynak ön eki**| Temel olarak dağıtılan kaynaklar adlandırmak için kullanılan bir dize.|6 karakter veya daha az|NA
**VM kullanıcı adı**| Bu üye için dağıtılan sanal makinelerin her biri için yönetici kullanıcı adı.|1 - 64 karakter|azureuser
**Kimlik doğrulaması türü**| Sanal makinenin kimliğini doğrulamak için yöntem.|Parola veya SSH ortak anahtarı|Parola
**Parola (kimlik doğrulaması türü = parola)**|Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Parola şu karakter türlerinin üç içermelidir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter.<br /><br />Tüm VM'lerin aynı parolayı başlangıçta olsa da, parola sağladıktan sonra değiştirebilirsiniz.|12 - 72 karakter|NA
**SSH anahtarı (kimlik doğrulaması türü ortak anahtar =)**|Uzaktan oturum açma için kullanılan güvenli Kabuk anahtarı.||NA
**IP adresine göre erişimi kısıtlama**|Ayar türü, istemci uç noktası erişimi kısıtlı olup olmadığını belirlemek için.|Evet/Hayır| Hayır
**IP adresi veya alt ağı izin (IP adresine göre erişimi kısıtlama = Yes)**|IP adresi veya erişim denetimi etkinleştirildiğinde istemci uç noktasına erişmek için izin verilen IP adresleri kümesi.||NA
**Abonelik** |Dağıtımın yapılacağı abonelik.
**Kaynak Grubu** |Hangi Konsorsiyum ağı dağıtmak kaynak grubu.||NA
**Konum** |İlk üye dağıtılacağı Azure bölgesi ** s ağ kaplama alanını.

### <a name="network-size-and-performance"></a>Ağ boyutu ve performansı

Ardından **boyutu ve performansı, ağ** girişleri Konsorsiyum ağı boyutunu belirtin. Üyelik, sipariş eden ve eş düğümleri sayısı gibi. Farklı altyapı seçeneklerini ve sanal makine boyutu seçin.

![Ağ boyutu ve performansı](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Parametre Adı| Açıklama| İzin Verilen Değerler|Varsayılan Değer
---|---|---|---
**Üyelik düğüm sayısı**|Üyelik hizmeti çalışan düğüm sayısı. Üyelik hizmeti ile ilgili ek ayrıntılar için güvenlik ve Üyelik Hizmetleri Hyperledger altında bakmak [belgeleri](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf).<br /><br />Bu değer 1 düğüm için şu anda sınırlıdır, ancak sonraki düzeltmede kümeleme aracılığıyla, ölçeği genişletme desteklemeyi planlıyoruz.|1| 1
**Sipariş eden düğüm sayısı** |Sipariş düğüm sayısını (düzenleme) bu işlemleri engelle.--> nefesiniz ve en kafa karıştırıcı deyimi. Sıralama hizmeti hakkında ek ayrıntılar için Hyperledger ziyaret [belgeleri](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html).<br /><br />Bu değer 1 düğüme kısıtlı olur. |1 |1
**Eş düğüm sayısı**| İşlemleri ve Bakım durumu ve muhasebe bir kopyasını consortium üyeleri tarafından sahip olunan düğümleri.<br /><br />Sıralama hizmeti hakkında ek ayrıntılar için Hyperledger ziyaret [belgeleri](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|3| 3 - 9
**Depolama performansı**|Dağıtılan düğümler yedekleme depolama türü. Depolama hakkında daha fazla bilgi edinmek için [Microsoft Azure Storage'a giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction) ve [Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Standart veya Premium|Standart
**Sanal makine boyutu** |Ağdaki tüm düğümler için kullanılan sanal makine boyutu|Standart A<br />Standart D<br />Standart D v2<br />Standart F serisi<br />Standart DS<br />ve standart FS|Standart D1_v2

### <a name="fabric-specific-settings"></a>Özel Yapı ayarları

Son olarak, altında **yapı ayarları**, yapı ile ilgili yapılandırma ayarlarını belirtin.

![Yapı ayarları](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Parametre Adı| Açıklama| İzin Verilen Değerler|Varsayılan Değer
---|---|---|---
**Önyükleme kullanıcı adı**| İlk üye dağıtılan ağ hizmetleri ile kayıtlı kullanıcısı yetkilendirildi.|9 veya daha az karakter|Yönetici
**Fabric CA için önyükleme kullanıcı parolası**|Üyelik düğümüne alınan doku CA hesabının güvenliğini sağlamak için kullanılan yönetici parolası.<br /><br />Parola bir büyük harf karakter, bir küçük harf ve bir sayı içermelidir.|en az 12 karakter|NA

### <a name="deploy"></a>Dağıtma

İçinde **özeti**, belirtilen girişler gözden geçirin ve temel dağıtım öncesi doğrulama çalıştırmak için.

![Özet](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Yasal ve gizlilik koşulları gözden geçirin ve tıklayın **satın alma** dağıtılacak. VM'ler sağlanırken sayısına bağlı olarak birkaç dakika veya on dakika için dağıtım süresi farklılık gösterebilir.

### <a name="accessing-nodes"></a>Accessing düğümleri

Dağıtım tamamlandıktan sonra bir **genel bakış** görüntülenir.

![Dağıtımlar](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Ekran otomatik olarak görünmüyorsa (belki de Yönetim Portalı sırasında geçici bir çözüm taşınmış olduğundan dağıtım çalışıyordu), her zaman, sol taraftaki gezinti çubuğunda kaynak gruplar sekmesinde bulabilirsiniz. Girdiğiniz gitmek için 1. adımda kaynak grubu adı tıklayın **genel bakış** sayfası.

Genel Bakış çözüm şablonu tarafından dağıtılan kaynakların tümünü listeler. Dilediğiniz zaman keşfedin, ancak bu ekrandan da erişebilirsiniz _çıkış parametresi_ şablon tarafından oluşturulur. Bu çıktı parametreleri Hyperledger Fabric ağa bağlanırken yararlı bilgiler verir.

Çıkış parametreleri erişmek için öncelikle tıklayarak **dağıtımları** kaynak grubu dikey penceresinde sekme. Dağıtım geçmişi görüntülenir.

![Dağıtım geçmişi](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

Dağıtım geçmişinden ayrıntılara bakmak için listedeki ilk dağıtım tıklayın.

![Dağıtım ayrıntıları](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

Ayrıntılar ekranını ve ardından üç yararlı çıktıyı parametrelerle, dağıtımın bir özeti gösterilir:

- _API uç noktası_ ağı üzerinde bir uygulamayı dağıttığınızda kullanılabilir.
- _ÖNEK_ ayrıca adlı _dağıtım ön eki_ , benzersiz olarak kaynaklarınızı ve dağıtım tanımlar. Komut satırı tabanlı araçlar kullanıldığında kullanılır.
- _SSH için ilk VM_ sağlar, önceden oluşturulmuş bir ssh komutu ağınızdaki; ilk VM bağlanmak için gereken doğru parametrelere sahip Fabric CA düğüm Hyperledger Fabric için de artar.

Belirtilen yönetici kullanıcı adı ve parolası/SSH anahtarı ile sanal makineler SSH aracılığıyla her düğüm için uzaktan bağlanabilir. Kendi genel IP adresleri düğümünü sanal makinelere sahip olduğundan, yük dengeleyici üzerinden gidin ve bağlantı noktası numarasını belirtmeniz gerekir. Üçüncü şablon çıktısı, ilk işlem düğümüne erişmek için SSH komutu şöyledir ** SSH için ilk VM (için örnek dağıtımı: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Ek işlem düğümlerine almak için bağlantı noktası numarası bir artırmasına (örneğin, bağlantı noktası 3000, ilk işlem düğümü olan ikinci 3001, üçüncü 3002 üzerinde vs.).

## <a name="next-steps"></a>Sonraki adımlar

Uygulama ve chaincode geliştirme Hyperledger consortium blockchain ağınıza karşı odaklanmak artık hazırsınız.
