---
title: "Azure portalını kullanarak Batch hesabı oluşturma | Microsoft Docs"
description: "Büyük ölçekli paralel iş yükleri bulutta çalıştırmak için Azure portalda bir Azure Batch hesabı oluşturmayı öğrenin"
services: batch
documentationcenter: 
author: v-dotren
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/28/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b7629e496f2d73798b94acdc611014a8b3afead7
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Azure portalıyla Batch hesabı oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](batch-account-create-portal.md)
> * [Batch Yönetimi .NET](batch-management-dotnet.md)
>
>

[Azure portalında][azure_portal] Azure Batch hesabı oluşturma hakkında bilgi alın ve işlem senaryonuza uygun hesap özelliklerini seçin. Erişim anahtarları ve hesap URL’leri gibi önemli hesap özelliklerini nerede bulabileceğinizi öğrenin.

Batch hesapları ve senaryoları hakkında arka plan bilgileri için bkz. [özelliğe genel bakış](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Batch hesabı oluşturma



1. [Azure portalında][azure_portal] oturum açın.
2. **Yeni**'ye tıklayın ve Market içinde **Batch Hizmeti** araması yapın.

    ![Market’te Batch][marketplace_portal]
3. **Batch Hizmeti**'ni seçin, **Oluştur**'a tıklayın ve **Yeni Batch hesabı ayarları** yazın. Aşağıdaki ayrıntılara bakın.

    ![Batch hesabı oluşturma][account_portal]

    a. **Hesap adı**: Seçtiğiniz ad, hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki **Konum** bölümüne bakın). Hesap adı yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    b. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.

    c. **Havuz ayırma modu**: Bu seçenek görünürse varsayılan **Batch hizmeti**'ni kabul edin.

    c. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubu seçebilir ya da isterseniz yeni bir tane oluşturabilirsiniz.

    d. **Konum**: Batch hesabının oluşturulacağı bir Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.

    e. **Depolama hesabı** (isteğe bağlı): Batch hesabınızla ilişkilendireceğiniz genel amaçlı depolama hesabı. Çoğu Batch hesabı için önerilen seçenek budur. Ayrıntılar için bu makalenin sonraki bölümlerinde [Bağlı Azure Depolama hesabı](#linked-azure-storage-account) konusuna bakın.

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.



## <a name="view-batch-account-properties"></a>Batch hesabı özelliklerini görüntüleme
Hesap oluşturulduktan sonra üzerine tıklayarak ayarlarına ve özelliklerine erişebilirsiniz. Sol menüyü kullanarak tüm hesap ayarlarına ve özelliklerine erişebilirsiniz.

![Azure portalında Batch hesabı dikey penceresi][account_blade]

* **Batch hesabı URL'si**: [Batch API'leri](batch-apis-tools.md#azure-accounts-for-batch-development) ile uygulama geliştirirken, Batch kaynaklarınıza erişebilmeniz için bir hesap URL'si gereklidir. Batch hesabı URL’sinin biçimi aşağıdaki gibidir:

    `https://<account_name>.<region>.batch.azure.com`

![Portalda Batch hesabı URL’si][account_url]

* **Erişim anahtarları**: Batch hesabınıza uygulamanızdan yapılan erişimlere kimlik doğrulaması gerçekleştirmek için bir hesap erişim anahtarına ihtiyaç duyabilirsiniz. (Batch, Azure Active Directory kimlik doğrulamasını da destekler.)

    Erişim anahtarlarını görüntülemek veya yeniden oluşturmak için **Anahtarlar**'ı seçin.

    ![Azure portalında Batch hesabı anahtarları][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Bağlı Azure Storage hesabı

Genel amaçlı bir Azure Depolama hesabını Batch hesabınıza bağlayabilirsiniz. Bu seçenek birçok senaryoda yardımcı olacaktır. Batch'in [uygulama paketleri](batch-application-packages.md) özelliği, [Batch Dosya Kuralları .NET](batch-task-output.md) kitaplığının yaptığı gibi Azure Blob depolama kullanır. Bu isteğe bağlı özellikler Batch görevlerinizin çalıştırdığı uygulamaları dağıtmanıza ve oluşturduğu verileri kalıcı hale getirmeniz yardımcı olur.

Yalnızca Batch hesabınız tarafından kullanılacak yeni bir Depolama hesabı oluşturmanız önerilir. Azure Batch şu anda Depolama hesabı türünün yalnızca genel amaçlı kullanımını desteklemektedir. Bu hesap türü [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md) bölümünün [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) başlıklı 5. adımında anlatılmıştır.

![Genel amaçlı depolama hesabı oluşturma][storage_account]

> [!NOTE]
> Bağlantılı bir Depolama hesabının erişim anahtarlarını yeniden oluştururken dikkatli olun. Yalnızca bir Storage hesap anahtarını yeniden oluşturun ve bağlı Storage hesabı dikey penceresinde **Anahtarları Eşitle**’ye tıklayın. Anahtarların havuzlarınızdaki işlem düğümlerine yayılması için beş dakika bekleyin, ardından gerekirse diğer anahtarı yeniden oluşturup eşitleyin. İki anahtarı da aynı anda oluşturursanız işlem düğümleriniz iki anahtarı da eşitleyemez ve anahtarlar Storage hesabına erişimi kaybederler.
>
>

![Depolama hesabı anahtarlarını yeniden oluşturma][4]

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
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
