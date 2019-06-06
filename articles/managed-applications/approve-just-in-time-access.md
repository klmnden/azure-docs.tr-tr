---
title: Yapılandırma ve Azure yönetilen uygulamalar için tam zamanında erişim onaylayın
description: Nasıl Azure yönetilen uygulamalar tüketicilerinin yönetilen bir uygulama için tam zamanında erişim isteklerini onaylama açıklar.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: 55fbf7292ab894cad3de3de9e96ddc96fe0b79b3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481722"
---
# <a name="configure-and-approve-just-in-time-access-for-azure-managed-applications"></a>Yapılandırma ve Azure yönetilen uygulamalar için tam zamanında erişim onaylayın

Yönetilen bir uygulama kullanıcısı, yayımcı vererek rahat olmadıklarınız yönetilen kaynak grubuna kalıcı erişim. Daha fazla denetime yönetilen kaynaklara erişim izni vermek için Azure yönetilen uygulamalar, şu anda Önizleme aşamasında olan, just-ın-time (JIT) erişim denilen bir özelliği sağlar. Bu ne zaman onaylamanız sağlar ve kaynak grubu için ne kadar yayımcı erişebilir. Yayımcı bu süre boyunca gerekli güncelleştirmeleri yapabilirsiniz, ancak bu süre sona erdiğinde, yayımcının erişim süresi dolar.

Erişim izni verme için iş akışı şöyledir:

1. Yayımcının Market'te bir yönetilen uygulama ekler ve JIT erişim kullanılabilir olduğunu belirtir.

1. Dağıtım sırasında yönetilen uygulama örneğini JIT erişimini etkinleştirin.

1. Dağıtımdan sonra JIT erişim ayarlarını değiştirebilirsiniz.

1. Yayımcı erişim için bir istek gönderir.

1. İsteği onaylayın.

Bu makalede, tüketicilerin JIT erişimi etkinleştirmek ve istekleri onaylamak için eylemleri üzerinde odaklanır. JIT ağ erişimi olan bir yönetilen uygulama yayımlama hakkında bilgi edinmek için [Azure yönetilen uygulamalar tam zamanında erişim isteği](request-just-in-time-access.md).

> [!NOTE]
> Tam zamanında erişim kullanmak için olmalıdır bir [Azure Active Directory P2 lisansı](../active-directory/privileged-identity-management/subscription-requirements.md).

## <a name="enable-during-deployment"></a>Dağıtım sırasında etkinleştir

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Bir Market girişi ile etkin JIT için bir yönetilen uygulamayı bulun. **Oluştur**’u seçin.

1. Yeni yönetilen uygulama için değerleri sağlayarak **JIT yapılandırma** adım etkinleştirme veya devre dışı yönetilen uygulama için JIT erişim sağlar. Seçin **Evet** için **JIT erişimi etkinleştirme**. Bu seçenek, Market'te etkin JIT ile tanımlanmış yönetilen uygulamalar için varsayılan olarak seçilidir.

   ![Erişimi yapılandırma](./media/approve-just-in-time-access/configure-jit-access.png)

   Bu gibi durumlarda, JIT erişimi yalnızca dağıtım sırasında etkinleştirebilirsiniz. Seçerseniz **Hayır**, yayımcı yönetilen kaynak grubuna kalıcı erişim alır. Daha sonra JIT erişim etkinleştiremezsiniz.

1. Varsayılan onay ayarlarını değiştirmek için seçin **özelleştirme JIT yapılandırma**.

   ![Erişim özelleştirme](./media/approve-just-in-time-access/customize-jit-access.png)

   Varsayılan olarak etkin JIT ile yönetilen bir uygulama aşağıdaki ayarlara sahip:

   * Otomatik onay modda
   * En fazla erişim süresi – 8 saat
   * Onaylayanlar – yok

   Onay modu ayarlandığında **otomatik**, onaylayanlar her istek için bir bildirim alır ancak istek otomatik olarak onaylanır. Ayarlandığında **el ile**onaylayanlar her istek için bir bildirim alır ve bunlardan biri gerekir onaylayın.

   Maksimum etkinleştirme süresi en fazla yönetilen kaynak grubuna erişim için bir yayımcı isteyebilir süreyi belirtir.

   JIT erişim istekleri onaylamak için kullanabileceğiniz Azure Active Directory Kullanıcıları onaylayanlar listesidir. Approver eklemek için seçin **onaylayan ekleme** kullanıcıyı arayın.

   Ayar güncelleştirdikten sonra Seç **Kaydet**.

## <a name="update-after-deployment"></a>Dağıtımdan sonra güncelleştir

İstekleri nasıl onaylanmaktadır değerleri değiştirebilirsiniz. Dağıtım sırasında JIT erişimini etkinleştirmediyseniz, ancak, bunu daha sonra etkinleştiremezsiniz.

Dağıtılan bir yönetilen uygulama ayarlarını değiştirmek için:

1. Portalda yönetme uygulaması'nı seçin.

1. Seçin **JIT yapılandırma** ve ayarları gerektiği gibi değiştirin.

   ![Erişim ayarlarını değiştir](./media/approve-just-in-time-access/change-settings.png)

1. İşiniz bittiğinde **Kaydet**’i seçin.

## <a name="approve-requests"></a>İstekleri onaylama

Yayımcı erişim istediğinde, isteği bildirim alırsınız. JIT erişim isteklerini doğrudan üzerinden yönetilen uygulamayı veya Azure AD Privileged Identity Management hizmeti aracılığıyla tüm yönetilen uygulamalar arasında onaylayabilirsiniz. Tam zamanında erişim kullanmak için olmalıdır bir [Azure Active Directory P2 lisansı](../active-directory/privileged-identity-management/subscription-requirements.md).

Yönetilen uygulama isteklerini onaylamak için:

1. Seçin **JIT erişim** seçin ve yönetilen uygulama için **istekleri onaylama**.

   ![İstekleri onaylama](./media/approve-just-in-time-access/approve-requests.png)
 
1. Onaylanacak bir istek'i seçin.

   ![İstek'i seçin](./media/approve-just-in-time-access/select-request.png)

1. Neden seçin ve onay biçiminde belirtin **Onayla**.

Azure AD Privileged Identity Management aracılığıyla istekleri onaylamak için:

1. Seçin **tüm hizmetleri** ve için aramaya başlanacak **Azure AD Privileged Identity Management**. Kullanılabilir seçenekler arasından seçin.

   ![Arama hizmeti](./media/approve-just-in-time-access/search.png)

1. Seçin **istekleri onaylama**.

   ![İstekleri Onayla seçeneğini belirleme](./media/approve-just-in-time-access/select-approve-requests.png)

1. Seçin **Azure yönetilen uygulamalar**ve onaylanacak bir istek'i seçin.

   ![İstekleri seçin](./media/approve-just-in-time-access/view-requests.png)

## <a name="next-steps"></a>Sonraki adımlar

JIT ağ erişimi olan bir yönetilen uygulama yayımlama hakkında bilgi edinmek için [Azure yönetilen uygulamalar tam zamanında erişim isteği](request-just-in-time-access.md).
