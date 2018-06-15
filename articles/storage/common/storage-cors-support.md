---
title: Çıkış noktaları arası kaynak paylaşımını (CORS) desteği | Microsoft Docs
description: Microsoft Azure Storage Hizmetleri için CORS desteğini etkinleştirme hakkında bilgi edinin.
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: carmonm
editor: tysonn
ms.assetid: a0229595-5b64-4898-b8d6-fa2625ea6887
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: cbrooks
ms.openlocfilehash: 8d189d3ec3e6081dd37b912824f287cd75f39b35
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "23873978"
---
# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Çıkış noktaları arası kaynak paylaşımı (CORS) Azure Storage Hizmetleri desteği
Sürüm 2013-08-15'den başlayarak, Azure storage Hizmetleri için Blob, tablo, kuyruk ve Dosya Hizmetleri çıkış noktaları arası kaynak paylaşımı (CORS) destekler. CORS başka bir etki alanındaki kaynaklara erişmek bir etki alanında çalışan bir web uygulaması sağlayan bir HTTP özelliğidir. Web tarayıcıları uygulamak olarak bilinen bir güvenlik kısıtlama [kaynak aynı ilke](http://www.w3.org/Security/wiki/Same_Origin_Policy) arama API'leri; farklı bir etki alanındaki bir web sayfasından engelleyen CORS başka bir etki alanındaki API'leri çağırmak bir etki alanı (kaynak etki alanı) izin vermek için güvenli bir yol sağlar. Bkz: [CORS belirtimi](http://www.w3.org/TR/cors/) CORS hakkında ayrıntılı bilgi için.

CORS kuralları tek tek her depolama hizmetleri için çağırarak ayarlayabileceğiniz [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), ve [tablo hizmeti özellikleriniAyarla](https://msdn.microsoft.com/library/hh452240.aspx). Hizmet için CORS kuralları ayarladıktan sonra farklı bir etki alanından karşı hizmet yapılan düzgün bir şekilde kimliği doğrulanmış bir isteği, belirlediğiniz kurallara göre izin verilip verilmediğini belirlemek için değerlendirilir.

> [!NOTE]
> CORS bir kimlik doğrulama mekanizması olmadığını unutmayın. CORS etkinleştirildiğinde bir depolama kaynağı karşı yapılan herhangi bir istek doğru kimlik imza ya da sahip olmalı veya genel kaynağı karşı yapılması gerekir.
> 
> 

## <a name="understanding-cors-requests"></a>CORS isteklerini anlama
Bir kaynak etki alanı CORS isteğinden iki ayrı isteklerinin oluşabilir:

* Hizmet tarafından uygulanan CORS kısıtlamalar sorgular bir denetim öncesi isteği. İstek yöntemini olmadığı sürece denetim öncesi isteği gereklidir bir [basit yöntem](http://www.w3.org/TR/cors/), GET, HEAD veya POST anlamına gelir.
* İstenen kaynak karşı yapılan gerçek isteği.

### <a name="preflight-request"></a>Denetim öncesi isteği
Denetim öncesi isteği sorgular için depolama birimi hizmeti hesabı sahibi tarafından kurulmuş CORS kısıtlamaları. Web tarayıcısı (veya diğer kullanıcı aracısı) istek üstbilgileri, yöntemi ve kaynak etki alanı içeren bir seçenekleri isteği gönderir. Depolama hizmetinin depolama kaynağı karşı gerçek bir istek hangi kaynak etki alanları, istek yöntemleri ve istek üstbilgileri belirtilebilir belirten CORS kuralları önceden yapılandırılmış bir dizi göre amaçlanan işlemi değerlendirir.

CORS hizmeti için etkinleştirilip etkinleştirilmeyeceğini ve denetim öncesi isteği eşleşen bir CORS kuralı ise hizmet (Tamam) 200 durum koduyla yanıt verir ve gerekli erişim denetimi üstbilgileri yanıtta içerir.

CORS hizmeti için etkin değil veya denetim öncesi isteği hiçbir CORS kuralıyla eşleşen, hizmet durum kodu 403 (Yasak) ile yanıt verir.

OPTIONS isteği gerekli CORS üstbilgilerini (kaynak ve Access-Control-Request-Method üstbilgiler) içermiyorsa, hizmet durum kodu 400 (Hatalı istek) ile yanıt verir.

İstenen kaynak adlarla değil ve hizmet (Blob, kuyruk ve tablo) bir denetim öncesi isteği değerlendirilir unutmayın. Hesap sahibinin isteğin başarılı olması sırayla hesap hizmeti özelliklerinin bir parçası olarak CORS etkinleştirmiş olmanız gerekir.

### <a name="actual-request"></a>Gerçek isteği
Denetim öncesi isteği kabul edildikten sonra bir yanıt döndürdü, tarayıcı depolama kaynağı karşı gerçek isteği gönderme. Tarayıcı fiili Reddet hemen denetim öncesi isteği reddedilirse isteyin.

Gerçek istek depolama hizmeti karşı normal istek olarak kabul edilir. Kaynak üstbilgisi varlığını CORS istek isteğidir ve hizmet eşleşen CORS kuralları kontrol eder gösterir. Bir eşleşme bulunamazsa, erişim denetimi üstbilgilerini yanıta eklenir ve istemciye gönderilen. Bir eşleşme bulunmazsa, CORS Access-Control üstbilgileri döndürülmez.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Azure Storage Hizmetleri için CORS etkinleştirme
CORS kuralları, hizmet düzeyinde ayarlanır, böylece etkinleştirmek veya CORS her hizmet için (Blob, kuyruk ve tablo) devre dışı gerek ayrı olarak. Varsayılan olarak, her hizmet için CORS devre dışıdır. CORS etkinleştirmek için sürüm 2013-08-15 kullanarak uygun hizmet özelliklerini ayarlamanız gerekir veya sonraki sürümleri ve hizmet özellikleri CORS kuralları ekleyin. Etkinleştirmek veya devre dışı bırakma hakkında ayrıntılı bilgi için CORS bir hizmet ve nasıl CORS kuralları ayarlamak için lütfen bkz [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), ve [tablo hizmeti ayarlayın Özellikler](https://msdn.microsoft.com/library/hh452240.aspx).

Hizmet özelliklerini ayarlama işlemi belirtilen tek bir CORS kural örneği şöyledir:

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

CORS kurala dahil her öğe aşağıda açıklanmıştır:

* **AllowedOrigins**: depolama hizmeti CORS ile bir isteği yapmak için izin verilen çıkış noktası etki alanlarını. Kaynak etki alanı İsteğin kaynaklandığı etki alanıdır. Kaynak tam büyük/küçük harfe eşleşme kullanıcı yaş hizmetine gönderir kaynağına sahip olması gerektiğini unutmayın. Joker karakter kullanabilirsiniz ' *' CORS keşfi yapmak, tüm kaynak etki alanlarının izin vermek için. Etki alanları yukarıdaki örnekte [ http://www.contoso.com ](http://www.contoso.com) ve [ http://www.fabrikam.com ](http://www.fabrikam.com) CORS kullanarak hizmet isteklerinde yapabilirsiniz.
* **AllowedMethods**: kaynak etki alanı için CORS istek kullanabilir yöntemleri (HTTP isteği fiiller). Yukarıdaki örnekte, yalnızca PUT ve GET isteklerini izin verilir.
* **AllowedHeaders**: kaynak etki alanı CORS isteğini belirtebilir istek üstbilgileri. Yukarıdaki örnekte, x-ms-meta veri ile x-ms-meta-hedef ve x-ms-meta-abc başlayan tüm meta veri üstbilgileri izin verilir. Unutmayın joker karakteri ' *' Belirtilen önek herhangi üstbilgi başlayarak izin verildiğini gösterir.
* **ExposedHeaders**: İstek veren tarayıcıya tarafından kullanıma sunulan ve CORS isteğine yanıt olarak gönderilen olabilir yanıt üstbilgileri. Yukarıdaki örnekte, tüm üst bilgisi x-ms-meta başlayarak kullanıma sunmak için tarayıcı talimatı verilir.
* **MaxAgeInSeconds**: istek bir tarayıcı ön kontrol seçenekleri önbelleğe maksimum süresi.

Azure storage Hizmetleri belirten önekli üstbilgileri her ikisi için destek **AllowedHeaders** ve **ExposedHeaders** öğeleri. Bir kategori üstbilgilerinin izin vermek için ortak bir önek kategorisine belirtebilirsiniz. Örneğin, belirten *x-ms-meta** gibi önekli üst bilgisi x-ms-meta ile başlayan tüm üstbilgileri eşleşecek bir kural oluşturur.

CORS kuralları aşağıdaki sınırlamalar uygulanır:

* Depolama Birimi hizmetini (Blob, tablo ve Kuyruk) başına en fazla beş CORS kuralları belirtebilirsiniz.
* XML etiketleri hariç tüm CORS kuralları ayarları isteğinde en büyük boyutu 2 KB aşamaz.
* Bir izin verilen üst bilgi, sunulan üstbilgisi veya izin verilen kaynağı uzunluğu 256 karakteri aşamaz.
* İzin verilen üstbilgileri ve sunulan üstbilgileri ya da olabilir:
  * Değişmez değer üst bilgiler, burada tam üstbilgi adı sağlanır, gibi **x-ms-meta-işlenen**. En fazla 64 değişmez değer üstbilgileri isteği belirtilebilir.
  * Üst bilgiler, burada bir önek üstbilgisinin sağlanır, gibi önekli ** x-ms-meta-data ***. Bu şekilde bir önek belirtme izin verir veya belirtilen önek ile başlayan herhangi bir başlığını kullanıma sunar. En fazla iki önekli üstbilgi isteği belirtilebilir.
* Belirtilen yöntemler (veya HTTP fiilleri) **AllowedMethods** öğesi Azure depolama hizmeti API tarafından desteklenen yöntemleri için uygun olmalıdır. Desteklenen yöntemler, silme, GET, HEAD, birleştirme, POST, seçenekleri ve PUT ' tur.

## <a name="understanding-cors-rule-evaluation-logic"></a>CORS kural değerlendirme mantığı anlama
Bir depolama birimi hizmeti denetim öncesi ya da gerçek bir istek aldığında, uygun hizmet özelliklerini ayarlama işlemi aracılığıyla hizmeti için oluşturulan CORS kurallar temel alınarak bu isteği değerlendirir. CORS kuralları hizmet özelliklerini ayarlama işlemi istek gövdesinde ayarlanan sırayla değerlendirilir.

CORS kuralları aşağıdaki gibi değerlendirilir:

1. İlk olarak, isteğin kaynak etki alanı için listelenen etki alanları karşılaştırılarak **AllowedOrigins** öğesi. Kaynak etki alanı listesinde yer ya da tüm etki alanları joker karakterle izin verilen ' *', değerlendirme kazançlar kuralları. Kaynak etki alanı dahil değilse istek başarısız olur.
2. Ardından, istek yöntemi (veya HTTP fiili) listelenen yöntemleri karşılaştırılarak **AllowedMethods** öğesi. Yöntemi listeye dahil, kuralları değerlendirme devam eder; değilse istek başarısız olur.
3. İstek, kaynak etki alanı ve onun yöntemi kuralında eşleşiyorsa, bu kural istek ve başka hiçbir kural değerlendirilir işlemi için seçilir. İstek başarılı olabilmesi için önce ancak istekte belirtilen üstbilgileri listelenen üstbilgileri karşı denetlenir **AllowedHeaders** öğesi. Gönderilen üstbilgileri izin verilen üstbilgileri eşleşmiyorsa istek başarısız olur.

İstek gövdesinde sırada işlenir olduğundan, böylece bunlar ilk olarak değerlendirilir en iyi yöntemler kaynakları göre en kısıtlayıcı kurallar ilk listede belirtilmesi önerilir. Daha az kısıtlayıcı – – listesinin sonunda tüm kaynaklara izin veren bir kural Örneğin, kuralları belirtin.

### <a name="example--cors-rules-evaluation"></a>Örnek – değerlendirme CORS kuralları
Aşağıdaki örnek, depolama hizmetleri için CORS kuralları ayarlamak bir işlem için bir kısmi istek gövdesi gösterir. Bkz: [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), ve [tablo hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452240.aspx) isteği oluşturma hakkında bilgi.

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
| **Yöntemi** |**Origin** |**İstek üstbilgileri** |**Kural eşleştirme** |**Sonuç** |
| **PUT** |http://www.contoso.com |x-ms-blob-content-type |İlk kural |Başarılı |
| **GET** |http://www.contoso.com |x-ms-blob-content-type |İkinci kuralı |Başarılı |
| **GET** |http://www.contoso.com |x-ms-client-request-id |İkinci kuralı |Hata |

İlk istek ilk kuralıyla eşleşen – kaynak etki alanı izin verilen çıkış noktası eşleştirir, izin verilen yöntemleri yöntem eşleşiyor ve üstbilgi izin verilen üstbilgileri – eşleşen ve bu nedenle başarılı.

Yöntemi, izin verilen yöntemleri eşleşmediği için ikinci bir istek ilk kural eşleşmiyor. Başarılı şekilde ancak, ikinci kural, aynı.

Daha fazla kural değerlendirilir şekilde üçüncü isteği yöntemi ve kaynak etki alanı ikinci kuralında eşleşir. Ancak, *x-ms-istemci-request-id üstbilgi* üçüncü kural semantiği başarılı olması için izin verilen olgu isteği başarısız şekilde ikinci kural tarafından izin verilmiyor.

> [!NOTE]
> Bu örnek daha az kısıtlayıcı bir kural önce daha kısıtlayıcı bir gösterir, ancak genel en iyi en kısıtlayıcı kurallarını listelemeyi uygulamadır.
> 
> 

## <a name="understanding-how-the-vary-header-is-set"></a>Ayırmayı üstbilgi nasıl belirlendiğini anlama
*Ayırmayı* başlığıdır, tarayıcı veya Kullanıcı Aracısı sunucu tarafından isteği işlemek üzere seçilmiş olan ölçütleri hakkında öneri isteği üstbilgi alanları kümesinden oluşan bir standart HTTP/1.1 üstbilgisi. *Ayırmayı* üstbilgisi, proxy'leri, tarayıcıları ve nasıl yanıt önbelleğe alınması belirlemek için kullanın CDN'ler tarafından önbelleğe alma işlemi için temel olarak kullanılır. Ayrıntılar için bkz: belirtimi için [ayırmayı üstbilgi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Kaynak etki alanı, tarayıcı veya başka bir kullanıcı aracısı CORS istek yanıtı önbelleğe alır, izin verilen kaynağı önbelleğe alınır. Önbelleği etkin durumdayken ikinci bir etki alanı depolama kaynak aynı isteği gönderdiğinde, önbelleğe alınan kaynak etki alanı kullanıcı aracısını alır. Aksi takdirde başarılı, istek başarısız şekilde ikinci etki alanına önbelleğe alınan etki alanı eşleşmiyor. Bazı durumlarda, Azure Storage ayırmayı üstbilgisini ayarlar **kaynak** isteyen etki alanı önbelleğe alınmış kaynaktan farklı olduğu durumlarda hizmete sonraki CORS istek göndermek için kullanıcı aracısı yüklemesini istemek için.

Azure depolama kümeleri *ayırmayı* başlığına **kaynak** aşağıdaki durumlarda gerçek GET/HEAD istekleri için:

* Ne zaman istek kaynağını CORS kuralı tarafından tanımlanan izin verilen kaynağı tam olarak eşleşir. Tam olarak eşleşmelidir için CORS kuralı bir joker karakter içeremez ' * ' karakteri.
* İstek kaynağı eşleşen kural yok ancak CORS depolama hizmeti için etkinleştirilir.

GET/HEAD isteği tüm kaynaklara izin veren bir CORS kuralı eşleştiği durumda yanıt tüm çıkış noktaları kullanılabilir ve önbellek etkinken kullanıcı aracısı önbellek herhangi bir kaynak etki alanı gelen sonraki istekleri sağlayacak gösterir.

GET/HEAD dışındaki yöntemlerle istekleri için unutmayın, bu yöntemleri yanıtlarını kullanıcı aracıları tarafından önbelleğe alınmaz beri depolama hizmetleri ayırmayı üstbilgi ayarlamaz.

Aşağıdaki tabloda nasıl Azure depolama gösterir üzerinde yukarıda açıklanan durumlarda GET/HEAD isteklerini yanıtlar:

| İstek | Hesabı ayarı ve kural değerlendirme sonucu |  |  | Yanıt |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **İstekte mevcut kaynak üstbilgisi** |**Bu hizmet için belirtilen CORS kuralları** |**Tüm origins(*) veren eşleşen kural var** |**Tam kaynak eşleşme için eşleşen bir kural var** |**Yanıt kaynağa ayarlamak ayırmayı üstbilgisi içeriyor** |**Access-Control-izin verilen-Origin yanıtı içerir: "*"** |**Access-Control-kullanıma sunulan-Headers yanıtı içerir** |
| Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |
| Hayır |Evet |Hayır |Hayır |Evet |Hayır |Hayır |
| Hayır |Evet |Evet |Hayır |Hayır |Evet |Evet |
| Evet |Hayır |Hayır |Hayır |Hayır |Hayır |Hayır |
| Evet |Evet |Hayır |Evet |Evet |Hayır |Evet |
| Evet |Evet |Hayır |Hayır |Evet |Hayır |Hayır |
| Evet |Evet |Evet |Hayır |Hayır |Evet |Evet |

## <a name="billing-for-cors-requests"></a>CORS isteklerini faturalama
Başarılı ön istekleri hesabınız için depolama hizmetlerden herhangi biri için CORS etkinleştirilirse faturalandırılır (çağırarak [Blob hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452235.aspx), [kuyruk hizmeti özelliklerini ayarla](https://msdn.microsoft.com/library/hh452232.aspx), veya [Tablo hizmeti özelliklerini ayarlama](https://msdn.microsoft.com/library/hh452240.aspx)). Ücretleri en aza indirmek için ayarı göz önünde bulundurun. **MaxAgeInSeconds** , CORS öğesinde kuralları büyük bir değere böylece kullanıcı aracısı istek önbelleğe alır.

Başarısız denetim öncesi isteklerde fatura değil.

## <a name="next-steps"></a>Sonraki adımlar
[Blob hizmeti özelliklerini ayarlama](https://msdn.microsoft.com/library/hh452235.aspx)

[Sıra Hizmeti özelliklerini ayarlama](https://msdn.microsoft.com/library/hh452232.aspx)

[Tablo hizmeti özelliklerini ayarlama](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C çıkış noktaları arası kaynak paylaşımı belirtimi](http://www.w3.org/TR/cors/)

