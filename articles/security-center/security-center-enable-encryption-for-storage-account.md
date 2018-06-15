---
title: Azure Güvenlik Merkezi'nde depolama hesabı için şifrelemeyi etkinleştirme | Microsoft Docs
description: Bu belgede Azure Güvenlik Merkezi önerileri uygulamak gösterilmiştir **etkinleştirmek Azure depolama hesabı için şifreleme**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: 82bb201c0b518d0b45e06a1eb25d54f60cb3e028
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30235028"
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Azure depolama hesabı için şifrelemeyi etkinleştir
Azure Güvenlik Merkezi, Azure depolama hizmeti şifrelemesi bekleyen veri için etkinleştirmenizi öneririz.

Azure depolama birimine yazıldığında verileri şifrelemek ve alma önce verilerin şifresini çözmek depolama hizmeti şifreleme (SSE) çalışır.  SSE yalnızca Azure Blob hizmeti için şu anda kullanılabilir değil ve blok blobları, sayfa bloblarını kullanılabilir ve ekleme blobları.  Daha fazla bilgi için bkz: [bekleyen veri için depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).


> [!Note]
> Şifreleme etkinleştirdikten sonra yalnızca yeni veri şifrelenir. Varolan BLOB storage hesabınızdaki şifrelenmemiş kalır. Mevcut BLOB'ları şifrelemek için bkz: [depolama hizmeti şifrelemesi SSS](../storage/common/storage-service-encryption.md#faq-for-storage-service-encryption).
>
>

Depolama hizmeti şifreleme yalnızca Resource Manager depolama hesaplarında desteklenir. Klasik depolama hesapları şu anda desteklenmemektedir. Klasik ve Resource Manager dağıtım modellerinde anlamak için bkz: [Azure dağıtım modelleri](../azure-classic-rm.md).

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belge hakkında adım adım kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **önerileri** dikey penceresinde, select **etkinleştirmek Azure depolama hesabı için şifreleme**.
   ![Depolama hesabı için şifrelemeyi etkinleştirme][1]
2. **Etkinleştirmek depolama şifreleme** dikey pencere açılır. Bu dikey burada depolama şifreleme devre dışı Azure depolama hesaplarını listeler. Bu örnekte, şimdi seçin **storageacct1**.
   ![Depolama şifrelemeyi etkinleştir][2]
3. **Şifreleme** dikey **storageacct1** açar. Seçin **etkin**.
   ![Şifreleme dikey penceresi][3]
4. **Kaydet**’i seçin.

Depolama şifrelemesi için şimdi etkinleştirdiğiniz **storageacct1**.


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede "Azure depolama hesabı için şifrelemeyi etkinleştirir." Güvenlik Merkezi öneriyi uygulamayı nasıl oluşturulacağını gösterir Azure depolama hizmeti şifrelemesi hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Rest verileri için Azure Storage hizmeti şifreleme](../storage/common/storage-service-encryption.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemek öğrenin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) -yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) -Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
