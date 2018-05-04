---
title: Microsoft Azure veri şifreleme çalışmıyorken-| Microsoft Docs
description: Bu makalede, Microsoft Azure veri şifreleme çalışmıyorken genel bir bakış, genel özellikleri ve genel konular sağlar.
services: security
documentationcenter: na
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: 54dc97c0d20f90d3b57b715fb21714a11e5a1525
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="azure-data-encryption-at-rest"></a>Azure veri şifreleme çalışmıyorken-
Microsoft, şirketinizin güvenlik ve uyumluluk gereksinimlerine göre verilerinizi korumak için Azure içinde birden çok araç vardır. Bu yazı odaklanır:
- Verileri Microsoft Azure üzerinde bekleyen nasıl korunur
- Bölümü veri koruma uygulamasında alma çeşitli bileşenler açıklar,
- Artıları ve eksileri farklı anahtar yönetimi koruma yaklaşımlardan inceler. 

Bekleyen şifreleme ortak güvenlik gerekli değildir. Microsoft Azure avantajı, uygulama ve Yönetim maliyetini ve bir özel anahtar yönetimi çözümü riskini gerek kalmadan kuruluşların bekleyen şifreleme gerçekleştirebilirsiniz ' dir. Kuruluşlar tamamen bekleyen şifreleme yönetmek Azure izin vererek seçeneğiniz vardır. Ayrıca, kuruluşlar yakından şifreleme ya da şifreleme anahtarlarını yönetmek için çeşitli seçenekleriniz vardır.

## <a name="what-is-encryption-at-rest"></a>Bekleyen Şifreleme nedir?
Kalıcı, bekleyen şifreleme şifreleme kodlama (), veri şifreleme için ifade eder. Rest tasarımlar Azure, şifreleme, şifreleme ve büyük miktarlarda verileri hızlı bir şekilde basit bir kavramsal model göre şifresini çözmek için simetrik şifreleme kullanın:

- Simetrik şifreleme anahtarını, kalıcı olarak verileri şifrelemek için kullanılır 
- Aynı şifreleme anahtarını bellek kullanmak için readied gibi bu verilerin şifresini çözmek için kullanılır
- Veri bölümlendirilebilir ve farklı anahtarlar her bölüm için kullanılabilir
- Anahtarları belirli kimlikleri erişimi sınırlayan ve anahtar kullanımı günlüğü erişim denetimi ilkeleri ile güvenli bir konumda depolanması gerekir. Veri şifreleme anahtarları genellikle daha fazla erişimi sınırlamak için asimetrik şifreleme ile şifrelenmiş (ele *anahtar hiyerarşi*, bu makalenin ilerisinde yer)

Yukarıdaki bekleyen şifreleme ortak üst düzey öğelerini açıklar. Uygulamada, ölçek ve kullanılabilirlik çıkışların yanı sıra, yönetim ve denetimi senaryoları ek yapılarını gerektirir. Microsoft Azure şifreleme Rest kavramları ve bileşenleri aşağıda açıklanmıştır.

## <a name="the-purpose-of-encryption-at-rest"></a>Bekleyen şifreleme amacı
Bekleyen şifreleme verileri (yukarıda açıklandığı gibi.) çalışmıyorken veri koruma sağlamak üzere tasarlanmıştır Çalışmıyorken veri saldırıları verinin depolanma biçimini donanımına fiziksel erişim elde etme girişimi içerir ve içerilen verileri tehlikeye. Bu tür bir saldırısında, bir sunucunun sabit disk sabit sürücüyü kaldırmak bir saldırgan izin vererek bakım sırasında mishandled. Daha sonra saldırgan sabit sürücü verilere erişme girişimi için kendi denetimindeki bir bilgisayara yerleştirin. 

Bekleyen şifreleme saldırgan şifrelenmemiş erişimini engellemek için tasarlanmıştır, diskteki verileri sağlayarak veriler şifrelenir. Bir saldırganın bir sabit sürücü gibi ile elde etmek için veri ve şifreleme anahtarları erişim yok şifrelenmiş olsaydı, saldırganın veri harika zorlanmadan tehlikeye değil. Böyle bir senaryoda, bir saldırganın saldırılarına karşı daha karmaşık olan şifrelenmiş verileri girişiminde yoktur ve erişme fazla kaynak tüketen verileri bir sabit sürücüde şifresiz. Bu nedenle, bekleyen şifreleme önerilir ve çoğu kuruluş için yüksek öncelikli gereksinimdir. 

Bazı durumlarda, bir kuruluşun gerektiğinden veri denetim ve uyumu çalışmaları için bekleyen şifreleme gereklidir. Endüstri ve devlet düzenlemeleri HIPAA, PCI ve FedRAMP, gibi veri koruma ve şifreleme gereksinimleri ile ilgili belirli güvenlik önlemleri düzenlenmesine. Çoğu bu düzenlemeler için bekleyen şifreleme uyumlu veri yönetimi ve koruması için gerekli bir zorunlu ölçüsüdür. 

Uyumluluk ve Mevzuat gereklilikleri ek olarak bekleyen şifreleme savunma platformu özelliği algılanan. Microsoft Hizmetleri, uygulamaları, uyumlu bir platform sağlar ve verileri, kapsamlı tesis ve fiziksel güvenliği, veri erişim denetimi ve denetim olsa da, başka biri durumda "çakışan" ek güvenlik önlemleri sağlamak önemlidir Güvenlik başarısız ölçer. Bekleyen şifreleme bir böyle bir ek koruma mekanizması sağlar.

Microsoft bulut hizmetlerinde ve şifreleme anahtarlarının uygun yönetilebilirlik müşteriler sağlamak ve şifreleme anahtarları kullanıldığında gösteren günlüklerine erişmek için rest seçenekleri şifreleme sağlamayı taahhüt eder. Ayrıca, Microsoft, varsayılan olarak şifrelenen tüm müşteri verilerine yapma hedefe çalışmaktadır.

## <a name="azure-encryption-at-rest-components"></a>Rest bileşenleri Azure şifreleme

Daha önce açıklandığı gibi bekleyen şifreleme diskte kalıcı veri gizli şifreleme anahtarıyla şifrelenir hedefidir. Bu hedef güvenli anahtar oluşturma, depolama, elde etmek için erişim denetimi ve şifreleme anahtarlarının yönetim sağlanmalıdır. Cinsinden ayrıntıları değişebilir ancak geri kalan uygulamaları adresinde Azure Hizmetleri şifreleme açıklanabilir sonra aşağıdaki çizimde gösterilen kavramları aşağıda.

![Bileşenler](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

Bu anahtarlar için erişim denetimi ve şifreleme anahtarları depolama konumunu rest modeli bir şifreleme taşımaktadır. Anahtarların yüksek oranda güvenli ancak belirtilen kullanıcılar tarafından yönetilebilir ve belirli hizmetler için kullanılabilir olması gerekir. Azure Hizmetleri için Azure anahtar kasası önerilen anahtar depolama çözümü ve hizmetleri arasında ortak bir yönetim deneyimi sağlar. Anahtarları saklanır ve anahtar kasalarını içinde yönetilir ve bir anahtar kasası erişim kullanıcı ve hizmet için verilebilir. Azure anahtar kasası, müşteri tarafından yönetilen bir şifreleme anahtarı senaryolarda kullanım için anahtarların müşteri oluşturma veya içeri aktarma müşteri anahtarların destekler.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Active Directory hesaplarınızı Azure anahtar kasasında depolanan anahtarları yönetmek veya bekleyen şifreleme ve şifre çözme, şifreleme için erişmeye kullanma izni verilebilir. 

### <a name="key-hierarchy"></a>Anahtar hiyerarşisi

Birden fazla şifreleme anahtarı rest uygulaması bir şifreleme kullanılır. Asimetrik şifreleme anahtarı erişim ve Yönetim için gereken kimlik doğrulama ve güven oluşturmak için kullanışlıdır. Simetrik şifreleme toplu şifreleme ve şifre çözme, daha güçlü şifreleme ve daha iyi performans için izin vermek için daha verimli olur. Bir anahtar değiştirilmelidir, ayrıca, tek şifreleme anahtar kullanımını sınırlama anahtar tehlikeye risk ve yeniden şifreleme maliyeti azaltır. Asimetrik ve simetrik şifreleme avantajlarından yararlanın ve kullanım ve tek bir anahtar riskini sınırlandırmak için aşağıdaki anahtarları türlerini oluşan bir anahtar hiyerarşi rest modelleri adresindeki Azure şifrelemeleri kullanın:

- **Veri şifreleme anahtarı (DEK)** – bir bölüm veya veri bloğu şifrelemek için kullanılan simetrik AES256 anahtar.  Tek bir kaynak birçok bölüm ve çok sayıda veri şifreleme anahtarları olabilir. Her veri bloğu farklı bir anahtarla şifreleme şifre analiz saldırıları daha zor hale getirir. DEKs erişimi şifreleme ve belirli bir blok çözme kaynak sağlayıcısı veya uygulama örneği tarafından gereklidir. Yeni bir anahtarla bir DEK değiştirildiğinde yalnızca kendi ilişkili bloğundaki verileri yeniden yeni anahtarla şifrelenmiş olması gerekir.
- **Anahtar şifreleme anahtarı (KEK)** – veri şifreleme anahtarları şifrelemek için kullanılan bir asimetrik şifreleme anahtarı. Anahtar şifreleme anahtarı veri şifreleme anahtarları kendilerini şifrelenip denetlenen izin verir. KEK erişimi varlık DEK gerektiren varlık farklı olabilir. Bu, belirli bir bölüm için her DEK sınırlı erişimi sağlamak amacıyla DEK erişimi Aracısı bir varlık sağlar. KEK DEKs şifresini çözmek için gerekli olduğundan, KEK tek bir nokta olarak DEKs etkili bir şekilde KEK silinmesini tarafından silinebilir etkili bir şekilde kalır.

Anahtar şifreleme anahtarları ile şifrelenmiş veri şifreleme anahtarları ayrı olarak depolanır ve bu anahtarla şifrelenen tüm veri şifreleme anahtarları erişim anahtarı şifreleme anahtarına sahip yalnızca bir varlık alabilir. Anahtar depolama başka modellerinin desteklenir. Her model daha ayrıntılı bir sonraki bölümde aşağıdakiler ele alınacaktır.

## <a name="data-encryption-models"></a>Veri şifreleme modelleri

Çeşitli şifreleme modelleri ve Artıları ve eksileri anlamanız, nasıl Azure çeşitli kaynak sağlayıcıları bekleyen şifreleme uygulamak anlamak için önemlidir. Bu tanımları Azure ortak dil ve sınıflandırma emin olmak için tüm kaynak sağlayıcıları arasında paylaşılır. 

Sunucu tarafı şifrelemesi için üç senaryo vardır:

- Sunucu tarafı şifreleme hizmeti yönetilen tuşlarını kullanma
    - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemleri
    - Microsoft anahtarları yönetir
    - Tam bulut işlevi

- Müşteri tarafından yönetilen anahtarları Azure anahtar kasası kullanarak sunucu tarafı şifreleme
    - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemleri
    - Azure anahtar kasası anahtarlarında müşteri denetler
    - Tam bulut işlevi

- Müşteri tarafından denetlenen donanımda müşteri tarafından yönetilen tuşlarını kullanarak sunucu tarafı şifreleme
    - Azure kaynak sağlayıcıları şifreleme ve şifre çözme işlemleri
    - Müşteri tarafından denetlenen donanım anahtarları müşteri denetler
    - Tam bulut işlevi

İstemci tarafı şifreleme için aşağıdakileri göz önünde bulundurun:

- Azure Hizmetleri şifresi çözülmüş veriler göremiyorum
- Müşteriler yönetin ve anahtarları şirket içi depolama (veya diğer depoları güvenli). Anahtarları Azure hizmetlerine kullanılabilir değil
- Azaltılmış bulut işlevi

İki ana gruplara ayırın Azure desteklenen şifreleme modellerinde: "İstemci şifrelemesi" ve "daha önce bahsedilen sunucu tarafı şifreleme" olarak. Rest modeli şifreleme bağımsız kullanılan, Azure Hizmetleri zaman TLS ya da HTTPS gibi güvenli bir taşıma kullanımını, unutmayın. Bu nedenle, aktarım şifreleme aktarım iletişim kuralı tarafından ele alınması gereken ve kullanmak için rest modeli hangi şifreleme belirlemede önemli bir etken olmamalıdır.

### <a name="client-encryption-model"></a>İstemci şifreleme modeli

İstemci şifreleme modeli kaynak sağlayıcısı ya da Azure dışında hizmet veya çağıran uygulama tarafından yapılması şifreleme ifade eder. Şifreleme, Azure hizmet uygulaması tarafından ya da müşteri veri merkezinde çalışan bir uygulama tarafından gerçekleştirilebilir. Her iki durumda da bu şifreleme modeli yararlanırken verileri herhangi bir şekilde şifrelemek veya şifreleme anahtarlarının erişimi olanağı olmadan verilerin şifrelenmiş bir blobu Azure kaynak sağlayıcısı alır. Bu modelde, anahtar yönetimi arama hizmeti/uygulama tarafından yapılır ve Azure hizmetine opak.

![İstemci](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Sunucu tarafı şifreleme modeli

Sunucu tarafı şifreleme modelleri Azure hizmeti tarafından gerçekleştirilen şifreleme bakın. Bu modelde, kaynak sağlayıcısı şifreleme ve şifre çözme işlemleri gerçekleştirir. Örneğin, Azure Storage veri düz metin işlemlerinde alabilirsiniz ve şifreleme ve şifre çözme dahili olarak gerçekleştirir. Kaynak sağlayıcısını Microsoft veya sağlanan yapılandırmasına bağlı müşteri tarafından yönetilen şifreleme anahtarlarını kullanabilir. 

![Sunucu](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Sunucu tarafı şifreleme anahtar yönetimi modelleri

Her sunucu tarafı şifreleme rest modelleri farklı anahtar yönetimi özelliklerini anlamına gelir. Bu içerir nerede ve nasıl şifreleme anahtarları oluşturulur ve erişim modelleri ve anahtar döndürme yordamları yanı sıra depolanır. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Hizmet tarafından yönetilen tuşlarını kullanarak sunucu tarafı şifreleme

Birçok müşteri için bekleyen olduğunda, verilerin şifrelendiğinden emin olun önemli gereksinimdir. Sunucu tarafı şifreleme hizmeti yönetilen anahtarları kullanarak bu model için şifreleme müşterilerin belirli bir kaynak (depolama hesabı, SQL DB, vb.) işaretlemek olanak tanır ve anahtar verme, döndürme ve yedekleme gibi tüm anahtar yönetimi özelliklerini Microsoft'a bırakarak sağlar. Bekleyen şifreleme desteği genellikle çoğu Azure Hizmetleri şifreleme anahtarları Azure Yönetimi boşaltma bu modelini destekler. Azure kaynak sağlayıcısı anahtarları oluşturur, bunları güvenli depolama yerleştirir ve bunları gerektiğinde alır. Bu hizmet anahtarları tam erişimi vardır ve hizmetin kimlik bilgileri yaşam döngüsü yönetimi üzerinde tam denetime sahip anlamına gelir.

![Yönetilen](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Yönetilen hizmet anahtarları bu nedenle hızlı bir şekilde kullanarak sunucu tarafı şifreleme düşük yüküyle REST müşteriye şifreleme olması gerekir karşılar. Kullanılabilir olduğunda bir müşteri genellikle hedef abonelik ve kaynak sağlayıcısı için Azure Portalı'nı açar ve verilerin şifrelenmesi için istediğiniz belirten bir kutusunu denetler. Bazı kaynak yöneticileri sunucu tarafı şifreleme hizmeti tarafından yönetilen anahtarlarla varsayılan olarak açıktır. 

Sunucu tarafı şifreleme Microsoft tarafından yönetilen anahtarlarla hizmet depolamak için tam erişimi vardır ve anahtarları yönetir anlamına gelmez. Bazı müşteriler büyük güvenlik sağlayabilirsiniz eşitleyerek çünkü anahtarları yönetmek isteyebilirsiniz, ancak bu modeli değerlendirirken, maliyet ve bir özel anahtar depolama çözümü ile ilişkili riski düşünülmelidir. Çoğu durumda, bir kuruluşun kaynak kısıtlamaları veya bir şirket içi çözüm risklerini rest anahtarları şifrelemeyi bulut yönetimini riskini büyük olabilir belirleyebilir.  Ancak, bu model oluşturma veya şifreleme anahtarlarının yaşam döngüsü kontrol etmek veya farklı personel (yani, ayrımı hizmetini yönetme olandan bir hizmetin şifreleme anahtarlarını yönetmek için gereksinimleri olan kuruluşlar için yeterli olmayabilir. Anahtar Yönetim hizmeti için genel yönetim modelden).

##### <a name="key-access"></a>Anahtar erişimi

Sunucu tarafı şifreleme hizmeti yönetilen anahtarlarla kullanıldığında, anahtar oluşturma, depolama ve Erişim hizmeti hizmet tarafından yönetilen tüm öğeler. Genellikle, temel Azure kaynak sağlayıcıları veri şifreleme anahtarları yakın veri deposunda depolamak ve hızlı bir şekilde kullanılabilir ve erişilebilir anahtar şifreleme anahtarları sırasında güvenli bir iç deposunda saklanır.

**Avantajları**

- Basit Kurulumu
- Microsoft anahtar döndürme, yedekleme ve artıklık yönetir
- Müşteri, uygulama veya bir özel anahtar yönetimi düzeni riskini ile ilişkili maliyet yok.

**Olumsuz yönleri**

- Şifreleme anahtarları (anahtar belirtimi, yaşam döngüsü, iptal, vb.) üzerinde hiçbir müşteri denetimi
- Anahtar Yönetim hizmeti için genel yönetim modelden tutma olanağı

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Müşteri kullanarak sunucu tarafı şifreleme anahtarları Azure anahtar Kasası'yönetilen 

Rest ve denetim verileri şifrelemek için gereksinim olduğu senaryolar için şifreleme anahtarları müşterileri anahtar kasasına müşteri yönetilen tuşlarını kullanarak sunucu tarafı şifreleme kullanabilir. Bazı hizmetler Azure anahtar kasası yalnızca kök anahtar şifreleme anahtarı depolamak ve şifrelenmiş veri şifreleme anahtarı bir iç konumuna yakın veri depolamak olabilirsiniz. Uygulamasında senaryo müşteriler kendi anahtarları getir (BYOK – kendi anahtarını Getir), anahtar kasası veya yenilerini oluşturmak ve bunları istediğiniz kaynakları şifrelemek için kullanın. Kaynak sağlayıcısı şifreleme ve şifre çözme işlemleri gerçekleştirirken tüm şifreleme işlemleri için kök anahtarı olarak yapılandırılmış olan anahtar kullanır. 

##### <a name="key-access"></a>Anahtar erişimi

Sunucu tarafı şifreleme modeli yönetilen müşteri anahtarları Azure anahtar Kasası'ile şifrelemek ve gerektiğinde şifresini çözmek için anahtarlarına Erişim hizmeti içerir. Rest anahtarları şifreleme yapılan bir hizmete bir erişim denetimi İlkesi aracılığıyla erişilebilir. Bu ilke anahtarı almak için hizmet kimlik erişim verir. Bu aboneliğin bir kimlikle ilişkili bir abonelik adına çalıştıran bir Azure hizmeti yapılandırılabilir. Hizmet, Azure Active Directory kimlik doğrulaması yapmak ve kendisini abonelik adına hareket hizmet olarak tanımlayan bir kimlik doğrulama belirtecini alma. Bu belirteç, anahtar kasası erişim verilen bir anahtarı edinmek için sonra sunulabilir.

Şifreleme anahtarları kullanılarak işlemleri için aşağıdaki işlemlerden birini erişimi bir hizmet kimliği verilebilir: şifre çözme, wrapKey unwrapKey, şifreleme, doğrulayın, oturum, almak, listesinde, güncelleştirme, oluşturma, alma, silme, yedekleme ve geri.

Resource Manager hizmet örneği olarak çalışacağını hizmet kimliği kullanımda şifrelemek veya durağan verilerin şifresini çözmek için bir anahtar almak için (şifre çözme için anahtar) UnwrapKey ve WrapKey (bir anahtar anahtar kasasının yeni bir anahtar oluştururken eklemek için) olması gerekir.


>[!NOTE] 
>Anahtar kasası hakkında daha fazla ayrıntı için bkz: yetkilendirmesi güvenli anahtar kasası sayfasında [Azure anahtar kasası belgelerine](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Avantajları**

- Anahtarlar üzerinde tam denetim kullanılan – şifreleme anahtarları Müşteri'nin anahtar kasası Müşteri'nin denetimindeki yönetilir.
- Birden fazla hizmet için bir ana şifreleme olanağı
- Anahtar Yönetim hizmeti için genel yönetim modelden ayırabilirsiniz
- Hizmet ve anahtar konumu bölgeler arasında tanımlayabilir

**Olumsuz yönleri**

- Müşteri, anahtarı erişim yönetimi için tam sorumluluk sahiptir.
- Müşteri, anahtar yaşam döngüsü yönetimi için tam sorumluluk sahiptir.
- Ek kurulum ve yapılandırma yükü

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Hizmetini kullanarak sunucu tarafı şifreleme anahtarları denetlenen müşteri donanımında yönetilen

Kalan verileri şifrelemek ve Microsoft'un denetimi dışında özel bir depo anahtarlarında yönetmek için gereksinim olduğu senaryolar için bazı Azure Hizmetleri ana bilgisayarınızı kendi anahtarını Taşı (HYOK) anahtar yönetim modeli sağlar. Bu model, hizmet anahtarı bir dış sitesinden almanız gerekir ve bu nedenle performansı ve kullanılabilirliği garanti etkilenen yapılandırma daha karmaşıktır. Ayrıca, şifreleme ve şifre çözme işlemleri sırasında hizmet DEK erişiminiz olduğundan bu modelin genel güvenlik garantileri anahtarları Azure anahtar kasası yönetilen müşteri olduğunda benzer.  Sonuç olarak, belirli anahtar yönetimi gereksinimleri, araya sahip oldukları sürece bu modeli çoğu kuruluş için uygun değil. Bu sınırlamaları nedeniyle, çoğu Azure Hizmetleri denetlenen müşteri donanımında sunucusu yönetilen tuşlarını kullanarak sunucu tarafı şifrelemeyi desteklemez.

##### <a name="key-access"></a>Anahtar erişimi

Yönetilen hizmet anahtarları denetlenen müşteri donanımında kullanarak sunucu tarafı şifreleme kullanıldığında anahtarları müşteri tarafından yapılandırılan bir sistemde saklanır. Bir müşteri güvenli bir bağlantı kurarak yolu anahtar deposunda sağlanan bu modelini destekleyen azure hizmetleri sağlar.

**Avantajları**

- Kök anahtarı üzerinde tam denetim kullanılan – şifreleme anahtarları sağlanan müşteri mağaza tarafından yönetilir
- Birden fazla hizmet için bir ana şifreleme olanağı
- Anahtar Yönetim hizmeti için genel yönetim modelden ayırabilirsiniz
- Hizmet ve anahtar konumu bölgeler arasında tanımlayabilir

**Olumsuz yönleri**

- Anahtar depolama, güvenlik, performans ve kullanılabilirlik tam sorumluluk
- Tam sorumluluk anahtar erişim yönetimi
- Tam sorumluluk anahtar yaşam döngüsü yönetimi
- Önemli Kurulum, yapılandırma ve devam eden bakım maliyeti
- Ağ kullanılabilirliğini müşteri veri merkezi ve Azure veri merkezleri arasında artan bağımlılığı.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Microsoft bulut hizmetlerinde bekleyen şifreleme

Microsoft Cloud services, tüm üç bulut modellerinde kullanılır: Iaas, PaaS, SaaS. Aşağıda, bunların her model üzerinde nasıl uyduğunu örnekler vardır:

- Yazılım Hizmetleri, bir sunucu veya Office 365 gibi bulut tarafından sağlanan uygulamaya sahip SaaS olarak yazılım olarak adlandırılır.
- Platform Hizmetleri kullanılarak bulut depolama, analiz ve hizmet veri yolu işlevselliğini gibi şeyler için hangi müşterilerin kendi uygulamalarında bulutta yararlanın.
- Altyapı hizmetleri veya hangi müşteri (Iaas) hizmet olarak altyapı işletim sistemlerini dağıtmak ve Bulut ve muhtemelen barındırılan uygulamalara diğer bulut hizmetlerini kullanır.

### <a name="encryption-at-rest-for-saas-customers"></a>SaaS müşteriler için bekleyen şifreleme

Bir hizmet (SaaS) müşteri olarak genellikle yazılımınız şifreleme REST etkin ya da her hizmetindeki kullanılabilir. Office 365 müşterilerin doğrulamak veya REST şifrelemeyi etkinleştirmek birkaç seçenek vardır. Office 365 hizmetleri hakkında bilgi için Office 365 için veri şifreleme teknolojileri bakın.

### <a name="encryption-at-rest-for-paas-customers"></a>PaaS müşteriler için bekleyen şifreleme

Platform olarak hizmet (PaaS) Müşteri'nin veri genellikle uygulama yürütme ortamında bulunur ve hiçbir Azure kaynak sağlayıcısı müşteri verilerini depolamak için kullanılır. Bekleyen şifreleme görmek için kullanabileceğiniz seçenekler kullandığınız depolama ve uygulama platformlar için aşağıdaki tabloyu inceleyin. Desteklenen durumlarda, her kaynak sağlayıcısı için bekleyen şifreleme etkinleştirmekle ilgili yönergeler için bağlantılar sağlanır. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Iaas müşteriler için bekleyen şifreleme

Altyapı (Iaas) müşteri olarak hizmetler ve uygulamalar, çeşitli kullanımda olabilir. Iaas Hizmetleri etkinleştirebilir kendi Azure bekleyen şifreleme barındırılan sanal makineleri ve VHD Azure Disk Şifrelemesi'ni kullanarak. 

#### <a name="encrypted-storage"></a>Şifrelenmiş depolama

PaaS gibi Iaas çözümleri şifrelenen verileri depolamak diğer Azure hizmetleriyle yararlanabilirsiniz. Bu durumlarda, her tüketilen Azure hizmeti tarafından sağlanan gibi Rest destek şifrelemeyi etkinleştirebilirsiniz. Ana depolama, hizmetleri ve uygulama platformları ve desteklenen bekleyen şifreleme modelinin aşağıdaki tabloya numaralandırır. Desteklenen durumlarda bekleyen şifreleme etkinleştirmekle ilgili yönergeler için bağlantılar sağlanmaktadır. 

#### <a name="encrypted-compute"></a>Şifrelenmiş işlem

Rest çözüm tam bir şifreleme, verileri hiçbir zaman şifresiz biçimde kalıcı olmasını gerektirir. Kullanımdayken, bellek, verileri yüklenirken bir sunucudaki verileri yerel olarak Windows disk belleği dosyası, bir kilitlenme dökümü ve uygulama gerçekleştirebilir günlük dahil olmak üzere çeşitli yollarla kalıcı. Bu veriler bekleme sırasında şifrelenir emin olmak için Iaas uygulamaları Azure Disk şifrelemesi bir Azure Iaas sanal makine (Windows veya Linux) ve sanal disk kullanabilirsiniz. 

#### <a name="custom-encryption-at-rest"></a>Bekleyen özel şifreleme

Mümkün olduğunda, Iaas uygulamaları Azure Disk şifrelemesi ve şifreleme tüketilen Azure Hizmetleri tarafından sağlanan Rest seçeneklerini adresindeki yararlanan olduğunu önerilir. Düzensiz şifreleme gereksinimleri veya Azure dışı tabanlı depolama, bir Iaas uygulama geliştiricisi şifreleme uygulamak gerekebilir gibi bazı durumlarda, kendilerini bekletin. Iaas çözümleri daha iyi geliştiriciler, Azure belirli bileşenlerini yararlanarak Azure yönetimi ve müşteri beklentilerini ile tümleştirin. Özellikle, geliştiriciler yanı sıra güvenli anahtar depolama alanı sağlamak, çoğu Azure platform hizmetlerini ile tutarlı anahtar yönetim seçeneklerini müşterilerine sağlamak için Azure anahtar kasası hizmetindeki kullanmanız gerekir. Ayrıca, özel çözümler şifreleme anahtarları erişmek hizmet hesaplarını etkinleştirmek için Azure yönetilen hizmet kimliği kullanmanız gerekir. Azure anahtar kasası ve yönetilen hizmet kimlikleri hakkında bilgi için geliştirici kendi ilgili SDK'ları bakın.

## <a name="azure-resource-providers-encryption-model-support"></a>Azure kaynak sağlayıcıları şifreleme modeli desteği

Microsoft Azure hizmetleri her bir veya daha fazla rest modelleri şifrelemeyi destekler. Bazı hizmetler için ancak, bir veya daha fazla şifreleme modelleri geçerli olmayabilir. Ayrıca, değişik zamanlamalar, bu senaryolar için Destek Hizmetleri yayımlayabilir. Bu bölümde rest destek zaman büyük Azure veri depolama hizmetlerinin her biri için bu yazma sırasında şifreleme açıklanmaktadır.

### <a name="azure-disk-encryption"></a>Azure disk şifrelemesi

Bir hizmet (Iaas) olarak Azure altyapısı kullanarak müşteri özellikleri Iaas Vm'leri ve Azure Disk şifrelemesi ile diskleri bekleyen şifreleme elde edebilirsiniz. Azure diski hakkında daha fazla bilgi için bkz: şifreleme [Azure Disk şifrelemesi belgelerine](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Azure Storage

Tüm Azure Storage Hizmetleri (Blob storage, kuyruk depolama, Table storage ve Azure dosyaları), REST, sunucu tarafı şifreleme anahtarları müşteri tarafından yönetilen ve istemci tarafı şifreleme destekleyen bazı hizmetler ile destekler.  

- Sunucu tarafı: Tüm Azure Storage Hizmetleri hizmet tarafından yönetilen, uygulamanız için saydamdır tuşlarıyla varsayılan olarak sunucu tarafı şifrelemeyi etkinleştirin. Daha fazla bilgi için bkz: [bekleyen veri için Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Ayrıca Azure Blob Depolama ve Azure dosyaları müşteri tarafından yönetilen anahtarları Azure anahtar kasası destekler. Daha fazla bilgi için bkz: [depolama hizmeti şifrelemesi müşteri tarafından yönetilen anahtarları Azure anahtar kasası kullanarak](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption-customer-managed-keys).
- İstemci-tarafı: Azure BLOB'ları, tabloları ve kuyrukları istemci tarafı Şifreleme destekler. İstemci tarafı şifreleme kullanırken, müşterilerin verileri şifrelemek ve verileri şifrelenmiş bir blobu olarak yükleyin. Anahtar Yönetimi, müşteri tarafından gerçekleştirilir. Daha fazla bilgi için bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).


#### <a name="sql-azure"></a>SQL Azure

SQL Azure şu anda Microsoft tarafından yönetilen hizmet tarafı ve istemci tarafı şifreleme senaryoları için bekleyen şifreleme desteklemektedir.

Sunucu şifreleme desteği şu anda saydam veri şifreleme adlı SQL özelliği sağlanır. Bir SQL Azure müşterisi etkinleştirir sonra TDE anahtarı otomatik olarak oluşturulur ve bunlar için yönetilir. Bekleyen şifreleme ve veritabanı ve sunucu düzeylerinde etkinleştirilebilir. Haziran 2017'dan sonra [saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) yeni oluşturulan veritabanı üzerinde varsayılan olarak etkinleştirilir.

İstemci tarafı şifreleme SQL Azure veri aracılığıyla desteklenir [her zaman şifreli](https://msdn.microsoft.com/library/mt163865.aspx) özelliği. Her zaman şifreli oluşturulan ve saklanan bir anahtar istemci tarafından kullanır. Müşteriler, Windows sertifika deposu, Azure anahtar kasası veya yerel bir donanım güvenlik modülü ana anahtar depolayabilirsiniz. SQL Server Management Studio'yu kullanarak, hangi sütunun şifrelemek için kullanmak istediğiniz hangi anahtar SQL kullanıcıları seçin.

|                                  |                |                     | **Şifreleme modeli**             |                              |        |
|----------------------------------|----------------|---------------------|------------------------------|------------------------------|--------|
|                                  |                |                     |                              |                              | **İstemci** |
|                                  | **Anahtar Yönetimi** | **Hizmet anahtarı yönetilen** | **Anahtar kasasına yönetilen müşteri** | **Yönetilen müşterinin şirket içi** |        |
| **Depolama ve veritabanları**            |                |                     |                              |                              |        |
| Disk (Iaas)                      |                | -                   | Evet                          | Evet*                         | -      |
| SQL Server (IaaS)                |                | Evet                 | Evet                          | Evet                          | Evet    |
| SQL Azure (PaaS)                 |                | Evet                 | Evet                          | -                            | Evet    |
| Azure depolama (blok/sayfa Bloblarını) |                | Evet                 | Evet                          | -                            | Evet    |
| Azure depolama (dosyaları)            |                | Evet                 | Evet                          | -                            | -      |
| Azure depolama (tablolar, kuyruklar)   |                | Evet                 | -                            | -                            | Evet    |
| Cosmos DB (belge DB)          |                | Evet                 | -                            | -                            | -      |
| StorSimple                       |                | Evet                 | -                            | -                            | Evet    |
| Backup                           |                | -                   | -                            | -                            | Evet    |
| **Intelligence ve analizi**       |                |                     |                              |                              |        |
| Azure Data Factory               |                | Evet                 | -                            | -                            | -      |
| Azure Machine Learning           |                | -                   | Önizleme                      | -                            | -      |
| Azure Stream Analytics           |                | Evet                 | -                            | -                            | -      |
| Hdınsights (Azure Blob Depolama)  |                | Evet                 | -                            | -                            | -      |
| Hdınsights (Data Lake Store)   |                | Evet                 | -                            | -                            | -      |
| Azure Data Lake Store            |                | Evet                 | Evet                          | -                            | -      |
| Azure Veri Kataloğu               |                | Evet                 | -                            | -                            | -      |
| Power BI                         |                | Evet                 | -                            | -                            | -      |
| **IOT Hizmetleri**                     |                |                     |                              |                              |        |
| IoT Hub’ı                          |                | -                   | -                            | -                            | Evet    |
| Service Bus                      |                | Evet              | -                            | -                            | Evet    |
| Event Hubs                       |                | Evet             | -                            | -                            | -      |


## <a name="conclusion"></a>Sonuç

Azure Services içinde depolanan Müşteri verilerinin Microsoft'a en önemli önem korumadır. Barındırılan tüm Azure Hizmetleri Rest seçenekleri şifreleme sağlamayı taahhüt. Zaten Azure Storage, SQL Azure ve anahtar analizi gibi temel Hizmetleri ve Destek Hizmetleri şifreleme Rest seçenekleri sağlar. Bu hizmetlerden bazılarını denetlenen müşteri anahtarları ve istemci tarafı şifreleme desteği olarak yönetilen anahtarlar ve şifreleme hizmeti. Microsoft Azure Hizmetleri Rest kullanılabilirliğine şifreleme geliştirme geniş ve gelecek aylarda Önizleme ve genel kullanılabilirlik için yeni seçenekler aşağıda verilmiştir.

