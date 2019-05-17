---
title: Uygulama ve Azure Active Directory'de Hizmet sorumlusu nesneleri
description: Uygulama ve Azure Active Directory'de Hizmet sorumlusu nesneleri arasındaki ilişki hakkında bilgi edinin.
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
services: active-directory
editor: ''
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47626cbd8d5b8a5ddf7cc5c6ea42fb9453e5bfb2
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540679"
---
# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Uygulama ve Azure Active Directory'de Hizmet sorumlusu nesneleri

Bazı durumlarda, anlamı "uygulama" terimi, Azure Active Directory (Azure AD) bağlamında kullanıldığında yanlış anlaşılabilir. Bu makalede Azure AD uygulaması tümleştirme, kavramsal ve somut yönleri kayıt ve onaylarının bir gösterimi ile NET bir [çok kiracılı uygulama](developer-glossary.md#multi-tenant-application).

## <a name="overview"></a>Genel Bakış

Azure AD ile tümleştirilmiş bir uygulama, yazılım en boy gidin etkilere sahiptir. "Uygulama", yalnızca uygulama yazılımı, ancak Ayrıca kendi Azure AD kaydı ve kimlik doğrulama/yetkilendirme çalışma zamanında "konuşmaları" rolünde başvuran kavramsal bir terim sıklıkla kullanılır.

Tanımı gereği, uygulamanın bu rollere çalışabilir:

- [İstemci](developer-glossary.md#client-application) rol (kaynak kullanma)
- [Kaynak sunucuda](developer-glossary.md#resource-server) rol (istemciler için API'leri kullanıma sunma)
- Hem istemci rolü hem de kaynak sunucu rolü

Bir [OAuth 2.0 yetkilendirme verme akışı](developer-glossary.md#authorization-grant) erişim/kaynak verileri sırasıyla korumak istemci/kaynak sağlayan konuşma protokolü tanımlar.

Aşağıdaki bölümlerde, Azure AD uygulama modeli tasarım zamanı ve çalışma zamanı bir uygulamayı nasıl temsil ettiğini görürsünüz.

## <a name="application-registration"></a>Uygulama kaydı

Bir Azure AD uygulaması içinde kaydettiğinizde [Azure portalında][AZURE-Portal], iki nesne, Azure AD kiracınız oluşturulur:

- Bir uygulama nesnesi ve
- Hizmet sorumlusu nesnesi

### <a name="application-object"></a>Uygulama nesnesi

Azure AD uygulaması, bir Azure AD kiracısında uygulama kaydedildiği bulunduğu uygulama nesnesi ile uygulamanın "ana" Kiracı bilinen tanımlanır. Microsoft Graph [uygulama varlığı] [ MS-Graph-App-Entity] uygulama nesnenin özellikleri için bir şema tanımlar.

### <a name="service-principal-object"></a>Hizmet sorumlusu nesnesi

Azure AD kiracısı tarafından korunan kaynaklara erişmek için bir güvenlik sorumlusu tarafından erişim gerektiren varlık gösterilmelidir. Bu, kullanıcılara (kullanıcı sorumlusu) ve (hizmet sorumlusu) uygulamaları için geçerlidir.

Kullanıcı/uygulama için izinler ve erişim ilkesi, güvenlik sorumlusu Azure AD kiracısında tanımlar. Bu, kullanıcı/uygulama kimlik doğrulaması sırasında oturum açma ve yetkilendirme sırasında kaynak erişimi gibi temel özellikleri sağlar.

Ne zaman bir uygulama verildiğinde kaynaklara erişim izni bir kiracıda (kaydı veya [onay](developer-glossary.md#consent)), hizmet sorumlusu nesnesi oluşturulur. Microsoft Graph [ServicePrincipal varlık] [ MS-Graph-Sp-Entity] şema için bir hizmet sorumlusu nesnesinin özelliklerini tanımlar.

### <a name="application-and-service-principal-relationship"></a>Uygulama ve hizmet sorumlusu ilişkisi

Uygulama nesnesi olarak göz önünde bulundurun *genel* tüm kiracılar ve hizmet sorumlusu olarak kullanmak için uygulamanızı gösterimini *yerel* belirli kiracısında kullanım için temsili.

Varsayılan özellikleri ve hangi ortak bir şablondan uygulama nesnesi gören olan *türetilmiş* karşılık gelen hizmet sorumlusu nesneleri oluştururken kullanmak için. Uygulama nesnesi, bu nedenle, karşılık gelen hizmet sorumlusu nesneleri 1:many ilişkilerle yanı sıra yazılım uygulamayla 1:1 ilişki vardır.

Bir hizmet sorumlusu uygulama kullanıldığı, her kiracıya oluşturulmalıdır, oturum açma ve/veya Kiracı tarafından korunan kaynaklara erişim için bir kimlik oluşturmak etkinleştirme. Tek kiracılı bir uygulama (kiracısındaki kendi giriş), oluşturduğunuz ve uygulama kaydı sırasında kullanım için onaylı yalnızca bir hizmet sorumlusu sahiptir. Çok kiracılı Web uygulaması/API'si de burada bu kiracıda bir kullanıcı kendi kullanımı için onay vermiş her kiracıda oluşturulan hizmet sorumlusu vardır.

> [!NOTE]
> Uygulama nesnenizin yaptığınız tüm değişiklikler de yansıtılır uygulamanın giriş kiracısında yalnızca (Kiracı, kayıtlı), hizmet sorumlusu nesnesi. Erişim aracılığıyla kaldırılana kadar çok kiracılı uygulamalar için uygulama nesnesi değişiklikleri içinde hiçbir tüketici Kiracı hizmet sorumlusu nesneleri, yansıtılmaz [uygulama erişim panelinde](https://myapps.microsoft.com) ve yeniden verilir.
>
> De yerel uygulamalar varsayılan çok kiracılı kayıtlı olduklarını unutmayın.

## <a name="example"></a>Örnek

Aşağıdaki diyagramda bir uygulamanın uygulama nesnesini ve karşılık gelen hizmet sorumlusu nesneleri örnek çok kiracılı bir uygulama bağlamında adlı arasındaki ilişkiyi gösterir **ik uygulaması**. Bu örnek senaryoda üç Azure AD kiracıları vardır:

- **Adatum** -geliştirilen şirket tarafından kullanılan Kiracı **ik uygulaması**
- **Contoso** -Contoso kuruluş tarafından kullanılan Kiracı tüketicisi olduğu **ik uygulaması**
- **Fabrikam** -ayrıca tüketir Fabrikam kuruluş tarafından kullanılan Kiracı **ik uygulaması**

![Uygulama nesnesi ve bir hizmet sorumlusu nesnesi arasındaki ilişki](./media/app-objects-and-service-principals/application-objects-relationship.svg)

Bu örnek senaryoda:

| Adım | Açıklama |
|------|-------------|
| 1    | Uygulamanın giriş kiracısında uygulama ve hizmet sorumlusu nesneleri oluşturma işlemidir. |
| 2    | Contoso ve Fabrikam Yöneticiler onay tamamladıktan sonra hizmet sorumlusu nesnesi şirketlerinin Azure AD kiracısında oluşturulur ve yönetici verilen izinleri atanmış. Ayrıca, ik uygulaması yapılandırılmış/izin verecek kullanıcıların bireysel kullanım için tasarlanmış emin unutmayın. |
| 3    | İK uygulaması (Contoso ve Fabrikam) her tüketici kiracılar kendi hizmet sorumlusu nesnesi vardır. Kullanımları zamanında izinler tarafından yönetilen uygulama örneği tarafından onaylanan her temsil eder ilgili yönetici tarafından. |

## <a name="next-steps"></a>Sonraki adımlar

- Kullanabileceğiniz [Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer) hem uygulama hem de hizmet sorumlusu nesneleri sorgulamak için.
- Microsoft Graph API'sini kullanarak bir uygulamanın uygulama nesnesini erişebileceğiniz [Azure portal'ın] [ AZURE-Portal] uygulama bildirim düzenleyicisini veya [Azure AD PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0) , OData tarafından temsil edilen [uygulama varlığı][MS-Graph-App-Entity].
- Microsoft Graph API ile bir uygulamanın hizmet sorumlusu nesnesi erişebilir veya [Azure AD PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), OData tarafından temsil edilen [ServicePrincipal varlık] [ MS-Graph-Sp-Entity].

<!--Image references-->

<!--Reference style links -->
[MS-Graph-App-Entity]: https://docs.microsoft.com/graph/api/resources/application
[MS-Graph-Sp-Entity]: https://docs.microsoft.com/graph/api/resources/serviceprincipal
[AZURE-Portal]: https://portal.azure.com
