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
ms.date: 06/05/2018
ms.author: cephalin;dariac
ms.openlocfilehash: 2ec08b45fab9987e9271c1ff3101eaf321dc84be
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234232"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>FTP/S kullanarak Azure App Service için uygulamanızı dağıtma

Bu makalede, FTP veya FTPS web uygulaması, mobil uygulama arka ucu veya API uygulamasına dağıtmak için nasıl kullanılacağı gösterilmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Uygulamanız için FTP/S uç nokta zaten etkindir. Hiçbir FTP/S dağıtımını etkinleştirmek gerekli yapılandırmadır.

## <a name="open-ftp-dashboard"></a>Açık FTP Panosu

İçinde [Azure portal](https://portal.azure.com), uygulamanızın açmak [kaynak sayfası](../azure-resource-manager/resource-group-portal.md#manage-resources).

FTP panoyu açmak için **kesintisiz teslim (Önizleme)** > **FTP** > **Pano**.

![Açık FTP Panosu](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>FTP bağlantı bilgileri alma

FTP panosunda tıklatın **kopyalama** FTPS uç noktası ve uygulama kimlik bilgilerini kopyalamak için.

![FTP bilgilerini Kopyala](./media/app-service-deploy-ftp/ftp-dashboard.png)

Kullanmanız önerilir **uygulama kimlik bilgilerini** her uygulama için benzersiz olduğundan uygulamanıza dağıtmak için. Ancak, tıklatırsanız **kullanıcı kimlik bilgilerini**, aboneliğinizde tüm App Service uygulamalarının FTP/S oturum açma için kullanabilmeniz için kullanıcı düzeyinde kimlik bilgilerini ayarlayın.

## <a name="deploy-files-to-azure"></a>Dosyaları Azure'a dağıtma

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

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

FTP kullanarak dağıtımı için [Azure CLI](/cli/azure), bkz: [bir web uygulaması oluşturma ve dosyaları FTP (Azure CLI) ile dağıtma](./scripts/app-service-cli-deploy-ftp.md).

FTP kullanarak dağıtımı için [Azure PowerShell](/cli/azure), bkz: [FTP (PowerShell) kullanarak bir web uygulaması dosyaları karşıya](./scripts/app-service-powershell-deploy-ftp.md).

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-ftp-deployment"></a>FTP dağıtım sorunlarını gider

- [FTP dağıtım nasıl giderebilirim?](#how-can-i-troubleshoot-ftp-deployment)
- [I FTP mümkün değildir ve kodumu yayımlayın. Sorunu nasıl çözümlemek için?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Nasıl ı Azure App Service'te FTP Pasif modu bağlanabilir miyim?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>FTP dağıtım nasıl giderebilirim?

FTP dağıtım sorunlarını giderme için ilk adım, bir çalışma zamanı uygulama sorunu dağıtım sorundan yalıtma.

Bir dağıtım sorunu genellikle yok veya yanlış dosyaları uygulamanıza dağıtılan sonuçlanır. FTP dağıtımınızı araştırma veya diğer dağıtım yolu (örneğin, kaynak denetimi) seçerek giderebilirsiniz.

Bir çalışma zamanı uygulama sorunu genellikle uygulama ancak yanlış uygulamanızın davranışını için Dağıtılmış dosya sağ kümesini sonuçlanır. Çalışma zamanında kod davranışına odaklanma ve belirli hata yolları inceleme giderebilirsiniz.

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
