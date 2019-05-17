---
title: Bulut karşıya yükleme ve Azure Media Services ile depolama | Microsoft Docs
description: Bu makalede bulut karşıya yükleme ve depolama kavramları.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/11/2019
ms.author: juliako
ms.openlocfilehash: 9cbb995eb3310a2263185d6fd6dba20efce37f38
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550162"
---
# <a name="cloud-upload-and-storage"></a>Bulutta karşıya yükleme ve depolama

Yönetmek, şifreleme, kodlama, çözümleme ve azure'da medya içeriği akışı başlatmak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. 

Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Depolama hesapları Media Services hesabıyla aynı konumda ek gecikme süresi ve veri kullanım maliyetleri önlemek için önerilir

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. 

>[!NOTE]
> Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. 

Sık erişimli arasında seçim avantajlarından yararlanmak ve seyrek erişimli depolama katmanları, GPv2 kullanmanızı öneririz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakışın](../../storage/common/storage-account-overview.md). 

Depolama hesabınız için seçebileceğiniz farklı SKU'ları vardır. Daha fazla bilgi için [depolama hesapları](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest). Depolama hesapları ile denemek istiyorsanız, kullanın `--sku Standard_LRS`. Ancak, üretim için bir SKU seçilmesi sırasında dikkate almanız gereken, `--sku Standard_RAGRS`, coğrafi çoğaltma için iş sürekliliği sağlar. 

## <a name="assets-in-a-storage-account"></a>Varlıkları bir depolama hesabı

Media Services v3 sürümünde depolama API'leri varlıklarına dosyaları karşıya yüklemek için kullanılır. Daha fazla bilgi için [varlıklar kavramı](assets-concept.md).

> [!Note]
> Media Services API'leri kullanmadan Media Services SDK'sı tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.
 
## <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

Bekleyen veri varlıklarınızı korumanın varlıklar tarafından depolama tarafı şifrelemesi şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifrelemesi Media Services v3 sürümünde nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v3|
|---|---|---|
|Media Services'ı depolama şifrelemesi| Media Services tarafından yönetilen AES-256 şifreleme anahtarı|Desteklenmeyen<sup>(1)</sup>|
|[Bekleyen veriler için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifrelemesi, Azure Depolama tarafından sunulan anahtarı Azure tarafından veya müşteri tarafından yönetilen|Desteklenen|
|[Depolama istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar Kasası'nda müşteri tarafından yönetilen bir kiracı anahtarı tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|

<sup>1</sup> , Media Services v3 (AES-256 şifreleme) depolama şifrelemesi, yalnızca varlıklarınızı Media Services v2 ile oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

## <a name="storage-account-errors"></a>Depolama hesabı hataları

Media Services hesabı için "Bağlantı kesildi" durumu, hesap erişim için bir veya daha fazla bağlı depolama hesabını bir değişiklikten dolayı depolama erişim anahtarlarını artık sahip olduğunu gösterir. Güncel depolama erişim anahtarlarını, Media Services tarafından hesabında çok sayıda görevi gerçekleştirmek için gereklidir.

Bir Media Services hesabında bağlı depolama hesaplarına erişim olmaması ortaya çıkabilecek birincil senaryolar aşağıda verilmiştir. 

|Sorun|Çözüm|
|---|---|
|Media Services hesabı ya da bağlı depolama hesapları, abonelikleri ayırmak için geçirildi. |Tümü aynı abonelikte olmasını sağlamak ve depolama hesapları Media Services hesabına geçirin. |
|Burada destekleniyordu erken bir Media Services hesabı olduğu gibi Media Services hesabı farklı bir abonelikte bağlı depolama hesabını kullanıyor. Tüm erken Media Services hesapları modern Azure Kaynak Yöneticisi (ARM) tabanlı hesaplarına dönüştürüldü ve bağlantı kesildi durumunda olacaktır. |Tümü aynı abonelikte olmasını sağlamak, Media Services hesabı ve depolama hesabını geçirin.|

## <a name="next-steps"></a>Sonraki adımlar

Media Services hesabınızla bir depolama hesabı ekleme konusunda bilgi almak için bkz: [hesap oluşturma](create-account-cli-quickstart.md).
