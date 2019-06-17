---
title: Azure'da Hyperledger Fabric Konsorsiyum ağı
description: Bir Hyperledger Fabric Konsorsiyum ağı yapılandırmak ve dağıtmak için çözüm şablonu
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/09/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: caleteet
manager: femila
ms.openlocfilehash: 80de4e1479fac7296889e45289a5f20e586e3f57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65510761"
---
# <a name="hyperledger-fabric-consortium-network"></a>Hyperledger Fabric Konsorsiyum ağı

Hyperledger Fabric consortium çözüm şablonu, dağıtmak ve Azure üzerinde bir Hyperledger Fabric Konsorsiyum ağı yapılandırmak için kullanabilirsiniz.

Bu makaleyi okuduktan sonra şunları yapabilir olacaksınız:

- Blok zinciri, Hyperledger Fabric ve daha karmaşık bir konsorsiyum ağı mimarileri elde
- Bir Hyperledger Fabric consortium ağdan Azure Portal'ı yapılandırmak ve dağıtmak hakkında bilgi edinin

## <a name="about-blockchain"></a>Blockchain hakkında

Blok zinciri topluluğuna yeniyseniz, bu çözüm şablonu kolay ve yapılandırılabilir bir şekilde azure'da teknolojisi hakkında bilgi edinmek için harika bir fırsattır. Blok zinciri Bitcoin arkasındaki temel teknolojidir; Ancak, sanal bir para birimi Etkinleştirici çok daha fazlasına olur. Bu, güvenli çok taraflı hesaplama saldırılara karşı değiştirilemezlik, doğrulanabilirliğini denetler, denetlenebilirlik ve dayanıklılık Garantisi ile sağlayan bileşik bir var olan veritabanı, dağıtılmış sisteme ve şifreleme teknolojileri olur. Farklı protokollere bu öznitelikler sağlamak için farklı mekanizmaları. [Hyperledger Fabric](https://github.com/hyperledger/fabric) böyle bir protokoldür.

## <a name="consortium-architecture-on-azure"></a>Azure'da Consortium mimarisi

Azure'da Hyperledger Fabric etkinleştirmek için desteklenen iki birincil dağıtım türleri vardır. Bu dağıtımlar, istenen hedef göre farklı topolojileri uyum sağlayacak şekilde tasarlanmıştır.

- **Tek sanal makine, geliştirici sunucu** -bu dağıtım türü oluşturmak ve Hyperledger Fabric üzerinde derlenmiş çözümlerini test etmek için kullanılan bir geliştirme ortamı olarak tasarlanmıştır.
- **Birden çok sanal makine dağıtımını ölçeklendirmek** -bu dağıtım türü farklı katılımcıları paylaşılan bir ortamda yararlanarak bir model ortamlar için tasarlanmıştır.

Her iki dağıtımda Hyperledger Fabric temelini oluşturan yapı taşları aynıdır.  Fark dağıtımlarda bu bileşenler kullanıma nasıl ölçeklendirilir vardır.

- **CA düğümleri**: Ağdaki kimlikleri için kullanılan sertifikaları oluşturmak için kullanılan sertifika yetkilisini çalıştıran bir düğümdür.
- **Sipariş eden düğümler**: Toplam sırasını yayın veya atomik işlemler gibi bir teslim garantisi uygulama iletişim hizmetini çalıştıran bir düğümdür.
- **Eş düğümleri**: Hareketlerini tamamlar ve durumu ve dağıtılmış kayıt defteri bir kopyasını tutan bir düğümdür.
- **CouchDB düğümleri**: CouchDB hizmeti çalıştıran bir düğüm, durumu veritabanını tutun ve basit anahtar/değer JSON türü depolama birimine genişletme chaincode verilerin zengin sorgulama sağlayın.

### <a name="single-virtual-machine-architecture"></a>Tek sanal makine mimarisi

Tek sanal daha önce belirtildiği gibi makine mimarisi uygulamalar geliştirmek için kullanılan bir az alan kaplaması sunucu, geliştiriciler için oluşturulmuştur. Gösterilen tüm kapsayıcıları tek bir sanal makinede çalışıyor. Sıralama hizmetinin [SOLO](https://github.com/hyperledger/fabric/tree/master/orderer) bu yapılandırma için. Bu yapılandırma *değil* sıralama bir hataya dayanıklı hizmet ancak geliştirme amacıyla basit olacak şekilde tasarlanmıştır.

![Tek sanal makine mimarisi](./media/hyperledger-fabric-consortium-blockchain/hlf-single-arch.png)

### <a name="multiple-virtual-machine-architecture"></a>Birden çok sanal makine mimarisi

Birden çok sanal makine ölçek genişletmeli mimarisinden yerleşik yüksek kullanılabilirlik ve ölçeklendirme her bileşenin faydalanabilirsiniz. Bu mimari, üretim sınıf dağıtımları için daha uygundur.

![Birden çok sanal makine mimarisi](./media/hyperledger-fabric-consortium-blockchain/hlf-multi-arch.png)

## <a name="getting-started"></a>Başlarken

Başlamak için birkaç sanal makineler ve standart depolama hesapları dağıtımı destekleyen bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/).

Abonelik aldıktan sonra Git [Azure portalında](https://portal.azure.com). Seçin **kaynak Oluştur > blok zinciri > Hyperledger Fabric Consortium**.

![Hyperledger Fabric tek üye Blockchain Market şablonu](./media/hyperledger-fabric-consortium-blockchain/marketplace-template.png)

## <a name="deployment"></a>Dağıtım

İçinde **Hyperledger Fabric Consortium** şablon seçme **Oluştur**.

Şablon dağıtımı, çok düğümlü nasıl yapılandıracağınız anlatılmaktadır [Hyperledger 1.3](https://hyperledger-fabric.readthedocs.io/en/release-1.3/) ağ. Dağıtım akışı dört adımlarına ayrılmıştır: Temel bilgileri, Consortium ağ ayarları, doku yapılandırması ve isteğe bağlı bileşenler.

### <a name="basics"></a>Temel Bilgiler

İçinde **Temelleri**, herhangi bir dağıtım için standart parametreler için değerler belirtin. Özellikleri gibi abonelik, kaynak grubu ve temel sanal makine.

![Temel Bilgiler](./media/hyperledger-fabric-consortium-blockchain/basics.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Kaynak ön eki** | Dağıtımın bir parçası sağlanan kaynakları için adı ön eki |6 karakter veya daha az |
**Kullanıcı Adı** | Bu üye için dağıtılan sanal makinelerin her biri için bir yönetici kullanıcı adı |1 - 64 karakter |
**Kimlik doğrulaması türü** | Sanal makinenin kimliğini doğrulamak için yöntemi |Parola veya SSH ortak anahtarı|
**Parola (kimlik doğrulaması türü = parola)** |Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Parola şu karakter türlerinin üç içermelidir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter<br /><br />Tüm VM'lerin aynı parolayı başlangıçta olsa da, sağladıktan sonra parolayı değiştirebilirsiniz|12 - 72 karakter|
**SSH anahtarı (kimlik doğrulaması türü SSH ortak anahtarı =)** |Uzaktan oturum açma için kullanılan güvenli Kabuk anahtarı ||
**Abonelik** |Dağıtmak istediğiniz abonelik ||
**Kaynak grubu** |Bir kaynak grubuna Konsorsiyum ağı dağıtmak için ||
**Konum** |İlk üye dağıtılacağı Azure bölgesi ||

**Tamam**’ı seçin.

### <a name="consortium-network-settings"></a>Consortium ağ ayarları

İçinde **ağ ayarları**, oluşturmaya yönelik girişleri belirtin veya var olan bir konsorsiyum birleştirme, ağ ve kuruluş ayarlarınızı yapılandırın.

![Consortium ağ ayarları](./media/hyperledger-fabric-consortium-blockchain/network-settings.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Ağ yapılandırması** |Yeni ağ oluşturma veya mevcut bir katılmak seçebilirsiniz. Seçerseniz *varolan katılın*, ek değerler sağlamanız gerekir. |Yeni ağ <br/> Varolan katılın |
**HLF CA parola** |Dağıtımın bir parçası oluşturulan sertifika yetkilisi tarafından oluşturulan sertifikaları için kullanılan parola. Parola şu karakter türlerinin üç içermelidir: 1 büyük harf karakter, 1 küçük harf, 1 sayı ve 1 özel karakter.<br /><br />Tüm sanal makineler aynı parolayı başlangıçta olsa da, parola sağladıktan sonra değiştirebilirsiniz.|1 - 25 karakter |
**Kuruluş Kurulumu** |Kuruluşunuzun adı ve sertifika özelleştirebilir ya da kullanılacak varsayılan değerlere sahip.|Varsayılan <br/> Gelişmiş |
**VPN ağ ayarları** | Sanal makinelere erişmek için VPN tünel ağ geçidi sağlama | Evet <br/> Hayır |

**Tamam**’ı seçin.

### <a name="fabric-specific-settings"></a>Özel Yapı ayarları

İçinde **doku yapılandırma**, ağ boyutunu ve performansını yapılandırın ve ağ kullanılabilirliğini girişleri belirtin. Sipariş eden ve eş düğümleri, her düğüm ve sanal makine boyutu tarafından kullanılan bir Kalıcılık altyapısı gibi numarası.

![Yapı ayarları](./media/hyperledger-fabric-consortium-blockchain/fabric-specific-settings.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Ölçek türü** |Tek bir sanal makine ile birden çok kapsayıcı ya da birden çok sanal makine ölçek genişletme modeli dağıtım türü.|Tek bir VM veya çoklu VM |
**VM Disk türü** |Dağıtılan düğümler yedekleme depolama türü. <br/> Kullanılabilir disk türleri hakkında daha fazla bilgi edinmek için [bir disk türü seçin](../../virtual-machines/windows/disks-types.md).|Standart SSD <br/> Premium SSD |

### <a name="multiple-vm-deployment-additional-settings"></a>Birden çok VM dağıtımı (ek ayarları)

![Yapı ayarları için birden çok vm dağıtımları](./media/hyperledger-fabric-consortium-blockchain/multiple-vm-deployment.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Sipariş eden düğüm sayısı** |Sipariş düğüm sayısını (düzenleme) bir blok işlemleri. <br />Sıralama hizmeti hakkında ek ayrıntılar için Hyperledger ziyaret [belgeleri](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html) |1-4 |
**Sipariş eden düğüm sanal makine boyutu** |Ağ sipariş eden düğümler için kullanılan sanal makine boyutu|Standart Bs<br />Standart Ds<br />Standart FS |
**Eş düğüm sayısı** | İşlemleri ve Bakım durumu ve muhasebe bir kopyasını consortium üyeleri tarafından sahip olunan düğümleri.<br />Sıralama hizmeti hakkında ek ayrıntılar için Hyperledger ziyaret [belgeleri](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|1-4 |
**Düğüm durumu kalıcılığını** |Eş düğümleri tarafından kullanılan Kalıcılık altyapısı. Bu altyapı Eş düğüm başına yapılandırabilirsiniz. Birden çok eş düğümleri için aşağıdaki ayrıntıları bakın.|CouchDB <br />LevelDB |
**Eş düğüm sanal makine boyutu** |Ağdaki tüm düğümler için kullanılan sanal makine boyutu|Standart Bs<br />Standart Ds<br />Standart FS |

### <a name="multiple-peer-node-configuration"></a>Birden çok eş düğüm yapılandırması

Bu şablon, Kalıcılık altyapınız Eş düğüm başına seçmenize olanak sağlar. Üç eş düğüm varsa, örneğin, CouchDB birini kullanabilirsiniz ve diğer iki üzerinde LevelDB.

![Birden çok eş düğüm yapılandırması](./media/hyperledger-fabric-consortium-blockchain/multiple-peer-nodes.png)

**Tamam**’ı seçin.

### <a name="deploy"></a>Dağıtma

İçinde **özeti**, belirtilen girişler gözden geçirin ve temel dağıtım öncesi doğrulama çalıştırmak için.

![Özet](./media/hyperledger-fabric-consortium-blockchain/summary.png)

Yasal ve gizlilik koşulları gözden geçirin ve seçin **satın alma** dağıtılacak. VM'ler sağlanırken sayısına bağlı olarak birkaç dakika veya on dakika için dağıtım süresi farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama ve chaincode geliştirme Hyperledger consortium blockchain ağınıza karşı odaklanmak artık hazırsınız.
