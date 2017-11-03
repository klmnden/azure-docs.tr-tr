---
title: "Azure kimlik geliştiriciler için destek ve Yardım seçenekleri | Microsoft Docs"
description: "Yardım ve Destek geliştirme ile ilgili sorular ve sorunlar için Microsoft Azure kimlikleri (Azure Active Directory ve MSA) ile tümleşen uygulama oluştururken elde etme bildirin"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/27/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 7c382da9bd9032b30f7c620e839c41c1756ba3f6
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="support-and-help-options-for-developers"></a>Geliştiriciler için destek ve Yardım seçenekleri 

Ne olursa olsun yalnızca Azure Active Directory ile Microsoft kimlikleri veya Microsoft Graph API tümleştirmek başlatıyorsanız veya uygulamanız için yeni bir özellik uygularken topluluktan yardım alın veya anlamanıza gerek zamanlar vardır Geliştirici olarak seçenekleri destekler. Bu makale, bir özeti aşağıda bu seçenekleri anlamanıza yardımcı olur:

> [!div class="checklist"]
> * Sorun sorunuzu topluluk tarafından yanıtlanan değil veya var olan bir özelliğin belgelerine zaten uygulamak deniyorsanız denetlemek için arama var.
> * Bazı durumlarda, yalnızca bizim destek araçları belirli bir sorununuzu hata ayıklama yardımcı olması için kullanmak istediğiniz
> * Gerekenler gelen yanıt bulamazsanız, üzerinde bir soru sorun isteyebilirsiniz *yığın taşması*
> * Bizim kimlik doğrulama kitaplıkları biri ile ilgili bir sorun bulursanız, yükseltmek bir *GitHub* sorunu
> * Son olarak, birine konuşun ihtiyacınız varsa, bir destek isteği açmak isteyebilirsiniz


## <a name="search"></a>Arama

Geliştirme ile ilgili bir sorunuz varsa, gereksinim duyduğunuz Belgelerimizi üzerinde yanıt bulamıyor olabilir bizim [github örnekleri](https://github.com/azure-samples), veya için yanıtlar [yığın taşması](https://www.stackoverflow.com) sorular.

### <a name="scoped-search"></a>Kapsamlı arama
Daha hızlı sonuçlar için aşağıdakileri kullanarak yığın taşması, Belgelerimizi ve bizim kod örnekleri arama kapsamınızı, [sık kullanılan arama motoru](https://bing.com):
```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/en-us/graph)
```
Burada *{Your arama terimleri}* , arama anahtar sözcüklerinizi değil.
<br/>

## <a name="use-our-development-support-tools"></a>Bizim geliştirme destek araçlarını kullanma

|Aracı  |Açıklama  |
|---------|---------|
|[jwt.MS](https://jwt.ms)| Talep adları ve değerleri çözecek kimliği veya erişim belirteçleri yapıştırın |
|[Hata kodu Çözümleyicisi](https://apps.dev.microsoft.com/portal/tools/errors)| Oturum açma sırasında alınan bir hata kodu yapıştırın veya olası nedenleri ve düzeltmeler görmek için sayfaları onayı |
|[Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer)| İsteği yapmak ve yanıtları Microsoft Graph API karşı bakın sağlar aracı|

<br/>

[![Yığın Taşması](media/active-directory-develop-help-support/stackoverflow-logo.png)](https://www.stackoverflow.com)
## <a name="post-a-question-to-stack-overflow"></a>Yığın taşması için bir soru gönderin

Yığın taşması olduğundan geliştirme ile ilgili sorular - tercih edilen kanalı burada hem Microsoft olarak topluluk üyeleri ekip üyelerinin sorununuzu çözmenize yardımcı olacak doğrudan ilgilidir.

Sorununuzu arama ile yanıtını bulamazsanız, yığın taşması için yeni bir soru gönderin: aşağıdakilerden birini etiketlerini tanımlayın, topluluk yardımcı olması için soruları yaparken kullanım sonra sorunuzu zamanında yanıt:

|Bileşen/alan  |Etiketler  |
|---------|---------|
|ADAL kitaplığı |[[adal]](http://stackoverflow.com/questions/tagged/adal)|
|MSAL kitaplığı     |[[msal]](http://stackoverflow.com/questions/tagged/msal)|
|OWIN ara yazılımı  |[[azure-active-directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)|
|[Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  |[[azure ad b2b]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  |[[azure ad b2c]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Microsoft Graph API](https://developer.microsoft.com/graph/) |[[microsoft graph]](http://stackoverflow.com/questions/tagged/microsoft-graph)
|Herhangi bir alan kimlik doğrulama veya yetkilendirme ilgili konular |[[azure-active-directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)
<br/>
> [!TIP]
> Yığın taşması gelen aşağıdaki gönderileri soruları yapma ipuçları içerir ve kaynak kodu - bu yönergeleri izleyerek ekleme ile ilgili ipuçları değerlendirmek ve sorunuzu hızlı yanıt topluluk üyeleri olasılığını artırmaya yardımcı olabilir:  
> - [İyi bir soru nasıl sorun](https://stackoverflow.com/help/how-to-ask)
> - [En az, tam ve doğrulanabilen örnek oluşturma](https://stackoverflow.com/help/mcve)

<br/>


[![Yığın Taşması](media/active-directory-develop-help-support/github-logo.png)](https://www.github.com)
## <a name="create-a-github-issue"></a>GitHub sorunu oluşturma

 Bir hata veya bizim kitaplıklara ilgili bir sorun bulursanız, bir sorunu bizim GitHub depolarının yükseltin. Bizim kitaplıkları açık kaynak olduğundan, ayrıca de bir çekme isteği gönderin serbestsiniz. Aşağıdaki makalede kitaplıkları ve bunların GitHub depolarının listesi içerir:

- [ADAL, MSAL ve Owın ara yazılımı](active-directory-authentication-libraries.md) kitaplıkları ve GitHub depoları

<br/>

## <a name="open-a-support-request"></a>Bir destek isteği açın

Birine konuşun ihtiyacınız varsa, bir destek isteği açabilirsiniz. Bir Azure müşterisiyseniz, çeşitli destek seçeneği bulunur. Planları karşılaştırmak için bkz: [bu sayfayı](https://azure.microsoft.com/support/plans/). Geliştirici desteği de Azure müşterileri için kullanılabilir. Geliştirici destek planları satın alma hakkında daha fazla bilgi için bkz: [bu sayfayı](https://azure.microsoft.com/support/plans/developer/).

- Bir Azure destek planı, zaten varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

- Bir Azure müşterisi değilseniz, ayrıca bir destek isteği Microsoft ile açabilirsiniz [ticari desteği hizmetimizi](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial).

Ayrıca deneyebilirsiniz [bizim sanal Aracısı](https://support.microsoft.com/contactus/?ws=support) destek almak veya sorular sormak için.

### <a name="free-chat-support-for-a-limited-time"></a>Sınırlı bir süre için ücretsiz sohbet desteği

Sınırlı bir süre için Microsoft Partners'ücretsizdir bizim sohbet desteği - de kullanabilirsiniz. Şirketiniz Microsoft Partner değilse, ücretsiz kaydetmek ve başka avantajları giderek elde [burada](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx).

Şirketiniz kaydolduktan sonra sohbet isteği başlatabilirsiniz [burada](https://aka.ms/devchat).