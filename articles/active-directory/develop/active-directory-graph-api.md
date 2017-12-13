---
title: Azure Active Directory grafik API'si | Microsoft Docs
description: "Azure ad REST API uç noktaları yoluyla programlı erişim sağlayan grafik API'si için bir genel bakış ve Hızlı Başlangıç Kılavuzu."
services: active-directory
documentationcenter: 
author: viv-liu
manager: mtillman
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: 815b9f75864ba3a08f623af12417391fba430f7d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API'si
> [!IMPORTANT]
> Azure Active Directory kaynaklarına erişmek için Azure AD Graph API'si yerine [Microsoft Graph](https://graph.microsoft.io/) kullanmanız önemle tavsiye edilir. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Graph API’sinin hala uygun olabileceği senaryo sayısı çok sınırlıdır; daha fazla bilgi için Office Geliştirici Merkezi’ndeki [Microsoft Graph veya Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog gönderisine bakın.
> 
> 

Azure Active Directory grafik API'si, REST API uç noktaları aracılığıyla Azure AD için programlı erişim sağlar. Uygulamaları gerçekleştirmek için grafik API'sini kullanabilirsiniz oluşturma, okuma, güncelleştirme ve (CRUD) işlemleridir dizin verileri ve nesneleri silme. Örneğin, bir kullanıcı nesnesi için aşağıdaki ortak işlemleri grafik API'si destekler:

* Yeni bir kullanıcı bir dizin oluşturun
* Kendi gruplar gibi bir kullanıcının ayrıntılı özelliklerini alma
* Konum ve telefon numarası gibi bir kullanıcının özelliklerini güncelleştirmek veya parolasını değiştirme
* Rol tabanlı erişim için kullanıcının grup üyeliğini denetleyin
* Bir kullanıcı hesabı devre dışı bırakmak veya tamamen silme

Kullanıcı nesneleri ek olarak, gruplar ve uygulamalar gibi diğer nesneler üzerinde benzer işlemler gerçekleştirebilirsiniz. Bir dizin grafik API'sini çağırmak için uygulama Azure AD ile kayıtlı olması gerekir ve dizinine erişimi izin verecek şekilde yapılandırılmalıdır. Bu, normal bir kullanıcı veya yönetici onayı akışını sağlanır.

Azure Active Directory grafik API'sini kullanmaya başlamak için bkz: [grafik API'si Hızlı Başlangıç Kılavuzu](active-directory-graph-api-quickstart.md), veya görüntülemek [etkileşimli grafik API başvuru belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Özellikler
Grafik API'si aşağıdaki özellikleri sağlar:

* **REST API uç noktaları**: grafik API'si standart HTTP isteklerini kullanarak erişilen uç noktaları oluşan bir RESTful hizmettir. Grafik API'si istekleri ve yanıtları için XML veya Javascript nesne gösterimi (JSON) içerik türlerini destekler. Daha fazla bilgi için bkz: [Azure AD Graph REST API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Azure AD ile kimlik doğrulaması**: bir JSON Web Token (JWT) isteğinin yetkilendirme üst ekleyerek grafik API'si için her isteğin kimliğinin doğrulanması gerekir. Bu belirteç, Azure AD belirteç uç noktası için istekte ve geçerli kimlik bilgilerini sağlayan alınır. OAuth 2.0 istemci kimlik bilgileri akışını kullanabilir veya yetki kodu izin akışı grafiği çağırmak üzere bir belirteç almak üzere. Daha fazla bilgi için [OAuth 2.0 Azure AD'de](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Rol tabanlı yetkilendirme (RBAC)**: güvenlik grupları, grafik API'sini RBAC gerçekleştirmek için kullanılır. Örneğin, bir kullanıcının belirli bir kaynağa erişim izni olup olmadığını, uygulamayı çağırabilir belirlemek istiyorsanız [grup üyeliğini denetleyin (Geçişli)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) true veya false döndürür işlemi.
* **Fark sorgu**: grafik API'sine sık sorguları yapmak zorunda kalmadan iki dönemleri arasında bir dizinde değişiklik denetlemek istiyorsanız, bir fark sorgu isteği yapabilirsiniz. Bu istek türü yalnızca önceki fark sorgu isteği ve geçerli istek arasında yapılan değişiklikler döndürür. Daha fazla bilgi için bkz: [Azure AD Graph API fark sorgu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Dizin genişletmeleri**: Okuma veya yazma dizin nesneleri için benzersiz özellikleri gerektiren bir uygulama geliştiriyorsanız, kaydetme ve grafik API'sini kullanarak uzantısı değerleri kullanın. Örneğin, uygulamanızın her kullanıcı için Skype ID özelliği gerektiriyorsa, dizinde yeni özellik kaydedebilirsiniz ve her kullanıcı nesnesi üzerinde kullanılabilir. Daha fazla bilgi için bkz: [Azure AD Graph API Directory şema uzantıları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **İzin kapsamları tarafından güvenliği sağlanan**: AAD grafik API'si AAD verilerine erişimi güvenli ve rıza etkinleştirmek ve istemci uygulama türleri dahil olmak üzere, çeşitli destek izin kapsamları gösterir:
  
  * verilen bir kullanıcı arabirimi olanlar erişim yetkilendirme aracılığıyla verilere (temsilci) oturum açmış kullanıcıdan temsilci
  * kullananlar uygulama-rol tabanlı erişim denetimi hizmeti/arka plan programı istemcileri (uygulama rolleri) gibi tanımlama
    
    Her ikisi de devredildi ve uygulama rolü izin kapsamları grafik API'si tarafından kullanıma sunulan ayrıcalık temsil eder ve uygulama kayıt izinleri üzerinden istemci uygulamaları tarafından istenen [Azure portalında özellikleri](https://portal.azure.com). İstemcileri izin kapsamları verilmiş kendilerine izinlere temsilci için erişim belirtecini alınan kapsam ("scp") talep inceleyerek doğrulayabilirsiniz ve rolleri ("rol") için uygulama rolü izinleri isteyin. Daha fazla bilgi edinmek [Azure AD grafik API'si izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).

## <a name="scenarios"></a>Senaryolar
Grafik API'si birçok uygulama senaryolara olanak sağlar. Aşağıdaki senaryolar en yaygın şunlardır:

* **(Tek Kiracılı) iş hattı uygulaması**: Bu senaryoda, bir Office 365 aboneliğine sahip bir kuruluş için bir kuruluş Geliştirici çalışır. Geliştirici, böyle bir kullanıcıya lisans atanmasını görevleri gerçekleştirmek için Azure AD ile etkileşime giren bir web uygulaması oluşturuyor. Tek Azure AD'de uygulama Kiracı ve yapılandırır Geliştirici yazmaçlar okuma ve yazma izinlerine grafik API'si için bu görev grafik API'sine erişim gerektirir. Sonra uygulama kendi kimlik bilgilerini veya şu anda oturum açma kullanıcı içeriğiyle grafik API'sini çağırmak için bir belirteç almak için kullanmak üzere yapılandırılır.
* **Bir hizmet uygulaması (çok Kiracılı) olarak yazılım**: Bu senaryoda, bağımsız yazılım satıcısı (ISV), Azure AD kullanan başka kuruluşlar için kullanıcı yönetimi özellikleri sağlar barındırılan çok kiracılı web uygulaması geliştirme. Bu özellikler dizin nesneleri erişim gerektirir ve bu nedenle grafik API'sini çağırmak uygulamanın gerekir. Geliştirici Azure AD'de uygulama kaydeder, grafik API'si için izinleri yazma ve okuma gerektirecek şekilde yapılandırır ve böylece diğer kuruluşlardan kendi dizininde uygulamayı kullanmak için onay dış erişim sağlar. Bir kullanıcı başka bir kuruluştaki uygulama ilk kez doğruladığında, bunlar uygulama istenirken izinlere sahip bir onay iletişim kutusu gösterilir.  Onay sonra uygulama verecektir verme olanlar kullanıcının dizininde grafik API'si izin istedi. Onay framework hakkında daha fazla bilgi için bkz: [onayı Framework'e Genel Bakış](active-directory-integrating-applications.md).

## <a name="see-also"></a>Ayrıca Bkz.
[Azure AD grafik API'si Hızlı Başlangıç Kılavuzu](active-directory-graph-api-quickstart.md)

[AD grafik REST belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory geliştirici kılavuzu](active-directory-developers-guide.md)

