---
title: Microsoft Azure veri bekleyen şifreleme | Microsoft Docs
description: Bu makalede, Microsoft Azure şifreleme bekleyen veri için genel bir bakış, genel özellikleri ve genel konular sağlar.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2018
ms.author: barclayn
ms.openlocfilehash: 4ced712b1b2716d85f0366ea892460053db598b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60613029"
---
# <a name="azure-data-encryption-at-rest"></a>Azure veri şifreleme bekleyen

Microsoft Azure, şirketinizin güvenlik ve uyumluluk gereksinimlerine göre verilerinizi korumak için Araçlar içerir. Bu yazıda odaklanır:

- Verileri Microsoft Azure üzerinde bekleyen nasıl korunur
- Veri koruma uygulamasında bölümü alma çeşitli bileşenleri açıklanır,
- Olumlu ve olumsuz yanlarını farklı anahtar yönetimi koruma yaklaşım incelenir. 

Bekleme sırasında şifreleme ortak bir güvenlik gereksinimidir. Azure'da, kuruluşlar risk veya bir özel anahtar yönetimi çözümünün maliyet olmadan bekleyen verileri şifreleme de yapabilirsiniz. Kuruluşlar, bekleme sırasında şifreleme tamamen Azure'un izin vererek seçeneğiniz vardır. Buna ek olarak, kuruluşlar yakından şifreleme ya da şifreleme anahtarlarını yönetmek için çeşitli seçenekleriniz vardır.

## <a name="what-is-encryption-at-rest"></a>Bekleme sırasında Şifreleme nedir?

Bekleme sırasında şifreleme verinin kodlama (şifreleme) andır kalıcı hale getirilir. Azure Rest tasarımlarında şifreleme, şifreleme ve büyük miktarlarda verileri hızlı bir şekilde basit bir kavramsal model göre şifresini çözmek için simetrik şifreleme kullanın:

- Bir simetrik şifreleme anahtarı için depolama yazılır verileri şifrelemek için kullanılır. 
- Aynı şifreleme anahtarını kullanmak bellekte readied gibi bu verilerin şifresini çözmek için kullanılır.
- Verileri bölümlere ayrılması ve her bölüm için farklı anahtar kullanılabilir.
- Anahtarlar, kimlik tabanlı erişim denetimi ile güvenli bir konumda depolanmalıdır ve denetim ilkeleri. Veri şifreleme anahtarları, genellikle daha fazla erişimi sınırlamak için asimetrik şifreleme ile şifrelenir.

Uygulamada, ölçek ve kullanılabilirlik Güvenceleri yanı sıra, yönetim ve denetim senaryoları, ek yapıları gerektirir. Microsoft Azure şifreleme Rest kavramları ve bileşenleri aşağıda açıklanmıştır.

## <a name="the-purpose-of-encryption-at-rest"></a>Amacı, bekleme sırasında şifreleme

Bekleyen şifreleme (Bekleyen) depolanan veriler için veri koruması sağlar. Bekleyen veriler saldırılar, verilerin depolandığı donanımına fiziksel erişim elde etmek için deneme içerir ve içerdiği veri güvenliğinin aşılmasına neden. Bu tür bir saldırı bir sunucunun sabit disk bakım sırasında saldırganın sabit sürücüsünü Kaldır mishandled. Daha sonra saldırgan, sabit sürücü verilere denemek için kendi denetimi altında bir bilgisayara koyabilirsiniz. 

Bekleme sırasında şifreleme saldırgan şifrelenmemiş erişimini engellemek için tasarlanan veri sağlayarak verileri, diskte şifrelenir. Saldırgan, bir saldırganın bir sabit sürücü ile şifrelenmiş veriler, ancak şifreleme anahtarları alırsa, verileri okumak için şifreleme aşılması gerekir. Bu, çok daha karmaşıktır ve bir sabit sürücüde şifresiz veriler erişimden çok kaynak tüketen saldırıdır. Bu nedenle, bekleme sırasında şifreleme önemle tavsiye edilir ve yüksek öncelikli çoğu kuruluş için zorunludur. 

Bekleme sırasında şifreleme, bir kuruluşun verileri idare ve uyum çabalarınızı gereksinimini tarafından da gerekebilir. HIPAA, PCI ve FedRAMP, gibi sektör ve kamu düzenlemeleri veri koruma ve şifreleme gereksinimleri ile ilgili belirli güvenlik önlemleri düzenlenmesine. Bekleme sırasında şifreleme bazı bu yasal düzenlemelerle uyumluluğu için gereken zorunlu bir ölçüdür.

Uyumluluk ve Mevzuat gereksinimlerini karşılamadığınızı ek olarak bekleyen şifreleme savunma koruma sağlar. Microsoft Azure Hizmetleri, uygulamalar ve veriler için uyumlu bir platform sağlar. Ayrıca, kapsamlı tesis ve fiziksel güvenlik, veri erişimi ve denetim sağlar. Ancak, bekleyen şifreleme gibi güvenlik önlemi sağlar ve diğer güvenlik önlemleri biri başarısız durumunda "çakışan" ek güvenlik önlemleri sağlamak önemlidir

Microsoft bulut Hizmetleri ve şifreleme anahtarları denetiminizde ve anahtar kullanımı günlükleri müşteriler vererek üzerinden rest seçeneklerinin şifreleme için taahhüt eder. Microsoft ayrıca, varsayılan olarak tüm müşteri verileri, bekleyen veri şifreleme doğrultusunda çalışmaktadır.

## <a name="azure-encryption-at-rest-components"></a>Azure Rest bileşenleri şifrelemesi

Daha önce açıklandığı gibi bekleme sırasında şifreleme diskte kalıcı veri bir gizli şifreleme anahtarıyla şifrelenir hedefidir. Bu hedef güvenli anahtar oluşturma elde etmek için depolama, erişim denetimi ve yönetimi, şifreleme anahtarlarının sağlanmalıdır. Ayrıntılar değişebilir ancak Azure Hizmetleri şifreleme Rest uygulamaları Aşağıdaki diyagramda gösterildiği koşulları açıklanabilir.

![Bileşenler](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

Bu anahtarlar için erişim denetimi ve şifreleme anahtarları depolama konumu, bir rest modeli şifrelemesi taşımaktadır. Anahtarların yüksek oranda güvenli, ancak belirtilen kullanıcılar tarafından yönetilebilir ve belirli hizmetler için kullanılabilir olması gerekir. Azure anahtar kasası, Azure Hizmetleri için önerilen anahtar depolama çözümüdür ve hizmetler arasında ortak bir yönetim deneyimi sağlar. Anahtarları depolanan ve yönetilen anahtar kasalarında ve kullanıcıları veya hizmetler için bir anahtar kasasına erişim verilebilir. Azure Key Vault, müşteri tarafından yönetilen bir şifreleme anahtarı senaryolarında kullanım için anahtarları oluşturulmasını müşteri veya müşteri anahtarları alma destekler.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory hesaplarını yönetmenizi veya bekleyen şifreleme ve şifre çözme, şifreleme için erişmeye Azure Key Vault'ta depolanan anahtarları kullanma izni verilebilir. 

### <a name="key-hierarchy"></a>Anahtar hiyerarşisi

Birden fazla şifreleme anahtarını bir rest uygulama şifrelemeyi kullanılır. Asimetrik şifreleme, anahtar erişim ve Yönetim için gereken kimlik doğrulama ve güven oluşturmak için kullanışlıdır. Simetrik şifreleme için toplu şifreleme ve şifre çözme, daha güçlü şifreleme ve daha iyi performans için daha verimlidir. Bir anahtar değiştirilmelidir olduğunda tek şifreleme anahtar kullanımını sınırlama anahtar tehlikeye risk ve yeniden şifreleme maliyeti azaltır. Azure şifrelemeler rest modelleri, anahtarların aşağıdaki türde bir anahtar hiyerarşi kullanın:

- **Veri şifreleme anahtarı (DEK)** – bölüm veya veri bloğu şifrelemek için kullanılan simetrik AES256 anahtar.  Tek bir kaynak, çok sayıda bölümleri ve çok sayıda veri şifreleme anahtarları sahip olabilir. Her veri bloğu farklı bir anahtarla şifreleme şifre analiz saldırılara daha zor hale getirir. Şifreleme ve şifre çözme belirli bir blok kaynak sağlayıcısı veya uygulama örneği tarafından DEKs erişimi gereklidir. Bir DEK yeni bir anahtar kullanılarak değiştirildiğinde, ilişkili bir blok içinde yalnızca veri yeniden yeni anahtarla şifrelenmiş olması gerekir.
- **Anahtar şifreleme anahtarı (KEK)** – veri şifreleme anahtarlarını şifrelemek için kullanılan bir asimetrik şifreleme anahtarı. Anahtar şifreleme anahtarı veri şifreleme anahtarları kendilerini şifrelenip denetlenen izin verir. KEK erişimi olan varlık DEK gerektiren varlık farklı olabilir. Bir varlığın belirli bir bölüme her DEK erişimini sınırlamak için DEK erişim aracı. KEK DEKs şifresini çözmek için gerekli olduğundan, KEK etkili bir şekilde tarafından DEKs etkili bir şekilde KEK silinmesini tarafından silinebilir tek bir noktasıdır.

Anahtar şifreleme anahtarları ile şifrelenmiş veri şifreleme anahtarları ayrı olarak depolanır ve bu anahtar ile şifrelenmiş veri şifreleme anahtarı yalnızca erişim için anahtar şifreleme anahtarına sahip bir varlık alabilirsiniz. Anahtar depolama farklı modelleri desteklenir. Her model sonraki bölümde ilerleyen bölümlerinde daha ayrıntılı olarak ele alınacaktır.

## <a name="data-encryption-models"></a>Veri şifreleme modelleri

Çeşitli şifreleme modelleri ve Artıları ve eksileri'nın bilinmesini nasıl çeşitli kaynak sağlayıcıları azure'da bekleyen şifreleme uygulama anlamak için önemlidir. Bu tanımları, Azure'da ortak dil ve sınıflandırma emin olmak için tüm kaynak sağlayıcıları arasında paylaşılır. 

Sunucu tarafı şifrelemesi için üç senaryo vardır:

- Hizmet tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi
    - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirme
    - Microsoft anahtarları yönetir
    - Tam bulut işlevi

- Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi
    - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirme
    - Anahtarları Azure Key Vault aracılığıyla müşteri denetler
    - Tam bulut işlevi

- Müşteri tarafından denetlenen donanımda müşteri tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi
    - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemlerini gerçekleştirme
    - Müşteri tarafından denetlenen donanımda anahtarları müşteri denetler
    - Tam bulut işlevi

İstemci tarafı şifreleme için aşağıdakileri göz önünde bulundurun:

- Azure Hizmetleri, şifresi çözülmüş veriler göremiyorum
- Müşterileri Yönetme ve anahtarlar şirket içi depolama (veya diğer depoları güvenli). Anahtarlar, Azure Hizmetleri için kullanılabilir değildir.
- Daha düşük bulut işlevi

Desteklenen şifreleme modelleri azure'da iki ana gruplara ayırın: "İstemci şifreleme" ve "daha önce bahsedilen sunucu tarafı şifrelemesi" olarak. Bağımsız olarak kullanıldığında, rest modeli her zaman Azure Hizmetleri şifrelemeyi TLS veya HTTPS gibi güvenli aktarım kullanılmasını öneririz. Bu nedenle, aktarım şifreleme Aktarım Protokolü tarafından ele alınması ve hangi kullanmak için rest modeli şifrelemesi belirlemede önemli bir etken olmamalıdır.

### <a name="client-encryption-model"></a>İstemci şifreleme modeli

İstemci şifreleme modeli kaynak sağlayıcısı ya da Azure dışında hizmet veya çağıran uygulama tarafından gerçekleştirilen şifreleme ifade eder. Şifreleme, müşteri veri merkezinde çalışan bir uygulama tarafından veya Azure hizmeti tarafından gerçekleştirilebilir. Her iki durumda da bu şifreleme modeli ne herhangi bir şekilde verilerin şifresini çözmek veya şifreleme anahtarlarına erişim olanağı olmadan verilerin şifrelenmiş bir blobu Azure kaynak sağlayıcısı alır. Bu modelde, anahtar yönetimi çağırma hizmet/uygulama tarafından gerçekleştirilir ve Azure hizmeti için donuktur.

![İstemci](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Sunucu tarafı şifreleme modeli

Sunucu tarafı şifreleme modelleri Azure hizmeti tarafından gerçekleştirilir şifreleme bakın. Bu modelde, kaynak sağlayıcısı şifreleme ve şifre çözme işlemleri gerçekleştirir. Örneğin, Azure depolama, düz metin işlemlerinde veri alabilirsiniz ve şifreleme ve şifre çözme dahili olarak gerçekleştirir. Microsoft tarafından veya sağlanan yapılandırmasına bağlı olarak müşteri tarafından yönetilen bir şifreleme anahtarları kaynak sağlayıcısını kullanabilirsiniz. 

![Sunucu](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Sunucu tarafı şifreleme anahtar yönetimi modelleri

Her rest modelleri, sunucu tarafı şifreleme anahtar yönetimi ayırıcı özelliklerini gösterir. Bu içerir nerede ve nasıl şifreleme anahtarları oluşturulur ve modellere erişme ve anahtar döndürme yordamları yanı sıra depolanır. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Hizmet tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi

Birçok müşteri için bekleme durumunda olduğunda verilerin şifrelendiğinden emin olun önemli gereksinimdir. Hizmet tarafından yönetilen anahtarları etkinleştirir, müşteriler, belirli bir kaynak (depolama hesabı, SQL DB, vb.) işaretlemek izin vermek için şifreleme ve anahtar döndürme ve yedekleme gibi tüm anahtar yönetimi özelliklerini Microsoft'a bırakarak bu modeli kullanarak sunucu tarafı şifrelemesi . Bekleme sırasında şifreleme genellikle destekleyen birçok Azure hizmeti, bu model yönetimi, şifreleme anahtarlarının azure'a boşaltma destekler. Azure kaynak sağlayıcısının anahtarları oluşturur, bunları güvenli depolama veritabanında yerleştirir ve gerektiğinde alır. Bu hizmet anahtarları tam erişime sahip olur ve hizmet kimlik bilgileri yaşam döngüsü yönetimi üzerinde tam denetime sahip anlamına gelir.

![Yönetilen](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Müşteri için düşük ek yük bekleyen şifreleme sağlamak için gereken sunucu tarafı şifreleme hizmeti tarafından yönetilen anahtarlar kullanarak bu nedenle hızlı bir şekilde ele alır. Kullanılabilir olduğunda bir müşteri genellikle hedef abonelik ve kaynak sağlayıcısı için Azure portalını açar ve belirten bir kutusu denetler, verilerin şifrelenmesini ister misiniz. Bazı kaynak yöneticileri hizmet tarafından yönetilen anahtarlarla sunucu tarafı şifreleme varsayılan olarak açıktır.

Sunucu tarafı şifreleme Microsoft tarafından yönetilen anahtarlarla hizmet anahtarları yönetmek için tam erişime sahip anlamına gelmez. Bazı müşteriler çünkü bunlar daha fazla güvenlik elde düşünüyorsanız anahtarları yönetmek isteyebilirsiniz, ancak bu modeli değerlendirirken, maliyet ve bir özel anahtar depolama çözümü ile ilişkili risk düşünülmelidir. Çoğu durumda, bir kuruluşun kaynak kısıtlamaları veya bir şirket içi çözüm risklerini bulut Yönetimi rest anahtarlarını, şifreleme riskini büyük olabilir belirleyebilirsiniz.  Ancak, bu model oluşturulmasını ve yaşam döngüsü, şifreleme anahtarlarının denetlemek için veya farklı sorumlunuza (diğer bir deyişle, ayrımı hizmetini yönetme olanlardan bir hizmetin şifreleme anahtarlarını yönetmek için gereksinimleri olan kuruluşlar için yeterli olmayabilir Anahtar Yönetimi hizmeti için genel yönetim modelinden).

##### <a name="key-access"></a>Anahtar erişimi

Sunucu tarafı şifreleme hizmeti tarafından yönetilen anahtarlar ile kullanıldığında, anahtar oluşturma, depolama ve hizmet erişim tüm hizmet tarafından yönetilir. Genellikle, temel Azure kaynak sağlayıcıları yakın veri deposuna veri şifreleme anahtarları depolar ve hızlı bir şekilde kullanılabilir ve erişilebilir sırasında anahtar şifreleme anahtarları güvenli bir iç deposunda depolanır.

**Avantajları**

- Basit kurulum
- Anahtar döndürme, yedekleme ve yedeklilik Microsoft yönetir
- Müşteri, uygulama veya bir özel anahtar yönetimi düzeni riskini ile ilişkilendirilmiş maliyet yok.

**Dezavantajları**

- (Anahtar belirtimi, yaşam döngüsü, iptal, vb.) Şifreleme anahtarlarınızın müşteri denetim yok
- Hizmeti genel yönetim modelinden anahtar yönetimi görevlerini birbirinden ayırın olanağı

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi 

Bekleyen verileri şifrelemek ve şifreleme denetlemek için gereksinim olduğu senaryolar için sunucu tarafı şifreleme anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanarak anahtarları müşteriler kullanabilir. Bazı hizmetler, yalnızca kök anahtar şifreleme anahtarı Azure Key Vault'ta depolamak ve bir iç konumuna yakın bir konumda veri şifrelenmiş veri şifreleme anahtarı depolamak. Senaryo müşteriler kendi anahtarları getir (BYOK – kendi anahtarını Getir), anahtar kasası için veya yenilerini oluşturmak ve bunları istediğiniz kaynakları şifrelemek için kullanın. Kaynak sağlayıcısı şifreleme ve şifre çözme işlemlerini gerçekleştirirken tüm şifreleme işlemleri için kök anahtarı olarak yapılandırılmış olan anahtar kullanır. 

##### <a name="key-access"></a>Anahtar erişimi

Sunucu tarafı şifreleme modeli Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlarla şifrelenir ve gerektiğinde şifresi anahtarlarına Erişim hizmeti içerir. Kalan anahtarlar, şifreleme yapılan bir hizmete bir erişim denetimi İlkesi aracılığıyla erişilebilir. Bu ilke anahtarı almak için hizmet kimliği erişim verir. İlişkili bir abonelik adına çalıştıran bir Azure hizmeti bu abonelikte bir kimlik ile yapılandırılabilir. Hizmet, Azure Active Directory kimlik doğrulaması gerçekleştirmek ve kendisini abonelik adına hareket, hizmet olarak tanımlayan bir kimlik doğrulama belirteci alırsınız. Bu belirteci, ardından anahtar kasası erişim verilen bir anahtarı almak için sunulabilir.

Şifreleme anahtarlarını kullanarak işlemlerinde, bir hizmet kimliği aşağıdaki işlemlerden birini erişim verilebilir: şifre çöz, şifrele, unwrapKey, wrapKey, doğrulayın, oturum, Al, listesinde, güncelleştirme, oluşturma, alma, silme, yedekleme ve geri.

Resource Manager hizmet örneği çalışacağı hizmet kimliği kullanımda şifrelenirken veya bekleyen verilerin şifresini çözmek için bir anahtar almak için UnwrapKey (şifre çözme anahtarı almak için) ve (bir anahtarı anahtar kasasına yeni bir anahtar oluştururken eklemek için) WrapKey olması gerekir.


>[!NOTE] 
>Key Vault hakkında daha fazla ayrıntı için bkz güvenli anahtar kasası sayfanızın [Azure Key Vault belgelerindeki](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Avantajları**

- Kullanılan anahtarlar üzerinde tam denetim – şifreleme anahtarları, müşterinin denetimi altında müşterinin anahtar Kasası'nda yönetilir.
- Birden fazla hizmet için bir ana şifreleme olanağı
- Anahtar Yönetimi hizmeti için genel yönetim modelinden ayırabilirsiniz
- Hizmet ve anahtar konumunu bölgeler arasında tanımlayabilirsiniz

**Dezavantajları**

- Anahtar erişim yönetimi için tam sorumluluk müşterinin
- Müşterinin tam sorumluluk anahtar yaşam döngüsü yönetimi
- Kurulum ve yapılandırma ek yükü

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Müşteri tarafından denetlenen donanımda hizmet tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi

Bazı Azure Hizmetleri, konak bilgisayarınızı kendi anahtarını Taşı (HYOK) anahtar yönetimi modeli sağlar. Bu yönetim modu senaryolarda yararlıdır, bekleyen verileri şifrelemek ve Microsoft'un kontrolü dışında özel bir depo içindeki anahtarları yönetmek için ihtiyaç olduğunda. Bu modelde, hizmet bir dış sitesinden anahtarı almanız gerekir. Performans ve kullanılabilirlik garantileri etkilendiğini ve daha karmaşık bir yapılandırmadır. Ayrıca, şifreleme ve şifre çözme işlemleri sırasında hizmet DEK erişiminiz olduğundan bu modelin genel güvenlik Güvenceleri ne zaman Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar için benzer.  Sonuç olarak, belirli bir anahtar yönetimi gereksinimlerine sahip olmadıkları sürece bu modeli çoğu kuruluş için uygun değil. Bu sınırlamaları nedeniyle, birçok Azure hizmeti, müşteri tarafından denetlenen donanımda sunucusu tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi desteklemez.

##### <a name="key-access"></a>Anahtar erişimi

Müşteri tarafından denetlenen donanımda hizmet tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifrelemesi kullanıldığında anahtarları müşteri tarafından yapılandırılmış bir sistemde korunur. Bu model destekleyen azure Hizmetleri, bir müşteri için güvenli bir bağlantı kurmak için bir araç tarafından sağlanan anahtar deposu sağlar.

**Avantajları**

- Kullanılan kök anahtarının tam denetim – şifreleme anahtarları, müşteri tarafından sağlanan mağaza tarafından yönetilir
- Birden fazla hizmet için bir ana şifreleme olanağı
- Anahtar Yönetimi hizmeti için genel yönetim modelinden ayırabilirsiniz
- Hizmet ve anahtar konumunu bölgeler arasında tanımlayabilirsiniz

**Dezavantajları**

- Anahtar depolama alanı, güvenlik, performans ve kullanılabilirlik için tam sorumluluk
- Anahtar erişim yönetimi için tam sorumluluk
- Tam sorumluluk anahtar yaşam döngüsü yönetimi
- Devam eden bakım maliyeti önemli kurulum ve yapılandırma
- Ağ kullanılabilirliğini müşteri veri merkeziniz ile Azure veri merkezleri arasında artan bağımlı.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Microsoft bulut hizmetlerinde bekleme sırasında şifreleme

Microsoft Cloud services, tüm üç bulut modellerinde kullanılır: IaaS, PaaS, SaaS. Aşağıda, bunlar her model üzerinde nasıl uyduğunu örnekleri vardır:

- Yazılım Hizmetleri, sunucu veya Office 365 gibi bulut tarafından sağlanan uygulama olan SaaS olarak yazılım olarak adlandırılır.
- Platform Hizmetleri, depolama, analiz ve hizmet veri yolu işlevselliğini gibi şeyler için Bulutu kullanarak kendi uygulamalarını bulutta hangi müşteriler yararlanın.
- Altyapı Hizmetleri ya da (Iaas) hizmet olarak altyapı işletim sistemleri ve büyük olasılıkla ve bulutta barındırılan uygulamalar hangi müşterinin dağıtır yararlanarak diğer bulut Hizmetleri.

### <a name="encryption-at-rest-for-saas-customers"></a>SaaS müşteriler için bekleyen şifrelemenin

Yazılım olarak hizmet (SaaS) müşteriler genellikle sahip şifreleme bekleyen etkin ya da her hizmetinde kullanılabilir. Office 365, müşterilerin doğrulayın veya bekleme sırasında şifreleme etkinleştirmek birkaç seçenek vardır. Office 365 hizmetleri hakkında daha fazla bilgi için bkz. [Office 365'te şifreleme](https://docs.microsoft.com/office365/securitycompliance/encryption).

### <a name="encryption-at-rest-for-paas-customers"></a>PaaS müşteriler için bekleyen şifrelemenin

Bir platform olarak hizmet (PaaS) müşteri verilerini genellikle bir uygulama yürütme ortamında bulunan ve müşteri verilerini depolamak için bir Azure kaynak sağlayıcısı kullanılır. Şifrelemeyi rest seçenekleri görmek için kullandığınız depolama ve uygulama platformları için aşağıdaki tabloyu inceleyin. Destekleniyorsa, bekleme sırasında şifreleme etkinleştirme yönergeleri için bağlantıları her kaynak sağlayıcısı için sağlanır. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Iaas müşteriler için bekleyen şifrelemenin

Çeşitli hizmetler ve uygulamalar, altyapı (Iaas) müşteri olarak kullanımda olabilir. Iaas hizmetlerini etkinleştirebilir, Azure bölgesinde, bekleyen şifreleme barındırılan sanal makineler ve Azure Disk şifrelemesi kullanılarak VHD'ler. 

#### <a name="encrypted-storage"></a>Şifrelenmiş depolama

Gibi PaaS, Iaas çözümleri şifrelenen verileri depolayan diğer Azure Hizmetleri yararlanabilirsiniz. Bu durumlarda, her tüketilen Azure hizmeti tarafından sağlanan Rest destek şifrelemeyi etkinleştirebilirsiniz. Aşağıdaki tabloda ana depolama, hizmetleri ve uygulama platformları ve desteklenen bekleyen şifreleme modelini numaralandırır. Destekleniyorsa, bekleme sırasında şifreleme etkinleştirme hakkında yönergeler için bağlantılar sağlanır. 

#### <a name="encrypted-compute"></a>Şifrelenmiş işlem

Rest çözüm tam bir şifreleme, verilerin hiçbir zaman şifrelenmemiş biçiminde kalıcı olmasını gerektirir. Kullanımdayken, bellek, verileri yüklenirken bir sunucuda verileri yerel olarak Windows disk belleği dosyası ve kilitlenme bilgi dökümü uygulama gerçekleştirebilir herhangi bir günlük'dahil olmak üzere çeşitli yollarla kalıcı. Bu veriler bekleme durumundayken şifrelenir emin olmak için IaaS uygulamaları Azure Disk şifrelemesi bir Azure Iaas sanal makine (Windows veya Linux) ve sanal disk kullanabilirsiniz. 

#### <a name="custom-encryption-at-rest"></a>Özel bekleme sırasında şifreleme

Mümkün olduğunda, IaaS uygulamaları Azure Disk şifrelemesi ve şifreleme tüketilen tüm Azure Hizmetleri tarafından sağlanan Rest seçeneklerinin yararlanarak, önerilir. Düzensiz şifreleme gereksinimleri ya da Azure dışı tabanlı depolama, Iaas uygulamanın geliştiricisi bekleme sırasında şifreleme uygulamanız gerekebilir gibi bazı durumlarda, kendilerini içinizi rahat tutun. Iaas çözümleri daha iyi geliştiriciler, belirli Azure bileşenlerini yararlanarak Azure yönetim ve Müşteri beklentileri ile tümleştirin. Özellikle, geliştiricilerin yanı sıra güvenli anahtar depolama alanı sağlamak, müşterilerine, çoğu Azure platform Hizmetleri ile tutarlı anahtar yönetim seçeneklerini sağlamak için Azure Key Vault hizmeti kullanmanız gerekir. Ayrıca, özel çözümler şifreleme anahtarlarına erişmek hizmet hesaplarını etkinleştirmek için Azure-Managed hizmet kimlikleri kullanmanız gerekir. Geliştirici hakkında bilgi için Azure Key Vault ve yönetilen hizmet kimlikleri, ilgili SDK'ları bakın.

## <a name="azure-resource-providers-encryption-model-support"></a>Azure kaynak sağlayıcıları şifreleme modeli desteği

Microsoft Azure hizmetleri her birini veya rest modeli şifrelemesi daha fazlasını destekler. Bazı hizmetler için ancak bir veya daha fazla şifreleme modelleri geçerli olmayabilir. Müşteri tarafından yönetilen anahtar senaryolarını destekleyen hizmetler için Azure Key Vault anahtar şifreleme anahtarları için desteklenen anahtar türleri yalnızca bir kısmı desteklemiyor olabilir. Ayrıca, hizmetleri bu senaryoları ve farklı zamanlamalar temel türler için destek yayımlayabilir. Bu bölümde, Azure büyük veri depolama hizmetlerinin her biri için bu yazma sırasında rest Destek'teki şifreleme açıklanmaktadır.

### <a name="azure-disk-encryption"></a>Azure disk şifrelemesi

Azure altyapı (Iaas) hizmet olarak tüm müşteriler, Iaas Vm'leri ve diskleri Azure Disk şifrelemesi ile bekleyen şifreleme özellikleri elde edebilirsiniz. Azure Disk şifrelemesi hakkında daha fazla bilgi için bkz. [Azure Disk şifrelemesi belgeleri](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Azure Storage

Tüm Azure depolama hizmetleri (Blob Depolama, kuyruk depolama, tablo depolama ve Azure dosyaları), müşteri tarafından yönetilen anahtarlar ve istemci tarafı şifreleme destekleyen bazı hizmetler ile sunucu tarafı şifrelemesi, bekleyen destekler.  

- Sunucu tarafı: Tüm Azure Depolama Hizmetleri sunucu tarafı şifreleme varsayılan olarak hizmet tarafından yönetilen anahtarlar, uygulamaya saydamdır kullanarak etkinleştirin. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Ayrıca Azure Blob Depolama ve Azure dosyaları Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar RSA 2048 bit destekler. Daha fazla bilgi için [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).
- İstemci tarafı: Azure Blobları, tablolar ve Kuyruklar, istemci tarafı Şifreleme destekler. İstemci tarafı şifreleme kullanırken, müşterilerin verileri şifrelemek ve verileri şifrelenmiş bir blob olarak karşıya yükleyin. Anahtar Yönetimi, müşteri tarafından gerçekleştirilir. Daha fazla bilgi için [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).


#### <a name="azure-sql-database"></a>Azure SQL Veritabanı

Azure SQL veritabanı, şu anda Microsoft tarafından yönetilen hizmet tarafı ve istemci tarafı şifreleme senaryoları için bekleyen şifrelemenin destekler.

Sunucu şifreleme desteği şu anda saydam veri şifrelemesi adlı SQL özelliği aracılığıyla sağlanır. Bir Azure SQL veritabanı müşterisi sağlayan sonra TDE anahtarı otomatik olarak oluşturulur ve bunlar için yönetilir. Bekleme sırasında şifreleme veritabanı ve sunucu düzeylerinde etkinleştirilebilir. Haziran 2017'den itibaren [saydam veri şifrelemesi (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) yeni oluşturulan veritabanları üzerinde varsayılan olarak etkindir. Azure SQL veritabanı, Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar RSA 2048 bit destekler. Daha fazla bilgi için [Azure SQL veritabanı ve veri ambarı için kendi anahtarını Getir destekli saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql?view=azuresqldb-current).

Azure SQL veritabanı verilerinin istemci tarafı şifreleme aracılığıyla desteklenir [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) özelliği. Her zaman şifreli, istemci tarafından oluşturulan ve depolanan bir anahtar kullanır. Müşteriler, bir Windows sertifika deposu, Azure anahtar kasası veya yerel bir donanım güvenlik modülü ana anahtarı depolayabilirsiniz. SQL Server Management Studio kullanarak, hangi anahtar sütunu şifrelemek üzere kullanmak istediğiniz SQL kullanıcıları seçin.

|                                  |                    | **Şifreleme modeli ve anahtar yönetimi** |                    |
|----------------------------------|--------------------|-----------------------------------------|--------------------|
|                                  | **Hizmetle yönetilen anahtarı kullanarak sunucu tarafı**     | **Anahtar Kasası'nda müşteri tarafından yönetilen kullanarak sunucu tarafı**             | **İstemci tarafı yönetilen kullanma**      |
| **Depolama ve veritabanları**        |                    |                    |                    |
| Disk (Iaas)                      | -                  | Evet, RSA 2048 bit  | -                  |
| SQL Server (IaaS)                | Evet                | Evet, RSA 2048 bit  | Evet                |
| Azure SQL (veritabanı/veri ambarı) | Evet                | Evet, RSA 2048 bit  | Evet                |
| Azure SQL (veritabanı yönetilen örneği) | Evet                | Önizleme, RSA 2048 bit  | Evet                |
| Azure depolama (blok/sayfa Blobları) | Evet                | Evet, RSA 2048 bit  | Evet                |
| Azure depolama (dosyalar)            | Evet                | Evet, RSA 2048 bit  | -                  |
| Azure depolama (tablolar, kuyruklar)   | Evet                | -                  | Evet                |
| Cosmos DB (belge DB)          | Evet                | -                  | -                  |
| StorSimple                       | Evet                | -                  | Evet                |
| Backup                           | Evet                | -                  | Evet                |
| **Zeka ve analiz**   |                    |                    |                    |
| Azure Data Factory               | Evet                | -                  | -                  |
| Azure Machine Learning           | -                  | Önizleme, RSA 2048 bit | -                  |
| Azure Stream Analytics           | Evet                | -                  | -                  |
| HDInsight'ı (Azure Blob Depolama)   | Evet                | -                  | -                  |
| HDInsight (Data Lake depolama)    | Evet                | -                  | -                  |
| HDInsight için Apache Kafka       | Evet                | Önizleme, tüm RSA uzunlukları | -                  |
| Azure Data Lake Store            | Evet                | Evet, RSA 2048 bit  | -                  |
| Azure Veri Kataloğu               | Evet                | -                  | -                  |
| Power BI                         | Evet                | -                  | -                  |
| **IOT Hizmetleri**                 |                    |                    |                    |
| IoT Hub                          | -                  | -                  | Evet                |
| Service Bus                      | Evet                | -                  | Evet                |
| Event Hubs                       | Evet                | -                  | -                  |
| Event Grid                       | Evet                | -                  | -                  |


## <a name="conclusion"></a>Sonuç

Azure Services içinde depolanan müşteri verileri, Microsoft'a güvenemediği önem korumadır. Barındırılan tüm Azure Hizmetleri Rest seçeneklerine şifreleme sağlamaya kararlıdır. Zaten Azure depolama, Azure SQL veritabanı ve önemli analiz gibi temel Hizmetleri ve Destek Hizmetleri şifreleme Rest seçeneklerini belirtin. Bu hizmetlerden bazıları, denetlenen müşteri anahtarları ve istemci tarafı şifreleme hem de hizmet tarafından yönetilen anahtarlar ve şifreleme ya da destekler. Microsoft Azure hizmetlerini Rest kullanılabilirliğine şifreleme geliştirme geniş kapsamda ve yeni seçenekler Önizleme ve genel kullanılabilirlik için gelecek ay içinde planlanmaktadır.
