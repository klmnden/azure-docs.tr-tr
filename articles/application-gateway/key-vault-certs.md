---
title: Azure Key Vault sertifikalar ile SSL sonlandırma
description: Azure Application Gateway ile Key Vault için HTTPS etkin dinleyiciler için bağlı sunucu sertifikaları nasıl tümleştirilebileceğini öğrenmek.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: victorh
ms.openlocfilehash: 18af315c58c838a7237acfbcc32f622a0edbd3b3
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827638"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>Key Vault sertifikalar ile SSL sonlandırma

[Azure Key Vault](../key-vault/key-vault-whatis.md) platform yönetilen gizli dizi depoladığı gizli dizileri, anahtarlar ve SSL sertifikalarınızı korumak için kullanabilirsiniz. Azure Application Gateway, HTTPS etkin dinleyiciler için bağlı sunucu sertifikaları için (genel önizlemede) anahtar kasası ile tümleştirmeyi destekler. Bu destek, Application Gateway için v2 SKU sınırlıdır.

> [!IMPORTANT]
> Application Gateway ile Key Vault tümleştirmesi şu anda genel Önizleme aşamasındadır. Bu önizleme sürümü, hizmet düzeyi sözleşmesi (SLA) sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu genel Önizleme, SSL sonlandırma için iki modeli sunar:

- SSL sertifikaları için dinleyici bağlı açıkça sağlayabilirsiniz. Bu model, SSL sertifikaları, SSL sonlandırma için uygulama ağ geçidine geçirmek için geleneksel yöntemidir.
- Bir HTTPS etkin dinleyici oluşturduğunuzda, isteğe bağlı olarak var olan bir anahtar kasası sertifikası veya gizli bir başvuru sağlayabilirsiniz.

Key Vault ile uygulama ağ geçidi tümleştirmesi gibi birçok avantaj sunar:

- Daha güçlü güvenlik, SSL sertifikaları ve uygulama geliştirme ekibi tarafından doğrudan işlenmeyen olduğundan. Ayrı güvenlik ekibine tümleştirmesi sağlar:
  * Uygulama ağ geçitleri ayarlarsınız ayarlayın.
  * Uygulama ağ geçidi yaşam döngülerini denetler.
  * Anahtar kasanızda depolanan sertifikaları erişmek için seçili uygulama ağ geçitleri için izinler verir.
- Anahtar kasanızı mevcut sertifikaları içeri aktarmaya yönelik destek. Veya oluşturup yeni sertifikalar herhangi bir güvenilir Key Vault iş ortakları ile yönetmek için anahtar kasası API'lerini kullanın.
- Anahtar kasanızda depolanan sertifikaları otomatik olarak yenilenmesi için destek.

Application Gateway şu anda yalnızca yazılım doğrulandı sertifikaların destekler. Donanım Güvenlik Modülü (HSM)-doğrulanmış sertifikaları desteklenmez. Key Vault sertifikalar kullanmak üzere uygulama ağ geçidi yapılandırıldıktan sonra örneklerini sertifika anahtar Kasasından almak ve SSL sonlandırma için yerel olarak yükleyin. Varsa örnekleri Key Vault ayrıca yenilenmiş bir sertifika sürümünü almak için 24 saatlik aralıklarla yoklar. Güncelleştirilmiş bir sertifika bulunursa, şu anda HTTPS dinleyicisi ile ilişkili bir SSL sertifikası otomatik olarak döndürülür.

## <a name="how-integration-works"></a>Tümleştirme nasıl çalışır?

Key Vault ile uygulama ağ geçidi tümleştirme üç adımlı yapılandırma işlemi gerektirir:

1. **Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma**

   Oluşturduğunuzda veya sertifikaları Key Vault'tan sizin adınıza almak için uygulama ağ geçidini kullanan bir mevcut kullanıcı tarafından atanan yönetilen kimlik, yeniden kullanın. Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir?](../active-directory/managed-identities-azure-resources/overview.md). Bu adımda, Azure Active Directory kiracısında yeni bir kimlik oluşturulur. Kimlik, kimliği oluşturmak için kullanılan abonelik tarafından güvenilir.

1. **Anahtar kasanızı yapılandırın**

   Ardından var olan bir sertifikayı içeri aktarmak ya da, anahtar Kasası'nda yeni bir tane oluşturun. Sertifika uygulama ağ geçidi üzerinden çalışan uygulamalar tarafından kullanılır. Bu adımda, bir parola olmadan, taban 64 ile kodlanmış PFX dosyası olarak depolanan bir anahtar kasası gizli dizi de kullanabilirsiniz. Anahtar Kasası'nda sertifika türü nesneleri ile kullanılabilir otomatik yenileme özelliğini nedeniyle bir sertifika türü kullanmanızı öneririz. Bir sertifika veya bir gizli dizi oluşturduktan sonra kimlik verilecek izin vermek için anahtar kasası erişim ilkeleri tanımlar *alma* erişim gizli anahtarı.

1. **Uygulama ağ geçidi yapılandırma**

   İki yukarıdaki adımları tamamladıktan sonra ayarlayın veya kullanıcı tarafından atanan bir yönetilen kimlik kullanmak için mevcut bir application Gateway'i değiştirin. Anahtar kasası sertifikası veya gizli kimliği tam bir URI işaret edecek şekilde dinleyicinin HTTP SSL sertifikası da yapılandırabilirsiniz

   ![Anahtar kasası sertifikaları](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure PowerShell kullanarak SSL sonlandırma Key Vault ile sertifikaları yapılandırma](configure-keyvault-ps.md)
