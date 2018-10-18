---
title: Azure Anahtar Kasası nedir? | Microsoft Docs
description: Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure Anahtar Kasası'nı kullanarak müşteriler, anahtarları ve gizli anahtarları (kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları, .PFX dosyaları ve parolalar gibi), donanım güvenlik modülleri tarafından korunan anahtarları kullanarak şifreleyebilir.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: barclayn
ms.openlocfilehash: 56a1ebcfbb6dda9bc96aa241bd2b8d753022181a
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49385875"
---
# <a name="what-is-azure-key-vault"></a>Azure Anahtar Kasası nedir?

Azure Key Vault, aşağıdaki sorunların çözülmesine yardımcı olur
- **Gizli Dizi Yönetimi**: Belirteçleri, parolaları, sertifikaları, API anahtarlarını ve diğer gizli dizileri Güvenle depolamak ve bunlara erişimi sıkı bir şekilde denetlemek için Azure Key Vault kullanılabilir.
- **Anahtar Yönetimi**: Azure Key Vault, Anahtar Yönetimi çözümü olarak da kullanılabilir. Azure Key Vault, verilerinizi şifrelemek için kullanılan şifreleme anahtarlarını oluşturmayı ve denetlemeyi kolaylaştırır. 
- **Sertifika Yönetimi**: Azure Key Vault aynı zamanda, Azure ve bağlı iç kaynaklarınızla kullanım için genel ve özel Güvenli Yuva Katmanı/Aktarım Katmanı Güvenliği (SSL/TLS) sertifikalarını kolayca hazırlamanıza, yönetmenize ve dağıtmanıza olanak sağlayan bir hizmettir. 
- **Gizli dizileri Donanım Güvenlik Modülleri desteğiyle saklayın**: Gizli diziler ve anahtarlar yazılım tarafından korunabilir veya FIPS 140-2 Düzey 2, HSM'leri doğrular

## <a name="basic-concepts"></a>Temel kavramlar

Azure Key Vault, gizli dizilerin güvenli bir şekilde depolanması ve bunlara erişim sağlanması için tasarlanmış bir araçtır. Gizli dizi; API anahtarları, parolalar veya sertifikalar gibi erişimi sıkı bir şekilde denetlemek istediğiniz tüm öğelerdir. A **kasası** gizli dizileri oluşan mantıksal gruptur. Artık Key Vault ile herhangi bir işlemi yapmak için önce için kimlik doğrulaması gerekir. 

Temelde anahtar Kasası'na kimlik doğrulaması için izleyebileceğiniz 3 yol vardır

1. **Kullanarak [kimliklerini Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)**  (**önerilen ve en iyi**): azure'da bir sanal makinede bir uygulamayı dağıttığınızda, sanal makinenize bir kimlik atayabilir. Bu anahtar kasası erişimi vardır. Listelenen diğer azure kaynakları için kimlikleri de atayabilirsiniz [burada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Bu yaklaşımın avantajı, uygulama / hizmet ilk gizli döndürmesini yönetmeye değil. Azure, otomatik olarak kimliğini döndürür. 
2. **Hizmet sorumlusu ve sertifika kullanılarak:** 2 seçenek bir hizmet sorumlusu ve anahtar kasası erişimi olan ilişkili bir sertifika kullanmaktır. Uygulama sahibi veya Geliştirici sertifika döndürme kullanýldýðýnýanýmsama olduğunu ve bu nedenle bu önerilmez
3. **Hizmet sorumlusu gizli anahtarı ile:** 3 (değil tercih edilen seçenek) anahtar Kasası'na kimlik doğrulaması için bir hizmet sorumlusu ve bir gizli dizi kullanmak için bir seçenektir

Aşağıda bazı önemli terimler verilmiştir:
- **Kiracı**: Kiracı, Microsoft bulut hizmetlerinin belirli bir örneğine sahip olan ve onu yöneten kuruluştur. Genelde bir kuruluş için Azure ve Office 365 hizmetlerinden oluşan kümeyi belirtmek üzere kullanılır.
- **Kasa sahibi**: Kasa sahibi, anahtar kasası oluşturabilir ve anahtar kasası üzerinde tam erişime ve denetime sahip olabilir. Kasa sahibi, gizli dizilere ve anahtarlara kimlerin eriştiğini günlüğe kaydetmek için belirli denetim özellikleri de ayarlayabilir. Yöneticiler anahtarların yaşam döngüsünü kontrol edebilir. Anahtarı yeni sürüme geçirebilir, yedekleyebilir ve ilgili görevleri gerçekleştirebilirler.
- **Kasa tüketicisi**: Kasa tüketicisi, kasa sahibi tarafından tüketici erişimi sağlandığında, anahtar kasasındaki varlıklar üzerinde belirli eylemler gerçekleştirebilir. Kullanılabilen eylemler verilen izinlere bağlıdır.
- **Kaynak**: Azure aracılığıyla kullanılan, yönetilebilir bir öğedir. Sanal makine, depolama hesabı, web uygulaması, veritabanı ve sanal ağ bazı yaygın kaynaklardandır, ancak çok daha fazlası mevcuttur.
- **Kaynak grubu**: Kaynak grubu, Azure çözümleri için ilgili kaynakları bir arada tutan kapsayıcılardır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz.
- **Hizmet sorumlusu** - Azure AD kiracısı tarafından korunan kaynaklar, bir güvenlik sorumlusu tarafından erişim temsil gerektiren varlık erişebilmek için. Bu, kullanıcılara (kullanıcı sorumlusu) ve (hizmet sorumlusu) uygulamaları için geçerlidir. Güvenlik sorumlusu erişim ilkesini ve kullanıcı/uygulama izinlerini bu kiracıda tanımlar. Bu, kullanıcı/uygulama kimlik doğrulaması sırasında oturum açma ve yetkilendirme sırasında kaynak erişimi gibi temel özellikleri sağlar.
- **[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md)**: Azure AD, bir kiracıya ilişkin Active Directory hizmetidir. Her dizinde bir veya daha fazla etki alanı vardır. Dizinde birden fazla abonelik bulunabilir ancak tek bir kiracı olur. 
- **Azure kiracı kimliği**: Kiracı kimliği, bir Azure aboneliğinde Azure AD örneğini tanımlamanın benzersiz bir yoludur.
- **Kimlikler Azure kaynakları için yönetilen**: Azure Key Vault kimlik bilgilerini ve diğer anahtarlar ve gizli anahtarları güvenli bir şekilde depolamak için bir yol sağlar, ancak bunları almak için anahtar Kasası'na kimlik doğrulaması kodunuzu gerekiyor. Yönetilen kimlik kullanarak Azure hizmetleri otomatik olarak yönetilen bir kimlik sağlayarak Azure AD'de basit bu sorunu çözme yapar. Bu kimliği kullanarak, Key Vault veya Azure AD kimlik doğrulamasını destekleyen tüm hizmetler için kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. Daha fazla bilgi için aşağıdaki resme bakın ve [yönetilen Azure kaynaklarına genel bakış için kimlikleri](../active-directory/managed-identities-azure-resources/overview.md).

    ![Nasıl diyagramı yönetilen Azure kaynaklarını çalıştığı için kimlikleri](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Key Vault rolleri

Geliştiricilerin ve güvenlik yöneticilerinin ihtiyaçlarını karşılamaya Anahtar Kasası'nın nasıl yardımcı olabileceğini daha iyi anlamak için aşağıdaki tabloyu kullanın.

| Rol | Sorun bildirimi | Azure Anahtar Kasası tarafından çözüldü |
| --- | --- | --- |
| Bir Azure uygulaması geliştiricisi |"İmzalama ve şifreleme için anahtarları kullanan Azure'a yönelik bir uygulama yazmak istiyorum ancak bu anahtarların benim uygulamamın dışında olmasını ve böylelikle çözümün coğrafi olarak dağıtılan bir uygulamaya uygun olmasını istiyorum. <br/><br/>Kodu kendim yazmak zorunda kalmadan bu anahtarların ve parolaların korunmasını istiyorum. Ayrıca bu anahtarları ve parolaları uygulamamdan en iyi performans ile kolayca kullanabilmek istiyorum." |√ Anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.<br/><br/> √ Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri kullanılarak Azure tarafından korunur.<br/><br/> √ Anahtarlar uygulamalarla aynı Azure veri merkezinde bulunan HSM'lerde işlenir. Bu yöntem, şirket içi gibi ayrı konumlarda bulunan anahtarlardan daha fazla güvenilirlik ve daha az gecikme sağlar. |
| Hizmet olarak yazılım (SaaS) geliştiricisi |"Müşterilerimin kiracı anahtarları ve gizli anahtarları için sorumluluk veya olası bir yükümlülük almak istemiyorum. <br/><br/>Müşterilerin kendi anahtarlarına sahip olmalarını ve bunları yönetmelerini istiyorum; böylece en iyi yaptığım şeye, yani çekirdek yazılım özelliklerini sağlamaya odaklanabilirim." |√ Müşteriler kendi anahtarları Azure'a aktarabilir ve bunları yönetebilir. Bir SaaS uygulamasının müşterilerin anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, Anahtar Kasası uygulama adına bu işlemleri yapar. Uygulama, müşterilerin anahtarlarını görmez. |
| Güvenlik başkanı (CSO) |"Uygulamalarımızın güvenli anahtar yönetimi için FIPS 140-2 Düzey 2 HSM'lerle uyumlu olduğunu bilmek istiyorum. <br/><br/>Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum. <br/><br/>Ayrıca, birden çok Azure hizmeti ve kaynağı kullanıyor olsak da, anahtarları Azure'da tek bir konumdan yönetmek istiyorum." |√ HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir.<br/><br/>√ Anahtar Kasası, Microsoft anahtarlarınızı görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.<br/><br/>√ Anahtar kullanımı neredeyse gerçek zamanlı olarak günlüğe kaydedilir.<br/><br/>√ Kasa, Azure'da kaç kasanız olduğuna, bunların hangi bölgeleri desteklediğine ve hangi uygulamaların bunları kullandığına bakmaksızın tek bir arabirim sağlar. |

Bir Azure aboneliği olan herhangi biri, anahtar kasalarını oluşturabilir ve kullanabilir. Anahtar Kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da bir kuruluş için diğer Azure hizmetlerini yöneten bir kuruluş yöneticisi tarafından uygulanabilir ve yönetilebilir. Örneğin, bu yönetici bir Azure aboneliğiyle oturum açabilir, anahtarların depolanacağı kuruluş için bir kasa oluşturur ve ardından şunlar gibi işlemsel görevlerden sorumlu olur:

* Bir anahtar veya gizli anahtarı oluşturma veya içeri aktarma
* Bir anahtar veya gizli anahtarı iptal etme veya silme
* Kullanıcıların veya uygulamaların, anahtar ve gizli anahtarları yönetmeleri veya kullanmaları için anahtar kasasına erişmelerini yetkilendirme
* Anahtar kullanımını (örneğin, imzalama veya şifreleme) yapılandırma
* Anahtar kullanımını izleme

Ardından, bu yönetici uygulamalarından çağırmaları için geliştiricilere URI'ler sağlar ve güvenlik yöneticisine anahtar kullanımı günlüğü bilgilerini sunar. 

![Azure Anahtar Kasası'na Genel Bakış][1]

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için bkz. [Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

Yöneticilere yönelik kullanmaya başlama öğreticisi için bkz. [Azure Key Vault'u kullanmaya başlama](key-vault-get-started.md).

Key Vault kullanım günlüğü hakkında daha fazla bilgi için bkz. [Azure Key Vault günlüğü](key-vault-logging.md).

Azure Key Vault ile anahtarları ve gizli anahtarları kullanma hakkında daha fazla bilgi için bkz. [Anahtarlar, parolalar ve sertifikalar hakkında](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).
