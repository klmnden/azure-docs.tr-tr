---
title: Azure Resource Manager’a erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma
description: Linux VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure Resource Manager’a erişme işleminde size yol gösteren bir hızlı başlangıç.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: bryanla
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5b9e42da3ae4510c217cb29c211557e28a30074
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66236189"
---
# <a name="use-a-linux-vm-system-assigned-managed-identity-to-access-azure-resource-manager"></a>Azure Resource Manager’a erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu hızlı başlangıçta, Azure Resource Manager API'sine erişmek amacıyla, Linux sanal makinesi (VM) için sistem tarafından atanmış bir kimliği nasıl kullanacağınız gösterilmektedir. Azure kaynaklarına yönelik yönetilen kimlikler Azure tarafından otomatik olarak yönetilir ve kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure Resource Manager’da Kaynak Grubuna VM'niz için erişim verme 
> * VM kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="grant-your-vm-access-to-a-resource-group-in-azure-resource-manager"></a>Azure Resource Manager’da Kaynak Grubuna VM'niz için erişim verme 

Azure kaynakları için yönetilen kimlikler kullanıldığında kodunuz Azure AD kimlik doğrulamasını destekleyen kaynaklarda kimlik doğrulaması yapmak için erişim belirteçleri alabilir. Azure Resource Manager API’si Azure AD kimlik doğrulamasını destekler. Öncelikle bu VM’in kimliğine Azure Resource Manager’da bulunan bir kaynak için erişim izni vermemiz gerekir; bu durumda bu kaynak VM’in içinde yer aldığı Kaynak Grubudur.  

1. **Kaynak Grupları** sekmesine gidin.
2. Önceden oluşturduğunuz **Kaynak Grubu**’nu seçin.
3. Sol paneldeki **Erişim denetimi (IAM)** öğesine gidin.
4. VM’nize yeni rol ataması eklemek için **Ekle**’ye tıklayın. **Rol** olarak **Okuyucu**'yu seçin.
5. Sonraki açılan listede **Erişimin atanacağı hedef** olarak **Sanal Makine**’yi seçin.
6. Ardından, **Abonelik** açılan listesinde uygun aboneliğin listelendiğinden emin olun. **Kaynak Grubu** için de **Tüm kaynak grupları**'nı seçin.
7. Son olarak **Seç** alanındaki açılan listeden Linux Sanal Makinenizi seçin ve **Kaydet**’e tıklayın.

    ![Alternatif resim metni](media/msi-tutorial-linux-vm-access-arm/msi-permission-linux.png)

## <a name="get-an-access-token-using-the-vms-system-assigned-managed-identity-and-use-it-to-call-resource-manager"></a>VM’nin sistem tarafından atanan yönetilen kimliğini kullanarak erişim belirteci alma ve Resource Manager çağrısı yapmak için bunu kullanma 

Bu adımları tamamlamak için bir SSH istemciniz olmalıdır. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about)'ndeki SSH istemcisini kullanabilirsiniz. SSSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](../../virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Portalda Linux VM’nize gidin ve **Genel Bakış**’ta **Bağlan**’a tıklayın.  
2. Tercih ettiğiniz SSH istemcisiyle VM’ye bağlanmak için **Bağlan**’ı seçin. 
3. Terminal penceresinde kullanarak `curl`, Azure Resource Manager için bir erişim belirteci almak Azure kaynaklarını uç noktası için yönetilen yerel kimlikler için bir istek oluşturun.  
 
    `curl` İsteği erişim belirteci aşağıda verilmiştir.  
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' -H Metadata:true   
    ```
    
    > [!NOTE]
    > "Resource" parametre değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir.  Resource Manager kaynak kimliğinin kullanılması durumunda, URI'nin sonundaki eğik çizgiyi de eklemelisiniz. 
    
    Yanıtta, Azure Resource Manager’a erişmek için ihtiyacınız olan erişim belirteci vardır. 
    
    Yanıt:  

    ```bash
    {"access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
    ```
    
    Azure Resource Manager’a erişmek ve örneğin daha önceden bu VM’ye erişim verdiğiniz Kaynak Grubunun ayrıntılarını okumak için bu erişim belirtecini kullanabilirsiniz. \<SUBSCRIPTION ID\>, \<RESOURCE GROUP\>, ve \<ACCESS TOKEN\> değerlerini daha önce oluşturduğunuz değerlerle değiştirin. 
    
    > [!NOTE]
    > URL büyük/küçük harfe duyarlıdır; dolayısıyla daha önce Kaynak Grubunu adlandırırken kullandığınız büyük/küçük harf düzenini kullanmaya dikkat edin ("resourceGroups" adındaki büyük "G" harfine de dikkat edin).  
    
    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    Belirli Kaynak Grubu bilgileriyle gelen yanıt:    
    ```bash
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}} 
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Resource Manager API'sine erişmek amacıyla, sistem tarafından atanmış bir yönetilen kimliği nasıl kullanacağınızı öğrendiniz.  Azure Resource Manager hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)
