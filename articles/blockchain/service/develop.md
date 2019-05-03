---
title: Azure Blockchain hizmet geliştirmeye genel bakış
description: Azure blok zinciri hizmeti çözümleri geliştirmeye giriş.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 388a5d8c80c3e2462602959e9d5cbc1452974d1f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027908"
---
# <a name="azure-blockchain-service-development-overview"></a>Azure Blockchain hizmet geliştirmeye genel bakış

Azure Blockchain hizmetiyle consortium varlık izleme, dijital belirteci, bağlılık programı ve ödül, tedarik zinciri finansal ve provenance gibi Kurumsal senaryoları etkinleştirmek için blok zinciri ağlar oluşturabilirsiniz. Bu makalede, Azure Blockchain Service geliştirmeye genel bakış ve kuruluş için blok zinciri uygulamak için önemli konulara bir giriş niteliğindedir.

## <a name="client-connection-to-azure-blockchain-service"></a>Azure Blockchain hizmeti istemci bağlantısı

İstemciler tam düğümleri, hafif düğümleri ve uzak istemciler dahil olmak üzere blok zinciri ağlar için farklı türde vardır. Azure Blockchain hizmeti düğümleri içeren bir blok zinciri ağ oluşturur. Blok zinciri geliştirme için Azure blok zinciri hizmetine ağ geçidiniz olarak farklı istemciler kullanabilir. Azure Blockchain hizmeti, bir geliştirme uç noktası olarak temel kimlik doğrulaması veya erişim anahtarı sunar. Kullanabileceğiniz popüler istemciler şunlardır bağlanın.

### <a name="metamask"></a>MetaMask

MetaMask, bir tarayıcı tabanlı Cüzdan (Uzak istemci), RPC istemci ve temel sözleşmesi Gezgini içindir. Diğer tarayıcı wallets MetaMask web3 örnek tarayıcı JavaScript bağlamına çeşitli Ethereum blok zincirleri paylaşıldıkça bağlanan bir RPC istemcisi işlevi gören ekler (*mainnet*, *Ropsten testnet*, *Kovan testnet*, Yerel RPC düğümü, vs.). Kolayca Azure blok zinciri hizmetine bağlanın ve blok zinciri geliştirme Remix kullanarak başlatmak için özel RPC ayarlayabilirsiniz.

### <a name="geth"></a>Geth

Geth Git içinde uygulanan tam bir Ethereum düğümünde çalıştırmak için komut satırı arabirimidir. Çalıştırmanız gerekmez düğümü tam ancak etkileşim kurmak için bir JavaScript API'si ile Azure blok zinciri hizmeti kullanıma sunan bir JavaScript çalışma zamanı ortamı sağlayan kendi etkileşimli konsol başlatabilirsiniz.

## <a name="development-framework-configuration"></a>Geliştirme çerçevesi yapılandırma

Karmaşık kurumsal blok zinciri çözümleri geliştirmek için bir geliştirme çerçevesi farklı blockchain ağlarına bağlanmak, akıllı sözleşme yaşam döngüsünü yönetme, testleri otomatikleştirmek, akıllı sözleşme betikleri ile dağıtın ve etkileşimli bir konsol Donatı için gereklidir.

Truffle yazma, derlemek, dağıtmak ve Ethereum blok zincirleri paylaşıldıkça merkezi olmayan uygulamaları test etmek için bir popüler blok zinciri geliştirme çerçevesidir. Ayrıca, Truffle sorunsuz bir şekilde akıllı sözleşme geliştirme ve geleneksel web geliştirme tümleştirmeye bir altyapı düşünebilirsiniz.

En az iki blockchain düğümüyle bile küçük proje kurar: Bir geliştiricinin makinesindeki ve diğer geliştirici kendi uygulama yeri dağıtır ağ temsil eden. Örneğin, ana genel Ethereum ağ veya Azure blok zinciri hizmet. Truffle her ağ için derleme ve dağıtım yapıtları yönetmek için bir sistem sağlar ve bunu son uygulama dağıtımını basitleştiren bir şekilde yapar. Daha fazla bilgi için [hızlı başlangıç: Truffle bağlanmak için kullandığınız bir Azure blok zinciri hizmet ağ](connect-truffle.md).

## <a name="ethereum-quorum-private-transaction"></a>Özel işlem Ethereum çekirdek

Çekirdek işlem plus sözleşmesi gizlilik ve yeni fikir birliğine varılmış mekanizmaları Ethereum tabanlı, dağıtılmış kayıt defteri protokolüdür. Git Ethereum üzerinden önemli geliştirmeler şunları içerir:

* Gizlilik - çekirdek özel işlemleri ve özel sözleşmeleri aracılığıyla ortak ve özel durum ayrımı destekler ve eşler arası şifreli ileti değişimleri ağ katılımcılara yönlendirilmiş özel veri aktarımı için kullanır.
* Alternatif fikir birliğine varılmış mekanizmaları - permissioned ağındaki iş kavram veya kavram, proje Katılımcısı fikir birliğine varılmış gerek kalmaz. Çekirdek RAFT ve IBFT gibi consortium zincirleri için tasarlanmış birden fazla fikir birliğine varılmış mekanizmaları sağlar.  Azure Blockchain Hizmetleri IBFT fikir birliğine varılmış mekanizması kullanır.

* Eş kullanarak - yalnızca bilinen taraflar sağlama akıllı sözleşmelerinizin kullanarak düğümü ve eş kullanarak ağ katılmak
* Daha yüksek performans - çekirdek genel Geth daha yüksek performans sunar.

Bkz: [Öğreticisi: Azure Blockchain Hizmeti'ni kullanarak bir işlem gönderme](send-transaction.md) özel işlem örneği.

## <a name="block-explorers"></a>Blok gezginleri

Blok gezginler tek blok içeriği, işlem adresi verilerini ve geçmişini görüntüleme çevrimiçi blockchain tarayıcılar ' dir. Azure İzleyici'de Azure blok zinciri Service aracılığıyla temel blok bilgi kullanılabilir, geliştirme sırasında daha ayrıntılı bilgi gerekiyorsa, ancak blok gezginler yararlı olabilir.  Kullanabileceğiniz popüler açık kaynak blok gezginler vardır. Azure Blockchain hizmetiyle çalışacak blok gezginler listesi verilmiştir:

* [Azure Blockchain hizmet Gezgini](https://web3labs.com/azure-offer) Web3 Labs gelen
* [BlockScout](https://github.com/Azure-Samples/blockchain/blob/master/ledger/template/ethereum-on-azure/technology-samples/blockscout/README.md)

## <a name="tps-measurement"></a>TPS ölçüm

Daha fazla Kurumsal senaryolarda blok zinciri kullanıldıkça, işlem ikinci (TPS) hız başına performans sorunlarını ve sistem verimsizlikleri kaçınmak önemli. Yüksek işlem hızları içinde merkezi olmayan bir blok zinciri bakımını yapmak zor olabilir. Doğru bir TPS ölçüm, sunucu iş parçacığı, işlem kuyruk boyutu, ağ gecikme süresi ve güvenlik gibi farklı faktörler tarafından etkilenebilir. Geliştirme sırasında TPS hızı ölçmek gerekiyorsa, popüler bir açık kaynaklı araç olan [ChainHammer](https://github.com/drandreaskrueger/chainhammer).

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı Başlangıç: Truffle bağlanmak için kullandığınız bir Azure blok zinciri hizmet ağ](connect-truffle.md)
