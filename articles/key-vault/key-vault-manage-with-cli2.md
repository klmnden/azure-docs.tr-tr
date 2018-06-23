---
title: Azure anahtar kasası CLI kullanarak yönetme | Microsoft Docs
description: CLI 2.0 kullanarak anahtar kasası ortak görevleri otomatikleştirmek için bu makaleyi kullanın
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: barclayn
ms.openlocfilehash: 8c2501b5e89e81709de074c0b0c93b317ecebd7b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316603"
---
# <a name="manage-key-vault-using-cli-20"></a>Anahtar kasası CLI 2.0 kullanarak yönetme

Bu makalede, Azure CLI 2.0 kullanan Azure anahtar kasası ile çalışmaya başlamak alınmaktadır. Hakkında bilgi bulabilirsiniz:
- Azure'da sağlamlaştırılmış bir kapsayıcı (bir kasa) oluşturma
- Depolamak ve şifreleme anahtarları ve gizli anahtarları azure'da yönetmek nasıl. 
- Bir kasa oluşturmak için Azure CLI kullanma.
- Bir anahtar ya da sonra bir Azure uygulamasıyla kullanabileceğiniz parola oluşturma. 
- Bir uygulamanın nasıl oluşturulan anahtarı veya parolayı kullanabilirsiniz.

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).


> [!NOTE]
> Bu makalede adımlardan biri, bir anahtar veya gizli anahtarı kasaya kullanmak için bir uygulamanın nasıl gösterir içerdiğini Azure uygulaması yazma yönergeler içermez.
>

Azure anahtar kasası genel bakış için bkz: [Azure anahtar kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar
Bu makalede Azure CLI komutları kullanmak için aşağıdakilerin olması gerekir:

* Bir Microsoft Azure aboneliği. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial).
* Komut satırı arabirimi sürüm 2.0 veya üstü. En son sürümünü yüklemek için bkz: [Azure platformlar arası komut satırı arabirimi 2.0 yükleyip](/cli/azure/install-azure-cli).
* Anahtar veya bu makalede oluşturduğunuz parolayı kullanmak üzere yapılandırılmış bir uygulama. [Microsoft Yükleme Merkezi](http://www.microsoft.com/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Yönergeler için gelen Benioku dosyasına bakın.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure platformlar arası komut satırı arabirimi ile ilgili Yardım alma
Bu makalede, komut satırı arabirimi (Bash, Terminal, komut istemi) hakkında bilgi sahibi olduğunuz varsayılır.

Yardım veya -h parametresi, belirli komutlar için Yardım görüntülemek için kullanılabilir. Alternatif olarak, Azure Yardım [[Seçenekler] biçimi de çok kullanılabilir komut]. Zaman şüpheli bir komut tarafından gerekli parametreleri hakkında yardıma başvurun. Örneğin, aşağıdaki komutları tüm aynı bilgileri döndürün:

```azurecli-interactive
az account set --help
az account set -h
```

Ayrıca, Azure platformlar arası komut satırı arabirimi ile Azure Resource Manager hakkında bilgi edinmek için aşağıdaki makalelere okuyabilirsiniz:

* [Azure CLI'yı yükleme](/cli/azure/install-azure-cli)
* [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Aboneliklerinize bağlanma

Etkileşimli olarak oturum açmak için aşağıdaki komutu kullanın:

```azurecli
az login
```
Bir kurumsal hesap kullanarak oturum açması, kullanıcı adı ve parolanızı geçirebilirsiniz.

```azurecli
az login -u username@domain.com -p password
```

Birden fazla aboneliğiniz varsa ve kullanılacağı belirtmeniz gerekir, hesabınız için abonelikleri görmek için aşağıdaki komutu yazın:

```azurecli
az account list
```

Bir abonelik abonelik parametresiyle belirtin.

```azurecli
az account set --subscription <subscription name or ID>
```

Azure platformlar arası komut satırı arabirimi yapılandırma hakkında daha fazla bilgi için bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Yeni bir kaynak grubu oluşturma
Azure Kaynak Yöneticisi'ni kullanırken, tüm ilgili kaynaklar bir kaynak grubu içinde oluşturulur. Bir anahtar kasası varolan bir kaynak grubu oluşturabilirsiniz. Yeni bir kaynak grubu kullanmak istiyorsanız, yeni bir tane oluşturabilirsiniz.

```azurecli
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

İlk parametre kaynak grubu adı ve ikinci parametre konumu. Tüm olası bir listesini almak için konumları yazın:

```azurecli
az account list-locations
``` 

## <a name="register-the-key-vault-resource-provider"></a>Anahtar kasası kaynak sağlayıcısını Kaydet
 "Abonelik 'Microsoft.KeyVault' ad alanını kullanmak için kayıtlı değil" hata görebilirsiniz çalıştığınızda yeni bir anahtar kasası oluşturun. Bu ileti görüntülenirse, bu anahtar kasası kaynak sağlayıcısı aboneliğinizde kayıtlı olduğundan emin olun. Bu, her bir abonelik için tek seferlik bir işlemdir.

```azurecli
az provider register -n Microsoft.KeyVault
```


## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kullanım `az keyvault create` bir anahtar kasası oluşturmak için komutu. Bu komut dosyasını üç zorunlu parametreye sahiptir: bir kaynak grubu adı, bir anahtar kasası adı ve coğrafi konum.

Adlı yeni bir kasa oluşturmak için **ContosoKeyVault**, kaynak grubundaki **ContosoResourceGroup**, içinde bulunan **Doğu Asya** konum, türü: 

```azurecli
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Bu komutun çıktısı, oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **ad**: örnekte ContosoKeyVault addır. Bu adı diğer anahtar kasası komutları için kullanacaksınız.
* **vaultUri**: örnekte URI'dir https://contosokeyvault.vault.azure.net. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Kimse henüz itibariyle, yetkili.

## <a name="add-a-key-secret-or-certificate-to-the-key-vault"></a>Bir anahtar, parola veya sertifika anahtar Kasası'na ekleyin

Azure anahtar Kasası'nın yazılım korumalı bir anahtar oluşturmasını istiyorsanız, kullanmak `az key create` komutu.

```azurecli
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```

.Pem dosyasında mevcut bir anahtarı varsa, Azure anahtar Kasası'na karşıya yükleyebilirsiniz. Yazılım ya da HSM anahtarıyla korumak seçebilirsiniz. .Pem dosyasından anahtarı içeri aktarmak ve yazılım ile korumak için aşağıdakileri kullanın:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'Pa$$w0rd' --protection software
```

Artık oluşturduğunuz veya Azure anahtar Kasası'na URI'sini kullanarak karşıya anahtar başvurabilirsiniz. Kullanım **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** her zaman geçerli sürümü almak için. Kullanım https://[keyvault-name].vault.azure.net/keys/[keyname]/[key-unique-id] bu belirli sürümü almak için. Örneğin, **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**. 

SQLPassword adlı bir parola olduğu ve değer, Pa$ $w0rd Azure anahtar kasası için olan kasasına gizli ekleyin. 

```azurecli
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```

Bu parolayı URI'sini kullanarak başvuru. Kullanım **https://ContosoVault.vault.azure.net/secrets/SQLPassword** her zaman geçerli sürümü ve https://[keyvault-name].vault.azure.net/secret/[secret-name]/[secret-unique-id almak için] bu belirli sürümü almak için. Örneğin, **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Bir sertifika .pem veya .pfx kullanarak Kasası'na aktarın.

```azurecli
az keyvault certificate import --vault-name 'ContosoKeyVault' --file 'c:\cert\cert.pfx' --name 'ContosoCert' --password 'Pa$$w0rd'
```

Şimdi anahtarı, gizli veya oluşturduğunuz sertifika görüntüleyin:

* Anahtarlarınızı görüntülemek için şunu yazın: 

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

* Gizli anahtarlarınız görüntülemek için şunu yazın: 

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

* Sertifikaları görüntülemek için şunu yazın: 

```azurecli
az keyvault certificate list --vault-name 'ContosoKeyVault'
```

## <a name="register-an-application-with-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'ye kaydetme
Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure anahtar Kasası'na özgü değildir, ancak burada için tanıma bulunur. Uygulama kayıt işlemini tamamlamak için hesabınızı, kasaya ve uygulama aynı Azure dizininde olması gerekir.

Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir.  Uygulama sahibi, Azure Active Directory'de önce kaydetmeniz gerekir. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

- Bir **uygulama kimliği** (AAD istemci Kimliğini veya uygulama kimliği olarak da bilinir)
- **Kimlik doğrulaması anahtarı** (paylaşılan gizli dizi olarak da bilinir). 

Uygulamanın bir belirteç almak için bu değerlerin her ikisini de Azure Active Directory'ye sunması gerekir. Bir uygulama bir belirteç almak üzere nasıl yapılandırılır, uygulamaya göre değişir. [Key Vault örnek uygulaması](https://www.microsoft.com/download/details.aspx?id=45343) için uygulama sahibi bu değerleri app.config dosyasında ayarlar.

Uygulama Azure Active Directory ile kaydetme hakkında ayrıntılı adımlar için başlıklı makaleleri gözden geçirmeniz gereken [uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/active-directory-integrating-applications.md), [kullanım portalı bir Azure Active oluşturmak için Dizin uygulama ve kaynaklarına erişebilir hizmet sorumlusu](../azure-resource-manager/resource-group-create-service-principal-portal.md), ve [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturmak](/cli/azure/create-an-azure-service-principal-azure-cli).

Bir uygulamayı Azure Active Directory'ye kaydetmek için:

```azurecli
az ad sp create-for-rbac -n "MyApp" --password 'Pa$$w0rd' --skip-assignment
# If you don't specify a password, one will be created for you.
```

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme

Anahtar veya gizli kasadaki erişmek için uygulama yetkilendirmek için `az keyvault set-policy` komutu.

Örneğin, kasa adınızdır ContosoKeyVault, uygulama bir AppID 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, varsa ve şifresini çözmek ve kasanızı anahtarlarında oturum açmak için uygulama yetkilendirmek istediğiniz, aşağıdaki komutu kullanın:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek için aşağıdaki komutu yazın:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```

## <a name="bkmk_KVperCLI"></a> Set anahtar kasası erişim ilkelerini Gelişmiş 
Kullanım [az keyvault güncelleştirme](/cli/azure/keyvault#az-keyvault-update) anahtar kasası için Gelişmiş ilkelerini etkinleştirmek için. 

 Anahtar kasası dağıtım için etkinleştir: sanal makinelerin Kasası'ndan gizlilikleri olarak depolanan sertifikalar almaya izin verir.
 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-deployment 'true'
 ``` 

Anahtar kasası için disk şifrelemeyi etkinleştirir: kasa için Azure Disk şifrelemesi kullanılırken gereklidir.

 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-disk-encryption 'true'
 ```  

Anahtar kasası için şablon dağıtımını etkinleştirmek: kasadan parolaları almak için Resource Manager sağlar.
 ```azurecli 
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-template-deployment 'true'
 ```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Bir donanım güvenlik modülü (HSM) kullanmak istiyorsanız

Ek güvence için alma ya da HSM sınırını asla bırakın donanım güvenlik modülleri (HSM'ler) anahtarları oluştur. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete-the-key-vault-and-associated-keys-and-secrets)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için HSM korumalı anahtarları destekleyen bir kasa aboneliğinizin olması gerekir.

Keyvault oluşturduğunuzda 'sku' parametresini ekleyin:

```azurecli
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

Bu kasaya yazılım korumalı anahtarlar ve HSM korumalı anahtarlar (daha önce gösterildiği gibi) ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için hedef parametresini 'HSM' ayarlayın:

```azurecli
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

.Pem dosyasını bilgisayarınızdaki bir anahtar almak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```

Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Böylece anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanız ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanız sağlanır.

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```

Daha ayrıntılı bu BYOK paketini oluşturma hakkında yönergeler için bkz: [HSM-Protected anahtarları Azure anahtar kasası ile kullanmak nasıl](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme

Anahtar kasası ve anahtarları veya gizli artık ihtiyacınız varsa, anahtar kasası kullanarak silebilirsiniz `az keyvault delete` komutu:

```azurecli
az keyvault delete --name 'ContosoKeyVault'
```

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

```azurecli
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Diğer Azure platformlar arası komut satırı arabirimi komutları

Azure anahtar Kasası'nı yönetmede kullanışlı bulabileceğiniz diğer komutlar.

Bu komut tüm anahtarların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

Bu komut belirtilen anahtar için özelliklerin tam listesini görüntüler:

```azurecli
az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Bu komut tüm gizli adların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

Belirli bir anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

```azurecli
az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Belirli bir gizli anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

```azurecli
az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'
```

## <a name="next-steps"></a>Sonraki adımlar

- Anahtar kasası komutları için tam Azure CLI başvuru için bkz: [anahtar kasası CLI başvuru](/cli/azure/keyvault).

- Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

- Azure anahtar kasası ve HSM'ler hakkında daha fazla bilgi için bkz: [HSM-Protected anahtarları Azure anahtar kasası ile kullanmak nasıl](key-vault-hsm-protected-keys.md).
