---
title: Hyperledger doku Konsorsiyumu
description: Bir tek üye ağ yapılandırmak ve dağıtmak Hyperledger doku Konsorsiyumu çözüm şablonu kullanın
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/21/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 7b60c086896506e5883607db48a64d2a2efbd967
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655451"
---
# <a name="hyperledger-fabric-single-member-network"></a>Hyperledger doku tek üye ağ

Hyperledger doku tek üye (çok düğümlü) ağ yapılandırmak ve dağıtmak Hyperledger doku Konsorsiyumu çözüm şablonu kullanabilirsiniz.

Bu makaleyi okuduktan sonra şunları yapabilir olacaksınız:

- Ağ mimarisi blockchain, Hyperledger doku ve daha karmaşık Konsorsiyumu alın
- Tek üye Hyperledger doku Konsorsiyumu ağdan Azure portalı içinde yapılandırmak ve dağıtmak hakkında bilgi edinin

## <a name="about-blockchain"></a>Blockchain hakkında

Blockchain topluluğuna yeniyseniz, kolay ve yapılandırılabilir bir şekilde Azure üzerinde teknolojisi hakkında bilgi edinmek için harika bir fırsat budur. Blockchain Bitcoin ardındaki temel teknolojidir; Ancak, sanal bir para birimi Etkinleştirici çok daha fazlasına olur. Güvenli çok kişili hesaplama saldırmak için garanti girişi, verifiability, auditability ve dayanıklılık çevresinde ile sağlayan bileşik var olan veritabanı, Dağıtılmış Sistem ve şifreleme teknolojileri olur. Farklı protokollere bu öznitelikler sağlamak için farklı mekanizmaları. [Hyperledger doku](https://github.com/hyperledger/fabric) bu tür bir protokol.

## <a name="consortium-architecture-on-azure"></a>Azure üzerinde Konsorsiyumu mimarisi

Bu şablon test ve üretim tek bir kullanıcı için benzetimini yardımcı olmak için topoloji dağıtır kuruluş (tek üye). Bu dağıtım için birden çok bölgeye en kısa sürede genişletilecek tek bir bölgede bir çok düğümlü ağının oluşur.

Ağ düğümlerinin üç tür oluşmaktadır:

1. **Üye düğümü**: kaydeder ve ağ üyeleri yöneten doku üyelik hizmetini çalıştıran bir düğüm. Bu laboratuvarda tek üyesi düğümü kullanılır ancak bu düğüm sonunda ölçeklenebilirlik ve yüksek kullanılabilirlik için kümelenebilir.
2. **Sipariş eden düğümleri**: toplam gibi bir teslim garantisi uygulama iletişim hizmetini çalıştıran bir düğüm sipariş yayın veya atomik işlemleri.
3. **Eş düğümleri**: işlemleri kaydeder ve durumu ve dağıtılmış defter kopyasını tutar bir düğüm.

## <a name="getting-started"></a>Başlarken

Başlamak için birkaç sanal makinelerin ve standart depolama hesapları dağıtımı destekleyen bir Azure aboneliği gerekir. Bir Azure aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/).

Varsayılan olarak, Kotayı artırmak gerek kalmadan, bir küçük dağıtım topolojisi çoğu abonelik türlerini destekler. En küçük olası dağıtım için bir üyesi olmanız gerekir:

- 5 sanal makineleri (5 çekirdek)
- 1 sanal ağ
- 1 yük dengeleyici
- 1 genel IP adresi

Bir abonelik aldıktan sonra Git [Azure portal](https://portal.azure.com). Seçin **+** seçin **Blockchain**seçip **Hyperledger doku tek üye Blockchain**.

![Hyperledger doku tek üye Blockchain Market şablonu](./media/hyperledger-fabric-single-member-blockchain/marketplace-template.png)

## <a name="deployment"></a>Dağıtım

Başlatmak için **Hyperledger doku tek üye Blockchain** tıklatıp **oluşturma**. Bu açılır **Temelleri** dikey Sihirbazı'nda.

Şablon dağıtımı çok düğümlü ağ yapılandırması için size yol gösterir. Dağıtım akışı üç adımı ayrılmıştır: temel bilgileri, ağ yapılandırması ve yapı yapılandırma.

### <a name="basics"></a>Temel Bilgiler

Altında **Temelleri** dikey penceresinde, abonelik, kaynak grubu ve temel sanal makine özellikleri gibi herhangi bir dağıtım için standart parametrelerin değerlerini belirtin.

![Temel Bilgiler](./media/hyperledger-fabric-single-member-blockchain/basics.png)

Parametre Adı| Açıklama| İzin Verilen Değerler|Varsayılan Değer
---|---|---|---
**Kaynak öneki**| Temel olarak dağıtılan kaynakları adlandırmak için kullanılan bir dize.|6 karakter veya daha az|NA
**VM kullanıcı adı**| Bu üye için dağıtılan sanal makinelerin her yönetici kullanıcı adı.|1 - 64 karakter|azureuser
**Kimlik doğrulama türü**| Sanal makine kimliğini doğrulamak için yöntem.|Parola veya SSH ortak anahtarı|Parola
**Parola (kimlik doğrulama türü = parola)**|Dağıtılan sanal makinelerin her biri için yönetici hesabı için parola. Parola şunlardan 3 tanesini içermelidir: 1 büyük harf, 1 küçük harf, 1 sayı ve 1 özel karakter.<br /><br />Tüm sanal makineleri aynı parolayı başlangıçta olmakla birlikte, sağlama işlemi sonrası parolasını değiştirebilirsiniz.|12 - 72 karakter|NA
**SSH anahtarı (kimlik doğrulama türü = ortak anahtar)**|Uzak oturum açma için kullanılan güvenli Kabuk anahtarı.||NA
**IP adresine göre erişimi kısıtlama**|İstemci uç noktası erişimi kısıtlanmış olsun veya olmasın türünü belirlemek için ayarlama.|Evet/Hayır| Hayır
**IP adresi veya alt ağı izin (IP adresi ile erişimi kısıtlamak = Yes)**|IP adresi veya erişim denetimi etkinleştirildiğinde, istemci uç noktasına erişmek için izin verilen IP adresleri kümesi.||NA
**Abonelik** |Dağıtılacağı abonelik.
**Kaynak Grubu** |Kaynak grubu Konsorsiyumu ağ dağıtılacağı.||NA
**Konum** |İlk üye dağıtılacağı Azure bölgesi ** s ağ kaplama alanını.

### <a name="network-size-and-performance"></a>Ağ boyutu ve performans

Sonraki altında **ağ boyutu ve performans,** girişleri üyeliği, sipariş eden ve eş düğümleri sayısı gibi Konsorsiyumu ağ boyutunu belirtin. Altyapı seçenekleri ve sanal makine boyutu seçin.

![Ağ boyutu ve performans](./media/hyperledger-fabric-single-member-blockchain/network-size-performance.png)

Parametre Adı| Açıklama| İzin Verilen Değerler|Varsayılan Değer
---|---|---|---
**Üyelik düğüm sayısı**|Üyelik hizmeti çalışan düğüm sayısı. Güvenlik ve Üyelik Hizmetleri Hyperledger altında üyelik hizmeti hakkında daha fazla ayrıntı için bakmak [belgelerine](https://media.readthedocs.org/pdf/hyperledger-fabric/latest/hyperledger-fabric.pdf).<br /><br />Bu değer 1 düğümü şu anda kısıtlı olur, ancak sonraki düzeltmede kümeleme aracılığıyla genişletme desteği planlıyoruz.|1| 1
**Sipariş eden düğüm sayısı** |Sipariş düğüm sayısı (düzenleme) işlemlere blok. Bu--> deyimi yalnızca sözcüklerden oluşan yapıların ve karmaşık. Sıralama hizmeti hakkında daha fazla ayrıntı için Hyperledger ziyaret [belgelerine](http://hyperledger-fabric.readthedocs.io/en/latest/orderingservice.html).<br /><br />Bu değer 1 düğümü şu anda kısıtlı olur. |1 |1
**Eş düğüm sayısı**| İşlemleri yürütmek ve durumu ve muhasebe kopyasını korumak Konsorsiyumu üyeleri tarafından sahip olunan düğümleri.<br /><br />Sıralama hizmeti hakkında daha fazla ayrıntı için Hyperledger ziyaret [belgelerine](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html).|3| 3 - 9
**Depolama performansı**|Dağıtılan düğümlerin her birinde yedekleme depolama türü. Depolama hakkında daha fazla bilgi için [Microsoft Azure Storage'a giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction) ve [Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage).|Standart veya Premium|Standart
**Sanal makine boyutu** |Ağdaki tüm düğümler için kullanılan sanal makine boyutu|Standart bir<br />Standart D<br />Standart D v2<br />Standart F serisi<br />Standart DS<br />ve standart FS|Standart D1_v2

### <a name="fabric-specific-settings"></a>Yapı belirli ayarları

Son olarak, altında **yapı ayarları**, yapı ile ilgili yapılandırma ayarlarını belirtin.

![Yapı ayarları](./media/hyperledger-fabric-single-member-blockchain/fabric-settings.png)

Parametre Adı| Açıklama| İzin Verilen Değerler|Varsayılan Değer
---|---|---|---
**Önyükleme kullanıcı adı**| Dağıtılan ağ Üye Hizmetleri ile kayıtlı ilk yetkili kullanıcı.|9 veya daha az karakter|Yönetici
**Doku CA için önyükleme kullanıcı parolası**|Üyelik düğümüne alınan doku CA hesabı güvenliğini sağlamak için kullanılan yönetici parolası.<br /><br />Parola, bir büyük harf karakter, bir küçük harf karakter ve bir rakam içermelidir.|12 veya daha fazla karakter|NA

### <a name="deploy"></a>Dağıtma

İçinde **Özet**, belirtilen girişleri gözden geçirin ve temel dağıtım öncesi doğrulama çalıştırmak için.

![Özet](./media/hyperledger-fabric-single-member-blockchain/summary.png)

Yasal ve gizlilik koşullarını gözden geçirin ve tıklatın **satın alma** dağıtmak için. Sağlanan VMs sayısına bağlı olarak, dağıtım süresini dakika onlarca için birkaç dakika arasında değişebilir.

### <a name="accessing-nodes"></a>Düğümlere erişme

Dağıtım tamamlandıktan sonra bir **genel bakış** görüntülenir.

![Dağıtımlar](./media/hyperledger-fabric-single-member-blockchain/deployments.png)

Ekran otomatik olarak görünmüyorsa (belki de Yönetim Portalı geçici taşınmış olduğundan dağıtım çalışıyordu), her zaman, sol taraftaki gezinti çubuğu kaynak grupları sekmesinde bulabilirsiniz. Girdiğiniz gitmek için 1. adımda kaynak grubu adı tıklatın **genel bakış** sayfası.

Genel Bakış çözüm şablonu tarafından dağıtılan kaynakların tümünü listeler. Gerçekleştirilse keşfedebilirsiniz, ancak bu ekranda da erişebilirsiniz _çıktı parametreleri_ şablon tarafından oluşturulur. Bu çıktı parametreleri Hyperledger doku ağınıza bağlanırken yararlı bilgiler verir.

Çıkış parametreleri erişmek için öncelikle tıklayın **dağıtımları** kaynak grubu dikey sekmesindedir. Dağıtım geçmişi görüntülenir.

![Dağıtım geçmişi](./media/hyperledger-fabric-single-member-blockchain/deployment-history.png)

Dağıtım geçmişinden ayrıntılara bakmak için listedeki ilk dağıtım'a tıklayın.

![Dağıtım ayrıntıları](./media/hyperledger-fabric-single-member-blockchain/deployment-details.png)

Ayrıntıları ekran ve ardından üç kullanışlı çıkış parametrelerini dağıtım özetini gösterir:

- _API uç noktası_ ağ üzerindeki bir uygulamayı dağıtmak sonra kullanılabilir.
- _ÖNEK_ , olarak da bilinir _dağıtım önek_ , benzersiz olarak kaynaklarınızın ve dağıtımınız tanımlar. Komut satırı tabanlı araçlar kullanırken kullanılır.
- _SSH için ilk VM_ önceden oluşturulmuş, ssh komutu sağ parametrelerle verir gerekli ilk VM, ağınıza bağlanmak için; Hyperledger doku söz konusu olduğunda, doku CA düğümü olacaktır.

Sağlanan yönetici kullanıcı adı ve parola/SSH anahtarı ile sanal makineleri SSH aracılığıyla her düğüm için uzaktan bağlanabilir. VM'ler düğümü değil sahip olduğundan, kendi ortak IP adresleri, yük dengeleyici üzerinden gidin ve bağlantı noktası numarasını belirtmeniz gerekir. İlk işlem düğümü erişmek için SSH üçüncü şablon çıktısı komuttur ** VM ilk SSH (örnek dağıtımı: `sh -p 3000 azureuser@hlf2racpt.northeurope.cloudapp.azure.com`). Ek işlem düğümlerine almak için bağlantı noktası numarası bir artırmasına (örneğin, bağlantı noktası 3000, ilk işlem düğümü ikinci 3001 üzerinde üçüncü 3002 üzerinde vs.).

## <a name="next-steps"></a>Sonraki adımlar

Artık uygulama ve chaincode geliştirme Hyperledger Konsorsiyumu blockchain ağınıza karşı odaklanmak hazırsınız.
