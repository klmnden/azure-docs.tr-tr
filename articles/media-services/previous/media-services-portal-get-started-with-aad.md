---
title: Azure portalını kullanarak Azure AD kimlik doğrulamasını kullanmaya başlama | Microsoft Docs
description: Azure Media Services API'sine kullanmak için Azure Active Directory (Azure AD) kimlik erişmek için Azure portalını kullanmayı öğrenin.
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
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: b7962f42b4244121a67b88ef3bf789ce40f7b1e5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719629"
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure AD kimlik doğrulamasını kullanmaya başlama

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Azure Media Services API'sine erişmek için Azure Active Directory (Azure AD) kimlik erişmek için Azure portalını kullanmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Bir hesabınız yoksa, başlayan bir [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için [Azure portalını kullanarak bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).
- Gözden emin [Azure AD kimlik doğrulamasına genel bakış ile Azure Media Services API'sine erişim](media-services-use-aad-auth-to-access-ams-api.md). 

Azure Media Services ile Azure AD kimlik doğrulaması kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Kullanıcı kimlik doğrulaması**. Uygulama Media Services kaynaklarıyla etkileşim kurmak için kullanıyor bir kişiyi kimlik doğrulaması. Etkileşimli uygulama, kullanıcıdan kimlik bilgilerini ilk isteyecektir. Kodlama işi izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir. 
- **Hizmet sorumlusu kimlik doğrulaması**. Bir hizmet kimlik doğrulaması. Genellikle bu kimlik doğrulama yöntemi kullanan uygulamalar daemon Hizmetleri, orta katman Hizmetleri ya da zamanlanmış işleri çalışan uygulamalar şunlardır: web apps, işlev uygulamaları, logic apps, API veya bir mikro hizmet.

> [!IMPORTANT]
> Şu anda Media Services, Azure erişim denetimi hizmeti kimlik doğrulama modeli destekler. Ancak, erişim denetimi yetkilendirme 1 Haziran 2018'de kullanımdan kaldırılacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="select-the-authentication-method"></a>Kimlik doğrulama yöntemini seçin

1. İçinde [Azure portalında](https://portal.azure.com/), Media Services hesabınızı seçin.
2. Media Services API'sine bağlanma seçin.

    ![Bağlantı yöntemi sayfası seçin](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması

Kullanıcı kimlik doğrulaması seçeneği kullanarak Media Services API'sine bağlanmak için aşağıdaki parametreleri olan Azure AD belirteçlerini istemek istemci uygulaması gerekir:  

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si
* Media Services (yerel) uygulama istemci kimliği 
* Media Services (yerel) uygulama yeniden yönlendirme URI'si 
* Kaynak URI REST Media Services için

Değerleri bu parametrelerin alma **kullanıcı kimlik doğrulaması ile Media Services API'sine** sayfası. 

![Kullanıcı kimlik doğrulaması sayfası ile bağlanma](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Medya Hizmetleri Microsoft .NET SDK'sını kullanarak Media Services API'sine bağlanma, gerekli değerleri, SDK'ın bir parçası olarak kullanılabilir. Daha fazla bilgi için [.NET ile Azure Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması](media-services-dotnet-get-started-with-aad.md).

Media Services .NET İstemci SDK'sı kullanmıyorsanız, daha önce açıklanan parametreleri kullanarak, el ile bir Azure AD belirteç isteği oluşturmalısınız. Daha fazla bilgi için [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusu seçeneğini kullanarak Media Services API'sine bağlanmak için aşağıdaki parametreleri olan Azure AD belirteçlerini istemek orta katman uygulamanızı (web API'si veya web uygulaması) gerekir:  

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si 
* Kaynak URI REST Media Services için
* Azure AD uygulama değerlerini: **istemci kimliği** ve **istemci gizli anahtarı**

Değerleri bu parametrelerin alma **hizmet sorumlusu ile Media Services API'sine bağlanma** sayfası. Yeni bir oluşturmak için bu sayfayı kullanın. Azure AD uygulaması veya varolan bir tanesini seçin. Azure AD uygulamasını seçtikten sonra istemci kimliği (uygulama kimliği) alın ve istemci gizli anahtarı (anahtar) değerlerini oluşturur. 

![İle hizmet sorumlusu sayfasına bağlanma](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

Zaman **hizmet sorumlusu** dikey penceresi açıldığında, aşağıdaki ölçütleri karşılayan ilk Azure AD uygulaması seçilir:

- Kayıtlı olan Azure AD uygulaması.
- Bu hesapta katkıda bulunan veya Owner Role-Based erişim denetimi izinleri vardır.

Oluşturun veya bir Azure AD uygulamasını seçin sonra oluşturabilir ve bir gizli anahtar (anahtar) ve istemci kimliği (uygulama kimliği) kopyalayın. İstemci kimliği ve gizli anahtar erişim Bu senaryoda, belirteci almak için gereklidir.

Etki alanınızı Azure AD uygulamaları oluşturmak için izinleriniz yoksa, Azure AD uygulama denetimleri dikey pencerenin gösterilmez ve bir uyarı iletisi görüntülenir.

Media Services .NET SDK'sını kullanarak Media Services API'sine bağlanma olup [.NET ile Azure Media Services API'sine erişmek için Azure AD kullanım kimlik doğrulaması](media-services-dotnet-get-started-with-aad.md).

Media Services .NET İstemci SDK'sı kullanmıyorsanız, daha önce açıklanan parametreleri kullanarak bir Azure AD belirteç isteği el ile oluşturmanız gerekir. Daha fazla bilgi için [Azure AD belirtecini almak için Azure AD kimlik doğrulama kitaplığı kullanmayı](../../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="get-the-client-id-and-client-secret"></a>İstemci Kimliğini ve istemci gizli anahtarını alma

Mevcut bir Azure AD uygulamasını seçin veya yeni bir tane oluşturmak için bu seçeneği seçin sonra aşağıdaki düğmeleri görünür:

![İzinler ve yönetme uygulaması düğmesi yönetme](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

Azure AD uygulama dikey penceresini açmak için **uygulamasını Yönet**. Üzerinde **uygulamasını Yönet** dikey penceresinde, uygulamanın istemci kimliği (uygulama kimliği) elde edebilirsiniz. Bir gizli anahtar (anahtar) oluşturmak üzere **anahtarları**.

![Uygulama dikey penceresinde anahtarlar seçeneğini yönetme](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a>İzinler ve uygulama yönetme

Azure AD uygulaması seçtikten sonra uygulamanızı ve izinlerini yönetebilir. Diğer uygulamalara erişmek için Azure AD uygulamanızı ayarlamak için tıklayın **izinleri Yönet**. İçin yönetim görevleri, anahtarları ve yanıt URL'lerini değiştirme gibi veya uygulama bildirimini düzenlemek için tıklayın **uygulamasını Yönet**.

### <a name="edit-the-apps-settings-or-manifest"></a>Bildirim veya uygulamanın ayarlarını Düzenle

Uygulama ayarları veya bildirimi düzenlemek için tıklayın **uygulamasını Yönet**.

![Uygulamasını Yönet sayfası](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-portal-upload-files.md).
