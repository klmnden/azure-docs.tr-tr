---
title: Verileri Azure Stack'te bekleyen şifreleme
description: Bilgi edinmek için nasıl Azure Stack, bekleyen veri şifreleme ile verilerinizi koruma
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 12/06/2018
ms.author: patricka
ms.reviewer: fiseraci
keywords: ''
ms.openlocfilehash: f9e76b255647f62b273fef8336ed845e365261cf
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731719"
---
# <a name="data-at-rest-encryption-in-azure-stack"></a>Verileri Azure Stack'te bekleyen şifreleme

Azure Stack, kullanıcı ve bekleme durumunda kullanılan şifreleme depolama alt sistemi düzeyinde altyapı verileri korur. Azure yığını'nın depolama alt sistemi, BitLocker'ı kullanarak 128 bit AES şifreleme ile şifrelenir. BitLocker anahtarları, iç bir gizli dizi deposu kalır.

Bekleyen şifreleme verileri, birçok önemli uyumluluk standardı (PCI-DSS, FedRAMP, HIPAA gibi) için ortak bir gereksinimdir. Azure Stack, hiçbir ek iş ya da gerekli yapılandırmaları ile bu gereksinimleri karşılamanıza olanak sağlar. Azure Stack, uyumluluk standartlarını karşılamak için bkz: nasıl yardımcı olduğunu hakkında daha fazla bilgi için [Microsoft hizmet güveni portalı](https://aka.ms/AzureStackCompliance).

> [!NOTE]
> Bekleyen şifreleme verileri, bir veya daha fazla sabit sürücüleri fiziksel olarak çalması kişi tarafından erişilen karşı verilerinizi korur. Bekleyen şifreleme veri, sistem ayarlama modundayken exfiltrated olmasına ve çalışan veri ağı (Taşınmakta olan veriler), şu anda kullanılan verileri (bellekte) veya daha fazla üzerinden genel olarak, geçirilmesinin veri karşı korumaz.

## <a name="retrieving-bitlocker-recovery-keys"></a>BitLocker kurtarma anahtarları alma

Azure Stack BitLocker anahtarlarını bekleyen veriler için dahili olarak yönetilir. Normal işlemler için veya sistem başlatma sırasında sağlamak için gerekli değildir. Ancak, destek senaryoları sistem çevrimiçi duruma getirmek için BitLocker kurtarma anahtarlarını gerektirebilir.  

> [!WARNING]
> BitLocker kurtarma anahtarlarınızı almak ve bunları Azure Stack dışında güvenli bir konumda depolayın. Kurtarma anahtarlarını sırasında belirli destek senaryoları olmaması, veri kaybına neden ve bir yedek görüntüsünden sistem geri yükleme gerektirir.

BitLocker kurtarma anahtarlarını alma erişmesi gerekiyor [ayrıcalıklı uç nokta](azure-stack-privileged-endpoint.md) (CESARETLENDİRİCİ). Get-AzsRecoveryKeys cmdlet'i bir CESARETLENDİRİCİ oturumundan çalıştırın.

```powershell
##This cmdlet retrieves the recovery keys for all the volumes that are encrypted with BitLocker.
Get-AzsRecoveryKeys
```

İsteğe bağlı parametreler için *Get-AzsRecoveryKeys* cmdlet:

| Parametre | Açıklama | Tür | Gereklidir |
|---------|---------|---------|---------|
|*Ham* | Kurtarma anahtarı, bilgisayar adı ve parola kimlikleri şifrelenmiş her birimin arasında eşleme öğesinin ham verisine döndürür  | anahtarı | yok (tasarlanmıştır) için destek senaryoları|


## <a name="troubleshoot-issues"></a>Sorunları giderme

Aşırı durumlarda, bir BitLocker kilidini isteği önyükleme için belirli bir birimde kaynaklanan başarısız. BitLocker kurtarma anahtarlarınızı yoksa, bazı mimarisinin bileşenleri kullanılabilirliğine bağlı olarak, bu hata kapalı kalma süresi ve olası veri kaybına neden olabilir.

> [!WARNING]
> BitLocker kurtarma anahtarlarınızı almak ve bunları Azure Stack dışında güvenli bir konumda depolayın. Kurtarma anahtarlarını sırasında belirli destek senaryoları olmaması, veri kaybına neden ve bir yedek görüntüsünden sistem geri yükleme gerektirir.

Sisteminizi BitLocker'ı başlatmak, başarısız olan Azure Stack gibi sorunlarla karşılaşıyor şüpheleriniz varsa desteğe başvurun. BitLocker kurtarma anahtarlarınızı desteği gerektirir. BitLocker'ın büyük çoğunluğu, sorunları, belirli VM/konak/birim için bir FRU işlemle çözümlenebilir ilgili. Diğer durumlarda, BitLocker kurtarma anahtarlarını kullanarak el ile kilit açma yordam gerçekleştirilemez. BitLocker kurtarma anahtarları mevcut değilse yalnızca seçeneğin Yedekleme görüntüsü geri yüklemektir. Ne zaman en son yedekleme gerçekleştirilip gerçekleştirilmediğine bağlı olarak, veri kaybı karşılaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md)
- BitLocker'ın Csv'leri nasıl koruduğu hakkında daha fazla bilgi için bkz. [kümesini koruyan birimler ve depolama alanı ağlarını BitLocker'la paylaşılan](https://docs.microsoft.com/windows/security/information-protection/bitlocker/protecting-cluster-shared-volumes-and-storage-area-networks-with-bitlocker).