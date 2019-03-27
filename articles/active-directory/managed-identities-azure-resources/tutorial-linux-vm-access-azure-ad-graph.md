---
title: Azure AD Graph API'ye erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma
description: Linux VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure AD Graph API'ye erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 481cb560daa26e59de2c78cc64bab9fb168eed58
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445407"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Öğretici: Azure AD Graph API'ye erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice.md)]

Bu öğreticide, grup üyeliklerini almak için Azure AD Graph API’ye erişmek amacıyla, Linux sanal makinesi (VM) için sistem tarafından atanmış bir yönetilen kimliği nasıl kullanacağınız gösterilmektedir. Azure kaynaklarına yönelik yönetilen kimlikler Azure tarafından otomatik olarak yönetilir ve kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmanıza olanak tanır.  

Bu öğreticide, VM kimliğinizin Azure AD gruplarındaki üyeliğini sorgulayacaksınız. Grup bilgileri genellikle yetkilendirme kararlarında kullanılır. Arka planda, sanal makinenizin yönetilen kimliği Azure AD’de bir **Hizmet Sorumlusu** ile temsil edilir. 

> [!div class="checklist"]
> * Azure AD'ye Bağlanma
> * VM kimliğinizi Azure AD'de bir gruba ekleme 
> * VM kimliğine Azure AD Graph erişimi verme 
> * VM kimliğini kullanarak erişim belirteci alma ve Azure AD Graph çağrısı yapmak için kullanma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Azure CLI’nin en son sürümünü yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli)

- Bir VM kimliğine Azure AD Graph erişimi vermek için hesabınıza Azure AD’de **Global Admin** rolünün atanmış olması gerekir.

## <a name="connect-to-azure-ad"></a>Azure AD'ye Bağlanma

VM’yi bir gruba atamak ve VM’ye grup üyeliklerini alma izni vermek için Azure AD’ye bağlanmanız gerekir.

```cli
az login
```

## <a name="add-your-vms-identity-to-a-group-in-azure-ad"></a>VM kimliğinizi Azure AD'de bir gruba ekleme

Linux VM’de sistem tarafından atanmış yönetilen kimliği etkinleştirdiğinizde, Azure AD’de bir hizmet sorumlusu oluşturulmuştur.  VM’yi bir gruba eklemeniz gerekir. Sanal makinenizi Azure AD’de bir gruba ekleme yönergeleri için aşağıdaki makaleye bakın:

- [Grup üyeleri ekleme](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add)

## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Sanal makinenize Azure AD Graph API erişimi verme

Azure kaynakları için yönetilen kimlikler kullanıldığında kodunuz Azure AD kimlik doğrulamasını destekleyen kaynaklarda kimlik doğrulaması yapmak için erişim belirteçleri alabilir. Microsoft Azure AD Graph API, Azure AD kimlik doğrulamasını destekler. Bu adımda, VM kimliğinizin hizmet sorumlusuna grup üyeliklerini sorgulayabilmesi için Azure AD Graph erişimi vereceksiniz. Hizmet sorumlularına Microsoft veya Azure AD Graph erişimi, **Uygulama İzinleri** aracılığıyla verilir. Vermeniz gereken uygulama izninin türü, MS veya Azure AD Graph’ta erişmek istediğiniz varlığa bağlıdır.

Bu öğreticide VM kimliğinize `Directory.Read.All` uygulama iznini kullanarak grup üyeliğini sorgulama olanağı vereceksiniz. Bu izni vermek için Azure AD'de Genel Yönetici rolü atanmış bir kullanıcı hesabı gerekir. Normalde Azure portalda uygulamanızın kaydını ziyaret edip izni oraya ekleyerek bir uygulama izni verebilirsiniz. Ancak, Azure kaynakları için yönetilen kimlikler uygulama nesnelerini Azure AD'ye kaydetmez, yalnızca hizmet sorumlularını kaydeder. Uygulama iznini kaydetmek için Azure AD PowerShell komut satırı aracını kullanırsınız. 

Azure AD Graph:
- Hizmet sorumlusu uygulama kimliği (uygulama izni verilirken kullanılır): 00000002-0000-0000-c000-000000000000
- Kaynak Kimliği (Azure kaynakları için yönetilen kimliklerden erişim belirteci istenirken kullanılır): https://graph.windows.net
- İzin kapsam başvurusu: [Azure AD Graph izinleri başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-curl"></a>CURL kullanarak uygulama izinleri verme

1. CURL isteğinde bulunmak için belirteç alma:

   ```cli
   az account get-access-token --resource "https://graph.windows.net/"
   ```

2. Sanal makinenizin `objectId` değerini alıp not etmeniz gerekir. Sonraki adımlarda sanal makineye grup üyeliğini okuma izinleri vermek için kullanılır. `<ACCESS TOKEN>` değerini önceki adımda aldığınız erişim belirteciyle değiştirin.

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals?$filter=startswith%28displayName%2C%27myVM%27%29&api-version=1.6' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

3. Azure AD Graph 00000002-0000-0000-c000-000000000000 appID değerini kullanarak, `odata.type: Microsoft.DirectoryServices.ServicePrincipal` için `objectId` ve `Directory.Read.All` uygulama rolü izni için `id` değerini alın ve not edin.  `<ACCESS TOKEN>` değerini daha önce aldığınız erişim belirteciyle değiştirin.

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals?api-version=1.6&%24filter=appId%20eq%20'00000002-0000-0000-c000-000000000000'" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Yanıt:

   ```json
   "odata.metadata":"https://graph.windows.net/myorganization/$metadata#directoryObjects",
   "value":[
      {
         "odata.type":"Microsoft.DirectoryServices.ServicePrincipal",
         "objectType":"ServicePrincipal",
         "objectId":"81789304-ff96-402b-ae73-07ec0db26721",
         "deletionTimestamp":null,
         "accountEnabled":true,
         "addIns":[
         ],
         "alternativeNames":[
         ],
         "appDisplayName":"Windows Azure Active Directory",
         "appId":"00000002-0000-0000-c000-000000000000",
         "appOwnerTenantId":"f8cdef31-a31e-4b4a-93e4-5f571e91255a",
         "appRoleAssignmentRequired":false,
         "appRoles":[
            {
               "allowedMemberTypes":[
                  "Application"
               ],
               "description":"Allows the app to read data in your company or school directory, such as users, groups, and apps.",
               "displayName":"Read directory data",
               "id":"5778995a-e1bf-45b8-affa-663a9f3f4d04",
               "isEnabled":true,
               "value":"Directory.Read.All"
            },
            {
               //other appRoles values
            }
   ``` 

4. Şimdi, Azure AD Graph API'yi kullanarak sanal makinenin hizmet sorumlusuna Azure AD dizin nesnelerini okuma erişimi verin.  `id` değeri `Directory.Read.All` uygulama rolü izninin, `resourceId` ise `odata.type:Microsoft.DirectoryServices.ServicePrincipal` hizmet sorumlusunun `objectId` değeridir (önceki adımda not ettiğiniz değerler).

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals/<VM Object ID>/appRoleAssignments?api-version=1.6" -X POST -d '{"id":"5778995a-e1bf-45b8-affa-663a9f3f4d04","principalId":"<VM Object ID>","resourceId":"81789304-ff96-402b-ae73-07ec0db26721"}'-H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ``` 
 
## <a name="get-an-access-token-using-the-vms-identity-to-call-azure-ad-graph"></a>VM kimliğini kullanarak erişim belirteci alma ve Azure AD Graph çağrısı yapma 

Bu adımları tamamlamak için bir SSH istemciniz olmalıdır. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about)'ndeki SSH istemcisini kullanabilirsiniz. SSSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](../../virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Portalda Linux VM’nize gidin ve **Genel Bakış**’ta **Bağlan**’a tıklayın.  
2. Tercih ettiğiniz SSH istemcisiyle VM’ye bağlanmak için **Bağlan**’ı seçin. 
3. Terminal penceresinde CURL, kullanarak Azure AD Graph için bir erişim belirteci almak Azure kaynaklarını uç noktası için yönetilen yerel kimlikler için bir istek olun.  
    
   ```bash
   curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -H Metadata:true
   ```    
   Yanıtta, Azure AD Graph’a erişmek için ihtiyacınız olan erişim belirteci vardır.

   Yanıt:

   ```bash
   {
       "access_token":"eyJ0eXAiOiJKV...",
       "expires_in":"3599",
       "expires_on":"1519622535",
       "not_before":"1519618635",
       "resource":"https://graph.windows.net",
       "token_type":"Bearer"
   }
   ```

4. Sanal makinenizin hizmet sorumlusuna ait nesne kimliğini (önceki adımlarda aldığınız değer) kullanarak, grup üyeliklerini almak üzere Azure AD Graph API’yi sorgulayabilirsiniz. Değiştirin `<OBJECT-ID>` VM'NİZİN hizmet sorumlusu nesne kimliği ve `<ACCESS-TOKEN>` daha önce alınan erişim belirteci ile:

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals/<OBJECT-ID>/getMemberGroups?api-version=1.6' -X POST -d "{\"securityEnabledOnly\": false}" -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS-TOKEN>"
   ```

   Yanıt:

   ```bash   
   Content : {"odata.metadata":"https://graph.windows.net/myorganization/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure AD Graph'a erişmek için Linux VM sistem tarafından atanan yönetilen kimlik kullanmayı öğrendiniz.  Azure AD Graph hakkında daha fazla bilgi için bkz.:

>[!div class="nextstepaction"]
>[Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)
