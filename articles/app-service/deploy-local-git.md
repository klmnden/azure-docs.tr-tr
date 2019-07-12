---
title: Yerel Git deposundan - Azure App Service'e dağıtın
description: Azure App Service için yerel Git dağıtımı nasıl etkinleştireceğinizi öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: dariagrigoriu;cephalin
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: fa961e43408fed014be2266c14c7972d39435b97
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836869"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Azure uygulama Hizmeti'nde yerel Git dağıtımı

Bu nasıl yapılır kılavuzunda uygulamanızı dağıtma işlemi gösterilmektedir [Azure App Service](overview.md) bir Git deposundan yerel bilgisayarınızdaki.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır Kılavuzu'ndaki adımları izlemek için:

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Git’i yükleyin](https://www.git-scm.com/downloads).
  
- Dağıtmak istediğiniz kodu ile yerel bir Git deposunda var. Örnek depoyu indirmek için yerel terminal penceresinde aşağıdaki komutu çalıştırın:
  
  ```bash
  git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
  ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-build-server"></a>Kudu derleme sunucusu ile dağıtma

App Service Kudu derleme sunucusu ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için en kolay yolu, Azure Cloud Shell kullanmaktır. 

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="get-the-deployment-url"></a>Dağıtım URL'sini alma

Mevcut bir uygulamaya yönelik yerel Git dağıtımını etkinleştirmek için bir URL almak için çalıştırın [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) Cloud shell'de. Değiştirin \<-adı > ve \<grup-adı > ile uygulamanız ve onun Azure kaynak grubu adları.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```

Veya yeni bir Git özellikli uygulama oluşturmak için çalıştırılması [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) ile Cloud shell'de `--deployment-local-git` parametresi. Değiştirin \<-adı >, \<grup-adı >, ve \<planı adı > için yeni Git uygulamanızı ve Azure kaynak grubu, Azure App Service planı adları ile.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

Her iki komutu gibi bir URL döndürür: `https://<deployment-username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Uygulamanızı bir sonraki adımda dağıtmak için bu URL'yi kullanın.

Bu hesap düzeyinde URL'yi kullanmak yerine, uygulama düzeyinde kimlik bilgilerini kullanarak, yerel Git da etkinleştirebilirsiniz. Azure App Service, her uygulama için bu kimlik bilgileri otomatik olarak oluşturur. 

Cloud Shell'de aşağıdaki komutu çalıştırarak uygulama kimlik bilgilerini alın. Değiştirin \<-adı > ve \<grup-adı > Uygulama adı ve Azure kaynak grubu adı.

```azurecli-interactive
az webapp deployment list-publishing-credentials --name <app-name> --resource-group <group-name> --query scmUri --output tsv
```

Uygulamanızı bir sonraki adımda dağıtmak için döndüren URL'yi kullanın.

### <a name="deploy-the-web-app"></a>Web uygulaması dağıtma

1. Yerel Git deponuza yerel bir terminal penceresi açın ve bir Azure uzak deposu ekleyin. Aşağıdaki komutta \<url > Dağıtım kullanıcıya özel URL veya önceki adımda aldığınız uygulamaya özgü URL ile.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Anında iletme Azure ile uzak `git push azure master`. 
   
1. İçinde **Git kimlik bilgileri Yöneticisi** penceresinde girin, [dağıtım kullanıcısı parolası](#configure-a-deployment-user), Azure oturum açma parolanız yerine telefonunuzla.
   
1. Çıktıyı gözden geçirin. ASP.NET, MSBuild gibi çalışma zamanı özel Otomasyon görebilirsiniz `npm install` Node.js için ve `pip install` Python için. 
   
1. Uygulamanıza içerik dağıtıldığını doğrulamak için Azure portalında göz atın.

## <a name="deploy-with-azure-pipelines-builds"></a>Azure işlem hatları yapılarla dağıtma

Hesabınızın gerekli izinlere sahip değilse, uygulamanız için yerel Git dağıtımını etkinleştirmek için Azure işlem hatlarını (Önizleme) ayarlayabilirsiniz. 

- Azure hesabınız, Azure Active Directory'ye yazma ve hizmet oluşturmak için izinleri olmalıdır. 
  
- Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizdeki rol.

- Kullanmak istediğiniz Azure DevOps projesi içinde bir yönetici olması gerekir.

Azure işlem hatları (Önizleme) ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için:

1. Azure App Service uygulama sayfanıza gidin [Azure portalında](https://portal.azure.com)seçip **Dağıtım Merkezi** soldaki menüde.
   
1. Üzerinde **Dağıtım Merkezi** sayfasında **yerel Git**ve ardından **devam**. 
   
   ![Yerel Git seçin ve ardından Devam'ı seçin](media/app-service-deploy-local-git/portal-enable.png)
   
1. Üzerinde **oluşturma sağlayıcısını** sayfasında **Azure işlem hatları (Önizleme)** ve ardından **devam**. 
   
   ![Azure işlem hatları (Önizleme) seçin ve ardından Devam'ı seçin.](media/app-service-deploy-local-git/pipeline-builds.png)

1. Üzerinde **yapılandırma** sayfasında, yeni bir Azure DevOps kuruluş yapılandırın veya mevcut bir kuruluşa belirtin ve ardından **devam**.
   
   > [!NOTE]
   > Mevcut Azure DevOps kuruluşunuz listede yoksa, Azure aboneliğinize bağlayın gerekebilir. Daha fazla bilgi için [CD yayın işlem hattınızı tanımlamak](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
   
1. App Service planınıza bağlı olarak [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/plans/), görebileceğiniz bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](deploy-staging-slots.md)ve ardından **devam**.
   
1. Üzerinde **özeti** sayfasında, ayarları gözden geçirin ve ardından **son**.
   
1. Azure işlem hattı hazır olduğunda, Gıt deposu URL'Sİ'dan kopyalama **Dağıtım Merkezi** sonraki adımda kullanmak üzere sayfa. 
   
   ![Git deposu URL'sini kopyalayın](media/app-service-deploy-local-git/vsts-repo-ready.png)

1. Yerel terminal pencerenizde, yerel Git deponuza bir Azure uzak deposu ekleyin. Komutta \<url > önceki adımda aldığınız Git deposunun URL'si ile.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Anında iletme Azure ile uzak `git push azure master`. 
   
1. Üzerinde **Git kimlik bilgileri Yöneticisi** sayfasında, oturum visualstudio.com adınızı oturum açın. Diğer kimlik doğrulama yöntemleri için bkz. [Azure DevOps Hizmetleri kimlik doğrulamasına genel bakış](/vsts/git/auth-overview?view=vsts).
   
1. Dağıtım tamamlandıktan sonra derleme ilerlemeyi görüntüleyebilirsiniz `https://<azure_devops_account>.visualstudio.com/<project_name>/_build`ve dağıtımın ilerleme durumunu, `https://<azure_devops_account>.visualstudio.com/<project_name>/_release`.
   
1. Uygulamanıza içerik dağıtıldığını doğrulamak için Azure portalında göz atın.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-deployment"></a>Dağıtım sorunlarını giderme

App Service uygulamanızı Azure'a yayımlamak için Git kullandığınızda, aşağıdaki genel hata iletileri görebilirsiniz:

|`Message`|Nedeni|Çözüm
---|---|---|
|`Unable to access '[siteURL]': Failed to connect to [scmAddress]`|Uygulamayı çalışır duruma değildir.|Azure portalında uygulamayı başlatın. Git dağıtımını web app durdurulduğunda kullanılamaz.|
|`Couldn't resolve host 'hostname'`|'Azure' uzak adres bilgilerini doğru değil.|Kullanım `git remote -v` ilişkili URL ile birlikte tüm uzaktan kumandalar listelemek için komutu. 'Azure' uzak URL'nin doğru olduğunu doğrulayın. Gerekirse, kaldırmak ve doğru URL'yi kullanarak bu uzaktan yeniden oluşturun.|
|`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`|Bir dal sırasında belirtmediğiniz `git push`, veya ayarlamasını yapmadığınızı `push.default` değerini `.gitconfig`.|Çalıştırma `git push` tekrar ana dal belirtme: `git push azure master`.|
|`src refspec [branchname] does not match any.`|Ana dışındaki bir dalı 'azure' uzak göndermeye çalıştı.|Çalıştırma `git push` tekrar ana dal belirtme: `git push azure master`.|
|`RPC failed; result=22, HTTP code = 5xx.`|Büyük git deposu HTTPS üzerinden göndermeyi denerseniz bu hata oluşabilir.|Makinede yerel git yapılandırmasını değiştirmek `postBuffer` daha büyük. Örneğin: `git config --global http.postBuffer 524288000`|
|`Error - Changes committed to remote repository but your web app not updated.`|Bir Node.js uygulaması ile dağıtılan bir _package.json_ ek gerekli modülleri belirten dosyası.|Gözden geçirme `npm ERR!` önce bu hata hatayla ilgili daha fazla bağlam için hata iletileri. Bu hata ve karşılık gelen bilinen nedenleri şunlardır `npm ERR!` iletileri:<br /><br />**Hatalı biçimlendirilmiş bir package.json dosyası**: `npm ERR! Couldn't read dependencies.`<br /><br />**Windows için yerel modül ikili dağıtım yoksa**:<br />`npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1` <br />veya <br />' npm hata! [modulename@version] önceden: \make || gmake\`|

## <a name="additional-resources"></a>Ek kaynaklar

- [Proje Kudu belgeleri](https://github.com/projectkudu/kudu/wiki)
- [Azure uygulama Hizmeti'ne sürekli dağıtım](deploy-continuous-deployment.md)
- [Örnek: Bir web uygulaması oluşturma ve (Azure CLI) yerel Git deposundan kod dağıtma](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
- [Örnek: Bir web uygulaması oluşturma ve (PowerShell) yerel Git deposundan kod dağıtma](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
