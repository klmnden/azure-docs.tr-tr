---
title: "Azure portalı kullanarak Azure AD kimlik doğrulaması ile çalışmaya başlama | Microsoft Docs"
description: "Azure Media Services API kullanmak için Azure Active Directory (Azure AD) kimlik doğrulaması erişmek için Azure portalını kullanmayı öğrenin."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 829e0759f8aeb8758f6b8895b88b60b66ffb22ed
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a>Azure portalı kullanarak Azure AD kimlik doğrulaması ile çalışmaya başlama

Azure Media Services API erişmek için Azure Active Directory (Azure AD) kimlik doğrulaması erişmek için Azure portalını kullanmayı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Bir hesabınız yoksa, başlayan bir [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için bkz: [Azure portalını kullanarak Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Gözden geçirmeniz emin olun [Azure AD kimlik doğrulamasına genel bakış ile Azure Media Services API erişme](media-services-use-aad-auth-to-access-ams-api.md). 

Azure Media Services ile Azure AD kimlik doğrulaması kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Kullanıcı kimlik doğrulaması**. Uygulama etkileşim kurmak için Media Services kaynaklarla kullanan bir kişi kimlik doğrulaması. Etkileşimli uygulama önce kullanıcıdan kimlik bilgilerini ister. Kodlama işleri izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet asıl kimlik doğrulaması**. Bir hizmetin kimliğini. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamaları arka plan programı services, orta katman Hizmetleri veya zamanlanmış işler çalışan uygulamalar şunlardır: web uygulamaları, işlev uygulamalarının, logic apps, API veya bir mikro hizmet.

> [!IMPORTANT]
> Şu anda, Media Services, Azure erişim denetimi hizmeti kimlik doğrulama modelini destekler. Ancak, erişim denetimi yetkilendirme 1 Haziran 2018 kullanım dışı kalacaktır. Azure AD kimlik doğrulama modeline mümkün olan en kısa sürede geçirmek öneririz.

## <a name="select-the-authentication-method"></a>Kimlik doğrulama yöntemini seçin

1. İçinde [Azure portal](https://portal.azure.com/), Media Services hesabınızı seçin.
2. Medya Hizmetleri API'sine bağlanma seçin.

    ![Bağlantı yöntemi sayfası seçin](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması

Kullanıcı kimlik doğrulama seçeneğini kullanarak Media Services API'sine bağlanmak için aşağıdaki parametreleri olan bir Azure AD belirteç istemek istemci uygulamanın gerekir:  

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si
* Media Services (yerel) uygulama istemci kimliği 
* Media Services (yerel) uygulama yeniden yönlendirme URI'si 
* Kaynak URI'si REST Media Services için

Değerleri bu parametreler için alma **kullanıcı kimlik doğrulaması ile Media Services API** sayfası. 

![Kullanıcı kimlik doğrulaması sayfası ile bağlanma](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Media Services Microsoft .NET SDK'sını kullanarak Media Services API'sine bağlanıyorsanız, gerekli değerleri, SDK'ın bir parçası olarak kullanılabilir. Daha fazla bilgi için bkz: [.NET ile Azure Media Services API erişmek için kimlik doğrulamasını kullan Azure AD](media-services-dotnet-get-started-with-aad.md).

Media Services .NET İstemci SDK kullanmıyorsanız, daha önce açıklanan parametreleri kullanarak bir Azure AD belirteç isteği el ile oluşturmalısınız. Daha fazla bilgi için bkz: [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet asıl seçeneğini kullanarak Media Services API'sine bağlanmak için aşağıdaki parametreleri olan bir Azure AD belirteç istemek Orta katmanda uygulamanızı (web API ya da web uygulaması) gerekir:  

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si 
* Kaynak URI'si REST Media Services için
* Azure AD uygulama değerleri: **istemci kimliği** ve **istemci parolası**

Değerleri bu parametreler için alma **Media Services API hizmet asıl Bağlan** sayfası. Yeni bir oluşturmak için bu sayfayı kullanın Azure AD uygulaması veya varolan bir tanesini seçin. Azure AD uygulaması seçtikten sonra istemci kimliği (uygulama kimliği) almak ve istemci gizli anahtarı (anahtar) değerlerini oluşturmak. 

![Hizmet asıl sayfasıyla Bağlan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

Zaman **hizmet sorumlusu** dikey penceresi açıldığında, aşağıdaki ölçütleri karşılaması ilk Azure AD uygulaması seçilir:

- Kayıtlı olan Azure AD uygulaması.
- Bu hesapta katkıda bulunan veya Owner Role-Based erişim denetimi iznine sahiptir.

Oluşturma veya bir Azure AD uygulaması seçtikten sonra oluşturun ve istemci parolası (anahtar) ve istemci kimliği (uygulama kimliği) kopyalayın. İstemci kimliği ve istemci parolası erişim Bu senaryoda belirtecini almak için gereklidir.

Etki alanınızda Azure AD uygulamaları oluşturmak için izinlere sahip değilseniz, Azure AD uygulama denetimleri dikey gösterilmez ve bir uyarı iletisi görüntülenir.

Media Services .NET SDK'sını kullanarak Media Services API'sine bağlanmak istiyorsanız bkz [.NET ile Azure Media Services API erişmek için kimlik doğrulamasını kullan Azure AD](media-services-dotnet-get-started-with-aad.md).

Media Services .NET İstemci SDK kullanmıyorsanız, daha önce açıklanan parametreleri kullanarak bir Azure AD belirteç isteği el ile oluşturmanız gerekir. Daha fazla bilgi için bkz: [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="get-the-client-id-and-client-secret"></a>İstemci Kimliğini ve istemci gizli anahtarı alma

Var olan bir Azure AD uygulama seçin veya yeni bir tane oluşturmak için seçeneği seçtikten sonra aşağıdaki düğmeler görüntülenir:

![İzinler ve Yönet uygulama düğmesi yönetme](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

Azure AD uygulaması dikey penceresini açmak için **uygulamasını Yönet**. Üzerinde **uygulamasını Yönet** dikey penceresinde, uygulamanın istemci Kimliğini (uygulama kimliği) alabilirsiniz. İstemci parolası (anahtar) oluşturmak için seçin **anahtarları**.

![Uygulama dikey penceresinde anahtarlar seçeneğini yönetme](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a>İzinler ve uygulama yönetme

Azure AD uygulaması seçtikten sonra izinleri ve uygulama yönetebilirsiniz. Diğer uygulamalara erişmek için Azure AD uygulamanızı ayarlama için tıklatın **izinleri yönetmek**. İçin yönetim görevleri, anahtarları ve yanıt URL'leri değiştirme gibi veya uygulama bildirimi düzenlemek için tıklatın **uygulamasını Yönet**.

### <a name="edit-the-apps-settings-or-manifest"></a>Listesi veya uygulamanın ayarlarını Düzenle

Uygulamanın ayarları ya da bildirim düzenlemek için tıklatın **uygulamasını Yönet**.

![Uygulamasını Yönet sayfası](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-portal-upload-files.md).
