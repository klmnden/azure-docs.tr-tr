---
title: Azure AD uygulama geliştiricileri için destek ve Yardım seçenekleri | Microsoft Docs
description: Yardım ve geliştirme ile ilgili sorular ve sorunlar için destek Microsoft kimlikleri (Azure Active Directory ve Microsoft hesabı) ile tümleştirme uygulaması oluştururken elde etme bildirin
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2019
ms.author: ryanwi
ms.reviewer: jmprieur, dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c4882e991045b4a79c0ea0a19ad8fedc2fb8892
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540380"
---
# <a name="support-and-help-options-for-developers"></a>Geliştiriciler için destek ve Yardım seçenekleri

Yalnızca Azure Active Directory (Azure AD), Microsoft kimlikleri veya Microsoft Graph API ile tümleştirmek başlatılıyor veya uygulamanız için yeni bir özellik uygulama, topluluktan Yardım almak veya anlamak için gerektiğinde zamanlar vardır bir geliştirici olarak sahip olduğunuz seçeneklerini destekler. Bu makalede de dahil olmak üzere, bu seçenekleri anlamanıza yardımcı olur:

> [!div class="checklist"]
> * Sorunuzu topluluk tarafından yanıtlanmamış olup olmadığını veya mevcut bir belge özelliği için zaten uygulamak çalıştığınız arama yapma var.
> * Bazı durumlarda yalnızca destek araçlarımızı belirli bir sorunu çözmeye yardımcı olması için kullanmak istediğiniz
> * Gereksinim duyduğunuz yanıt bulamazsanız, soru sorabilirsiniz isteyebilirsiniz *Stack Overflow*
> * Kimlik doğrulama kitaplıklarımızı biri ile ilgili bir sorun bulursanız, yükseltmek bir *GitHub* sorunu
> * Son olarak, birine konuşmak gerekirse, bir destek isteği açmak isteyebilirsiniz

## <a name="search"></a>Arama

Geliştirme ile ilgili sorularınız varsa belgelerinde yanıt bulmak mümkün olabilir [GitHub örneklerine](https://github.com/azure-samples), veya yanıtlarını [Stack Overflow](https://www.stackoverflow.com) sorular.

### <a name="scoped-search"></a>Kapsamlı arama

Daha hızlı sonuçlar için sık kullanılan arama motorunuz içinde aşağıdaki sorguyu kullanarak Stack Overflow, belgelere ve kod örneklerini aramanızın kapsamını:

```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/graph)
```

Burada *{Your arama terimlerini}* arama sözcüklerinizi karşılık gelir.

## <a name="use-the-development-support-tools"></a>Geliştirme Destek Araçları'nı kullanın

| Tool  | Açıklama  |
|---------|---------|
| [jwt.ms](https://jwt.ms) | Yapıştırma kimliği veya talep adlarını ve değerlerini kodunu çözmek için erişim belirteci. |
| [Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer)| Aracı istekleri ve yanıtları Microsoft Graph API karşı görmeniz olanak tanır. |

## <a name="post-a-question-to-stack-overflow"></a>Stack Overflow için bir soru gönderin

Yığın taşması geliştirme ile ilgili sorular için tercih edilen bir kanaldır. Burada, topluluk üyeleri ve Microsoft Takım üyeleri doğrudan sorunlarınızı çözmenize yardımcı faydalanırsınız.

Bir arama aracılığıyla sorunuzun yanıtını bulamazsanız, yeni Stack Overflow soru gönderin. Aşağıdaki etiketlerin birine tanımlamak ve daha hızlı bir şekilde sorusunu denetlemesi yönelik sorular sorma kullanın:

|Bileşen/alan  | Tags |
|---------|---------|
| ADAL kitaplığı | [[adal]](https://stackoverflow.com/questions/tagged/adal) |
| MSAL kitaplığı     | [[msal]](https://stackoverflow.com/questions/tagged/msal) |
| OWIN ara yazılımı  | [[azure-active-directory]](https://stackoverflow.com/questions/tagged/azure-active-directory) |
| [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | [[azure ad b2b]](https://stackoverflow.com/questions/tagged/azure-ad-b2b) |
| [Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  | [[azure-ad-b2c]](https://stackoverflow.com/questions/tagged/azure-ad-b2c) |
| [Microsoft Graph API'si](https://developer.microsoft.com/graph/) | [[microsoft graph]](https://stackoverflow.com/questions/tagged/microsoft-graph) |
| Herhangi bir alan için kimlik doğrulama veya yetkilendirme ilgili konular | [[azure-active-directory]](https://stackoverflow.com/questions/tagged/azure-active-directory) |

Stack Overflow aşağıdaki gönderilerinden soru sorma ve kaynak kodu ekleme ipuçları içerir. Topluluk üyelerinin değerlendirmek ve sorularınıza hızla yanıt olasılığını artırmak için aşağıdaki yönergeleri izleyin:

* [İyi bir soru nasıl miyim](https://stackoverflow.com/help/how-to-ask)
* [En az, tam ve doğrulanabilir bir örnek oluşturma](https://stackoverflow.com/help/mcve)

## <a name="create-a-github-issue"></a>Bir GitHub sorunu oluşturun

Bir hata veya bizim kitaplıklara ilgili bir sorun bulursanız, bir sorun GitHub depolarımızdaki yükseltin. Bizim kitaplıkları açık kaynak olduğundan, ayrıca bir çekme isteği gönderebilirsiniz.

Kitaplıklar ve kendi GitHub depoları listesini görmek için aşağıdaki makalelere bakın:

* [ADAL](active-directory-authentication-libraries.md) kitaplıkları ve GitHub depoları
* [MSAL](reference-v2-libraries.md) kitaplıkları ve GitHub depoları

## <a name="open-a-support-request"></a>Bir destek isteği açın

Birine konuşmak gerekirse, bir destek isteği açabilirsiniz. Bir Azure müşterisiyseniz, kullanılabilir çeşitli destek seçenekleri. Planları karşılaştırmak için bkz [bu sayfayı](https://azure.microsoft.com/support/plans/). Geliştirici Desteği, Azure müşterileri için de kullanılabilir. Geliştirici destek planlarını satın alma hakkında daha fazla bilgi için bkz: [bu sayfayı](https://azure.microsoft.com/support/plans/developer/).

* Bir Azure destek planı, zaten varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

* Bir Azure müşterisi değilseniz, ayrıca bir destek isteği Microsoft ile açabilirsiniz [ticari desteğimiz](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial).

Ayrıca deneyebileceğiniz bir [sanal aracı](https://support.microsoft.com/contactus/?ws=support) destek almak veya soru sormak için.

### <a name="free-chat-support-for-a-limited-time"></a>Sınırlı bir süreliğine ücretsiz sohbet desteği

Sınırlı bir süre için ücretsiz Microsoft Partners için Sohbet desteğimiz de kullanabilirsiniz. Şirketiniz Microsoft Partner değilse, ücretsiz kaydedebilir ve giderek diğer avantajları elde [burada](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx).

Şirketiniz kaydettikten sonra sohbet istek başlayabilirsiniz [burada](https://aka.ms/devchat).
