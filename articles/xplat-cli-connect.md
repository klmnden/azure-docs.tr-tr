---
title: "Oturum açmak için Azure CLI üzerinden | Microsoft Docs"
description: "Mac, Linux ve Windows için Azure komut satırı arabiriminden (Azure CLI) Azure aboneliğinize bağlanma"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 31efab60690b54faf7992251fcd01e307c4464f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="log-in-to-azure-from-the-azure-cli"></a>Oturum açmak için Azure Azure CLI üzerinden
Azure CLI, Azure kaynakları ile çalışmak için açık kaynak, platformlar arası komut kümesidir. Bu makalede Azure CLI Azure aboneliğinize bağlanmak için bir Azure hesabı kimlik bilgilerinizi sağlamak için farklı yollar açıklanmaktadır:

* Çalıştırma `azure login` Azure Active Directory ile kimlik doğrulaması için CLI komutu. Bu yöntem, hem de CLI komutları erişmenizi sağlayan [komut modları](#cli-command-modes). Komutu ek seçenekleri olmadan çalıştırdığınızda `azure login` etkileşimli bir web Portalı aracılığıyla oturum açmayı devam etmek isteyip istemediğinizi sorar. İçin ek `azure login` komutu seçenekleri, bu makale veya türü senaryolarda bkz `azure login --help`.
* Yalnızca Azure hizmet yönetimi modu CLI komutları (en yeni dağıtımlar için önerilmez) kullanmanız gerekiyorsa, indirin ve yayımlama ayarları dosyası, bilgisayarınıza yükleyin.

CLI henüz yüklemediyseniz bkz [Azure CLI yükleme](cli-install-nodejs.md). Bir Azure aboneliğiniz yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](http://azure.microsoft.com/free/) oluşturabilirsiniz.

Farklı bir hesap kimlikleri ve Azure abonelikleri hakkında arka plan için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>Senaryo 1: etkileşimli oturum açma ile azure oturum açma
Belirli hesaplarıyla, CLI çalıştırmanızı gerektirir `azure login` ve bir web tarayıcısı adı verilen bir işlem bir web portalı üzerinden oturum açma işlemine devam *etkileşimli oturum açma*. Bir iş veya Okul hesabı varsa, ortak bir nedeni (olarak da adlandırılan bir *kuruluş hesabı*), çok faktörlü kimlik doğrulaması gerektiren üzere ayarlandı. Resource Manager modunu komutlarını kullanmak istediğinizde de Microsoft hesabınızla etkileşimli oturum açma kullanın.

Etkileşimli oturum açma kolaydır: türü `azure login` ----seçenekleri olmadan aşağıdaki örnekte gösterildiği gibi:

```
azure login
```                                                                                             

Çıktı aşağıdakine benzer görünür:

```         
info:    Executing command login
info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate.
```
Komut çıktısında size sunulan kodu kopyalayın ve belirtilmişse http://aka.ms/devicelogin veya diğer sayfası için bir tarayıcı açın. (Aynı bilgisayarda ya da farklı bir bilgisayar veya aygıt bir tarayıcı açabilirsiniz.) Kodu girin ve ardından kullanmak istediğiniz kimliği için kullanıcı adı ve parola girmeniz istenir. Bu işlem tamamlandığında, oturum açma komut kabuğu tamamlar. Şöyle görünebilir:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> Etkileşimli oturum açma ile kimlik doğrulaması ve yetkilendirme Azure Active Directory'yi kullanarak gerçekleştirilir. Bir Microsoft hesabı kimlik kullanırsanız, oturum açma işlemi, Azure Active Directory varsayılan etki alanınıza erişir. (Ücretsiz bir Azure hesabı için kaydolduysanız, Azure Active Directory'ye otomatik olarak hesabınız için varsayılan etki alanı oluşturulur.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>2. Senaryo: bir kullanıcı adı ve parola ile azure oturum açma
Kullanmak `azure login` kullanıcı komutu (`-u`) bir iş kullanmak istediğiniz ya da Okul hesabı kimlik doğrulaması için parametre çok faktörlü kimlik doğrulama gerekli değildir. Parola için komut satırına istenir (veya parola isteğe bağlı olarak ek bir parametre geçirebilir `azure login` komutu). Aşağıdaki örnekte, kullanıcı bir kurumsal hesap adını geçirir:

    azure login -u myUserName@contoso.onmicrosoft.com

Parolanızı girmeniz istenir:

    info:    Executing command login
    Password: *********

Daha sonra oturum açma işlemini tamamlar.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Bu kimlik bilgileri, ilk zaman oturum varsa, kimlik doğrulama belirtecini önbelleğe istediğiniz doğrulamak için istenir. Bu istem, ayrıca daha önce kullanılan saptanmışsa `azure logout` (makalenin sonraki bölümlerinde açıklanan) komutu. Otomasyon senaryoları için bu istemini atlamak için Çalıştır `azure login` ile `-q` parametresi.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Senaryo 3: bir hizmet sorumlusu ile azure oturum açma
Bir Active Directory uygulaması için bir hizmet sorumlusu oluşturmak ve hizmet sorumlusu, aboneliğinizde izinleri varsa kullanabileceğiniz `azure login` hizmet sorumlusunun kimliğini doğrulamak için komutu. Senaryonuza bağlı olarak, açık parametre olarak hizmet sorumlusu kimlik bilgilerini sağlayabilir `azure login` komutu. Örneğin, aşağıdaki komutu, Active Directory Kiracı kimliği ve hizmet asıl adı geçirir:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Ardından parolayı girmeniz istenir. CLI komut dosyası veya uygulama kod aracılığıyla kimlik bilgilerini sağlayın veya etkileşimsiz Otomasyon senaryoları için hizmet sorumlusunun kimliğini doğrulamak için bir sertifika kullanır. Ayrıntılı bilgi ve örnekler için bkz: [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Senaryo 4: yayımlama ayarları dosyası kullanın
Yalnızca Azure hizmet yönetimi modu CLI komutları (örneğin, Klasik dağıtım modelinde Azure sanal makineleri dağıtmak için) kullanmanız gerekiyorsa, yayımlama ayarları dosyası kullanarak bağlanabilir. Bu yöntem bir sertifika abonelik ve sertifika geçerli olduğu sürece için yönetim görevleri gerçekleştirmenizi sağlar, yerel bilgisayarınıza yükler.

* **Yayımlama ayarları dosyasını indirmek için** hesabınız için yazarak CLI Hizmet Yönetimi modunda olduğundan emin olun `azure config mode asm`. Ardından aşağıdaki komutu çalıştırın:

        azure account download

Bu, varsayılan tarayıcı açar ve oturum açmak için ister [Klasik Azure portalı](https://manage.windowsazure.com). Oturum açma, sonra bir `.publishsettings` dosya yüklemelerini. Bu dosyanın kaydedildiği yeri not edin.

> [!NOTE]
> Hesabınızın birden çok Azure Active Directory Kiracı ile ilişkili ise, hangi etkin bir yayımlama ayarları dosyası indirmek istediğiniz dizini seçin istenebilir.
>
>

Yükleme sayfasını kullanarak seçildikten sonra veya Klasik Azure portalını ziyaret, seçili Active Directory Klasik portal ve indirme sayfa tarafından kullanılan varsayılan olur. Varsayılan kurulduktan sonra gördüğünüz metin '**seçimi sayfaya dönmek için buraya tıklayın**' sayfanın üst kısmındaki indirme. Seçim sayfasına geri dönmek için sağlanan bağlantıyı kullanın.

* **Yayımlama ayarları dosyasını içeri aktarmak için**, aşağıdaki komutu çalıştırın:

        azure account import <path to your .publishsettings file>

> [!IMPORTANT]
> Aldıktan sonra yayımlama ayarları, silmeniz gerekir `.publishsettings` dosya. Azure CLI tarafından artık gerekli ve aboneliğinizi erişmek için kullanılabilir olarak bir güvenlik riski oluşturur.
>
>

## <a name="cli-command-modes"></a>CLI komut modları
Azure CLI farklı komut kümeleriyle Azure kaynakları ile çalışmak için iki komut modları sağlar:

* **Resource Manager moduna** - Resource Manager dağıtım modelinde Azure kaynakları ile çalışmak için. Bu modu ayarlamak için Çalıştır `azure config mode arm`.
* **Hizmet Yönetimi modu** - Klasik dağıtım modelinde Azure kaynakları ile çalışmak için. Bu modu ayarlamak için Çalıştır `azure config mode asm`.

İlk yüklediğinizde, kaynak yöneticisi modunda CLI sürümü geçerli değil.

> [!NOTE]
> Hizmet Yönetimi ve Resource Manager moduna karşılıklı olarak birbirini dışlar. Diğer bir deyişle, bir modunda oluşturulan kaynakları diğer modundan yönetilemez.
>
>

## <a name="multiple-subscriptions"></a>Birden çok abonelik
Birden çok Azure aboneliğiniz varsa, Azure'a bağlanılıyor kimlik bilgilerinizle ilişkili tüm Aboneliklerde erişim verir. Bir abonelik, varsayılan olarak seçilen ve işlemleri gerçekleştirirken Azure CLI tarafından kullanılır. Abonelikleri görüntüleme geçerli varsayılan abonelik dahil olmak üzere, kullanarak `azure account list` komutu. Bu komut, aşağıdakine benzer bilgiler döndürür:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

Yukarıdaki listede **geçerli** sütunu geçerli varsayılan abonelik Azure-alt-1 olarak gösterir. Varsayılan abonelik değiştirmek için `azure account set` komutunu ve varsayılan olmasını istediğiniz aboneliği belirtin. Örneğin:

    azure account set Azure-sub-2

Bu varsayılan abonelik Azure-alt-2'ye değiştirir.

> [!NOTE]
> Varsayılan abonelik değiştirme hemen etkili olur ve genel bir değişikliktir; bunları aynı komut satırı örneği veya farklı bir örnek çalıştırdığınızda yeni Azure CLI komutları, yeni varsayılan abonelik kullanın.
>
>

Varsayılan olmayan bir abonelik Azure CLI ile kullanmak istediğiniz, ancak geçerli varsayılan değiştirmek istemiyorsanız, kullanabileceğiniz `--subscription` seçeneği için komutu ve işlem için kullanmak istediğiniz abonelik adını sağlayın.

Azure aboneliğinize bağlandıktan sonra Azure kaynakları ile çalışmak için Azure CLI komutları kullanarak başlatabilirsiniz.

## <a name="storage-of-cli-settings"></a>CLI ayarlarını depolama
İle olup olmadığını oturum `azure login` komut veya içeri aktarma yayımlama ayarları, CLI profili ve günlükleri depolanmış bir `.azure` directory bulunan, `user` dizin. `user` Dizin işletim sisteminiz tarafından korunur. Ancak, şifrelemek için ek adımlar uygulamanız önerilir, `user` dizin. Bunu aşağıdaki yöntemlerle yapabilirsiniz:

* Windows, dizin özelliklerini değiştirin veya BitLocker'ı kullanın.
* Mac üzerinde üzerinde FileVault dizinini açın.
* Ubuntu’da Şifreli Giriş dizini özelliğini kullanın. Diğer Linux dağıtımları benzer özellikleri sunar.

## <a name="logging-out"></a>Günlük
Oturum kapatma için aşağıdaki komutu kullanın:

    azure logout -u <username>

Hesapla ilişkili abonelikleri yalnızca siler abonelik bilgileri yerel profilinden günlüğü Active Directory ile doğrulanmışsa. Yayımlama ayarları dosyası da abonelikleri alındıysa, siler Active Directory ile ilgili bilgiler yerel profilinden yalnızca ancak oturum kapatılıyor.

## <a name="next-steps"></a>Sonraki adımlar
* Azure CLI komutları kullanmak için bkz: [Kaynak Yöneticisi modunda Azure CLI komutları](virtual-machines/azure-cli-arm-commands.md) ve [Hizmet Yönetimi modunda Azure CLI komutları](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* Projeye katkıda ya da Azure CLI, yükleme kaynak kodu, rapor sorunları hakkında daha fazla bilgi için ziyaret [Azure CLI için GitHub depo](https://github.com/azure/azure-xplat-cli).
* Azure CLI veya Azure kullanarak sorunlarla karşılaşırsanız, ziyaret [Azure forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).
