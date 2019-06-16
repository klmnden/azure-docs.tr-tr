---
title: Azure Active Directory Graph API'si | Microsoft Docs
description: Azure AD'ye REST API uç noktaları yoluyla programlı erişim sağlayan Azure AD Graph API için bir genel bakış ve Hızlı Başlangıç Kılavuzu.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/01/2019
ms.author: ryanwi
ms.reviewer: dkershaw, sureshja
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77730ea7302b4abd6c17ebfe5620c0dc55fa407c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544579"
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API'si

> [!IMPORTANT]
>
> Şubat 2019 tarihinde Azure Active Directory Graph API'si yerine Microsoft Graph API önceki bazı sürümlerinde kullanımdan kaldırmaya sürecini başlattık. 
>
> Ayrıntılar, güncelleştirmeleri ve zaman çerçevelerine görmek [Microsoft Graph veya Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) Office geliştirici Merkezi'ndeki.
>
> İlerletme, uygulamaları Microsoft Graph API'si kullanmanız gerekir. 



Bu makale, Azure AD Graph API için geçerlidir. Microsoft Graph API için ilgili benzer bilgileri için bkz. [Microsoft Graph API'sini](https://docs.microsoft.com/graph/use-the-api). 

Azure Active Directory Graph API, Azure AD'ye REST API uç noktaları yoluyla programlı erişim sağlar. Uygulamalar Azure AD Graph API'si gerçekleştirmek için kullanabileceğiniz oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri dizin verileri ve nesneleri. Örneğin, Azure AD Graph API, bir kullanıcı nesnesi için şu ortak işlemleri destekler:

* Bir dizinde yeni bir kullanıcı oluşturma
* Kendi grupları gibi ayrıntılı bir kullanıcının özelliklerini alma
* Konumlarıyla ve telefon numarası gibi bir kullanıcının özelliklerini güncelleştirmek veya parolasını değiştirme
* Rol tabanlı erişim için kullanıcının grup üyeliğini denetleyin
* Bir kullanıcı hesabı devre dışı bırakın veya tamamen silin

Ayrıca, gruplar ve uygulamalar gibi diğer nesneler üzerinde benzer işlemleri gerçekleştirebilir. Bir dizine Azure AD Graph API'sini çağırmak için uygulamanızı Azure AD'ye kayıtlı olması gerekir. Uygulamanızı Azure AD Graph API'sine erişim verilmelidir. Bu erişim, normal bir kullanıcı veya yönetici onay akışı elde edilir.

Azure Active Directory Graph API'sini kullanmaya başlamak için bkz: [Azure AD Graph API'si Hızlı Başlangıç Kılavuzu](active-directory-graph-api-quickstart.md), veya görüntüleme [etkileşimli Azure AD Graph API başvuru belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Özellikler

Azure AD Graph API'sini aşağıdaki özellikleri sağlar:

* **REST API uç noktalarını**: Azure AD Graph API, standart HTTP isteklerini kullanarak erişilen uç oluşan bir RESTful hizmeti olduğu. Azure AD Graph API, istekleri ve yanıtları için XML veya Javascript nesne gösterimi (JSON) içerik türlerini destekler. Daha fazla bilgi için [Azure AD Graph REST API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Azure AD ile kimlik doğrulaması**: Azure AD Graph API'sine yapılan her isteği bir JSON Web Token (JWT) isteğin yetkilendirme üst bilgisinde ekleyerek kimlik doğrulaması gerekir. Bu belirteç, Azure AD'nin belirteç uç noktası için bir istekte ve geçerli kimlik bilgileri alınır. OAuth 2.0 istemci kimlik bilgileri akışı kullanabilir veya yetki kodu izin akışı Graph'i çağırmaya yönelik bir belirteç almak için. Daha fazla bilgi için [OAuth 2.0, Azure AD'de](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Rol tabanlı yetkilendirme (RBAC)** : Güvenlik grupları, Azure AD Graph API RBAC gerçekleştirmek için kullanılır. Örneğin, bir kullanıcı belirli bir kaynağa erişim izni olup olmadığını, uygulamayı çağırabilir belirlemek istiyorsanız [denetleyin (geçici) grup üyeliğini](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) işlemi, true veya false döndürür.
* **Fark sorgusu**: Fark sorgusu, Azure AD Graph API için sık kullanılan sorgular yapmak zorunda kalmadan iki dönemleri arasında bir dizinde değişiklik izlemenize imkan tanır. Bu istek türü yalnızca geçerli istek ve önceki fark sorgusu istek arasında yapılan değişiklikleri döndürür. Daha fazla bilgi için [Azure AD Graph API değişiklik sorgu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Dizin genişletmeleri**: Dış veri deposuna gerek kalmadan directory nesnelerine özel özellikler ekleyebilirsiniz. Örneğin, uygulamanızın her bir kullanıcı için bir Skype kimliği özelliği gerektiriyorsa, dizinde yeni özellik kaydedebilirsiniz ve her kullanıcı nesnesi üzerinde kullanılabilir. Daha fazla bilgi için [Azure AD Graph API'si directory şema uzantıları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **İzin kapsamları tarafından güvenliği sağlanan**: Azure AD Graph API'si izin kapsamları, OAuth 2.0 kullanarak Azure AD verilerine güvenli erişim sağlamak kullanıma sunar. Bunu çeşitli dahil olmak üzere istemci uygulama türlerini destekler:
  
  * Temsilcili erişim yetkilendirme aracılığıyla verilere (yönetici temsilcisi) oturum açmış kullanıcıdan verilen kullanıcı arabirimleri
  * var olan bir oturum açmış kullanıcı arka planda çalışır ve uygulama tarafından tanımlanan rol tabanlı erişim denetimi kullanan bir hizmetin/daemon uygulamalar
    
    Uygulama izinleri Azure AD Graph API'si tarafından kullanıma sunulan bir ayrıcalık temsil eder ve uygulama kayıt izinleri özellikleri aracılığıyla istemci uygulamalar tarafından istenen her ikisi de temsilci ve [Azure portalında](https://portal.azure.com). [Azure AD Graph API'si izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) ne istemci uygulamanız tarafından kullanılabilir bilgi sağlar.

## <a name="scenarios"></a>Senaryolar

Azure AD Graph API, birçok uygulama senaryolarını olanaklı kılar. En yaygın aşağıdaki senaryolar şunlardır:

* **İş kolu (tek Kiracılı) uygulaması**: Bu senaryoda, bir Office 365 aboneliğine sahip bir kuruluş için bir kurumsal Geliştirici çalışır. Geliştirici, bir kullanıcıya lisans atama gibi görevleri gerçekleştirmek için Azure AD ile etkileşime giren bir web uygulaması oluşturuyor. Bu görev Azure AD Graph API'sine erişim gerekiyorsa, geliştiricinin tek kiracılı uygulama Azure AD'de kaydeder ve yapılandırır, okuma ve yazma izinleri Azure AD Graph API'si için. Ardından uygulama, Azure AD Graph API'sini çağırmak için bir belirteç almak için kendi kimlik bilgilerini veya bu şu anda oturum açma kullanıcı kullanmak üzere yapılandırılmıştır.
* **Bir hizmet uygulaması (çok Kiracılı) olarak yazılım**: Bu senaryoda, bir bağımsız yazılım satıcısı (ISV) Azure AD kullanan başka kuruluşlar için kullanıcı yönetimi özellikleri sağlayan bir barındırılan çok kiracılı web uygulaması geliştiriyor. Uygulama Azure AD Graph API'sini çağırmak gereken şekilde bu özellikler, dizin nesnesi, erişim gerektirir. Geliştirici, Azure AD'de uygulama kaydeder, gerekli okuma ve yazma izinleri Azure AD Graph API'si için yapılandırır ve diğer kurumların kendi dizinde uygulamayı kullanmak için onay verebilir böylece ardından dış erişim sağlar. Başka bir kuruluştaki bir kullanıcı uygulamayı ilk kez doğruladığında, bunlar uygulamanın istediği izinleri içeren bir onay iletişim kutusu gösterilir. Onay sonra uygulama sunacak verme, bu kullanıcı dizini içinde Azure AD Graph API'si izin istedi. Onay çerçevesine hakkında daha fazla bilgi için bkz. [onay çerçevesine genel bakış](consent-framework.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory Graph API'sini kullanmaya başlamak için aşağıdaki konulara bakın:

* [Azure AD Graph API'si Hızlı Başlangıç Kılavuzu](active-directory-graph-api-quickstart.md)
* [Azure AD Graph REST belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
