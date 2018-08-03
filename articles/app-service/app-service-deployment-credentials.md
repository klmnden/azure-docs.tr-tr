---
title: Azure App Service dağıtımı kimlik bilgileri | Microsoft Docs
description: Azure App Service dağıtım kimlik bilgilerini kullanmayı öğrenin.
services: app-service
documentationcenter: ''
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: a17260770f0b2e0a73585ce4108bd5625ac22229
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436157"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Azure App Service için dağıtım kimlik bilgilerini yapılandırma
[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) iki tür kimlik bilgilerini destekler [yerel Git dağıtımı](app-service-deploy-local-git.md) ve [FTP/S dağıtım](app-service-deploy-ftp.md). Bunlar Azure Active Directory kimlik bilgilerinizi ile aynı değildir.

* **Kullanıcı düzeyinde kimlik**: tek tüm Azure hesabı için kimlik bilgileri kümesi. Azure hesabı için erişim iznine sahip olduğu herhangi bir abonelikte, herhangi bir uygulama için App Service'e dağıtmak için kullanılabilir. Yapılandırdığınız varsayılan kimlik bilgileri kümesi bunlar **uygulama hizmetleri** > **&lt;app_name >** > **dağıtım kimlik bilgileri**. Bu ayrıca varsayılandır GUI portalında ortaya kümesi (gibi **genel bakış** ve **özellikleri** , uygulamanızın [kaynak sayfası](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > Rol tabanlı erişim denetimi (RBAC) veya ortak yönetici izinleri aracılığıyla Azure kaynaklarına erişimi temsilci, erişimi iptal kadar bir uygulamaya erişim aldığı her Azure kullanıcı kendi kişisel kullanıcı düzeyi kimlik bilgilerini kullanabilirsiniz. Bu dağıtım kimlik bilgileri Azure başkalarıyla Paylaşılmaması gereken.
    >
    >

* **Uygulama düzeyinde kimlik**: tek her uygulama için kimlik bilgileri kümesi. Yalnızca bu uygulamasına dağıtmak için kullanılabilir. Her uygulama, uygulama oluşturma sırasında otomatik olarak oluşturulur ve uygulamanın bulunan yayımlama profili kimlik bilgileri. Kimlik bilgilerini el ile yapılandıramazsınız, ancak bunları bir uygulama için dilediğiniz zaman sıfırlayabilirsiniz.

    > [!NOTE]
    > Vermek için birisi erişmek için bu kimlik bilgileri ile rol tabanlı erişim denetimi (RBAC), katkıda bulunan yapmanız gereken veya Web uygulamasında daha yüksek. Okuyucular, yayımlama izin verilmez ve bu nedenle bu kimlik bilgilerine erişemez.
    >
    >

## <a name="userscope"></a>Ayarlayın ve kullanıcı düzeyi kimlik bilgilerini sıfırlama

Bir uygulamanın kullanıcı düzeyinde kimlik bilgilerinizi yapılandırabileceğiniz [kaynak sayfası](../azure-resource-manager/resource-group-portal.md#manage-resources). Bakılmaksızın hangi uygulamanın bu kimlik bilgilerini yapılandırın, bunu tüm uygulamalar ve Azure hesabınızda tüm abonelikler için geçerlidir. 

Kullanıcı düzeyinde kimlik bilgilerinizi yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), App Service'ı tıklayın >  **&lt;any_app >** > **dağıtım kimlik bilgileri**.

    > [!NOTE]
    > Portalda dağıtım kimlik bilgileri sayfasında erişebilmeniz için önce en az bir uygulama olmalıdır. Bununla birlikte, [Azure CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set), mevcut bir uygulamayı olmadan kullanıcı düzeyinde kimlik yapılandırabilirsiniz.

2. Kullanıcı adı ve parola yapılandırın ve ardından **Kaydet**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Dağıtım kimlik bilgilerinizi ayarladıktan sonra bulabilirsiniz *Git* uygulamanızın dağıtım kullanıcı adı **genel bakış**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

ve *FTP* uygulamanızın dağıtım kullanıcı adı **özellikleri**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure kullanıcı düzeyinde dağıtım parolanızı göstermez. Parolayı unutursanız alınamıyor. Ancak, bu bölümdeki adımları izleyerek kimlik bilgilerinizi sıfırlayabilirsiniz.
>
>  

## <a name="appscope"></a>Alma ve uygulama düzeyinde kimlik bilgilerini sıfırlama
App Service her uygulama için uygulama düzeyinde kimlik bilgileri XML dosyasında depolanan yayımlama profili.

Uygulama düzeyinde kimlik bilgilerini almak için:

1. İçinde [Azure portalında](https://portal.azure.com), App Service'ı tıklayın >  **&lt;any_app >** > **genel bakış**.

2. Tıklayın **... Daha fazla** > **yayımlama profili Al**, ve indirme başladığında için bir. PublishSettings dosyası.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Açık. PublishSettings dosyası ve bulma `<publishProfile>` özniteliğine sahip etiket `publishMethod="FTP"`. Daha sonra kendi `userName` ve `password` öznitelikleri.
Bu, uygulama düzeyinde kimlik bilgileridir.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Kullanıcı düzeyinde kimlik, FTP dağıtım kullanıcı adı biçiminde benzer `<app_name>\<username>`, ve Git dağıtım kullanıcı adı yalnızca `<username>` önceki olmadan `<app_name>\`.

Uygulama düzeyinde kimlik bilgilerini sıfırlamak için:

1. İçinde [Azure portalında](https://portal.azure.com), App Service'ı tıklayın >  **&lt;any_app >** > **genel bakış**.

2. Tıklayın **... Daha fazla** > **yayımlama profilini Sıfırla**. Tıklayın **Evet** sıfırlamasını onaylamak için.

    Sıfırlama eylemi herhangi daha önce indirilen geçersiz kılar. PublishSettings dosyaları.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızdan dağıtmak için bu kimlik bilgilerini kullanmak nasıl kaydolacağınızı [yerel Git](app-service-deploy-local-git.md) veya bu adı kullanıyor [FTP/S](app-service-deploy-ftp.md).
