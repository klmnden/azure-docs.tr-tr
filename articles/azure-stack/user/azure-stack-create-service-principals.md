---
title: "Azure yığını için bir hizmet sorumlusu oluşturma | Microsoft Docs"
description: "Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilecek yeni bir hizmet sorumlusu oluşturmak açıklar."
services: azure-resource-manager
documentationcenter: na
author: heathl17
manager: byronr
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 058b01a37e2858801895fd22cf73dd6bd342ca04
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="provide-applications-access-to-azure-stack"></a>Azure yığın uygulama erişim sağlamak

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir uygulama dağıtmak veya Azure yığınında kaynakları aracılığıyla Azure Kaynak Yöneticisi'ni yapılandırmak için erişim gerektiğinde, uygulamanız için bir kimlik bilgisi olan bir hizmet sorumlusu oluşturun.  Ardından, bu hizmet sorumlusunun yalnızca gerekli izinleri atayabilirsiniz.  

Örnek olarak, Azure Resource Manager Azure kaynaklarını stok için kullandığı yapılandırma yönetim aracı olabilir.  Bu senaryoda, bir hizmet sorumlusu oluşturmak, bu hizmet sorumlusunun okuyucu rolüne verin ve yapılandırma Yönetim Aracı'na salt okunur erişimi sınırlamak. 

Hizmet sorumluları çünkü uygulama kendi kimlik bilgileri altında çalışırken için tercih edilir:

* Hizmet sorumlusu kendi hesap izinlerini farklı olan izinleri atayabilirsiniz. Genellikle, bu izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.
* Sizin Sorumluluklarınız uygulamanın kimlik bilgilerini değiştirirseniz gerekmez.
* Katılımsız betik yürütülürken kimlik doğrulaması otomatikleştirmek için bir sertifika kullanabilirsiniz.  

## <a name="getting-started"></a>Başlarken

Nasıl Azure yığın dağıttığınız bağlı olarak, bir hizmet asıl oluşturmaya başlayın.  Bu belge, her ikisi için de bir hizmet sorumlusu oluşturma size rehberlik eder [Azure Active Directory (Azure AD)](azure-stack-create-service-principals.md#create-service-principal-for-azure-ad) ve [Active Directory Federasyon Services(AD FS)](azure-stack-create-service-principals.md#create-service-principal-for-ad-fs).  Hizmet sorumlusu oluşturduktan sonra AD FS ile Azure Active Directory için ortak adımlar kümesi için kullanılan [temsilci izinleri](azure-stack-create-service-principals.md#assign-role-to-service-principal) rolü.     

## <a name="create-service-principal-for-azure-ad"></a>Azure AD hizmet sorumlusu oluşturma

Azure AD kimlik deposu olarak kullanarak Azure yığın dağıttıktan sonra Azure için gibi hizmet asıl adı oluşturabilirsiniz.  Bu bölümde, portal üzerinden adımların nasıl gerçekleştirileceğini gösterir.  Sahip olduğunuz onay [gereken Azure AD izinler](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) başlamadan önce.

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma
Bu bölümde, uygulamanızın temsil edecek Azure AD'de uygulama (hizmet sorumlusu) oluşturma.

1. Oturum açtığınızda Azure hesabınız üzerinden [Azure portal](https://portal.azure.com).
2. Seçin **Azure Active Directory** > **uygulama kayıtlar** > **Ekle**   
3. Uygulama için bir ad ve URL belirtin. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü için. Değerleri ayarladıktan sonra Seç **oluşturma**.

Uygulamanız için bir hizmet sorumlusu oluşturdunuz.

### <a name="get-credentials"></a>Kimlik bilgilerini alma
Program aracılığıyla oturum açarken uygulamanız ve bir kimlik doğrulama anahtarı kimliği kullanın. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtlar** Active Directory'de uygulamanızı seçin.

2. Kopya **uygulama kimliği** ve uygulama kodunuzda saklayın. Uygulamalarda [örnek uygulamaları](#sample-applications) istemci kimliği olarak bu değer için bölümüne bakın.

     ![istemci kimliği](./media/azure-stack-create-service-principal/image12.png)
3. Bir kimlik doğrulama anahtarı oluşturmak için seçin **anahtarları**.

4. Anahtar ve bir süre anahtarı için bir açıklama belirtin. İşiniz bittiğinde, seçin **kaydetmek**.

Anahtar kaydedildikten sonra anahtar değeri görüntülenir. Daha sonra anahtar almak mümkün olmadığı için bu değeri kopyalayın. Uygulama kimliği ile uygulama olarak imzalamak için anahtar değeri sağlayın. Burada, uygulamanızın alabildiği anahtar değer deposu.

![anahtar kaydedildi](./media/azure-stack-create-service-principal/image15.png)


Tamamlandıktan sonra devam [uygulamanızı rol atama](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu oluşturma
AD FS ile Azure yığın dağıttıysanız, bir hizmet sorumlusu oluşturma, erişim için bir rol atayın ve Powershell'den bu kimliğini kullanarak oturum açmak için PowerShell'i kullanabilirsiniz.

### <a name="before-you-begin"></a>Başlamadan önce

[Yerel bilgisayarınıza Azure yığın ile çalışmak için gereken araçları indirin.](azure-stack-powershell-download.md)

### <a name="import-the-identity-powershell-module"></a>Kimlik PowerShell modülünü içeri aktarın
Araçlar indirdikten sonra indirilen klasöre gidin ve aşağıdaki komutu kullanarak kimlik PowerShell modülünü içeri aktarın:

```PowerShell
Import-Module .\Identity\AzureStack.Identity.psm1
```

Modül içe aktardığınızda, "AzureStack.Connect.psm1 dijital olarak imzalanmamış. bildiren bir hata iletisi alabilirsiniz Komut dosyası sistemde çalıştırmaz". Bu sorunu gidermek için komut dosyasını yükseltilmiş bir PowerShell oturumunda aşağıdaki komutla çalıştırmaya izin vermek için yürütme ilkesi ayarlayabilirsiniz:

```PowerShell
Set-ExecutionPolicy Unrestricted
```

### <a name="create-the-service-principal"></a>Hizmet sorumlusunu oluşturma
Bir hizmet sorumlusu aşağıdaki komutu çalıştırarak güncelleştirmek emin oluşturabileceğiniz *DisplayName* parametre:
```powershell
$servicePrincipal = New-AzSADGraphServicePrincipal `
 -DisplayName "<YourServicePrincipalName>" `
 -AdminCredential $(Get-Credential) `
 -AdfsMachineName "AZS-ADFS01" `
 -Verbose
```
### <a name="assign-a-role"></a>Rol atama
Hizmet sorumlusu oluşturulduktan sonra şunları yapmalısınız [rol atama](azure-stack-create-service-principals.md#assign-role-to-service-principal)

### <a name="sign-in-through-powershell"></a>PowerShell aracılığıyla oturum açın
Bir rolü atadığınız sonra size Azure hizmet sorumlusu aşağıdaki komutla kullanarak yığınına oturum açabilirsiniz:

```powershell
Add-AzureRmAccount -EnvironmentName "<AzureStackEnvironmentName>" `
 -ServicePrincipal `
 -CertificateThumbprint $servicePrincipal.Thumbprint `
 -ApplicationId $servicePrincipal.ApplicationId ` 
 -TenantId $directoryTenantId
```

## <a name="assign-role-to-service-principal"></a>Hizmet sorumlusuna rolü atama
Aboneliğinizde kaynaklara erişmek için bir rol uygulamaya atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir rolleri hakkında bilgi edinmek için [RBAC: yerleşik roller](../../active-directory/role-based-access-built-in-roles.md).

Abonelik, kaynak grubu ya da kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam devralınan izinleri. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubu ve içerdiği tüm kaynaklar okuyabilir anlamına gelir.

1. Azure yığın Portalı'nda uygulamaya atamak istediğiniz kapsam düzeyine gidin. Örneğin, abonelik kapsamında bir rol atamak için seçin **abonelikleri**. Bunun yerine, bir kaynak grubu veya kaynak seçebilirsiniz.

2. Belirli bir abonelik (kaynak grubu veya kaynak) uygulama atamak için seçin.

     ![atama için abonelik seçin](./media/azure-stack-create-service-principal/image16.png)

3. Seçin **erişim denetimi (IAM)**.

     ![erişim seçin](./media/azure-stack-create-service-principal/image17.png)

4. **Add (Ekle)** seçeneğini belirleyin.

5. Uygulamaya atamak istediğiniz rolü seçin.

6. Uygulamanız için arayın ve seçin.

7. Seçin **Tamam** rol atama tamamlamak için. Listenin uygulamanızda bu kapsam için bir rolüne atanan kullanıcıların bakın.

Bir hizmet sorumlusu oluşturulur ve bir rolü atanmış göre bu Azure yığın kaynaklara erişmek için uygulamanızdaki kullanmaya başlayabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)