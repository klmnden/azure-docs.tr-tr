---
title: Azure Anahtar Kasası nedir? | Microsoft Docs
description: Azure Key Vault, şifreleme anahtarlarını koruma sağlar ve bulut uygulamaları ve Hizmetleri gizli dizileri kullanmak nasıl öğrenin.
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: 48ac0c3efe74723099e87a77871aa1a78834efbd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640574"
---
# <a name="what-is-azure-key-vault"></a>Azure Anahtar Kasası nedir?

Bulut uygulamaları ve Hizmetleri şifreleme anahtarları kullanır ve bilgi tutmaya yardımcı olmak için gizli dizileri koruyun. Bu anahtarları ve gizli anahtarları Azure Key Vault korur. Anahtar Kasası'nı kullandığınızda, kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları, .pfx dosyaları ve parolalar, donanım güvenlik modülleri (HSM'ler) tarafından korunan anahtarları kullanarak şifreleme de yapabilirsiniz.

Key Vault, aşağıdaki sorunları çözmenize yardımcı olur:

- **Gizli dizi Yönetimi**: Güvenli bir şekilde saklayın ve sıkı bir şekilde erişim belirteçleri, parolaları, sertifikaları, API anahtarlarını ve diğer gizli dizileri denetleyebilirsiniz.
- **Anahtar Yönetimi**: Oluşturun ve verilerinizi şifrelemek için şifreleme anahtarları denetleyebilirsiniz. 
- **Sertifika yönetimi**: Sağlama, yönetme ve Azure ve iç bağlı kaynaklarınız ile kullanılmak üzere ortak ve özel Güvenli Yuva Katmanı/Aktarım Katmanı Güvenliği (SSL/TLS) sertifikalarını dağıtma. 
- **Gizli dizileri Hsm'leri tarafından desteklenen Store**: Yazılım veya FIPS 140-2 Düzey 2 doğrulanmasına HSM'ler gizli dizileri ve anahtarları korunmasına yardımcı olmak için kullanın.

## <a name="basic-concepts"></a>Temel kavramlar

Azure Key Vault, gizli dizilerin güvenli bir şekilde depolanması ve bunlara erişim sağlanması için tasarlanmış bir araçtır. Gizli dizi; API anahtarları, parolalar veya sertifikalar gibi erişimi sıkı bir şekilde denetlemek istediğiniz tüm öğelerdir. Bir kasa gizli dizilerinin mantıksal grubudur.

Diğer önemli koşullar şunlardır:

- **Kiracı**: Bir kiracının sahip olduğu ve belirli bir Microsoft bulut Hizmetleri örneği yöneten kuruluştur. Genellikle, bir kuruluş için Azure ve Office 365 Hizmetleri kümesi başvurmak için kullanılır.

- **Kasasının sahibine**: Kasasının sahibine, anahtar kasası oluşturma ve tam erişime ve denetime ele elde edebilirsiniz. Kasa sahibi, gizli dizilere ve anahtarlara kimlerin eriştiğini günlüğe kaydetmek için belirli denetim özellikleri de ayarlayabilir. Yöneticiler anahtarların yaşam döngüsünü kontrol edebilir. Anahtarı yeni sürüme geçirebilir, yedekleyebilir ve ilgili görevleri gerçekleştirebilirler.

- **Kasa tüketici**: Tüketici erişim kasasının sahibine verdiğinde kasa tüketici anahtar kasasının içine varlıklar üzerinde eylemler gerçekleştirebilirsiniz. Kullanılabilen eylemler verilen izinlere bağlıdır.

- **Kaynak**: Azure ile kullanılabilen yönetilebilir bir öğe bir kaynaktır. Sanal makine, depolama hesabı, web uygulaması, veritabanı ve sanal ağ ortak örnek verilebilir. Vardır çok daha fazlası.

- **Kaynak grubu**: Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz.

- **Hizmet sorumlusu**: Bir Azure hizmet sorumlusu, kullanıcı tarafından oluşturulan uygulamalar, hizmetler ve Otomasyon araçlarının belirli Azure kaynaklarına erişmek için kullanan bir güvenlik kimliğidir. "Kullanıcı kimliği" olarak düşünebilirsiniz (kullanıcı adı ve parola veya sertifika) belirli bir rolü ve sıkı bir şekilde denetlenen ile. Genel kullanıcı kimliğinin aksine, hizmet sorumlusunun yalnızca belirli şeyleri yapabilmesi gerekir. Hizmet sorumlusuna yönetim görevlerini gerçekleştirmek için gereken en düşük izin düzeyini atamanız, güvenliği geliştirir.

- [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md): Azure AD, bir kiracı için Active Directory hizmetidir. Her dizinde bir veya daha fazla etki alanı vardır. Dizinde birden fazla abonelik bulunabilir ancak tek bir kiracı olur.

- **Azure Kiracı kimliği**: Kiracı kimliği, bir Azure aboneliğinde bir Azure AD örneğini tanımlamak için benzersiz bir yoldur.

- **Yönetilen kimlik**: Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. Yönetilen kimlik kullanarak Azure hizmetleri otomatik olarak yönetilen bir kimlik sağlayarak Azure AD'de basit bu sorunu çözme yapar. Bu kimliği kullanarak, Key Vault veya Azure AD kimlik doğrulamasını destekleyen tüm hizmetler için kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. Daha fazla bilgi için aşağıdaki resme bakın ve [Azure kaynakları için yönetilen kimlikleri bakış](../active-directory/managed-identities-azure-resources/overview.md).

    ![Azure kaynaklarını iş için yönetilen kimlikleri diyagramı](./media/key-vault-whatis/msi.png)

## <a name="authentication"></a>Kimlik Doğrulaması
Key Vault ile herhangi bir işlemi yapmak için önce için kimlik doğrulaması yapması gerekir. Anahtar Kasası'na kimlik doğrulaması için üç yolu vardır:

- [Kimlikler Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview): Azure'da bir sanal makinede bir uygulamayı dağıttığınızda, sanal makinenize Key Vault erişimi olan bir kimlik atayabilirsiniz. Kimlikleri de atayabilirsiniz [diğer Azure kaynakları](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Bu yaklaşımın avantajı uygulama veya hizmetin ilk gizli döndürmesini yönetme değil ' dir. Azure, otomatik olarak kimliğini döndürür. En iyi uygulama bu yaklaşım önerilir. 
- **Hizmet sorumlusu ve sertifika**: Bir hizmet sorumlusu ve anahtar kasası erişimi olan ilişkili bir sertifika kullanabilirsiniz. Uygulama sahibi veya Geliştirici sertifika döndürme gerekir çünkü bu yaklaşım önerilmemektedir.
- **Hizmet sorumlusu ve gizli dizi**: Anahtar Kasası'na kimlik doğrulaması için bir hizmet sorumlusu ve bir gizli dizi kullanabilirsiniz, ancak bunu önermiyoruz. Anahtar Kasası'na kimliğini doğrulamak için kullanılan önyükleme gizli dizi otomatik olarak döndürmek zordur.


## <a name="key-vault-roles"></a>Key Vault rolleri

Geliştiricilerin ve güvenlik yöneticilerinin ihtiyaçlarını karşılamaya Anahtar Kasası'nın nasıl yardımcı olabileceğini daha iyi anlamak için aşağıdaki tabloyu kullanın.

| Rol | Sorun bildirimi | Azure Anahtar Kasası tarafından çözüldü |
| --- | --- | --- |
| Bir Azure uygulaması geliştiricisi |"İmzalama ve şifreleme için anahtarları kullanan Azure'a yönelik bir uygulama yazmak istiyorum. Ancak, bu anahtarlar, çözümün coğrafi olarak dağıtılmış bir uygulama için uygun olacak şekilde benim uygulamamın dışında olmasını istiyorum. <br/><br/>Kodu kendim yazmak zorunda kalmadan bu anahtarların ve parolaların korunmasını istiyorum. Ayrıca bu anahtarları ve parolaları uygulamamdan en iyi performans ile kolayca kullanabilmek istiyorum." |√ Anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.<br/><br/> √ Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri kullanılarak Azure tarafından korunur.<br/><br/> √ Anahtarlar uygulamalarla aynı Azure veri merkezinde bulunan HSM'lerde işlenir. Bu yöntem, şirket içi gibi ayrı konumlarda bulunan anahtarlardan daha fazla güvenilirlik ve daha az gecikme sağlar. |
| Hizmet olarak yazılım (SaaS) geliştiricisi |"Müşterilerimin kiracı anahtarları ve gizli anahtarları için sorumluluk veya olası bir yükümlülük almak istemiyorum. <br/><br/>Müşterilerin kendi anahtarlarına sahip olmalarını ve bunları yönetmelerini istiyorum; böylece en iyi yaptığım şeye, yani çekirdek yazılım özelliklerini sağlamaya odaklanabilirim." |√ Müşteriler kendi anahtarları Azure'a aktarabilir ve bunları yönetebilir. Bir SaaS uygulamasının müşterilerin anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, anahtar kasası uygulama adına bu işlemleri yapar. Uygulama, müşterilerin anahtarlarını görmez. |
| Güvenlik başkanı (CSO) |"Uygulamalarımızın güvenli anahtar yönetimi için FIPS 140-2 Düzey 2 HSM'lerle uyumlu olduğunu bilmek istiyorum. <br/><br/>Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum. <br/><br/>Ayrıca, birden çok Azure hizmeti ve kaynağı kullanıyor olsak da, anahtarları Azure'da tek bir konumdan yönetmek istiyorum." |√ HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir.<br/><br/>√ Anahtar Kasası, Microsoft anahtarlarınızı görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.<br/><br/>√ Anahtar kullanımı neredeyse gerçek zamanlı olarak günlüğe kaydedilir.<br/><br/>√ Kasa, Azure'da kaç kasanız olduğuna, bunların hangi bölgeleri desteklediğine ve hangi uygulamaların bunları kullandığına bakmaksızın tek bir arabirim sağlar. |

Bir Azure aboneliği olan herhangi biri, anahtar kasalarını oluşturabilir ve kullanabilir. Anahtar kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da, uygulanan ve diğer Azure hizmetlerini yöneten bir kuruluşun Yöneticisi tarafından yönetilir. Örneğin, bu yönetici bir Azure aboneliği ile oturum açın, oturum anahtarlarını depolamak ve ardından bunlar gibi işletimsel görevleri sorumlu kuruluş için bir kasa oluşturun:

- Bir anahtar veya gizli anahtarı oluşturma veya içeri aktarma
- Bir anahtar veya gizli anahtarı iptal etme veya silme
- Kullanıcıların veya uygulamaların, anahtar ve gizli anahtarları yönetmeleri veya kullanmaları için anahtar kasasına erişmelerini yetkilendirme
- Anahtar kullanımını (örneğin, imzalama veya şifreleme) yapılandırma
- Anahtar kullanımını izleme

Bu yönetici, geliştiricilerin uygulamalarından çağırmaları için bir URI'leri ardından sağlar. Bu yönetici, ayrıca güvenlik yöneticisine anahtar kullanımı günlüğü bilgilerini sağlar. 

![Azure Key Vault nasıl çalıştığına ilişkin genel bakış][1]

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için bkz. [Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).
