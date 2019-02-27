---
title: Azure uygulama yapılandırması anahtar-değer deposu | Microsoft Docs
description: Yapılandırma verilerini Azure uygulama yapılandırmasında nasıl depolandığını genel bakış
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: f04067358b0b2bae745727a5dd7a1f5554f9f70e
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884821"
---
# <a name="key-value-store"></a>Anahtar değeri deposu

Azure uygulama yapılandırması yapılandırma verilerini çeşitli türdeki geliştiriciler, alışık olduğunuz uygulama ayarları temsil eden bir basit ancak esnek şekilde olan anahtar-değer çiftleri olarak depolar.

## <a name="keys"></a>Anahtarlar

Anahtarlar, anahtar-değer çiftleri için ad olarak görev yapar ve depolamak ve ilgili değerleri almak için kullanılır. Bir karakter sınırlayıcıyı kullanarak hiyerarşik ad anahtarları düzenlemek için yaygın bir yöntemdir (gibi `/` veya `:`) uygulamanız için en uygun olan bir kurala göre. Uygulama yapılandırma anahtarlarını bir bütün olarak değerlendirir. Tuşlarını kullanarak nasıl adlarıyla yapılandırılmış veya bunlar üzerinde herhangi bir kural zorlama ekleyeceğimi ayrıştırma değil.

Anahtar-değer için belirli bir adlandırma düzeni uygulama çerçeveleri içinde yapılandırma deposu kullanımını gerektirebilir. Örneğin, Java'nın Spring Cloud çerçevesi tanımlar `Environment` dahil olmak üzere değişkenleri tarafından parametre haline getirilip için Spring uygulaması ayarları sağlamak kaynakları *uygulama adı* ve *profili*. Spring grubundan tuşları ilişkin yapılandırma verileri genellikle bu iki öğenin ayrı ayrı bir sınırlandırıcı ile başlar.

Uygulama Yapılandırması içinde depolanan anahtarlar büyük/küçük harfe, unicode tabanlı dizelerdir. Anahtarları *app1* ve *App1* bir uygulama yapılandırma deposunda farklıdır. Yapılandırma ayarları, uygulama içindeki bazı çerçeveleri yapılandırma anahtarları case-insensitively işleyeceği kullanırken göz önünde bulundurun. Örneğin, ASP.NET Core yapılandırma sistemi anahtarlarını büyük küçük harf duyarsız dize olarak değerlendirir. Uygulama Yapılandırması içinde ASP.NET Core uygulamasını sorgulanırken beklenmeyen davranışları önlemek için yalnızca büyük/küçük harf tarafından farklı anahtarları kullanılmamalıdır.

Anahtar adları dışında Uygulama yapılandırması girilen herhangi bir unicode karakter kullanmasına izin `*`, `,` ve `\` hangi ayrılmıştır. Ayrılmış bir karakter içerecek şekilde gerekiyorsa kullanarak atlatmak gerekir `\{Reserved Character}`. 10K karakterler bir anahtar-değer çifti üzerinde toplam boyut sınırı yoktur. Bu anahtar, değeri, tüm karakterleri içerir ve ilişkili tüm isteğe bağlı öznitelikleri. Bu sınırı içinde anahtarları için birçok hiyerarşik düzeylerine sahip olabilir.

### <a name="designing-key-namespaces"></a>Anahtar ad alanları tasarlama

Yapılandırma verileri için kullanılan anahtarları adlandırma için iki genel yaklaşım vardır: düz veya hiyerarşik. Bu yöntemler bir uygulama kullanım açısından oldukça benzerdir, ancak ikinci çok sayıda avantaj sunar:

* Kolay okunur. Uzun bir dizi karakter yerine hiyerarşik bir anahtar adı sınırlayıcı bir cümle boşluk olarak işlevler ve sözcükler arasındaki doğal sonu sağlar.
* Yönetilmesi daha kolay. Anahtar adı hiyerarşi yapılandırma verilerinin mantıksal gruplar temsil eder.
* Kullanmayı daha kolay. Desen-eşleşmeleri hiyerarşik bir yapıda anahtarları ve yapılandırma verilerini yalnızca bir kısmını alır bir sorgu yazmak daha kolaydır. Ayrıca, birçok yeni programlama çerçevelerini hiyerarşik yapılandırma verileri, uygulamanızın yapabileceğiniz gibi belirli yapılandırma kümesi kullanmak için yerel destek sahip.

Uygulama yapılandırma anahtarlarını birçok yönden hiyerarşik olarak düzenleyebilirsiniz. Bu tür anahtarlar olarak düşünebilirsiniz [URI'ler](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier). Her bir kaynak hiyerarşik anahtardır *yolu* sınırlayıcılarla birleştirilen bir veya daha fazla bileşenden oluşur. Programlama dili veya framework, hangi bir uygulamaya göre sınırlayıcı olarak kullanmak için hangi karakter gereken seçebilirsiniz. Anahtarları farklı uygulama yapılandırması için birden çok sınırlayıcıya kullanabilirsiniz.

Bir hiyerarşiye, anahtar adlarını nasıl yapısı çeşitli örnekler şunlardır:

* Ortamda tabanlı

        AppName:Test:DB:Endpoint
        AppName:Staging:DB:Endpoint
        AppName:Production:DB:Endpoint

* Bileşen Hizmetleri tabanlı

        AppName:Service1:Test:DB:Endpoint
        AppName:Service1:Staging:DB:Endpoint
        AppName:Service1:Production:DB:Endpoint
        AppName:Service2:Test:DB:Endpoint
        AppName:Service2:Staging:DB:Endpoint
        AppName:Service2:Production:DB:Endpoint

* Dağıtım bölgelerine bağlı

        AppName:Production:Region1:DB:Endpoint
        AppName:Production:Region2:DB:Endpoint

### <a name="versioning-key-values"></a>Sürüm oluşturma anahtar-değer

Anahtar değerlerini uygulama yapılandırmasında isteğe bağlı olarak olabilir bir **etiket** özniteliği. Etiketler, aynı anahtarla anahtar-değer ayırt etmek için kullanılır. Bir anahtar *app1* etiketlerle *v1* ve *v2* iki ayrı anahtar-değer uygulama yapılandırma deposunda form. Varsayılan olarak, bir anahtar-değer etiketi boş (veya `null`).

Uygulama yapılandırması değiştirilmiş olarak değil sürüm anahtar-değer otomatik olarak yapar. Bir anahtar-değer birden fazla sürümünü oluşturmak için bir yol etiketleri kullanabilirsiniz. Örneğin, bir uygulama sürüm numarası girebilirsiniz veya anahtar değerlerini tanımlamak için bir Git işleme kimliği etiketlerde belirli yazılım derleme ile ilişkili.

Etiketler, herhangi bir unicode karakter dışında kullanmak için izin `*`, `,` ve `\`, hangi ayrılmıştır. Ayrılmış bir karakter içerecek şekilde gerekiyorsa kullanarak atlatmak gerekir `\{Reserved Character}`.

### <a name="querying-key-values"></a>Anahtar-değer sorgulama

Her anahtar-değer anahtarıyla ayrıca olabilir bir etiket tarafından benzersiz şekilde tanımlanır `null`. Bir uygulama yapılandırma deposu için anahtar-değer, bir desen belirterek sorgulayın. Uygulama yapılandırma deposu tüm anahtar-deseniyle eşleşen değerleri döndürür ve bunlara karşılık gelen değerleri ve öznitelikleri. Uygulama yapılandırması için REST API çağrıları, aşağıdaki anahtar desenleri kullanabilirsiniz:

| Anahtar | |
|---|---|
| `key` Atlanırsa veya `key=*` | Tüm anahtarları eşleşir |
| `key=abc` | Anahtar adı eşleşmeleri **abc** tam olarak. |
| `key=abc*` | Eşleşme ile başlayan adlar anahtar **abc** |
| `key=*abc` | Eşleşme ile biten anahtar **abc** |
| `key=*abc*` | Eşleşme anahtar içeren adları **abc** |
| `key=abc,xyz` | Eşleşme anahtar adları **abc** veya **xyz**, 5 Csv'leri sınırlıdır |

Aşağıdaki etiketi desenleri de ekleyebilirsiniz:

| Etiket | |
|---|---|
| `label` Atlanırsa veya `label=*` | Herhangi bir etiketi ile eşleşen dahil olmak üzere `null` |
| `label=%00` | Eşleşme `null` etiketi. |
| `label=1.0.0` | Eşleşen etiket **1.0.0** tam olarak |
| `label=1.0.*` | İle başlayan etiket ile eşleşen **1.0.** |
| `label=*.0.0` | Eşleşen ile bitiş etiketleri **.0.0** |
| `label=*.0.*` | Eşleşen etiketler içeren **.0.** |
| `label=%00,1.0.0` | Eşleşen etiketler `null` veya **1.0.1**, 5 Csv'leri sınırlıdır |

## <a name="values"></a>Değerler

Anahtarlar için atanan değerler de unicode dizelerdir. Tüm unicode karakterleri değerlerini kullanabilirsiniz. Var olan kullanıcı tanımlı isteğe bağlı **içerik türü** her değeri ile ilişkili. Uygulamanızın düzgün bir şekilde işlemeye yardımcı olan bir değer hakkında bilgi (örneğin, kodlama düzeni) depolamak için bu özniteliği kullanabilirsiniz.

Tüm anahtarları ve değerleri de dahil olmak üzere bir uygulama yapılandırma deposunda, depolanan yapılandırma verilerini beklerken ve aktarım sırasında şifrelenir. Yine de uygulama yapılandırması Azure Key Vault için bir yedek çözüm değildir. Bunu herhangi bir uygulama gizli dizilerini depolanmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Kavram: Zaman içinde nokta anlık görüntü](concept-point-time-snapshot.md)  
