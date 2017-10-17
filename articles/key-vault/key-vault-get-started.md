---
title: "Azure Anahtar Kasası’nı kullanmaya başlama | Microsoft Belgeleri"
description: "Bu öğreticiyi, Azure'da sağlamlaştırılmış bir kapsayıcı oluşturmak ve Azure'da şifreleme anahtarlarını ve gizli anahtarları depolayıp yönetmek amacıyla Azure Anahtar Kasası kullanmaya başlamanıza yardımcı olması için kullanın."
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/16/2017
ms.author: barclayn
ms.translationtype: HT
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: 73b4ae4b7baca434c6aed99a2e59a9102b0d96ed
ms.contentlocale: tr-tr
ms.lasthandoff: 07/22/2017

---
# <a name="get-started-with-azure-key-vault"></a>Azure Anahtar Kasası kullanmaya başlama 
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Giriş
Bu öğreticiyi, Azure'da sağlamlaştırılmış bir kapsayıcı (bir kasa) oluşturmak ve Azure'da şifreleme anahtarlarını ve gizli anahtarları depolayıp yönetmek amacıyla Azure Anahtar Kasası ile çalışmaya başlamaya yardımcı olması için kullanın. Bu öğretici, bir Azure uygulamasıyla kullanabileceğiniz anahtar veya parolayı içeren bir kasa oluşturmak için Azure PowerShell kullanma sürecinde size rehberlik eder. Ardından, bu anahtarı veya parolayı bir uygulamanın nasıl kullanacağını gösterir.

**Tahmini tamamlanma süresi:** 20 dakika

> [!NOTE]
> Bu öğretici, adımlardan birinde yani bir uygulamanın anahtar kasasında bir anahtarı veya gizli anahtarı kullanmasının yetkilendirilmesinde bulunan Azure uygulaması yazma için yönergeler içermez.
>
> Bu öğretici Azure PowerShell kullanır. Platformlar Arası Komut Satırı Arabirimi yönergeleri için [bu eşdeğer öğreticiye](key-vault-manage-with-cli2.md) bakın.
>
>

Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Bir Microsoft Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure PowerShell'in, **en az 1.1.0 sürümü**. Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Azure PowerShell'i zaten yüklediyseniz ve sürümünü bilmiyorsanız Azure PowerShell konsolunda `(Get-Module azure -ListAvailable).Version` yazın. Azure PowerShell'in 0.9.8 - 0.9.1 sürümleri arasında bir sürümü yüklüyse bazı küçük değişikliklerle bu öğreticiyi kullanmaya devam edebilirsiniz. Örneğin, `Switch-AzureMode AzureResourceManager` komutunu kullanmanız gerekir ve bazı Azure Anahtar Kasası komutları değişmiştir. 0.9.1 - 0.9.8 arasındaki sürümlere yönelik Anahtar Kasası cmdlet listesi için bkz. [Azure Anahtar Kasası Cmdlet'leri](/powershell/module/azurerm.keyvault/#key_vault).
* Bu öğreticide oluşturduğunuz anahtarı veya parolayı kullanmak üzere yapılandırılacak bir uygulama. [Microsoft Yükleme Merkezi](http://www.microsoft.com/en-us/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Yönergeler için beraberinde gelen Benioku dosyasına bakın.

Bu öğretici Azure PowerShell'e yeni başlayanlar için tasarlanmıştır ancak modüller, cmdlet'ler ve oturumlar gibi temel kavramları anladığınız varsayılır. Daha fazla bilgi için bkz. [Windows Power Shell ile Çalışmaya Başlama](https://technet.microsoft.com/library/hh857337.aspx)

Bu öğreticide gördüğünüz herhangi bir cmdlet hakkında ayrıntılı yardım almak için **Get-Help** cmdlet'ini kullanın.

    Get-Help <cmdlet-name> -Detailed

Örneğin, **Login-AzureRmAccount** cmdlet'i hakkında yardım almak için şunu yazın:

    Get-Help Login-AzureRmAccount -Detailed

Ayrıca, Azure PowerShell'de Azure Resource Manager hakkında bilgi edinmek için aşağıdaki öğreticileri okuyabilirsiniz:

* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Aboneliklerinize bağlanma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

    Login-AzureRmAccount

Azure Kamu gibi belirli bir Azure örneği kullanıyorsanız bu komutla birlikte -Environment parametresini kullanmayı unutmayın. Örneğin, `Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa ve Azure Anahtar Kasası için kullanmak üzere belirli bir tanesini belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:

    Get-AzureRmSubscription

Ardından, kullanılacak aboneliği belirtmek için şunu yazın:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure Power Shell'i yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a id="resource"></a>Yeni bir kaynak grubu oluşturma
Azure Resource Manager'ı kullandığınızda, tüm ilgili kaynaklar bir kaynak grubu içinde oluşturulur. Bu öğretici için **ContosoResourceGroup** adlı yeni bir kaynak grubu oluşturacağız:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Anahtar kasası oluşturma
Bir anahtar kasası oluşturmak için [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) cmdlet'ini kullanın. Bu cmdlet, üç zorunlu parametreye sahiptir: bir **kaynak grubu adı**, bir **anahtar kasası adı** ve **coğrafi konum**.

Örneğin, **ContosoKeyVault** kasa adını, **ContosoResourceGroup** kaynak grubu adını ve **Doğu Asya** konumunu kullanıyorsanız şunu yazın:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Bu cmdlet'in çıkışı, yeni oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **Kasa Adı**: Örnekte **ContosoKeyVault**'tur. Bu adı diğer Anahtar Kasası cmdlet'leri için kullanacaksınız.
* **Kasa URI'si**: Örnekte https://contosokeyvault.vault.azure.net/ şeklindedir. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Henüz başka kimse yetkilendirilmedi.

> [!NOTE]
> Yeni anahtar kasanızı oluşturmaya çalışırken **Abonelik 'Microsoft.KeyVault ad alanını kullanmaya kayıtlı değil'** hatasını görürseniz `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` çalıştırın ve ardından New-AzureRmKeyVault komutunuzu yeniden çalıştırın. Daha fazla bilgi için bkz. [AzureRmResourceProvider'ı kaydetme](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Anahtar kasasına bir anahtar veya gizli anahtar ekleme
Azure Anahtar Kasası'nın sizin için yazılım korumalı bir anahtar oluşturmasını istiyorsanız [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) cmdlet'ini kullanın ve aşağıdakini yazın:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Ancak C:\ sürücünüzde Azure Anahtar Kasası'na yüklemek istediğiniz softkey.pfx adlı bir dosyada var olan bir yazılım korumalı anahtar varsa .PFX dosyasının **123** parolası için **securepfxpwd** değişkenini ayarlamak üzere aşağıdakini yazın:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Ardından, Anahtar Kasası hizmetinde yazılım ile anahtarı koruyan .PFX dosyasından anahtarı içeri aktarmak için aşağıdakini yazın:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Artık oluşturduğunuz veya Azure Anahtar Kasası'na yüklediğiniz bu anahtara URI'sini kullanarak başvurabilirsiniz. Her zaman geçerli sürümü almak için **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** kullanın ve bu belirli sürümü almak için **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** kullanın.  

Bu anahtar için URI görüntülemek üzere şunu yazın:

    $Key.key.kid

SQLPassword adlı bir parola olup Azure Anahtar Kasası'na Pa$$w0rd değerine sahip olan bir gizli anahtarı kasaya eklemek için öncelikle Pa$$w0rd değerini şunları yazarak güvenli bir dizeye dönüştürün:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Ardından aşağıdakini yazın:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Artık Azure Anahtar Kasası'na eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Her zaman geçerli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword** kullanın ve bu belirli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** kullanın.

Bu parola için URI görüntülemek üzere şunu yazın:

    $secret.Id

Yeni oluşturduğunuz anahtarı veya gizli anahtarı görüntüleyelim:

* Anahtarınızı görüntülemek için şunu yazın: `Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
* Gizli anahtarı görüntülemek için şunu yazın: `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Artık anahtar kasanız ve anahtarınız veya gizli anahtarınız uygulamaların kullanması için hazır. Uygulamaları bunları kullanmaları için yetkilendirmeniz gerekir.  

## <a id="register"></a>Bir uygulamayı Azure Active Directory’ye kaydetme
Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure Anahtar Kasası'na özgü değildir ancak bütünlük açısından buraya dahil edilir.

> [!IMPORTANT]
> Öğreticiyi tamamlamak için bu adımda kaydedeceğiniz hesabınızın, kasanızın ve uygulamanızın hepsinin aynı Azure dizininde olması gerekir.
>
>

Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir. Bunu yapmak için uygulama sahibinin öncelikle Azure Active Directory'sinde uygulamayı kaydetmesi gerekir. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

* Bir **Uygulama Kimliği** (İstemci Kimliği olarak da bilinir) ve **kimlik doğrulama anahtarı** (paylaşılan gizli anahtar olarak da bilinir). Uygulamanın bir belirteç almak için bu değerlerin her ikisini de Azure Active Directory'ye sunması gerekir. Uygulamanın bunu yapmak için nasıl yapılandırılacağı uygulamaya bağlıdır. Anahtar Kasası örnek uygulaması için uygulama sahibi bu değerleri app.config dosyasında ayarlar.

Bir uygulamayı Azure Active Directory'ye kaydetmek için:

1. Klasik Azure portalında oturum açın.
2. Solda **Active Directory**'ye tıklayın ve ardından uygulamanızı içine kaydedeceğiniz dizini seçin. <br> <br> **Not:** Anahtar kasanızı oluşturduğunuz Azure aboneliğini içeren aynı dizini seçmeniz gerekir. Bu dizinin hangisi olduğunu bilmiyorsanız **Ayarlar**'a tıklayın, anahtar kasanızı oluşturduğunuz aboneliği tanımlayın ve son sütunda görüntülenen dizinin adını not edin.
3. **UYGULAMALAR**'a tıklayın. Dizininize hiçbir uygulama eklenmemişse bu sayfa yalnızca **Uygulama Ekle** bağlantısını gösterir. Bağlantıya tıklayın veya alternatif olarak komut çubuğunda **EKLE**'ye tıklayabilirsiniz.
4. **UYGULAMA EKLEME** sihirbazında **Ne yapmak istiyorsunuz?** sayfasında **Add an application my organization is developing (Kuruluşumun geliştirdiği bir uygulama ekle)** seçeneğine tıklayın.
5. **Bize uygulamanızı anlatın** sayfasında uygulamanız için bir ad belirtin ve ardından **WEB UYGULAMASI VE/VEYA WEB API'Sİ**'ni (varsayılan) seçin. **İleri** simgesine tıklayın.
6. **Uygulama özellikleri** sayfasında web uygulamanız için **OTURUM AÇMA URL'Sİ** ve **UYGULAMA KİMLİĞİ URI'Sİ** belirtin. Uygulamanız bu değerlere sahip değilse değerleri bu adım için kendiniz belirleyebilirsiniz. (örneğin, her iki kutu için http://test1.contoso.com belirtebilirsiniz). Bu sitelerin mevcut olup olmaması önemli değildir. Önemli olan her bir uygulama için uygulama kimliği URI'sinin dizininizdeki tüm uygulamalar için farklı olmasıdır. Dizin uygulamanızı tanımlamak için bu dizeyi kullanır.
7. Sihirbazda yaptığınız değişiklikleri kaydetmek için **Tamamlandı** simgesine tıklayın.
8. **Hızlı Başlangıç** sayfasında **YAPILANDIR**'a tıklayın.
9. **Anahtarlar** bölümüne kaydırın, süre seçin ve ardından **KAYDET**'e tıklayın. Sayfa yenilenir ve ardından bir anahtar değeri gösterir. Bu anahtar değeri ve **İSTEMCİ KİMLİĞİ** değeri ile uygulamanızı yapılandırmanız gerekir. (Bu yapılandırma için yönergeler uygulamaya özgü olur.)
10. Kasanızda izinleri ayarlamak için bir sonraki adımda kullanacağınız istemci kimliği değerini bu sayfadan kopyalayın.

## <a id="authorize"></a>Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme
Uygulamayı kasadaki anahtar veya gizli anahtara erişmek üzere yetkilendirmek için [AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet'ini ayarlayın.

Örneğin, kasa adınız **ContosoKeyVault** ise ve yetkilendirmek istediğiniz uygulama 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed istemci kimliğine sahipse ve uygulamaya kasanızdaki anahtarların şifresini çözme ve bunlarla oturum açma yetkisi vermek istiyorsanız şunu çalıştırın:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız şunu çalıştırın:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Bir donanım güvenlik modülü (HSM) kullanmak istiyorsanız
Ek güvence için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için [HSM korumalı anahtarları desteklemek için Azure Anahtar Kasası Premium hizmet katmanını](https://azure.microsoft.com/pricing/free-trial/) kullanmanız gerekir. Ek olarak, bu işlevin Azure Çin'de kullanılamadığını unutmayın.

Anahtar kasasını oluşturduğunuzda **-SKU** parametresini ekleyin:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Bu anahtar kasasına yazılım korumalı anahtarlar (daha önce gösterildiği gibi) ve HSM korumalı anahtarlar ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için **-Destination** parametresini 'HSM' olarak ayarlayın.

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Bilgisayarınızdaki bir .PFX dosyasından bir anahtarı içeri aktarmak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Bu senaryo anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanızı ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanızı sağlar:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Bu BYOK paketini oluşturma hakkında daha ayrıntılı yönergeler için bkz. [Azure Anahtar Kasası için HSM korumalı anahtarlar oluşturma ve aktarma](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Anahtar kasasını, ilişkili anahtarları ve gizli anahtarları silme
Anahtar kasasına veya içerdiği anahtar veya gizli anahtara artık ihtiyacınız yoksa anahtar kasasını [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault) cmdlet'ini kullanarak silebilirsiniz.

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Diğer Azure PowerShell Cmdlet’leri
Azure Anahtar Kasası'nı yönetmede kullanışlı bulabileceğiniz diğer komutlar:

* `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Bu komut, tüm anahtarların ve seçilen özelliklerin tablosal bir görüntüsünü alır.
* `$Keys[0]`: Bu komut, belirtilen anahtar için özelliklerin tam bir listesini görüntüler
* `Get-AzureKeyVaultSecret`: Bu komut, tüm gizli adların ve seçilen özelliklerin tablosal bir görüntüsünü listeler.
* `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Belirli bir anahtarın nasıl kaldırılacağına örnek verir.
* `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Belirli bir gizli anahtarın nasıl kaldırılacağına örnek verir.

## <a id="next"></a>Sonraki adımlar
Azure Anahtar Kasası'nı bir web uygulamasında kullanan bir izleme öğreticisi için bkz. [Azure Anahtar Kasası'nı bir Web Uygulamasından Kullanma](key-vault-use-from-web-application.md).

Anahtar kasanızın nasıl kullanıldığını görmek için bkz. [Azure Anahtar Kasası Günlüğü](key-vault-logging.md).

Azure Anahtar Kasası'na yönelik en son Azure PowerShell cmdlet'leri listesi için bkz. [Azure Anahtar Kasası Cmdlet'leri](/powershell/module/azurerm.keyvault/#key_vault).

Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

