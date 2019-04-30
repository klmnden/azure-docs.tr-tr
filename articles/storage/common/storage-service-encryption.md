---
title: Bekleyen veri için Azure depolama şifrelemesi | Microsoft Docs
description: Azure depolama, verilerinizi otomatik olarak şifreleyerek korur önce buluta kalıcı. Tüm veriler Azure depolama blobları, diskler, dosyalar, kuyruklar veya tablolar, olup olmadığını şifrelenir ve şeffaf bir şekilde 256 bit AES şifrelemesi kullanılarak şifresi ve FIPS 140-2 uyumludur.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 211cfeb3aba29245e154f4a7db86fb4a3659c36f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730846"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Bekleyen veri için Azure depolama şifrelemesi

Azure depolama, verilerinizi buluta kalıcı olduğunda otomatik olarak şifreler. Şifreleme verilerinizi korur ve organizasyonel güvenlik ve uyumluluk yükümlülüklerinizin yerine yardımcı olacak. Azure Depolama'daki verilere şifrelenir ve 256 bit şeffaf bir şekilde kullanarak şifresi [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), bir en güçlü blok şifreleme kullanılabilir ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi, Windows BitLocker şifrelemesini benzerdir.

Azure depolama şifrelemesi için tüm yeni ve var olan depolama hesapları etkinleştirilir ve devre dışı bırakılamaz. Verilerinizi varsayılan olarak korumalı olduğundan, kod veya Azure depolama şifrelemeden yararlanmak için uygulamaları değişiklik gerekmez. Depolama hesapları, kendi performans katmanını (standart veya premium) veya (Azure Resource Manager veya Klasik) dağıtım modeli bağımsız olarak şifrelenir. Tüm Azure depolama yedekliliği seçenekleri, şifreleme, destek ve tüm kopyalarını bir depolama hesabı şifrelenmiş. Blobları, diskleri, dosyaları, kuyruklar ve tablolar dahil olmak üzere tüm Azure depolama kaynaklarını şifrelenir.

Şifreleme, Azure depolama performansını etkilemez. Azure depolama şifrelemesi için ek ücret yoktur.

Şifreleme modüllerini temel alınan Azure depolama şifrelemesi hakkında daha fazla bilgi için bkz. [şifreleme API'si: Yeni nesil](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="key-management"></a>Anahtar yönetimi

Microsoft tarafından yönetilen anahtarlar için depolama hesabınızın şifrelenmesini güvenebilirsiniz veya Azure anahtar kasası ile birlikte kendi anahtarlarla şifreleme yönetebilirsiniz.

### <a name="microsoft-managed-keys"></a>Microsoft tarafından yönetilen anahtarlar

Varsayılan olarak, şifreleme Microsoft tarafından yönetilen anahtarları, depolama hesabını kullanır. Şifreleme ayarları için depolama hesabınızda gördüğünüz **şifreleme** bölümünü [Azure portalında](https://portal.azure.com), aşağıdaki görüntüde gösterildiği gibi.

![Microsoft tarafından yönetilen anahtarlarla şifrelenir hesabı görüntüle](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

### <a name="customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar

Azure depolama şifrelemesi müşteri tarafından yönetilen anahtarlarla yönetebilirsiniz. Müşteri tarafından yönetilen anahtarlar, oluşturma, döndürme, devre dışı bırakın ve erişim denetimleri iptal daha fazla esneklik sağlar. Verilerinizi korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz. 

Azure Key Vault, anahtarlarınızı yönetmek ve anahtar kullanımınızı denetlemek için kullanın. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında depolama veya Azure anahtar kasası API'leri, anahtarlar oluşturmak için kullanabilirsiniz. Depolama hesabı ve anahtar kasasının aynı bölgede olması gerekir, ancak bunlar farklı Aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure anahtar kasası nedir?](../../key-vault/key-vault-overview.md).

Müşteri tarafından yönetilen anahtarlar erişimi iptal etmek için bkz: [Azure anahtar kasası PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) ve [Azure anahtar kasası CLI](https://docs.microsoft.com/cli/azure/keyvault). Erişimi etkili bir şekilde iptal ediliyor, şifreleme anahtarı Azure Depolama tarafından erişilemez olduğundan, depolama hesabındaki tüm verilere erişimi engeller.

Müşteri tarafından yönetilen anahtarlar Azure depolama ile kullanma konusunda bilgi almak için şu makalelerden birine bakın:

- [Müşteri tarafından yönetilen anahtarlar Azure portalından Azure depolama şifrelemesi için yapılandırma](storage-encryption-keys-portal.md)
- [Müşteri tarafından yönetilen anahtarlar powershell'den Azure depolama şifrelemesi için yapılandırma](storage-encryption-keys-powershell.md)
- [Azure clı'dan Azure depolama şifreleme ile müşteri tarafından yönetilen anahtarlar kullan](storage-encryption-keys-cli.md)

> [!NOTE]  
> Müşteri tarafından yönetilen anahtarlar için desteklenmez [Azure yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Disk şifreleme ve Azure depolama şifrelemesi

Azure depolama şifreleme ile tüm Azure depolama hesapları ve içerdikleri kaynaklara, Azure sanal makine diskleri sayfa blobları dahil olmak üzere şifrelenir. Ayrıca, Azure sanal makine diskleri ile şifrelenmelidir [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption-overview.md). Azure Disk şifrelemesi, endüstri standardı kullanan [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) Windows üzerinde ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Azure Key Vault ile tümleşik olan işletim sistemi tabanlı şifreleme çözümleri sağlamak için Linux üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure anahtar kasası nedir?](../../key-vault/key-vault-overview.md)
