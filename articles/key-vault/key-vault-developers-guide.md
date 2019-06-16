---
title: Azure anahtar kasası Geliştirici Kılavuzu
description: Geliştiriciler, Azure anahtar kasası, Microsoft Azure ortamında şifreleme anahtarlarını yönetmek için kullanabilirsiniz.
services: key-vault
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: mbaldwin
ms.openlocfilehash: 72ec3080658b98376952f72f746c1b53fdf7de77
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64704335"
---
# <a name="azure-key-vault-developers-guide"></a>Azure anahtar kasası Geliştirici Kılavuzu

Key Vault, uygulamalarınızın içinden hassas bilgilere güvenli bir şekilde erişmenizi sağlar:

- Kodu kendiniz yazmak zorunda kalmadan anahtarları ve gizli dizileri korunur ve bunları uygulamalarınızdan kolayca kullanabilirsiniz.
- Çekirdek yazılım özelliklerini sağlamaya hakkında yoğunlaşabilirsiniz. böylece kendi anahtarlarınızı yönetme ve müşterilerinize kendi sahip olursunuz. Bu şekilde, uygulamalarınızı müşterilerinizin Kiracı anahtarları ve gizli anahtarları olası bir yükümlülük ve Sorumluluk sahibi değil.
- Uygulamanızı imzalamak için anahtar kullanabilirsiniz ve henüz şifreleme anahtar yönetimi çözümünüz, coğrafi olarak dağıtılmış bir uygulama olarak uygun olmasını sağlar, uygulamanızın dış tutar.
- Key Vault Eylül 2016 sürümü itibarıyla, uygulamalarınızın artık sertifikaları Key Vault yönetebilirsiniz. Daha fazla bilgi için [anahtarlara, parolalara ve sertifikalara hakkında](/rest/api/keyvault/about-keys--secrets-and-certificates).

Azure Key Vault hakkında daha fazla genel bilgi için bkz. [Key Vault nedir](key-vault-whatis.md).

## <a name="public-previews"></a>Genel Önizleme

Periyodik olarak yeni bir Key Vault özelliği genel Önizleme bırakın. Bu deneyin ve aracılığıyla düşüncelerinizi bize iletin azurekeyvault@microsoft.com, bizim geri bildirim e-posta adresi.

### <a name="storage-account-keys---july-10-2017"></a>Depolama hesabı anahtarları - 10 Temmuz 2017

>[!NOTE]
>Azure Key Vault Bu güncelleştirme yalnızca **depolama hesabı anahtarlarını** özelliği önizlemededir.

Bu önizleme, yeni depolama hesabı anahtarlarını özelliğimiz, bu arabirimler kullanılabilir içerir; [.NET / C#](/dotnet/api/microsoft.azure.keyvault/), [REST](/rest/api/keyvault/) ve [PowerShell](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault). 

Yeni depolama hesabı anahtarlarını özelliği hakkında daha fazla bilgi için bkz. [Azure Key Vault depolama hesabı anahtarlarına genel bakış](key-vault-ovw-storage-keys.md).

## <a name="videos"></a>Videolar

Bu videoda, anahtar kasanızı oluşturmak nasıl ve 'Hello anahtar Kasası' örnek uygulamadan kullanma işlemini gösterir.

- [Anahtar kasası Geliştirici - Hızlı Başlangıç Kılavuzu](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

Yukarıdaki videoda belirtilen kaynaklar:

- [Azure PowerShell](https://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Azure anahtar kasası örnek kod](https://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>Oluşturma ve anahtar kasalarını yönetme

Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. Azure kaynakları için yönetilen kimlikleri, Azure hizmetleri otomatik olarak yönetilen bir kimlik Azure Active Directory (Azure AD) sağlayarak bu sorunu daha basit çözme hale getirir. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. 

Azure kaynakları için yönetilen kimlikleri hakkında daha fazla bilgi için bkz. [yönetilen kimlikleri genel bakış](../active-directory/managed-identities-azure-resources/overview.md). AAD ile çalışma hakkında daha fazla bilgi için bkz. [uygulamaları Azure Active Directory ile tümleştirme](../active-directory/develop/active-directory-integrating-applications.md).

Anahtar kasanızda anahtarları, gizli dizileri veya sertifikalar ile çalışmaya başlamadan önce oluşturacak ve CLI, PowerShell, Resource Manager şablonları ya da, REST üzerinden anahtar kasanıza aşağıdaki makalelerde açıklanan şekilde yönetin:

- [Oluşturma ve CLI ile anahtar kasalarını yönetme](key-vault-manage-with-cli2.md)
- [Oluşturma ve PowerShell ile anahtar kasalarını yönetme](key-vault-overview.md)
- [Key vault oluşturma ve bir Azure Resource Manager şablonu aracılığıyla bir gizli dizi ekleme](../azure-resource-manager/resource-manager-template-keyvault.md)
- [REST ile anahtar kasalarını yönetme ve oluşturma](/rest/api/keyvault/)


## <a name="coding-with-key-vault"></a>Key Vault ile kodlama

Anahtar kasası yönetim sistemi programcıları için birkaç arabirimleri oluşur. Bu bölüm, tüm diller yanı sıra bazı kod örnekleri bağlantılarını içerir. 

### <a name="supported-programming-and-scripting-languages"></a>Desteklenen dilleri komut dosyası ve programlama

#### <a name="rest"></a>REST

Tüm anahtar kasası kaynaklarınızın REST arabirimi aracılığıyla erişilebilen; kasaları, anahtarları, gizli dizileri, vb. 

[Anahtar kasası REST API Başvurusu](/rest/api/keyvault/).

#### <a name="net"></a>.NET

[Key Vault için .NET API Başvurusu](/dotnet/api/microsoft.azure.keyvault).

.NET SDK 2.x sürümü hakkında daha fazla bilgi için bkz. [sürüm notları](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

[Java SDK'sı için anahtar kasası](/java/api/overview/azure/keyvault)

#### <a name="nodejs"></a>Node.js

Node.js'de Key Vault Yönetimi API'si ve API Key Vault nesne ayrıdır. Aşağıdaki genel bakış makalesi hem de erişmenizi sağlar. 

[Node.js için Azure Key Vault modülleri](/nodejs/api/overview/azure/key-vault)

#### <a name="python"></a>Python

[Python için Azure Key Vault kitaplıkları](/python/api/overview/azure/key-vault)

#### <a name="azure-cli-2"></a>Azure CLI 2

[Key Vault için Azure CLI](/cli/azure/keyvault)

#### <a name="azure-powershell"></a>Azure PowerShell 

[Key Vault için Azure PowerShell](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)

### <a name="quick-start-guides"></a>Hızlı Başlangıç kılavuzları

- [Anahtar kasası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [Node.js'de Key Vault'u kullanmaya başlama](https://github.com/Azure-Samples/key-vault-node-getting-started)

### <a name="code-examples"></a>Kod örnekleri

Key Vault ile uygulamalarınızı kullanan tam örnekler için bkz:

- [Azure Key Vault kod örnekleri](https://azure.microsoft.com/resources/samples/?service=key-vault) -Azure Key Vault için kod örnekleri. 
- [Bir Web uygulamasından Azure Key Vault'u kullanın](quick-create-net.md) -azure'da bir web uygulamasından Azure Key Vault kullanmayı öğrenmenize yardımcı olacak öğretici. 

## <a name="how-tos"></a>Nasıl yapılır makaleleri

Aşağıdaki makaleler ve senaryoları Azure anahtar kasası ile çalışmaya yönelik görev özgü yönergeler sağlar:

- [Değişiklik anahtar kasası Kiracı Kimliğini sonra abonelik taşıma](key-vault-subscription-move-fix.md) - Azure aboneliğinizi A kiracısından B kiracısı ile taşıdığınızda mevcut anahtar kasalarınıza b Bu kılavuzu kullanarak bu düzeltme b kiracısındaki Sorumlular (kullanıcılar ve uygulamalar) tarafından erişilemez hale gelir.
- [Güvenlik duvarının arkasındaki anahtar kasası erişim](key-vault-access-behind-firewall.md) - anahtar kasası istemci uygulamanızın çeşitli işlevlere ilişkin birden çok uç noktaya erişebilmesi için bir anahtar kasasına erişmek için.
- [Oluşturma ve Transfer HSM-Protected anahtarları Azure Key Vault için](key-vault-hsm-protected-keys.md) -Bu, planlama, oluşturma ve Azure anahtar kasası ile kullanmak için kendi HSM korumalı anahtarlar'ı aktarım yardımcı olur.
- [Dağıtım sırasında güvenlik değerlerini (parolalar gibi) geçirmek nasıl](../azure-resource-manager/resource-manager-keyvault-parameter.md) - güvenli bir değerle (parola gibi) dağıtım sırasında parametre olarak geçirmek, ihtiyacınız olduğunda bu değer, bir Azure anahtar Kasası'nda bir gizli dizi olarak depolamak ve diğer kaynak değeri başvurusu Yönetici Şablonları.
- [SQL Server ile Genişletilebilir anahtar yönetimi için Key Vault kullanma](https://msdn.microsoft.com/library/dn198405.aspx) -Azure Key Vault için SQL Server Bağlayıcısı, SQL Server ve VM, SQL korumak için bir Genişletilebilir anahtar yönetimi (EKM) sağlayıcısı olarak Azure Key Vault hizmetinden yararlanmasını etkinleştirir, uygulamalar için şifreleme anahtarları; Saydam veri şifrelemesi, yedekleme şifrelemesi ve sütun düzeyinde şifreleme desteklenmiyor.
- [Sertifikaları Key Vault'tan Vm'lere dağıtmak nasıl](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - VM ile bir sertifika Azure gereksinimlerine göre çalışan bir bulut uygulaması. Nasıl, bu sertifika bu VM'ye bugün elde ederim?
- [Anahtar kasası uçtan uca anahtar döndürme ve denetleme ile ayarlama nasıl](key-vault-key-rotation-log-monitoring.md) - bu Azure anahtar kasası ile denetim ve anahtar döndürme ayarlama konusunda yol göstermektedir.
- [Key Vault aracılığıyla Azure Web App sertifikası dağıtma]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) sertifikaları Key Vault'ta depolanan bir parçası olarak dağıtmak için adım adım yönergeler sağlar [App Service sertifikası](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) teklifidir.
- [Çok sayıda uygulamaya bir anahtar kasasına erişim izni vermek](key-vault-group-permissions-for-apps.md) anahtar kasası erişim denetimi İlkesi, en fazla 1024 girişleri destekler. Ancak, bir Azure Active Directory güvenlik grubu oluşturabilirsiniz. Tüm ilişkili hizmet sorumlularını bu sistem güvenlik grubuna ekleyin ve bu güvenlik grubu için Key Vault erişim hakkı.
- Tümleştirme ve Azure ile Key Vault kullanma hakkında daha fazla görev özgü yönergeler için bkz: [Ryan Gareth Azure Resource Manager şablonu örnekleri için Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- [Key Vault geçici silmeyi CLI ile kullanmayı](key-vault-soft-delete-cli.md) etkin ile geçici silme kullanın ve bir anahtar kasası ve çeşitli anahtar kasası nesne yaşam döngüsü ile yol gösterir.
- [PowerShell ile Key Vault geçici silmeyi kullanma](key-vault-soft-delete-powershell.md) etkin ile geçici silme kullanın ve bir anahtar kasası ve çeşitli anahtar kasası nesne yaşam döngüsü ile yol gösterir.

## <a name="integrated-with-key-vault"></a>Key Vault ile tümleşik

Bu makaleler diğer senaryolar ve kullanabilir veya Key Vault ile tümleştirme hizmetleri üzeresiniz.

- [Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md) endüstri standardı yararlanır [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için bir özelliğidir. Denetim ve disk şifreleme anahtarlarını ve gizli dizileri sanal makine disklerini tüm veriler Azure depolama alanınızda bekleme sırasında şifrelenir sağlarken, anahtar kasası aboneliğinizdeki yönetmenize yardımcı olması için Azure Key Vault ile tümleşik bir çözüm.
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) hesapta depolanan verilerin şifrelenmesi için seçenek sunar. Anahtar Yönetimi için Data Lake Store, Data Lake Store içinde depolanan tüm verilerin şifresini çözmek için gerekli olan, ana şifreleme anahtarlarının (Mek'ler) yönetimi için iki mod sağlar. Data Lake Store, Mek'leri yönetmek veya Azure anahtar kasası hesabınızı kullanarak Mek'ler sahipliğini tutmayı seçin ya da sağlayabilirsiniz. Anahtar Yönetimi modunda, bir Data Lake Store hesabı oluşturma sırasında belirtin.
- [Azure Information Protection](/azure/information-protection/plan-implement-tenant-key) kendi Kiracı anahtarınızı yöneticisine sağlar. Örneğin, Microsoft Kiracı anahtarınızı (varsayılan) yerine, kuruluşunuz için geçerli olan belirli düzenlemelere uymak üzere kendi Kiracı anahtarınızı yönetebilirsiniz. Kendi Kiracı anahtarınızı yönetme olarak kendi anahtarını Getir veya BYOK için de denir.

## <a name="key-vault-overviews-and-concepts"></a>Key Vault genel bakışlar ve kavramları

- [Key Vault geçici silme davranışı](key-vault-ovw-soft-delete.md) silinen nesneleri kurtarma sağlayan bir özelliği silme yanlışlıkla veya kasıtlı olup olmadığını açıklar.
- [Anahtar kasası istemci azaltma](key-vault-ovw-throttling.md) azaltma temel kavramlara yönlendirir ve uygulamanız için bir yaklaşım sunar.
- [Key Vault depolama hesabı anahtarlarına genel bakış](key-vault-ovw-storage-keys.md) Key Vault tümleştirmesi Azure depolama hesabı anahtarları açıklar.
- [Key Vault güvenlik dünyaları](key-vault-ovw-security-worlds.md) bölgeleri ve güvenlik alanlar arasındaki ilişkileri açıklar.

## <a name="social"></a>Sosyal

- [Anahtar kasası blogu](https://aka.ms/kvblog)
- [Anahtar kasası Forumu](https://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Destek kitaplıkları

- [Microsoft Azure anahtar kasası çekirdek Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) sağlar **IKey** ve **IKeyResolver** tanımlayıcıları anahtarlarını bulma ve anahtarlarla işlemleri gerçekleştirmek için arabirim.
- [Microsoft Azure anahtar kasası uzantıları](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) Azure anahtar kasası için genişletilmiş özellikler sunar.
