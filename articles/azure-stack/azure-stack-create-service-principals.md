---
title: Azure yığını için bir hizmet sorumlusu oluşturma | Microsoft Docs
description: Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilecek yeni bir hizmet sorumlusu oluşturmak açıklar.
services: azure-resource-manager
documentationcenter: na
author: mattbriggs
manager: femila
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/15/2018
ms.author: mabrigg
ms.openlocfilehash: c2e18f30e55007a0625a19258ec3745f64dc25da
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
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

Azure AD kimlik deposu olarak kullanarak Azure yığın dağıttıktan sonra Azure için gibi hizmet asıl adı oluşturabilirsiniz.  Bu bölümde, portal üzerinden adımların nasıl gerçekleştirileceğini gösterir.  Sahip olduğunuz onay [gereken Azure AD izinler](../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions) başlamadan önce.

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma
Bu bölümde, uygulamanızın temsil eden Azure AD'de uygulama (hizmet sorumlusu) oluşturma.

1. Oturum açtığınızda Azure hesabınız üzerinden [Azure portal](https://portal.azure.com).
2. Seçin **Azure Active Directory** > **uygulama kayıtlar** > **Ekle**   
3. Uygulama için bir ad ve URL sağlayın. Şunlardan birini seçin **Web uygulaması / API** veya **yerel** oluşturmak istediğiniz uygulama türü için. Değerleri ayarladıktan sonra Seç **oluşturma**.

Uygulamanız için bir hizmet sorumlusu oluşturdunuz.

### <a name="get-credentials"></a>Kimlik bilgilerini alma
Program aracılığıyla oturum açarken kimliği uygulamanız için ve bir Web uygulaması için kullandığınız / API, bir kimlik doğrulama anahtarı. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtlar** Active Directory'de uygulamanızı seçin.

2. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Uygulamalarda [örnek uygulamaları](#sample-applications) istemci kimliği olarak bu değer için bölümüne bakın

     ![istemci kimliği](./media/azure-stack-create-service-principal/image12.png)
3. Bir Web uygulaması için bir kimlik doğrulama anahtarı oluşturmak için / API, select **ayarları** > **anahtarları**. 

4. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Bu değeri kopyalayın çünkü daha sonra anahtarı alamazsınız. Uygulama kimliği ile uygulama olarak imzalamak için anahtar değeri sağlayın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

![kaydedilen anahtar](./media/azure-stack-create-service-principal/image15.png)


Tamamlandıktan sonra devam [uygulamanızı rol atama](azure-stack-create-service-principals.md#assign-role-to-service-principal).

## <a name="create-service-principal-for-ad-fs"></a>AD FS için hizmet sorumlusu oluşturma
AD FS ile Azure yığın dağıttıysanız, bir hizmet sorumlusu oluşturma, erişim için bir rol atayın ve Powershell'den bu kimliğini kullanarak oturum açmak için PowerShell'i kullanabilirsiniz.

Komut dosyası ayrıcalıklı uç noktasından bir ERCS sanal makinede çalıştırılır.


Gereksinimleri:
- Sertifikalı bir gereklidir.

**Parametreler**

Aşağıdaki bilgiler gereklidir Otomasyon parametreleri için giriş olarak:


|Parametre|Açıklama|Örnek|
|---------|---------|---------|
|Ad|SPN hesabının adı|Uygulamam|
|ClientCertificates|Sertifika nesneler dizisi|X509 sertifika|
|ClientRedirectUris<br>(İsteğe bağlı)|Uygulama yeniden yönlendirme URI'si|         |

**Örnek**

1. Yükseltilmiş bir Windows PowerShell oturumu açın ve aşağıdaki komutları çalıştırın:

   > [!NOTE]
   > Bu örnek, otomatik olarak imzalanan bir sertifika oluşturur. Bir üretim dağıtımında bu komutları çalıştırdığınızda, kullanmak istediğiniz sertifika için sertifika nesnesi almak için Get-sertifika kullanın.

   ```
   $creds = Get-Credential

   $session = New-PSSession -ComputerName <IP Address of ECRS> -ConfigurationName PrivilegedEndpoint -Credential $creds

   $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=testspn2" -KeySpec KeyExchange

   Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name 'MyApp' -ClientCertificates $using:cert}

   $session|remove-pssession

   ```

2. Otomasyon tamamlandıktan sonra SPN kullanmak için gerekli ayrıntıları görüntüler. 

   Örneğin:

   ```
   ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
   ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
   Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
   ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
   PSComputerName        : azs-ercs01
   RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
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
Aboneliğinizde kaynaklara erişmek için bir rol uygulamaya atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir rolleri hakkında bilgi edinmek için [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md).

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

[ADFS için kullanıcı ekleme](azure-stack-add-users-adfs.md)
[kullanıcı izinlerini Yönet](azure-stack-manage-permissions.md)
