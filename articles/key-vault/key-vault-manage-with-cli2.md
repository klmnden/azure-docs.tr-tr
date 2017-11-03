---
title: "Azure anahtar kasası CLI kullanarak yönetme | Microsoft Docs"
description: "CLI 2.0 kullanarak anahtar kasası ortak görevleri otomatikleştirmek için bu öğreticiyi kullanın"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 5da9f5eceda71ac85259193e0f183c72813e1679
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Anahtar kasası CLI 2.0 kullanarak yönetme
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Giriş
Bu öğreticiyi, Azure'da sağlamlaştırılmış bir kapsayıcı (bir kasa) oluşturmak ve Azure'da şifreleme anahtarlarını ve gizli anahtarları depolayıp yönetmek amacıyla Azure Anahtar Kasası ile çalışmaya başlamaya yardımcı olması için kullanın. Bu, bir anahtar ya da sonra bir Azure uygulamasıyla kullanabileceğiniz parola içeren bir kasa oluşturmak için Azure platformlar arası komut satırı arabirimi kullanarak işlemi açıklanmaktadır. Bunu daha sonra nasıl bir uygulama daha sonra bu anahtarı veya parolayı kullanabilirsiniz gösterir.

**Tahmini tamamlanma süresi:** 20 dakika

> [!NOTE]
> Bu öğretici, adımlardan biri, bir anahtar veya gizli anahtarı kasaya kullanmak için bir uygulamanın nasıl gösterir içerdiğini Azure uygulaması yazma yönergeler içermez.
>
> Bu öğretici, en son Azure CLI 2.0 kullanır.
>
>

Azure Anahtar Kasası genel bakış bilgileri için bkz. [Azure Anahtar Kasası nedir?](key-vault-whatis.md)

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Bir Microsoft Azure aboneliği. Bir yoksa için kaydolabilirsiniz bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial).
* Komut satırı arabirimi sürüm 2.0 veya üstü. En son sürümünü yükleyin ve Azure aboneliğinize bağlanmak için bkz: [Azure platformlar arası komut satırı arabirimi 2.0 yükleyip](/cli/azure/install-azure-cli).
* Bu öğreticide oluşturduğunuz anahtarı veya parolayı kullanmak üzere yapılandırılacak bir uygulama. [Microsoft Yükleme Merkezi](http://www.microsoft.com/download/details.aspx?id=45343)'nde örnek bir uygulama kullanılabilir. Yönergeler için beraberinde gelen Benioku dosyasına bakın.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Azure platformlar arası komut satırı arabirimi ile ilgili Yardım alma
Bu öğretici, komut satırı arabirimi (Bash, Terminal, komut istemi) bildiğinizi varsayar

Yardım veya -h parametresi, belirli komutlar için Yardım görüntülemek için kullanılabilir. Alternatif olarak, azure help [komut] [Seçenekler] biçimi, aynı bilgileri döndürmek için de kullanılabilir. Örneğin, aşağıdaki komutları tüm aynı bilgileri döndürün:

```
az account set --help
az account set -h
```

Kullanarak--yardımcı olmak için şüpheli bir komut tarafından gerekli parametreleri hakkında başvuru Yardım, -h veya [komut] Yardım az.

Ayrıca, Azure platformlar arası komut satırı arabirimi ile Azure Resource Manager hakkında bilgi edinmek için aşağıdaki öğreticileri okuyabilirsiniz:

* [Azure CLI'yı yükleme](/cli/azure/install-azure-cli)
* [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Aboneliklerinize bağlanma
Bir kurumsal hesap kullanarak oturum açması aşağıdaki komutu kullanın:

```
az login -u username@domain.com -p password
```

veya etkileşimli olarak yazarak oturum açmak istiyorsanız

```
az login
```

Birden çok aboneliğiniz varsa ve Azure Anahtar Kasası için kullanmak üzere belirli bir tanesini belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:

```
az account list
```

Ardından, kullanılacak aboneliği belirtmek için şunu yazın:

```
az account set --subscription <subscription name or ID>
```

Azure platformlar arası komut satırı arabirimi yapılandırma hakkında daha fazla bilgi için bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Yeni bir kaynak grubu oluşturma
Azure Kaynak Yöneticisi'ni kullanırken, tüm ilgili kaynaklar bir kaynak grubu içinde oluşturulur. Bu öğretici için yeni bir kaynak grubu 'ContosoResourceGroup' oluşturacağız.

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

İlk parametre kaynak grubu adı ve ikinci parametre konumu. Konum için komutunu `az account list-locations` Bu örnekte bir alternatif bir konum belirtmek nasıl belirlemek üzere. Daha fazla bilgiye ihtiyacınız varsa, yazın: `az account list-locations -h`.

## <a name="register-the-key-vault-resource-provider"></a>Anahtar kasası kaynak sağlayıcısını Kaydet
Bu anahtar kasası kaynak sağlayıcısı aboneliğinizde kayıtlı olduğundan emin olun:

```
az provider register -n Microsoft.KeyVault
```

Bu yalnızca bir kez abonelik başına yapılmalıdır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Kullanım `az keyvault create` bir anahtar kasası oluşturmak için komutu. Bu komut dosyasını üç zorunlu parametreye sahiptir: bir kaynak grubu adı, bir anahtar kasası adı ve coğrafi konum.

Örneğin, ContosoKeyVault kasa adını kullanırsanız, ContosoResourceGroup kaynak grubu adını ve Doğu Asya konumunu yazın:
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Bu komutun çıktısı, yeni oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **ad**: örnekte ContosoKeyVault budur. Bu adı diğer anahtar kasası komutları için kullanır.
* **vaultUri**: örnekte https://contosokeyvault.vault.azure.net budur. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Henüz başka kimse yetkilendirilmedi.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Anahtar kasasına bir anahtar veya gizli anahtar ekleme
Azure anahtar Kasası'nın yazılım korumalı bir anahtar oluşturmasını istiyorsanız, kullanmak `az key create` komutunu ve aşağıdaki komutu yazın:
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Ancak, yerel dosya Azure anahtar Kasası'na karşıya yüklemek istediğiniz softkey.pem adlı bir dosya olarak kaydedilmiş bir .pem dosyasını mevcut bir anahtarı varsa, anahtarı içeri aktarmak için aşağıdaki komutu yazın. Anahtar kasası hizmetinde yazılım ile anahtarı koruyan PEM dosyası:
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
Artık oluşturduğunuz veya Azure anahtar Kasası'na URI'sini kullanarak karşıya anahtar başvurabilirsiniz. Kullanmak **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** her zaman geçerli sürümü almak ve kullanmak için **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** bu belirli sürümü almak için.

Gizli SQLPassword adlı bir parola olduğu ve değer, Pa$ $w0rd Azure anahtar kasası için olan, kasaya eklemek için aşağıdaki komutu yazın:
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
Artık Azure Anahtar Kasası'na eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Her zaman geçerli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword** kullanın ve bu belirli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** kullanın.

Şimdi yeni anahtar veya gizli görünümü oluşturan:

* Anahtarınızı görüntülemek için şunu yazın: `az keyvault key list --vault-name 'ContosoKeyVault'`
* Gizli anahtarı görüntülemek için şunu yazın: `az keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'ye kaydetme
Bu adım genellikle ayrı bir bilgisayarda bir geliştirici tarafından yapılır. Azure anahtar Kasası'na özgü değildir ancak bütünlük açısından buraya eklenmiştir.

> [!IMPORTANT]
> Öğreticiyi tamamlamak için bu adımda kaydedeceğiniz hesabınızın, kasanızın ve uygulamanızın hepsinin aynı Azure dizininde olması gerekir.
>
>

Bir anahtar kasası kullanan uygulamaların, Azure Active Directory'den bir belirteç kullanarak kimlik doğrulama yapması gerekir. Bunu yapmak için uygulama sahibinin öncelikle Azure Active Directory'sinde uygulamayı kaydetmesi gerekir. Kaydın sonunda, uygulama sahibi aşağıdaki değerleri elde eder:

* Bir **Uygulama Kimliği** (İstemci Kimliği olarak da bilinir) ve **kimlik doğrulama anahtarı** (paylaşılan gizli anahtar olarak da bilinir). Uygulama bu değerleri her ikisi de Azure bir belirteç almak için Active Directory ile sunması gerekir. Uygulamanın bunu yapmak için nasıl yapılandırılacağı uygulamaya bağlıdır. Anahtar Kasası örnek uygulaması için uygulama sahibi bu değerleri app.config dosyasında ayarlar.

Bir uygulamayı Azure Active Directory'ye kaydetmek için:

1. Azure Portal’da oturum açın.
2. Sol bölmede, tıklatın **Azure Active Directory**ve ardından içinde kaydettiğiniz uygulamanızı dizini seçin. <br> <br> 

> [!Note] 
> Anahtar kasanızı oluşturduğunuz Azure aboneliğini içeren aynı dizini seçmeniz gerekir. Bu dizinin hangisi olduğunu bilmiyorsanız **Ayarlar**'a tıklayın, anahtar kasanızı oluşturduğunuz aboneliği tanımlayın ve son sütunda görüntülenen dizinin adını not edin.

3. **UYGULAMALAR**'a tıklayın. Dizininize hiçbir uygulama eklenmemişse bu sayfa yalnızca Göster **bir uygulama ekleyin** bağlantı. Bağlantıya tıklayın veya alternatif olarak, tıklatabilirsiniz **ekleme** komut çubuğunda.
4. **UYGULAMA EKLEME** sihirbazında **Ne yapmak istiyorsunuz?** sayfasında **Add an application my organization is developing (Kuruluşumun geliştirdiği bir uygulama ekle)** seçeneğine tıklayın.
5. Üzerinde **bize uygulamanızı anlatın** sayfasında uygulamanız için bir ad belirtin ve seçin **WEB uygulaması ve/veya WEB API** (varsayılan). Sonraki simgeyi tıklatın.
6. **Uygulama özellikleri** sayfasında web uygulamanız için **OTURUM AÇMA URL'Sİ** ve **UYGULAMA KİMLİĞİ URI'Sİ** belirtin. Uygulamanız bu değerlere sahip değilse değerleri bu adım için kendiniz belirleyebilirsiniz. (örneğin, her iki kutu için http://test1.contoso.com belirtebilirsiniz). Bu sitelerin var olup olmaması önemli değildir; önemli olan her bir uygulama için uygulama kimliği URI'sinin dizininizdeki tüm uygulamalar için farklı olmasıdır. Dizin uygulamanızı tanımlamak için bu dizeyi kullanır.
7. Sihirbazda yaptığınız değişiklikleri kaydetmek için tam simgesine tıklayın.
8. Hızlı Başlangıç sayfasında, tıklatın **yapılandırma**.
9. **Anahtarlar** bölümüne kaydırın, süre seçin ve ardından **KAYDET**'e tıklayın. Sayfa yenilenir ve ardından bir anahtar değeri gösterir. Bu anahtar değeri ve **İSTEMCİ KİMLİĞİ** değeri ile uygulamanızı yapılandırmanız gerekir. (Bu yapılandırma için yönergeler uygulamaya özgü olur.)
10. Kasanızda izinleri ayarlamak için bir sonraki adımda kullanacağınız istemci kimliği değerini bu sayfadan kopyalayın.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Anahtar veya gizli anahtarı kullanması için uygulamayı yetkilendirme
Anahtar veya gizli kasadaki erişmek için uygulama yetkilendirmek için `az keyvault set-policy` komutu.

Örneğin, kasa adınız ContosoKeyVault ve yetkilendirmek istediğiniz uygulama 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed istemci Kimliğine sahip kasanızı anahtarlarında oturum açın ve şifresini çözmek için uygulama yetkilendirmek istediğiniz ise, ardından şu komutu çalıştırın:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız şunu çalıştırın:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Bir donanım güvenlik modülü (HSM) kullanmak istiyorsanız
Ek güvence için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete-the-key-vault-and-associated-keys-and-secrets)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için HSM korumalı anahtarları destekleyen bir kasa aboneliğinizin olması gerekir.

Keyvault oluşturduğunuzda 'sku' parametresini ekleyin:

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
Bu kasaya yazılım korumalı anahtarlar ve HSM korumalı anahtarlar (daha önce gösterildiği gibi) ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için hedef parametresini 'HSM' ayarlayın:

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

.Pem dosyasını bilgisayarınızdaki bir anahtar almak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Böylece anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanız ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanız sağlanır.

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Daha ayrıntılı bu BYOK paketini oluşturma hakkında yönergeler için bkz: [HSM-Protected anahtarları Azure anahtar kasası ile kullanmak nasıl](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme
Anahtar kasası ve anahtar veya içerdiği gizli artık ihtiyacınız varsa, anahtar kasası kullanarak silebilirsiniz `az keyvault delete` komutu:

```
az keyvault delete --name 'ContosoKeyVault'
```

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Diğer Azure platformlar arası komut satırı arabirimi komutları
Diğer komutlar Azure anahtar kasası yönetmek için kullanışlı olabilir.

Bu komut tüm anahtarların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

az keyvault anahtar listesi--kasa adı 'ContosoKeyVault'

Bu komut belirtilen anahtar için özelliklerin tam listesini görüntüler:

az keyvault anahtar Göster--kasa adı 'ContosoKeyVault'--ad 'ContosoFirstKey'

Bu komut tüm gizli adların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

az keyvault gizli listesi--kasa adı 'ContosoKeyVault'

Belirli bir anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

az keyvault anahtarını silmek--kasa adı 'ContosoKeyVault'--ad 'ContosoFirstKey'

Belirli bir gizli anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

az keyvault gizli anahtarı silme--kasa adı 'ContosoKeyVault'--ad 'SQLPassword'


## <a name="next-steps"></a>Sonraki adımlar
Anahtar kasası komutları için tam Azure CLI başvuru için bkz: [anahtar kasası CLI başvurusu](/cli/azure/keyvault)

Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).
