---
title: Azure Blockchain hizmetine genel bakış
description: Azure Blockchain hizmetine genel bakış
services: azure-blockchain
keywords: Blok zinciri
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: a200649493354f1264afb0df4cf74acb4a274017
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406413"
---
# <a name="what-is-azure-blockchain-service"></a>Azure Blockchain hizmeti nedir?

Azure Blockchain kullanıcılar büyütün ve uygun ölçekte azure'da blok zinciri ağları olanağı sağlayan tam olarak yönetilen kayıt defteri hizmeti hizmetidir. Hem altyapı Yönetimi yanı sıra blok zinciri ağ idare birleşik denetimi sağlayarak, Azure Blockchain hizmeti sunar:

* Basit ağ dağıtımı ve işlemleri
* Yerleşik consortium Yönetimi
* Nitelikli akıllı anlaşmalar aşina olduğunuz geliştirme araçları ile geliştirme

Azure Blockchain hizmeti, birden çok kayıt defteri protokol desteklemek için tasarlanmıştır. Şu anda Ethereum desteği sağladığı [çekirdek](https://www.jpmorgan.com/Quorum) muhasebe kullanarak [IBFT](https://github.com/jpmorganchase/quorum/wiki/Quorum-Consensus) fikir birliğine varılmış mekanizması.

Bu özellikler neredeyse hiç yönetim gerektirmez ve tümüyle ek ücret ödemeden sağlanır. Uygulama geliştirme ve iş mantığı yerine zamanınızı ve kaynaklarınızı sanal makine ve altyapı Yönetimi odaklanabilirsiniz. Ayrıca, yeni beceriler edinmek zorunda kalmadan çözümlerinizi sunmak için tercih ettiğiniz platform ve açık kaynaklı araçları geliştirmeye devam edebilirsiniz.

## <a name="network-deployment-and-operations"></a>Ağ dağıtımı ve işlemleri

Azure Blockchain Hizmeti'ni dağıtma Azure blok zinciri uzantıyı kullanarak Visual Studio kod yanı sıra Azure portalı, Azure CLI aracılığıyla yapılabilir.  Hem işlem hem de doğrulayıcının düğüm, hizmet tarafından yönetilen depolama yanı sıra güvenlik yalıtımı için Azure sanal ağları sağlama dahil olmak üzere dağıtım basitleştirilmiştir.  Ayrıca, yeni bir blok zinciri üye dağıtırken kullanıcılar ayrıca oluşturmak veya Konsorsiyumu katılın.  Farklı Azure aboneliklerinden başka bir işlemle bir paylaşılan uygun blok zinciri güvenli bir şekilde iletişim kurabilmesi birden çok tarafları Konsorsiyum etkinleştirin.  Bu Basitleştirilmiş Dağıtım güne ilişkin dakika, blok zinciri ağ dağıtımı azaltır.

### <a name="performance-and-service-tiers"></a>Performans ve hizmet katmanları

Azure Blockchain hizmeti iki hizmet katmanı sunmaktadır: *Temel* ve *standart*. Her katman farklı performans ve basit geliştirme desteği ve test iş yüklerini en çok yüksek düzeyde yeteneklerini üretim blockchain dağıtımları ölçeği sunar. Her iki katmanda en az bir işlem düğümü ve bir doğrulayıcı (Temel) veya iki Doğrulayıcı düğümü (standart) içerir.

![Fiyatlandırma katmanları](./media/overview/pricing-tiers.png)

İki Doğrulayıcı düğüm sunan yanı sıra *standart* Katman 2 sunar *sanal çekirdekler* işlem ve Doğrulayıcı her düğüm için temel katmanı 1 sanal çekirdek yapılandırma sunar ancak.  İşlem ve Doğrulayıcı düğümler için 2 sanal çekirdek sunarak, kalan 1 sanal çekirdek, blok zinciri iş yüklerini üretim için en uygun performansı sağlamaya diğer altyapı ile ilgili hizmetler için kullanılabilse de 1 sanal çekirdek çekirdek muhasebe ayrılabilir. Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için bkz: [Azure blok zinciri Service fiyatlandırması](https://azure.microsoft.com/pricing/details/blockchain-service).

### <a name="security-and-maintenance"></a>Güvenlik ve Bakım

İlk blok zinciri üyelik sağlandıktan sonra ek işlem düğümleri, üye eklemek için imkanına sahip olursunuz.  Varsayılan olarak, işlem düğümleri aracılığıyla güvenlik duvarı kurallarını güvenli olduğundan ve erişimi için yapılandırılmış gerekir.  Ayrıca, tüm işlem düğümleri TLS üzerinden Hareket halindeki verileri şifreleyin.  Güvenlik duvarı kuralları, temel kimlik doğrulaması, erişim anahtarlarını yanı sıra Azure Active Directory Tümleştirme dahil olmak üzere işlem düğümü erişiminin güvenliğini sağlamak için birden çok seçenek vardır. Daha fazla bilgi için [işlem düğümleri yapılandırma](configure-transaction-nodes.md) ve [Azure Active Directory erişimini yapılandırma](configure-aad.md).

Yönetilen bir hizmet olarak blockchain üyenin düğümlerin işletim sistemi ve kayıt defteri yazılım yığını güncelleştirmeleri, yüksek kullanılabilirlik için (yalnızca standart katman) yapılandırılmış, DevOps çoğunu ortadan en son konak ile düzeltme eki Azure blok zinciri hizmeti sağlar. Geleneksel Iaas blok zinciri düğümleri için gereklidir.  Düzeltme eki uygulama ve güncelleştirmeler hakkında daha fazla bilgi için bkz. [desteklenen Azure blok zinciri hizmet muhasebe sürümleri](ledger-versions.md).

### <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

Ayrıca, Azure Blockchain hizmet düğümlerinin CPU, bellek ve depolama kullanımı hakkında Öngörüler yanı sıra, işlemler ve İncelenmiş blokları gibi ağ etkinliği blok zinciri faydalı içgörüler sağlayan Azure İzleyici hizmeti aracılığıyla zengin ölçümleri sağlar, işlem sıra derinliğini, hem de etkin bağlantılar.  Ölçümler, görünümler, blok zinciri uygulamanız için önemli olan öngörüleri sağlayacak özelleştirilebilir.  Ayrıca, kullanıcıların bir e-posta veya SMS mesajı göndermek için bir mantıksal uygulama, Azure işlevi çalıştıran veya özel tanımlı bir Web kancası gönderme gibi eylemleri tetiklemek uyarılar aracılığıyla eşikleri tanımlanabilir.

![Ölçümler](./media/overview/metrics.png)

Azure Log Analytics, kullanıcıların çekirdek muhasebe ya da diğer önemli bilgiler gibi çalıştı bağlantıları işlem düğümlerine ilgili günlüklerini görüntüleyebilirsiniz.

## <a name="built-in-consortium-management"></a>Yerleşik consortium Yönetimi

İlk blok zinciri üyelik dağıtırken katılın veya yeni bir konsorsiyum oluşturun.  Konsorsiyumu, idare ve çok taraflı bir işlemde transact blockchain üyeleri arasında bağlantı yönetmek için kullanılan mantıksal bir gruptur.  Azure blok zinciri hizmeti aracılığıyla hangi eylemleri üyeleri consortium sürebilir belirlemek için önceden tanımlanmış akıllı sözleşmeler, yerleşik idare denetimleri sağlar.  Bu idare denetimleri gerektiği şekilde consortium yönetici tarafından özelleştirilebilir. Yeni consortium oluşturduğunuzda, blok zinciri diğer taraflar, consortium katılmaya davet özelliğini etkinleştirme varsayılan yönetici Consortium üyesidir.  Yalnızca daha önce davet edildiniz varsa Konsorsiyumu katılabilirsiniz.  Konsorsiyumu eklerken, blok zinciri üyelik consortium'ın Yöneticisi tarafından yerleştirdiniz idare denetimleri tabidir.

![Consortium Yönetimi](./media/overview/consortium.png)

Ekleme ve Konsorsiyumu üyeler kaldırılırken gibi yönetim işlemlerini consortium, PowerShell ve REST API erişilebilir. Genel arabirimler kullanarak yerine değiştirme ve nitelikli akıllı anlaşmalar solidity tabanlı gönderme Konsorsiyumu programlı olarak yönetebilirsiniz. Daha fazla bilgi için [consortium Yönetim](consortium.md).

## <a name="develop-using-familiar-development-tools"></a>Aşina olduğunuz geliştirme araçlarını kullanarak geliştirme

Ethereum uygulamalarınız için yaptığınız gibi açık kaynaklı çekirdek Ethereum defterini bağlı olarak, uygulamaları Azure blok zinciri hizmeti için aynı şekilde geliştirebilirsiniz. Sektörün önde gelen iş ortaklarıyla çalışma, Azure blok zinciri Geliştirme Seti Visual Studio Code uzantısı Truffle nitelikli akıllı anlaşmalar oluşturun Suite gibi tanıdık araçlarından yararlanarak geliştiricilerin sağlar. Azure blok zinciri Geliştirme Seti eklentiyi kullanarak, geliştiriciler oluşturma veya bağlanın ve mevcut consortium oluşturmak ve dağıtmak, akıllı sözleşmeler tüm tek bir IDE içinden. Azure Blockchain Visual Studio Code uzantısı'nı kullanarak oluşturabilir veya, oluşturabilir ve akıllı sözleşmelerinizi tek bir IDE içinden dağıtmak için mevcut bir konsorsiyum bağlanın. Daha fazla bilgi için [VS Code Marketi'nde Azure blok zinciri Geliştirme Seti](https://aka.ms/vscodebcextension) ve [Azure blok zinciri Geliştirme Seti Kullanıcı Kılavuzu](https://aka.ms/vscodebcextensionwiki ).

## <a name="support-and-feedback"></a>Destek ve geri bildirim

İhtiyaç duyarsanız veya geri bildirimde bulunmak?

* Ziyaret [Azure blok zinciri blogu](https://azure.microsoft.com/blog/topics/blockchain/), [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain), ve [Azure blok zinciri Forumu](https://social.msdn.microsoft.com/Forums/home?forum=azureblockchain).
* Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/921130-azure-blockchain-service) aracılığıyla bir giriş oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bir hızlı başlangıcı deneyin veya bu kaynaklar daha ayrıntılı bilgi edinin.
* [Azure portalını kullanarak blok zinciri üye oluşturmak](create-member.md) veya [Azure CLI kullanarak bir blok zinciri üye oluştur]()
* Maliyet karşılaştırmaları ve hesaplayıcıları için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/blockchain-service).
* İlk kullanıp uygulamanızın derleme [Azure blok zinciri Geliştirme Seti](https://github.com/Azure-Samples/blockchain-devkit)
* Azure Blockchain VSCode uzantısı [Kullanıcı Kılavuzu](https://github.com/Microsoft/vscode-azure-blockchain-ethereum/wiki)
