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
ms.date: 03/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: 842cd6f67a04bec0ed06282bdeeea8b8a51c0667
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
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

## <a name="prepare-your-repository"></a>Deponuzda hazırlama

Depo kök projenizdeki doğru dosyalar sahip olduğundan emin olun.

| Çalışma Zamanı | Kök dizin dosyaları |
|-|-|
| ASP.NET (yalnızca Windows) | _*.sln_, _*.csproj_, veya _default.aspx_ |
| ASP.NET Çekirdeği | _*.sln_ veya _*.csproj_ |
| PHP | _PHP için index.php'dir_ |
| Ruby (yalnızca Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, veya _package.json_ başlangıç komut dosyası |
| Python (yalnızca Windows) | _\*.PY_, _requirements.txt_, veya _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, veya  _iisstart.htm_ |
| WebJobs | _\<job_name > / çalıştırın. \<uzantısı >_ altında _uygulama\_veri/işleri/sürekli_ (için sürekli Webjob'lar) veya _uygulama\_veri/işleri/tetiklenen_ (tetiklenir için Web işleri). Daha fazla bilgi için bkz: [Kudu Web işleri belgeleri](https://github.com/projectkudu/kudu/wiki/WebJobs) |
| İşlevler | Bkz: [Azure işlevleri için sürekli dağıtım](../azure-functions/functions-continuous-deployment.md#continuous-deployment-requirements). |

Dağıtımınızı özelleştirmek için dahil edebileceğiniz bir _.deployment_ depo kök dosyasında. Daha fazla bilgi için bkz: [dağıtımlarını özelleştirme](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) ve [özel dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Şunları yaptığınızdan emin olun `git commit` dağıtmak istediğiniz tüm değişiklikleri.
>
>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user.md)]

## <a name="enable-git-for-your-app"></a>Git için uygulamanızı etkinleştirme

Var olan bir uygulama hizmeti uygulaması için Git dağıtımı etkinleştirmek için çalıştırın [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) bulut Kabuğu'nda.

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

## <a name="deploy-your-project"></a>Projenizi dağıtma

_Yerel terminal penceresine_ dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. Değiştir  _\<url >_ dan aldığınız Git uzak URL'si ile [etkinleştirmek Git uygulamanız için](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```bash
git push azure master
```

MSBuild ASP.NET gibi bir çıkış çalışma zamanı özel Otomasyon görebilirsiniz `npm install` Node.js için ve `pip install` Python için. 

Dağıtım tamamlandıktan sonra uygulamanızı Azure portalında şimdi kaydı olmalıdır, `git push` içinde **dağıtım seçenekleri** sayfası.

![](./media/app-service-deploy-local-git/deployment_history.png)

İçerik dağıtıldığını doğrulamak için uygulamanızın göz atın.

## <a name="troubleshooting"></a>Sorun giderme

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
