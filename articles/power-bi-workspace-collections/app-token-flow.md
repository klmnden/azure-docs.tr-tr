---
title: Kimlik doğrulama ve yetkilendirme Power BI çalışma koleksiyonlarla | Microsoft Docs
description: Kimlik doğrulaması ve yetkilendirme Power BI çalışma koleksiyonlarla.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ROBOTS: NOINDEX
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: 74d34e708fb74daa295642d50643b78af8f6cb7a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="authenticating-and-authorizing-with-power-bi-workspace-collections"></a>Kimlik doğrulaması ve Power BI çalışma koleksiyonlarla yetkisi verme

Power BI çalışma koleksiyonları kullanım **anahtarları** ve **uygulama belirteçleri** kimlik doğrulaması ve yetkilendirme açık son kullanıcı kimlik doğrulaması yerine. Bu modelde, kimlik doğrulama ve yetkilendirme, son kullanıcılarınız için uygulamanızı yönetir. Gerekli olduğunda, uygulamanızın oluşturur ve istenen rapor oluşturmak üzere hizmete söyleyin uygulama belirteçleri gönderir. Hala; ancak bu tasarımı uygulamanız Azure Active Directory kullanıcı kimlik doğrulaması ve yetkilendirme için kullanılacak gerektirmez.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

## <a name="two-ways-to-authenticate"></a>Kimlik doğrulaması için iki yol

**Anahtar** -tüm Power BI çalışma koleksiyonları REST API çağrıları için tuşlarını kullanabilirsiniz. Anahtarları bulunabilir **Microsoft Azure portal** seçerek **tüm ayarları** ve ardından **erişim anahtarları**. Her zaman bir parola varsa gibi anahtarınızı kabul eder. Bu anahtarlar üzerinde belirli çalışma alanı koleksiyonu çağrı herhangi bir REST API'nin yapma izinlerine sahiptir.

Bir anahtar REST çağrısı kullanmak için aşağıdaki authorization üstbilgisi ekleyin:

    Authorization: AppKey {your key}

**Uygulama belirteci** -uygulama belirteçleri katıştırma tüm istekler için kullanılır. İstemci-tarafı çalıştırılmak üzere tasarlanmışlardır. Belirtecin tek bir rapor ve bir sona erme süresini ayarlamak için en iyi uygulama sınırlıdır.

Uygulama belirteçleri anahtarlarınızı biri tarafından imzalanan JWT (JSON Web belirteci) var.

Uygulama belirteci aşağıdaki talep içerebilir:

| İste | Açıklama |
| --- | --- |
| **ver** |Uygulama belirteci sürümü. 0.2.0 geçerli sürümdür. |
| **aud** |Belirtecin hedeflenen alıcı. Power BI çalışma koleksiyonları kullanın: "https://analysis.windows.net/powerbi/api." |
| **ISS** |Belirtecin uygulama gösteren bir dize. |
| **type** |Oluşturulan uygulama Belirtecin türü. Geçerli tek desteklenen türüdür **katıştırmak**. |
| **WCN** |Çalışma alanı koleksiyonu adı belirteci için yayımlanmaktadır. |
| **WID** |Çalışma alanı kimliği belirteci için yayımlanmaktadır. |
| **RID** |Rapor Kimliği belirteci için yayımlanmaktadır. |
| **Kullanıcı adı** (isteğe bağlı) |RLS ile kullanıldığında, kullanıcı adı, kullanıcı RLS kuralları uygularken belirlemenize yardımcı olabilecek bir dizedir. |
| **rolleri** (isteğe bağlı) |Satır düzeyi güvenlik kuralları uygularken seçmek için rolleri içeren bir dize. Birden fazla rol geçirilirse, dizesi dizisi olarak aktarılmalıdır. |
| **SCP** (isteğe bağlı) |İzinleri kapsamları içeren bir dize. Birden fazla rol geçirilirse, dizesi dizisi olarak aktarılmalıdır. |
| **exp** (isteğe bağlı) |Hangi belirtecin süresi sona erdiği saati belirtir. Değer UNIX zaman damgaları geçirilmesi. |
| **NBF** (isteğe bağlı) |İçinde belirtecin geçerli olan başladığı saati belirtir. Değer UNIX zaman damgaları geçirilmesi. |

Örnek bir uygulama belirtecini şuna benzer:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

Kodunu çözdü, şöyle görünür:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

Uygulama belirteçleri oluşturulmasını kolaylaştırmak SDK içinde kullanılabilir yöntem vardır. Örneğin, .NET için bakabilirsiniz [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) sınıfı ve [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) yöntemleri.

.NET SDK için başvurabilirsiniz [kapsamları](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Kapsamlar

Embed belirteçleri kullanırken, erişim hakkı vermek kaynak kullanımını kısıtlamak istediğinizi düşünelim. Bu nedenle, kapsamlı izinlere sahip bir belirteç oluşturabilirsiniz.

Power BI çalışma alanı koleksiyonu için kullanılabilir kapsamı verilmiştir.

|Kapsam|Açıklama|
|---|---|
|Dataset.Read|Belirtilen veri okuma izni sağlar.|
|Dataset.Write|Belirtilen veri kümesi için yazma izni sağlar.|
|Dataset.ReadWrite|Belirtilen veri kümesi için okumasına ve yazmasına izin sağlar.|
|Report.Read|Belirtilen rapor görüntüleme izni sağlar.|
|Report.ReadWrite|Görüntülemek ve belirtilen rapor düzenleme izni sağlar.|
|Workspace.Report.Create|Belirtilen çalışma alanı içinde yeni bir rapor oluşturma izni sağlar.|
|Workspace.Report.Copy|Var olan bir raporu belirtilen çalışma alanı içindeki kopyalamaya izin sağlar.|

Aşağıdaki gibi kapsamları arasında bir boşluk kullanarak birden çok kapsam sağlayabilir.

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Gerekli talep - kapsamları**

SCP: {scopesClaim} scopesClaim bir dize veya bir çalışma alanı kaynaklarına (rapor, veri kümesi, vb.) izin verilen izinleri belirterek bir dizeler dizisi olabilir

Kodu çözülmüş bir belirteç tanımlanan kapsamlarla benzer şekilde görünür:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a>İşlemler ve kapsamlar

|İşlem|Hedef kaynak|Belirteç izinleri|
|---|---|---|
|(Bellek içi) dayalı bir veri kümesine yeni bir rapor oluşturun.|Veri kümesi|Dataset.Read|
|(Bellek içi) dayalı bir veri kümesine yeni bir rapor oluşturun ve raporu kaydedin.|Veri kümesi|* Dataset.Read<br>* Workspace.Report.Create|
|Görüntüleyin ve (bellek içi) var olan bir raporu keşfedin/düzenleyin. Report.Read Dataset.Read anlamına gelir. Report.Read izin vermez düzenlemeler kaydediliyor.|Rapor|Report.Read|
|Düzenle ve var olan bir raporu kaydedin.|Rapor|Report.ReadWrite|
|(Farklı Kaydet) bir raporun bir kopyasını kaydedin.|Rapor|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-the-flow-works"></a>İşte akışı nasıl çalışır?
1. API anahtarları uygulamanıza kopyalayın. Anahtarları alabileceğiniz **Azure portal**.
   
    ![Azure portalında API anahtarları nerede bulacağını](media/get-started-sample/azure-portal.png)
1. Belirteç talep onaylar ve sona erme süresi vardır.
   
    ![Uygulama belirteci akışı - belirteç talep onaylar.](media/get-started-sample/token-2.png)
1. Belirteci bir API erişim anahtarlarla imzalanması.
   
    ![Uygulama belirteci akışı - token imzalı](media/get-started-sample/token-3.png)
1. Bir raporu görüntülemek için kullanıcı istekleri.
   
    ![Uygulama belirteci akışı - bir raporu görüntülemek için kullanıcı istekleri](media/get-started-sample/token-4.png)
1. Belirteç API'si erişim tuşları ile doğrulanır.
   
   ![Uygulama belirteci akışı - token doğrulandı](media/get-started-sample/token-5.png)
1. Power BI çalışma koleksiyonları, kullanıcıya bir rapor gönderir.
   
   ![Uygulama belirteci akışı - hizmet gönderme raporu kullanıcıya](media/get-started-sample/token-6.png)

Sonra **Power BI çalışma koleksiyonları** rapor kullanıcı, kullanıcı için bir özel uygulamanızda raporunu görüntüleyebilirsiniz. Örneğin, içeri aktardığınız [çözümleme satış verileri PBIX örnek](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), örnek web uygulaması aşağıdaki gibidir:

![Uygulamada ekli raporu örneği](media/get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Ayrıca Bkz.

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Microsoft Power BI çalışma koleksiyonları örneği kullanmaya başlama](get-started-sample.md)  
[Microsoft Power BI çalışma koleksiyonları senaryoları](scenarios.md)  
[Microsoft Power BI çalışma koleksiyonları ile çalışmaya başlama](get-started.md)  
[Powerbı CSharp Git deposu](https://github.com/Microsoft/PowerBI-CSharp)

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](http://community.powerbi.com/)