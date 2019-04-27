---
title: Güvenlik işlemleri Microsoft Graph Security - Azure Logic Apps ile tümleştirme
description: Microsoft Graph güvenlik ve Azure Logic Apps ile güvenlik işlemlerini yöneterek uygulamanızın tehdit koruması, algılama ve yanıt özellikleri geliştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: preetikr
ms.author: preetikr
ms.reviewer: klam, estfan, LADocs
ms.topic: article
ms.date: 01/30/2019
tags: connectors
ms.openlocfilehash: 24963a35bc3e54b2d140bf4ed1d169b213bd9b2a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60448062"
---
# <a name="improve-threat-protection-by-integrating-security-operations-with-microsoft-graph-security--azure-logic-apps"></a>Tehdit koruması güvenlik işlemleri Microsoft Graph güvenlik ve Azure Logic Apps ile tümleştirerek geliştirin

İle [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [Microsoft Graph güvenlik](https://docs.microsoft.com/graph/security-concept-overview) Bağlayıcısı, uygulamanızı nasıl algıladığı artırabilir, korur ve tehditleri Microsoft tümleştirme için otomatik iş akışları oluşturarak yanıt verir. güvenlik ürünleri, hizmetleri ve iş ortakları. Örneğin, oluşturabileceğiniz [Azure Güvenlik Merkezi playbook'ları](../security-center/security-center-playbooks.md) , izleme ve uyarılar gibi Microsoft Graph güvenlik varlıkları yönetme. Microsoft Graph güvenlik bağlayıcı tarafından desteklenen bazı senaryolar aşağıda verilmiştir:

* Sorguları veya uyarı kimliğine göre temel uyarılar alın Örneğin, yüksek önem düzeyindeki uyarılar içeren bir listesini alabilirsiniz.
* Uyarılar güncelleştirin. Örneğin, atama, uyarılar için açıklama ekleme veya uyarıları etiket uyarı güncelleştirebilirsiniz.
* İzleme, uyarılar oluşturulduğunda veya oluşturarak değiştirilen [uyarı abonelikleri (Web kancaları)](https://docs.microsoft.com/graph/api/resources/webhooks).
* Uyarı aboneliklerinizi yönetin. Örneğin, etkin abonelikleri Al, bir abonelik için kullanım süresi sonu genişletmek veya abonelikleri silin.

Mantıksal uygulamanızın iş akışı, Microsoft Graph güvenlik bağlayıcısından yanıtları alın ve bu çıktı, iş akışınızı diğer eylemler için kullanılabilir hale eylemlerini kullanabilirsiniz. Diğer Eylemler, iş akışı Kullanımdaki Microsoft Graph güvenlik bağlayıcı eylemleri çıktısı da olabilir. Microsoft Graph Security Bağlayıcısı yüksek öneme sahip uyarılar alın, örneğin, bu Uyarıları e-posta iletisinde Outlook Bağlayıcısı'nı kullanarak gönderebilirsiniz. 

Microsoft Graph güvenlik hakkında daha fazla bilgi için bkz: [Microsoft Graph güvenlik API'sine genel bakış](https://aka.ms/graphsecuritydocs). Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md). Microsoft Flow veya PowerApps için arıyorsanız bkz [Flow nedir?](https://flow.microsoft.com/) veya [PowerApps nedir?](https://powerapps.microsoft.com/)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Microsoft Graph güvenlik bağlayıcıyı kullanmak için olmalıdır *açıkça belirtilen* parçası olan Azure Active Directory (AD) Kiracı yönetici onayı, [Microsoft Graph güvenlik kimlik doğrulama gereksinimleri ](https://aka.ms/graphsecurityauth). Microsoft Graph güvenlik bağlayıcı'nın uygulama kimliği ve ayrıca bulabileceğiniz adı, bu onayı gerektirir [Azure portalında](https://portal.azure.com):

   | Özellik | Değer |
   |----------|-------|
   | **Uygulama adı** | `MicrosoftGraphSecurityConnector` |
   | **Uygulama Kimliği** | `c4829704-0edc-4c3d-a347-7c4a67586f3c` |
   |||

   Bağlayıcı için izin vermek için Azure AD Kiracı yöneticinizle ya da şu adımları izleyin:

   * [Azure AD uygulamaları için Kiracı yönetici izni verme](../active-directory/develop/v2-permissions-and-consent.md).

   * Mantıksal uygulamanızın ilk çalıştırma sırasında Azure AD Kiracı yöneticiniz gelen, onay uygulamanızı isteyebilir [uygulama onay deneyiminde](../active-directory/develop/application-consent-experience.md).
   
* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Uyarıları gibi Microsoft Graph güvenlik varlıklarınızı erişmek istediğiniz mantıksal uygulaması. Şu anda bu bağlayıcı, tetikleyici vardır. Bu nedenle, Microsoft Graph güvenlik eylemi kullanmak için mantıksal uygulamanızın bir tetikleyici ile örneğin başlatın, **yinelenme** tetikleyici.

## <a name="connect-to-microsoft-graph-security"></a>Microsoft Graph Security'ye bağlama 

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com/)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için tetikleyici ve Microsoft Graph güvenlik eylemi eklemeden önce istediğiniz herhangi bir eylem ekleyin.

   -veya-

   Var olan mantıksal uygulamalar, son adım, bir Microsoft Graph güvenlik eylem eklemek istediğiniz altında seçin için **yeni adım**.

   -veya-

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Görünür ve seçin, artı (+) seçin **Eylem Ekle**.

1. Arama kutusuna filtreniz olarak "microsoft graph güvenliği" girin. Eylem listesinden istediğiniz eylemi seçin.

1. Microsoft Graph güvenlik kimlik bilgilerinizle oturum açın.

1. Seçili eyleminiz için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="add-actions"></a>Eylem ekle

Kullanılabilir eylemler Microsoft Graph güvenlik Bağlayıcısı ile kullanma hakkında daha özel ayrıntılar aşağıda verilmiştir.

### <a name="manage-alerts"></a>Uyarıları yönetme

Filtre uygulamak için sıralama, veya en son sonuçlar elde edin, sağlayın *yalnızca* [Microsoft Graph tarafından desteklenen ODATA sorgu parametrelerini](https://docs.microsoft.com/graph/query-parameters). *Belirtmeyin* tam temel URL'yi veya örneğin, HTTP eylemi `https://graph.microsoft.com/v1.0/security/alerts`, veya `GET` veya `PATCH` işlemi. Parametrelerini gösteren belirli bir örnek bir **uyarıları alma** yüksek önem düzeyindeki uyarılar listesiyle istediğinizde eylem:

`Filter alerts value as Severity eq 'high'`

Bu bağlayıcı ile kullanabileceğiniz sorguları hakkında daha fazla bilgi için bkz. [Microsoft Graph güvenlik uyarıları başvuru belgeleri](https://docs.microsoft.com/graph/api/alert-list). Bu bağlayıcı ile Gelişmiş deneyimler oluşturmak üzere daha fazla bilgi edinin [şema özellikleri uyarılar](https://docs.microsoft.com/graph/api/resources/alert) , bir bağlayıcıyı destekler.

| Eylem | Açıklama |
|--------|-------------|
| **Uyarılar alın** | Get göre filtrelenmiş bir veya daha fazla uyarı [uyarı özellikleri](https://docs.microsoft.com/graph/api/resources/alert), örneğin: <p>`Provider eq 'Azure Security Center' or 'Palo Alto Networks'` | 
| **Uyarı Kimliğine göre Al** | Uyarı Kimliği temel alınarak belirli bir uyarı alma | 
| **Güncelleştirme uyarısı** | Uyarı Kimliği temel alınarak belirli bir uyarı güncelleştirme <p>Gerekli ve düzenlenebilir özellikler İsteğinizde geçirdiğinizden emin olmak için bkz. [uyarılar için düzenlenebilir özellikler](https://docs.microsoft.com/graph/api/alert-update). Örneğin, bunlar araştırmak için bir güvenlik analisti bir uyarı atamak için uyarının güncelleştirebilirsiniz **atanan** özelliği. |
|||

### <a name="manage-alert-subscriptions"></a>Uyarı abonelikleri yönetme

Microsoft Graph'ı destekleyen [ *abonelikleri*](https://docs.microsoft.com/graph/api/resources/subscription), veya [ *Web kancaları*](https://docs.microsoft.com/graph/api/resources/webhooks). Almak için güncelleştirme veya abonelikleri silin, sağlayın [Microsoft Graph tarafından desteklenen ODATA sorgu parametrelerini](https://docs.microsoft.com/graph/query-parameters) Microsoft Graph varlık oluşturun ve dahil `security/alerts` ODATA sorgu tarafından izlenen. 
*İçerme* temel URL'si, örneğin, `https://graph.microsoft.com/v1.0`. Bunun yerine, bu örnekte biçimi kullanın:

`security/alerts?$filter=status eq 'New'`

| Eylem | Açıklama |
|--------|-------------|
| **Abonelik oluşturma** | [Abonelik oluşturma](https://docs.microsoft.com/graph/api/subscription-post-subscriptions) , herhangi bir değişiklik hakkında sizi bilgilendirir. Bu aboneliği istediğiniz belirli uyarı türleri için filtre uygulayabilirsiniz. Örneğin, yüksek öneme sahip uyarılar hakkında uyaran bir aboneliği oluşturabilirsiniz. |
| **Etkin abonelikleri Al** | [Süresi dolmamış abonelikleri Al](https://docs.microsoft.com/graph/api/subscription-list). | 
| **Aboneliği güncelleştirme** | [Bir aboneliği güncelleştirme](https://docs.microsoft.com/graph/api/subscription-update) abonelik kimliğini sağlayarak Örneğin, aboneliğinizi genişletmek için aboneliğin güncelleştirebilirsiniz `expirationDateTime` özelliği. | 
| **Aboneliği silme** | [Aboneliği silme](https://docs.microsoft.com/graph/api/subscription-delete) abonelik kimliğini sağlayarak | 
||| 

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](https://aka.ms/graphsecurityconnectorreference).

## <a name="get-support"></a>Destek alın

Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
