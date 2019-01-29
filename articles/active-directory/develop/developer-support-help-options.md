---
title: Azure kimlik geliştiriciler için destek ve Yardım seçenekleri | Microsoft Docs
description: Yardım ve geliştirme ile ilgili sorular ve sorunlar için destek Microsoft Azure kimlikleri (Azure Active Directory ve MSA) ile tümleştirme uygulaması oluştururken elde etme bildirin
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/27/2017
ms.author: celested
ms.reviewer: andret
ms.custom: aaddev
ms.openlocfilehash: f8c5e5f598ab8566eacb594ff66b63ce3793f57f
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093207"
---
# <a name="support-and-help-options-for-developers"></a>Geliştiriciler için destek ve Yardım seçenekleri

Bağımsız olarak yalnızca Azure Active Directory ile Microsoft kimlikleri veya Microsoft Graph API, tümleştirme başlatıyorsanız veya uygulamanız için yeni bir özellik uygularken topluluktan Yardım almak veya anlamak için ihtiyacınız olan zamanlar vardır bir geliştirici olarak sahip olduğunuz seçeneklerini destekler. Bu makalede bir özeti aşağıda bu seçenekleri anlamanıza yardımcı olur:

> [!div class="checklist"]
> * Sorun sorunuzu topluluk tarafından cevaplanıp değil veya özellik için mevcut bir belge zaten uygulamak çalıştığınız denetlemek için arama var.
> * Bazı durumlarda yalnızca destek araçlarımızı belirli bir sorununuz hatalarını ayıklamanıza yardımcı olmak için kullanmak istediğiniz
> * Aradığınızı gelen yanıt bulamazsanız, soru sorabilirsiniz isteyebilirsiniz *Stack Overflow*
> * Kimlik doğrulama kitaplıklarımızı biri ile ilgili bir sorun bulursanız, yükseltmek bir *GitHub* sorunu
> * Son olarak, birine konuşmak gerekirse, bir destek isteği açmak isteyebilirsiniz


## <a name="search"></a>Arama

Geliştirme ile ilgili bir sorunuz varsa, belgelerimize üzerinde ihtiyacınız yanıt bulmak mümkün olabilir bizim [GitHub örneklerine](https://github.com/azure-samples), veya yanıtlarını [Stack Overflow](https://www.stackoverflow.com) sorular.

### <a name="scoped-search"></a>Kapsamlı arama
Aşağıdakileri kullanarak aramanızı Stack Overflow, belgelerimize ve kod örneklerimizi daha hızlı sonuçlar için kapsam, [sık kullanılan arama motorunuz](https://bing.com):
```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/en-us/graph)
```
Burada *{Your arama terimlerini}* arama anahtar sözcüklerinizi olduğu.
<br/>

## <a name="use-our-development-support-tools"></a>Geliştirme desteği araçlarımızı kullanın

|Aracı  |Açıklama  |
|---------|---------|
|[jwt.ms](https://jwt.ms)| Talep adlarını ve değerlerini çözülecek kimliği veya erişim belirteçleri yapıştırın |
|[Hata kodu Çözümleyicisi](https://apps.dev.microsoft.com/portal/tools/errors)| Oturum açma sırasında alınan hata kodu yapıştırın veya sayfaları olası nedenleri ve düzeltmeleri görmek için onay |
|[Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer)| İstekleri ve yanıtları Microsoft Graph API karşı görmek olanak tanıyan bir araç|

<br/>

[![Yığın Taşması](./media/developer-support-help-options/stackoverflow-logo.png)](https://www.stackoverflow.com)
## <a name="post-a-question-to-stack-overflow"></a>Stack Overflow için bir soru gönderin

Yığın taşması olduğundan geliştirme ile ilgili sorular için-tercih edilen kanal burada her iki olarak Microsoft, topluluk üyeleri takım üyeleri, sorununuzu çözmenize yardımcı olacak doğrudan ilgilidir.

Bir arama ile sorununuzun yanıtını bulamazsanız, yeni Stack Overflow soru gönderin: ardından yanıt sorunuzu zamanında üzerinde tanımlayın, Topluluk Yardımı soruların yaparken etiketleri aşağıdakilerden birini kullanın:

|Bileşen/alan  |Etiketler  |
|---------|---------|
|ADAL kitaplığı |[[adal]](https://stackoverflow.com/questions/tagged/adal)|
|MSAL kitaplığı     |[[msal]](https://stackoverflow.com/questions/tagged/msal)|
|OWIN ara yazılımı  |[[azure-active-directory]](https://stackoverflow.com/questions/tagged/azure-active-directory)|
|[Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  |[[azure ad b2b]](https://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  |[[azure-ad-b2c]](https://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Microsoft Graph API'si](https://developer.microsoft.com/graph/) |[[microsoft graph]](https://stackoverflow.com/questions/tagged/microsoft-graph)
|Herhangi bir alan için kimlik doğrulama veya yetkilendirme ilgili konular |[[azure-active-directory]](https://stackoverflow.com/questions/tagged/azure-active-directory)
<br/>
> [!TIP]
> Stack Overflow aşağıdaki gönderilerinden sorular yapma konusunda ipuçları içerir ve bu yönergeleri izleyerek kaynak kodu - ekleme ipuçları topluluk üyelerinin değerlendirmek ve sorularınıza hızla yanıt olasılığını artırmaya yardımcı olabilir:
> - [İyi bir soru nasıl miyim](https://stackoverflow.com/help/how-to-ask)
> - [En az, tam ve doğrulanabilir örnek oluşturma](https://stackoverflow.com/help/mcve)

<br/>


[![Yığın Taşması](./media/developer-support-help-options/github-logo.png)](https://www.github.com)
## <a name="create-a-github-issue"></a>Bir GitHub sorunu oluşturun

 Bir hata veya bizim kitaplıklara ilgili bir sorun bulursanız, bir sorunu GitHub depolarımızdaki yükseltin. Bizim kitaplıkları açık kaynak olduğundan, ayrıca de bir çekme isteği göndermek ücretsizdir. Kitaplıkları ve kendi GitHub depoları listesini makaleyi içerir:

- [ADAL MSAL ve Owın ara yazılımı](active-directory-authentication-libraries.md) kitaplıkları ve GitHub depoları

<br/>

## <a name="open-a-support-request"></a>Bir destek isteği açın

Birine konuşmak gerekirse, bir destek isteği açabilirsiniz. Bir Azure müşterisiyseniz, kullanılabilir çeşitli destek seçenekleri. Planları karşılaştırmak için bkz [bu sayfayı](https://azure.microsoft.com/support/plans/). Geliştirici Desteği, Azure müşterileri için de kullanılabilir. Geliştirici destek planlarını satın alma hakkında daha fazla bilgi için bkz: [bu sayfayı](https://azure.microsoft.com/support/plans/developer/).

- Bir Azure destek planı, zaten varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

- Bir Azure müşterisi değilseniz, ayrıca bir destek isteği Microsoft ile açabilirsiniz [ticari desteğimiz](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial).

Ayrıca deneyebilirsiniz [sanal aracımızı](https://support.microsoft.com/contactus/?ws=support) destek almak veya soru sormak için.

### <a name="free-chat-support-for-a-limited-time"></a>Sınırlı bir süreliğine ücretsiz sohbet desteği

Sınırlı bir süre için ücretsiz Microsoft Partners için sunduğumuz sohbet desteği - de kullanabilirsiniz. Şirketiniz Microsoft Partner değilse, ücretsiz kaydedebilir ve giderek diğer avantajları elde [burada](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx).

Şirketiniz kaydettikten sonra sohbet istek başlayabilirsiniz [burada](https://aka.ms/devchat).
