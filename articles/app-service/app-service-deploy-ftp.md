---
title: FTP/S kullanarak Azure App Service için uygulamanızı dağıtma | Microsoft Docs
description: Uygulamanızı Azure App Service'e FTP veya FTPS kullanarak dağıtmayı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 561f317cd7afd740b83709efc8a75ed515626192
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
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
2. Seçin **genel bakış** sol gezinti bölmesinde, ardından değerlerini Not **FTP/dağıtım kullanıcı**, **FTP konak adı**, ve **FTPS konak adı**. 

    ![FTP bağlantı bilgileri](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > FTP sunucusu için uygun bağlamı sağlamak için **FTP/dağıtım kullanıcısı** Azure portal tarafından görüntülenen değeri uygulama adını içerir.
    > Seçtiğinizde, aynı bilgileri bulabilirsiniz **özellikleri** sol gezinti bölmesinde. 
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

## <a name="enforce-ftps"></a>FTPS zorla

Gelişmiş güvenlik için FTP SSL üzerinden yalnızca izin vermeniz. FTP dağıtım kullanmazsanız, ayrıca FTP ve FTPS devre dışı bırakabilirsiniz.

Uygulamanızın kaynak sayfasında [Azure portal](https://portal.azure.com)seçin **uygulama ayarları** sol gezinti bölmesinde.

Şifrelenmemiş FTP devre dışı bırakmak için seçin **yalnızca FTPS**. FTP ve FTPS tamamen devre dışı bırakmak seçin **devre dışı**. Tamamlandığında tıklatarak **kaydetmek**.

![FTP/S devre dışı bırak](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="troubleshoot-ftp-deployment"></a>FTP dağıtım sorunlarını gider

- [FTP dağıtım nasıl giderebilirim?](#how-can-i-troubleshoot-ftp-deployment)
- [I FTP mümkün değildir ve kodumu yayımlayın. Sorunu nasıl çözümlemek için?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Nasıl ı Azure App Service'te FTP Pasif modu bağlanabilir miyim?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>FTP dağıtım nasıl giderebilirim?

FTP dağıtım sorunlarını giderme için ilk adım, bir çalışma zamanı uygulama sorunu dağıtım sorundan yalıtma.

Bir dağıtım sorunu genellikle yok veya yanlış dosyaları uygulamanıza dağıtılan sonuçlanır. FTP dağıtımınızı araştırma veya diğer dağıtım yolu (örneğin, kaynak denetimi) seçerek çözülebilir.

Bir çalışma zamanı uygulama sorunu genellikle uygulama ancak yanlış uygulamanızın davranışını için Dağıtılmış dosya sağ kümesini sonuçlanır. Çalışma zamanında kod davranışına odaklanma ve belirli hata yolları inceleme çözülebilir.

Bir dağıtımı veya çalışma zamanı sorunu belirlemek için bkz: [dağıtım çalışma zamanı sorunlarına karşılaştırması](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

 
### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>I FTP mümkün değildir ve kodumu yayımlayın. Sorunu nasıl çözümlemek için?
Doğru ana bilgisayar adı girdiğiniz denetleyin ve [kimlik bilgileri](#step-1--set-deployment-credentials). Ayrıca, makinenizde aşağıdaki FTP bağlantı noktalarını güvenlik duvarı tarafından engellenmez denetleyin:

- FTP denetim bağlantı noktası: 21
- FTP veri bağlantı noktası: 989, 10001 10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Nasıl ı Azure App Service'te FTP Pasif modu bağlanabilir miyim?
Azure uygulama hizmeti aktif ve Pasif modu bağlanmayı destekler. Dağıtım makinelerinizi (işletim sistemi veya bir giriş parçası veya iş ağı olarak) bir güvenlik duvarının arkasında genellikle olduğundan Pasif modu tercih edilir. Bkz: bir [WinSCP belgelerindeki örnek](https://winscp.net/docs/ui_login_connection). 

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için deneyin [Git ile azure'a dağıtma](app-service-deploy-local-git.md). Azure Git tabanlı dağıtımına sürüm denetimi, paket geri yüklemesi, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Daha Fazla Kaynak

* [Azure uygulama hizmeti dağıtım kimlik bilgileri](app-service-deploy-ftp.md)
