---
title: İş ortağı kimliği için Azure hesabınızın bağlamak | Microsoft Docs
description: İş ortağı kimliği müşterinin kaynakları yönetmek için kullandığınız kullanıcı hesabına bağlayarak Azure müşterileriyle yaşadığımız izleyin.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: cwatson
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 1e2492d978073f63c1c9494d652ec35a7d6565b7
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52274188"
---
# <a name="link-partner-id-to-your-azure-accounts"></a>Azure hesaplarınızı için iş ortağı Kimliğini bağlama

Bir iş ortağı olarak, müşterinin kaynaklarını yönetmek için kullanılan hesaplara, iş ortağı Kimliğinizi bağlayarak, etki, müşterilerle yaşadığımız takip edebilirsiniz.

Bu özellik, genel önizlemede kullanılabilir.

## <a name="get-access-from-your-customer"></a>Müşteriden erişin

İş ortağı Kimliğinizin bağlamadan önce müşteri erişim aşağıdaki seçeneklerden birini kullanarak Azure kaynaklarını vermelidir:

- **Konuk kullanıcı:** müşteriniz Konuk kullanıcı olarak ekleyebilir ve herhangi bir RBAC rolü atayın. Daha fazla bilgi için [başka bir dizinden Konuk kullanıcıları eklemek](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Dizin hesabı:** müşterinizin directory'lerinde kuruluşunuzdaki yeni kullanıcı oluşturma ve herhangi bir RBAC rolü atayın.

- **Hizmet sorumlusu:** müşterinizin directory'lerinde kuruluşunuzdaki bir uygulamanızı veya betiğinizi ekleyebilir ve herhangi bir RBAC rolü atayın. Uygulamanızı veya betiğinizi kimliğini, hizmet sorumlusu olarak bilinir.

## <a name="link-partner-id"></a>İş ortağı kimliğini bağlama

Müşterinin kaynaklarına erişiminiz olduğunda Azure portalı, PowerShell veya CLI, Microsoft iş ortağı ağ Kimliğini (MPN kimliği) bağlamak için kullanıcı kimliği veya hizmet sorumlusu kullanın. Her müşteri kiracısında iş ortağı kimliği Bağla gerekir.

### <a name="use-azure-portal-to-link-new-partner-id"></a>Yeni iş ortağı kimliği Bağla için Azure portalını kullanma

1. Git [bir iş ortağı kimliği Bağla](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) Azure portalında.

2. Azure Portal’da oturum açın.

3. Microsoft iş ortağı kimliğini girin. İş ortağı kimliği [Microsoft iş ortağı ağı (MPN)](https://partner.microsoft.com/) kuruluş kimliği.

  ![İş ortağı Kimliğini bağlama gösteren ekran görüntüsü](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Başka bir müşteri için iş ortağı kimliği Bağla için dizin değiştiricisini kullanın. Anahtar dizini altında dizininizi seçin.

  ![İş ortağı Kimliğini bağlama gösteren ekran görüntüsü](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-new-partner-id"></a>Yeni iş ortağı kimliği Bağla için PowerShell kullanma

1. Yükleme [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner) PowerShell modülü.

2. Müşterinin kiracısına kullanıcı hesabı veya daha fazla bilgi için hizmet sorumlusu ile oturum açmak için bkz: [Powershell ile oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-5.2.0).
 
   ```azurepowershell-interactive
    C:\> Connect-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. Yeni bir iş ortağı kimliği Bağla İş ortağı kimliği [Microsoft iş ortağı Network(MPN)](https://partner.microsoft.com/) kuruluş kimliği.

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini alın
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>Güncelleştirme bağlı iş ortağı kimliği
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliği Sil
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-cli-to-link-new-partner-id"></a>Yeni iş ortağı kimliği Bağla için CLI kullanma
1.  CLI uzantısını yükleyin.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ``` 

2.  Müşterinin Kiracı Kullanıcı hesabı veya hizmet sorumlusu ile oturum açın. Daha fazla bilgi için [Azure CLI ile oturum açın](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ``` 

3.  Yeni bir iş ortağı kimliği Bağla İş ortağı kimliği [Microsoft iş ortağı Network(MPN)](https://partner.microsoft.com/) kuruluş kimliği.

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

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

**Kimin iş ortağı Kimliğini bağlayabilir miyim?**

Müşterinin Azure kaynaklarını yöneten iş ortağı kuruluştan herhangi bir kullanıcı, iş ortağı Kimliğini hesabınıza bağlayabilirsiniz.

**Bir iş ortağı kimliği bağlandığında değiştirilebilir mi?**

Evet, bağlantılı iş ortağı kimliği değişti, eklenen veya kaldırılacak.

**Ne bir kullanıcı birden fazla müşteri Kiracı hesabınız var?**

İş ortağı Kimliğini ve hesap arasındaki bağlantı, her bir müşterinin Kiracı için gerçekleştirilir.  Her müşteri kiracısında iş ortağı kimliği Bağla gerekir.

**Diğer iş ortağı veya müşterinin düzenleyebilir veya iş ortağı Kimliğini bağlantısını kaldırmak?**

Bağlantı kullanıcı hesabı düzeyinde ilişkili değil. Yalnızca düzenlemek veya iş ortağı kimliğini bağlantısını Kaldır Müşteri ve diğer iş ortağı bağlantısını için iş ortağı kimliğini değiştiremezsiniz 
