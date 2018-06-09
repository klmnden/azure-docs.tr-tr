---
title: Azure Storage (Önizleme) erişmek için Azure CLI veya PowerShell komutları bir Azure AD kimlik altında çalıştırın | Microsoft Docs
description: Azure CLI ve PowerShell komutlarını Azure Storage kapsayıcıları ve Kuyruklar ve verileri üzerinde çalıştırmak için bir Azure AD kimlik oturum açma destekler. Bir erişim belirteci oturum için sağlanan ve arama işlemlerinin yetkilendirmek için kullanılır. İzinler için Azure AD kimlik atanan role göre değişir.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/30/2018
ms.author: tamram
ms.openlocfilehash: 98af46707485d1ab49e7d8c6fb1729e6edc6b2ff
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235874"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>CLI veya PowerShell (Önizleme) ile Azure depolama erişmek için bir Azure AD kimlik kullanın

Azure depolama etkinleştirmeniz oturum açma komut dosyası komut ve bir Azure Active Directory (Azure AD) kimlik altında çalıştırmak Azure CLI ve PowerShell Önizleme uzantıları sağlar. Azure AD kimlik bir kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir veya olabilir bir [yönetilen hizmet kimliği](../../active-directory/managed-service-identity/overview.md). Rol tabanlı erişim denetimi (RBAC) aracılığıyla Azure AD kimlik depolama kaynaklarına erişmek için izinler atayabilirsiniz. Azure Storage RBAC rolleri hakkında daha fazla bilgi için bkz: [RBAC (Önizleme) ile Azure Storage veri Yönet erişim hakları](storage-auth-aad-rbac.md).

Azure CLI veya PowerShell için Azure AD kimlik ile oturum açtığınızda, bu kimliği altında Azure Storage erişmek için bir erişim belirteci döndürülür. Bu belirteci sonra otomatik olarak CLI veya PowerShell ile Azure Storage'a karşı işlemleri yetkilendirmek için kullanılır. Desteklenen işlemler için artık bir hesap anahtarı ya da SAS belirteci komutuyla geçmesi gerekir.

> [!IMPORTANT]
> Bu önizleme yalnızca üretim dışı kullanılması amaçlanmıştır. Azure AD tümleştirmesi için Azure Storage genel kullanıma bildirilmiş kadar üretim hizmet düzeyi sözleşmelerine (SLA) kullanılamaz. Azure AD tümleştirme senaryonuz için henüz desteklenmiyorsa paylaşılan anahtar yetkilendirme veya SAS belirteci uygulamalarınızı kullanmaya devam. ' Nin önizlemesi hakkında ek bilgi için bkz: [Azure Active Directory'yi (Önizleme) kullanarak Azure Storage erişimi kimlik doğrulaması](storage-auth-aad.md).
>
> Önizleme sırasında RBAC rolü atamalarını yayılması beş dakika kadar sürebilir.
>
> Azure Storage ile Azure AD tümleştirme Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

## <a name="supported-operations"></a>Desteklenen işlemler

Önizleme uzantıları kapsayıcıları ve Kuyruklar işlemleri için desteklenir. Çağrı hangi işlemleri ile Azure CLI veya PowerShell oturum Azure AD kimlik için izinler bağlıdır. Azure Storage kapsayıcıları veya sıralara izinler rol tabanlı erişim denetimi (RBAC) atanır. Örneğin, bir veri okuyucu rolü kimliğine atanırsa, bir kapsayıcı veya kuyruktan veri okuma komut dosyası komutları çalıştırabilirsiniz. Ardından bir veri katkıda bulunan rolü kimliğine atanırsa, okuma, yazma ya da bir kapsayıcı veya sıra veya içerdikleri veriler Sil komut dosyası komutları çalıştırabilirsiniz. 

Her Azure depolama işlemi bir kapsayıcı veya sırası için gerekli izinleri hakkında daha fazla ayrıntı için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>CLI komutları bir Azure AD kimlik ile arama

Azure CLI için Önizleme uzantıyı yüklemek için:

1. Sonraki veya Azure CLI sürümü 2.0.32 yüklediğinizden emin olun. Çalıştırma `az --version` yüklü sürümünü denetlemek için.
2. Önizleme uzantıyı yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurecli
    az extension add -n storage-preview
    ```

Önizleme uzantısı yeni ekler `--auth-mode` parametresi desteklenen komutlar:

- Ayarlama `--auth-mode` parametresi `login` bir Azure AD kimlik kullanarak oturum.
- Ayarlama `--auth-mode` eski parametresi `key` hesabı için hiçbir kimlik doğrulama parametreleri için bir hesap anahtar if sorgulama girişimi için değer sağlanır. 

Örneğin, bir blob'a Azure CLI kullanarak bir Azure AD kimlik indirmek için ilk çalıştırma `az login`, komutuyla çağrısı `--auth-mode` kümesine `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

İle ilişkili ortam değişkeni `--auth-mode` parametresi `AZURE_STORAGE_AUTH_MODE`.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>PowerShell komutları bir Azure AD kimlik ile arama

Bir Azure AD kimlik bilgilerinizle oturum için Azure PowerShell kullanmak için:

1. Yüklü PowerShellGet en son sürümüne sahip olduğunuzdan emin olun. En son yüklemek için aşağıdaki komutu çalıştırın:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

2. Azure PowerShell önceki yüklemeleri kaldırın.
3. AzureRM yükleyin:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Önizleme modülünü yükleyin:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Çağrı [yeni AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) bir bağlam oluşturmak ve dahil etmek için cmdlet `-UseConnectedAccount` parametresi. 
6. Bir Azure AD kimlik cmdlet'iyle çağırmak için bağlam cmdlet'e.

Aşağıdaki örnek, bir Azure AD kimlik kullanarak BLOB'ları bir kapsayıcıda Azure PowerShell üzerinden listelemek nasıl gösterir: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure depolama için RBAC rolleri hakkında daha fazla bilgi için bkz: [RBAC (Önizleme) depolama verilerle Yönet erişim hakları](storage-auth-aad-rbac.md).
- Yönetilen hizmet kimliği Azure Storage ile kullanma hakkında bilgi edinmek için bkz: [bir Azure yönetilen hizmet kimliği (Önizleme) Azure AD ile kimlik doğrulama](storage-auth-aad-msi.md).
- Kapsayıcılar ve depolama uygulamalarınızdaki sıralarından erişim yetkisi vermek öğrenmek için bkz: [depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure depolama ekibi blog gönderisi, Azure BLOB'ları ve kuyrukları için Azure AD tümleştirme hakkında ek bilgi için bkz: [Azure depolama için Azure Önizleme AD Authentication Duyurusu](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
