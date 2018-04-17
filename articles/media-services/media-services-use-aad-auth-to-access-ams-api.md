---
title: Azure Active Directory kimlik doğrulaması ile Azure Media Services API erişim | Microsoft Docs
description: Kavramları ve Azure Media Services API erişimini doğrulamak için Azure Active Directory (Azure AD) kullanmak için uygulanacak adımlar hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 57f2680d6b3f06a88a13a09018e7d72afcb710a6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="access-the-azure-media-services-api-with-azure-ad-authentication"></a>Azure AD kimlik doğrulaması ile Azure Media Services API erişimi
 
Azure Media Services API bir RESTful API'sidir. REST API kullanarak veya kullanılabilir istemci SDK'ları kullanarak medya kaynaklar üzerinde işlem gerçekleştirmek için kullanabilirsiniz. Azure Media Services, Microsoft .NET için Media Services istemci SDK sunar. Media Services kaynaklarına ve Media Services API erişmek için yetki verilmesi için önce kimliğinin doğrulanması gerekir. 

Media Services destekler [Azure Active Directory (Azure AD)-tabanlı kimlik doğrulaması](../active-directory/active-directory-whatis.md). Azure Media REST hizmeti gerektiren kullanıcı veya REST API yapan uygulamada ya da sahip istek **katkıda bulunan** veya **sahibi** kaynaklara erişmek için rol. Daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../role-based-access-control/overview.md).  

> [!IMPORTANT]
> Şu anda, Media Services, Azure erişim denetimi hizmeti kimlik doğrulama modelini destekler. Ancak, erişim denetimi yetkilendirme 1 Haziran 2018 kullanım dışı kalacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

Bu belge, Media Services API REST veya .NET API'lerini kullanarak erişmek nasıl bir genel bakış sağlar.

## <a name="access-control"></a>Erişim denetimi

Azure Media REST istek başarılı olması arama kullanıcının erişmeye çalışır Media Services hesap için katılımcı veya sahibi bir rolü olmalıdır.  
Yalnızca sahibi rolüne sahip bir kullanıcı, yeni kullanıcılar veya uygulamalar için medya kaynağı (hesap) erişim verebilirsiniz. Katkıda bulunan rolü yalnızca medya kaynağı erişebilir.
Yetkisiz istekler 401 durum kodu ile başarısız olur. Bu hata kodu görürseniz, kullanıcı için kullanıcının Media Services hesabı atanan katkıda bulunan veya sahibi rolü olup olmadığını denetleyin. Bu Azure portalında kontrol edebilirsiniz. Medya hesabınız için arama yapın ve ardından **erişim denetimi** sekmesi. 

![Erişim denetimi sekmesi](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Kimlik doğrulama türleri 
 
Azure Media Services ile Azure AD kimlik doğrulaması kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Kullanıcı kimlik doğrulaması**. Uygulama etkileşim kurmak için Media Services kaynaklarla kullanan bir kişi kimlik doğrulaması. Etkileşimli uygulaması ilk kullanıcı için kullanıcı kimlik bilgilerini ister. Kodlama işleri izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet asıl kimlik doğrulaması**. Bir hizmetin kimliğini. Genellikle bu kimlik doğrulama yöntemini kullanan uygulamalar, arka plan programı services, orta katman Hizmetleri veya zamanlanmış işleri çalıştırma uygulamalardır. Web uygulamaları, işlev uygulamalarının, logic apps, API ve mikro örnektir.

### <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması 

Kullanıcı kimlik doğrulama yöntemini kullanması gereken uygulamalardır Yönetimi'ni veya yerel uygulamalar izleme: mobil uygulamaları, Windows uygulamaları ve konsol uygulamaları. Bu tür bir çözüm, aşağıdaki senaryolardan birinde hizmeti ile insan etkileşimi istediğinizde yararlıdır:

- İzleme Panosu kodlama işleriniz için.
- İzleme Panosu, Canlı akışlar için.
- Bir Media Services hesabında kaynakları yönetmek Masaüstü ve mobil kullanıcılar için uygulama yönetimi.

> [!NOTE]
> Bu kimlik doğrulama yöntemini tüketiciye yönelik uygulamalar için kullanılmamalıdır. 

Bir yerel uygulamayı ilk Azure AD'den bir erişim belirteci alması ve Media Services REST API için HTTP isteklerini yaptığınızda kullanmanız gerekir. Erişim belirteci istek üstbilgisi ekleyin. 

Aşağıdaki diyagram tipik etkileşimli uygulama kimlik doğrulama akışı gösterilmektedir: 

![Yerel uygulamalar diyagramı](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

Önceki diyagramda, istekleri kronolojik sırada akışını sayıları temsil eder.

> [!NOTE]
> Kullanıcı kimlik doğrulama yöntemini kullandığınızda, tüm uygulamalar aynı (varsayılan) yerel uygulama istemci Kimliğini ve yerel uygulama yeniden yönlendirme URI'si paylaşır. 

1. Bir kullanıcıdan kimlik bilgilerini ister.
2. Aşağıdaki parametrelerle bir Azure AD erişim belirteci isteği:  

    * Azure AD Kiracı uç noktası.

        Kiracı bilgileri Azure portalından alınabilir. İmleç üzerinden üst oturum açmış kullanıcının adını sağ alt köşesinde yerleştirin.
    * Media Services kaynak URI'si. 

        Bu URI aynı Azure ortamında Media Services hesapları aynı olduğundan (örneğin, https://rest.media.azure.net).

    * Media Services (yerel) uygulama istemci kimliği
    * Media Services (yerel) uygulama yeniden yönlendirme URI'si.
    * Kaynak URI'si REST Media Services için.
        
        URI REST API uç noktasını temsil eder (örneğin, https://test03.restv2.westus.media.azure.net/api/).

    Bu parametrelerin değerlerini almak için bkz: [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portal'ı kullanmanızı](media-services-portal-get-started-with-aad.md) kullanarak kullanıcı kimlik doğrulaması seçeneği.

3. Azure AD erişim belirteci istemciye gönderilir.
4. İstemci Azure medya REST API'sini Azure AD erişim belirteci ile isteği gönderir.
5. İstemci verilerini Media Services'den geri alır.

Media Services .NET İstemci SDK'sını kullanarak REST istekleri ile iletişim kurmak için Azure AD kimlik doğrulaması kullanma hakkında daha fazla bilgi için bkz: [.NET ile Media Services API erişmek için kimlik doğrulamasını kullan Azure AD](media-services-dotnet-get-started-with-aad.md). 

Media Services .NET İstemci SDK kullanmıyorsanız, el ile bir Azure AD erişim belirteci isteği 2. adımda açıklanan parametreleri kullanarak oluşturmanız gerekir. Daha fazla bilgi için bkz: [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamaları orta katman Hizmetleri ve zamanlanmış işler çalışan uygulamalar şunlardır: web uygulamaları, işlev uygulamalarının, logic apps, API'ler ve mikro hizmetler. Bu kimlik doğrulama yöntemini de kaynakları yönetmek için bir hizmet hesabı kullanmak isteyebilirsiniz etkileşimli uygulamalar için uygundur.

Tüketici senaryoları oluşturmak için hizmet asıl kimlik doğrulama yöntemi kullandığınızda, kimlik doğrulama (bazı API) aracılığıyla orta katman ve doğrudan bir mobil veya masaüstü uygulaması tipik olarak işlenir. 

Bu yöntemi kullanmak için bir Azure AD uygulaması ve hizmeti, kendi Kiracı sorumlusu oluşturun. Uygulamayı oluşturduktan sonra Media Services hesabına uygulamasını katkıda bulunan veya sahibi rol erişim verin. Azure portalında Azure CLI kullanarak veya bir PowerShell Betiği ile bunu yapabilirsiniz. Var olan Azure AD uygulaması de kullanabilirsiniz. Kaydet ve Azure AD uygulaması ve hizmet sorumlusu yönetmek [Azure portalında](media-services-portal-get-started-with-aad.md). Ayrıca kullanarak bunu yapabilirsiniz [Azure CLI 2.0](media-services-use-aad-auth-to-access-ams-api.md) veya [PowerShell](media-services-powershell-create-and-configure-aad-app.md). 

![Orta katman uygulamalar](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Azure AD uygulaması oluşturduktan sonra aşağıdaki ayarları için değerleri alır. Kimlik doğrulaması için bu değerleri gerekir:

- İstemci Kimliği 
- Gizli anahtar 

Yukarıdaki şekilde, istekleri kronolojik sırada akışını sayıları temsil eder:
    
1. Orta katman uygulama (web API ya da web uygulaması) aşağıdaki parametrelere sahip bir Azure AD erişim belirteci ister:  

    * Azure AD Kiracı uç noktası.

        Kiracı bilgileri Azure portalından alınabilir. İmleç üzerinden üst oturum açmış kullanıcının adını sağ alt köşesinde yerleştirin.
    * Media Services kaynak URI'si. 

        Bu URI aynı Azure ortamında bulunan Media Services hesapları aynı olduğundan (örneğin, https://rest.media.azure.net).

    * Kaynak URI'si REST Media Services için.

        URI REST API uç noktasını temsil eder (örneğin, https://test03.restv2.westus.media.azure.net/api/).

    * Azure AD uygulama değerleri: istemci Kimliğini ve istemci gizli anahtarı.
    
    Bu parametrelerin değerlerini almak için bkz: [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portal'ı kullanmanızı](media-services-portal-get-started-with-aad.md) hizmet asıl kimlik doğrulama seçeneğini kullanarak.

2. Azure AD erişim belirteci orta katman gönderilir.
4. Orta katman Azure medya REST API'sini Azure AD belirteci ile isteği gönderir.
5. Orta katman veri Media Services'den geri alır.

Media Services .NET İstemci SDK'sını kullanarak REST istekleri ile iletişim kurmak için Azure AD kimlik doğrulaması kullanma hakkında daha fazla bilgi için bkz: [.NET ile Azure Media Services API erişmek için kimlik doğrulamasını kullan Azure AD](media-services-dotnet-get-started-with-aad.md). 

Media Services .NET İstemci SDK kullanmıyorsanız, el ile bir Azure AD belirteç isteği 1. adımda açıklanan parametreleri kullanarak oluşturmanız gerekir. Daha fazla bilgi için bkz: [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Sorun giderme

Özel durum: "uzak sunucusu bir hata döndürdü: (401) yetkisiz."

Çözüm: Media Services REST istek başarılı olması arama kullanıcının erişmeye çalışıyor Media Services hesabı katkıda bulunan veya sahibi rolünde olması gerekir. Daha fazla bilgi için bkz: [erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control) bölümü.

## <a name="resources"></a>Kaynaklar

Aşağıdaki makaleler Azure AD kimlik doğrulaması kavramlarını genel bakış şunlardır: 

- [Azure AD tarafından ele alınan kimlik doğrulama senaryoları](../active-directory/develop/active-directory-authentication-scenarios.md#basics-of-authentication-in-azure-ad)
- [Ekleme, güncelleştirme veya Azure AD'de uygulama kaldırma](../active-directory/develop/active-directory-integrating-applications.md)
- [Yapılandırma ve rol tabanlı erişim denetimi PowerShell kullanarak yönetme](../role-based-access-control/role-assignments-powershell.md)

## <a name="next-steps"></a>Sonraki adımlar

* Azure portalını kullanarak [Azure Media Services API kullanmak için erişim Azure AD kimlik doğrulaması](media-services-portal-get-started-with-aad.md).
* Kimlik doğrulamasını kullan Azure AD [.NET ile Azure Media Services API erişim](media-services-dotnet-get-started-with-aad.md).

