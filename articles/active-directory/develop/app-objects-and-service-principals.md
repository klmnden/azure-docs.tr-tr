---
title: Azure Active Directory uygulaması ve hizmet sorumlusu nesneleri
description: Uygulama ve Azure Active Directory'de Hizmet sorumlusu nesneleri arasındaki ilişki hakkında ayrıntılı bilgi
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
services: active-directory
editor: ''
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/19/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: 057465567217cff080b189bcdabee3042f41468d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595884"
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Uygulama ve Azure Active Directory'de (Azure AD) hizmet sorumlusu nesneleri
Bazen anlamı "uygulama" terimi, Azure AD bağlamında kullanıldığında yanlış anlaşılabilir. Azure AD uygulaması tümleştirme, kavramsal ve somut yönleri kayıt bir gösterimi ile açıklamak ve için onayı için bu makalenin hedefi olan bir [çok kiracılı uygulama](developer-glossary.md#multi-tenant-application).

## <a name="overview"></a>Genel Bakış
Azure AD ile tümleştirilmiş bir uygulama, yazılım en boy gidin etkilere sahiptir. "Uygulama", yalnızca uygulama yazılımı, ancak Ayrıca kendi Azure AD kaydı ve kimlik doğrulama/yetkilendirme çalışma zamanında "konuşmaları" rolünde başvuran kavramsal bir terim sıklıkla kullanılır. Uygulama tanımı tarafından işlevi bir [istemci](developer-glossary.md#client-application) (bir kaynak tüketen), rol bir [kaynak sunucusu](developer-glossary.md#resource-server) rol (istemcilere ifşa edildi. API) ya da her ikisini bile. Konuşma protokolü tarafından tanımlanan bir [OAuth 2.0 yetkilendirme verme akışı](developer-glossary.md#authorization-grant), erişim/kaynak verileri sırasıyla korumak istemci/kaynak sağlar. Şimdi daha ayrıntılı bir düzeyde dönelim ve Azure AD uygulama modeli tasarım zamanı ve çalışma zamanı bir uygulamayı nasıl temsil eder bakın. 

## <a name="application-registration"></a>Uygulama kaydı
Bir Azure AD uygulaması içinde kaydettiğinizde [Azure portalında][AZURE-Portal], iki nesne, Azure AD kiracınız oluşturulur: uygulama nesnesi ve bir hizmet sorumlusu nesnesi.

#### <a name="application-object"></a>Uygulama nesnesi
Azure AD uygulaması, bir Azure AD kiracısında uygulama kaydedildiği bulunduğu uygulama nesnesi ile uygulamanın "ana" Kiracı bilinen tanımlanır. Azure AD Graph [uygulama varlığı] [ AAD-Graph-App-Entity] uygulama nesnenin özellikleri için bir şema tanımlar. 

#### <a name="service-principal-object"></a>Hizmet sorumlusu nesnesi
Azure AD kiracısı tarafından korunan kaynaklara erişmek için bir güvenlik sorumlusu tarafından erişim gerektiren varlık gösterilmelidir. Bu, kullanıcılara (kullanıcı sorumlusu) ve (hizmet sorumlusu) uygulamaları için geçerlidir. Güvenlik sorumlusu erişim ilkesini ve kullanıcı/uygulama izinlerini bu kiracıda tanımlar. Bu, kullanıcı/uygulama kimlik doğrulaması sırasında oturum açma ve yetkilendirme sırasında kaynak erişimi gibi temel özellikleri sağlar.

Ne zaman bir uygulama verildiğinde kaynaklara erişim izni bir kiracıda (kaydı veya [onay](developer-glossary.md#consent)), hizmet sorumlusu nesnesi oluşturulur. Azure AD Graph [ServicePrincipal varlık] [ AAD-Graph-Sp-Entity] şema için bir hizmet sorumlusu nesnesinin özelliklerini tanımlar. 

#### <a name="application-and-service-principal-relationship"></a>Uygulama ve hizmet sorumlusu ilişkisi
Uygulama nesnesi olarak göz önünde bulundurun *genel* tüm kiracılar ve hizmet sorumlusu olarak kullanmak için uygulamanızı gösterimini *yerel* belirli kiracısında kullanım için temsili. Varsayılan özellikleri ve hangi ortak bir şablondan uygulama nesnesi gören olan *türetilmiş* karşılık gelen hizmet sorumlusu nesneleri oluştururken kullanmak için. Uygulama nesnesi, bu nedenle, karşılık gelen hizmet sorumlusu nesneleri 1:many ilişkilerle yanı sıra yazılım uygulamayla 1:1 ilişki vardır.

Bir hizmet sorumlusu uygulama kullanıldığı, her kiracıya oluşturulmalıdır, oturum açma ve/veya Kiracı tarafından korunan kaynaklara erişim için bir kimlik oluşturmak etkinleştirme. Tek kiracılı bir uygulama (kiracısındaki kendi giriş), oluşturduğunuz ve uygulama kaydı sırasında kullanım için onaylı yalnızca bir hizmet sorumlusu sahiptir. Çok kiracılı Web uygulaması/API'si de burada bu kiracıda bir kullanıcı kendi kullanımı için onay vermiş her kiracıda oluşturulan hizmet sorumlusu vardır. 

> [!NOTE]
> Uygulama nesnenizin yaptığınız tüm değişiklikler de yansıtılır uygulamanın giriş kiracısında yalnızca (Kiracı, kayıtlı), hizmet sorumlusu nesnesi. Erişim aracılığıyla kaldırılana kadar çok kiracılı uygulamalar için uygulama nesnesi değişiklikleri içinde hiçbir tüketici Kiracı hizmet sorumlusu nesneleri, yansıtılmaz [uygulama erişim panelinde](https://myapps.microsoft.com) ve yeniden verilir.
><br>  
> De yerel uygulamalar varsayılan çok kiracılı kayıtlı olduklarını unutmayın.
> 
> 

## <a name="example"></a>Örnek
Aşağıdaki diyagramda bir uygulamanın uygulama nesnesini ve karşılık gelen hizmet sorumlusu nesneleri örnek çok kiracılı bir uygulama bağlamında adlı arasındaki ilişkiyi gösterir **ik uygulaması**. Bu senaryoda üç Azure AD kiracıları vardır: 

* **Adatum** -geliştirilen şirket tarafından kullanılan Kiracı **ik uygulaması**
* **Contoso** -Contoso kuruluş tarafından kullanılan Kiracı tüketicisi olduğu **ik uygulaması**
* **Fabrikam** -ayrıca tüketir Fabrikam kuruluş tarafından kullanılan Kiracı **ik uygulaması**

![Uygulama nesnesi ve bir hizmet sorumlusu nesnesi arasındaki ilişki](./media/app-objects-and-service-principals/application-objects-relationship.png)

Önceki diyagramda, 1. adım uygulama ve hizmet sorumlusu nesneleri uygulamanın giriş kiracısında oluşturma işlemidir.

Adım 2'de, onay, Contoso ve Fabrikam Yöneticiler tamamladığınızda, bir hizmet sorumlusu nesnesi şirketlerinin Azure AD kiracısında oluşturulur ve yönetici verilen izinleri atanmış. Ayrıca, ik uygulaması yapılandırılmış/izin verecek kullanıcıların bireysel kullanım için tasarlanmış emin unutmayın.

Adım 3'te ik uygulaması (Contoso ve Fabrikam) her tüketici kiracılar kendi hizmet sorumlusu nesnesi var. Kullanımları zamanında izinler tarafından yönetilen uygulama örneği tarafından onaylanan her temsil eder ilgili yönetici tarafından.

## <a name="next-steps"></a>Sonraki adımlar
Bir uygulamanın uygulama nesnesini Azure AD Graph API aracılığıyla erişilebilen [Azure portal'ın] [ AZURE-Portal] uygulama bildirim düzenleyicisini veya [Azure AD PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), olarak OData tarafından temsil edilen [uygulama varlığı][AAD-Graph-App-Entity].

Bir uygulamanın hizmet sorumlusu nesnesi, Azure AD Graph API aracılığıyla erişilebilen veya [Azure AD PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), OData tarafından temsil edilen [ServicePrincipal varlık] [ AAD-Graph-Sp-Entity].

[Azure AD Graph Gezgini](https://graphexplorer.azurewebsites.net/) hem uygulama hem de hizmet sorumlusu nesneleri sorgulama için kullanışlıdır.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
