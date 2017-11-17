---
title: "Azure anahtar kasası CLI kullanarak yönetme | Microsoft Docs"
description: "CLI kullanarak anahtar kasası ortak görevleri otomatikleştirmek için bu öğreticiyi kullanın"
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: barclayn
ms.openlocfilehash: 94ea95e7f40c8d47dd18422a9c0795655dae365b
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="manage-key-vault-using-cli"></a>Anahtar kasası CLI kullanarak yönetme

Bu öğreticiyi, Azure'da sağlamlaştırılmış bir kapsayıcı (bir kasa) oluşturmak için Azure anahtar kasası ile çalışmaya başlamanıza yardımcı olması için kullanın. Azure anahtar kasası ve şifreleme anahtarları ve gizli anahtarları depolayıp yönetmek için kullanılır. Bu makalede bir kasa oluşturmak için Azure platformlar arası komut satırı arabirimi kullanarak sürecinde yardımcı olur. Ardından, çeşitli ortak işlemleri gerçekleştirmek için kasa ile etkileşime gireceğini. 

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

**Tahmini tamamlanma süresi:** 20 dakika

> [!NOTE]
> Bu öğreticide adımları birini içeren Azure uygulaması yazma yönergeler içermez. Bu örnek uygulama, nasıl bir uygulama bir anahtar veya gizli anahtar kasasında depolanan kullanma yetkisi verilebilir göstermek için kullanılır.
> Bu makalede, Azure anahtar kasası platformlar arası komut satırı arabirimini kullanarak yapılandırma üzerinde durulmaktadır. Azure PowerShell yönergeleri için bkz: [bu eşdeğer öğreticiye](key-vault-get-started.md).

Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Bir Microsoft Azure aboneliği. Bir yoksa için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial).
* Komut satırı arabirimi sürümünü 0.9.1 veya sonraki bir sürümü. En son sürümünü yükleyin ve Azure aboneliğinize bağlanmak için bkz: [Azure platformlar arası komut satırı arabirimi yükleyip](../cli-install-nodejs.md).
* Bu öğreticide oluşturduğunuz anahtarı veya parolayı kullanmak üzere yapılandırılacak bir uygulama. [Microsoft Yükleme Merkezi](http://www.microsoft.com/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Yönergeler için beraberinde gelen Benioku dosyasına bakın.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure platformlar arası komut satırı arabirimi ile ilgili Yardım alma

Bu öğretici, komut satırı arabirimi (Bash, Terminal, komut istemi) bildiğinizi varsayar

Yardım veya -h parametresi, belirli komutlar için Yardım görüntülemek için kullanılabilir. Alternatif olarak, azure help [komut] [Seçenekler] biçimi, aynı bilgileri döndürmek için de kullanılabilir. Örneğin, aşağıdaki komutları tüm aynı bilgileri döndürün:

    azure account set --help

    azure account set -h

    azure help account set

Kullanarak--yardımcı olmak için şüpheli bir komut tarafından gerekli parametreleri hakkında başvuru, -h veya azure yardıma [komut].

Ayrıca, Azure platformlar arası komut satırı arabirimi ile Azure Resource Manager dağıtımı modeli hakkında bilgi edinmek için aşağıdaki öğreticileri okuyabilirsiniz:

* [Yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi](../cli-install-nodejs.md)
* [Azure Resource Manager ile Azure platformlar arası komut satırı arabirimi kullanma](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Aboneliklerinize bağlanma

Aşağıdaki komutu kullanarak Azure'da oturum açmak için:

```azurecli-interactive
azure login -u username -p password
```

veya etkileşimli olarak yazarak oturum açmak istiyorsanız

```azurecli-interactive
azure login
```

Birden çok aboneliğiniz varsa ve Azure Anahtar Kasası için kullanmak üzere belirli bir tanesini belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:

```azurecli-interactive
azure account list
```

Ardından, kullanılacak aboneliği belirtmek için şunu yazın:

```azurecli-interactive
azure account set <subscription name>
```

Azure platformlar arası komut satırı arabirimi yapılandırma hakkında daha fazla bilgi için bkz: [yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Azure Resource Manager kullanarak geçiş
Azure Resource Manager anahtar kasası gerektirir, bu nedenle Azure Resource Manager moduna geçmek için aşağıdaki komutu yazın:

```azurecli-interactive
azure config mode arm
```

## <a name="create-a-new-resource-group"></a>Yeni bir kaynak grubu oluşturma
Azure Kaynak Yöneticisi'ni kullanırken, tüm ilgili kaynaklar bir kaynak grubu içinde oluşturulur. Bu öğretici için yeni bir kaynak grubu 'ContosoResourceGroup' oluşturacağız.

```azurecli-interactive
azure group create 'ContosoResourceGroup' 'East Asia'
```

İlk parametre kaynak grubu adı ve ikinci parametre konumu. Konum için komutunu `azure location list` Bu örnekte bir alternatif bir konum belirtmek nasıl belirlemek üzere. Daha fazla bilgiye ihtiyacınız varsa, yazın:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Anahtar kasası kaynak sağlayıcısını Kaydet
Bu anahtar kasası kaynak sağlayıcısı aboneliğinizde kayıtlı olduğundan emin olun:

```azurecli-interactive
azure provider register Microsoft.KeyVault
```

Bu yalnızca bir kez abonelik başına yapılmalıdır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kullanım `azure keyvault create` bir anahtar kasası oluşturmak için komutu. Bu komut dosyasını üç zorunlu parametreye sahiptir: bir kaynak grubu adı, bir anahtar kasası adı ve coğrafi konum.

Örneğin:
- Kasa adını kullanırsanız, **ContosoKeyVault**
- Kaynak grubu adı **ContosoResourceGroup**
- Konumunu **Doğu Asya** türü:

```azurecli-interactive
azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Bu komutun çıktısı, yeni oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **Ad**: örnekte ContosoKeyVault budur. Bu adı diğer Anahtar Kasası cmdlet'leri için kullanacaksınız.
* **vaultUri**: örnekte https://contosokeyvault.vault.azure.net budur. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Henüz başka kimse yetkilendirilmedi.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Anahtar kasasına bir anahtar veya gizli anahtar ekleme

Azure anahtar Kasası'nın yazılım korumalı bir anahtar oluşturmasını istiyorsanız, kullanmak `azure key create` komutunu ve aşağıdaki komutu yazın:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software
````

Ancak, yerel dosya Azure anahtar Kasası'na karşıya yüklemek istediğiniz softkey.pem adlı bir dosya olarak kaydedilmiş bir .pem dosyasını mevcut bir anahtarı varsa, anahtarı içeri aktarmak için aşağıdaki komutu yazın. Anahtar kasası hizmetinde yazılım ile anahtarı koruyan PEM dosyası:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software
```

Artık oluşturduğunuz veya Azure anahtar Kasası'na URI'sini kullanarak karşıya anahtar başvurabilirsiniz. Kullanmak **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** her zaman geçerli sürümü almak ve kullanmak için **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** bu belirli sürümü almak için.

Gizli SQLPassword adlı bir parola olduğu ve değer, Pa$ $w0rd Azure anahtar kasası için olan, kasaya eklemek için aşağıdaki komutu yazın:

```azurecli-interactive
azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'
```

Artık Azure Anahtar Kasası'na eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Her zaman geçerli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword** kullanın ve bu belirli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** kullanın.

Şimdi yeni anahtar veya gizli görünümü oluşturan:

* Anahtarınızı görüntülemek için şunu yazın: `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Gizli anahtarı görüntülemek için şunu yazın: `azure keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'ye kaydetme

Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure anahtar Kasası'na özgü değildir ancak bütünlük açısından buraya eklenmiştir.

> [!IMPORTANT]
> Öğretici, hesabınızı, kasaya ve bu adımda kaydedeceğiniz uygulama tamamlamak için tüm aynı Azure Active Directory'de olmalıdır.

Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir. Bunu yapmak için uygulama sahibinin öncelikle Azure Active Directory'sinde uygulamayı kaydetmesi gerekir. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

- Bir **uygulama kimliği** 
- Bir **kimlik doğrulama anahtarı** (paylaşılan gizlilik olarak da bilinir). 

Uygulamanın bir belirteç almak için bu değerlerin her ikisini de Azure Active Directory'ye sunması gerekir. Uygulamanın bunu yapmak için nasıl yapılandırılacağı uygulamaya bağlıdır. İçin [anahtar kasası örnek uygulaması](https://www.microsoft.com/download/details.aspx?id=45343), uygulama sahibi bu değerleri app.config dosyasında ayarlar.

Uygulama Azure Active Directory ile kaydetme hakkında ayrıntılı adımlar için başlıklı makaleyi gözden geçirmeniz gereken [uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/active-directory-integrating-applications.md) veya [kullanım portalı bir Azure Active oluşturmak için Dizin uygulama ve kaynaklarına erişebilir hizmet sorumlusu](../azure-resource-manager/resource-group-create-service-principal-portal.md) uygulama Azure Active Directory'ye kaydetmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol bölmede, tıklatın **uygulama kayıtlar**. Uygulama kayıtlar görmüyorsanız, tıklayın **daha fazla hizmet** ve burada bulabilirsiniz.  
    >[!NOTE]
    Anahtar kasanızı oluşturduğunuz Azure aboneliğini içeren aynı dizini seçmeniz gerekir. 
3. Tıklatın **yeni uygulama kaydı**.
4. Üzerinde **oluşturma** dikey penceresinde uygulamanız için bir ad sağlayın ve ardından **WEB uygulaması ve/veya WEB API** (varsayılan) ve belirtin **oturum açma URL** web için uygulama. Şu anda bu bilgiler yoksa, bu adım için yapabilirsiniz (örneğin, http://test1.contoso.com belirtebilirsiniz). Bu sitelerin mevcut olup olmaması önemli değildir. 

   ![Yeni uygulama kaydı](./media/key-vault-manage-with-cli/new-application-registration.png)

5. **Oluştur** düğmesine tıklayın.
6. Uygulama Kayıt tamamlandığında kayıtlı uygulamaların listesini görebilirsiniz. Yalnızca kayıtlı ve tıklayın uygulamayı bulun.
7. Tıklayın **kayıtlı uygulama** dikey kopyalama **uygulama kimliği**
8. Tıklayın **tüm ayarlar**
9. Üzerinde **ayarları** dikey penceresinde **anahtarları**
10. Açıklama türü **anahtar açıklama** kutu ve bir süre seçin ve ardından **KAYDETMEK**. Sayfa yenilenir ve ardından bir anahtar değeri gösterir. 
11. Kullanacağınız **uygulama kimliği** ve **anahtar** kasanızda izinleri ayarlamak için bir sonraki adımda bilgi.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme
Anahtar veya gizli kasadaki erişmek için uygulama yetkilendirmek için kullanın:

```azurecli-interactive
azure keyvault set-policy
```

Örneğin, kasa adınız ContosoKeyVault ve yetkilendirmek istediğiniz uygulama 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed istemci Kimliğine sahip kasanızı anahtarlarında oturum açın ve şifresini çözmek için uygulama yetkilendirmek istediğiniz ise, ardından şu komutu çalıştırın:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'
```

> [!NOTE]
> Windows komut istemi üzerinde çalıştırıyorsanız, tek tırnak işaretleri çift tırnak işareti ile değiştirin ve ayrıca iç çift tırnak işareti kaçış gerekir. Örneğin: "[\"şifresini\",\"oturum\"]".
> 

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız şunu çalıştırın:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'
```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Bir donanım güvenlik modülü (HSM) kullanmak istiyorsanız
Ek güvence için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete-the-key-vault-and-associated-keys-and-secrets)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için HSM korumalı anahtarları destekleyen bir kasa aboneliğinizin olması gerekir.

Keyvault oluşturduğunuzda 'sku' parametresini ekleyin:

```azurecli-interactive
azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

Bu kasaya yazılım korumalı anahtarlar ve HSM korumalı anahtarlar (daha önce gösterildiği gibi) ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için hedef parametresini 'HSM' ayarlayın:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'
```

.Pem dosyasını bilgisayarınızdaki bir anahtar almak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'
```

Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Böylece anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanız ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanız sağlanır.

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'
```

Daha ayrıntılı bu BYOK paketini oluşturma hakkında yönergeler için bkz: [HSM-Protected anahtarları Azure anahtar kasası ile kullanmak nasıl](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme
Anahtar kasası ve anahtar veya içerdiği gizli artık ihtiyacınız varsa, anahtar kasası azure keyvault silme komutunu kullanarak silebilirsiniz:

```azurecli-interactive
azure keyvault delete --vault-name 'ContosoKeyVault'
```

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

```azurecli-interactive
azure group delete --name 'ContosoResourceGroup'
```


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Diğer Azure platformlar arası komut satırı arabirimi komutları
Diğer komutlar Azure anahtar kasası yönetmek için kullanışlı olabilir.

Bu komut tüm anahtarların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

```azurecli-interactive
azure keyvault key list --vault-name 'ContosoKeyVault'
```

Bu komut belirtilen anahtar için özelliklerin tam listesini görüntüler:

```azurecli-interactive
azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Bu komut tüm gizli adların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

```azurecli-interactive
azure keyvault secret list --vault-name 'ContosoKeyVault'
```

Belirli bir anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

```azurecli-interactive
azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Belirli bir gizli anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

```azurecli-interactive
azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'
```


## <a name="next-steps"></a>Sonraki adımlar
- Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).
- Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)
- Powershell kullanarak Azure anahtar kasası ile çalışmaya nasıl [Azure anahtar kasası Başlarken](key-vault-get-started.md).


