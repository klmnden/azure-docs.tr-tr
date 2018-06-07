---
title: Azure kimlik doğrulaması bir Azure VM AD'den yönetilen hizmet kimliği (Önizleme) | Microsoft Docs
description: Azure kimlik doğrulaması bir Azure VM AD'den yönetilen hizmet kimliği (Önizleme).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 83d3a2d973604e3b8a709b24cabcb3abba1e304c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660999"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Azure yönetilen hizmet kimliği (Önizleme) Azure AD ile kimlik doğrulaması

Azure Storage ile Azure Active Directory (Azure AD) kimlik doğrulamasını destekler [yönetilen hizmet kimliği](../../active-directory/managed-service-identity/overview.md). Yönetilen hizmet kimliği (MSI) Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir kimlik sağlar. MSI Azure sanal makineler, işlev uygulamalarının, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalardan Azure depolama alanına kimlik doğrulaması yapmak için kullanabilirsiniz. MSI kullanarak ve Azure AD kimlik doğrulama power yararlanarak, uygulamalarınızı bulutta çalışan bilgileriyle depolama önleyebilirsiniz.  

Yönetilen hizmet kimliği depolama kapsayıcıları veya sıralara için izinleri MSI depolama izinleri kapsayan bir RBAC rolü atayın. Depolama RBAC rolleri hakkında daha fazla bilgi için bkz: [RBAC (Önizleme) depolama verilerle Yönet erişim hakları](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> Bu önizleme yalnızca üretim dışı kullanılması amaçlanmıştır. Azure AD tümleştirmesi için Azure Storage genel kullanıma bildirilmiş kadar üretim hizmet düzeyi sözleşmelerine (SLA) kullanılamaz. Azure AD tümleştirme senaryonuz için henüz desteklenmiyorsa paylaşılan anahtar yetkilendirme veya SAS belirteci uygulamalarınızı kullanmaya devam. ' Nin önizlemesi hakkında ek bilgi için bkz: [Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması](storage-auth-aad.md).
>
> Önizleme sırasında RBAC rolü atamalarını yayılması beş dakika kadar sürebilir.

Bu makalede Azure Storage MSI ile bir Azure sanal makineden kimlik doğrulaması kullanmayı gösterir.  

## <a name="enable-msi-on-the-vm"></a>MSI VM'de etkinleştir

Azure depolama alanına sanal makineden kimliğini doğrulamak için MSI kullanabilmeniz için önce VM üzerinde MSI etkinleştirmeniz gerekir. MSI etkinleştirme hakkında bilgi edinmek için Bu makalelerden birine bakın:

- [Azure Portal](https://docs.microsoft.com/en-us/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-service-identity/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-service-identity/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-service-identity/qs-configure-template-windows-vm.md)
- [Azure SDK'ları](../../active-directory/managed-service-identity/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>Bir MSI erişim belirteci alma

MSI ile kimlik doğrulaması yapmak için uygulama veya betik bir MSI erişim belirteci edinmeniz gerekir. Bir erişim belirteci alma hakkında bilgi edinmek için [belirteci alımı için bir Azure VM yönetilen hizmet kimliği (MSI) kullanmayı](../../active-directory/managed-service-identity/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>.NET kodu örneği: blok blobu oluşturma

Kod örneği, bir MSI erişim belirtecine sahip olduğunuzu varsayar. Erişim belirteci bir blok blobu oluşturmak için Yönetilen hizmet kimliği yetkilendirmek için kullanılır.

### <a name="add-references-and-using-statements"></a>Başvurular ekleyin ve using deyimleri  

Visual Studio'da Azure Storage istemci kitaplığı önizleme sürümünü yükleyin. Gelen **Araçları** menüsünde, select **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Konsola aşağıdaki komutu yazın:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Aşağıdaki using deyimlerini kodunuz için:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>MSI erişim belirtecinden kimlik bilgileri oluşturun

Blok blobu oluşturmak üzere kullanmanız **TokenCredentials** Önizleme paketi tarafından sağlanan sınıfı. Yeni bir örneğini oluşturmak **TokenCredentials**, daha önce edindiğiniz MSI erişim belirteci geçen:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Azure Storage ile Azure AD tümleştirme Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure depolama için RBAC rolleri hakkında daha fazla bilgi için bkz: [RBAC (Önizleme) depolama verilerle Yönet erişim hakları](storage-auth-aad-rbac.md).
- Kapsayıcılar ve depolama uygulamalarınızdaki sıralarından erişim yetkisi vermek öğrenmek için bkz: [depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure CLI ve PowerShell ile bir Azure AD kimlik günlüğe kaydetme hakkında bilgi edinmek için [CLI veya PowerShell (Önizleme) ile Azure depolama alanına erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).
- Azure depolama ekibi blog gönderisi, Azure BLOB'ları ve kuyrukları için Azure AD tümleştirme hakkında ek bilgi için bkz: [Azure depolama için Azure Önizleme AD Authentication Duyurusu](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
