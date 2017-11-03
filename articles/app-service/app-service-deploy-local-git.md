---
title: "Azure App Service için Yerel Git Dağıtımı"
description: "Azure uygulama hizmeti için yerel Git dağıtımı etkinleştirmeyi öğrenin."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: ed0239df7bf1e4d37987aaa929d0c67bec595b30
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="local-git-deployment-to-azure-app-service"></a>Azure App Service için Yerel Git Dağıtımı
Bu öğretici, uygulamanızın dağıtılacağı gösterilmiştir [Azure Web Apps](app-service-web-overview.md) , yerel bilgisayarınızda bir Git deposundan. Uygulama hizmetini destekleyen bu yaklaşımı **yerel Git** dağıtım seçeneği [Azure Portal].  
Bu makalede açıklanan Git komutların çoğu kullanarak bir uygulama hizmeti uygulaması oluştururken, otomatik olarak gerçekleştirilir [Azure komut satırı arabirimi] açıklandığı gibi [burada](app-service-web-get-started-dotnet.md).

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Git. İkili yükleme indirebilirsiniz [burada](http://www.git-scm.com/downloads).  
* Git temel bilgiye.
* Bir Microsoft Azure hesabı. Bir hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details)

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service'i kullanmaya başlamak istiyorsanız, Git [App Service'i deneyin](https://azure.microsoft.com/try/app-service/), burada hemen bir kısa süreli başlangıç uygulaması App Service'te oluşturabilirsiniz. Kredi kartı ve taahhüt gerekmez.  
> 
> 

## <a name="Step1"></a>1. adım: yerel bir havuz oluşturma
Yeni bir Git deposu oluşturmak için aşağıdaki görevleri gerçekleştirin.

1. Bir komut satırı aracı gibi başlatın **Gitbash'i** (Windows) veya **Bash** (UNIX kabuğu). OS X sistemlerinde, komut satırı aracılığıyla erişebilirsiniz **Terminal** uygulama.
2. Dağıtmak için içeriğin bulunduğu olduğu dizine gidin.
3. Yeni bir Git deposunu başlatmak için aşağıdaki komutu kullanın:

```bash  
git init
```

## <a name="Step2"></a>2. adım: içeriğinizi Yürüt
Uygulama hizmeti çeşitli programlama dillerini içinde oluşturulan uygulamaları destekler. 

1. Deponuzda zaten içerik Atla içeriyorsa, bu noktası ve aşağıdaki 2'in üzerine taşıyın. Deponuzda zaten içeriği yalnızca içermiyorsa statik .html dosyasıyla gibi doldurun: 
   
   * Bir metin düzenleyicisi kullanarak adlı yeni bir dosya oluşturun **index.html** Git deposu kökündeki
   * Aşağıdaki metni index.html için içerik dosya ve kaydedin gibi ekleyin: *Hello Git!*
2. Komut satırından Git deponuzu kök altında olduğundan emin olun. Ardından depoya dosyaları eklemek için aşağıdaki komutu kullanın:

```bash  
git add -A
```
3. Ardından, aşağıdaki komutu kullanarak depoya değişiklikleri uygulayın:

```bash  
git commit -m "Hello Azure App Service"
```  

## <a name="Step3"></a>3. adım: uygulama hizmeti uygulama havuzu etkinleştirme
App Service uygulamanız için bir Git deposu etkinleştirmek için aşağıdaki adımları gerçekleştirin.

1. [Azure Portal]’da oturum açın.
2. App Service uygulamanızın dikey penceresinde tıklayın **ayarlar > dağıtım kaynağı**. Tıklatın **Kaynak Seç**, ardından **yerel Git deposu**ve ardından **Tamam**.  
   
    ![Yerel Git deposu](./media/app-service-deploy-local-git/local_git_selection.png)
3. Bu ilk zaman ayarınız Azure deposunu ise, bunun için oturum açma kimlik bilgileri oluşturmanız gerekir. Yerel Git deposundan Azure depo ve anında iletme değişiklikleri oturum kullanır. Uygulamanızın dikey penceresinden tıklayın **ayarlar > Dağıtım kimlik bilgileri**, dağıtım kullanıcı adı ve parola yapılandırın. İşiniz bittiğinde tıklatın **kaydetmek**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Adım 4: projenizi dağıtma
Yerel Git kullanarak uygulama hizmeti uygulamanızı yayımlamak için aşağıdaki adımları kullanın.

1. Azure Portal'da uygulamanızın dikey penceresinde tıklayın **ayarlar > Özellikler** için **Git URL'si**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **Git URL'si** yerel depodan dağıtmak için uzaktan başvurudur. Bu URL aşağıdaki adımlarda kullanacaksınız.
2. Komut satırı kullanarak, yerel Git deponuzu kök dizininde olduğundan emin olun.
3. Kullanım `git remote` listelenen uzak başvuru eklemek için **Git URL'si** adım 1. Komutunuzu aşağıdakine benzer görünecektir:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > **Uzak** komut uzak deponuza adlandırılmış bir başvuru ekler. Bu örnekte, web uygulamanızın depo için 'azure' adlı bir başvuru oluşturur.
   > 
   > 
4. İçeriğinizi kullanarak yeni App Service'e anında **azure** uzak oluşturduğunuz.

```bash  
git push azure master
```
    You will be prompted for the password you created earlier when you reset your deployment credentials in the Azure Portal. Enter the password (note that Gitbash does not echo asterisks to the console as you type your password). 
5. Uygulamanızı Azure Portal'da dönün. En son gönderme, bir günlük girişi görüntülenmelidir **dağıtımları** dikey. 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Tıklatın **Gözat** içeriği dağıtılan doğrulamak için uygulamanın dikey pencerenin üstündeki düğmesi. 

## <a name="Step5"></a>Sorun giderme
Git Azure App Service uygulama yayımlamak için kullanırken sık karşılaşılan sorunları veya hatalar şunlardır:

- - -
**Belirti**: '[siteURL]' erişilemedi: [scmAddress] bağlanılamadı

**Neden**: uygulama hazır ve çalışır değilse bu hata oluşabilir.

**Çözümleme**: Azure Portal'da uygulamayı başlatın. Uygulama çalışmıyorsa Git dağıtımı çalışmaz. 

- - -
**Belirti**: ana bilgisayar 'hostname' çözümleyemedik

**Neden**: 'azure' uzak oluştururken girilen adres bilgilerini doğru değilse bu hata oluşabilir.

**Çözümleme**: kullanım `git remote -v` ilişkili URL ile birlikte tüm uzaktan kumandalar listelemek için komutu. 'Azure' uzak URL'sini doğru olduğundan emin olun. Gerekirse, kaldırın ve doğru URL'yi kullanarak bu uzaktan erişim oluşturun.

- - -
**Belirti**: ortak hiçbir refs ve hiçbiri belirtilen; hiçbir şey yapılması. Belki 'master' gibi bir dal belirtmeniz gerekir.

**Neden**: git gerçekleştirme gönderme işlemi ve Git tarafından kullanılan push.default değeri ayarlı değil bir dal belirtmezseniz, bu hata oluşabilir.

**Çözümleme**: ana dala belirtme gönderme işlemi yeniden gerçekleştirin. Örneğin:

```bash  
git push azure master
```
- - -
**Belirti**: src refspec [branchname] herhangi eşleşmiyor.

**Neden**: ana dışında bir dal için 'azure' uzak itme çalışırsanız, bu hata oluşabilir.

**Çözümleme**: ana dala belirtme gönderme işlemi yeniden gerçekleştirin. Örneğin:

```bash  
git push azure master
```
- - -
**Belirti**: RPC başarısız oldu; yol = 22, HTTP kodu 502 =.

**Neden**: HTTPS üzerinden büyük git deposu itme çalışırsanız, bu hata oluşabilir.

**Çözümleme**: postBuffer büyütmeniz için yerel makinede git yapılandırmasını değiştirme

```bash  
git config --global http.postBuffer 524288000
```
- - -
**Belirti**: hata - uzak depoya kaydedilen değişiklikler, ancak web uygulamanızı güncelleştirilmez.

**Neden**: ek gerekli modüllerini belirten bir package.json dosyası içeren bir Node.js uygulaması dağıtıyorsanız bu hata oluşabilir.

**Çözümleme**: 'npm hata!' içeren ek iletiler Bu hata oturum öncesinde olmalıdır ve ek bağlam hatada sağlayabilir. Bu hata ve karşılık gelen 'npm hata!' nedenlerini aşağıdaki bilinmektedir İleti:

* **Hatalı biçimlendirilmiş package.json dosyası**: npm hata! Bağımlılıklar okunamadı.
* **İkili bir dağıtım için Windows olmayan yerel modül**:
  
  * npm hata! \`cmd "/ c" "düğümü gyp yeniden oluşturma"\` 1 ile başarısız oldu
    
      OR
  * npm hata! [modulename@version] önceden: \`olun || gmake\`

## <a name="additional-resources"></a>Ek Kaynaklar
* [Git belgeleri](http://git-scm.com/documentation)
* [Proje Kudu belgeleri](https://github.com/projectkudu/kudu/wiki)
* [Azure uygulama hizmeti için sürekli dağıtım](app-service-continuous-deployment.md)
* [Azure için PowerShell'i kullanma](/powershell/azure/overview)
* [Azure komut satırı arabirimi kullanma](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure komut satırı arabirimi]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
