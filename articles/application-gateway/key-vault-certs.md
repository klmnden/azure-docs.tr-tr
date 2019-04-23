---
title: Key Vault sertifikalar ile SSL sonlandırma
description: Azure uygulama ağ geçidi ile Key Vault etkin HTTPS dinleyicileri için bağlı sunucu sertifikaları için nasıl tümleştirilebileceğini öğrenmek.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/19/2019
ms.author: victorh
ms.openlocfilehash: 9ab1a42578081d4e6537f221e3f3a8f4c7babde0
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014691"
---
# <a name="ssl-termination-with-key-vault-certificates"></a>Key Vault sertifikalar ile SSL sonlandırma

[Azure Key Vault](../key-vault/key-vault-whatis.md) gizli dizileri, anahtarlar ve SSL sertifikalarınızı korumak için kullanabileceğiniz bir platform yönetilen gizli deposudur. Application Gateway, etkin HTTPS dinleyicileri için bağlı sunucu sertifikaları için anahtar kasası ile tümleştirme (genel önizlemede) destekler. Bu destek, Application Gateway için v2 SKU sınırlıdır.

> [!IMPORTANT]
> Uygulama ağ geçidi Key Vault tümleştirmesi şu anda genel Önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu genel Önizleme ile SSL sonlandırma için iki modeli vardır:

- SSL sertifikaları için dinleyici bağlı açıkça sağlayabilirsiniz. Bu, SSL sertifikaları, SSL sonlandırma için uygulama ağ geçidine geçirme geleneksel modelidir.
- İsteğe bağlı olarak var olan bir anahtar kasası sertifikası başvuru sağlayabilir veya gizli anahtarı sırasında HTTPS dinleyicisi oluşturma etkin.

Key Vault tümleştirmesi, çok sayıda avantaj vardır dahil olmak üzere:

- SSL sertifikaları ve uygulama geliştirme ekibi tarafından doğrudan işlenmemiş olduğundan daha güçlü güvenlik. Key Vault ile tümleştirme sağlamak, yaşam döngüsünü denetleyen ve uygulama ağ geçitleri erişim sertifikaları Key Vault'ta depolanan seçmek için uygun izin vermek ayrı güvenlik takım sağlar.
- Key Vault'a mevcut sertifikaları almak için destek veya oluşturup yeni sertifikalar herhangi bir güvenilir Key Vault iş ortakları ile yönetmek için anahtar kasası API'lerini kullanın.
- Otomatik olarak yenilemek için anahtar Kasası'nda depolanan sertifikaları için destek.

Application Gateway şu anda yalnızca doğrulanmış yazılım sertifikalarını destekler. Donanım Güvenlik Modülü (HSM) doğrulandı sertifikaların desteklenmez. Key Vault sertifikalar kullanmak üzere uygulama ağ geçidi yapılandırıldıktan sonra kendi örnekleri sertifika anahtar Kasasından almak ve SSL sonlandırma için yerel olarak yükleyin. Varsa örnekleri Key Vault sertifikası yenilenmiş bir sürümünü almak için bir 24 saatlik aralıkta da düzenli aralıklarla yoklar. Güncelleştirilmiş bir sertifika bulunursa, şu anda HTTPS dinleyicisi ile ilişkili bir SSL sertifikası otomatik olarak döndürülür.

## <a name="how-it-works"></a>Nasıl çalışır?

Key Vault ile tümleştirme, üç adımlı yapılandırma işlemi gerektirir:

1. **Kullanıcı tarafından atanan yönetilen kimliği oluşturma**

   Oturum açma oluşturmalı veya sertifikaları Key Vault'tan sizin adınıza almak için Application Gateway kullanan yönetilen kimlik atanmış mevcut bir kullanıcının yeniden kullanın. Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir?](../active-directory/managed-identities-azure-resources/overview.md) Bu adımda, kimliğini oluşturmak için kullanılan abonelik tarafından güvenilen Azure AD kiracısında yeni bir kimlik oluşturulur.
1. **Key Vault'u Yapılandır**

   Key vault'ta uygulama ağ geçidi üzerinden çalışan uygulamalar tarafından kullanılan yeni bir sertifika oluşturun veya içe ardından gerekir. Parola olmadan taban 64 kodlanmış PFX dosyası, bu adımda kullanılabilir olarak depolanan bir Key Vault gizli anahtarı. Bir sertifika türü kullanarak anahtar Kasası'nda sertifika türü nesneleri ile sunulan otomatik yenileme özellikleri nedeniyle tercih edilir. Verilecek kimlik izin vermek için anahtar Kasası'nda bir sertifika veya gizli dizi oluşturulduktan sonra erişim ilkeleri tanımlanmalıdır *alma* erişim gizli anahtarı getirilemedi.

1. **Uygulama ağ geçidi yapılandırma**

   Önceki iki adımı tamamladıktan sonra sağlama veya kullanıcı tarafından atanan yönetilen kimliği kullanmak için mevcut bir Application Gateway'i değiştirin. Ayrıca tam URI, Key Vault'un sertifika veya gizli kimliği işaret edecek şekilde dinleyicinin HTTP SSL sertifikası yapılandırma

![Key Vault sertifikaları](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure PowerShell kullanarak Key Vault sertifikaları ile SSL sonlandırma yapılandırma](configure-keyvault-ps.md).