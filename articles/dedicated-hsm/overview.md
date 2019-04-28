---
title: Ayrılmış HSM nedir? -Ayrılmış HSM azure | Microsoft Docs
description: Azure ayrılmış HSM genel bakış 140-2 Düzey 3 sertifika anahtarı depolama alanı kapasitesini FIPS karşılayan Azure sağlar.
services: dedicated-hsm
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc, seodec18
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 88a49c9c124c5399749d2b60595d7f5c7ec77b20
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118001"
---
# <a name="what-is-azure-dedicated-hsm"></a>Azure Ayrılmış HSM nedir?

Azure ayrılmış HSM azure'da şifreleme anahtarı depolama alanı sağlayan bir Azure hizmetidir. Ayrılmış HSM en katı güvenlik gereksinimleri karşılar. FIPS 140-2 Düzey 3 doğrulanmış cihazlar ve HSM gerecinin eksiksiz ve özel denetim ihtiyaç duyan müşteriler için ideal çözümdür. 

 HSM cihazlarına genel olarak çeşitli Azure bölgeleri arasında dağıtılır. Bunlar kolayca cihazların bir çift olarak sağlanabilir ve yüksek kullanılabilirlik için yapılandırılır. HSM cihazlarına bölge düzeyinde yük devretme karşı güvence altına almak için bölge arasında da sağlanabilir. Microsoft teslim ayrılmış HSM hizmetini kullanarak [SafeNet Luna ağ HSM 7 (Model A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) Gemalto gereçten. Bu cihaz en yüksek düzeyde performans ve şifreleme tümleştirme seçenekleri sunar. 

Sağlanan sonra HSM cihazlarına doğrudan bir müşterinin sanal ağa bağlanır. Noktadan siteye veya siteden siteye VPN bağlantısı yapılandırırken şirket içi uygulama ve yönetim araçları tarafından da erişilebilir. Müşteriler, yazılım ve yapılandırıp Gemalto'nın destek Portalı'ndan HSM cihazları yönetmek için belgeleri edinin.

## <a name="why-use-azure-dedicated-hsm"></a>Azure ayrılmış HSM neden kullanmalısınız?

### <a name="fips-140-2-level-3-compliance"></a>FIPS 140-2 Düzey 3 uyumluluk

Birçok kuruluşun katı sektör düzenlemelerinin şifreleme anahtarı depolama alanı karşıladığından bu dikte sahip [FIPS 140-2-3. düzey](https://csrc.nist.gov/publications/detail/fips/140/2/final) gereksinimleri. Microsoft'un çok kiracılı Azure Key Vault hizmetine şu anda yalnızca FIPS 140-2 Düzey 2 sertifika sağlar. Azure ayrılmış HSM, finansal hizmet sektöründe, devlet kurumları ve başkaları tarafından FIPS 140-2-3. düzey gereksinimleri karşılaması gerekir gerçek bir gereksinimi karşılar.

### <a name="single-tenant-devices"></a>Tek kiracılı cihazlar

Birçok müşterimizin tek kiracılı şifreleme depolama cihazının gereksinim. Azure ayrılmış HSM hizmeti, Microsoft'un Global olarak dağıtılmış veri merkezlerinden oluşan bir fiziksel CİHAZDAN sağlamak bunları sağlar. Müşteriye sağlandıktan sonra yalnızca ilgili müşterinin cihaza erişebilirsiniz.

### <a name="full-administrative-control"></a>Tam yönetim denetimi

Birçok müşteri tam yönetimsel denetime gerektirir ve şahıs cihazlarını yönetim amaçları için erişim. Bir cihaz sağlandıktan sonra yalnızca müşterinin cihaz yönetim veya uygulama düzeyinde erişimi vardır.

 Microsoft, müşteri ilk kez bu noktada, müşteri parolayı değiştirir. cihaz eriştikten sonra hiçbir yönetimsel denetime sahiptir. Bu noktadan itibaren müşterinin bir true tek kiracılı tam yönetimsel denetime ve uygulama yönetimi özelliği ' dir. Microsoft, seri bağlantı noktası aracılığıyla telemetri için izleme düzeyi erişim (Yönetici rolü değil) korur. Bu erişim sıcaklık ve güç kaynağı durumu fanı sistem durumu gibi donanım izleyiciler kapsar. 
 
 Müşteri, gerekli izleme devre dışı bırakmak ücretsizdir. Ancak, bunu devre dışı bırakırsanız, bunlar Microsoft önleyici sağlık durumu uyarılar alınmayacak.

### <a name="high-performance"></a>Yüksek performans

Gemalto cihazın çeşitli nedenlerle için bu hizmeti seçildi. Geniş bir şifreleme algoritması desteği, çeşitli desteklenen işletim sistemleri ve kapsamlı API desteği sunar. Dağıtılan belirli bir modeli için RSA 2048 saniye başına 10.000 işlem ile mükemmel performans sunar. Bu benzersiz uygulama örnekleri için kullanılabilecek 10 bölümlerini destekler. Bu, düşük gecikme süreli, yüksek kapasite ve yüksek aktarım hızı cihaz cihazdır.

### <a name="unique-cloud-based-offering"></a>Benzersiz bulut tabanlı teklif

Microsoft, müşterilerin benzersiz bir dizi için belirli bir gereksiniminiz tanınan. Bu yeni müşterilere FIPS 140-2 Düzey 3 doğrulandı ve böyle bir kapsamını bulut tabanlı sunar ve şirket içi uygulama tümleştirme adanmış bir HSM hizmetini sunan tek bulut sağlayıcısıdır.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>Azure ayrılmış HSM sizin için doğru mu?

Azure ayrılmış HSM, büyük ölçekli bir kuruluşun belirli bir türü için benzersiz gereksinimlerini karşılar özel bir hizmettir. Sonuç olarak, Azure müşterilerine getirerek bu hizmet için kullanım profili sığmayacak beklenmektedir. Çok daha uygun ve uygun maliyetli olması için Azure Key Vault hizmetini bulur. Gereksinimleriniz için uygun olduğuna karar vermenize yardımcı olması için aşağıdaki ölçütleri belirledik.

### <a name="best-fit"></a>En uygun

Azure ayrılmış HSM doğrudan ve tek HSM cihazlarına erişimi gerektiren "lift-and-shift" senaryolar için çok uygundur. Örneklere şunlar dahildir:

- Geçirme uygulamaları için Azure sanal makineleri şirket içi
- AWS bulut HSM Klasik hizmet (Amazon bu hizmetin yeni müşteriler teklifidir değil) kullanan sanal makineler için Amazon AWS EC2'den uygulamalarını geçirme
- Çalışan yazılım Apache/Ngnix SSL yük boşaltma, Oracle TDE ve Azure sanal Makineler'de ADCS gibi verdiği 

### <a name="not-a-fit"></a>Uygun değil

Azure ayrılmış HSM aşağıdaki senaryo türü için uygun değil: Microsoft bulut ile tümleşik olmayan müşteri tarafından yönetilen anahtarlar (örneğin, Azure Information Protection, Azure Disk şifrelemesi, Azure Data Lake Store, Azure depolama, Azure SQL veritabanı ve Office 365 için müşteri anahtarı) ile söz konusu destek Şifreleme Hizmetleri Azure ayrılmış HSM.

### <a name="it-depends"></a>Duruma göre değişir

Azure ayrılmış HSM sizin için uygun olup olmadığını, gereksinimleri ve yapamaz veya ödün bulunabilecek karmaşık bir karışımını üzerinde bağlıdır. FIPS 140-2 Düzey 3 gereksinim buna bir örnektir. Bu gereksinim için ortaktır ve ayrılmış HSM şu anda, toplantı için kullanılabilecek tek seçenektir. Bu mandated gereksinimleri ilgili değilse, genellikle, Azure anahtar kasası ve HSM ayrılmış arasında bir seçim ise. Bir karar vermeden önce gereksinimlerinizi değerlendirin.

Seçeneklerinizi Tart gerekecektir durumlar şunlardır: 

- Bir müşterinin Azure sanal makinesinde çalışan yeni kod
- Azure sanal makine'de SQL Server TDE
- Azure depolama istemci tarafı şifreleme
- SQL Server ve Azure SQL DB her zaman şifreli

## <a name="next-steps"></a>Sonraki adımlar

Bu yüksek oranda özelleştirilmiş bir hizmettir. Bu nedenle, tam olarak bu belge kümesindeki fiyatlandırması, destek ve hizmet düzeyi sözleşmeleri gibi temel kavramları anlamanız önerilir. 

[Gemalto tümleştirme kılavuzlarını](https://safenet.gemalto.com/partners/microsoft/) HSM'ler var olan bir sanal ağ ortamına sağlamayı kolaylaştırmak yardımcı olur. Var olan dağıtım Mimarinizi ayarlama işlemini belirlemenize yardımcı olmak için nasıl yapılır kılavuzlarından de olur.

* [Yüksek kullanılabilirlik](high-availability.md)
* [fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirliği](supportability.md)
* [İzleme](monitoring.md)
