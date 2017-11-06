---
title: "Bir Linux VM Azure CLI 1.0 ile disklerde şifrelemek | Microsoft Docs"
description: "Azure CLI 1.0 ve Resource Manager dağıtım modeli kullanarak bir Linux VM disklerde şifreleme"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: b436f2d43c41000f4385889edb3fa3983d4a8c66
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli-10"></a>Azure CLI 1.0 kullanarak bir Linux VM disklerde şifrele
Geliştirilmiş sanal makine (VM) güvenlik ve uyumluluk için Azure sanal diskleri bekleyen şifrelenebilir. Diskleri, bir Azure anahtar kasası güvenli şifreleme anahtarları kullanılarak şifrelenir. Bu şifreleme anahtarları denetlemek ve bunların kullanılması denetleyebilirsiniz. Bu makalede Azure CLI 1.0 ve Resource Manager dağıtım modeli kullanarak bir Linux VM sanal disklerde şifrelemek nasıl ayrıntılarını verir.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="quick-commands"></a>Hızlı komutlar
VM sanal disklerde şifrelemek için hızlı bir şekilde, aşağıdaki bölümde ayrıntıları temel görevi gerekiyorsa komutları. Her adım, belgenin geri kalanında bulunabilir bilgi ve içerik daha ayrıntılı [burada başlangıç](#overview-of-disk-encryption).

Gereksinim duyduğunuz [en son Azure CLI 1.0](../../xplat-cli-install.md) yüklü ve Resource Manager modunu aşağıdaki gibi kullanarak oturum:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `myKeyVault`, ve `myVM`.

İlk olarak, Azure aboneliğinizde içinde Azure anahtar kasası Sağlayıcısı'nı etkinleştirin ve bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu adı oluşturur `myResourceGroup` içinde `WestUS` konumu:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Azure anahtar kasası oluşturun. Aşağıdaki örnek adlı bir anahtar kasası oluşturur `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Bir şifreleme anahtarı anahtar kasanızı oluşturmak ve disk şifrelemesi için etkinleştirin. Aşağıdaki örnek adlı bir anahtar oluşturur `myKey`:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Kimlik doğrulama işleme ve şifreleme anahtarlarının anahtar Kasası'ndan değişimi için Azure Active Directory'yi kullanarak bir uç nokta oluşturun. `--home-page` Ve `--identifier-uris` gerçek yönlendirilebilir adresi olması gerekmez. Yüksek düzeyde güvenlik için istemci gizli yerine parolalar kullanılmalıdır. Azure CLI istemci gizli şu anda oluşturulamıyor. İstemci gizli yalnızca Azure portalında oluşturulabilir. Aşağıdaki örnek adlı bir Azure Active Directory uç nokta oluşturuyor `myAADApp` ve bir parola kullanıyorsa `myPassword`. Kendi parolanızı aşağıdaki gibi belirtin:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Not `applicationId` yukarıdaki komut çıktısı gösterilmektedir. Bu uygulama kimliği, aşağıdaki adımlarda kullanılır:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Bir veri diski için mevcut bir VM'yi ekleyin. Aşağıdaki örnek, adlı VM için bir veri diski ekler `myVM`:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Anahtar kasası ve oluşturduğunuz anahtarı için ayrıntıları inceleyin. Anahtar kasası kimliği, URI ve anahtar gerek son adımda URL. Aşağıdaki örnek ayrıntıları adlı bir anahtar kasası için incelemeleri `myKeyVault` ve adlı anahtar `myKey`:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Disklerinizi gibi kendi parametre adları boyunca girme şifrelemek:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Azure CLI ayrıntılı hataları şifreleme işlemi sırasında sağlamaz. Ek sorun giderme bilgileri için inceleyin `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Yukarıdaki komut birçok değişkeni yok ve işlem neden başarısız konusunda kadar göstergesi alamayabilirsiniz gibi tam komut örneği aşağıdaki gibi olur:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Son olarak, sanal diskler artık şifrelenmiş olduğunu onaylamak için yeniden şifreleme durumunu gözden geçirin. Aşağıdaki örnek adlı bir VM'den durumunu denetler `myVM` içinde `myResourceGroup` kaynak grubu:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Disk şifrelemesi'ne genel bakış
Linux sanal makineleri sanal disklerde rest kullanarak şifrelenir [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Azure sanal diskleri şifreleme ücretsizdir. Şifreleme anahtarları yazılım koruması'nı kullanarak Azure anahtar kasası içinde depolanır veya içe aktarmak ya da anahtarlarınızı FIPS 140-2 Düzey 2 standartları sertifikalı donanım güvenlik modüllerinde (HSM'ler) oluşturur. Bu şifreleme anahtarları denetimin korumak ve kullanımlarını denetleyebilirsiniz. Bu şifreleme anahtarlarını şifrelemek ve şifresini çözmek, VM'ye bağlı sanal diskler için kullanılır. Bir Azure Active Directory uç nokta VM'ler açma ve kapatma gücü gibi bu şifreleme anahtarları verme için güvenli bir mekanizma sağlar.

Bir VM şifreleme işlemi aşağıdaki gibidir:

1. Bir şifreleme anahtarının bir Azure anahtar kasası oluşturun.
2. Diskleri şifrelemek için kullanılabilmesi için şifreleme anahtarını yapılandırın.
3. Şifreleme anahtarı Azure anahtar Kasası'okumak için Azure Active Directory'yi kullanarak uygun izinlere sahip bir uç nokta oluşturun.
4. Kullanılacak uygun şifreleme anahtarını ve Azure Active Directory uç noktası belirterek, sanal diskler şifrelemek için komutu yürütün.
5. Azure Active Directory uç nokta gereken şifreleme anahtarını Azure anahtar Kasası'ister.
6. Sanal diskler sağlanan şifreleme anahtarı kullanılarak şifrelenir.

## <a name="supporting-services-and-encryption-process"></a>Destek Hizmetleri ve şifreleme işlemi
Disk şifrelemesi aşağıdaki ek bileşenlerini dayanır:

* **Azure anahtar kasası** - şifreleme anahtarları ve gizli anahtarları disk şifreleme/şifre çözme işlemi için kullanılan korumak için kullanılan.
  * Varsa, mevcut bir Azure anahtar kasası kullanabilirsiniz. Diskleri şifrelemek için bir anahtar kasası ayrılması gerekmez.
  * Yönetim sınırlarını ve anahtar görünürlük ayırmak için ayrılmış bir anahtar kasası oluşturabilirsiniz.
* **Azure Active Directory** -güvenli değişimi gerekli şifreleme anahtarlarını ve kimlik doğrulaması istenen eylemler için işler.
  * Genellikle, uygulamanızı barındırmak için var olan bir Azure Active Directory örneğini kullanabilirsiniz.
  * Daha fazla istemek ve uygun şifreleme anahtarları verilen anahtar kasası ve sanal makine Hizmetleri için bir uç nokta uygulamasıdır. Azure Active Directory ile tümleşen gerçek uygulama geliştirme değil.

## <a name="requirements-and-limitations"></a>Gereksinimler ve sınırlamalar
Desteklenen senaryolar ve disk şifrelemesi için gereksinimleri:

* Aşağıdaki Linux sunucusu SKU'ları - Ubuntu ve CentOS, SUSE ve SUSE Linux Enterprise Server (SLES) ve Red Hat Enterprise Linux.
* Tüm kaynaklar (örneğin, anahtar kasası, depolama hesabı ve VM) aynı Azure bölgesinde ve abonelik olmalıdır.
* Standart bir, D, DS, G ve GS serisi VM'ler.

Disk şifrelemesi aşağıdaki senaryolarda şu anda desteklenmiyor:

* Temel katman VM'ler.
* Klasik dağıtım modeli kullanılarak oluşturulan VM'ler.
* İşletim sistemi disk şifrelemesi Linux sanal makineleri üzerinde devre dışı bırakılıyor.
* Zaten şifrelenmiş bir Linux VM üzerinde şifreleme anahtarlarını güncelleştiriliyor.

## <a name="create-the-azure-key-vault-and-keys"></a>Azure anahtar kasası ve anahtarları oluşturma
Bu kılavuz kalanını tamamlamak için ihtiyacınız [en son Azure CLI 1.0](../../xplat-cli-install.md) yüklü ve Resource Manager modunu aşağıdaki gibi kullanarak oturum:

```azurecli
azure config mode arm
```

Komut örnekleri tüm örnek parametreleri kendi adları, konum ve anahtar değerlerini değiştirin. Aşağıdaki örnekler, bir kuralı kullanmak `myResourceGroup`, `myKeyVault`, `myAADApp`vb.

İlk adım, şifreleme anahtarlarını depolamak için bir Azure anahtar kasası oluşturmaktır. Azure anahtar kasası anahtarları, gizli ya da, uygulama ve hizmetlerinize güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Sanal disk şifreleme için şifreleme veya şifrelerini çözme, sanal diskler için kullanılan bir şifreleme anahtarı depolamak için anahtar kasası kullanın.

Azure aboneliğinizde Azure anahtar kasası sağlayıcısını etkinleştirin, ardından bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur `myResourceGroup` içinde `WestUS` konumu:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Şifreleme anahtarlarını ve depolama ve VM gibi ilişkili işlem kaynakları içeren Azure anahtar kasası aynı bölgede bulunmalıdır. Aşağıdaki örnek, bir Azure anahtar kasası adlı oluşturur `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Yazılım veya donanım güvenlik modeli (HSM) koruması kullanarak şifreleme anahtarlarını depolayabilirsiniz. HSM kullanmanın premium anahtar kasası gerektirir. Premium yazılım korumalı anahtarlar depolar standart anahtar kasası yerine anahtar kasası oluşturmak için ek bir maliyet yoktur. Premium anahtar kasası oluşturmak için önceki adımda ekleyin `--sku Premium` komutu. Aşağıdaki örnek, standart bir anahtar kasası oluşturduğumuz bu yana yazılım korumalı anahtarlar kullanır.

Her iki koruma modeli için Azure platformu VM sanal diskleri şifresini çözmek için önyüklendiğinde şifreleme anahtarları istemek için erişim verilmesi gerekir. Bir şifreleme anahtarı, anahtar kasası içinde oluşturun, sonra sanal disk şifrelemesi ile kullanmak için etkinleştirin. Aşağıdaki örnek adlı bir anahtar oluşturur `myKey` ve disk şifrelemesi için sağlar:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Azure Active Directory uygulaması oluşturma
Sanal diskler şifrelenmiş veya şifresi, kimlik doğrulama ve şifreleme anahtarlarının anahtar Kasası'ndan değişimi işlemek için bir uç nokta kullanın. Bu uç noktası, bir Azure Active Directory uygulaması VM adına uygun şifreleme anahtarları istemek Azure platformu sağlar. Birçok kuruluş Azure Active Directory dizinleri ayrılmış olsa, aboneliğinizde varsayılan Azure Active Directory örneği kullanılabilir.

Tam bir Azure Active Directory Uygulama oluşturmadığınızı gibi `--home-page` ve `--identifier-uris` parametreleri aşağıdaki örnekte gerçek yönlendirilebilir adresi olması gerekmez. Aşağıdaki örnek, ayrıca Azure portal oluşturma anahtarları yerine bir parola tabanlı gizlilik belirtir. Bu sürede anahtarlar oluşturma Azure CLI üzerinden yapılamaz.

Azure Active Directory uygulamanızı oluşturun. Aşağıdaki örnek adlı bir uygulama oluşturur `myAADApp` ve bir parola kullanıyorsa `myPassword`. Kendi parolanızı aşağıdaki gibi belirtin:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Not `applicationId` döndürülen çıktıda önceki komutu. Bu uygulama kimliği kalan adımları bazıları kullanılır. Ardından, uygulama ortamınızda erişilebilir olmasını sağlamak bir hizmet asıl adı (SPN) oluşturun. Başarılı bir şekilde şifrelemek veya sanal diskleri şifresini çözmek için anahtar kasasında depolanan şifreleme anahtarı üzerindeki izinleri anahtarları okumak için Azure Active Directory Uygulama izin verecek şekilde ayarlanması gerekir.

SPN oluşturmak ve uygun izinleri aşağıdaki gibi ayarlayın:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Sanal bir disk ekleyin ve şifreleme durumunu gözden geçirin
Bazı sanal diskler gerçekte şifrelemek için bir disk eklemek için mevcut bir VM'yi olanak sağlar. 5 Gb veri diski için mevcut bir VM'yi aşağıdaki şekilde ekleyin:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Sanal diskleri şu anda şifrelenmez. VM geçerli şifreleme durumunu aşağıdaki gibi gözden geçirin:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Sanal diskler şifrele
Şimdi sanal diskleri şifrelemek için birlikte tüm önceki bileşenleri getirin:

1. Azure Active Directory uygulaması ve parola belirtin.
2. Şifrelenmiş diskleriniz için meta verileri depolamak için anahtar kasası belirtin.
3. Gerçek şifreleme ve şifre çözme için kullanılacak şifreleme anahtarları belirtin.
4. İşletim sistemi diski, veri diskleri veya tüm şifrelemek isteyip istemediğinizi belirtin.

Azure anahtar kasası ve URI, anahtar kasası kimliği gerekir ve ardından son adım URL anahtar olarak, oluşturulan anahtarın ayrıntıları incelemenizi sağlar:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Çıktısını kullanarak, sanal disklere şifrelemek `azure keyvault show` ve `azure keyvault key show` gibi komutlar:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Yukarıdaki komut birçok değişken taşıdığından, tam komut başvurusu için aşağıdaki örnek verilmiştir:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Azure CLI ayrıntılı hataları şifreleme işlemi sırasında sağlamaz. Ek sorun giderme bilgileri için inceleyin `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` şifreleme VM üzerinde.

Son olarak, sanal diskler artık şifrelenmiş olduğunu onaylamak için yeniden şifreleme durumunu gözden olanak sağlar:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Ek veri disklerinin ekleme
Veri disklerinizi şifrelenmiş sonra daha sonra ek sanal diskleri VM'nize eklemek ve de şifreleme. Çalıştırdığınızda `azure vm enable-disk-encryption` komutu, dizisi sürümü kullanılarak Artır `--sequence-version` parametresi. Bu sıra sürüm parametresi aynı VM üzerinde yinelenen işlemler gerçekleştirmenizi sağlar.

Örneğin, ikinci bir sanal disk VM'nize aşağıdaki şekilde ekleyin olanak sağlar:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Sanal diskler şifrelemek için komutu yeniden çalıştırın bu zaman ekleme `--sequence-version` parametresi ve değeri bizim ilk çalıştırma gibi artırma:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Sonraki adımlar
* Azure anahtar kasası, şifreleme anahtarlarını ve kasa, silme de dahil olmak üzere yönetme hakkında daha fazla bilgi için bkz: [anahtar kasası CLI kullanarak yönetme](../../key-vault/key-vault-manage-with-cli2.md).
* Azure için karşıya yüklemek için şifrelenmiş bir özel VM hazırlama gibi disk şifrelemesi hakkında daha fazla bilgi için bkz: [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption.md).
