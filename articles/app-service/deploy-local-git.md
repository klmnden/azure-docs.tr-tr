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
ms.date: 06/14/2019
ms.author: dariagrigoriu;cephalin
ms.custom: seodec18
ms.openlocfilehash: e66c625c3f30580715762d2dd3f48eeaa6e548dc
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67143977"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Azure App Service için Yerel Git Dağıtımı

Bu nasıl yapılır kılavuzunda kodunuzu dağıtma işlemi gösterilmektedir [Azure App Service](overview.md) bir Git deposundan yerel bilgisayarınızdaki.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır Kılavuzu'ndaki adımları izlemek için:

* [Git’i yükleyin](https://www.git-scm.com/downloads).
* Dağıtmak istediğiniz kodu ile bir yerel Git deposu korur.

Örneği takip etmek için bir örnek depoya kullanmak için yerel terminal penceresinde aşağıdaki komutu çalıştırın:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-builds"></a>Kudu yapılarla dağıtma

Kudu derleme sunucusu ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için en kolay yolu, Cloud Shell'i kullanmaktır.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

> [!NOTE]
> Hesap düzeyinde kimlik yerine her uygulama için otomatik olarak oluşturulan uygulama düzeyinde kimlik bilgileri ile dağıtabilirsiniz.
>

### <a name="enable-local-git-with-kudu"></a>Yerel Git Kudu ile etkinleştirme

Kudu derleme sunucusu ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için çalıştıracağınız [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) Cloud shell'de.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```

Bunun yerine Git özellikli bir uygulama oluşturmak için çalıştırın [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) ile Cloud shell'de `--deployment-local-git` parametresi.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

### <a name="deploy-your-project"></a>Projenizi dağıtın

_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. Değiştirin  _\<kullanıcıadı >_ dağıtım kullanıcıdan ile [dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) ve  _\<-adı >_ uygulama adı ile [Git için uygulamanızı etkinleştirme](#enable-local-git-with-kudu).

```bash
git remote add azure https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git
```

> [!NOTE]
> Bunun yerine uygulama düzeyinde kimlik bilgileriyle dağıtmak için kimlik bilgilerini Cloud Shell'de aşağıdaki komutu çalıştırarak uygulamanıza özgü alın:
>
> ```azurecli-interactive
> az webapp deployment list-publishing-credentials -n <app-name> -g <group-name> --query scmUri --output tsv
> ```
>
> Daha sonra çalıştırmak için çıkış komutunu kullanın `git remote add azure <url>` yukarıda ister.

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde, oluşturduğunuz parolayı girdiğinizden emin olun [dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user), Azure portalında oturum açmak için kullandığınız parolayı değil.

```bash
git push azure master
```

Çalışma zamanı özel Otomasyon çıkışında, ASP.NET, MSBuild gibi görebilirsiniz `npm install` Node.js için ve `pip install` Python için. 

İçerik dağıtıldığını doğrulamak için uygulamanıza göz atın.

## <a name="deploy-with-azure-devops-builds"></a>Azure DevOps yapılarla dağıtma

> [!NOTE]
> App Service'nın Azure DevOps Hizmetleri kuruluşunuzda gerekli Azure işlem hatları oluşturmak rolü Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizdeki.
>

Kudu derleme sunucusu ile uygulamanıza yönelik yerel Git dağıtımını etkinleştirmek için uygulamanıza gidin: [Azure portalında](https://portal.azure.com).

Uygulaması sayfanızın sol gezinti bölmesinde tıklayın **Dağıtım Merkezi** > **yerel Git** > **devam**.

![](media/app-service-deploy-local-git/portal-enable.png)

Tıklayın **Azure işlem hatları (Önizleme)**  > **devam**.

![](media/app-service-deploy-local-git/pipeline-builds.png)

İçinde **yapılandırma** sayfasında, yeni bir Azure DevOps kuruluş yapılandırın veya mevcut bir kuruluşa belirtin. İşiniz bittiğinde tıklayın **devam**.

> [!NOTE]
> Listede olmayan mevcut bir Azure DevOps kuruluşa kullanmak istiyorsanız, yapmanız [Azure DevOps hizmetler kuruluşundan Azure aboneliğinize bağlayın](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Yapılandırmanıza bağlı olarak [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/plans/) de görebilirsiniz, App Service planı, bir **hazırlama Dağıt** sayfası. Dağıtım yuvalarını etkinleştirin ve ardından yüklememeyi **devam**.

İçinde **özeti** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Azure DevOps hizmetler kuruluşundan hazır olması birkaç dakika sürer. Hazır olduğunda dağıtım merkezinde Git deposu URL'sini kopyalayın.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. Değiştirin  _\<url >_ son adımda aldığınız URL.

```bash
git remote add vsts <url>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git kimlik bilgileri Yöneticisi tarafından istendiğinde, visualstudio.com kullanıcı bilgilerinizle oturum açın. Ek kimlik doğrulama yöntemleri için bkz. [Azure DevOps Hizmetleri kimlik doğrulamasına genel bakış](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Dağıtım tamamlandıktan sonra derleme ilerleme bulabilirsiniz `https://<vsts_account>.visualstudio.com/<project_name>/_build` ve dağıtımın ilerleme durumunu, `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

İçerik dağıtıldığını doğrulamak için uygulamanıza göz atın.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshooting-kudu-deployment"></a>Kudu dağıtım sorunlarını giderme

Yaygın hatalar veya sorunlar App Service uygulamanızı Azure'a yayımlamak için Git'i kullanarak şunlardır:

---
**Belirti**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Neden**: Uygulamayı çalışır duruma gelmezse, bu hata oluşabilir.

**Çözüm**: Azure portalında uygulamayı başlatın. Git dağıtımını Web App durdurulduğunda kullanılamıyor.

---
**Belirti**: `Couldn't resolve host 'hostname'`

**Neden**: 'Azure' uzaktan oluşturma sırasında girilen adres bilgilerini yanlış bu hata oluşabilir.

**Çözüm**: Kullanım `git remote -v` ilişkili URL ile birlikte tüm uzaktan kumandalar listelemek için komutu. 'Azure' uzak URL'nin doğru olduğunu doğrulayın. Gerekirse, kaldırmak ve doğru URL'yi kullanarak bu uzaktan yeniden oluşturun.

---
**Belirti**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Neden**: Bir dal sırasında belirtmezseniz, bu hata oluşabilir `git push`, veya ayarlamasını yapmadığınızı `push.default` değerini `.gitconfig`.

**Çözüm**: Çalıştırma `git push` tekrar ana dal belirtme. Örneğin:

```bash
git push azure master
```

---
**Belirti**: `src refspec [branchname] does not match any.`

**Neden**: Ana dışındaki bir dalı 'azure' uzak deposuna gönderin denerseniz bu hata oluşabilir.

**Çözüm**: Çalıştırma `git push` tekrar ana dal belirtme. Örneğin:

```bash
git push azure master
```

---
**Belirti**: `RPC failed; result=22, HTTP code = 5xx.`

**Neden**: Büyük git deposu HTTPS üzerinden göndermeyi denerseniz bu hata oluşabilir.

**Çözüm**: Yerel makinede postBuffer büyütmeniz için git yapılandırmasını değiştirme

```bash
git config --global http.postBuffer 524288000
```

---
**Belirti**: `Error - Changes committed to remote repository but your web app not updated.`

**Neden**: İle bir Node.js uygulaması dağıtırsanız, bu hata oluşabilir bir _package.json_ ek gerekli modülleri belirten dosyası.

**Çözüm**: 'Npm hata!' ile ek iletiler önce bu hata oturum açmış olmanız ve ek bağlam hatası sağlayabilir. Aşağıdaki bilinen nedenlerini bu hatayı ve karşılık gelen 'npm hata!' İleti:

* **Hatalı biçimlendirilmiş bir package.json dosyası**: npm hata! Bağımlılıkları okunamadı.
* **İkili bir dağıtım için Windows olmayan yerel modül**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OR
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Ek kaynaklar

* [Proje Kudu belgeleri](https://github.com/projectkudu/kudu/wiki)
* [Azure uygulama Hizmeti'ne sürekli dağıtım](deploy-continuous-deployment.md)
* [Örnek: Web uygulaması oluşturma ve (Azure CLI) yerel Git deposundan kod dağıtma](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Örnek: Web uygulaması oluşturma ve (PowerShell) yerel Git deposundan kod dağıtma](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
