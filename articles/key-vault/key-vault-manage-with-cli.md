---
title: "Azure anahtar kasası CLI kullanarak yönetme | Microsoft Docs"
description: "CLI kullanarak anahtar kasası ortak görevleri otomatikleştirmek için bu öğreticiyi kullanın"
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: bruceper
ms.openlocfilehash: c2565a742ce4f6ab5f7639a54c4a475f00cbc260
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli"></a>Anahtar kasası CLI kullanarak yönetme

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Giriş

Bu öğreticiyi, Azure'da sağlamlaştırılmış bir kapsayıcı (bir kasa) oluşturmak ve Azure'da şifreleme anahtarlarını ve gizli anahtarları depolayıp yönetmek amacıyla Azure Anahtar Kasası ile çalışmaya başlamaya yardımcı olması için kullanın. Bu, bir anahtar ya da sonra bir Azure uygulamasıyla kullanabileceğiniz parola içeren bir kasa oluşturmak için Azure platformlar arası komut satırı arabirimi kullanarak işlemi açıklanmaktadır. Bunu daha sonra nasıl bir uygulama daha sonra bu anahtarı veya parolayı kullanabilirsiniz gösterir.

**Tahmini tamamlanma süresi:** 20 dakika

> [!NOTE]
> Bu öğretici, adımlardan biri, bir anahtar veya gizli anahtarı kasaya kullanmak için bir uygulamanın nasıl gösterir içerdiğini Azure uygulaması yazma yönergeler içermez.
> 
> Şu anda Azure portalında Azure Anahtar Kasası'nı yapılandıramazsınız. Bunun yerine, bu platformlar arası komut satırı arabirimi yönergeleri kullanın. Veya, Azure PowerShell yönergeleri için bkz: [bu eşdeğer öğreticiye](key-vault-get-started.md).
> 
> 

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

Ayrıca, Azure platformlar arası komut satırı arabirimi ile Azure Resource Manager hakkında bilgi edinmek için aşağıdaki öğreticileri okuyabilirsiniz:

* [Yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi](../cli-install-nodejs.md)
* [Azure Resource Manager ile Azure platformlar arası komut satırı arabirimi kullanma](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Aboneliklerinize bağlanma

Bir kurumsal hesap kullanarak oturum açması aşağıdaki komutu kullanın:

    azure login -u username -p password

veya etkileşimli olarak yazarak oturum açmak istiyorsanız

    azure login

> [!NOTE]
> Oturum açma yöntemi, yalnızca kuruluş hesabı ile çalışır. Bir kurumsal hesap kuruluşunuz tarafından yönetilen ve kuruluşunuzun Azure Active Directory kiracısı'nda tanımlanan bir kullanıcıdır.
> 
> 

Şu anda bir kurumsal hesap yok ve Azure aboneliğinizde oturum açmak için bir Microsoft hesabı kullanarak, aşağıdaki adımları kullanarak kolayca oluşturabilirsiniz.

1. Oturum açma için oturum açma [Azure Yönetim Portalı](https://manage.windowsazure.com/)ve üzerinde Active Directory'ı tıklatın.
2. Hiçbir dizin varsa, dizin oluştur'u seçin ve istenen bilgileri sağlayın.
3. Dizininizi seçin ve yeni bir kullanıcı ekleyin. Bu kullanıcı bir kurumsal hesap ' dir. Kullanıcı oluşturma sırasında kullanıcı ve geçici bir parola için her iki e-posta adresi olan sunulacaktır. Başka bir adımda kullanılan olarak bu bilgileri kaydedin.
4. Portal ayarlarını seçin ve yöneticileri'ni seçin. Ekle'yi seçin ve bir ortak yönetici yeni kullanıcı ekleyin. Bu, Azure aboneliğinizi yönetmek için Kurumsal hesap sağlar.
5. Son olarak dışında Azure portalında oturum açın ve sonra geri yeni Kurumsal hesap kullanarak oturum aç. Bu hesap ilk zaman oturum varsa, parolayı değiştirmek için istenir.

Microsoft Azure ile bir kurumsal hesap kullanma hakkında daha fazla bilgi için bkz: [Microsoft Azure'a kuruluş olarak kaydolma](../active-directory/sign-up-organization.md).

Birden çok aboneliğiniz varsa ve Azure Anahtar Kasası için kullanmak üzere belirli bir tanesini belirtmek istiyorsanız hesabınızın aboneliklerini görmek için aşağıdaki komutu yazın:

    azure account list

Ardından, kullanılacak aboneliği belirtmek için şunu yazın:

    azure account set <subscription name>

Azure platformlar arası komut satırı arabirimi yapılandırma hakkında daha fazla bilgi için bkz: [yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Azure Resource Manager kullanarak geçiş
Azure Resource Manager anahtar kasası gerektirir, bu nedenle Azure Resource Manager moduna geçmek için aşağıdaki komutu yazın:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Yeni bir kaynak grubu oluşturma
Azure Kaynak Yöneticisi'ni kullanırken, tüm ilgili kaynaklar bir kaynak grubu içinde oluşturulur. Bu öğretici için yeni bir kaynak grubu 'ContosoResourceGroup' oluşturacağız.

    azure group create 'ContosoResourceGroup' 'East Asia'

İlk parametre kaynak grubu adı ve ikinci parametre konumu. Konum için komutunu `azure location list` Bu örnekte bir alternatif bir konum belirtmek nasıl belirlemek üzere. Daha fazla bilgiye ihtiyacınız varsa, yazın:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Anahtar kasası kaynak sağlayıcısını Kaydet
Bu anahtar kasası kaynak sağlayıcısı aboneliğinizde kayıtlı olduğundan emin olun:

`azure provider register Microsoft.KeyVault`

Bu yalnızca bir kez abonelik başına yapılmalıdır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kullanım `azure keyvault create` bir anahtar kasası oluşturmak için komutu. Bu komut dosyasını üç zorunlu parametreye sahiptir: bir kaynak grubu adı, bir anahtar kasası adı ve coğrafi konum.

Örneğin, ContosoKeyVault kasa adını kullanırsanız, ContosoResourceGroup kaynak grubu adını ve Doğu Asya konumunu yazın:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Bu komutun çıktısı, yeni oluşturduğunuz anahtar kasasının özelliklerini gösterir. En önemli iki özellik şunlardır:

* **Ad**: örnekte ContosoKeyVault budur. Bu adı diğer Anahtar Kasası cmdlet'leri için kullanacaksınız.
* **vaultUri**: örnekte https://contosokeyvault.vault.azure.net budur. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Azure hesabınız artık bu anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkilidir. Henüz başka kimse yetkilendirilmedi.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Anahtar kasasına bir anahtar veya gizli anahtar ekleme

Azure anahtar Kasası'nın yazılım korumalı bir anahtar oluşturmasını istiyorsanız, kullanmak `azure key create` komutunu ve aşağıdaki komutu yazın:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Ancak, yerel dosya Azure anahtar Kasası'na karşıya yüklemek istediğiniz softkey.pem adlı bir dosya olarak kaydedilmiş bir .pem dosyasını mevcut bir anahtarı varsa, anahtarı içeri aktarmak için aşağıdaki komutu yazın. Anahtar kasası hizmetinde yazılım ile anahtarı koruyan PEM dosyası:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Artık oluşturduğunuz veya Azure anahtar Kasası'na URI'sini kullanarak karşıya anahtar başvurabilirsiniz. Kullanmak **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** her zaman geçerli sürümü almak ve kullanmak için **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** bu belirli sürümü almak için.

Gizli SQLPassword adlı bir parola olduğu ve değer, Pa$ $w0rd Azure anahtar kasası için olan, kasaya eklemek için aşağıdaki komutu yazın:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Artık Azure Anahtar Kasası'na eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Her zaman geçerli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword** kullanın ve bu belirli sürümü almak için **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** kullanın.

Şimdi yeni anahtar veya gizli görünümü oluşturan:

* Anahtarınızı görüntülemek için şunu yazın: `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Gizli anahtarı görüntülemek için şunu yazın: `azure keyvault secret list --vault-name 'ContosoKeyVault'`

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
2. Solda **Active Directory**'ye tıklayın ve ardından uygulamanızı içine kaydedeceğiniz dizini seçin. <br> <br> 

>[!NOTE] 
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
Anahtar veya gizli kasadaki erişmek için uygulama yetkilendirmek için `azure keyvault set-policy` komutu.

Örneğin, kasa adınız ContosoKeyVault ve yetkilendirmek istediğiniz uygulama 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed istemci Kimliğine sahip kasanızı anahtarlarında oturum açın ve şifresini çözmek için uygulama yetkilendirmek istediğiniz ise, ardından şu komutu çalıştırın:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> Windows komut istemi üzerinde çalıştırıyorsanız, tek tırnak işaretleri çift tırnak işareti ile değiştirin ve ayrıca iç çift tırnak işareti kaçış gerekir. Örneğin: "[\"şifresini\",\"oturum\"]".
> 
> 

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız şunu çalıştırın:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Bir donanım güvenlik modülü (HSM) kullanmak istiyorsanız
Ek güvence için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir. Bu gereksinim sizin için geçerli değilse bu bölümü atlayın ve [Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme](#delete-the-key-vault-and-associated-keys-and-secrets)'ye gidin.

Bu HSM korumalı anahtarları oluşturmak için HSM korumalı anahtarları destekleyen bir kasa aboneliğinizin olması gerekir.

Keyvault oluşturduğunuzda 'sku' parametresini ekleyin:

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Bu kasaya yazılım korumalı anahtarlar ve HSM korumalı anahtarlar (daha önce gösterildiği gibi) ekleyebilirsiniz. HSM korumalı bir anahtar oluşturmak için hedef parametresini 'HSM' ayarlayın:

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

.Pem dosyasını bilgisayarınızdaki bir anahtar almak için aşağıdaki komutu kullanabilirsiniz. Bu komut, anahtarı Anahtar Kasası hizmetindeki HSM'lere aktarır:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Bir sonraki komut bir "kendi anahtarınızı getirin" (BYOK) paketini içeri aktarır. Böylece anahtar HSM sınırını terk etmeden yerel HSM'nizde anahtarınızı oluşturmanız ve Anahtar Kasası hizmetindeki HSM'lere bunu aktarmanız sağlanır.

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Daha ayrıntılı bu BYOK paketini oluşturma hakkında yönergeler için bkz: [HSM-Protected anahtarları Azure anahtar kasası ile kullanmak nasıl](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Anahtar kasasını ve ilişkili anahtarları ve gizli anahtarları silme
Anahtar kasası ve anahtar veya içerdiği gizli artık ihtiyacınız varsa, anahtar kasası azure keyvault silme komutunu kullanarak silebilirsiniz:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Alternatif olarak, anahtar kasasını ve bu gruba eklediğiniz herhangi diğer kaynakları içeren Azure kaynak grubunun tümünü silebilirsiniz.

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Diğer Azure platformlar arası komut satırı arabirimi komutları
Diğer komutlar Azure anahtar kasası yönetmek için kullanışlı olabilir.

Bu komut tüm anahtarların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Bu komut belirtilen anahtar için özelliklerin tam listesini görüntüler:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Bu komut tüm gizli adların ve seçilen özelliklerin tablolu bir görüntüsünü listeler:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Belirli bir anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Belirli bir gizli anahtarın nasıl kaldırılacağına bir örneği burada verilmiştir:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Sonraki adımlar
Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

