---
title: "Azure App Service için Yerel Git Dağıtımı"
description: "Azure uygulama hizmeti için yerel Git dağıtımı etkinleştirmeyi öğrenin."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: cfowler
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 948c54a2e9be2260d0a7d2cce31b67ffbbd23d03
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Azure App Service için Yerel Git Dağıtımı

Bu öğretici, uygulamanızın dağıtılacağı gösterilmiştir [Azure Web Apps](app-service-web-overview.md) , yerel bilgisayarınızda bir Git deposundan. Uygulama hizmetini destekleyen bu yaklaşımı **yerel Git** dağıtım seçeneği [Azure portal].

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Git. İkili yükleme indirebilirsiniz [burada](http://www.git-scm.com/downloads).
* Git temel bilgiye.
* Bir Microsoft Azure hesabı. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service'i kullanmaya başlamak istiyorsanız, Git [App Service'i deneyin](https://azure.microsoft.com/try/app-service/), burada hemen bir kısa süreli başlangıç uygulaması App Service'te oluşturabilirsiniz. Kredi kartı ve taahhüt gerekmez.
>

## <a name="Step1"></a>1. adım: yerel bir havuz oluşturma

Yeni bir Git deposu oluşturmak için aşağıdaki görevleri gerçekleştirin.

1. Bir komut satırı aracı gibi başlatın **Git Bash** (Windows) veya **Bash** (UNIX kabuğu). OS X sistemlerinde, komut satırı aracılığıyla erişebilirsiniz **Terminal** uygulama.
1. Dağıtmak için içeriğin bulunduğu olduğu dizine gidin.
1. Yeni bir Git deposunu başlatmak için aşağıdaki komutu kullanın:

  ```bash
  git init
  ```

## <a name="Step2"></a>2. adım: içeriğinizi Yürüt

Uygulama hizmeti çeşitli programlama dillerini içinde oluşturulan uygulamaları destekler.

1. Deponuzda içerik içermiyorsa, bir statik .html dosyası aşağıdaki şekilde ekleyin; ya da bu adımı atlayın:
   * Bir metin düzenleyicisi kullanarak adlı yeni bir dosya oluşturun **index.html** Git deposu kökündeki
   * Aşağıdaki metni index.html içeriğini dosya ve kaydedin gibi ekleyin: *Hello Git!*
1. Komut satırından Git deponuzu kök altında olduğundan emin olun. Ardından depoya dosyaları eklemek için aşağıdaki komutu kullanın:

        git add -A 
1. Ardından, aşağıdaki komutu kullanarak depoya değişiklikleri uygulayın:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>3. adım: uygulama hizmeti uygulama havuzu etkinleştirme

App Service uygulamanız için bir Git deposu etkinleştirmek için aşağıdaki adımları gerçekleştirin.

1. [Azure portal]’da oturum açın.
1. App Service uygulamanızın Görüntüle'yi tıklatın **ayarlar > dağıtım kaynağı**. Tıklatın **Kaynak Seç**, ardından **yerel Git deposu**ve ardından **Tamam**.

    ![Yerel 'Git' Havuzu](./media/app-service-deploy-local-git/local_git_selection.png)

1. Bu ilk zaman ayarınız Azure deposunu ise, bunun için oturum açma kimlik bilgileri oluşturmanız gerekir. Bunları yerel Git deposundan Azure depo ve anında iletme değişiklikleri kaydetmek için kullanın. Web uygulamanızın görünümünden tıklatın **ayarlar > Dağıtım kimlik bilgileri**, dağıtım kullanıcı adı ve parola yapılandırın. İşiniz bittiğinde tıklatın **kaydetmek**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Adım 4: projenizi dağıtma

Yerel Git kullanarak uygulama hizmeti uygulamanızı yayımlamak için aşağıdaki adımları kullanın.

1. Azure portalında Web uygulamanızın Görüntüle'yi tıklatın **ayarlar > Özellikler** için **Git URL'si**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Git URL'si** yerel depodan dağıtmak için uzaktan başvurudur. Sonraki adımlarda bu URL'yi kullanın.
1. Komut satırını kullanarak yerel Git deponuzu kök dizininde olduğundan emin olun.
1. Kullanım `git remote` listelenen uzak başvuru eklemek için **Git URL'si** adım 1. Komutunuzu aşağıdakine benzer:

    ```bash
    git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git
    ```

   > [!NOTE]
   > **Uzak** komut uzak deponuza adlandırılmış bir başvuru ekler. Bu örnekte, web uygulamanızın depo için 'azure' adlı bir başvuru oluşturur.
   >

1. İçeriğinizi kullanarak yeni App Service'e anında **azure** uzak oluşturuldu.

    ```bash
    git push azure master
    ```

    Azure portalında dağıtım kimlik bilgilerinizi sıfırladığınızda daha önce oluşturduğunuz parola istenir. (Parolanızı yazarken Gitbash'i konsoluna yıldızlar göstermez unutmayın) parolasını girin. 
1. Azure portalında uygulamanıza geri dönün. En son gönderme, bir günlük girişi görüntülenmelidir **dağıtımları** görünümü.

    ![](./media/app-service-deploy-local-git/deployment_history.png)

1. Tıklatın **Gözat** düğmesi sayfanın üst kısmındaki içeriği dağıtılan doğrulamak için web uygulaması.

## <a name="Step5"></a>Sorun giderme

Git Azure App Service uygulama yayımlamak için kullanırken sık karşılaşılan sorunları veya hatalar şunlardır:

---
**Belirti**:`Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Neden**: uygulama hazır ve çalışır değilse bu hata oluşabilir.

**Çözümleme**: Azure Portal'da uygulamayı başlatın. Git dağıtımı, Web uygulaması durduğunda kullanılamaz.

---
**Belirti**:`Couldn't resolve host 'hostname'`

**Neden**: 'azure' uzak oluştururken girilen adres bilgilerini doğru değilse bu hata oluşabilir.

**Çözümleme**: kullanım `git remote -v` ilişkili URL ile birlikte tüm uzaktan kumandalar listelemek için komutu. 'Azure' uzak URL'sini doğru olduğundan emin olun. Gerekirse, kaldırın ve doğru URL'yi kullanarak bu uzaktan erişim oluşturun.

---
**Belirti**:`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Neden**: dal sırasında belirtmezseniz, bu hata oluşabilir `git push`, veya değil ayarladıysanız `push.default` değeri `.gitconfig`.

**Çözümleme**: ana dala belirtme gönderme işlemi yeniden gerçekleştirin. Örneğin:

```bash
git push azure master
```

---
**Belirti**:`src refspec [branchname] does not match any.`

**Neden**: ana dışında bir dal için 'azure' uzak itme çalışırsanız, bu hata oluşabilir.

**Çözümleme**: ana dala belirtme gönderme işlemi yeniden gerçekleştirin. Örneğin:

```bash
git push azure master
```

---
**Belirti**:`RPC failed; result=22, HTTP code = 5xx.`

**Neden**: HTTPS üzerinden büyük git deposu itme çalışırsanız, bu hata oluşabilir.

**Çözümleme**: postBuffer büyütmeniz için yerel makinede git yapılandırmasını değiştirme

```bash
git config --global http.postBuffer 524288000
```

---
**Belirti**:`Error - Changes committed to remote repository but your web app not updated.`

**Neden**: ek gerekli modüllerini belirten bir package.json dosyası içeren bir Node.js uygulaması dağıtıyorsanız bu hata oluşabilir.

**Çözümleme**: 'npm hata!' içeren ek iletiler Bu hata oturum öncesinde olmalıdır ve ek bağlam hatada sağlayabilir. Bu hata ve karşılık gelen 'npm hata!' nedenlerini aşağıdaki bilinmektedir İleti:

* **Hatalı biçimlendirilmiş package.json dosyası**: npm hata! Bağımlılıklar okunamadı.
* **İkili bir dağıtım için Windows olmayan yerel modül**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OR
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Ek Kaynaklar

* [Git belgeleri](http://git-scm.com/documentation)
* [Proje Kudu belgeleri](https://github.com/projectkudu/kudu/wiki)
* [Azure uygulama hizmeti için sürekli dağıtım](app-service-continuous-deployment.md)
* [Azure için PowerShell'i kullanma](/powershell/azure/overview)
* [Azure komut satırı arabirimi kullanma](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
