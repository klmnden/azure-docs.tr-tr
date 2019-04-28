---
title: Çıkış noktaları arası kaynak paylaşımı (CORS) desteği | Microsoft Docs
description: Microsoft Azure depolama hizmetleri için CORS desteğini etkinleştirme hakkında bilgi edinin.
services: storage
author: cbrooksmsft
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: cbrooks
ms.subservice: common
ms.openlocfilehash: 5e65965678ed042081e4a406d3a207fb7ede299f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483494"
---
# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Çıkış noktaları arası kaynak paylaşımı (CORS) desteğiyle Azure depolama hizmetleri
2013-08-15 sürümünden başlayarak, Azure depolama hizmeti Blob, tablo, kuyruk ve Dosya Hizmetleri için çıkış noktaları arası kaynak paylaşımı (CORS) destekler. CORS, başka bir etki alanındaki kaynaklara erişmek bir etki alanı altında çalışan bir web uygulamasını etkinleştiren bir HTTP özelliğidir. Web tarayıcısı olarak bilinen bir güvenlik kısıtlaması uygulamak [aynı çıkış noktası İlkesi](https://www.w3.org/Security/wiki/Same_Origin_Policy) engelleyen bir web sayfasından; farklı bir etki alanında arama API'leri CORS, başka bir etki alanındaki API'leri çağırmak bir etki alanı (kaynak etki alanı) izin vermek için güvenli bir yol sağlar. Bkz: [CORS belirtimi](https://www.w3.org/TR/cors/) CORS hakkında ayrıntılı bilgi için.

CORS kurallarını ayrı ayrı Depolama hizmetlerinin her biri için çağırarak ayarlayabileceğiniz [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), ve [tablo hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452240.aspx). Hizmet için CORS kurallarını ayarladığınızda, hizmete yönelik farklı bir etki alanından yapılan düzgün şekilde yetkili bir istek, belirttiğiniz kurallara göre izin verilip verilmeyeceğini belirlemek için değerlendirilir.

> [!NOTE]
> CORS bir kimlik doğrulama mekanizması olmadığını unutmayın. CORS etkinleştirildiğinde bir depolama kaynağına karşı yapılan herhangi bir istek uygun kimlik doğrulamasını imza ya da sahip olmanız gerekir veya ortak bir kaynakta yapılan.
> 
> 

## <a name="understanding-cors-requests"></a>CORS istekleri anlama
Kaynak etki alanındaki bir CORS isteği, iki ayrı isteklerinin oluşabilir:

* Hizmet tarafından uygulanan CORS kısıtlamalar sorgular bir denetim öncesi isteği. İstek yöntemi olduğu sürece denetim öncesi isteği gereklidir bir [basit yöntemi](https://www.w3.org/TR/cors/), GET, HEAD veya POST anlamına gelir.
* İstenen kaynağa karşı yapılan asıl isteğinin.

### <a name="preflight-request"></a>Denetim öncesi isteği
Denetim öncesi isteği sorgular için depolama hizmeti hesap sahibi tarafından kurulmuş CORS kısıtlamaları. Web tarayıcı (veya diğer kullanıcı aracısı), yöntem ve kaynak etki alanı, istek üst bilgilerini içeren bir OPTIONS isteğini gönderir. Depolama hizmeti, hangi kaynak etki alanlarının, istek yöntemleri ve istek üstbilgileri, bir depolama kaynağına karşı gerçek bir istekte belirtilebilir belirten CORS kuralları önceden yapılandırılmış bir dizi temel amaçlanan işlemi olarak değerlendirilir.

Hizmet için CORS etkin ve denetim öncesi isteği eşleşen bir CORS kuralı varsa, hizmet 200 (Tamam) durum koduyla yanıt verir ve yanıtta gerekli Access-Control üst bilgilerini içerir.

Hizmet, hizmet için CORS etkin değil ya da Denetim öncesi isteği eşleşen CORS kural 403 (Yasak) durum koduyla yanıt verir.

Hizmet, OPTIONS isteğini (kaynak ve Access-Control-Request-Method üst bilgiler) gerekli CORS üstbilgilerini içermiyorsa, 400 (Hatalı istek) durum kodu ile yanıt verecektir.

İstenen kaynak adlarla değil ve hizmetini (Blob, kuyruk ve tablo) bir denetim öncesi isteği değerlendirilir unutmayın. Hesap sahibi CORS isteği başarılı olması için sırayla hesap hizmet özelliklerini bir parçası olarak etkinleştirmiş olmanız gerekir.

### <a name="actual-request"></a>Gerçek isteği
Denetim öncesi isteği kabul edildikten sonra bir yanıt döndürdü, tarayıcı gerçek depolama kaynağı isteği gönderir. Tarayıcı fiili Reddet denetim öncesi isteği reddedilirse hemen isteyin.

Asıl isteğinin normal depolama hizmeti isteği olarak kabul edilir. CORS istek isteğidir ve hizmet eşleşen CORS kurallarını denetler kaynak üst bilgisi varlığını gösterir. Bir eşleşme bulunursa, Access-Control üst bilgilerinin yanıta eklenir ve istemciye geri gönderilir. Bir eşleşme bulunmazsa, CORS Access-Control üst bilgilerinin döndürülmez.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Azure depolama hizmetleri için CORS'yi etkinleştirme
CORS kurallarını etkinleştirin veya CORS her hizmet için (Blob, kuyruk ve tablo) devre dışı. böylece, hizmet düzeyinde ayarlanmış ayrı olarak. Varsayılan olarak, her hizmet için CORS devre dışıdır. CORS etkinleştirmek için sürüm 2013-08-15 kullanarak uygun hizmet özelliklerini ayarlamanız gerekir veya sonraki sürümleri ve hizmet özellikleri için CORS kuralları ekleyin. Etkinleştirmek veya devre dışı bırakma hakkında ayrıntılı bilgi için CORS hizmet ve CORS kurallarını ayarlama için lütfen bkz [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), ve [tablo hizmeti Özellikleri](https://msdn.microsoft.com/library/hh452240.aspx).

Belirtilen bir hizmeti özelliklerini ayarla işlemine tek bir CORS kuralı örneği aşağıda verilmiştir:

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

CORS kuralı bulunan her öğe, aşağıda açıklanmıştır:

* **AllowedOrigins**: CORS aracılığıyla Depolama hizmetinden bir istekte bulunmak için izin verilen kaynak etki alanları. Kaynak etki alanı, İsteğin kaynaklandığı etki alanıdır. Kaynak kullanıcı yaş hizmetine gönderir kaynağına sahip büyük küçük harfe duyarlı bir tam eşleşme olması gerektiğini unutmayın. Joker karakter kullanabilirsiniz ' *' CORS aracılığıyla isteğinde bulunmak tüm kaynak etki alanlarının izin vermek için. Etki alanları http yukarıdaki örnekte:\//www.contoso.com ve http: \/ /www.fabrikam.com istekleri CORS kullanarak hizmete göre yapabilirsiniz.
* **AllowedMethods**: Kaynak etki alanının CORS istekleri için kullanabileceği yöntemleri (HTTP istek fiilleri). Yukarıdaki örnekte, yalnızca PUT ve GET isteklerine izin verilir.
* **AllowedHeaders**: Kaynak etki alanının CORS isteğinde belirtebilir istek üstbilgileri. Yukarıdaki örnekte, x-ms-meta verileri ile x-ms-meta-hedef ve x-ms-meta-abc başlayan tüm meta veri üst bilgileri izin verilir. Unutmayın joker karakter ' *' Belirtilen önek herhangi üstbilgi başlayarak izin verildiğini gösterir.
* **ExposedHeaders**: CORS isteğine yanıt olarak gönderilen ve isteği gönderene tarayıcı tarafından gösterilen yanıt üstbilgileri. Yukarıdaki örnekte, herhangi bir üst bilgisi x-ms-meta başlayarak kullanıma sunmak için tarayıcı talimat verilmiştir.
* **Maxageınseconds**: Bir tarayıcının denetim öncesi seçenekler isteğini önbelleğe alması gereken en uzun süre.

Azure depolama hizmetleri belirten ön ekli üst bilgiler her ikisi için destek **AllowedHeaders** ve **ExposedHeaders** öğeleri. Üst kategori izin vermek için bu kategoriye ortak bir öneki belirtebilirsiniz. Örneğin, belirten *x-ms-meta** gibi bir ön ekli üst bilgisi x-ms-meta'ile başlayan tüm başlıkları eşleşen bir kural oluşturur.

CORS kuralları için aşağıdaki sınırlamalar geçerlidir:

* Depolama Hizmeti (Blob, tablo ve Kuyruk) başına en fazla beş CORS kuralı belirtebilirsiniz.
* XML etiketleri hariç tüm CORS kuralları ayarları isteğinde en büyük boyutunu 2 KB aşmamalıdır.
* Bir izin verilen üst bilgi, kullanıma sunulan üst bilgi veya izin verilen çıkış noktaları uzunluğu 256 karakterden fazla olmamalıdır.
* İzin verilen üst bilgiler ve kullanıma sunulan üst bilgiler olabilir:
  * Burada kesin bir üst bilgi adı sağlanmışsa gibi sabit değerli üst bilgi **x-ms-meta-işlenen**. İstekte en fazla 64 sabit değerli üst bilgi belirtilebilir.
  * Üst bilgiler, burada bir ön ek üst bilgisi sağlanır, gibi önek ** x-ms-meta-veri ***. Bu şekilde bir önek belirleyerek izin verir veya belirli bir önek ile başlayan herhangi bir üst bilgi sunar. İstekte en fazla iki ön ekli üst bilgi belirtilebilir.
* Belirtilen yöntemler (veya HTTP fiilleri) **AllowedMethods** öğesi API'leri Azure depolama hizmeti tarafından desteklenen yöntemleri için uygun olması gerekir. Desteklenen yöntemler, DELETE, GET, HEAD, birleştirme, POST, SEÇENEKLERİNİ ve PUT ' tur.

## <a name="understanding-cors-rule-evaluation-logic"></a>CORS kuralı değerlendirme mantığı anlama
Bir depolama hizmetine bir denetim öncesi veya gerçek bir istek aldığında, bu isteği uygun hizmeti özelliklerini ayarla işlemine aracılığıyla hizmeti için oluşturulan CORS kurallarına göre değerlendirir. CORS kurallarını hizmeti özelliklerini ayarla işlemine istek gövdesinde ayarlanan sırayla değerlendirilir.

CORS kuralları gibi değerlendirilir:

1. İlk olarak isteğin kaynak etki alanı için listelenen etki alanları karşılaştırılarak **AllowedOrigins** öğesi. Kaynak etki alanı listesinde yer ya da tüm etki alanları, joker karakter ile izin verilir ' *', ardından değerlendirme kazançlar kuralları. Kaynak etki alanı dahil edilmezse, ardından istek başarısız olur.
2. Ardından, isteğin yöntemi (veya HTTP fiili) içinde listelenen yöntemlerden istediğinizi karşılaştırılarak **AllowedMethods** öğesi. Ardından yöntemi listede yer alıyorsa kuralları değerlendirme devam eder; Aksi durumda, ardından istek başarısız olur.
3. İstek, kaynak etki alanı ve onun yöntemi bir kuralla eşleşirse, bu kuralda veya ek işlem istek ve başka hiçbir kural değerlendirilir seçilir. İstek başarılı olabilmesi için önce ancak istekte belirtilen üst bilgileri listelenen üstbilgileri karşı denetlenir **AllowedHeaders** öğesi. Gönderilen üst bilgiler, izin verilen üstbilgileri eşleşmiyorsa, istek başarısız olur.

Kuralları, istek gövdesinde yok sırayla işlenir ve bu yana bunlar ilk olarak değerlendirilir, böylece en iyi kaynakları ile ilgili en kısıtlayıcı kuralları önce listede belirttiğiniz önerilir. Daha az kısıtlayıcı – – listesinin sonunda tüm kaynaklara izin veren bir kural gibi kuralları belirtin.

### <a name="example--cors-rules-evaluation"></a>Örneğin, değerlendirme CORS kuralları
Aşağıdaki örnek, depolama hizmeti için CORS kuralları ayarlamak bir işlem için kısmi istek gövdesi gösterir. Bkz: [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), ve [tablo hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452240.aspx) isteği oluşturulurken ilişkin ayrıntılar için.

```xml
<Cors>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>PUT,HEAD</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>*</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
    </CorsRule>
</Cors>
```

Ardından, aşağıdaki CORS isteklerini göz önünde bulundurun:

| İstek |  |  | Yanıt |  |
| --- | --- | --- | --- | --- |
| **Yöntem** |**Kaynak** |**İstek Üst Bilgileri** |**Kuralın eşleşmesi** |**Sonuç** |
| **PUT** |http:\//www.contoso.com |x-ms-blob-content-type |İlk kuralı |Başarılı |
| **GET** |http:\//www.contoso.com |x-ms-blob-content-type |İkinci kuralı |Başarılı |
| **GET** |http:\//www.contoso.com |x-ms-istemci-isteği-ID |İkinci kuralı |Hata |

İlk istek eşleşen ilk kural – kaynak etki alanı izin verilen çıkış noktaları eşleşir, izin verilen yöntemleri yöntemi eşleştirir ve üst bilgi izin verilen üstbilgileri – eşleşir ve bu nedenle başarılı.

Yöntemi, izin verilen yöntemleri eşleşmediği için ikinci isteği ilk kural eşleşmiyor. Ancak, ikinci kuralı başarılı şekilde eşleştirin.

Üçüncü isteği yöntemi ve kaynak etki alanı içindeki ikinci kuralı eşleşir ve bu sayede başka hiçbir kural değerlendirilir. Ancak, *x-ms-client-isteği-id üst bilgisi* başarılı olması için izin üçüncü kural semantiği gerçeği rağmen istek, başarısız olur ikinci kural tarafından izin verilmiyor.

> [!NOTE]
> Bu örnekte, önce daha kısıtlayıcı bir tane daha az kısıtlayıcı bir kural gösterir, ancak en kısıtlayıcı kuralları ilk listelemek için genel en iyi yöntem olacaktır.
> 
> 

## <a name="understanding-how-the-vary-header-is-set"></a>Vary üstbilgisi nasıl belirlendiğini anlama
*Ayırmayı* standart bir HTTP/1.1 üst bilgi, tarayıcı veya kullanıcı aracısı isteği işlemek için sunucu tarafından seçilen ölçütleri hakkında öneri isteği üst bilgi alanları kümesinden oluşan başlığıdır. *Ayırmayı* üst bilgi, proxy'leri, tarayıcılar ve yanıt nasıl önbelleğe alınması gerektiğini belirlemek için kullanabileceğiniz CDN tarafından önbelleğe almak için temel olarak kullanılır. Belirtimi için Ayrıntılar için bkz [Vary üstbilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Kaynak etki alanı, tarayıcı veya başka bir kullanıcı aracısı CORS istek yanıtı önbelleğe alır, izin verilen kaynağı önbelleğe alınır. Önbelleği etkin durumdayken ikinci bir etki alanı için depolama kaynağı aynı isteği gönderdiğinde, önbelleğe alınmış kaynak etki alanı kullanıcı aracısını alır. Aksi takdirde başarılı olduğunda, isteği başarısız olur, ikinci etki alanına önbelleğe alınan etki alanı eşleşmiyor. Bazı durumlarda, Azure depolama ayırmayı üstbilgisini ayarlar **kaynak** isteyen etki alanı ve önbelleğe alınan kaynaktan farklı olduğu durumlarda hizmete sonraki CORS isteği göndermek için kullanıcı aracısı bildirin.

Azure depolama kümeleri *ayırmayı* başlığına **kaynak** aşağıdaki durumlarda gerçek GET/HEAD istekleri için:

* İstek kaynağı ne zaman bir CORS kuralı tarafından tanımlanan izin verilen kaynağı tam olarak eşleşir. Tam olarak eşleşmelidir için CORS kuralı bir joker karakter içeremez ' * ' karakteri.
* İstek kaynağı eşleşen kural yoktur, ancak CORS depolama hizmeti için etkinleştirildi.

GET/HEAD isteği tüm kaynaklara izin veren bir CORS kuralı eşleştiği durumda, tüm çıkış noktaları ve önbellek etkin durumdayken sonraki istekleri kaynak etki alanı kullanıcı aracısı önbellek sağlayacak yanıt gösterir.

GET/HEAD dışındaki yöntemlerle istekleri unutmayın, bu yöntemlere yapılan yanıtları kullanıcı aracıları tarafından önbelleğe alınmaz olduğundan depolama hizmetleri Vary üstbilgisi ayarlamaz.

Aşağıdaki tabloda Azure depolama gösterir. daha önce bahsedilen örneklerine dayalı GET/HEAD isteklerini yanıtlar:

| İstek | Hesap ayarını ve kuralı değerlendirme sonucu |  |  | Yanıt |  |  |
| --- | --- | --- | --- | --- | --- | --- |
| **İstekte mevcut kaynak üst bilgisi** |**Bu hizmet için belirtilen CORS kuralları** |**Eşleşen kural mevcut olan tüm origins(*) sağlar** |**Eşleşen kural için tam kaynak eşleşme var.** |**Ayarlamak için kaynak ayırmayı üst bilgi yanıtı içerir** |**Access-Control-izin verilen-Origin yanıtı içerir: "*"** |**Access-Control-kullanıma sunulan-Headers yanıtı içerir** |
| Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |
| Hayır |Evet |Hayır |Hayır |Evet |Hayır |Hayır |
| Hayır |Evet |Evet |Hayır |Hayır |Evet |Evet |
| Evet |Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |
| Evet |Evet |Hayır |Evet |Evet |Hayır |Evet |
| Evet |Evet |Hayır |Hayır |Evet |Hayır |Hayır |
| Evet |Evet |Evet |Hayır |Hayır |Evet |Evet |

## <a name="billing-for-cors-requests"></a>CORS istekleri için faturalandırma
Başarılı ön istekleri hesabınız için depolama hizmetlerinden herhangi biri için CORS etkinleştirdiyseniz faturalandırılır (çağırarak [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), veya [Tablo hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452240.aspx)). Ücretleri en aza indirmek için ayarı göz önünde bulundurun. **Maxageınseconds** sizin CORS'niz öğesinde, böylece kullanıcı aracısı isteğini önbelleğe aldığı büyük bir değere kuralları.

Denetim öncesi başarısız istekleri faturalandırılmazsınız.

## <a name="next-steps"></a>Sonraki adımlar
[BLOB hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx)

[Kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx)

[Tablo hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C çıkış noktaları arası kaynak paylaşımı belirtimi](https://www.w3.org/TR/cors/)

