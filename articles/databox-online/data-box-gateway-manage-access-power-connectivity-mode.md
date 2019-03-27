---
title: Microsoft Azure veri kutusu ağ geçidi cihaz erişimi, güç ve bağlantı modunu | Microsoft Docs
description: Erişim, güç ve bağlantı modu için yardımcı olur, Azure'a veri aktarımı Azure veri kutusu ağ geçidi cihazı yönetme işlemi açıklanır
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 72d3455f37d0ccef0dd5b7d8882f70670de07572
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58497329"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-gateway"></a>Erişim, güç ve bağlantı modunu, Azure veri kutusu ağ geçidiniz için yönetme

Bu makalede, Azure veri kutusu ağ geçidiniz için erişim, güç ve bağlantı modunu yönetmek açıklar. Bu işlemleri yerel web kullanıcı Arabirimi veya Azure Portalı aracılığıyla gerçekleştirilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz erişimini yönetme
> * Bağlantı modunu yönetin
> * Güç Yönetimi

## <a name="manage-device-access"></a>Cihaz erişimini yönetme

Veri kutusu ağ geçidi cihazınıza erişim bir cihaz parolası kullanımı tarafından denetlenir. Parola yerel web UI aracılığıyla değiştirebilirsiniz. Ayrıca, Azure portalında cihaz parolasını sıfırlayabilirsiniz.

### <a name="change-device-password"></a>Cihaz parolasını değiştirme

Cihaz parolasını değiştirmek için yerel kullanıcı Arabirimi aşağıdaki adımları izleyin.

1. Yerel web kullanıcı Arabirimi, Git **Bakım > parola değişikliği**.
2. Geçerli parola ve yeni parolayı girin. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterleri 3 olması gerekir: büyük harf, küçük harfler, sayısal ve özel karakter. Yeni parolayı onaylayın.

    ![Parolayı değiştir](media/data-box-gateway-manage-access-power-connectivity-mode/change-password-1.png)

3. Tıklayın **parolasını değiştirme**.
 
### <a name="reset-device-password"></a>Cihaz parolasını sıfırla

Sıfırlama iş akışı, eski parolayı çağırmak kullanıcı gerektirmez ve parola kayıp olduğunda yararlıdır. Bu iş akışı, Azure portalında gerçekleştirilir.

1. Azure portalında Git **genel bakış > yönetici parolası sıfırlama**.

    ![Parola sıfırlama](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-1.png)

 
2. Yeni bir parola girin ve parolayı doğrulayın. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterleri 3 olması gerekir: büyük harf, küçük harfler, sayısal ve özel karakter. Tıklayın **sıfırlama**.

    ![Parola sıfırlama](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Bağlantı modunu yönetin

Varsayılan normal mod dışında Cihazınızı kısmen bağlantısı kesilmiş veya bağlantısı kesilmiş modunda da çalıştırabilirsiniz. Bu modların her birinde aşağıdaki gibi tanımlanır:

- **Kısmen bağlı** – bu modda, herhangi bir cihaz yükleyemezsiniz veri paylaşımları ancak yönetilen Azure portalı üzerinden.

    Bu mod genellikle zaman ölçülen uydu ağ üzerinde kullanılır ve hedef ağ bant genişliği kullanımını en aza indirmektir. En az bir ağ kullanımını izleme işlemleri için cihaz yine de meydana gelebilir.

- **Bağlantısı kesilmiş** : Bu modda, cihazın tamamen bulutta ve hem bulut karşıya kesilir ve indirmeler devre dışı bırakıldı. Cihaz, yalnızca yerel web UI yönetilebilir.

    Bu mod, genellikle Cihazınızı çevrimdışı duruma getirmek istediğinizde kullanılır.

Cihaz modunu değiştirmek için aşağıdaki adımları izleyin:

1. Yerel web kullanıcı Arabirimi cihazınızın, Git **yapılandırma > bulut ayarları**.
2. Devre dışı **bulut karşıya yükleme ve indirme**.
3. Cihaz kısmen bağlantı kesik moddayken çalıştırmak için etkinleştirme **Azure portal Yönetim**.

    ![Bağlantı modu](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-1.png)
 
4. Cihaz bağlantı kesik moddayken çalıştırmak için devre dışı **Azure portal Yönetim**. Artık cihaz yalnızca yerel web UI yönetilebilir.

    ![Bağlantı modu](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-2.png)

## <a name="manage-power"></a>Güç Yönetimi

Kapatabilir veya yerel web kullanıcı arabirimini kullanarak fiziksel ve sanal Cihazınızı yeniden başlatın. Cihazı yeniden başlatmadan önce konaktaki paylaşımları sonra da cihazı çevrimdışına almanız önerilir. Bu eylem veri bozulması olasılığını en aza indirir.

1. Yerel web kullanıcı Arabirimi, Git **Bakım > güç ayarları**.
2. Tıklayın **kapatma** veya **yeniden** yapmak istediğinize bağlı olarak.

    ![Güç ayarları](media/data-box-gateway-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Onayınız istendiğinde tıklayın **Evet** devam etmek için.

> [!NOTE]
> Sanal cihazı kapatmak, hiper yönetici Yönetimi aracılığıyla cihaz başlatmanız gerekir.
