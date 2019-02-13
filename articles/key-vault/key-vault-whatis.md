---
title: Azure Anahtar Kasası nedir? -Azure anahtar kasası | Microsoft Docs
description: Azure Key Vault koruma şifreleme anahtarlarını ve gizli dizileri bulut uygulamaları ve Hizmetleri tarafından kullanılan. Müşteriler kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları şifrelemenize olanak tanır. PFX dosyaları ve parolalar, donanım güvenlik modülleri (HSM'ler) tarafından korunan anahtarları kullanarak.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: cf45aeb88d9638dd2f0c93bbae0b8ff79b293435
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56108118"
---
# <a name="what-is-azure-key-vault"></a>Azure Anahtar Kasası nedir?

Azure Key Vault, aşağıdaki sorunları çözmenize yardımcı olur:

- **Gizli dizileri Yönetim** -Azure Key Vault, güvenli bir şekilde saklayın ve sıkı bir şekilde belirteçleri, parolaları, sertifikaları, API anahtarlarını ve diğer gizli dizileri erişimi denetlemek için kullanılabilir.
- **Anahtar Yönetimi**: Azure Key Vault, Anahtar Yönetimi çözümü olarak da kullanılabilir. Azure Key Vault, verilerinizi şifrelemek için kullanılan şifreleme anahtarlarını oluşturmayı ve denetlemeyi kolaylaştırır. 
- **Sertifika Yönetimi**: Azure Key Vault aynı zamanda, Azure ve bağlı iç kaynaklarınızla kullanım için genel ve özel Güvenli Yuva Katmanı/Aktarım Katmanı Güvenliği (SSL/TLS) sertifikalarını kolayca hazırlamanıza, yönetmenize ve dağıtmanıza olanak sağlayan bir hizmettir. 
- **Gizli dizileri donanım güvenlik modülleri tarafından desteklenen Store** -gizli dizileri ve anahtarları korunan yazılım veya FIPS 140-2 Düzey 2 HSM'ler doğrular.

## <a name="basic-concepts"></a>Temel kavramlar

Azure Key Vault, gizli dizilerin güvenli bir şekilde depolanması ve bunlara erişim sağlanması için tasarlanmış bir araçtır. Gizli dizi; API anahtarları, parolalar veya sertifikalar gibi erişimi sıkı bir şekilde denetlemek istediğiniz tüm öğelerdir. A **kasası** gizli dizileri oluşan mantıksal gruptur. Artık Key Vault ile herhangi bir işlemi yapmak için önce için kimlik doğrulaması gerekir. 

Temelde anahtar Kasası'na kimlik doğrulaması için üç yolu vardır:

1. **Kullanarak [kimliklerini Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)**  (**önerilen ve en iyi**): Azure'da bir sanal makinede bir uygulamayı dağıttığınızda, sanal makinenize Key Vault erişimi olan bir kimlik atayabilirsiniz. Listelenen diğer azure kaynakları için kimlikleri de atayabilirsiniz [burada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Bu yaklaşımın avantajı, uygulama / hizmet ilk gizli döndürmesini yönetmeye değil. Azure, otomatik olarak kimliğini döndürür. 
2. **Hizmet sorumlusu ve sertifika kullanma:** İkinci seçenek, bir hizmet sorumlusu ve anahtar kasası erişimi olan ilişkili bir sertifika kullanmaktır. Uygulama sahibi veya Geliştirici sertifika döndürme kullanýldýðýnýanýmsama olduğunu ve bu nedenle bu önerilmez.
3. **Hizmet sorumlusu ve gizli anahtarını kullanarak:** Üçüncü bir seçenek (değil tercih edilen seçenek) bir hizmet sorumlusu ve bir gizli anahtar Kasası'na kimlik doğrulaması için kullanmaktır.

> [!NOTE]
> Önyükleme gizli anahtar Kasası'na kimliğini doğrulamak için kullanılan Otomatik döndürme zor olduğundan, yukarıdaki 3 seçeneği kullanılmamalıdır.

Aşağıda bazı önemli terimler verilmiştir:

- **Kiracı**: Bir kiracının sahip olduğu ve belirli bir Microsoft bulut Hizmetleri örneği yöneten kuruluştur. Genelde bir kuruluş için Azure ve Office 365 hizmetlerinden oluşan kümeyi belirtmek üzere kullanılır.
- **Kasasının sahibine**: Kasasının sahibine, anahtar kasası oluşturma ve tam erişime ve denetime ele elde edebilirsiniz. Kasa sahibi, gizli dizilere ve anahtarlara kimlerin eriştiğini günlüğe kaydetmek için belirli denetim özellikleri de ayarlayabilir. Yöneticiler anahtarların yaşam döngüsünü kontrol edebilir. Anahtarı yeni sürüme geçirebilir, yedekleyebilir ve ilgili görevleri gerçekleştirebilirler.
- **Kasa tüketici**: Tüketici erişim kasasının sahibine verdiğinde kasa tüketici anahtar kasasının içine varlıklar üzerinde eylemler gerçekleştirebilirsiniz. Kullanılabilen eylemler verilen izinlere bağlıdır.
- **Kaynak**: Azure ile kullanılabilen yönetilebilir bir öğe bir kaynaktır. Sanal makine, depolama hesabı, web uygulaması, veritabanı ve sanal ağ bazı yaygın kaynaklardandır, ancak çok daha fazlası mevcuttur.
- **Kaynak grubu**: Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz.
- **Hizmet sorumlusu** -bir Azure hizmet sorumlusu olduğundan kullanıcı tarafından oluşturulan uygulamalar, hizmetler ve Otomasyon araçlarının belirli Azure kaynaklarına erişmek için kullanılan bir güvenlik kimliği. Bunu belirli bir rolü olan ve izinleri sıkı bir şekilde denetlenen bir "kullanıcı kimliği" (kullanıcı adı ve parola veya sertifika) olarak düşünebilirsiniz. Genel kullanıcı kimliğinin aksine, hizmet sorumlusunun yalnızca belirli şeyleri yapabilmesi gerekir. Hizmet sorumlusuna yönetim görevlerini gerçekleştirmesi için gereken en düşük izin düzeyini atamanız, güvenliği geliştirir.
- **[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md)**: Azure AD, bir kiracı için Active Directory hizmetidir. Her dizinde bir veya daha fazla etki alanı vardır. Dizinde birden fazla abonelik bulunabilir ancak tek bir kiracı olur. 
- **Azure Kiracı kimliği**: Kiracı kimliği, bir Azure aboneliğinde bir Azure AD örneğini tanımlamak için benzersiz bir yoldur.
- **Kimlikler Azure kaynakları için yönetilen**: Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. Yönetilen kimlik kullanarak Azure hizmetleri otomatik olarak yönetilen bir kimlik sağlayarak Azure AD'de basit bu sorunu çözme yapar. Bu kimliği kullanarak, Key Vault veya Azure AD kimlik doğrulamasını destekleyen tüm hizmetler için kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. Daha fazla bilgi için aşağıdaki resme bakın ve [yönetilen Azure kaynaklarına genel bakış için kimlikleri](../active-directory/managed-identities-azure-resources/overview.md).

    ![Nasıl diyagramı yönetilen Azure kaynaklarını iş kimlikleri](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Key Vault rolleri

Geliştiricilerin ve güvenlik yöneticilerinin ihtiyaçlarını karşılamaya Anahtar Kasası'nın nasıl yardımcı olabileceğini daha iyi anlamak için aşağıdaki tabloyu kullanın.

| Rol | Sorun bildirimi | Azure Anahtar Kasası tarafından çözüldü |
| --- | --- | --- |
| Bir Azure uygulaması geliştiricisi |"İmzalama ve şifreleme için anahtarları kullanan Azure'a yönelik bir uygulama yazmak istiyorum ancak bu anahtarların benim uygulamamın dışında olmasını ve böylelikle çözümün coğrafi olarak dağıtılan bir uygulamaya uygun olmasını istiyorum. <br/><br/>Kodu kendim yazmak zorunda kalmadan bu anahtarların ve parolaların korunmasını istiyorum. Ayrıca bu anahtarları ve parolaları uygulamamdan en iyi performans ile kolayca kullanabilmek istiyorum." |√ Anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.<br/><br/> √ Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri kullanılarak Azure tarafından korunur.<br/><br/> √ Anahtarlar uygulamalarla aynı Azure veri merkezinde bulunan HSM'lerde işlenir. Bu yöntem, şirket içi gibi ayrı konumlarda bulunan anahtarlardan daha fazla güvenilirlik ve daha az gecikme sağlar. |
| Hizmet olarak yazılım (SaaS) geliştiricisi |"Müşterilerimin kiracı anahtarları ve gizli anahtarları için sorumluluk veya olası bir yükümlülük almak istemiyorum. <br/><br/>Müşterilerin kendi anahtarlarına sahip olmalarını ve bunları yönetmelerini istiyorum; böylece en iyi yaptığım şeye, yani çekirdek yazılım özelliklerini sağlamaya odaklanabilirim." |√ Müşteriler kendi anahtarları Azure'a aktarabilir ve bunları yönetebilir. Bir SaaS uygulamasının müşterilerin anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, Anahtar Kasası uygulama adına bu işlemleri yapar. Uygulama, müşterilerin anahtarlarını görmez. |
| Güvenlik başkanı (CSO) |"Uygulamalarımızın güvenli anahtar yönetimi için FIPS 140-2 Düzey 2 HSM'lerle uyumlu olduğunu bilmek istiyorum. <br/><br/>Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum. <br/><br/>Ayrıca, birden çok Azure hizmeti ve kaynağı kullanıyor olsak da, anahtarları Azure'da tek bir konumdan yönetmek istiyorum." |√ HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir.<br/><br/>√ Anahtar Kasası, Microsoft anahtarlarınızı görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.<br/><br/>√ Anahtar kullanımı neredeyse gerçek zamanlı olarak günlüğe kaydedilir.<br/><br/>√ Kasa, Azure'da kaç kasanız olduğuna, bunların hangi bölgeleri desteklediğine ve hangi uygulamaların bunları kullandığına bakmaksızın tek bir arabirim sağlar. |

Bir Azure aboneliği olan herhangi biri, anahtar kasalarını oluşturabilir ve kullanabilir. Anahtar Kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da bir kuruluş için diğer Azure hizmetlerini yöneten bir kuruluş yöneticisi tarafından uygulanabilir ve yönetilebilir. Örneğin, bu yönetici bir Azure aboneliğiyle oturum açabilir, anahtarların depolanacağı kuruluş için bir kasa oluşturur ve ardından şunlar gibi işlemsel görevlerden sorumlu olur:

- Bir anahtar veya gizli anahtarı oluşturma veya içeri aktarma
- Bir anahtar veya gizli anahtarı iptal etme veya silme
- Kullanıcıların veya uygulamaların, anahtar ve gizli anahtarları yönetmeleri veya kullanmaları için anahtar kasasına erişmelerini yetkilendirme
- Anahtar kullanımını (örneğin, imzalama veya şifreleme) yapılandırma
- Anahtar kullanımını izleme

Ardından, bu yönetici uygulamalarından çağırmaları için geliştiricilere URI'ler sağlar ve güvenlik yöneticisine anahtar kullanımı günlüğü bilgilerini sunar. 

![Azure Key Vault nasıl çalıştığına ilişkin genel bakış][1]

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için bkz. [Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).
