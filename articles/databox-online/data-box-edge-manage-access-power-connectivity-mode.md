---
title: Azure veri kutusu Edge cihaz erişimi, güç ve bağlantı modunu | Microsoft Docs
description: Erişim, güç ve bağlantı modu için yardımcı olur, Azure'a veri aktarımı Azure veri kutusu sınır cihazı yönetme işlemi açıklanır
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: 8937f4c47f0fa84d4ec371e951cff8a2fdaa8481
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476907"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge"></a>Azure veri kutusu Edge için erişim, güç ve bağlantı modunu yönetin

Bu makalede, Azure veri kutusu Edge için erişim, güç ve bağlantı modunu yönetmek açıklar. Bu işlemleri yerel web kullanıcı Arabirimi veya Azure Portalı aracılığıyla gerçekleştirilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz erişimini yönetme
> * Bağlantı modunu yönetin
> * Güç Yönetimi


## <a name="manage-device-access"></a>Cihaz erişimini yönetme

Veri kutusu Edge cihazınıza erişim bir cihaz parolası kullanımı tarafından denetlenir. Parola yerel web UI aracılığıyla değiştirebilirsiniz. Ayrıca, Azure portalında cihaz parolasını sıfırlayabilirsiniz.

### <a name="change-device-password"></a>Cihaz parolasını değiştirme

Cihaz parolasını değiştirmek için yerel kullanıcı Arabirimi aşağıdaki adımları izleyin.

1. Yerel web kullanıcı Arabirimi, Git **Bakım > parola değişikliği**.
2. Geçerli parola ve yeni parolayı girin. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterleri 3 olması gerekir: büyük harf, küçük harfler, sayısal ve özel karakter. Yeni parolayı onaylayın.

    ![Parola Değiştir](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Seçin **parolasını değiştirme**.
 
### <a name="reset-device-password"></a>Cihaz parolasını sıfırlama

Sıfırlama iş akışı, eski parolayı çağırmak kullanıcı gerektirmez ve parola kayıp olduğunda yararlıdır. Bu iş akışı, Azure portalında gerçekleştirilir.

1. Azure portalında Git **genel bakış > yönetici parolası sıfırlama**.

    ![Parola sıfırla](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Yeni bir parola girin ve parolayı doğrulayın. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterleri 3 olması gerekir: büyük harf, küçük harfler, sayısal ve özel karakter. Seçin **sıfırlama**.

    ![Parola sıfırla](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Kaynak erişimini yönetme

Veri kutusu Edge/veri kutusu ağ geçidi, IOT Hub ve Azure depolama kaynağı oluşturmak için bir kaynak grubu düzeyinde izinleri katkıda bulunan olarak veya üzeri gerekir. Ayrıca ilgili kaynak sağlayıcıları kayıtlı olması gerekir. Etkinleştirme anahtarı ve kimlik bilgileri gerektiren işlemler için Azure Active Directory Graph API'si için izinleri de gereklidir. Bunlar aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-microsoft-azure-active-directory-graph-api-permissions"></a>Microsoft Azure Active Directory Graph API izinleri Yönet

Veri kutusu sınır cihazı veya kimlik bilgileri gerektiren bir işlem gerçekleştirmek için etkinleştirme anahtarı oluştururken, Azure Active Directory Graph API'si izinlerinin olması gerekir. Kimlik bilgileri gerektiren işlemler aşağıdakilerden biri olabilir:

-  Bir paylaşımı ile ilişkili depolama hesabı oluşturuluyor.
-  Cihaz paylaşımlarında erişebilen kullanıcı oluşturma.

Olması bir `User` yapabilmesi gereken Active Directory kiracısı üzerinde erişim `Read all directory objects`. İçin izniniz yok gibi Konuk kullanıcı olamaz `Read all directory objects`. Konuk, sonra bir etkinleştirme anahtarı, bir veri kutusu Edge cihazınıza paylaşımında oluşturulmasını oluşturma gibi işlemler kullanıyorsanız, bir kullanıcının oluşturma tüm başarısız olur.

Azure Active Directory Graph API'si için kullanıcılara erişim sağlamak nasıl hakkında daha fazla bilgi için bkz. [Yöneticiler, kullanıcılar ve Konuk kullanıcılar için erişim varsayılan](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).

### <a name="register-resource-providers"></a>Kaynak sağlayıcılarını kaydetme

Bir Azure kaynağında (Azure Resource Manager modelinde) sağlamak için bu kaynağın oluşturulmasını destekleyen bir kaynak sağlayıcısı gerekir. Örneğin, bir sanal makine sağlamak için bir abonelikte bulunan 'Microsoft.Compute' kaynak sağlayıcısı olmalıdır.
 
Abonelik düzeyinde kaynak sağlayıcı kayıtlı. Varsayılan olarak, yeni bir Azure aboneliği ile yaygın olarak kullanılan kaynak sağlayıcıları listesini önceden büyük/küçük harf kaydettirilir. Kaynak Sağlayıcı 'Microsoft.DataBoxEdge' Bu listede dahil edilmez.

Kullanıcıların bu kaynaklar için kaynak sağlayıcıları olduğu sürece zaten 'Microsoft.DataBoxEdge' gibi kaynaklara sahip hakları, sahip oldukları kendi kaynak grupları içindeki oluşturabilmek abonelik düzeyinde için erişim izinleri vermeniz gerekmez. kayıtlı.

Herhangi bir kaynak oluşturma girişiminde bulunmadan önce abonelikte kaynak sağlayıcısına kayıtlı olduğundan emin olun. Kaynak sağlayıcısı kayıtlı değilse, yeni kaynak oluşturan kullanıcıya abonelik düzeyinde gerekli kaynak sağlayıcısını kaydetmek için yeterli haklara sahip olduğundan emin olmak gerekir. Ardından bu de yapmadıysanız aşağıdaki hatayı görürsünüz:

*Abonelik <Subscription name> şu kaynak sağlayıcılarını kaydetme izni yok: Microsoft.DataBoxEdge.*


Geçerli abonelikte kaynak sağlayıcılarının bir listesini almak için aşağıdaki komutu çalıştırın:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Veri kutusu Edge cihazı için `Microsoft.DataBoxEdge` kaydedilmelidir. Kaydedilecek `Microsoft.DataBoxEdge`, abonelik yöneticisinin aşağıdaki komutu çalıştırın:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Bir kaynak sağlayıcısı kaydetme hakkında daha fazla bilgi için bkz. [çözmek için kaynak Sağlayıcısı kaydı hataları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors).

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