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
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: e3ac2f2156719ad975049b0c2b4cbca81d88e779
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>FTP/S kullanarak Azure App Service için uygulamanızı dağıtma

Bu makalede, FTP veya FTPS web uygulaması, mobil uygulama arka ucu veya API uygulamasına dağıtmak için nasıl kullanılacağı gösterilmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Uygulamanız için FTP/S uç nokta zaten etkindir. Hiçbir FTP/S dağıtımını etkinleştirmek gerekli yapılandırmadır.

> [!IMPORTANT]
> Biz, sürekli olarak Microsoft Azure Platform güvenliği iyileştirmek için gereken adımları sürüyor. Devam eden bu çalışmaların bir parçası olarak Web uygulamaları yükseltmesini Almanya merkezi ve Almanya Kuzeydoğu bölgeler için planlanmış bir görevdir. Bu sırasında Web Apps dağıtımları için düz metin FTP Protokolü kullanımını devre dışı bırakma. Müşterilerimiz için Bizim önerimiz, dağıtımları için FTPS için geçmektir. Hizmetinize herhangi kesinti 9/5 için planlandığı bu yükseltme sırasında bekliyoruz değil. Değer veriyoruz bu çaba destekler.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>1. adım: dağıtım kimlik bilgilerini ayarlama

Uygulamanız için FTP sunucusuna erişmek için önce dağıtım kimlik bilgileri gerekir. 

Dağıtım kimlik bilgilerinizi sıfırlayın veya ayarlamak için bkz: [Azure uygulama hizmeti dağıtım kimlik bilgileri](app-service-deployment-credentials.md). Bu öğretici, kullanıcı düzeyinde kimlik bilgilerinin kullanımını gösterir.

## <a name="step-2-get-ftp-connection-information"></a>2. adım: FTP bağlantı bilgileri alma

1. İçinde [Azure portal](https://portal.azure.com), uygulamanızın açmak [kaynak dikey](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Seçin **genel bakış** soldaki menüde sonra değerlerini Not **FTP/dağıtım kullanıcı**, **FTP konak adı**, ve **FTPS konak adı**. 

    ![FTP bağlantı bilgileri](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > **FTP/dağıtım kullanıcısı** FTP sunucusu için uygun içeriği sağlamak için uygulama adı dahil olmak üzere Azure Portal tarafından görüntülenen kullanıcı değeri.
    > Seçtiğinizde, aynı bilgileri bulabilirsiniz **özellikleri** soldaki menüde. 
    >
    > Ayrıca, dağıtım parola hiçbir zaman gösterilir. Dağıtım parolanızı unutursanız, geri dönüp [1. adım](#step1) ve dağıtım parolanızı sıfırlayın.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>3. adım: dosyaları Azure'a dağıtma

1. FTP istemcisinden ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), vb.), topladığınız uygulamanıza bağlanmak için bağlantı bilgisini kullanın.
3. Dosyalarınızı ve bunların ilgili dizin yapısını kopyalamak [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) Azure (veya **/site/wwwroot/App_Data/işleri/** Web işleri için dizin).
4. Uygulamanın düzgün çalıştığından doğrulamak için uygulamanızın URL'sine gidin. 

> [!NOTE] 
> Farklı [Git tabanlı dağıtımlar](app-service-deploy-local-git.md), FTP dağıtım, aşağıdaki dağıtım otomasyonlara desteklemez: 
>
> - bağımlılık geri yükleme (örneğin, NuGet, NPM, PIP ve oluşturucu otomasyonlara)
> - .NET ikili dosyalarının derlenmesini
> - web.config nesil (burada bir [Node.js örnek](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Geri yükleme gerekir, yapı ve bu gerekli dosyalar yerel makinenizde el ile oluşturmak ve bunları uygulamanızı birlikte dağıtabilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için deneyin [Git ile azure'a dağıtma](app-service-deploy-local-git.md). Azure Git tabanlı dağıtımına sürüm denetimi, paket geri yüklemesi, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Daha Fazla Kaynak

* [Azure uygulama hizmeti dağıtım kimlik bilgileri](app-service-deploy-ftp.md)
