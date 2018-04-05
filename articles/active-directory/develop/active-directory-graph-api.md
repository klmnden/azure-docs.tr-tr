---
title: Azure Active Directory grafik API'si | Microsoft Docs
description: Azure ad REST API uç noktaları yoluyla programlı erişim sağlayan Azure AD grafik API'si için bir genel bakış ve Hızlı Başlangıç Kılavuzu.
services: active-directory
documentationcenter: ''
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
ms.openlocfilehash: 1d1cfed6782ae2ea93f350aa11993d257800ce7b
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API'si
> [!IMPORTANT]
> Azure Active Directory kaynaklarına erişmek için Azure AD Graph API'si yerine [Microsoft Graph](https://graph.microsoft.io/) kullanmanız önemle tavsiye edilir. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Graph API’sinin hala uygun olabileceği senaryo sayısı çok sınırlıdır; daha fazla bilgi için Office Geliştirici Merkezi’ndeki [Microsoft Graph veya Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blog gönderisine bakın.
> 
> 

Azure Active Directory grafik API'si, REST API uç noktaları aracılığıyla Azure AD için programlı erişim sağlar. Uygulamaları gerçekleştirmek için Azure AD grafik API'si kullanabilir oluşturma, okuma, güncelleştirme ve (CRUD) işlemleridir dizin verileri ve nesneleri silme. Örneğin, Azure AD grafik API'si bir kullanıcı nesnesi için aşağıdaki ortak işlemleri destekler:

* Yeni bir kullanıcı bir dizin oluşturun
* Kendi gruplar gibi bir kullanıcının ayrıntılı özelliklerini alma
* Konum ve telefon numarası gibi bir kullanıcının özelliklerini güncelleştirmek veya parolasını değiştirme
* Rol tabanlı erişim için kullanıcının grup üyeliğini denetleyin
* Bir kullanıcı hesabı devre dışı bırakmak veya tamamen silme

Ayrıca, gruplar ve uygulamalar gibi diğer nesneler üzerinde benzer işlemler gerçekleştirebilirsiniz. Bir dizin üzerinde Azure AD Graph API çağrısı için uygulamanızı Azure AD ile kaydedilmesi gerekir. Uygulamanız, Azure AD grafik API'sine erişim de verilmelidir. Bu erişim, normal bir kullanıcı veya yönetici onayı akışını elde edilir.

Azure Active Directory grafik API'sini kullanmaya başlamak için bkz: [Azure AD Graph API Hızlı Başlangıç Kılavuzu](active-directory-graph-api-quickstart.md), veya görüntülemek [etkileşimli Azure AD Graph API başvuru belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Özellikler
Azure AD Graph API aşağıdaki özellikleri sağlar:

* **REST API uç noktaları**: Azure AD Graph API standart HTTP isteklerini kullanarak erişilen uç noktaları oluşan bir RESTful hizmettir. Azure AD Graph API istekleri ve yanıtları için XML veya Javascript nesne gösterimi (JSON) içerik türlerini destekler. Daha fazla bilgi için bkz: [Azure AD Graph REST API Başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Azure AD ile kimlik doğrulaması**: bir JSON Web Token (JWT) isteğinin yetkilendirme üst ekleyerek Azure AD Graph API her isteğin kimliğinin doğrulanması gerekir. Bu belirteç, Azure AD belirteç uç noktası için istekte ve geçerli kimlik bilgilerini sağlayan alınır. OAuth 2.0 istemci kimlik bilgileri akışını kullanabilir veya yetki kodu izin akışı grafiği çağırmak üzere bir belirteç almak üzere. Daha fazla bilgi için [OAuth 2.0 Azure AD'de](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Rol tabanlı yetkilendirme (RBAC)**: güvenlik grupları, Azure AD grafik API'si RBAC gerçekleştirmek için kullanılır. Örneğin, bir kullanıcının belirli bir kaynağa erişim izni olup olmadığını, uygulamayı çağırabilir belirlemek istiyorsanız [grup üyeliğini denetleyin (Geçişli)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) true veya false döndürür işlemi.
* **Fark sorgu**: fark sorgu, Azure AD grafik API'sine sık sorguları yapmak zorunda kalmadan iki dönemleri arasında bir dizinde değişiklikleri izlemek sağlar. Bu istek türü yalnızca önceki fark sorgu isteği ve geçerli istek arasında yapılan değişiklikler döndürür. Daha fazla bilgi için bkz: [Azure AD Graph API fark sorgu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Dizin genişletmeleri**: ou ekleyebilirsiniz özel özellikler dizin nesneleri için dış veri deposuna gerek kalmadan. Örneğin, uygulamanız her kullanıcı için Skype ID özelliği gerektiriyorsa, dizinde yeni özellik kaydedebilirsiniz ve her kullanıcı nesnesi üzerinde kullanım için kullanılabilir. Daha fazla bilgi için bkz: [Azure AD Graph API Directory şema uzantıları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **İzin kapsamları tarafından güvenliği sağlanan**: Azure AD Graph API OAuth 2.0 kullanan Azure AD verilere güvenli erişim sağlayan izin kapsamları kullanıma sunar. İstemci uygulama türleri dahil olmak üzere, çeşitli destekler:
  
  * Temsilci yetkilendirme aracılığıyla verilere (temsilci) oturum açmış kullanıcıdan dosyaya erişen kullanıcı arabirimleri
  * oturum açmış kullanıcı olmaksızın arka planda işlem sunmak ve rol tabanlı erişim uygulama tanımlı kullanmak hizmeti/arka plan programı uygulamaları denetleme
    
    Her ikisi de devredildi ve uygulama izinleri Azure AD grafik API'si tarafından kullanıma sunulan ayrıcalık temsil eder ve uygulama kayıt izinleri özellikleri ile istemci uygulamaları tarafından istenen [Azure portal](https://portal.azure.com). [Azure AD grafik API'si izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) istemci uygulamanız tarafından kullanılabilir nedir bilgi sağlar.

## <a name="scenarios"></a>Senaryolar
Azure AD Graph API birçok uygulama senaryolara olanak sağlar. Aşağıdaki senaryolar en yaygın şunlardır:

* **(Tek Kiracılı) iş hattı uygulaması**: Bu senaryoda, bir Office 365 aboneliğine sahip bir kuruluş için bir kuruluş Geliştirici çalışır. Geliştirici, böyle bir kullanıcıya lisans atanmasını görevleri gerçekleştirmek için Azure AD ile etkileşime giren bir web uygulaması oluşturuyor. Bu görev Azure AD Graph API erişimi gerektirir, geliştirici Azure AD içinde tek bir kiracı uygulama kaydeder ve yapılandırır okuma ve yazma izinlerine Azure AD grafik API'si. Sonra uygulama kendi kimlik bilgilerini veya şu anda oturum açma kullanıcı olan Azure AD grafik API'si yi çağırmak üzere bir belirteç almak için kullanmak üzere yapılandırılır.
* **Bir hizmet uygulaması (çok Kiracılı) olarak yazılım**: Bu senaryoda, bağımsız yazılım satıcısı (ISV), Azure AD kullanan başka kuruluşlar için kullanıcı yönetimi özellikleri sağlar barındırılan çok kiracılı web uygulaması geliştirme. Bu özellikler dizin nesneleri erişim gerektirir ve bu nedenle uygulamanın Azure AD Graph API çağrısı gerekir. Geliştirici Azure AD'de uygulama kaydeder, Azure AD Graph API izinlerini yazma ve okuma gerektirecek şekilde yapılandırır ve böylece diğer kuruluşlardan kendi dizininde uygulamayı kullanmak için onay dış erişim sağlar. Bir kullanıcı başka bir kuruluştaki uygulama ilk kez doğruladığında, bunlar uygulama istenirken izinlere sahip bir onay iletişim kutusu gösterilir.  Onay sonra uygulama verecektir verme olanlar kullanıcının dizininde Azure AD grafik API'si izin istedi. Onay framework hakkında daha fazla bilgi için bkz: [onayı Framework'e Genel Bakış](active-directory-integrating-applications.md).

## <a name="see-also"></a>Ayrıca Bkz.
[Azure AD grafik API'si Hızlı Başlangıç Kılavuzu](active-directory-graph-api-quickstart.md)

[Azure AD Graph REST belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory geliştirici kılavuzu](active-directory-developers-guide.md)

