---
title: Dağıtım kimlik bilgileri - Azure App Service'ı yapılandırma | Microsoft Docs
description: Azure App Service dağıtım kimlik bilgilerini kullanmayı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnoc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/10/2019
ms.author: cephalin
ms.reviewer: byvinyal
ms.custom: seodec18
ms.openlocfilehash: 65e5d6bacc67c64fa21268a853dc9c9d9b447da7
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617177"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Azure App Service için dağıtım kimlik bilgilerini yapılandırma
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) iki tür kimlik bilgilerini destekler [yerel Git dağıtımı](deploy-local-git.md) ve [FTP/S dağıtım](deploy-ftp.md). Bu kimlik bilgilerinin Azure Active Directory kimlik bilgilerinizi aynı değildir.

* **Kullanıcı düzeyinde kimlik**: tek tüm Azure hesabı için kimlik bilgileri kümesi. Azure hesabı için erişim iznine sahip olduğu herhangi bir abonelikte, herhangi bir uygulama için App Service'e dağıtmak için kullanılabilir. GUI portalında ortaya varsayılan kümesi olan (gibi **genel bakış** ve **özellikleri** uygulamanın [kaynak sayfası](../azure-resource-manager/manage-resources-portal.md#manage-resources)). Bir kullanıcıya rol tabanlı erişim denetimi (RBAC) veya coadmin izinler aracılığıyla uygulama erişimi verilir, bu kullanıcı erişimi iptal kadar kendi kullanıcı düzeyi kimlik bilgilerini kullanabilirsiniz. Bu kimlik bilgileri, diğer Azure kullanıcıları ile paylaşmayın.

* **Uygulama düzeyinde kimlik**: tek her uygulama için kimlik bilgileri kümesi. Yalnızca bu uygulamasına dağıtmak için kullanılabilir. Her uygulama için kimlik bilgileri uygulama oluşturma sırasında otomatik olarak oluşturulur. El ile yapılandırılmış, ancak sıfırlanabilir. Bir kullanıcı (RBAC) aracılığıyla uygulama düzeyinde kimlik bilgilerine erişim izni verilecek kullanıcı katkıda bulunan olması gerekir veya uygulamada daha yüksek. Okuyucular, yayımlama izin verilmez ve bu kimlik bilgilerine erişemez.

## <a name="userscope"></a>Ayarlayın ve kullanıcı düzeyi kimlik bilgilerini sıfırlama

Bir uygulamanın kullanıcı düzeyinde kimlik bilgilerinizi yapılandırabileceğiniz [kaynak sayfası](../azure-resource-manager/manage-resources-portal.md#manage-resources). Bakılmaksızın hangi uygulamanın bu kimlik bilgilerini yapılandırın, bunu tüm uygulamalar ve Azure hesabınızda tüm abonelikler için geçerlidir. 

Kullanıcı düzeyinde kimlik bilgilerinizi yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), soldaki menüden **uygulama hizmetleri** >  **&lt;any_app >**  > **dağıtım Merkezi** > **dağıtım kimlik bilgileri**.

    Portalda dağıtım kimlik bilgileri sayfasında erişebilmeniz için önce en az bir uygulama olmalıdır. Bununla birlikte, [Azure CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set), mevcut bir uygulamayı olmadan kullanıcı düzeyinde kimlik yapılandırabilirsiniz.

2. Tıklayın **kullanıcı kimlik bilgilerini**, kullanıcı adı ve parola yapılandırın ve ardından **kayıt kimlik bilgileri**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Dağıtım kimlik bilgilerinizi ayarladıktan sonra bulabilirsiniz *Git* uygulamanızın dağıtım kullanıcı adı **genel bakış**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

ve *FTP* uygulamanızın dağıtım kullanıcı adı **özellikleri**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure kullanıcı düzeyinde dağıtım parolanızı göstermez. Parolanızı unutursanız, bu bölümdeki adımları izleyerek kimlik bilgilerinizi sıfırlayabilirsiniz.
>
>  

## <a name="use-user-level-credentials-with-ftpftps"></a>FTP/FTPS ile kullanıcı düzeyi kimlik bilgilerini kullanın

Kullanıcı düzeyinde kimlik requirers şu biçimde bir kullanıcı adı kullanarak bir FTP/FTPS uç nokta kimlik doğrulama işlemi: `<app-name>\<user-name>`

Kullanıcı düzeyinde kimlik, kullanıcı ve belirli bir kaynağa bağlı olduğundan, kullanıcı adı oturum açma eylemi doğru uygulama uç noktasına yönlendirmek için şu biçimde olmalıdır.

## <a name="appscope"></a>Alma ve uygulama düzeyinde kimlik bilgilerini sıfırlama
Uygulama düzeyinde kimlik bilgilerini almak için:

1. İçinde [Azure portalında](https://portal.azure.com), soldaki menüden **uygulama hizmetleri** >  **&lt;any_app >**  > **dağıtım Merkezi** > **dağıtım kimlik bilgileri**.

2. Tıklayın **uygulama kimlik**, tıklatıp **kopyalama** kullanıcı adı veya parola kopyalamak için bağlantı.

    ![](./media/app-service-deployment-credentials/deployment_credentials_app_level.png)

Uygulama düzeyinde kimlik bilgilerini sıfırlamak için tıklayın **kimlik bilgilerini sıfırlama** aynı iletişim.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızdan dağıtmak için bu kimlik bilgilerini kullanmak nasıl kaydolacağınızı [yerel Git](deploy-local-git.md) veya bu adı kullanıyor [FTP/S](deploy-ftp.md).
