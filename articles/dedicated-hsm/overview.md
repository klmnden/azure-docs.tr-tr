---
title: Ayrılmış HSM nedir? | Microsoft Docs
description: Azure ayrılmış HSM anahtar depolama kapasitesini FIPS karşılayan Azure 140-2 Düzey 3 sertifika sağlar.
services: dedicated-hsm
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 11/26/2018
ms.author: barclayn
ms.openlocfilehash: 92d77ec886a0f37c28f5e3031a7e14f63299c8aa
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427123"
---
# <a name="what-is-dedicated-hsm"></a>Ayrılmış HSM nedir?

Azure ayrılmış HSM en katı güvenlik gereksinimleri karşılayan azure'da şifreleme anahtar depolama alanı sağlar. Ayrılmış HSM FIPS 140-2 Düzey 3'ü doğrulanmış cihaz ve HSM gerecinin eksiksiz ve özel denetim gerektiren müşteriler için ideal bir çözümdür. HSM cihazlarına birkaç Azure bölgeleri arasında genel olarak dağıtılan ve kolayca cihazların bir çift olarak sağlanabilir ve yüksek kullanılabilirlik için yapılandırılır. HSM'ler düzeyi bölgesel yük devretme karşı güvence altına almak için bölge arasında de sağlanmış. Microsoft teslim ayrılmış HSM hizmetini kullanarak [SafeNet Luna ağ HSM 7 (Model A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) Gemalto gereçten. Bu cihaz en yüksek düzeyde performans ve şifreleme tümleştirme seçenekleri sunar. Sağlanan HSM'ler doğrudan bir müşterinin sanal ağa bağlı ve noktadan siteye veya siteden siteye VPN bağlantısı yapılandırarak şirket içi uygulama ve yönetim araçları tarafından da erişilebilir. Müşteriler, yazılım ve yapılandırıp Gemalto'nın destek Portalı'ndan HSM cihazları yönetmek için belgeler alacaksınız.

## <a name="why-use-azure-dedicated-hsm"></a>Azure ayrılmış HSM neden kullanmalısınız?

### <a name="fips-140-2-level-3-compliance"></a>FIPS 140-2, 3 uyumluluk düzeyi

Şifreleme anahtarı depolama alanı karşıladığını dikte katı sektör düzenlemelerinin pek çok kuruluş sahip [FIPS 140-2 Düzey 3](https://csrc.nist.gov/publications/detail/fips/140/2/final) gereksinimleri. Microsoft'un çok kiracılı Azure Key Vault hizmetine şu anda yalnızca FIPS 140-2 2. düzey sertifika sağlar. Azure ayrılmış HSM, finansal hizmet sektöründe, devlet kurumları ve başkaları tarafından FIPS 140-2 Düzey 3 gereksinimleri karşılaması gerekir gerçek bir gereksinimi karşılar.

### <a name="single-tenant-devices"></a>Tek Kiracılı cihazlar

Birçok müşterimizin tek kiracılı şifreleme depolama cihazının gereksinim. Azure ayrılmış HSM hizmet, Microsoft'un Global olarak dağıtılmış veri merkezlerinden oluşan bir fiziksel bir cihaz sağlama için izin verir. Bir müşteriye sağlanan sonra yalnızca ilgili müşterinin cihazınıza erişim hakkı mümkün olacaktır.

### <a name="full-administrative-control"></a>Tam yönetim denetimi

Tek kiracılı cihazların yanı sıra birçok müşteri tam yönetimsel denetime gerektirir ve yönetimsel amaçlarla erişim şahıs. Sağlanan sonra yalnızca o müşteri cihaza bir yönetici veya uygulama düzeyinde erişimi vardır. Microsoft hiçbir yönetim denetimi parola değiştirme değişikliği gerektiren Müşteri'nin ilk sonra erişebilir. Bu noktadan itibaren müşterinin bir true tek kiracılı tam yönetimsel denetime ve uygulama yönetimi özelliği ' dir. Microsoft, telemetri sıcaklık ve güç kaynağı durumu fanı sistem durumu gibi donanım izleyiciler kapsayan seri bağlantı noktası bağlantısı aracılığıyla bir izleme düzeyi erişim (Yönetici rolü değil) korumak. Müşteri, bu gerekiyordu, ancak etkin sistem durumu uyarılarını Microsoft'tan almaz, devre dışı bırakmak ücretsizdir.

### <a name="high-performance"></a>Yüksek Performans

Gemalto cihaz şifreleme algoritması desteği, desteklenen işletim sistemlerinin çeşitli ve kapsamlı API desteği, geniş nedeniyle bu hizmet için seçildi. Dağıtılan belirli bir modeli için RSA 2048 saniye başına 10.000 işlem ile mükemmel performans sunar. Bu benzersiz uygulama örnekleri için kullanılabilecek 10 bölümlerini destekler. Düşük gecikme süreli, yüksek kapasite ve yüksek aktarım hızı cihaz budur.

### <a name="unique-cloud-based-offering"></a>Benzersiz bulut tabanlı teklif

Microsoft, benzersiz bir arasında belirli bir ihtiyacı müşterileri ayarlayın ve yeni müşterilere FIPS 140-2 Düzey 3 doğrulanır ve bu tür bir kapsamını bulut tabanlı sunar ve şirket içi uygulamalarınızı adanmış bir HSM hizmetini sunan tek bulut sağlayıcısıdır tanınan Tümleştirme.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>Azure ayrılmış HSM sizin için doğru mu?

Azure ayrılmış HSM, belirli türde bir büyük ölçekli kuruluş arasında benzersiz gereksinimlerini adresleme özel bir hizmettir. Sonuç olarak, Azure müşterilerine getirerek bu hizmet için kullanım profili sığmayacak beklenmektedir. Çok daha uygun ve daha uygun maliyetli olması için Azure Key Vault hizmetini bulur. Gereksinimlerinize uygun karar vermenize yardımcı olması için aşağıdaki ölçütleri belirledik.

### <a name="best-fit"></a>En uygun

Doğrudan ve tek HSM cihazlarına erişimi gerektiren "lift-and-shift" senaryolar için en uygun. Örneklere şunlar dahildir:

- Geçirme uygulamaları için Azure sanal makineleri şirket içi
- Amazon AWS EC2'den geçirme uygulamaları için Azure (Amazon bu hizmetin yeni müşteriler teklifidir değil) AWS bulut HSM Klasik hizmet kullanan sanal makineler
- Yazılım Apache/Ngnix SSL yük boşaltma, Oracle TDE ve ADCS gibi Azure sanal Makineler'de çalışan verdiği

### <a name="not-a-fit"></a>Uygun değil

Müşteri tarafından yönetilen anahtarlar (örneğin, Azure Information Protection, Azure Disk şifrelemesi, Azure Data Lake Store, Azure depolama, Azure SQL, Office 365 müşteri anahtarı) ile şifrelemeyi destekleyen bir Microsoft bulut Hizmetleri ile Azure ayrılmış HSM tümleşik değildir.

### <a name="it-depends"></a>Duruma göre değişir

Birçok senaryo gereksinimlerini ve hangi güvenlik ihlalleri olabilir veya yapılabilir duruma getirilemediğinden olası karmaşık Karıştır bağlıdır. FIPS 140-2 Düzey 3 gereksinim sıklıkla uygulanan bir örnektir ve bu nedenle, şu anda ayrılmış HSM Seçenekler.  Bu mandated gereksinimleri ilgili olmayan, sonra genellikle bir karışımını gereksinimlerini değerlendirme üzerinde Azure anahtar kasası ve HSM ayrılmış arasında karar bağlı olurdu. Örneklere şunlar dahildir:

- Bir müşterinin Azure sanal Makineler'de çalışan yeni kodu
- SQL Server Azure sanal makine'de TDE
- Azure depolama istemci tarafı şifreleme
- SQL Server ve Azure SQL DB her zaman şifreli

## <a name="next-steps"></a>Sonraki Adımlar

Bu hizmet yüksek oranda özelleştirilmiş yapısı göz önünde bulundurarak, bu dokümantasyon setinde bulunan önemli kavramlar bazıları tam olarak anlaşılır, fiyatlandırma, destek ve hizmet düzeyi sözleşmeleri tam olarak anlaşılır ve ardından bir öğreticidir önerilir HSM'ler var olan bir sanal ağ ortamına sağlamayı kolaylaştırmak kullanılabilir. [Gemalto tümleştirme kılavuzlarını](https://safenet.gemalto.com/partners/microsoft/) ve dağıtım mimarisi karar için nasıl yapılır kılavuzlarını da harika bir kaynak.

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirliği](supportability.md)
* [İzleme](monitoring.md)
