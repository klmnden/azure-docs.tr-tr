---
title: Azure Active Directory uygulaması ve hizmet sorumlusu nesneleri
description: Tartışma için uygulama ve Azure Active Directory hizmet asıl nesneleri arasındaki ilişki
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
ms.openlocfilehash: e8e693355fb9b30e1a69b49f20d5044c531e2fcd
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Uygulama ve hizmet asıl nesneler Azure Active Directory'de (Azure AD)
Bazen "uygulama" terimi anlamı Azure AD bağlamında kullanıldığında yanlış. Bu makalede Azure AD uygulama tümleştirmesi, kavramsal ve somut yönlerini kayıt bir çizimi ile açıklamak ve için onay hedefidir bir [çok kiracılı uygulama](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Genel Bakış
Azure AD ile tümleşik bir uygulama, yazılım en boy gidin etkilere sahiptir. "Uygulama", yalnızca uygulama yazılımı, ancak Ayrıca kendi Azure AD kaydını ve kimlik doğrulama/yetkilendirme çalışma zamanında "görüşmeleri" rolünde başvurma kavramsal bir terim sık kullanılır. Tanımı gereği, bir uygulama içinde çalışabilmesi için bir [istemci](active-directory-dev-glossary.md#client-application) (kaynak kullanma), rol bir [kaynak sunucusu](active-directory-dev-glossary.md#resource-server) rol (istemcilere sunan API) veya her ikisi de bile. Konuşma protokolü tarafından tanımlanan bir [OAuth 2.0 yetkilendirme verme akış](active-directory-dev-glossary.md#authorization-grant), erişim/kaynağın verileri sırasıyla korumak istemci/kaynak izin verme. Şimdi daha derin bir düzeye edelim ve nasıl tasarım zamanı ve çalışma zamanında uygulama Azure AD uygulama modelini gösteren bakın. 

## <a name="application-registration"></a>Uygulama kaydı
Bir Azure AD uygulaması kaydettiğinizde [Azure portal][AZURE-Portal], iki nesne, Azure AD kiracınız oluşturulur: uygulama nesnesi ve bir hizmet sorumlusu nesnesi.

#### <a name="application-object"></a>Uygulama nesnesi
Azure AD uygulaması, bir ve Azure AD kiracısında uygulama kaydedildiği bulunduğu uygulama nesnesi tarafından uygulamanın "home" Kiracı olarak bilinen tanımlanır. Azure AD grafik [uygulama varlığı] [ AAD-Graph-App-Entity] uygulama nesnenin özellikleri için şema tanımlar. 

#### <a name="service-principal-object"></a>Hizmet sorumlusu nesnesi
Bir Azure AD Kiracı tarafından güvenliği sağlanan kaynaklara erişmek için bir güvenlik sorumlusu tarafından erişim gerektiren varlık gösterilmelidir. Bu, kullanıcıların (kullanıcı asıl) ve (hizmet sorumlusu) uygulamaları için geçerlidir. Güvenlik sorumlusu Kiracı içinde kullanıcı/uygulama için izinler ve erişim ilkesi tanımlar. Bu kullanıcı/uygulama kimlik doğrulaması sırasında oturum açma ve kimlik doğrulama sırasında kaynak erişimi gibi temel özellikleri sağlar.

Ne zaman bir uygulama verilen kaynaklara erişmesine izin Kiracı (kaydı veya [onayı](active-directory-dev-glossary.md#consent)), bir hizmet sorumlusu nesnesi oluşturulur. Azure AD grafik [ServicePrincipal varlık] [ AAD-Graph-Sp-Entity] bir hizmet asıl nesnesinin özellikleri için şema tanımlar. 

#### <a name="application-and-service-principal-relationship"></a>Uygulama ve hizmet asıl ilişkisi
Uygulama nesnesi olarak göz önünde bulundurun *genel* tüm kiracılar ve hizmet sorumlusu olarak kullanmak için uygulama gösterimini *yerel* belirli bir kiracı kullanımda gösterimi. Uygulama nesnesi gören hangi ortak şablondan ve varsayılan özellikleri olan *türetilmiş* karşılık gelen hizmet asıl nesneleri oluşturulurken kullanılacak. Uygulama nesnesi bu nedenle yazılım uygulamayla 1:1 ilişki ve onun karşılık gelen hizmet asıl nesneleri ile 1:many ilişkileri vardır.

Bir hizmet sorumlusu uygulama kullanıldığı, her bir kiracı içinde oluşturulmalıdır oturum açma ve/veya Kiracı tarafından güvenli hale getirilmiş kaynaklara erişim için bir kimlik oluşturmak üzere etkinleştirme. Tek kiracılı uygulama oluşturulur ve uygulama kaydı sırasında kullanılmak üzere rıza yalnızca bir hizmet sorumlusu (kendi giriş Kiracı), sahiptir. Çok kiracılı Web uygulaması/API bu kiracısındaki bir kullanıcı kendi kullanımı için burada seçtiği her bir kiracı oluşturulan bir hizmet sorumlusu de vardır. 

> [!NOTE]
> Uygulama nesnesi yaptığınız tüm değişiklikler de yansıtılır uygulamanın giriş Kiracı yalnızca (burada kaydedildiği Kiracı) kendi hizmet sorumlusu nesnesi. Erişim aracılığıyla kaldırılana kadar çok kiracılı uygulamalar için uygulama nesnesi değişiklikleri hiçbir tüketici Kiracı hizmet asıl nesneleri, yansıtılmaz [uygulama erişim Paneli'ne](https://myapps.microsoft.com) ve yeniden verilir.
><br>  
> Ayrıca, yerel uygulamalar varsayılan olarak çok Kiracı olarak kaydedilen unutmayın.
> 
> 

## <a name="example"></a>Örnek
Aşağıdaki diyagramda bir uygulamanın uygulama nesnesi ve karşılık gelen hizmet asıl nesneleri, örnek bir çok kiracılı uygulama bağlamında adlı arasındaki ilişki gösterilmektedir **ik uygulama**. Bu senaryoda üç Azure AD kiracılarıyla vardır: 

* **Adatum** -geliştirilen şirket tarafından kullanılan Kiracı **ik uygulama**
* **Contoso** -Contoso kuruluş tarafından kullanılan Kiracı tüketicisi olduğu **ik uygulama**
* **Fabrikam** -ayrıca tüketir Fabrikam kuruluş tarafından kullanılan Kiracı **ik uygulama**

![Uygulama nesnesi ve bir hizmet sorumlusu nesnesi arasındaki ilişki](./media/active-directory-application-objects/application-objects-relationship.png)

Önceki diyagramda 1. adım uygulama ve hizmet asıl nesneleri uygulamanın ana Kiracı oluşturma işlemidir.

2. adımda onay, Contoso ve Fabrikam Yöneticiler tamamladığınızda, bir hizmet sorumlusu nesnesi şirketlerinin Azure AD kiracınızda oluşturulur ve yönetici verilen izinler atanmış. Ayrıca, ik uygulama yapılandırılmış/onay izin vermek için tek başına kullanım için kullanıcılar tarafından tasarlanmalıdır emin unutmayın.

Adım 3'te uygulamasının HR (Contoso ve Fabrikam) her tüketici kiracılar kendi hizmet sorumlusu nesnesi sahip. İzinler tarafından yönetilen zamanında uygulama örneğini kullanımını rıza her temsil eder ilgili yönetici tarafından.

## <a name="next-steps"></a>Sonraki adımlar
Bir uygulamanın uygulama nesnesi Azure AD Graph API üzerinden erişilebilir [Azure portal'ın] [ AZURE-Portal] uygulama bildirim Düzenleyicisi veya [Azure AD PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), olarak OData tarafından temsil edilen [uygulama varlığı][AAD-Graph-App-Entity].

Bir uygulamanın hizmet sorumlusu nesnesi Azure AD Graph API üzerinden erişilebilir veya [Azure AD PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), OData tarafından temsil edilen [ServicePrincipal varlık] [ AAD-Graph-Sp-Entity].

[Azure AD Graph Explorer'a](https://graphexplorer.azurewebsites.net/) uygulama ve hizmet asıl nesneleri sorgulamak için yararlıdır.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
