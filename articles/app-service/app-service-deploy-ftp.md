---
title: "FTP/S kullanarak Azure App Service için uygulamanızı dağıtma | Microsoft Docs"
description: "Uygulamanızı Azure App Service'e FTP veya FTPS kullanarak dağıtmayı öğrenin."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: fcd079306a8968505349bb3f4a805f203a5c9999
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>FTP/S kullanarak Azure App Service için uygulamanızı dağıtma

Bu makalede, FTP veya FTPS web uygulaması, mobil uygulama arka ucu veya API uygulamasına dağıtmak için nasıl kullanılacağı gösterilmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Uygulamanız için FTP/S uç nokta zaten etkindir. Hiçbir FTP/S dağıtımını etkinleştirmek gerekli yapılandırmadır.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>1. adım: dağıtım kimlik bilgilerini ayarlama

Uygulamanız için FTP sunucusuna erişmek için önce dağıtım kimlik bilgileri gerekir. 

Dağıtım kimlik bilgilerinizi sıfırlayın veya ayarlamak için bkz: [Azure uygulama hizmeti dağıtım kimlik bilgileri](app-service-deployment-credentials.md). Bu öğretici, kullanıcı düzeyinde kimlik bilgilerinin kullanımını gösterir.

## <a name="step-2-get-ftp-connection-information"></a>2. adım: FTP bağlantı bilgileri alma

1. İçinde [Azure portal](https://portal.azure.com), uygulamanızın açmak [kaynak sayfası](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Seçin **genel bakış** soldaki menüde sonra değerlerini Not **FTP/dağıtım kullanıcı**, **FTP konak adı**, ve **FTPS konak adı**. 

    ![FTP bağlantı bilgileri](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > FTP sunucusu için uygun bağlamı sağlamak için **FTP/dağıtım kullanıcısı** Azure portal tarafından görüntülenen değeri uygulama adını içerir.
    > Seçtiğinizde, aynı bilgileri bulabilirsiniz **özellikleri** soldaki menüde. 
    >
    > Ayrıca, dağıtım parola hiçbir zaman gösterilir. Dağıtım parolanızı unutursanız, geri dönüp [1. adım](#step1) ve dağıtım parolanızı sıfırlayın.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>3. adım: dosyaları Azure'a dağıtma

1. FTP istemcisinden (örneğin, [Visual Studio](https://www.visualstudio.com/vs/community/) veya [FileZilla](https://filezilla-project.org/download.php?type=client)), topladığınız uygulamanıza bağlanmak için bağlantı bilgisini kullanın.
3. Dosyalarınızı ve bunların ilgili dizin yapısını kopyalamak [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) Azure (veya **/site/wwwroot/App_Data/işleri/** Web işleri için dizin).
4. Uygulamanın düzgün çalıştığından doğrulamak için uygulamanızın URL'sine gidin. 

> [!NOTE] 
> Farklı [Git tabanlı dağıtımlar](app-service-deploy-local-git.md), FTP dağıtım, aşağıdaki dağıtım otomasyonlara desteklemez: 
>
> - bağımlılık geri yükler (örneğin, NuGet, NPM, PIP ve oluşturucu otomasyonlara)
> - .NET ikili dosyalarının derlenmesini
> - web.config nesil (burada bir [Node.js örnek](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Bu gerekli dosyalar yerel makinenizde el ile oluşturmak ve bunları birlikte uygulamanızı dağıtabilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için deneyin [Git ile azure'a dağıtma](app-service-deploy-local-git.md). Azure Git tabanlı dağıtımına sürüm denetimi, paket geri yüklemesi, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Daha Fazla Kaynak

* [Azure uygulama hizmeti dağıtım kimlik bilgileri](app-service-deploy-ftp.md)
