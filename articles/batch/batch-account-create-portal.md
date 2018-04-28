---
title: Azure portalını kullanarak Batch hesabı oluşturma | Microsoft Docs
description: Büyük ölçekli paralel iş yükleri bulutta çalıştırmak için Azure portalda bir Azure Batch hesabı oluşturmayı öğrenin
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6374e49f3f682d022613e3e5244d273337213311
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Azure portalıyla Batch hesabı oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Yönetimi .NET](batch-management-dotnet.md)
>
>

[Azure portalında][azure_portal] Azure Batch hesabı oluşturma hakkında bilgi alın ve işlem senaryonuza uygun hesap özelliklerini seçin. Erişim anahtarları ve hesap URL’leri gibi önemli hesap özelliklerini nerede bulabileceğinizi öğrenin.

Batch hesapları ve senaryoları hakkında arka plan bilgileri için bkz. [özelliğe genel bakış](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Batch hesabı oluşturma

> [!NOTE]
> Batch hesabı oluştururken genelde varsayılan **Batch hizmeti** modunu seçmeniz gerekir. Bu mod kullanıldığında havuzlar Azure tarafından yönetilen aboneliklerde, arka planda ayrılır. Kullanılması artık çoğu senaryo için önerilmeyen alternatif **kullanıcı aboneliği** modunda bir havuz oluşturulduğunda Batch VM'leri ve diğer kaynaklar doğrudan aboneliğinizde oluşturulur. Kullanıcı aboneliği modunda bir Batch hesabı oluşturmak için aboneliğinizi Azure Batch hizmetine kaydetmeniz ve hesabı bir Azure Key Vault ile ilişkilendirmeniz de gerekir.

1. [Azure portalında][azure_portal] oturum açın.
2. **Kaynak oluştur**'a tıklayın ve Market içinde **Batch Hizmeti** araması yapın.

    ![Market’te Batch][marketplace_portal]
3. **Batch Hizmeti**'ni seçin, **Oluştur**'a tıklayın ve **Yeni Batch hesabı ayarları** yazın. Aşağıdaki ayrıntılara bakın.

    ![Batch hesabı oluşturma][account_portal]

    a. **Hesap adı**: Seçtiğiniz ad, hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki **Konum** bölümüne bakın). Hesap adı yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    b. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.

    c. **Havuz ayırma modu**: Bu seçenek görünürse varsayılan **Batch hizmeti**'ni kabul edin.

    c. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubu seçebilir ya da isterseniz yeni bir tane oluşturabilirsiniz.

    d. **Konum**: Batch hesabının oluşturulacağı bir Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.

    e. **Depolama hesabı** (isteğe bağlı): Batch hesabınızla ilişkilendireceğiniz bir Azure Depolama hesabı. Çoğu Batch hesabı için önerilen seçenek budur. Ayrıntılar için bu makalenin sonraki bölümlerinde [Bağlı Azure Depolama hesabı](#linked-azure-storage-account) konusuna bakın.

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.



## <a name="view-batch-account-properties"></a>Batch hesabı özelliklerini görüntüleme
Hesap oluşturulduktan sonra üzerine tıklayarak ayarlarına ve özelliklerine erişebilirsiniz. Sol menüyü kullanarak tüm hesap ayarlarına ve özelliklerine erişebilirsiniz.

![Azure portalında Batch hesabı sayfası][account_blade]

* **Batch hesabı URL'si**: [Batch API'leri](batch-apis-tools.md#azure-accounts-for-batch-development) ile uygulama geliştirirken, Batch kaynaklarınıza erişebilmeniz için bir hesap URL'si gereklidir. Batch hesabı URL’sinin biçimi aşağıdaki gibidir:

    `https://<account_name>.<region>.batch.azure.com`

![Portalda Batch hesabı URL’si][account_url]

* **Erişim anahtarları**: Batch hesabınıza uygulamanızdan yapılan erişimlere kimlik doğrulaması gerçekleştirmek için bir hesap erişim anahtarına ihtiyaç duyabilirsiniz. (Batch, Azure Active Directory kimlik doğrulamasını da destekler.)

    Erişim anahtarlarını görüntülemek veya yeniden oluşturmak için **Anahtarlar**'ı seçin.

    ![Azure portalında Batch hesabı anahtarları][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Bağlı Azure Storage hesabı

Bir Azure Depolama hesabını Batch hesabınıza bağlayabilirsiniz. Bu seçenek birçok senaryoda yardımcı olacaktır. Batch'in [uygulama paketleri](batch-application-packages.md) özelliği, [Batch Dosya Kuralları .NET](batch-task-output.md) kitaplığının yaptığı gibi Azure Blob depolama kullanır. Bu isteğe bağlı özellikler Batch görevlerinizin çalıştırdığı uygulamaları dağıtmanıza ve oluşturduğu verileri kalıcı hale getirmeniz yardımcı olur.

Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](batch-api-basics.md#azure-storage-account).

![Depolama hesabı oluşturma][storage_account]

> [!NOTE]
> Bağlantılı bir Depolama hesabının erişim anahtarlarını yeniden oluştururken dikkatli olun. Yalnızca bir Storage hesap anahtarını yeniden oluşturun ve bağlı Storage hesabı sayfasında **Anahtarları Eşitle**’ye tıklayın. Anahtarların havuzlarınızdaki işlem düğümlerine yayılması için beş dakika bekleyin, ardından gerekirse diğer anahtarı yeniden oluşturup eşitleyin. İki anahtarı da aynı anda oluşturursanız işlem düğümleriniz iki anahtarı da eşitleyemez ve anahtarlar Storage hesabına erişimi kaybederler.
>
>

![Depolama hesabı anahtarlarını yeniden oluşturma][4]

## <a name="additional-configuration-for-user-subscription-mode"></a>Kullanıcı aboneliği modu için ek yapılandırma

Kullanıcı aboneliği modunda bir Batch hesabı oluşturmayı seçerseniz, hesabı oluşturmadan önce aşağıdaki ek adımları gerçekleştirin.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Azure Batch hizmetinin aboneliğe erişmesine izin verme (tek seferlik işlem)
Kullanıcı aboneliği modunda ilk Batch hesabınızı oluştururken, aboneliğinizi Batch hizmetine kaydetmeniz gerekir. (Bunu daha önce yaptıysanız sonraki bölüme atlayın.)

1. [Azure portalında][azure_portal] oturum açın.

2. **Diğer Hizmetler** > **Abonelikler**’e ve ardından Batch hesabı için kullanmak istediğiniz aboneliğe tıklayın.

3. **Abonelik** sayfasında **Erişim denetimi (IAM)** > **Ekle**’ye tıklayın.

    ![Abonelik erişim denetimi][subscription_access]

4. **İzin ekle** sayfasında **Katkıda Bulunan** rolünü seçin ve Batch API'sini arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Daha yeni Azure AD kiracıları bu adı kullanıyor olabilir.
    3. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**. 

5. Batch API'sini bulduktan sonra seçin ve **Kaydet**'e tıklayın.

    ![Batch izinleri ekleme][add_permission]

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Kullanıcı aboneliği modunda, oluşturulacak Batch hesabı ile aynı kaynak grubuna ait olan bir Azure key vault gereklidir. Kaynak grubunun, Batch hizmetinin [mevcut](https://azure.microsoft.com/regions/services/) olduğu ve aboneliğinizin desteklediği bir bölgede olduğundan emin olun.

1. [Azure portalı][azure_portal]’nda **Yeni** > **Güvenlik + Kimlik** > **Key Vault**’a tıklayın.

2. **Key Vault Oluştur** sayfasında anahtar kasası için bir ad girin ve Batch hesabınız için istediğiniz bölgede bir kaynak grubu oluşturun. Kalan ayarları varsayılan değerlerinde bırakın ve ardından **Oluştur**’a tıklayın.




## <a name="batch-service-quotas-and-limits"></a>Batch hizmet kotaları ve limitleri
Azure aboneliğinizde ve diğer Azure hizmetlerinde olduğu gibi Batch hesapları için belirli [kotalar ve limitler](batch-quota-limit.md) geçerlidir. Batch hesabının geçerli kotaları, **Kotalar** bölümünde görüntülenir.

![Azure portalında Batch hesabı kotaları][quotas]



Ayrıca bu kotaların birçoğu Azure portalına gönderilen ücretsiz bir ürün destek isteği ile artırılabilir. Kota artışı istemeye ilişkin ayrıntılar için bkz. [Azure Batch hizmeti için kotalar ve limitler](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Diğer Batch hesabı yönetim seçenekleri
Azure portalını kullanmaya ek olarak Batch hesaplarını aşağıdakilerle oluşturup yönetebilirsiniz:

* [Batch PowerShell cmdlet’leri](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Yönetimi .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar
* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele alır ve büyük ölçekli işlem iş yüklerine olanak tanıyan hizmetin özelliklerine genel bir bakış sağlar.
* [Batch .NET istemci kitaplığı](batch-dotnet-get-started.md) veya [Python](batch-python-tutorial.md) kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. Bu tanıtıcı makaleler, bir iş yükünü birden fazla işlem düğümünde yürütmek üzere Batch hizmetini kullanan çalışan uygulamalar konusunda size rehberlik sağlamanın yanı sıra, iş yükü dosyası hazırlama ve alma işlemleri için Azure Depolama kullanma ile ilgili bilgiler içerir.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Depolama hesabı anahtarlarını yeniden oluşturma"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png

