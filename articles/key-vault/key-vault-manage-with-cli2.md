---
title: "Azure anahtar kasası CLI kullanarak yönetme | Microsoft Docs"
description: "CLI 2.0 kullanarak anahtar kasası ortak görevleri otomatikleştirmek için bu öğreticiyi kullanın"
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: barclayn
ms.openlocfilehash: eaeb50ca8a83fcfee6689acf549f20ba5d44c51d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Anahtar kasası CLI 2.0 kullanarak yönetme

Bu makalede, Azure CLI 2.0 kullanan Azure anahtar kasası ile çalışmaya başlamak alınmaktadır. Hakkında bilgi bulabilirsiniz:
- Azure'da sağlamlaştırılmış bir kapsayıcı (bir kasa) oluşturma
- Depolamak ve şifreleme anahtarları ve gizli anahtarları azure'da yönetmek nasıl. 
- Bir anahtar ya da sonra bir Azure uygulamasıyla kullanabileceğiniz parola içeren bir kasa oluşturmak için Azure platformlar arası komut satırı arabirimini kullanarak işlem. 
- Nasıl bir uygulama, daha sonra bu anahtarı veya parolayı kullanabilirsiniz.

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

**Tahmini tamamlanma süresi:** 20 dakika

> [!NOTE]
> Bu öğretici, adımlardan biri, bir anahtar veya gizli anahtarı kasaya kullanmak için bir uygulamanın nasıl gösterir içerdiğini Azure uygulaması yazma yönergeler içermez.
>

Azure anahtar kasası genel bakış için bkz: [Azure anahtar kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Bir Microsoft Azure aboneliği. Bir yoksa için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial).
* Komut satırı arabirimi sürüm 2.0 veya üstü. En son sürümünü yükleyin ve Azure aboneliğinize bağlanmak için bkz: [Azure platformlar arası komut satırı arabirimi 2.0 yükleyip](/cli/azure/install-azure-cli).
* Bu öğreticide oluşturduğunuz anahtarı veya parolayı kullanmak üzere yapılandırılacak bir uygulama. [Microsoft Yükleme Merkezi](http://www.microsoft.com/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Yönergeler için beraberinde gelen Benioku dosyasına bakın.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure platformlar arası komut satırı arabirimi ile ilgili Yardım alma
Bu öğretici, komut satırı arabirimi (Bash, Terminal, komut istemi) bildiğinizi varsayar

Yardım veya -h parametresi, belirli komutlar için Yardım görüntülemek için kullanılabilir. Alternatif olarak, azure help [komut] [Seçenekler] biçimi, aynı bilgileri döndürmek için de kullanılabilir. Örneğin, aşağıdaki komutları tüm aynı bilgileri döndürün:

```azurecli-interactive
az account set --help
az account set -h
```

Kullanarak--yardımcı olmak için şüpheli bir komut tarafından gerekli parametreleri hakkında başvuru Yardım, -h veya [komut] Yardım az.

Ayrıca, Azure platformlar arası komut satırı arabirimi ile Azure Resource Manager hakkında bilgi edinmek için aşağıdaki öğreticileri okuyabilirsiniz:

* [Azure CLI'yı yükleme](/cli/azure/install-azure-cli)
* [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Aboneliklerinize bağlanma
Bir kurumsal hesap kullanarak oturum açması aşağıdaki komutu kullanın:

```azurecli-interactive
az login -u username@domain.com -p password
```

veya etkileşimli olarak yazarak oturum açmak istiyorsanız

```azurecli-interactive
az login
```

Birden çok aboneliğiniz varsa ve Azure Anahtar Kasası için kullanmak üzere belirli bir tanesini belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:

```azurecli-interactive
az account list
```

Ardından, kullanılacak aboneliği belirtmek için şunu yazın:

```azurecli-interactive
az account set --subscription <subscription name or ID>
```

Azure platformlar arası komut satırı arabirimi yapılandırma hakkında daha fazla bilgi için bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Yeni bir kaynak grubu oluşturma
Azure Kaynak Yöneticisi'ni kullanırken, tüm ilgili kaynaklar bir kaynak grubu içinde oluşturulur. Bu öğretici için yeni bir kaynak grubu 'ContosoResourceGroup' oluşturacağız.

```azurecli-interactive
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

İlk parametre kaynak grubu adı ve ikinci parametre konumu. Tüm olası bir listesini almak için konumları yazın:

```azurecli-interactive
az account list-locations
``` 

Daha fazla bilgiye ihtiyacınız varsa, yazın: 

```azurecli-interactive
az account list-locations -h
```

## <a name="register-the-key-vault-resource-provider"></a>Anahtar kasası kaynak sağlayıcısını Kaydet
Yeni bir anahtar kasası oluşturmayı denediğinizde "abonelik 'Microsoft.KeyVault' ad alanını kullanmak için kayıtlı değil" hata görebilirsiniz. Bu ileti görüntülenirse anahtar kasası kaynak sağlayıcısı aboneliğinizde kayıtlı olduğundan emin olun:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
```

>[!NOTE]
Bu yalnızca bir kez abonelik başına yapılmalıdır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Kullanım `az keyvault create` bir anahtar kasası oluşturmak için komutu. Bu komut dosyasını üç zorunlu parametreye sahiptir: bir kaynak grubu adı, bir anahtar kasası adı ve coğrafi konum.

Örneğin:

- Kasa adını kullanırsanız, **ContosoKeyVault**
- Kaynak grubu adı **ContosoResourceGroup**
- Konumunu **Doğu Asya**

şunu yazın:

```azurecli-interactive
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Bu komutun çıktısı, yeni oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **ad**: örnekte ContosoKeyVault budur. Bu adı diğer anahtar kasası komutları için kullanır.
* **vaultUri**: örnekte https://contosokeyvault.vault.azure.net budur. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Henüz başka kimse yetkilendirilmedi.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Anahtar kasasına bir anahtar veya gizli anahtar ekleme

Azure anahtar Kasası'nın yazılım korumalı bir anahtar oluşturmasını istiyorsanız, kullanmak `az key create` komutunu ve aşağıdaki komutu yazın:
```azurecli-interactive
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Ancak, yerel dosya Azure anahtar Kasası'na karşıya yüklemek istediğiniz softkey.pem adlı bir dosya olarak kaydedilmiş bir .pem dosyasını mevcut bir anahtarı varsa, anahtarı içeri aktarmak için aşağıdaki komutu yazın. Anahtar kasası hizmetinde yazılım ile anahtarı koruyan PEM dosyası:

```azurecli-interactive
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```

Artık oluşturduğunuz veya Azure anahtar Kasası'na URI'sini kullanarak karşıya anahtar başvurabilirsiniz. Kullanmak **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** her zaman geçerli sürümü almak ve kullanmak için **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** bu belirli sürümü almak için.

Gizli SQLPassword adlı bir parola olduğu ve değer, Pa$ $w0rd Azure anahtar kasası için olan, kasaya eklemek için aşağıdaki komutu yazın:

```azurecli-interactive
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```

Artık Azure Anahtar Kasası'na eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Her zaman geçerli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword** kullanın ve bu belirli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** kullanın.

Şimdi yeni anahtar veya gizli görünümü oluşturan:

* Anahtarınızı görüntülemek için şunu yazın: 

```azurecli-interactive
az keyvault key list --vault-name 'ContosoKeyVault'
```

* Gizli anahtarı görüntülemek için şunu yazın: 

```azurecli-interactive
az keyvault secret list --vault-name 'ContosoKeyVault'
```

## <a name="register-an-application-with-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'ye kaydetme
Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure anahtar Kasası'na özgü değildir ancak burada için tanıma bulunur.

> [!IMPORTANT]
> Öğreticiyi tamamlamak için bu adımda kaydedeceğiniz hesabınızın, kasanızın ve uygulamanızın hepsinin aynı Azure dizininde olması gerekir.
>
>

Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir. Bunu yapmak için uygulama sahibinin öncelikle Azure Active Directory'sinde uygulamayı kaydetmesi gerekir. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

- **Uygulama Kimliği** 
- **Kimlik doğrulaması anahtarı** (paylaşılan gizli dizi olarak da bilinir). 

Uygulamanın bir belirteç almak için bu değerlerin her ikisini de Azure Active Directory'ye sunması gerekir. Uygulamanın bunu yapmak için nasıl yapılandırılacağı uygulamaya bağlıdır. [Key Vault örnek uygulaması](https://www.microsoft.com/download/details.aspx?id=45343) için uygulama sahibi bu değerleri app.config dosyasında ayarlar.

Uygulama Azure Active Directory ile kaydetme hakkında ayrıntılı adımlar için başlıklı makaleyi gözden geçirmeniz gereken [uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/active-directory-integrating-applications.md) veya [kullanım portalı bir Azure Active oluşturmak için Dizin uygulama ve kaynaklarına erişebilir hizmet sorumlusu](../azure-resource-manager/resource-group-create-service-principal-portal.md) uygulama Azure Active Directory'ye kaydetmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki **Uygulama kayıtları**’na tıklayın. Uygulama kayıtlarını görmezseniz **diğer hizmetler**’e tıklayıp burada bulabilirsiniz.  
[!NOTE]
Anahtar kasanızı oluşturduğunuz Azure aboneliğini içeren dizini seçmeniz gerekir. 
3. **Yeni uygulama kaydı**’na tıklayın.
4. **Oluştur** dikey penceresinde uygulamanız için bir ad belirtin ve sonra **WEB UYGULAMASI VE/VEYA WEB API'Sİ** (varsayılan) seçeneğini belirleyip web uygulamanız için **OTURUM AÇMA URL’Sİ** değerini belirtin. Bu bilgilere şu anda sahip değilseniz, bu adım için rastgele değerler kullanabilirsiniz (örneğin, http://test1.contoso.com adresini belirtebilirsiniz). Bu sitelerin mevcut olup olmaması önemli değildir. 

    ![Yeni uygulama kaydı](./media/key-vault-manage-with-cli2/new-application-registration.png)
    >[!WARNING]
    **WEB UYGULAMASI VE/VEYA WEB API’Sİ** seçeneğini belirlediğinizden emin olun; aksi takdirde ayarlar altında **anahtarlar** seçeneğini görmezsiniz.

5. **Oluştur** düğmesine tıklayın.
6. Uygulama kaydı tamamlandığında kayıtlı uygulamaların listesini görebilirsiniz. Yeni kaydettiğiniz uygulamayı bulup tıklayın.
7. **Kayıtlı uygulama** dikey penceresine tıklayıp **Uygulama Kimliği**’ni kopyalayın
8. **Tüm ayarlar**’a tıklayın
9. **Ayarlar** dikey penceresinde **anahtarlar**’a tıklayın
9. **Anahtar açıklaması** kutusuna bir açıklama girip süre seçin ve sonra **KAYDET**’e tıklayın. Sayfa yenilenir ve ardından bir anahtar değeri gösterir. 
10. Kasanızdaki izinleri ayarlamak için sonraki adımda **Uygulama Kimliği** ve **Anahtar** bilgilerini kullanacaksınız.


## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme
Anahtar veya gizli kasadaki erişmek için uygulama yetkilendirmek için `az keyvault set-policy` komutu.

Örneğin, kasa adınız ContosoKeyVault ve yetkilendirmek istediğiniz uygulama 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed istemci Kimliğine sahip kasanızı anahtarlarında oturum açın ve şifresini çözmek için uygulama yetkilendirmek istediğiniz ise, ardından şu komutu çalıştırın:

```azurecli-interactive
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız şunu çalıştırın:

```azurecli-interactive
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Bir donanım güvenlik modülü (HSM) kullanmak istiyorsanız
Ek güvence için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete-the-key-vault-and-associated-keys-and-secrets)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için HSM korumalı anahtarları destekleyen bir kasa aboneliğinizin olması gerekir.

Keyvault oluşturduğunuzda 'sku' parametresini ekleyin:

```azurecli-interactive
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
Bu kasaya yazılım korumalı anahtarlar ve HSM korumalı anahtarlar (daha önce gösterildiği gibi) ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için hedef parametresini 'HSM' ayarlayın:

```azurecli-interactive
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

.Pem dosyasını bilgisayarınızdaki bir anahtar almak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

```azurecli-interactive
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```

Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Böylece anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanız ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanız sağlanır.

```azurecli-interactive
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Daha ayrıntılı bu BYOK paketini oluşturma hakkında yönergeler için bkz: [HSM-Protected anahtarları Azure anahtar kasası ile kullanmak nasıl](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme
Anahtar kasası ve anahtar veya içerdiği gizli artık ihtiyacınız varsa, anahtar kasası kullanarak silebilirsiniz `az keyvault delete` komutu:

```azurecli-interactive
az keyvault delete --name 'ContosoKeyVault'
```

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

```azurecli-interactive
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Diğer Azure platformlar arası komut satırı arabirimi komutları
Diğer komutlar Azure anahtar kasası yönetmek için kullanışlı olabilir.

Bu komut tüm anahtarların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

```azurecli-interactive
az keyvault key list --vault-name 'ContosoKeyVault'
```

Bu komut belirtilen anahtar için özelliklerin tam listesini görüntüler:

```azurecli-interactive
az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Bu komut tüm gizli adların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

```azurecli-interactive
az keyvault secret list --vault-name 'ContosoKeyVault'
```

Belirli bir anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

```azurecli-interactive
az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Belirli bir gizli anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

```azurecli-interactive
az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'
```

## <a name="next-steps"></a>Sonraki adımlar

- Anahtar kasası komutları için tam Azure CLI başvuru için bkz: [anahtar kasası CLI başvuru](/cli/azure/keyvault).

- Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

- Azure anahtar kasası ve HSM'ler hakkında daha fazla bilgi için bkz: [HSM-Protected anahtarları Azure anahtar kasası ile kullanmak nasıl](key-vault-hsm-protected-keys.md).
