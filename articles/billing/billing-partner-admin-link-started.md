---
title: Bağlantı kimliği ortağına Azure hesabı | Microsoft Docs
description: Müşteri'nin kaynakları yönetmek için kullandığınız kullanıcı hesabı için iş ortağı kimliği bağlayarak katılımlar Azure müşteri ile izleyebilirsiniz.
services: billing
author: dhirajgandhi
ms.author: dhgandhi
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: f729a0228c6b0c2f514ab7170299b0271ed28ad2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="link-partner-id-to-your-azure-accounts"></a>Azure hesaplarınıza bağlantı iş ortağı kimliği 
İş ortağı olarak, müşterinin kaynakları yönetmek için kullanılan hesaplar için iş ortağı Kimliğinizi bağlayarak, etkisi, müşteri katılımlar izleyebilirsiniz.

Bu özellik, genel önizleme olarak kullanılabilir. 

## <a name="get-access-from-your-customer"></a>Müşteriden erişin 
İş ortağı Kimliğinizi bağlamadan önce müşterinizin Azure kaynaklarını aşağıdaki seçeneklerden birini kullanarak erişmenizi gerekir:

- **Konuk kullanıcı:** müşteri Konuk kullanıcı olarak ekleyin ve herhangi bir RBAC rolü atayın. Daha fazla bilgi için bkz: [başka bir dizinden Konuk kullanıcılar eklemek](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Dizin hesabı:** müşteri directory'lerinde kuruluşunuzdaki yeni bir kullanıcı oluşturun ve herhangi bir RBAC rolü atayın. 

- **Hizmet sorumlusu:** müşteri kuruluşunuzdaki directory'lerinde bir uygulama ya da komut dosyasını ekleyin ve herhangi bir RBAC rolü atayın. Uygulama veya betik kimliğini hizmet sorumlusu bilinir.

## <a name="link-partner-id"></a>Bağlantı iş ortağı kimliği
Müşteri'nin kaynaklarına erişimi varsa, Microsoft iş ortağı ağ kimliği (MPN kimliği), kullanıcı kimliği veya hizmet sorumlusu bağlamak için PowerShell veya CLI kullanın. Her bir müşterinin Kiracı için iş ortağı Kimliğini bağlamak zorunda. 

### <a name="use-powershell-to-link-new-partner-id"></a>Yeni iş ortağı kimliği bağlamak için PowerShell kullanın

1. Yükleme [AzurePartnerRP](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner/0.1.0-preview) PowerShell modülü.

2. Oturum açtığınızda müşterinin Kiracı, kullanıcı hesabı veya hizmet sorumlusu, daha fazla bilgi için bkz: [Powershell ile oturum açma](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azurermps-5.2.0).
 
   ```azurepowershell-interactive
    C:\> Login-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. Yeni iş ortağı kimliğini bağlantı Kimliktir iş ortağı [Microsoft iş ortağı Network(MPN)](https://partner.microsoft.com/) , kuruluşunuzun kimliği.

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini Al
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini güncelleştir
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini Sil
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-cli-to-link-new-partner-id"></a>Yeni iş ortağı kimliği bağlamak için CLI kullanın
1.  CLI uzantısını yükleyin.

    ```azure-cli
    C:\ az extension add --name managementpartner
    ``` 

2.  Müşteri'nin Kiracı hizmet sorumlusu ve kullanıcı hesabı ile oturum açın. Daha fazla bilgi için bkz: [oturum Azure CLI 2.0 oturum](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azure-cli
    C:\ az login --tenant <tenant>
    ``` 


3.  Yeni iş ortağı kimliğini bağlantı Kimliktir iş ortağı [Microsoft iş ortağı Network(MPN)](https://partner.microsoft.com/) , kuruluşunuzun kimliği.

     ```azure-cli
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini Al
```azure-cli
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini güncelleştir
```azure-cli
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>Bağlantılı iş ortağı Kimliğini Sil
```azure-cli
C:\ az managementpartner delete --partner-id 12345
``` 


## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

**Kimin iş ortağı Kimliğini bağlayabilir miyim?**

Müşteri'nin kaynak yöneten iş ortağı kuruluştan herhangi bir kullanıcı hesabına iş ortağı Kimliğini bağlayabilirsiniz.
  

**Bir iş ortağı kimliği bağlı sonra değiştirilebilir mi?**

Evet, bağlı iş ortağı kimliği değişti, eklenen veya kaldırılabilir.

**Ne bir kullanıcı bir hesabı birden çok müşteri kiracılar var?**

İş ortağı Kimliğini ve hesap arasındaki bağlantıyı, her bir müşterinin Kiracı için yapılır.  Her bir müşterinin Kiracı İş ortağı kimliği bağlamak zorunda.

**Diğer ortak veya müşteri düzenleyebilir veya iş ortağı Kimliğini bağlantısını kaldırmak?**

Bağlantı hesap düzeyinde ilişkili değil. Yalnızca Düzenle veya iş ortağı kimliğini bağlantısını Kaldır Müşteri ve diğer iş ortağı bağlantı iş ortağı kimliğini değiştirilemiyor 


