---
title: Azure Güvenlik Merkezi'nde depolama hesabı için şifrelemeyi etkinleştirme | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerilerinin uygulanması gösterilmektedir **Azure depolama hesabı için şifrelemeyi etkinleştirme**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: 3765d150c63515337be13d821dce51944eae4655
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298277"
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki Azure depolama hesabı için şifrelemeyi etkinleştirme
Azure Güvenlik Merkezi, bekleyen veri için Azure depolama hizmeti şifrelemesi etkinleştirmenizi önerebilir.

Azure depolama birimine yazıldığında, verileri şifrelemek ve alma önce verilerin şifresini çözme depolama hizmeti şifrelemesi (SSE) çalışır.  SSE, şu anda yalnızca Azure Blob hizmeti için kullanılabilir ve blok blobları, sayfa blobları için kullanılabilir ve ekleme blobları.  Daha fazla bilgi için bkz. [bekleyen veriler için depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).


> [!Note]
> Şifreleme etkinleştirdikten sonra yalnızca yeni veriler şifrelenir. Var olan BLOB Depolama hesabınızda şifrelenmemiş olarak kalır. Mevcut blobları şifrelemek için bkz: [depolama hizmeti şifrelemesi hakkında SSS](../storage/common/storage-service-encryption.md#faq-for-storage-service-encryption).
>
>

Depolama hizmeti şifrelemesi yalnızca Resource Manager depolama hesaplarında desteklenir. Klasik depolama hesapları şu anda desteklenmemektedir. Klasik ve Resource Manager dağıtım modelleri anlamak için bkz [Azure dağıtım modelleri](../azure-classic-rm.md).

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belgede, adım adım bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **önerileri** dikey penceresinde **Azure depolama hesabı için şifrelemeyi etkinleştirme**.
   ![Depolama hesabı için şifrelemeyi etkinleştirme][1]
2. **Depolama şifrelemesini etkinleştir** dikey penceresi açılır. Bu dikey pencere, burada depolama şifreleme devre dışı Azure depolama hesaplarını listeler. Bu örnekte, seçelim **storageacct1**.
   ![Depolama şifrelemesini etkinleştirme][2]
3. **Şifreleme** dikey **storageacct1** açılır. Seçin **etkin**.
   ![Şifreleme dikey penceresi][3]
4. **Kaydet**’i seçin.

Depolama şifrelemesi için artık etkinleştirdiğiniz **storageacct1**.


## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Güvenlik Merkezi önerisini "Azure depolama hesabı için şifrelemeyi etkinleştirir." uygulama nasıl oluşturulacağını gösterir Azure depolama hizmeti şifrelemesi hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Bekleyen veri için Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izleme hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) -yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md) -hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) -Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
