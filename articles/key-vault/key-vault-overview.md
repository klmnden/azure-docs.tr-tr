---
title: Azure Key Vault’a Genel Bakış | Microsoft Docs
description: Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 34af20ee-3fa7-4f28-9d98-6168b1759764
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 07/17/2018
ms.author: barclayn
ms.openlocfilehash: aae7836448ff27b4c80d7bb53e108034ee52db1c
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586300"
---
# <a name="what-is-azure-key-vault"></a>Azure Anahtar Kasası nedir?

Azure Key Vault, aşağıdaki sorunların çözülmesine yardımcı olur
- **Gizli Dizi Yönetimi**: Belirteçleri, parolaları, sertifikaları, API anahtarlarını ve diğer gizli dizileri Güvenle depolamak ve bunlara erişimi sıkı bir şekilde denetlemek için Azure Key Vault kullanılabilir.
- **Anahtar Yönetimi**: Azure Key Vault, Anahtar Yönetimi çözümü olarak da kullanılabilir. Azure Key Vault, verilerinizi şifrelemek için kullanılan şifreleme anahtarlarını oluşturmayı ve denetlemeyi kolaylaştırır. 
- **Sertifika Yönetimi**: Azure Key Vault aynı zamanda, Azure ve bağlı iç kaynaklarınızla kullanım için genel ve özel Güvenli Yuva Katmanı/Aktarım Katmanı Güvenliği (SSL/TLS) sertifikalarını kolayca hazırlamanıza, yönetmenize ve dağıtmanıza olanak sağlayan bir hizmettir. 
- **Gizli dizileri Donanım Güvenlik Modülleri desteğiyle saklayın**: Gizli diziler ve anahtarlar yazılım tarafından korunabilir veya FIPS 140-2 Düzey 2, HSM'leri doğrular

## <a name="why-use-azure-key-vault"></a>Neden Azure Key Vault kullanmalıyım?

### <a name="centralize-application-secrets"></a>Uygulama gizli dizilerini merkezi hale getirme

Azure Key Vault’ta uygulama gizli dizilerinin depolanmasını merkezi hale getirerek dağılımlarını denetleyebilirsiniz. Key Vault, gizli dizilerin yanlışlıkla sızdırılma olasılığını büyük oranda azaltır. Key Vault kullanırken, uygulama geliştiricilerinin güvenlik bilgilerini uygulamalarında depolaması artık gerekli değildir. Bunun yapılması, bu bilgileri kodun bir parçası yapma gereksinimini ortadan kaldırır. Örneğin, bir uygulamanın bir veritabanına bağlanması gerekebilir. Bağlantı dizesini uygulama kodlarında depolamak yerine Key Vault’ta güvenli bir şekilde depolayın.

Uygulamalarınız, uygulamanın anahtarı veya gizli dizisi Azure Key Vault’ta depolandıktan sonra gizli dizinin belirli sürümlerini almanıza olanak tanıyan URI’ler kullanarak gerekli bilgilere güvenli bir şekilde erişebilirler. Bu, herhangi bir gizli bilgiyi korumak için özel kod yazmak zorunda kalmadan gerçekleşir.

### <a name="securely-store-secrets-and-keys"></a>Gizli dizileri ve anahtarları güvenle depolama

Gizli diziler ve anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri (HSM'ler) kullanılarak Azure tarafından korunur. Kullanılan HSM’ler Federal Bilgi İşleme Standartları (FIPS) 140-2 Düzey 2 ile doğrulanmıştır.

Bir anahtar kasasına erişim, bir çağıranın (kullanıcı ya da uygulama) erişim elde edebilmesi için uygun kimlik doğrulaması ve yetkilendirme gerektirir. Kimlik doğrulama işlemi çağıranın kimliğini, yetkilendirme işlemi ise çağıranın yapmaya izinli olduğu işlemleri belirler.

Kimlik doğrulaması Azure Active Directory aracılığıyla yapılır. Yetkilendirme, rol tabanlı erişim denetimi (RBAC) veya Key Vault erişim ilkesi aracılığıyla yapılabilir. Kasaların yönetimi gerçekleştirilirken RBAC, bir kasada depolanmış verilere erişmeye çalışırken ise anahtar kasası erişim ilkesi kullanılır.

Azure Anahtar Kasaları yazılım veya donanım HSM korumalı olabilir. Ek güvence gereken durumlar için HSM sınırını asla terk etmeyen donanım güvenlik modüllerinde (HSM'ler) anahtarları içeri aktarabilir veya oluşturabilirsiniz. Microsoft, Thales donanım güvenlik modüllerini kullanır. Bir anahtarı HSM'nizden Azure Vault’a taşımak için Thales araçlarını kullanabilirsiniz.

Son olarak Azure Key Vault, Microsoft verilerinizi görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.

### <a name="monitor-access-and-use"></a>Erişimi ve kullanımı izleme

Birkaç Key Vault oluşturduktan sonra anahtarlarınıza ve gizli dizilerinize nasıl ve ne zaman erişildiğini izlemek istersiniz. Key Vault için günlüğe kaydetmeyi etkinleştirerek bunu yapabilirsiniz. Azure Key Vault’u aşağıdaki işlemleri yapacak şekilde yapılandırabilirsiniz:

- Bir depolama hesabına arşivleme.
- Bir olay hub'ına akış yapma.
- Günlükleri Log Analytics’e gönderme.

Günlükleriniz üzerinde denetime sahip olursunuz ve erişimi kısıtlayarak günlükleri güvenli hale getirebilir ve artık gerekli olmayan günlükleri de silebilirsiniz.

### <a name="simplified-administration-of-application-secrets"></a>Uygulama gizli dizilerinin basitleştirilmiş yönetimi

Değerli verileri depolarken birkaç adım uygulamanız gerekir. Güvenlik bilgileri güvenli hale getirilmeli, bir yaşam döngüsüne uymalı ve yüksek oranda kullanılabilir olmalıdır. Azure Key Vault aşağıdakileri yaparak bunların büyük bölümünü basitleştirir:

- Şirket içi Donanım Güvenlik Modüllerine yönelik gereksinimi ortadan kaldırma
- Kuruluşunuzun ani kullanım artışlarını karşılamak için kısa süre içinde ölçek artırma.
- Bir bölge içindeki Anahtar Kasanızın içeriklerini ikincil bir bölgeye çoğaltma. Bunun yapılması yüksek kullanılabilirlik sağlar ve yük devretmeyi tetiklemek için yöneticinin herhangi bir işlem yapma gereksinimini ortadan kaldırır.
- Portal, Azure CLI ve PowerShell aracılığıyla standart Azure yönetim seçeneklerini sağlama.
- Genel CA’lardan satın aldığınız sertifikalarla ilgili kaydetme ve yenileme gibi belirli görevleri otomatikleştirme.

Ayrıca, Azure Anahtar Kasalarını kullanarak uygulama gizli dizilerini ayırabilirsiniz. Uygulamalar yalnızca erişmelerine izin verilen kasaya erişebilir ve yalnızca belirli işlemleri gerçekleştirecek şekilde sınırlandırılabilirler. Uygulama başına bir Azure Key Vault oluşturabilir ve bir Key Vault’ta depolanmış gizli dizileri belirli bir uygulama ve geliştirici takımı ile kısıtlayabilirsiniz.

### <a name="integrate-with-other-azure-services"></a>Diğer Azure hizmetleri ile tümleştirme

Azure’da güvenli bir depo olarak Key Vault, [Azure Disk Şifrelemesi](../security/azure-security-disk-encryption.md), SQL sunucusu ve Azure SQL Veritabanı'ndaki [her zaman şifrelenmiş]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) işlevselliği, [Azure web uygulamaları]( https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site) gibi senaryoları kolaylaştırmak için kullanılmıştır. Key Vault, depolama hesapları, olay hub’ları ve günlük analizi ile tümleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: CLI kullanarak bir Azure Key Vault oluşturma](quick-create-cli.md)
- [Anahtar Kasasından gizli dizi okumak için bir Azure web uygulaması yapılandırma](tutorial-web-application-keyvault.md)
