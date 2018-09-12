---
title: Azure Resource Manager’a erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma
description: Windows VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure Resource Manager’a erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: d978c5797766f62141d07e8e0a418863881bc13f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338060"
---
# <a name="use-a-windows-vm-system-assigned-managed-identity-to-access-resource-manager"></a>Resource Manager’a erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu hızlı başlangıçta, sistem tarafından atanmış yönetilen kimliğin etkinleştirildiği bir Windows sanal makinesini kullanarak Azure Resource Manager API'sine nasıl erişeceğiniz gösterilmektedir. Azure kaynaklarına yönelik yönetilen kimlikler Azure tarafından otomatik olarak yönetilir ve kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"] 
> * Azure Resource Manager’da Kaynak Grubuna VM'niz için erişim verme 
> * VM kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Azure portal'da oturum açma](https://portal.azure.com)

- [Windows sanal makinesi oluşturma](/azure/virtual-machines/windows/quick-create-portal)

- [Sanal makinenizde sistem tarafından atanan yönetilen kimlik etkinleştirme](/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm#enable-system-assigned-identity-on-an-existing-vm)

## <a name="grant-your-vm-access-to-a-resource-group-in-resource-manager"></a>Resource Manager’da kaynak grubuna VM'niz için erişim verme
Azure kaynakları için yönetilen kimlikler kullanıldığında kodunuz Azure AD kimlik doğrulamasını destekleyen kaynaklarda kimlik doğrulaması yapmak için erişim belirteçleri alabilir.  Azure Resource Manager Azure AD kimlik doğrulamasını destekler.  Öncelikle bu VM’in sistem tarafından atanan yönetilen kimliğine Resource Manager’da bulunan bir kaynak için erişim izni vermemiz gerekir; bu durumda bu kaynak VM’nin içinde yer aldığı Kaynak Grubudur.  

1.  **Kaynak Grupları** sekmesine gidin. 
2.  **Windows VM**’niz için oluşturduğunuz **Kaynak Grubu**’nu seçin. 
3.  Sol paneldeki **Erişim denetimi (IAM)** öğesine gidin. 
4.  **Windows VM**’nize yeni bir rol ataması eklemek için **Ekle**’ye tıklayın.  **Rol** olarak **Okuyucu**'yu seçin. 
5.  Sonraki açılan listede **Erişimin atanacağı hedef** olarak **Sanal Makine** seçeneğini ayarlayın. 
6.  Ardından, **Abonelik** açılan listesinde uygun aboneliğin listelendiğinden emin olun. **Kaynak Grubu** için de **Tüm kaynak grupları**'nı seçin. 
7.  Son olarak **Seç** altındaki açılan listeden Windows VM'nizi seçin ve **Kaydet**’e tıklayın.

    ![Alternatif resim metni](media/msi-tutorial-windows-vm-access-arm/msi-windows-permissions.png)

## <a name="get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-azure-resource-manager"></a>VM’nin sistem tarafından atanan yönetilen kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma 

Bu bölümde **PowerShell** kullanmanız gerekecektir.  **PowerShell** yüklü değilse [buradan](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1) indirin. 

1.  Portalda, **Sanal Makineler**'e ve Windows sanal makinenize gidin, ardından **Genel Bakış**'ta **Bağlan**'a tıklayın. 
2.  Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3.  Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda **PowerShell**'i açın. 
4.  PowerShell’in Invoke-WebRequest komutunu kullanarak, Azure kaynakları uç noktası için yerel yönetilen kimliğe Azure Resource Manager için erişim belirteci alma isteğinde bulunun.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > "Resource" parametre değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    
    Ardından, $response nesnesinde JavaScript Nesne Gösterimi (JSON) biçimlendirilmiş dizesi olarak depolanan tam yanıtı ayıklayın. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Ardından, yanıttan erişim belirtecini ayıklayın.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
    
    Son olarak erişim belirteci kullanarak Azure Resource Manager çağrısı yapın. Bu örnekte, Azure Resource Manager çağrısı yapmak ve Yetkilendirme üst bilgisine erişim belirtecini eklemek için PowerShell'in Invoke-WebRequest komutunu da kullanıyoruz.
    
    ```powershell
    (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $ArmToken"}).content
    ```
    > [!NOTE] 
    > URL büyük/küçük harfe duyarlıdır; dolayısıyla daha önce Kaynak Grubunu adlandırırken kullandığınız büyük/küçük harf düzenini kullanmaya dikkat edin ("resourceGroups" adındaki büyük "G" harfine de dikkat edin).
        
    Aşağıdaki komut Kaynak Grubu ayrıntılarını döndürür:

    ```powershell
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}}
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Resource Manager API'sine erişmek amacıyla, sistem tarafından atanmış bir yönetilen kimliği nasıl kullanacağınızı öğrendiniz.  Azure Resource Manager hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

