---
title: Başvuru - Azure Active Directory B2C içinde güven çerçeve | Microsoft Docs
description: Azure Active Directory B2C özel ilkeleri ve kimlik deneyimi Framework konusunda.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b75efe7464c32863781353549f73048b4e127ddf
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34710228"
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Azure AD B2C kimlik deneyimi Framework güven çerçeveleri tanımlayın

Azure Active Directory B2C kimlik deneyimi Framework kullanın (Azure AD B2C) özel ilkeler, merkezi bir hizmeti, kuruluşunuz sağlar. Bu hizmet ilgi büyük bir topluluk içinde Kimlik Federasyonu karmaşıklığını azaltır. Karmaşıklık tek güven ilişkisi ve tek meta veri değişimi azalır.

Aşağıdaki soruları yanıtlayın sağlamak için kimlik deneyimi Framework kullanan azure AD B2C özel ilkeler:

- Yasal, güvenlik, gizlilik ve bağlı gereken veri koruma ilkeleri nelerdir?
- Kimin kişiler ve onaylanmış bir katılımcısı olma işlemler nelerdir?
- Bunlar, onaylanmış kimlik bilgileri sağlayıcısı (olarak da bilinen "Talep Sağlayıcıları") olan ve neler sağlar?
- Onaylanmış bağlı olan taraflar kim (ve isteğe bağlı olarak, ne duyarlar)?
- Teknik nelerdir "hattaki" katılımcıları birlikte çalışabilirlik gereksinimlerini?
- Dijital kimlik bilgileri değişimi için zorlanan gerekir işletimsel "çalışma zamanı" kuralları nelerdir?

Tüm soruları yanıtlamak için kimlik deneyimi Framework güven Framework (TF) kullanma Azure AD B2C özel ilkeler oluşturun. Şimdi ne sağlar ve bu yapıyı göz önünde bulundurun.

## <a name="understand-the-trust-framework-and-federation-management-foundation"></a>Güven Framework ve Federasyon management foundation anlama

Güven kimlik, güvenlik, gizlilik ve veri koruma ilkeleri bir topluluk ilgi katılımcıları uygun olmalıdır yazılı belirtimini çerçevedir.

Federe kimlik, son kullanıcı kimliğini güvence Internet ölçeğinde ulaşmak için bir temel sağlar. Üçüncü taraflar için kimlik yönetimi için temsilci seçme ile birden çok bağlı olan taraflar bir son kullanıcı için tek bir dijital kimliği yeniden kullanılabilir.  

Kimlik güvence kimlik sağlayıcısı (IdPs) ve öznitelik sağlayıcıları (AtPs) için belirli güvenlik, gizlilik ve işletimsel ilkelerini ve uygulamalarını uyması gerekir.  Doğrudan incelemeleri gerçekleştirilemiyor, bağlı olan taraflar ile çalışmayı tercih AtPs ve IdPs güven ilişkileri (RPs) geliştirmelisiniz.  

Tüketicileri ve sağlayıcıları dijital kimlik bilgilerinin sayısı arttıkça, bu güven ilişkilerinin ikili yönetim veya ağ bağlantısı için gerekli olan teknik meta verilerin bile ikili exchange devam etmek zordur.  Federasyon hub'ları bu sorunları çözmek sırasında yalnızca sınırlı başarı elde.

### <a name="what-a-trust-framework-specification-defines"></a>Bir güven Framework belirtimi tanımlar
TFs ilgi her topluluk tarafından belirli bir TF belirtimi burada tabidir açık kimlik Exchange (OIX) güven Framework modeli linchpins ' dir. Bu tür bir TF belirtimi tanımlar:

- **Güvenlik ve gizlilik ölçümleri ilgi topluluk tanımıyla birlikte için:**
    - Katılımcıları tarafından sunulan/gerekli olan güvence düzeyleri (LOA); Örneğin, dijital kimlik bilgileri özgünlüğünü güvenirlik derecelendirmesi sıralı bir dizi.
    - Katılımcıları tarafından sunulan/gerekli olan koruma düzeyleri (LOP); Örneğin, ilgi topluluk katılımcıları tarafından işlenen dijital kimlik bilgileri koruması için güvenirlik derecelendirmelerini sıralı bir dizi.

- **Gerekli / sunulan dijital kimlik bilgilerinin katılımcılar tarafından açıklaması**.

- **Teknik ilkeleri üretim ve dijital kimlik bilgilerinin kullanım için ve bu nedenle LOA ve LOP ölçme. Yazılı Bu ilkeler genellikle ilkeleri aşağıdaki kategorileri şunlardır:**
    - İlkeleri, örneğin sağlama kimlik: *vetted kişinin kimlik bilgilerini'ne kadar güçlü olan?*
    - Güvenlik ilkeleri, örneğin: *ne kadar güçlü bilgi bütünlüğü ve gizliliği korumalı misiniz?*
    - Gizlilik ilkeleri, örneğin: *hangi denetim bir kullanıcı kişisel olarak tanımlanabilir bilgileri (PII) sahip*?
    - Survivability ilkeleri, örneğin: *bir sağlayıcı işlemleri başlamasıyla nasıl mu devamlılığı ve koruma PII işlevinin?*

- **Üretim ve dijital kimlik bilgilerinin kullanım için teknik profilleri. Bu profiller içerir:**
    - Kapsam arabirimler, dijital kimlik bilgileri belirtilen LOA kullanılabilir.
    - Teknik gereksinimleri üzerindeki hat birlikte çalışabilirlik.

- **Topluluk katılımcıları gerçekleştirebileceğiniz çeşitli rolleri ve bu rolleri gerçekleştirmek için gerekli niteliklere açıklamaları.**

Böylece kimlik bilgileri ilgi topluluğu katılımcılar nasıl değiştirilir TF belirtimi yönetir: bağlı olan taraflar, kimlik ve öznitelik sağlayıcıları ve öznitelik doğrulayıcıları.

İdare onaylama düzenler ilgi topluluğu ve topluluk içinde dijital kimlik bilgilerinin tüketim için bir başvuru olarak hizmet bir veya birden çok belge TF belirtimidir. İlkeleri ve ilgi bir topluluk üyeleri arasındaki çevrimiçi işlemleri için kullanılan dijital kimlikleri güven için tasarlanmış yordamları belgelenmiş bir kümesidir.  

Diğer bir deyişle, TF belirtimi bir topluluk için uygun Federal Kimlik ekosistemi oluşturmak için kurallar tanımlar.

Şu anda bu tür bir yaklaşım avantajı üzerinde yaygın anlaşmayı yoktur. Yoktur Şüphesiz, framework belirtimleri kolaylaştırmak dijital kimliği ekosistemlerini ilgi birden çok topluluğu arasında kullanılabilme, yani doğrulanabilen güvenlik güvencesi ve gizlilik özelliklere sahip geliştirme güven.

İçin nedeni, kimlik deneyimi Framework kullanan Azure AD B2C özel ilkeler belirtimi kendi veri temsili bir TF için temel olarak birlikte çalışabilirlik kolaylaştırmak için kullanılır.  

Kimlik deneyimi Framework yararlanan azure AD B2C özel ilkeler TF belirtimi İnsan ve makine tarafından okunabilir veri bileşimi olarak temsil eder. Bu modelin (genellikle doğru idare daha odaklı bölümleri) bazı bölümleri (varsa), ilgili yordamlar birlikte yayımlanan güvenlik ve Gizlilik İlkesi belgeleri başvuruları gösterilir. Diğer bölümleri işletimsel Otomasyon kolaylaştırmak yapılandırma meta verilerini ve çalışma zamanı kuralları ayrıntılı olarak açıklanmaktadır.

## <a name="understand-trust-framework-policies"></a>Güven Framework ilkelerini anlama

Uygulama bakımından kimlik davranışları ve deneyimleri üzerinde tam denetim ilkeleri kümesini TF belirtimi oluşur.  Kimlik deneyimi Framework yararlanan azure AD B2C özel ilkeler, yazar ve kendi TF tanımlayın ve yapılandırma gibi bildirim temelli ilkeleri aracılığıyla oluşturmak etkinleştir:

- Belge başvurusu veya TF ilişkili topluluk Federal Kimlik ekosistemi tanımlamak başvuruları. Bunlar TF belgelere bağlantılar vardır. (Önceden tanımlanmış) işletimsel "çalışma zamanı" kurallar veya otomatikleştirmek ve/veya exchange ve talep kullanımını kontrol kullanıcı Yolculuklar. Bu kullanıcı Yolculuklar bir LOA (ve bir LOP) ile ilişkilendirilir. Bir ilke, bu nedenle LOAs (ve LOPs) değişen ile kullanıcı Yolculuklar sahip olabilir.

- Kimlik ve öznitelik sağlayıcılar veya talep sağlayıcıları ilgi ve bunlara ilişkilendirir (bant-) LOA/LOP eşitlik belgesi birlikte destekledikleri teknik profilleri topluluğu.

- Öznitelik doğrulayıcılar veya talep sağlayıcıları ile tümleştirme.

- Bağlı olan taraflar topluluğundaki (tarafından çıkarım).

- Katılımcılar arasındaki ağ iletişim kurmak için meta veriler. Teknik profilleri yanı sıra bu meta veriler bir işlem sırasında "açık"kablo bağlı olan taraf ve diğer topluluk katılımcılar arasındaki birlikte çalışabilirlik düzenlemek için kullanılır.

- Protokol dönüştürme varsa (örneğin, SAML, OAuth2, WS-Federation ve Openıd Connect).

- Kimlik doğrulama gereksinimleri.

- Çok faktörlü orchestration varsa.

- Tüm kullanılabilir talepler ve ilgi topluluğunun katılımcılara eşlemeleri için paylaşılan bir şema.

- Tüm talep dönüştürmeleri, olası veri minimization exchange ve talep kullanımı sürdürebilmek için bu bağlamda birlikte.

- Bağlama ve şifreleme.

- Talep depolama.

### <a name="understand-claims"></a>Talep anlama

> [!NOTE]
> Biz topluca "talep" olarak değiştirilen kimlik bilgileri tüm olası türleri bakın: bir son kullanıcının kimlik doğrulaması hakkındaki talepler, kimlik, öznitelikler, kişisel olarak tanımlayan iletişim aygıtı, fiziksel konumu vetting kimlik bilgileri ve benzeri.  
>
> Çevrimiçi işlemlerde bu veri yapıları doğrudan bağlı olan taraf tarafından doğrulanabilen bulguları olduğundan--"öznitelikleri yerine"--"talep" terimi kullanırız. Bunun yerine onaylar veya bağlı olan taraf kullanıcının istenen işlem vermek için yeterli güvenirlik geliştirmelidir bulguları hakkında talepleri oldukları.  
>
> Kimlik deneyimi Framework kullanan Azure AD B2C özel ilkeler temel Protokolü Kullanıcı kimlik doğrulaması veya öznitelik alımı için tanımlı bağımsız olarak tutarlı bir şekilde her tür dijital kimlik bilgileri değişimi basitleştirmek için tasarlandığından ayrıca "talep" terimi kullanırız.  "Talep Sağlayıcıları" terimi kullanırız benzer şekilde, biz belirli işlevleri arasında ayrım yapmak kullanmak istemiyorsanız, kimlik sağlayıcısı, öznitelik sağlayıcıları ve öznitelik doğrulayıcılar topluca başvurmak için.   

Bu nedenle bunlar kimlik bilgilerini bir bağlı olan taraf, kimlik ve öznitelik sağlayıcıları ve öznitelik doğrulayıcılar arasında nasıl alınıp yönetir. Hangi kimlik denetlemek ve özniteliği sağlayıcıları için bir bağlı olan tarafın kimlik doğrulaması gerekir. Bunlar bir etki alanına özgü dil (DSL) başka bir deyişle, belirli uygulama etki alanı devralmayla, için özelleştirilmiş bir bilgisayar dil düşünülmesi gereken *varsa* deyimleri, çok biçimlilik.

Bu ilkeler kimlik deneyimi Framework yararlanarak Azure AD B2C özel ilkelerinde TF yapı makine tarafından okunabilir bölümünü oluşturur. Talep sağlayıcısı meta verileri ve teknik profilleri, talep şema tanımları, talep dönüştürme işlevleri ve işletimsel orchestration ve Otomasyon kolaylaştırmak için doldurulur kullanıcı Yolculuklar dahil olmak üzere tüm işlem ayrıntılarını, içerirler.  

Olarak kabul *yaşam belgeleri* içeriklerini aktif katılımcılara bildirilen ilkelerinde zaman ilgili üzerinden değiştirecek şansı olduğundan. Hüküm ve koşulları Katılımcısı olması için değişebilir olası yoktur.  

Federasyon Kurulum ve Bakım çok Basitleştirilmiş farklı talep sağlayıcıları/doğrulayıcılar katılma veya (topluluk tarafından temsil edilen) ayrılma gibi devam eden güven ve bağlantı yeniden yapılandırmaların bağlı olan tarafların koruma ilkeleri kümesi.

Birlikte çalışabilirlik başka bir önemli bir iştir. Bağlı olan taraflar tüm gerekli protokolleri desteklemek olası olduğundan, ek talep sağlayıcıları/doğrulayıcılar tümleşik gerekir. Azure AD B2C özel ilkeler, endüstri standardı protokolleri destekleyen ve bağlı olan taraflar ve öznitelik sağlayıcıları aynı protokol desteklemediğinde istekleri sırasını değiştirmek için belirli bir kullanıcı Yolculuklar uygulama tarafından bu sorunu çözün.  

Kullanıcı Yolculuklar Protokolü profilleri ve "" hat üzerinde bağlı olan taraf ve diğer katılımcılar arasındaki birlikte çalışabilirlik düzenlemek için kullanılan meta verileri içerir. Kimlik bilgileri exchange istek/yanıt iletilerini TF belirtiminin bir parçası olarak yayımlanan ilkeleriyle uyumluluğunu zorlama uygulanır işletimsel çalışma zamanı kurallarını da vardır. Müşteri Deneyimini Özelleştirme anahtarına kullanıcı Yolculuklar olur. Ayrıca, sistem protokol düzeyinde nasıl çalıştığı ışık sheds.

Bu temelinde bağlı olan taraf uygulamaları ve portalları bulunabilir bağlamları bağlı olarak belirli bir ilke adını geçirerek kimlik deneyimi Framework yararlanan Azure AD B2C özel ilkeler çağırma ve tüm muss, fuss veya riski istedikleri tam olarak davranışı ve bilgileri exchange alabilirsiniz.
