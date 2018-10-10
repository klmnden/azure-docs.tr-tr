---
title: Uygulamanızı FTP/S kullanarak Azure App Service'e dağıtma | Microsoft Docs
description: Uygulamanızı FTP veya FTPS kullanarak Azure App Service'e dağıtma konusunda bilgi edinin.
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
ms.openlocfilehash: f82a01f3a2b88e66988717eb008a65c8f2aecf28
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900334"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Uygulamanızı FTP/S kullanarak Azure App Service'e dağıtma

Bu makalede, web uygulaması, mobil uygulama arka ucuna veya API uygulamasına dağıtmak için FTP veya FTPS kullanmayı gösterilmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Uygulamanız için FTP/S uç nokta zaten etkin değil. FTP/S dağıtımını etkinleştirmek yapılandırma gereklidir.

## <a name="open-ftp-dashboard"></a>FTP panoyu Aç

İçinde [Azure portalında](https://portal.azure.com), uygulamanızın açın [kaynak sayfası](../azure-resource-manager/resource-group-portal.md#manage-resources).

FTP panoyu açmak için **sürekli teslim (Önizleme)** > **FTP** > **Pano**.

![FTP panoyu Aç](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>FTP bağlantı bilgilerini alma

FTP Panoda tıklayın **kopyalama** FTPS uç noktası ve uygulama kimlik bilgilerini kopyalanacak.

![FTP bilgilerini Kopyala](./media/app-service-deploy-ftp/ftp-dashboard.png)

Kullanmanız önerilir **uygulama kimlik** her uygulama için benzersiz olduğundan, uygulamanıza dağıtılacak. Ancak, tıklarsanız **kullanıcı kimlik bilgilerini**, aboneliğinizdeki tüm App Service uygulamaları için FTP/S oturum açma için kullanabileceğiniz bir kullanıcı düzeyinde kimlik ayarlayabilirsiniz.

## <a name="deploy-files-to-azure"></a>Dosyalar Azure'da dağıtma

1. FTP istemcinizden (örneğin, [Visual Studio](https://www.visualstudio.com/vs/community/) veya [FileZilla](https://filezilla-project.org/download.php?type=client)), uygulamanızı bağlamak için topladığınız bağlantı bilgilerini kullanın.
3. Dosyalarınızı ve ilgili dizin yapılarını kopyalama [ **/site/wwwroot** dizin](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) azure'da (veya **/site/wwwroot/App_Data/iş/** WebJobs için dizin).
4. Uygulamanın düzgün çalıştığından doğrulamak için uygulamanızın URL'sine gidin. 

> [!NOTE] 
> Farklı [Git tabanlı dağıtımlar](app-service-deploy-local-git.md), FTP dağıtımı, aşağıdaki dağıtım otomasyonları desteklemez: 
>
> - bağımlılık (NuGet, NPM, PIP ve Oluşturucusu otomasyonları gibi) geri yükler.
> - .NET ikili dosyalarının derlenmesini
> - web.config nesil (burada bir [Node.js örnek](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Bu gerekli dosyalar yerel makinenizde el ile oluşturun ve bunları birlikte uygulamanızın dağıtabilirsiniz.
>
>

## <a name="enforce-ftps"></a>FTPS zorla

Gelişmiş güvenlik için FTP SSL üzerinden yalnızca izin vermeniz. FTP dağıtım kullanmazsanız, ayrıca hem FTP ve FTPS devre dışı bırakabilirsiniz.

Uygulamanızın kaynak sayfasında [Azure portalında](https://portal.azure.com)seçin **uygulama ayarları** sol gezinti bölmesinde.

Şifrelenmemiş FTP devre dışı bırakmak için seçin **yalnızca FTPS**. FTP ve FTPS hem tamamen devre dışı bırakmak için seçin **devre dışı**. İşlemi tamamladıktan sonra **Kaydet**’e tıklayın. Kullanıyorsanız **yalnızca FTPS** giderek TLS 1.1 veya üzeri zorlamalıdır **SSL ayarları** web uygulamanızın dikey penceresinde. TLS 1.0 ile desteklenmiyor **yalnızca FTPS**.

![FTP/S devre dışı bırak](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

FTP kullanarak dağıtım için [Azure CLI](/cli/azure), bkz: [bir web uygulaması oluşturma ve dağıtma (Azure CLI) FTP ile dosyaları](./scripts/app-service-cli-deploy-ftp.md).

FTP kullanarak dağıtım için [Azure PowerShell](/cli/azure), bkz: [FTP (PowerShell) kullanarak bir web uygulamasına dosya yükleme](./scripts/app-service-powershell-deploy-ftp.md).

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-ftp-deployment"></a>FTP dağıtım sorunlarını giderme

- [FTP dağıtım'ilgili sorunları nasıl giderebilirim?](#how-can-i-troubleshoot-ftp-deployment)
- [FTP mümkün değil ve kodum yayımlama bildirimi. Sorunu çözmek ne?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Nasıl Azure App Service'te FTP'yi Pasif modu bağlanabilirim?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>FTP dağıtım'ilgili sorunları nasıl giderebilirim?

FTP dağıtım sorunlarını giderme için ilk adım, bir çalışma zamanı uygulama çıkıştan bir dağıtım sorunu izole olduğu.

Bir dağıtım sorunu genellikle yok veya yanlış dosyaları, uygulamaya dağıtılan sonuçlanır. FTP dağıtım araştırma veya bir alternatif dağıtım yolu (örneğin, kaynak denetimi) seçerek giderebilirsiniz.

Bir çalışma zamanı sorununu genellikle, uygulama, ancak yanlış uygulama davranışı için dağıtılan dosyaların doğru ortaklık kümesi sonuçlanır. Çalışma zamanında kod davranışı odaklanan ve belirli hata yollarını araştırma giderebilirsiniz.

Dağıtım veya çalışma zamanı sorunu belirlemek için bkz: [çalışma zamanı sorunlarına ve dağıtım](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>FTP mümkün değil ve kodum yayımlama bildirimi. Sorunu çözmek ne?
Doğru ana bilgisayar adı girdiğiniz denetleyin ve [kimlik bilgilerini](#step-1--set-deployment-credentials). Ayrıca, makinenizde aşağıdaki FTP bağlantı noktalarını güvenlik duvarı tarafından engellenmediğinden kontrol edin:

- FTP denetim bağlantı noktası: 21
- FTP veri bağlantı noktası: 989, 10001-10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Nasıl Azure App Service'te FTP'yi Pasif modu bağlanabilirim?
Azure App Service, hem etkin ve Pasif modu bağlanmayı destekler. (İşletim sistemi veya bir giriş parçası veya iş ağı olarak) bir güvenlik duvarının arkasındaki dağıtım makinelerinizi genellikle olduğundan Pasif modu tercih edilir. Bkz: bir [örnek için WinSCP belgelerinde](https://winscp.net/docs/ui_login_connection). 

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için deneyin [Git ile azure'a dağıtma](app-service-deploy-local-git.md). Git tabanlı azure'a dağıtım, sürüm denetimi, paket geri yükleme, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Daha Fazla Kaynak

* [Azure App Service'e dağıtım kimlik bilgileri](app-service-deployment-credentials.md)
