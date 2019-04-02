---
title: Azure Active Directory kimlik doğrulaması ile Azure Media Services API'sine erişim | Microsoft Docs
description: Kavramlar ve Azure Media Services API'sine erişim kimlik doğrulaması için Azure Active Directory (Azure AD) kullanmak için atılması gereken adımlar hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: d80a58f1886ecc1ca2a735881fc5822f2fc0c53b
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802499"
---
# <a name="access-the-azure-media-services-api-with-azure-ad-authentication"></a>Azure AD kimlik doğrulamasıyla Azure Media Services API'sine erişim  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Azure Media Services API'sine bir RESTful API'dir. Kullanılabilir istemci SDK'ları kullanarak ya da REST API'yi kullanarak medya kaynaklar üzerinde işlem gerçekleştirmek için kullanabilirsiniz. Azure Media Services, Microsoft .NET için Media Services istemci SDK'sı sunar. Media Services kaynakları ve Media Services API'sine erişim iznine sahip olması için önce kimliğinin doğrulanması gerekir. 

Media Services'ı destekleyen [Azure Active Directory (Azure AD)-tabanlı kimlik doğrulaması](../../active-directory/fundamentals/active-directory-whatis.md). Azure medya REST hizmeti gerektirir kullanıcı veya REST API yapan uygulamanın ya da sahip istekleri **katkıda bulunan** veya **sahibi** kaynaklara erişmek için rol. Daha fazla bilgi için [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../../role-based-access-control/overview.md).  

Bu belge, REST veya .NET API'lerini kullanarak Media Services API'sine erişmek nasıl bir genel bakış sağlar.

> [!NOTE]
> Erişim denetimi yetkilendirme, 1 Haziran 2018'de kullanım dışı bırakıldı.

## <a name="access-control"></a>Erişim denetimi

Azure medya REST isteği başarılı olması çağıran kullanıcı erişmeye çalışıyor Media Services hesap için katkıda bulunan veya sahip bir rolü olmalıdır.  
Sahip rolüne bir kullanıcı, yeni kullanıcılar veya uygulamalar için medya kaynağı (hesap) erişim verebilirsiniz. Katkıda bulunan rolü yalnızca medya kaynağı erişebilirsiniz.
Yetkisiz istekler, 401 durum kodu ile başarısız. Bu hata kodunu görürseniz, kullanıcının kullanıcı Media Services hesabı için atanan katkıda bulunan veya sahip rolü olup olmadığını denetleyin. Bu işlemi Azure portalında denetleyebilirsiniz. Medya hesabınız için arama yapın ve ardından **erişim denetimi** sekmesi. 

![Erişim denetim sekmesi](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Kimlik doğrulama türleri 
 
Azure Media Services ile Azure AD kimlik doğrulaması kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Kullanıcı kimlik doğrulaması**. Uygulama Media Services kaynaklarıyla etkileşim kurmak için kullanıyor bir kişiyi kimlik doğrulaması. Etkileşimli uygulama önce kullanıcının kimlik bilgilerini girmesini. Kodlama işi izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet sorumlusu kimlik doğrulaması**. Bir hizmet kimlik doğrulaması. Genellikle bu kimlik doğrulama yöntemi kullanan uygulamalar, daemon Hizmetleri, orta katman Hizmetleri ya da zamanlanmış işleri çalıştırma uygulamalardır. Web apps, işlev uygulamaları, logic apps, API ve mikro hizmetler verilebilir.

### <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması 

Kullanıcı kimlik doğrulama yöntemini kullanması gereken uygulamalardır Yönetimi veya yerel uygulamaları izleme: mobil uygulamaları, Windows uygulamaları ve konsol uygulamaları. Bu tür bir çözüm, aşağıdaki senaryolardan biri, hizmet insan etkileşimi istediğinizde yararlı olur:

- Kodlama işlerinizi için izleme Panosu.
- Canlı akışlarınız için izleme Panosu.
- Bir Media Services hesabında kaynakları yönetmek, masaüstü veya mobil kullanıcılar için uygulama yönetimi.

> [!NOTE]
> Bu kimlik doğrulama yöntemi, tüketiciye yönelik uygulamalar için kullanılmamalıdır. 

Yerel bir uygulama önce Azure AD'den erişim belirteci alma ve Media Services REST API'sine HTTP isteklerini yaptığınızda kullanmanız gerekir. İstek üstbilgisi için erişim belirtecini ekleyin. 

Tipik etkileşimli uygulama kimlik doğrulaması akışı aşağıdaki diyagramda gösterilmiştir: 

![Yerel uygulamalar diyagramı](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

Yukarıdaki diyagramda, istekleri kronolojik sırada akışını sayıları temsil eder.

> [!NOTE]
> Kullanıcı kimlik doğrulama yöntemini kullandığınızda, tüm uygulamalar aynı (varsayılan) yerel uygulama istemci Kimliğini ve yerel uygulama yeniden yönlendirme URI'si paylaşın. 

1. Bir kullanıcıdan kimlik bilgilerini ister.
2. Aşağıdaki parametrelerle bir Azure AD erişim belirteci isteği:  

   * Azure AD Kiracı uç noktası.

       Kiracı bilgileri, Azure portalından alınabilir. İmlecinizi üst oturum açan kullanıcı adının üzerine sağ alt köşesinde yerleştirin.
   * Media Services kaynak URI'si. 

       Bu URI aynı Azure ortamında olan Media Services hesapları için aynıdır (örneğin, https://rest.media.azure.net).

   * Media Services (yerel) uygulama istemci kimliği
   * Media Services (yerel) uygulama yeniden yönlendirme URI'si.
   * Kaynak REST Media Services için URI.
        
       REST API uç noktası URI'si temsil eder (örneğin, https://test03.restv2.westus.media.azure.net/api/).

     Bu parametrelerin değerlerini almak için bkz: [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portal'ı kullanmanızı](media-services-portal-get-started-with-aad.md) kullanarak kullanıcı kimlik doğrulaması seçeneği.

3. Azure AD erişim belirteci istemciye gönderilir.
4. İstemci, Azure medya REST API'si ile Azure AD erişim belirteci bir istek gönderir.
5. İstemci verileri Media Services'den geri alır.

Media Services .NET İstemci SDK'sı kullanarak REST istekleri ile iletişim kurmak için Azure AD kimlik doğrulamasını kullanma hakkında daha fazla bilgi için bkz: [.NET ile Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması](media-services-dotnet-get-started-with-aad.md). 

Media Services .NET İstemci SDK'sı kullanmıyorsanız, 2. adımda açıklanan parametreleri kullanarak, el ile bir Azure AD erişim belirteci isteği oluşturmalısınız. Daha fazla bilgi için [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Genellikle bu kimlik doğrulama yöntemi kullanan uygulamalar olan orta katman hizmet ve zamanlanan işler çalışan uygulamalar: web apps, işlev uygulamaları, logic apps, API ve mikro hizmetler. Bu kimlik doğrulama yöntemini de kaynakları yönetmek için bir hizmet hesabı kullanmak isteyebilirsiniz etkileşimli uygulamalar için uygundur.

Tüketici senaryoları oluşturmak için hizmet sorumlusu kimlik doğrulama yöntemini kullandığınızda, genellikle Orta katmanda (bazı API) aracılığıyla ve doğrudan bir mobil veya masaüstü uygulamasında kimlik doğrulaması ele alınır. 

Bu yöntemi kullanmak için bir Azure AD uygulaması ve hizmet sorumlusu kendi kiracısında oluşturun. Uygulamayı oluşturduktan sonra Media Services hesabına uygulama katkıda bulunan veya sahip rolü erişimi verin. Azure portalında, Azure CLI kullanarak veya bir PowerShell Betiği ile bunu yapabilirsiniz. Mevcut bir Azure AD uygulaması da kullanabilirsiniz. Kaydolun ve Azure AD uygulaması ve hizmet sorumlusu yönetme [Azure portalında](media-services-portal-get-started-with-aad.md). Ayrıca kullanarak bunu yapabilirsiniz [Azure CLI](media-services-use-aad-auth-to-access-ams-api.md) veya [PowerShell](media-services-powershell-create-and-configure-aad-app.md). 

![Orta katman uygulamaları](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Azure AD uygulamanızı oluşturduktan sonra aşağıdaki ayarları için değerleri alır. Kimlik doğrulaması için bu değerlere ihtiyacınız olur:

- İstemci Kimliği 
- Gizli anahtar 

Önceki şekilde kronolojik sırada isteklerinin akışını sayıları temsil eder:
    
1. Aşağıdaki parametrelere sahip bir Azure AD erişim belirteci (web API'si veya web uygulaması) bir orta katman uygulaması ister:  

   * Azure AD Kiracı uç noktası.

       Kiracı bilgileri, Azure portalından alınabilir. İmlecinizi üst oturum açan kullanıcı adının üzerine sağ alt köşesinde yerleştirin.
   * Media Services kaynak URI'si. 

       Bu URI aynı Azure ortamında bulunan Media Services hesapları için aynıdır (örneğin, https://rest.media.azure.net).

   * Kaynak REST Media Services için URI.

       REST API uç noktası URI'si temsil eder (örneğin, https://test03.restv2.westus.media.azure.net/api/).

   * Azure AD uygulama değerlerini: istemci Kimliğini ve istemci gizli anahtarı.
    
     Bu parametrelerin değerlerini almak için bkz: [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portalını kullanma](media-services-portal-get-started-with-aad.md) kullanarak hizmet sorumlusu kimlik doğrulaması seçeneği.

2. Azure AD erişim belirteci için orta katman gönderilir.
4. Orta katman Azure AD belirteciyle Azure medya REST API isteği gönderir.
5. Orta katman veri Media Services'den geri alır.

Media Services .NET İstemci SDK'sı kullanarak REST istekleri ile iletişim kurmak için Azure AD kimlik doğrulamasını kullanma hakkında daha fazla bilgi için bkz. [.NET ile Azure Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması](media-services-dotnet-get-started-with-aad.md). 

Media Services .NET İstemci SDK'sı kullanmıyorsanız, 1. adımda açıklanan parametreleri kullanarak, el ile bir Azure AD belirteç isteği oluşturmalısınız. Daha fazla bilgi için [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Sorun giderme

Özel durum: "Uzak sunucu bir hata döndürdü: (401) yetkisiz."

Çözüm: Media Services REST isteği başarılı olması çağıran kullanıcı bir katkıda bulunan veya sahip rolü Media Services hesabına erişmeye çalışıyor olması gerekir. Daha fazla bilgi için [erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control) bölümü.

## <a name="resources"></a>Kaynaklar

Azure AD kimlik doğrulaması kavramları genel bakış makaleleridir: 

- [Azure AD tarafından ele alınan kimlik doğrulama senaryoları](../../active-directory/develop/authentication-scenarios.md)
- [Ekleme, güncelleştirme veya Azure AD'de bir uygulamayı kaldırma](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md)
- [Yapılandırma ve rol tabanlı erişim denetimi PowerShell kullanarak yönetme](../../role-based-access-control/role-assignments-powershell.md)

## <a name="next-steps"></a>Sonraki adımlar

* Azure portalını kullanarak [erişim Azure AD kimlik doğrulaması, Azure Media Services API'sine tüketmeye](media-services-portal-get-started-with-aad.md).
* Kullanım Azure AD kimlik doğrulaması [.NET ile Azure Media Services API'sine erişim](media-services-dotnet-get-started-with-aad.md).

