---
title: "Azure uygulama hizmeti dağıtım kimlik bilgileri | Microsoft Docs"
description: "Azure uygulama hizmeti dağıtım kimlik bilgilerini kullanmayı öğrenin."
services: app-service
documentationcenter: 
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
ms.openlocfilehash: d66b5aa4eb2ad90596dfe9e26bbc18996c967295
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Dağıtım kimlik bilgileri Azure App Service için yapılandırma
[Azure uygulama hizmeti](http://go.microsoft.com/fwlink/?LinkId=529714) iki tür kimlik bilgilerini destekler [yerel Git dağıtımı](app-service-deploy-local-git.md) ve [FTP/S dağıtım](app-service-deploy-ftp.md). Bu, Azure Active Directory kimlik bilgileri ile aynı değildir.

* **Kullanıcı düzeyinde kimlik**: bir tüm Azure hesabı için kimlik bilgileri kümesi. Azure hesabı erişim iznine sahip herhangi bir abonelikte, herhangi bir uygulama için uygulama hizmeti dağıtmak için kullanılabilir. Yapılandırdığınız varsayılan kimlik bilgileri kümesi bunlar **uygulama hizmetleri** > **&lt;app_name >** > **dağıtım kimlik bilgileri**. Bu aynı zamanda varsayılandır GUI portalında ortaya kümesi (gibi **genel bakış** ve **özellikleri** , uygulamanızın, [kaynak sayfası](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > Rol tabanlı erişim denetimi (RBAC) veya ortak yönetici izinleri üzerinden Azure kaynaklarına erişimi temsilci atarken, bir uygulamaya erişim alan her bir Azure kullanıcı erişimi iptal kadar güncelleştirmesini kişisel kullanıcı düzeyinde kimlik bilgilerini kullanabilir. Bu dağıtım kimlik bilgileri Azure diğer kullanıcılarla Paylaşılmaması gerekiyor.
    >
    >

* **Uygulama düzeyinde kimlik**: bir her uygulama için kimlik bilgileri kümesi. Yalnızca bu uygulama dağıtmak için kullanılabilir. Her uygulamanın uygulama oluşturma sırasında otomatik olarak oluşturulur ve uygulamanın içinde bulunan için yayımlama profili kimlik bilgileri. Kimlik bilgilerini el ile yapılandıramazsınız, ancak bunları bir uygulama için dilediğiniz zaman sıfırlayabilirsiniz.

    > [!NOTE]
    > Sunmak için birisi erişim bu kimlik bilgileri aracılığıyla rol tabanlı erişim denetimi (RBAC), bunları Katılımcısı olmanız gerekir veya Web uygulaması üzerinde daha yüksek. Okuyucular yayımlama izin verilmez ve bu nedenle bu kimlik bilgilerini erişemez.
    >
    >

## <a name="userscope"></a>Ayarlama ve kullanıcı düzeyinde kimlik bilgilerini sıfırlama

Bir uygulamanın ait kullanıcı düzeyinde kimlik bilgilerinizi yapılandırabilirsiniz [kaynak sayfası](../azure-resource-manager/resource-group-portal.md#manage-resources). Ne olursa olsun hangi uygulamanın bu kimlik bilgilerini yapılandırın, bu tüm uygulamalar ve Azure hesabınızda tüm abonelikler için geçerlidir. 

Kullanıcı düzeyinde kimlik bilgilerinizi yapılandırmak için:

1. İçinde [Azure portal](https://portal.azure.com), uygulama hizmeti tıklayın >  **&lt;any_app >** > **dağıtım kimlik bilgileri**.

    > [!NOTE]
    > Portalda dağıtım kimlik bilgileri sayfasında erişebilmeniz için önce en az bir uygulama olmalıdır. Bununla birlikte, [Azure CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az_webapp_deployment_user_set), kullanıcı düzeyinde kimlik bilgileri olan bir uygulama olmadan yapılandırabilirsiniz.

2. Kullanıcı adını ve parolasını yapılandırın ve ardından **kaydetmek**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Dağıtım kimlik bilgilerinizi ayarladıktan sonra bulabileceğiniz *Git* dağıtım kullanıcıadı uygulamanızın **genel bakış**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

ve *FTP* dağıtım kullanıcıadı uygulamanızın **özellikleri**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure kullanıcı düzeyinde dağıtım parolanızı göstermez. Parolanızı unutursanız, alınamıyor. Ancak, bu bölümdeki adımları izleyerek kimlik bilgilerinizi sıfırlayabilirsiniz.
>
>  

## <a name="appscope"></a>Alma ve uygulama düzeyinde kimlik bilgilerini sıfırlama
XML dosyasında depolanan App Service'te her uygulama için uygulama düzeyinde kimlik bilgileri yayımlama profili.

Uygulama düzeyinde kimlik bilgilerini almak için:

1. İçinde [Azure portal](https://portal.azure.com), uygulama hizmeti tıklayın >  **&lt;any_app >** > **genel bakış**.

2. Tıklatın **... Daha fazla** > **Get yayımlama profili**, indirme başlatılması için ve bir. PublishSettings dosyası.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Açık. PublishSettings dosyasını ve Bul `<publishProfile>` öznitelik etiketiyle `publishMethod="FTP"`. Daha sonra kendi `userName` ve `password` öznitelikleri.
Bu, uygulama düzeyinde kimlik bilgileridir.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Kullanıcı düzeyinde kimlik bilgileri, FTP dağıtım kullanıcı adı biçiminde benzer `<app_name>\<username>`, ve Git dağıtım kullanıcı adı yalnızca `<username>` önceki olmadan `<app_name>\`.

Uygulama düzeyinde kimlik bilgilerini sıfırlamak için:

1. İçinde [Azure portal](https://portal.azure.com), uygulama hizmeti tıklayın >  **&lt;any_app >** > **genel bakış**.

2. Tıklatın **... Daha fazla** > **sıfırlama yayımlama profili**. Tıklatın **Evet** sıfırlama onaylamak için.

    Sıfırlama eylemi herhangi önceden indirilmiş geçersiz kılar. PublishSettings dosyaları.

## <a name="next-steps"></a>Sonraki adımlar

Bu kimlik bilgileri, uygulamanızdan dağıtmak için nasıl kullanılacağını öğrenin [yerel Git](app-service-deploy-local-git.md) veya kullanarak [FTP/S](app-service-deploy-ftp.md).
