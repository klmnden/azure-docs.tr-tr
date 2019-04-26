---
title: Bir Azure hesabı için bir iş ortağı kimliği bağlantı | Microsoft Docs
description: Müşterinin kaynakları yönetmek için kullandığınız kullanıcı hesabına bir iş ortağı kimliği bağlayarak Azure müşterileriyle yaşadığımız izleyin.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: banders
ms.date: 03/12/2019
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 6bf61e2afd96e3923938ac4f815d34ae08f7c618
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371300"
---
# <a name="link-a-partner-id-to-your-azure-accounts"></a>Azure hesaplarınızdan bir iş ortağı kimliği Bağla

Bir iş ortağı olarak, müşterilerle yaşadığımız arasında etkisi izleyebilirsiniz. Bir müşterinin kaynaklarını yönetmek için kullanılan hesaplara, iş ortağı Kimliğinizi bağlayabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-access-from-your-customer"></a>Müşteriden erişin

İş ortağı Kimliğinizin bağlamadan önce müşteri erişim aşağıdaki seçeneklerden birini kullanarak Azure kaynaklarını vermelidir:

- **Konuk kullanıcı**: Müşteri Konuk kullanıcı olarak ekleyebilir ve herhangi bir rol tabanlı erişim denetimi (RBAC) rollerini atayın. Daha fazla bilgi için [başka bir dizinden Konuk kullanıcıları eklemek](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Dizin hesabı**: Müşteriniz, sizin için kendi dizininde bir kullanıcı hesabı oluşturun ve herhangi bir RBAC rolü atayın.

- **Hizmet sorumlusu**: Müşteri, kuruluşunuzdaki directory'lerinde bir uygulama veya betik ekleme ve herhangi bir RBAC rolü atayın. Uygulamanızı veya betiğinizi kimliğini bir hizmet sorumlusu olarak bilinir.

## <a name="link-to-a-partner-id"></a>İş ortağı kimliğine bağlantı

Müşterinin kaynaklarına erişiminiz olduğunda, kullanıcı kimliği veya hizmet sorumlusu, Microsoft iş ortağı ağ Kimliğini (MPN kimliği) bağlamak için Azure portalı, PowerShell veya Azure CLI'yı kullanın. Her bir müşterinin Kiracı İş ortağı kimliği Bağla.

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı Kimliğine bağlamak için Azure portalını kullanma

1. Git [bir iş ortağı kimliği Bağla](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) Azure portalında.

2. Azure Portal’da oturum açın.

3. Microsoft iş ortağı kimliğini girin. İş ortağı kimliği [Microsoft iş ortağı ağı](https://partner.microsoft.com/) kuruluşunuz için kimliği.

   ![Bir iş ortağı Kimliğine bağlantısını gösteren ekran görüntüsü](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Başka bir müşteri için bir iş ortağı kimliği Bağla dizine geçin. Altında **dizini Değiştir**, dizininizi seçin.

   ![Dizini Değiştir gösteren ekran görüntüsü](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>İçin yeni bir iş ortağı kimliği Bağla için PowerShell kullanma

1. Yükleme [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner) PowerShell modülü.

2. Müşterinin Kiracı Kullanıcı hesabını veya hizmet sorumlusu ile oturum açın. Daha fazla bilgi için [oturum PowerShell ile oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

   ```azurepowershell-interactive
    C:\> Connect-AzAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
   ```

3. İçin yeni iş ortağı kimliği Bağla İş ortağı kimliği [Microsoft iş ortağı ağı](https://partner.microsoft.com/) kuruluşunuz için kimliği.

    ```azurepowershell-interactive
    C:\> new-AzManagementPartner -PartnerId 12345
    ```

#### <a name="get-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini alın
```azurepowershell-interactive
C:\> get-AzManagementPartner
```

#### <a name="update-the-linked-partner-id"></a>Güncelleştirme bağlı iş ortağı kimliği
```azurepowershell-interactive
C:\> Update-AzManagementPartner -PartnerId 12345
```
#### <a name="delete-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliği Sil
```azurepowershell-interactive
C:\> remove-AzManagementPartner -PartnerId 12345
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı Kimliğine bağlamak için Azure CLI kullanma
1. Azure CLI uzantısını yükleyin.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ```

2. Müşterinin Kiracı Kullanıcı hesabını veya hizmet sorumlusu ile oturum açın. Daha fazla bilgi için [oturum Azure CLI ile oturum açın](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ```

3. İçin yeni iş ortağı kimliği Bağla İş ortağı kimliği [Microsoft iş ortağı ağı](https://partner.microsoft.com/) kuruluşunuz için kimliği.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini alın
```azurecli-interactive
C:\ az managementpartner show
```

#### <a name="update-the-linked-partner-id"></a>Güncelleştirme bağlı iş ortağı kimliği
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
```

#### <a name="delete-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliği Sil
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
```

## <a name="next-steps"></a>Sonraki adımlar

Tartışmaya katılın [Microsoft iş ortağı topluluğu](https://aka.ms/PALdiscussion) güncelleştirmeleri almak ya da geri bildirim göndermek için.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Kimin iş ortağı Kimliğini bağlayabilir miyim?**

İş ortağı kuruluştan bir müşterinin Azure kaynakları yöneten herhangi bir kullanıcı, iş ortağı Kimliğini hesabınıza bağlayabilirsiniz.

**Sonra bağlı iş ortağı kimliği değiştirilebilir mi?**

Evet. Bağlantılı iş ortağı kimliği değişti, eklenen veya kaldırılacak.

**Ne bir kullanıcı birden fazla müşteri kiracısında bir hesap var?**

İş ortağı Kimliğini ve hesap arasındaki bağlantı, her bir müşterinin Kiracı için gerçekleştirilir. Her bir müşterinin Kiracı İş ortağı kimliği Bağla.

**Diğer iş ortakları veya müşterilerin düzenleyebilir veya iş ortağı Kimliğini bağlantısını kaldırmak?**

Bağlantı kullanıcı hesabı düzeyinde ilişkili değil. Yalnızca düzenlemek veya iş ortağı kimliğini bağlantısını Kaldır Müşteri ve diğer iş ortaklarıyla bağlantı iş ortağı kimliğine değiştiremezsiniz


**Şirketim birden fazla varsa, hangi MPN kimliği kullanmam gerekir?**

Sanal organization(v-org) MPN kimliği dışındaki tüm geçerli MPN kimliği kullanabilirsiniz. Çoğu iş ortakları, burada müşteri tabanlı veya hizmetlerine gönderilen coğrafyadaki MPN kimliği kullanmayı seçin.

**Bağlantılı iş ortağı kimliği için raporlama etkileyen gelir nerede bulabilirim?**

Adresindeki etkileyen gelir raporlama bulabilirsiniz [My öngörüleri Panosu](https://partner.microsoft.com/membership/reports/myinsights). İş ortağı yönetim bağlantı iş ortağı ilişkilendirme türü olarak seçmeniz gerekir.

**Müşterim raporlarında neden göremiyorum?**

Aşağıdaki nedenlerden dolayı raporlarında müşteri göremiyorum

1. Bağlı kullanıcı hesabı sahip olmadığı [rol tabanlı erişim](https://docs.microsoft.com/azure/role-based-access-control/overview) herhangi bir müşteri Azure abonelik veya kaynak.

2. Kullanıcının sahip olduğu Azure aboneliğini [rol tabanlı erişim](https://docs.microsoft.com/azure/role-based-access-control/overview) erişimi tüm kullanım yok.

**İş ortağı kimliği ile Azure Stack çalışır bağlantı mu?**

Evet, Azure Stack için iş ortağı Kimliğinizin bağlayabilirsiniz.
