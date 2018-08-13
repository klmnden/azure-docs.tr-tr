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
ms.topic: get-started-article
ms.date: 08/02/2018
ms.author: barclayn
ms.openlocfilehash: 08331a399044eba17060d15f24af1863df38caf5
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480262"
---
# <a name="what-is-azure-key-vault"></a>Azure Anahtar Kasası nedir?

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Anahtar Kasası'nı kullanarak anahtarları ve gizli anahtarları (kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları, .PFX dosyaları ve parolalar gibi), donanım güvenlik modülleri tarafından korunan anahtarları kullanarak şifreleyebilirsiniz. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz. Bunu yapmayı seçerseniz Microsoft anahtarlarınızı FIPS 140-2 Düzey 2 doğrulamasına sahip HSM'lerde (donanım ve bellenim) işler.  

Anahtar Kasası anahtar yönetimi işlemini kolaylaştırır ve verilerinize erişen ve bunları şifreleyen anahtarları denetiminizde tutmanıza olanak sağlar. Geliştiriciler, geliştirme ve test için dakikalar içinde anahtar oluşturabilir ve ardından bunları üretim anahtarlarına sorunsuz bir şekilde geçirebilir. Güvenlik yöneticileri gerektiğinde anahtarlara izin verebilir (ve iptal edebilir).

## <a name="basic-concepts"></a>Temel kavramlar

Azure Key Vault, gizli dizilerin güvenli bir şekilde depolanması ve bunlara erişim sağlanması için tasarlanmış bir araçtır. Gizli dizi; API anahtarları, parolalar veya sertifikalar gibi erişimi sıkı bir şekilde denetlemek istediğiniz tüm öğelerdir.
Aşağıda bazı önemli terimler verilmiştir:
- **Kiracı**: Kiracı, Microsoft bulut hizmetlerinin belirli bir örneğine sahip olan ve onu yöneten kuruluştur. Genelde bir kuruluş için Azure ve Office 365 hizmetlerinden oluşan kümeyi belirtmek için kullanılır
- **Kasa Sahibi**: Key Vault oluşturarak üzerinde tam erişime ve denetime sahip olabilir. Kasa sahibi gizli dizilere ve anahtarlara erişimi günlüğe kaydetmek için denetim özelliklerini de ayarlayabilir. Yöneticiler anahtarların yaşam döngüsünü kontrol edebilir. Anahtarı yeni sürüme geçirme, yedekleme gibi işlemler gerçekleştirebilir.
- **Kaynak**: Azure ile kullanılabilen yönetilebilir bir öğedir. Sanal makine, depolama hesabı, web uygulaması, veritabanı ve sanal ağ bazı yaygın kaynaklardandır, ancak çok daha fazlası mevcuttur.
- **Kaynak grubu**: Bir Azure çözümü için ilgili kaynakları bir arada tutan bir kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Bkz. Kaynak grupları.
- **Kasa Tüketicisi**: Key Vault sahibinin kendisine verdiği izinlere bağlı olarak varlıklar üzerinde eylem gerçekleştirebilir.
- **[Azure Active Directory](../active-directory/active-directory-whatis.md)**, kiracının Azure AD hizmetidir. Her dizinde bir veya daha fazla etki alanı vardır. Dizinde birden fazla abonelik bulunabilir ancak tek bir kiracı olur. 
- **Azure Kiracı Kimliği**: Bir Azure Aboneliği içindeki Azure Active Directory'nin benzersiz bir şekilde tanımlanmasını sağlar. 
- **Yönetilen Hizmet Kimliği**: Azure Key Vault kimlik bilgilerini ve diğer anahtarlarla gizli dizileri güvenle depolamak için bir yol sağlar, ama bunları alabilmek için kodunuzun Key Vault'ta kimlik doğrulaması yapması gerekir. Yönetilen Hizmet Kimliği (MSI), Azure hizmetlerine Azure Active Directory (Azure AD) üzerinde otomatik olarak yönetilen bir kimlik vererek bu soruna daha basit bir çözüm getirir. Bu kimliği kullanarak, Key Vault veya Azure AD kimlik doğrulamasını destekleyen tüm hizmetler için kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz. MSI hakkında daha fazla bilgiye [buradan](../active-directory/managed-service-identity/overview.md) ulaşabilirsiniz

    ![MSI grafiği](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Key Vault rolleri

Geliştiricilerin ve güvenlik yöneticilerinin ihtiyaçlarını karşılamaya Anahtar Kasası'nın nasıl yardımcı olabileceğini daha iyi anlamak için aşağıdaki tabloyu kullanın.

| Rol | Sorun bildirimi | Azure Anahtar Kasası tarafından çözüldü |
| --- | --- | --- |
| Bir Azure uygulaması geliştiricisi |"İmzalama ve şifreleme için anahtarları kullanan Azure'a yönelik bir uygulama yazmak istiyorum ancak bu anahtarların benim uygulamamın dışında olmasını ve böylelikle çözümün coğrafi olarak dağıtılan bir uygulamaya uygun olmasını istiyorum. <br/><br/>Ayrıca, kodu kendim yazmak zorunda kalmadan bu anahtarların ve parolaların korunmasını istiyorum. Ayrıca bu anahtarları ve parolaları uygulamamdan en iyi performans ile kolayca kullanabilmek istiyorum." |√ Anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.<br/><br/> √ Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri (HSM'ler) kullanılarak Azure tarafından korunur.<br/><br/> √ Anahtarlar uygulamalarla aynı Azure veri merkezinde bulunan HSM'lerde işlenir. Bunun yapılması, anahtarlar şirket içi gibi ayrı konumlarda bulunuyorsa daha fazla güvenilirlik ve daha az gecikme sağlar. |
| Hizmet olarak Yazılım (SaaS) geliştiricisi |"Müşterilerimin kiracı anahtarları ve gizli anahtarları için sorumluluk veya olası bir yükümlülük almak istemiyorum. <br/><br/>Müşterilerin kendi anahtarlarına sahip olmalarını ve bunları yönetmelerini istiyorum; böylece en iyi yaptığım şeye, yani çekirdek yazılım özelliklerini sağlamaya odaklanabilirim." |√ Müşteriler kendi anahtarları Azure'a aktarabilir ve bunları yönetebilir. Bir SaaS uygulamasının müşterilerin anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, Anahtar Kasası uygulama adına bu işlemleri yapar. Uygulama, müşterilerin anahtarlarını görmez. |
| Güvenlik başkanı (CSO) |"Uygulamalarımızın güvenli anahtar yönetimi için FIPS 140-2 Düzey 2 HSM'lerle uyumlu olduğunu bilmek istiyorum. <br/><br/>Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum. <br/><br/>Ayrıca, birden çok Azure hizmeti ve kaynağı kullanıyor olsak da, anahtarları Azure'da tek bir konumdan yönetmek istiyorum." |√ HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir.<br/><br/>√ Anahtar Kasası, Microsoft anahtarlarınızı görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.<br/><br/>√ Gerçek zamanlıya yakın anahtar kullanımı günlüğü.<br/><br/>√ Kasa, Azure'da kaç kasanız olduğuna, bunların hangi bölgeleri desteklediğine ve hangi uygulamaların bunları kullandığına bakmaksızın tek bir arabirim sağlar. |

Bir Azure aboneliği olan herhangi biri, anahtar kasalarını oluşturabilir ve kullanabilir. Anahtar Kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da, bir kuruluş için diğer Azure hizmetlerini yöneten bir kuruluş yöneticisi tarafından uygulanabilir ve yönetilebilir. Örneğin, bu yönetici bir Azure aboneliğiyle oturum açabilir, anahtarların depolanacağı kuruluş için bir kasa oluşturur ve ardından şunlar gibi işlemsel görevlerden sorumlu olur:

* Bir anahtar veya gizli anahtarı oluşturma veya içeri aktarma
* Bir anahtar veya gizli anahtarı iptal etme veya silme
* Kullanıcıların veya uygulamaların, anahtar ve gizli anahtarları yönetmeleri veya kullanmaları için anahtar kasasına erişmelerini yetkilendirme
* Anahtar kullanımını (örneğin, imzalama veya şifreleme) yapılandırma
* Anahtar kullanımını izleme

Ardından, bu yönetici uygulamalarından çağırmaları için geliştiricilere URI'ler sağlar ve güvenlik yöneticisine anahtar kullanımı günlüğü bilgilerini sunar. 

   ![Azure Anahtar Kasası'na Genel Bakış][1]

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için bkz. [Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir yöneticiye yönelik başlama öğreticisi için bkz. [Azure Anahtar Kasası ile çalışmaya başlama](key-vault-get-started.md).

Anahtar Kasası'na yönelik kullanım günlüğü hakkında daha fazla bilgi için bkz. [Azure Anahtar Kasası Günlüğü](key-vault-logging.md).

Azure Anahtar Kasası ile anahtarları ve gizli anahtarları kullanma hakkında daha fazla bilgi için bkz. [Anahtarlar, Parolalar ve Sertifikalar Hakkında](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).
