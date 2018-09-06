---
title: Azure Anahtar Kasası’nı kullanmaya başlama | Microsoft Belgeleri
description: Bu öğreticiyi, Azure'da sağlamlaştırılmış bir kapsayıcı oluşturmak ve Azure'da şifreleme anahtarlarını ve gizli anahtarları depolayıp yönetmek amacıyla Azure Anahtar Kasası kullanmaya başlamanıza yardımcı olması için kullanın.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2018
ms.author: barclayn
ms.openlocfilehash: 3f3adb1230d6ca6b3a7e616a0beed15d66895124
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43283006"
---
# <a name="get-started-with-azure-key-vault"></a>Azure Anahtar Kasası kullanmaya başlama 
Bu makale PowerShell kullanarak Azure Key Vault kullanmaya başlamanıza yardımcı olur ve aşağıdaki etkinliklerde size kılavuzluk eder:
- Azure’da güçlendirilmiş kapsayıcı (kasa) oluşturma.
- KeyVault kullanarak Azure’da şifreleme anahtarlarını ve gizli dizileri depolama ve yönetme.
- Bu anahtarı veya parolayı bir uygulamanın kullanması.

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

Platformlar Arası Komut Satırı Arabirimi yönergeleri için [bu eşdeğer öğreticiye](key-vault-manage-with-cli2.md) bakın.

## <a name="requirements"></a>Gereksinimler
Makaleye geçmeden önce aşağıdakilere sahip olduğunuzdan emin olun:

- **Bir Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/en-us/free/) için kaydolabilirsiniz.
- **Azure PowerShell**, **en düşük sürüm 1.1.0**. Azure PowerShell'i yüklemek ve Azure aboneliğinizle ilişkilendirmek için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Azure PowerShell'i zaten yüklediyseniz ve sürümünü bilmiyorsanız Azure PowerShell konsolunda `(Get-Module azure -ListAvailable).Version` yazın. Azure PowerShell'in 0.9.8 - 0.9.1 sürümleri arasında bir sürümü yüklüyse bazı küçük değişikliklerle bu öğreticiyi kullanmaya devam edebilirsiniz. Örneğin, `Switch-AzureMode AzureResourceManager` komutunu kullanmanız gerekir ve bazı Azure Anahtar Kasası komutları değişmiştir. 0.9.1 - 0.9.8 arasındaki sürümlere yönelik Anahtar Kasası cmdlet listesi için bkz. [Azure Anahtar Kasası Cmdlet'leri](/powershell/module/azurerm.keyvault/#key_vault).
- **Key Vault kullanacak şekilde yapılandırılabilecek bir uygulama**. [Microsoft Yükleme Merkezi](http://www.microsoft.com/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Yönergeler için ürünün beraberinde gelen **Benioku** dosyasına bakın.

>[!NOTE]
Bu makalede PowerShell ve Azure hakkında temel bilgilere sahip olduğunuz varsayılmıştır. PowerShell hakkında daha fazla bilgi için bkz. [Windows PowerShell ile çalışmaya başlama](https://technet.microsoft.com/library/hh857337.aspx).

Bu öğreticide gördüğünüz herhangi bir cmdlet hakkında ayrıntılı yardım almak için **Get-Help** cmdlet'ini kullanın.

```powershell-interactive
Get-Help <cmdlet-name> -Detailed
```
    
Örneğin, **Connect-AzureRmAccount** cmdlet’i hakkında yardım almak için şunu yazın:

```PowerShell
Get-Help Connect-AzureRmAccount -Detailed
```

Ayrıca, Azure PowerShell’de Azure Resource Manager dağıtım modeli hakkında bilgi edinmek için aşağıdaki makaleleri okuyabilirsiniz:

* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell’i Resource Manager ile kullanma](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Aboneliklerinize bağlanma
Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:  

```PowerShell
Connect-AzureRmAccount
```

>[!NOTE]
 Belirli bir Azure örneği kullanıyorsanız -Environment parametresini kullanın. Örnek: 
 ```powershell
 Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
 ```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Azure PowerShell bu hesapla ilişkili tüm abonelikleri alır ve varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa ve Azure Anahtar Kasası için kullanmak üzere belirli bir tanesini belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:

```powershell
Get-AzureRmSubscription
```

Ardından, kullanılacak aboneliği belirtmek için şunu yazın:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

Azure Power Shell'i yapılandırma hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a id="resource"></a>Yeni bir kaynak grubu oluşturma
Azure Resource Manager'ı kullandığınızda, tüm ilgili kaynaklar bir kaynak grubu içinde oluşturulur. Bu öğretici için **ContosoResourceGroup** adlı yeni bir kaynak grubu oluşturacağız:

```powershell
New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East US'
```

## <a id="vault"></a>Anahtar kasası oluşturma
Bir anahtar kasası oluşturmak için [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) cmdlet'ini kullanın. Bu cmdlet, üç zorunlu parametreye sahiptir: bir **kaynak grubu adı**, bir **anahtar kasası adı** ve **coğrafi konum**.

Örneğin, şunları kullanıyorsanız:
- **ContosoKeyVault** kasa adı.
- **ContosoResourceGroup** kaynak grubu adı.
- **Doğu ABD** konumu.

Şunu yazın:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```
![Anahtar Kasası oluşturma komutu tamamlandıktan sonraki çıktı](./media/key-vault-get-started/output-after-creating-keyvault.png)

Bu cmdlet’in çıktısı, oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **Kasa Adı**: Örnekte **ContosoKeyVault**'tur. Bu adı diğer Anahtar Kasası cmdlet'leri için kullanacaksınız.
* **Kasa URI’si**: Örnekte bu: https://contosokeyvault.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Henüz başka kimse yetkilendirilmedi.

> [!NOTE]
> Yeni anahtar kasanızı oluşturmaya çalışırken **Abonelik 'Microsoft.KeyVault' ad alanını kullanmak için kayıtlı değil** hatasını görebilirsiniz. Bu iletiyi görürseniz `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` komutunu çalıştırın. Kayıt başarıyla tamamlandıktan sonra New-AzureRmKeyVault komutunu yeniden çalıştırabilirsiniz. Daha fazla bilgi için bkz. [AzureRmResourceProvider'ı kaydetme](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Anahtar kasasına bir anahtar veya gizli anahtar ekleme
Key Vault ve anahtarlar ya da gizli dizilerle etkileşimde bulunmak için birkaç farklı yöntem gerekli olabilir.

### <a name="azure-key-vault-generates-a-software-protected-key"></a>Azure Key Vault, yazılım korumalı bir anahtar oluşturur

Azure Key Vault’un sizin için yazılım korumalı bir anahtar oluşturmasını istiyorsanız [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) cmdlet’ini kullanın ve şunu yazın:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'
```
Bu anahtar türü için URI’yi görüntüleme:
```powershell
$key.id
```

Oluşturduğunuz veya Azure Key Vault’a yüklediğiniz bu anahtara, URI’sini kullanarak başvurabilirsiniz. Geçerli sürümü almak için **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** ve **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** kullanarak bu sürümü alabilirsiniz.  

### <a name="importing-an-existing-pfx-file-into-azure-key-vault"></a>Mevcut PFX dosyasını Azure Key Vault’a aktarma

Azure Key Vault’a yüklemek istediğiniz bir pfx dosyasında depolanmış mevcut anahtarlar söz konusu olduğunda adımlar farklıdır. Örnek:
- .PFX dosyasında mevcut bir yazılım korumalı anahtarınız varsa
- Pfx dosyası softkey.pfx olarak adlandırılır 
- Dosya C sürücüsünde depolanır.

Şunu yazabilirsiniz:

```powershell
$securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force  // This stores the password 123 in the variable $securepfxpwd
```

Ardından, Anahtar Kasası hizmetinde yazılım ile anahtarı koruyan .PFX dosyasından anahtarı içeri aktarmak için aşağıdakini yazın:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoImportedPFX' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd
```

Bu anahtar için URI görüntülemek üzere şunu yazın:

```powershell
$Key.id
```
Anahtarınızı görüntülemek için şunu yazın: 

```powershell
Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'
```
Portalda PFX dosyasının özelliklerini görüntülemek isterseniz, aşağıdakine benzer bir görüntüyle karşılaşırsınız.

![Bir sertifikanın portaldaki görünüşü](./media/key-vault-get-started/imported-pfx.png)
### <a name="to-add-a-secret-to-azure-key-vault"></a>Azure Key Vault’a gizli dizi eklemek için

SQLPassword adlı bir parola olan ve Pa$$w0rd değerine sahip olan bir gizli diziyi Azure Key Vault’a eklemek için öncelikle şunları yazarak Pa$$w0rd değerini bir güvenli dizeye dönüştürün:

```powershell    
$secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force
```

Sonra şunu yazın:

```powershell
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue
```


Artık Azure Anahtar Kasası'na eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Her zaman geçerli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword** ve söz konusu sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** kullanın.

Bu parola için URI görüntülemek üzere şunu yazın:

```powershell
$secret.Id
```
Gizli dizinizi görüntülemek için şunu yazın: `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'` Veya alternatif olarak gizli diziyi portal üzerinde görüntüleyebilirsiniz.

![gizli dizi](./media/key-vault-get-started/secret-value.png)

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:
```powershell
(get-azurekeyvaultsecret -vaultName "Contosokeyvault" -name "SQLPassword").SecretValueText
```
Artık anahtar kasanız ve anahtarınız veya gizli anahtarınız uygulamaların kullanması için hazır. Uygulamaları bunları kullanmaları için yetkilendirmeniz gerekir.  

## <a id="register"></a>Bir uygulamayı Azure Active Directory’ye kaydetme
Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure Key Vault’a özgü değildir. Bir uygulamayı Azure Active Directory’ye kaydetme hakkında ayrıntılı adımlar için [Uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) veya [Portalı kullanarak Azure Active Directory uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md)

> [!IMPORTANT]
> Öğreticiyi tamamlamak için bu adımda kaydedeceğiniz hesabınızın, kasanızın ve uygulamanızın hepsinin aynı Azure dizininde olması gerekir.


Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir. Bunu yapmak için uygulama sahibinin öncelikle Azure Active Directory'sinde uygulamayı kaydetmesi gerekir. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

- **Uygulama Kimliği** 
- **Kimlik doğrulaması anahtarı** (paylaşılan gizli dizi olarak da bilinir). 

Uygulamanın bir belirteç almak için bu değerlerin her ikisini de Azure Active Directory'ye sunması gerekir. Uygulamanın bunu yapmak için nasıl yapılandırılacağı uygulamaya bağlıdır. [Key Vault örnek uygulaması](https://www.microsoft.com/download/details.aspx?id=45343) için uygulama sahibi bu değerleri app.config dosyasında ayarlar.


Bir uygulamayı Azure Active Directory'ye kaydetmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki **Uygulama kayıtları**’na tıklayın. Uygulama kayıtlarını görmezseniz **diğer hizmetler**’e tıklayıp burada bulabilirsiniz.  
>[!NOTE]
Anahtar kasanızı oluşturduğunuz Azure aboneliğini içeren dizini seçmeniz gerekir. 
3. **Yeni uygulama kaydı**’na tıklayın.
4. **Oluştur** dikey penceresinde uygulamanız için bir ad belirtin ve sonra **WEB UYGULAMASI VE/VEYA WEB API'Sİ** (varsayılan) seçeneğini belirleyip web uygulamanız için **OTURUM AÇMA URL’Sİ** değerini belirtin. Bu bilgilere şu anda sahip değilseniz, bu adım için rastgele değerler kullanabilirsiniz (örneğin, http://test1.contoso.com adresini belirtebilirsiniz). Bu sitelerin mevcut olup olmaması önemli değildir. 

    ![Yeni uygulama kaydı](./media/key-vault-get-started/new-application-registration.png)
    >[!WARNING]
    **WEB UYGULAMASI VE/VEYA WEB API’Sİ** seçeneğini belirlediğinizden emin olun; aksi takdirde ayarlar altında **anahtarlar** seçeneğini görmezsiniz.

5. **Oluştur** düğmesine tıklayın.
6. Uygulama kaydı tamamlandığında kayıtlı uygulamaların listesini görebilirsiniz. Yeni kaydettiğiniz uygulamayı bulup tıklayın.
7. **Kayıtlı uygulama** dikey penceresine tıklayıp **Uygulama Kimliği**’ni kopyalayın
8. **Tüm ayarlar**’a tıklayın
9. **Ayarlar** dikey penceresinde **anahtarlar**’a tıklayın
9. **Anahtar açıklaması** kutusuna bir açıklama girip süre seçin ve sonra **KAYDET**’e tıklayın. Sayfa yenilenir ve ardından bir anahtar değeri gösterir. 
10. Kasanızdaki izinleri ayarlamak için sonraki adımda **Uygulama Kimliği** ve **Anahtar** bilgilerini kullanacaksınız.

## <a id="authorize"></a>Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme
Uygulamaya, kasadaki anahtara veya gizli diziye erişme yetkisi vermenin iki yolu vardır.

### <a name="using-powershell"></a>PowerShell’i kullanma
PowerShell kullanmak için [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet’ini kullanın.

Örneğin, kasa adınız **ContosoKeyVault** ise ve yetkilendirmek istediğiniz uygulama 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed istemci kimliğine sahipse ve uygulamaya kasanızdaki anahtarların şifresini çözme ve bunlarla oturum açma yetkisi vermek istiyorsanız şunu çalıştırın:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız şunu çalıştırın:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get
```
### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Bir uygulamanın anahtarları veya gizli dizileri kullanma yetkisini değiştirmek için:
1. Anahtar Kasası kaynak dikey penceresinden **Erişim İlkeleri**’ni seçin
2. Dikey pencerenin üst kısmındaki [+ Yeni ekle] düğmesine tıklayın
3. Daha önce oluşturmuş olduğunuz uygulamayı seçmek için **Sorumlu Seç**’e tıklayın
4. **Anahtar izinleri** açılır listesinden "Şifre Çöz" ve "İmzala" seçeneğini belirleyerek uygulamaya kasanızdaki anahtarlarla şifre çözme ve imzalama yetkisi verin
5. **Gizli dizi izinleri** açılır listesinden "Al" seçeneğini belirleyerek uygulamanın kasadaki gizli dizileri okumasına izin verin

## <a id="HSM"></a>Donanım güvenlik modülü (HSM) ile çalışma
Ek güvence için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için [HSM korumalı anahtarları desteklemek için Azure Anahtar Kasası Premium hizmet katmanını](https://azure.microsoft.com/pricing/details/key-vault/) kullanmanız gerekir. Ek olarak, bu işlevin Azure Çin'de kullanılamadığını unutmayın.

Anahtar kasasını oluşturduğunuzda **-SKU** parametresini ekleyin:

```powershell
New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US' -SKU 'Premium'
```


Bu anahtar kasasına yazılım korumalı anahtarlar (daha önce gösterildiği gibi) ve HSM korumalı anahtarlar ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için **-Destination** parametresini 'HSM' olarak ayarlayın.

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'
```

Bilgisayarınızdaki bir .PFX dosyasından bir anahtarı içeri aktarmak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'
```

Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Bu senaryo anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanızı ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanızı sağlar:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'
```

Bu BYOK paketini oluşturma hakkında daha ayrıntılı yönergeler için bkz. [Azure Anahtar Kasası için HSM korumalı anahtarlar oluşturma ve aktarma](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Anahtar kasasını, ilişkili anahtarları ve gizli anahtarları silme
Anahtar kasasına veya içerdiği anahtar veya gizli anahtara artık ihtiyacınız yoksa anahtar kasasını [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault) cmdlet'ini kullanarak silebilirsiniz.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'
```

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'
```

## <a id="other"></a>Diğer Azure PowerShell Cmdlet’leri
Azure Anahtar Kasası'nı yönetmede kullanışlı bulabileceğiniz diğer komutlar:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Bu komut, tüm anahtarların ve seçilen özelliklerin tablosal bir görüntüsünü alır.
- `$Keys[0]`: Bu komut, belirtilen anahtar için özelliklerin tam bir listesini görüntüler
- `Get-AzureKeyVaultSecret`: Bu komut, tüm gizli adların ve seçilen özelliklerin tablosal bir görüntüsünü listeler.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Belirli bir anahtarın nasıl kaldırılacağına örnek verir.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Belirli bir gizli anahtarın nasıl kaldırılacağına örnek verir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)
- Anahtar kasanızın nasıl kullanıldığını görmek için bkz. [Azure Anahtar Kasası Günlüğü](key-vault-logging.md).
- Azure Anahtar Kasası'nı bir web uygulamasında kullanan bir izleme öğreticisi için bkz. [Azure Anahtar Kasası'nı bir Web Uygulamasından Kullanma](key-vault-use-from-web-application.md).
- Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).
- Azure Anahtar Kasası'na yönelik en son Azure PowerShell cmdlet'leri listesi için bkz. [Azure Anahtar Kasası Cmdlet'leri](/powershell/module/azurerm.keyvault/#key_vault).
