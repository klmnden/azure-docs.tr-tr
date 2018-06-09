---
title: Azure App Service için Yerel Git Dağıtımı
description: Azure uygulama hizmeti için yerel Git dağıtımı etkinleştirmeyi öğrenin.
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
ms.date: 06/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: a614dadae40fcfc28eba85e5943f60a38653224b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233912"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Azure App Service için Yerel Git Dağıtımı

Nasıl yapılır bu kılavuz size nasıl kodunuzu dağıtılacağı gösterir [Azure App Service](app-service-web-overview.md) yerel bilgisayarınızdaki bir Git deposundan.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları takip etmek için:

* [Git’i yükleyin](http://www.git-scm.com/downloads).
* Yerel bir Git deposu dağıtmak istediğiniz kod ile koruyun.

İzlemek için bir örnek deposu kullanmak için yerel terminal penceresinde aşağıdaki komutu çalıştırın:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git-with-kudu-builds"></a>Yerel Git ile Kudu derlemeleri dağıtma

Uygulamanızın Kudu yapı sunucusuyla yerel Git dağıtımı etkinleştirmek için en kolay yolu, bulut kabuğunu kullanmaktır.

### <a name="create-a-deployment-user"></a>Dağıtım kullanıcısı oluşturma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Yerel Git Kudu ile etkinleştir

Uygulamanızın Kudu yapı sunucusuyla yerel Git dağıtımı etkinleştirmek için çalıştırın [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) bulut Kabuğu'nda.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Bunun yerine Git özellikli bir uygulama oluşturmak için çalıştırın [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) ile bulut kabuğunda `--deployment-local-git` parametresi.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

`az webapp create` Komutu size şuna benzer bir aşağıdaki çıktı:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Projenizi dağıtma

_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. Değiştir  _\<url >_ dan aldığınız Git uzak URL'si ile [etkinleştirmek Git uygulamanız için](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

MSBuild ASP.NET gibi bir çıkış çalışma zamanı özel Otomasyon görebilirsiniz `npm install` Node.js için ve `pip install` Python için. 

İçerik dağıtıldığını doğrulamak için uygulamanızın göz atın.

## <a name="deploy-from-local-git-with-vsts-builds"></a>Yerel Git VSTS derlemeleri ile dağıtma

> [!NOTE]
> App Service'nın gerekli yapı oluşturmak ve tanımları VSTS hesabınızda yayın rolü, Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizde.
>

Uygulamanızın Kudu yapı sunucusuyla yerel Git dağıtımı etkinleştirmek için uygulamanızı gidin [Azure portal](https://portal.azure.com).

Uygulama sayfanızı sol gezinti bölmesinde tıklatın **Dağıtım Merkezi** > **yerel Git** > **devam**. 

![](media/app-service-deploy-local-git/portal-enable.png)

Tıklatın **VSTS kesintisiz teslim** > **devam**.

![](media/app-service-deploy-local-git/vsts-build-server.png)

İçinde **yapılandırma** sayfasında, yeni bir VSTS hesabı yapılandırın veya var olan bir hesap belirtin. Tamamlandığında tıklatarak **devam**.

> [!NOTE]
> Listede olmayan VSTS hesabınız kullanmak istiyorsanız, gerek [Azure aboneliğinize VSTS hesabı bağlamak](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

İçinde **Test** sayfasında, yük testleri etkinleştirin ve ardından isteyip istemediğinizi seçin **devam**.

Bağlı olarak [fiyatlandırma katmanı](/pricing/details/app-service/plans/) de görebilirsiniz, uygulama hizmeti planını bir **hazırlama Dağıt** sayfası. Dağıtım yuvaları etkinleştirin ve ardından isteyip istemediğinizi seçin **devam**.

İçinde **Özet** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

VSTS hesabı hazır olması için birkaç dakika sürer. Hazır olduğunda, dağıtım merkezine Git deposu URL'sini kopyalayın.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. Değiştir  _\<url >_ son adımda aldığınız URL'si.

```bash
git remote add vsts <url>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git kimlik bilgileri Yöneticisi tarafından istendiğinde, visualstudio.com kullanıcıyla oturum oturum açın. Ek kimlik doğrulama yöntemleri için bkz: [VSTS kimlik doğrulaması genel bakış](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Dağıtım tamamlandıktan sonra yapı ilerleme bulabilirsiniz `https://<vsts_account>.visualstudio.com/<project_name>/_build` ve dağıtımının ilerleme durumunu adresindeki `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

İçerik dağıtıldığını doğrulamak için uygulamanızın göz atın.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshooting-kudu-deployment"></a>Kudu dağıtım sorunlarını giderme

Yaygın hatalar veya sorunlar Git Azure App Service uygulama yayımlamak için kullanırken şunlardır:

---
**Belirti**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Neden**: uygulama hazır ve çalışır durumda değilse bu hata oluşabilir.

**Çözümleme**: Azure Portal'da uygulamayı başlatın. Git dağıtımı, Web uygulaması durduğunda kullanılamaz.

---
**Belirti**: `Couldn't resolve host 'hostname'`

**Neden**: 'azure' uzak oluştururken girilen adres bilgilerini doğru değilse bu hata oluşabilir.

**Çözümleme**: kullanım `git remote -v` ilişkili URL ile birlikte tüm uzaktan kumandalar listelemek için komutu. 'Azure' uzak URL'sini doğru olduğundan emin olun. Gerekirse, kaldırın ve doğru URL'yi kullanarak bu uzaktan erişim oluşturun.

---
**Belirti**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Neden**: dal sırasında belirtmezseniz, bu hata oluşabilir `git push`, ya da size ayarlamadıysanız `push.default` değeri `.gitconfig`.

**Çözümleme**: çalıştırmak `git push` yeniden ana dala belirtme. Örneğin:

```bash
git push azure master
```

---
**Belirti**: `src refspec [branchname] does not match any.`

**Neden**: ana dışında bir dal için 'azure' uzak itme çalışırsanız, bu hata oluşabilir.

**Çözümleme**: çalıştırmak `git push` yeniden ana dala belirtme. Örneğin:

```bash
git push azure master
```

---
**Belirti**: `RPC failed; result=22, HTTP code = 5xx.`

**Neden**: HTTPS üzerinden büyük git deposu itme çalışırsanız, bu hata oluşabilir.

**Çözümleme**: postBuffer büyütmeniz için yerel makinede git yapılandırmasını değiştirme

```bash
git config --global http.postBuffer 524288000
```

---
**Belirti**: `Error - Changes committed to remote repository but your web app not updated.`

**Neden**: bir Node.js uygulaması ile dağıtırsanız, bu hata oluşabilir bir _package.json_ ek gerekli modüllerini belirten dosyası.

**Çözümleme**: 'npm hata!' ile ek iletiler önce bu hata oturum açmış olmanız ve ek bağlam hatada sağlayabilir. Bu hata ve karşılık gelen 'npm hata!' nedenlerini aşağıdaki bilinmektedir İleti:

* **Hatalı biçimlendirilmiş package.json dosyası**: npm hata! Bağımlılıklar okunamadı.
* **İkili bir dağıtım için Windows olmayan yerel modül**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OR
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Ek Kaynaklar

* [Proje Kudu belgeleri](https://github.com/projectkudu/kudu/wiki)
* [Azure uygulama hizmeti için sürekli dağıtım](app-service-continuous-deployment.md)
* [Örnek: Web uygulaması oluşturma ve yerel bir Git deposu (Azure CLI) koddan dağıtma](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Örnek: Web uygulaması oluşturma ve yerel bir Git deposu (PowerShell) koddan dağıtma](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
