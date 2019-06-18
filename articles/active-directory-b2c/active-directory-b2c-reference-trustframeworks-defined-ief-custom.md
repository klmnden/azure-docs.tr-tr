---
title: Başvuru - Azure Active Directory B2C'de güven çerçeveleri | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri ve kimlik deneyimi çerçevesi hakkındaki bir konu.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 47e45a7dac8abc65f414fedd0fd910e3a7a78113
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66508817"
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Azure AD B2C kimlik deneyimi çerçevesi güven çerçevelerle tanımlayın

Azure Active Directory B2C kimlik deneyimi çerçevesi kullanın (Azure AD B2C) özel ilkeler, merkezi bir hizmete kuruluşunuzla sağlar. Bu hizmet, büyük bir topluluk içinde Kimlik Federasyonu ilgi karmaşıklığını azaltır. Tek bir güven ilişkisi ve bir meta veri değişimi için karmaşıklığı azalır.

Kimlik deneyimi çerçevesi şu soruları yanıtlamak için etkinleştirmek için kullandığınız azure AD B2C özel ilkeler:

- Yasal, güvenlik, gizlilik ve bağlı gereken veri koruma ilkeleri nelerdir?
- Kullanan kişiler ve onaylanmış bir katılımcısı olma işlemler nelerdir?
- Bunlar, akredite kimlik bilgileri sağlayıcıları (diğer adıyla "Talep Sağlayıcıları") olan ve neler sağlar?
- Akredite bir bağlı olan taraflar kim (ve hangi bunlar isteğe bağlı olarak, gerek)?
- Teknik nedir "on"kablo katılımcıları birlikte çalışabilirlik gereksinimlerini?
- Dijital kimlik bilgileri değişimi için uygulanmasını gerektiren işletimsel "çalışma zamanı" kuralları nelerdir?

Tüm soruları yanıtlamak için kimlik deneyimi çerçevesi kullanımı güven Framework (TF) kullanan bir Azure AD B2C özel ilkeleri oluşturun. Tento konstruktor je ve neler sağladığını düşünelim.

## <a name="understand-the-trust-framework-and-federation-management-foundation"></a>Güven çerçevesi ve Federasyon Yönetimi foundation anlama

Güven kimlik, güvenlik, gizlilik ve veri koruma ilkeleri bir topluluk ilgi katılımcıları uygun olmalıdır yazılı belirtimini çerçevedir.

Federe kimlik, son kullanıcı kimlik güvencesi Internet ölçeğinde elde etmek için bir temel sağlar. Üçüncü taraf için kimlik yönetimi için temsilci seçme, son kullanıcı için tek bir dijital kimlik birden fazla bağlı olan taraflar ile yeniden kullanılabilir.  

Kimlik güvencesi kimlik sağlayıcı (IDP) ve öznitelik sağlayıcıları (AtPs) için belirli güvenlik, gizlilik ve operasyonel ilke ve uygulamaları uyması gerekir.  Doğrudan incelemeleri gerçekleştiremiyorsanız bağlı olan taraflar Idp'yi ve çalışmak üzere seçtikleri AtPs güven ilişkileri (Rp'ler) geliştirmeniz gerekir.  

Tüketicileri ve sağlayıcıları dijital kimlik bilgisi sayısı arttıkça, bu güven ilişkileri ikili Yönetimi veya ağ bağlantısı için gereken teknik meta verilerin bile ikili exchange devam etmek zordur.  Federasyon hub'ları, bu sorunları çözmek, yalnızca sınırlı başarı kullanıcıların elde ettiği başarılar.

### <a name="what-a-trust-framework-specification-defines"></a>Bir güven Framework belirtimi tanımlar
TFs her topluluk ilgilendiğiniz belirli bir TF belirtiminde burada tabidir açık kimlik değişimi (OIX) güven Framework modelin linchpins ' dir. Böyle bir TF belirtimi tanımlar:

- **Güvenlik ve gizlilik ölçüm tanımını toplulukla ilgi için:**
    - Katılımcılar tarafından sunulan/gerekli güvence düzeyleri (yü); Örneğin, dijital kimlik bilgilerini özgünlüğünü güvenle dereceleri sıralı bir dizi.
    - Katılımcıları tarafından sunulan/gerekli olan koruma düzeyleri (LOP); Örneğin, güvenirlik derecelendirmeleri ilgi topluluğundaki katılımcıları tarafından işlenen dijital kimlik bilgilerin korunması için sıralı bir dizi.

- **Dijital kimlik bilgilerini katılımcıları tarafından sunulan gerekli / açıklamasını**.

- **Üretim ve tüketimini dijital kimlik bilgilerini ve bu nedenle yü ve LOP ölçmek için teknik ilkeleri. Yazılan bu ilkeler genellikle ilkeleri aşağıdaki kategorileri içerir:**
    - İlkeler, örneğin sağlama kimliği: *Ne kadar güçlü bir kişinin kimlik bilgileri dikkatle mi?*
    - Güvenlik ilkeleri, örneğin: *Ne kadar güçlü bilgi bütünlüğü ve gizliliği korunuyor mu?*
    - Gizlilik ilkeleri, örneğin: *Hangi denetimi bir kullanıcının kişisel olarak tanımlanabilir bilgileri (PII) sahip*?
    - Örneğin survivability ilkeleri: *Sağlayıcı işlemleri başlamasıyla süreklilik ve koruma PII işlevinin nasıl mu?*

- **Üretim ve tüketimini dijital kimlik bilgileri için teknik profiller. Bu profiller içerir:**
    - Dijital kimlik bilgileri belirtilen bir yü kullanılabilir olduğu kapsamı arabirimleri.
    - Hat üzerinde birlikte çalışabilirlik teknik gereksinimleri.

- **Katılımcılar Topluluğu'nda gerçekleştirebileceğiniz çeşitli roller ve bu rolleri gerçekleştirmek için gerekli nitelikleri açıklamaları.**

Böylece kimlik bilgilerini bir topluluk ilgi katılımcılar nasıl değiştirilir TF belirtimi yönetir: bağlı olan taraflar, kimlik ve öznitelik sağlayıcıları ve öznitelik doğrulayıcıları.

İdare topluluk onaylama düzenleyen ilgi ve topluluk içinde dijital kimlik bilgilerinin kullanım için bir başvuru olarak hizmet veren bir veya birden çok belge TF özelliğidir. İlke ve yordamlar ilgilendiğiniz bir topluluk üyeleri arasında çevrimiçi işlem için kullanılan dijital kimlikler güven için tasarlanmış, belgelenmiş bir kümesidir.  

Diğer bir deyişle, TF belirtimi bir topluluk için uygun federe kimlik bir ekosistem oluşturmak için kurallar tanımlar.

Şu anda yaygın anlaşmasında avantajı, bu tür bir yaklaşım yoktur. Yok Şüphesiz, güven framework özellikleri arasında birden çok topluluklar ilgi kullanılabilirler, yani doğrulanabilir güvenlik, güvenilirlik ve gizlilik özellikleri ile dijital kimliği eko sistemlerinin geliştirme kolaylaştırmak.

İçin nedeni, kimlik deneyimi çerçevesi kullanan Azure AD B2C özel ilkeleri belirtimi veri gösterimine bir TF için temel olarak birlikte çalışabilirlik kolaylaştırmak için kullanır.  

Kimlik deneyimi çerçevesi yararlanan azure AD B2C özel ilkeler, veri İnsan ve makine tarafından okunabilir bir karışımını olarak TF belirtim temsil eder. Bu model (genellikle daha doğru idare yerleştirilir bölümleri) bazı bölümlerini (varsa) ilgili yordamları birlikte yayımlanan güvenlik ve Gizlilik İlkesi belgeleri başvuru olarak gösterilir. Diğer bölümlerde işlem Otomasyonu kolaylaştırmak yapılandırma meta verileri ve çalışma zamanı kuralları ayrıntılı olarak açıklanmaktadır.

## <a name="understand-trust-framework-policies"></a>Framework güven ilkelerini anlama

Uygulama açısından TF belirtimi bir kimlik davranışları ve deneyimler üzerinde tam denetime izin veren bir ilke kümesi oluşur.  Kimlik deneyimi çerçevesi yararlanan azure AD B2C özel ilkeleri yazar ve kendi TF tanımlama ve yapılandırma gibi bildirim temelli ilkeleri aracılığıyla oluşturmak aşağıdakileri sağlar:

- Belge başvuru veya tanımlamak için TF ilişkili bir topluluk federe kimlik ekosistemi başvuruları. Bunlar, TF belgelere bağlantılar vardır. (Önceden tanımlanmış) işletimsel "çalışma zamanı" kuralları veya kullanıcı yolculuklarından otomatikleştirin ve/veya exchange ve talepleri kullanımını denetler. Bu kullanıcı yolculuklarından bir yü (ve bir LOP) ile ilişkilendirilir. Bir ilke, bu nedenle LOAs (ve LOPs) değişen ile kullanıcı yolculuklarından sahip olabilir.

- Kimlik ve öznitelik sağlayıcıları veya faiz ve bunlara ilişkilendirir (bant-) yü/LOP akreditasyonu birlikte destekledikleri teknik profiller topluluğundaki talep sağlayıcıları.

- Öznitelik doğrulayıcılar veya talep sağlayıcıları ile tümleştirme.

- Bağlı olan taraflar topluluğundaki (tarafından çıkarımı).

- Katılımcılar arasındaki ağ iletişim kurmak için meta veriler. Bu meta veriler teknik profilini birlikte bir işlem sırasında "on"kablo bağlı olan taraf ve diğer topluluk katılımcılar arasındaki birlikte çalışabilirlik düzenlemek için kullanılır.

- Protokol dönüştürme varsa (örneğin, SAML 2.0, OAuth2, WS-Federation ve Openıd Connect).

- Kimlik doğrulama gereksinimleri.

- Çok faktörlü düzenleme varsa.

- Tüm kullanılabilir talepler ve ilgi topluluğunun katılımcılara eşlemeleri için paylaşılan bir şema.

- Tüm talep dönüştürmeleri, exchange ve talepleri kullanımını desteklemek için bu bağlamda olası veri küçültme yanı sıra.

- Bağlama ve şifreleme.

- Talep depolama.

### <a name="understand-claims"></a>Talep anlama

> [!NOTE]
> Topluca "talepler" olarak değiştirilen kimlik bilgileri tüm olası türleri diyoruz: kişisel tanımlama özniteliklerini, talep bir son kullanıcının kimlik doğrulama bilgileri, kimlik güvenlik incelemesi, iletişim cihaz, fiziksel konumu hakkında ve benzeri.  
>
> Çevrimiçi işlemleri bu veri yapıları, doğrudan bağlı olan taraf tarafından doğrulanabilir olgular olmadığı için--"öznitelikler" yerine"--"talepler"terimi kullanırız. Bunun yerine onaylar veya talepleri, bağlı olan taraf kullanıcının istenen işlem vermek için yeterli güvenle geliştirmelisiniz gerçekleri oldukları.  
>
> Kimlik deneyimi çerçevesi kullanan Azure AD B2C özel ilkeleri temel alınan protokoldeki olmasına bakılmaksızın tutarlı bir şekilde her tür dijital kimlik bilgileri değişimi basitleştirmek için tasarlandığından "talepler" terimini kullanacağız Kullanıcı kimlik doğrulaması veya öznitelik alımı için tanımlanır.  "Talep Sağlayıcıları" terimini kullanacağız benzer şekilde, biz belirli işlevleri arasında ayrım yapmak kullanmak istemiyorsanız, kimlik sağlayıcıları, öznitelik sağlayıcıları ve öznitelik doğrulayıcılar topluca başvurmak için.   

Bu nedenle bunlar arasında bir bağlı olan taraf, kimlik ve öznitelik sağlayıcıları ve öznitelik doğrulayıcılar kimlik bilgilerini nasıl değiştirilir yönetir. Bunlar hangi kimlik denetimi ve öznitelik sağlayıcıları bir bağlı olan tarafın kimlik doğrulaması için gereklidir. Bunlar bir etki alanına özgü dil (DSL) diğer bir deyişle, bir devralma, belirli bir uygulama etki alanını için özelleştirilmiş bir bilgisayar dili düşünülmesi gereken *varsa* deyimleri, çok biçimlilik.

Bu ilkeler, makine tarafından okunabilir TF yapısı içinde kimlik deneyimi çerçevesi yararlanarak Azure AD B2C özel ilkeler bölümünü oluşturur. Bunlar, talep sağlayıcısı meta verileri ve teknik profiller, talep şema tanımları, talep dönüştürme işlevleri ve işletimsel düzenleme ve Otomasyon kolaylaştırmak için doldurulan kullanıcı yolculuklarından dahil olmak üzere işlem tüm ayrıntılarını içerir.  

Oldukları varsayılır *living belgeleri* içeriklerini zaman aktif katılımcılara bildirilen ilkeleri ile ilgili üzerinden değiştirmek iyi bir fırsat olduğundan. Hüküm ve koşulları katılımcı teşekkür değişebilir olasılığı yoktur.  

Federasyon Kurulum ve Bakım büyük ölçüde basitleştirilir farklı talep sağlayıcıları/doğrulayıcılar katılın veya bırakın (topluluk tarafından temsil edilen) bağlı olan tarafların güven ve bağlantı yeniden yapılandırmalar devam eden koruma ilkeleri kümesi.

Birlikte çalışabilirlik başka bir önemli bir sorundur. Bağlı olan taraflar tüm gerekli protokolü de desteklemeyi olası olduğundan, ek talep sağlayıcıları/doğrulayıcılar tümleştirilmelidir. Azure AD B2C özel ilkeler, endüstri standardı protokoller destekleyerek ve bağlı olan taraflar ve öznitelik sağlayıcıları aynı protokolü desteklemediğinde istekleri sırasını değiştirmek için belirli bir kullanıcı yolculuklarından uygulayarak bu sorunu çözer.  

Kullanıcı yolculuklarından Protokolü profilleri ve "on"kablo ve diğer katılımcılarla bir bağlı olan taraf arasında birlikte çalışabilirlik düzenlemek için kullanılan meta verileri içerir. Kimlik bilgileri exchange istek/yanıt iletilerini TF belirtiminin bir parçası olarak yayımlanan ilkelerle uyumluluğu zorunlu tutmak için uygulanan işlem çalışma zamanı kuralları vardır. Müşteri Deneyimini özelleştirmesini anahtarına kullanıcı yolculuklarından olur. Ayrıca, protokol düzeyinde sistem birlikte nasıl çalıştığı hakkında açık sheds.

Bu esasına göre bağlı olan taraf uygulamaları ve portalları olabilir bağlamları bağlı olarak kimlik deneyimi Çerçevesi'nin belirli bir ilkenin adını geçirerek yararlanan Azure AD B2C özel ilkeler çağırmak ve tam olarak davranışı ve bilgi exchange alın Tüm muss, fuss veya riski isterler.
