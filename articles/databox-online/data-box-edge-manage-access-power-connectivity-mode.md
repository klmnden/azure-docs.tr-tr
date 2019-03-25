---
title: Microsoft Azure veri kutusu Edge cihaz erişimi, güç ve bağlantı modunu | Microsoft Docs
description: Erişim, güç ve bağlantı modu için yardımcı olur, Azure'a veri aktarımı Azure veri kutusu sınır cihazı yönetme işlemi açıklanır
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/21/2019
ms.author: alkohli
ms.openlocfilehash: e0b8b35c654f0716fae1e6ab785f57dcf04e1a5a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400896"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge-preview"></a>Erişim, güç ve bağlantı modunu, Azure veri kutusu Edge (Önizleme) için yönetme

Bu makalede, Azure veri kutusu Edge için erişim, güç ve bağlantı modunu yönetmek açıklar. Bu işlemleri yerel web kullanıcı Arabirimi veya Azure Portalı aracılığıyla gerçekleştirilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz erişimini yönetme
> * Bağlantı modunu yönetin
> * Güç Yönetimi

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Sipariş vermeden ve bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="manage-device-access"></a>Cihaz erişimini yönetme

Veri kutusu Edge cihazınıza erişim bir cihaz parolası kullanımı tarafından denetlenir. Parola yerel web UI aracılığıyla değiştirebilirsiniz. Ayrıca, Azure portalında cihaz parolasını sıfırlayabilirsiniz.

### <a name="change-device-password"></a>Cihaz parolasını değiştirme

Cihaz parolasını değiştirmek için yerel kullanıcı Arabirimi aşağıdaki adımları izleyin.

1. Yerel web kullanıcı Arabirimi, Git **Bakım > parola değişikliği**.
2. Geçerli parola ve yeni parolayı girin. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterleri 3 olması gerekir: büyük harf, küçük harfler, sayısal ve özel karakter. Yeni parolayı onaylayın.

    ![Parolayı değiştir](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Seçin **parolasını değiştirme**.
 
### <a name="reset-device-password"></a>Cihaz parolasını sıfırla

Sıfırlama iş akışı, eski parolayı çağırmak kullanıcı gerektirmez ve parola kayıp olduğunda yararlıdır. Bu iş akışı, Azure portalında gerçekleştirilir.

1. Azure portalında Git **genel bakış > yönetici parolası sıfırlama**.

    ![Parola sıfırlama](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Yeni bir parola girin ve parolayı doğrulayın. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterleri 3 olması gerekir: büyük harf, küçük harfler, sayısal ve özel karakter. Seçin **sıfırlama**.

    ![Parola sıfırlama](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Bağlantı modunu yönetin

Varsayılan tam olarak bağlı mod dışında cihazınız bağlı kısmen veya tamamen bağlantısı kesilmiş modunda da çalıştırabilirsiniz. Bu modların her birinde aşağıdaki gibi tanımlanır:

- **Tam olarak bağlı** -cihaz çalıştığı normal varsayılan mod budur. Bu modda bulut karşıya yükleme ve indirme verilerinin etkindir. Cihazı yönetmek için Azure portalı veya yerel web kullanıcı Arabirimi kullanabilirsiniz.

- **Kısmen bağlı** – bu modda, cihazı karşıya yükleyemez veya veri ancak yönetilen Azure portalı üzerinden paylaşım indirin.

    Bu mod genellikle zaman ölçülen uydu ağ üzerinde kullanılır ve hedef ağ bant genişliği kullanımını en aza indirmektir. En az bir ağ kullanımını izleme işlemleri için cihaz yine de meydana gelebilir.

- **Bağlantısı kesilmiş** : Bu modda, cihazın tamamen bulutta ve hem bulut karşıya kesilir ve indirmeler devre dışı bırakıldı. Cihaz, yalnızca yerel web UI yönetilebilir.

    Bu mod, genellikle Cihazınızı çevrimdışı duruma getirmek istediğinizde kullanılır.

Cihaz modunu değiştirmek için aşağıdaki adımları izleyin:

1. Yerel web kullanıcı Arabirimi cihazınızın, Git **yapılandırma > bulut ayarları**.
2. Açılan listeden, cihazı çalışmak istediğiniz modu seçin. Aralarından seçim yapabileceğiniz **tam olarak bağlı**, **kısmen bağlı**, ve **bağlantısı tamamen kesilmiş**. Cihaz kısmen bağlantı kesik moddayken çalıştırmak için etkinleştirme **Azure portal Yönetim**.

    ![Bağlantı modu](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Güç Yönetimi

Kapatabilir veya yerel web kullanıcı arabirimini kullanarak fiziksel Cihazınızı yeniden başlatın. Biz yeniden başlatmadan önce paylaşımları veri sunucusu ve cihaz çevrimdışı olması önerilir. Bu eylem veri bozulması olasılığını en aza indirir.

1. Yerel web kullanıcı Arabirimi, Git **Bakım > güç ayarları**.
2. Seçin **kapatma** veya **yeniden** yapmak istediğinize bağlı olarak.

    ![Güç ayarları](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Onayınız istendiğinde seçin **Evet** devam etmek için.

> [!NOTE]
> Fiziksel cihazı kapatmak, açmak için cihazdaki güç düğmesine anında iletme gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [paylaşımları yönetme](data-box-edge-manage-shares.md).