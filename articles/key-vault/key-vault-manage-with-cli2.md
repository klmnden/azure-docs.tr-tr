---
title: CLI kullanarak Azure Key Vault yönetme | Microsoft Docs
description: CLI 2.0 kullanarak anahtar Kasası'nda ortak görevleri otomatik hale getirmek için bu makaleyi kullanın.
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
ms.openlocfilehash: 47a78b71f51e4fe975341b8e9425f47fd8c4d31c
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503545"
---
# <a name="manage-key-vault-using-cli-20"></a>CLI 2.0 kullanarak Key Vault yönetme

Bu makalede, Azure CLI 2.0 kullanarak Azure Key Vault ile çalışmaya başlamak nasıl ele alınmaktadır. Şirket bilgileri görebilirsiniz:
- Azure'da sağlamlaştırılmış bir kapsayıcı (kasa) oluşturma
- Depolamak ve şifreleme anahtarlarını ve gizli anahtarları azure'da yönetmek nasıl. 
- Bir kasa oluşturmak için Azure CLI kullanarak.
- Bir anahtar veya ardından bir Azure uygulamasıyla kullanabileceğiniz parola oluşturuluyor. 
- Nasıl bir uygulama oluşturulan anahtarı veya parola kullanabilirsiniz.

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).


> [!NOTE]
> Bu makalede adımlarından biri, bir anahtar veya gizli anahtar Kasası'nda kullanmak için bir uygulamanın nasıl gösterir içerdiğini Azure uygulaması yazma yönergeleri içermez.
>

Azure anahtar kasası genel bakış için bkz. [Azure anahtar kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar
Bu makalede Azure CLI komutlarını kullanmak için aşağıdakilerin olması gerekir:

* Bir Microsoft Azure aboneliği. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial).
* Komut satırı arabirimi 2.0 veya sonraki bir sürümü. En son sürümünü yüklemek için bkz: [yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi 2.0](/cli/azure/install-azure-cli).
* Bir anahtar ya da bu makalede oluşturduğunuz parola kullanmak için yapılandırılmış bir uygulama. [Microsoft Yükleme Merkezi](http://www.microsoft.com/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Yönergeler için dahil edilen Benioku dosyasına bakın.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure platformlar arası komut satırı arabirimi ile Yardım alma
Bu makalede, komut satırı arabirimi (Bash, Terminal, komut istemi) ile ilgili bilgi sahibi olduğunuz varsayılır.

Yardım veya -h parametresi, özel komutlar için Yardım görüntülemek için kullanılabilir. Alternatif olarak, Azure help [komut] [Seçenekler] biçimi de çok kullanılabilir. Emin olamadığınız durumlarda bir komut tarafından gerekli parametreler hakkında daha fazla yardıma başvurun. Örneğin, tüm aşağıdaki komutları aynı bilgileri döndürür:

```azurecli-interactive
az account set --help
az account set -h
```

Ayrıca Azure platformlar arası komut satırı arabirimi içinde Azure Resource Manager hakkında bilgi almak için aşağıdaki makaleleri okuyabilirsiniz:

* [Azure CLI'yı yükleme](/cli/azure/install-azure-cli)
* [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Aboneliklerinize bağlanma

Etkileşimli olarak oturum açmak için aşağıdaki komutu kullanın:

```azurecli
az login
```
Bir kurumsal hesap kullanarak oturum açması, kullanıcı adı ve parola geçirebilirsiniz.

```azurecli
az login -u username@domain.com -p password
```

Birden fazla aboneliğiniz varsa ve kullanılacağını belirtmek ihtiyacınız varsa, hesabınız için abonelikleri görmek için aşağıdaki komutu yazın:

```azurecli
az account list
```

Bir aboneliğe abonelik parametresiyle belirtirsiniz.

```azurecli
az account set --subscription <subscription name or ID>
```

Azure platformlar arası komut satırı arabirimi yapılandırma hakkında daha fazla bilgi için bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Yeni bir kaynak grubu oluşturma
Azure Resource Manager kullanırken, tüm ilgili kaynakları bir kaynak grubu içinde oluşturulur. Mevcut bir kaynak grubunda bir anahtar kasası oluşturabilirsiniz. Yeni bir kaynak grubu kullanmak istiyorsanız, yeni bir tane oluşturabilirsiniz.

```azurecli
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

İlk parametre kaynak grubu adı ve ikinci parametre konumu. Tüm olası bir listesini almak için konumları yazın:

```azurecli
az account list-locations
``` 

## <a name="register-the-key-vault-resource-provider"></a>Key Vault kaynak sağlayıcısını kaydetme
 "Abonelik 'Microsoft.KeyVault' ad alanını kullanacak şekilde kaydedilmemiş" hatasıyla karşılaşabilirsiniz çalıştığınızda yeni bir anahtar kasası oluşturun. Bu ileti görüntülenirse, bu anahtar kasası kaynak sağlayıcısının aboneliğinize kayıtlı olduğundan emin olun. Bu, her bir abonelik için tek seferlik bir işlemdir.

```azurecli
az provider register -n Microsoft.KeyVault
```


## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kullanım `az keyvault create` anahtar kasası oluşturma komutu. Bu betik, üç zorunlu parametreye sahiptir: bir kaynak grubu adı, anahtar kasası adı ve coğrafi konumu.

Adlı yeni bir kasa oluşturun için **ContosoKeyVault**, kaynak grubundaki **ContosoResourceGroup**, içindeki kaynaklarınızda bulunan **Doğu Asya** konumu, türü: 

```azurecli
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Bu komutun çıktısı, oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **ad**: Bu örnekte ContosoKeyVault addır. Bu adı diğer Key Vault komutları için kullanacaksınız.
* **vaultUri**: Bu örnekte URI'dir https://contosokeyvault.vault.azure.net. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Henüz itibariyle, sizden başka kimse yetkisi.

## <a name="add-a-key-secret-or-certificate-to-the-key-vault"></a>Bir anahtar kasasına bir anahtar, parola veya sertifika Ekle

Azure anahtar Kasası'nın sizin için yazılım korumalı bir anahtar oluşturmasını istiyorsanız kullanın `az key create` komutu.

```azurecli
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```

.Pem dosyasında var olan bir anahtar varsa, Azure anahtar Kasası'na karşıya yükleyebilirsiniz. Yazılım veya HSM anahtarıyla korumak seçebilirsiniz. .Pem dosyasından anahtarı içeri aktarma ve yazılım ile korumak için aşağıdakileri kullanın:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'Pa$$w0rd' --protection software
```

Artık oluşturduğunuz veya Azure anahtar Kasası'na URI'sini kullanarak yüklediğiniz anahtar başvurabilirsiniz. Kullanım **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** her zaman geçerli sürümü almak için. Kullanım https://[keyvault-name].vault.azure.net/keys/[keyname]/[key-unique-id] bu belirli sürümü almak için. Örneğin, **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**. 

SQLPassword adlı bir parola olan ve Pa$ $w0rd değerini Azure anahtar kasaları için değer olan kasaya bir gizli dizi ekleyin. 

```azurecli
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```

Bu parola, URI'sini kullanarak başvuru. Kullanım **https://ContosoVault.vault.azure.net/secrets/SQLPassword** her zaman geçerli sürümü ve https://[keyvault-name].vault.azure.net/secret/[secret-name]/[secret-unique-id almak için] bu belirli sürümü almak için. Örneğin, **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Sertifika .pem veya .pfx kullanarak kasaya içeri aktarın.

```azurecli
az keyvault certificate import --vault-name 'ContosoKeyVault' --file 'c:\cert\cert.pfx' --name 'ContosoCert' --password 'Pa$$w0rd'
```

Anahtar, gizli ya da oluşturduğunuz sertifika şimdi görüntüleyin:

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
Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure Key Vault'a özgü değildir, ancak burada için tanıma dahildir. Uygulama kaydı tamamlamak için hesabınızın, kasa ve uygulama aynı Azure dizininde olması gerekir.

Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir.  Uygulama sahibinin öncelikle Azure Active Directory'de kaydetmelisiniz. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

- Bir **uygulama kimliği** (AAD istemci kimliği veya AppID olarak da bilinir)
- **Kimlik doğrulaması anahtarı** (paylaşılan gizli dizi olarak da bilinir). 

Uygulamanın bir belirteç almak için bu değerlerin her ikisini de Azure Active Directory'ye sunması gerekir. Bir uygulama bir belirteç almak üzere nasıl yapılandırılacağı uygulamaya göre değişir. [Key Vault örnek uygulaması](https://www.microsoft.com/download/details.aspx?id=45343) için uygulama sahibi bu değerleri app.config dosyasında ayarlar.

Bir uygulamayı Azure Active Directory'ye kaydetme hakkında ayrıntılı adımlar için başlıklı makaleleri gözden geçirmeniz gereken [uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md), [Azure Active oluşturmak için portalı kullanma Dizin uygulaması ve kaynaklara erişebilen hizmet sorumlusu](../azure-resource-manager/resource-group-create-service-principal-portal.md), ve [Azure, Azure CLI 2.0 ile hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli).

Bir uygulamayı Azure Active Directory'ye kaydetmek için:

```azurecli
az ad sp create-for-rbac -n "MyApp" --password 'Pa$$w0rd' --skip-assignment
# If you don't specify a password, one will be created for you.
```

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme

Anahtar veya gizli anahtarı kasadaki erişmek için uygulamayı yetkilendirme için kullanın `az keyvault set-policy` komutu.

Örneğin, kasa adınızdır ContosoKeyVault, uygulamanın bir AppID 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed varsa ve kasanızdaki anahtarlarla oturum ve şifresini çözmek için yetkilendirmek istiyorsanız aşağıdaki komutu kullanın:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek için aşağıdaki komutu yazın:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```

## <a name="bkmk_KVperCLI"></a> Set anahtar kasası erişim ilkeleri Gelişmiş 
Kullanım [az keyvault update](/cli/azure/keyvault#az-keyvault-update) anahtar kasası için Gelişmiş ilkelerini etkinleştirmek için. 

 Key Vault dağıtım için etkinleştir: kasasından gizli diziler olarak depolanan sertifikaları almak için sanal makineler sağlar.
 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-deployment 'true'
 ``` 

Anahtar kasası disk şifrelemesi için etkinleştir: kasa, Azure Disk şifrelemesi için kullanılırken gereklidir.

 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-disk-encryption 'true'
 ```  

Şablon dağıtımı için Key Vault etkinleştir: gizli dizileri kasadan almak için Resource Manager sağlar.
 ```azurecli 
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-template-deployment 'true'
 ```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Bir donanım güvenlik modülü (HSM) kullanmak istiyorsanız

Ek güvence için HSM sınırını asla terk donanım güvenlik modülleri (HSM'ler) anahtarları oluşturmak veya içeri aktarma kullanabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete-the-key-vault-and-associated-keys-and-secrets)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için HSM korumalı anahtarları destekleyen bir kasa aboneliğinizin olması gerekir.

Anahtar kasası oluşturduğunuzda 'sku' parametresini ekleyin:

```azurecli
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

Bu kasaya yazılım korumalı anahtarlar ve HSM korumalı anahtarlar (daha önce gösterildiği gibi) ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için hedef parametresini 'HSM' ayarlayın:

```azurecli
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

Bilgisayarınızda bir .pem dosyasından bir anahtarı içeri aktarmak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```

Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Böylece anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanız ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanız sağlanır.

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```

Daha ayrıntılı bu BYOK paketini oluşturma hakkında yönergeler için bkz. [HSM-Protected anahtarları Azure Key Vault ile kullanmak nasıl](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme

Key vault ve anahtarlar veya parolalar artık ihtiyacınız yoksa anahtar kasasını kullanarak silebilirsiniz `az keyvault delete` komutu:

```azurecli
az keyvault delete --name 'ContosoKeyVault'
```

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

```azurecli
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Diğer Azure platformlar arası komut satırı arabirimi komutları

Azure anahtar Kasası'nı yönetmede kullanışlı bulabileceğiniz diğer komutlar.

Bu komut tüm anahtarların ve seçilen özelliklerin tablosal bir görüntüsünü listeler:

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

Bu komut, belirtilen anahtar için özelliklerin tam bir listesini görüntüler:

```azurecli
az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Bu komut tüm gizli adların ve seçilen özelliklerin tablosal bir görüntüsünü listeler:

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

Belirli bir anahtarın nasıl kaldırılacağına örneği aşağıda verilmiştir:

```azurecli
az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Belirli bir gizli dizi kaldırmaya ilişkin bir örnek aşağıda verilmiştir:

```azurecli
az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'
```

## <a name="next-steps"></a>Sonraki adımlar

- Anahtar kasası komutları için tam Azure CLI başvurusu için bkz. [anahtar kasası CLI başvurusu](/cli/azure/keyvault).

- Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

- Azure Key Vault ve HSM'ler hakkında daha fazla bilgi için bkz: [HSM-Protected anahtarları Azure Key Vault ile kullanmak nasıl](key-vault-hsm-protected-keys.md).
