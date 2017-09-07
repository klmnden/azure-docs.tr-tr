---
title: "Azure portalını kullanarak Batch hesabı oluşturma | Microsoft Docs"
description: "Büyük ölçekli paralel iş yükleri bulutta çalıştırmak için Azure portalda bir Azure Batch hesabı oluşturmayı öğrenin"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 520d1d42d35b25db1a35d4317e9eb616cf5de565
ms.contentlocale: tr-tr
ms.lasthandoff: 08/28/2017

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

Portalı kullanarak iki *havuz ayırma modundan* birinde Batch hesabı oluşturma: **Batch hizmeti** modu veya daha fazla yapılandırma gerektiren yeni **kullanıcı aboneliği** modu. Bu iki mod hakkında bilgi için bkz. [özelliğe genel bakış](batch-api-basics.md#account). Kullanıcı aboneliği modunun özellikleri için ayrıca bkz. [blog gönderisi](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).

## <a name="batch-service-mode"></a>Batch hizmeti modu



1. [Azure portalında][azure_portal] oturum açın.
2. **Yeni** > **İşlem** > **Batch Hizmeti**'ne tıklayın.

    ![Market’te Batch][marketplace_portal]
3. **Yeni Batch Hesabı** dikey penceresi görüntülenir. Her dikey pencere öğesinin aşağıdaki açıklamalarına bakın.

    ![Batch hesabı oluşturma][account_portal]

    a. **Hesap adı**: Seçtiğiniz Batch hesabı adı, hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki **Konum** bölümüne bakın). Hesap adı yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    b. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.

    c. **Havuz ayırma modu**: **Batch hizmeti**’ni seçin.

    c. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubu seçebilir ya da isterseniz yeni bir tane oluşturabilirsiniz.

    d. **Konum**: Batch hesabının oluşturulacağı bir Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.

    e. **Depolama hesabı** (isteğe bağlı): Batch hesabınızla ilişkilendireceğiniz genel amaçlı depolama hesabı. Çoğu Batch hesabı için önerilen seçenek budur. Daha fazla bilgi için bu makalenin sonraki bölümlerinde [Bağlı Azure Depolama hesabı](#linked-azure-storage-account) konusuna bakın.

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.

   Portal, dağıtımın devam ettiğini gösterir. İşlem tamamlandıktan sonra **Bildirimler** bölümünde **Dağıtım başarılı** bildirimi görünür.

## <a name="user-subscription-mode"></a>Kullanıcı aboneliği modu

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Azure Batch hizmetinin aboneliğe erişmesine izin verme (tek seferlik işlem)
Kullanıcı aboneliği modunda ilk Batch hesabınızı oluştururken, aboneliğinizi Batch hizmetine kaydetmek için aşağıdaki adımları uygulayın. (Bunu daha önce yaptıysanız sonraki bölüme atlayın.)

1. [Azure portalında][azure_portal] oturum açın.

2. **Diğer Hizmetler** > **Abonelikler**’e ve ardından Batch hesabı için kullanmak istediğiniz aboneliğe tıklayın.

3. **Abonelik** dikey penceresinde **Erişim denetimi (IAM)** > **Ekle**’ye tıklayın.

    ![Abonelik erişim denetimi][subscription_access]

4. **İzin ekle** dikey penceresinde **Katkıda Bulunan** rolünü seçin ve Batch API'sini arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Daha yeni Azure AD kiracıları bu adı kullanıyor olabilir.
    3. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**. 

5. Batch API'sini bulduktan sonra seçin ve **Kaydet**'e tıklayın.

    ![Batch izinleri ekleme][add_permission]

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Kullanıcı aboneliği modunda, oluşturulacak Batch hesabı ile aynı kaynak grubuna ait olan bir Azure key vault gereklidir. Kaynak grubunun, Batch hizmetinin [mevcut](https://azure.microsoft.com/regions/services/) olduğu ve aboneliğinizin desteklediği bir bölgede olduğundan emin olun.

1. [Azure portalı][azure_portal]’nda **Yeni** > **Güvenlik + Kimlik** > **Key Vault**’a tıklayın.

2. **Key Vault Oluştur** dikey penceresinde anahtar kasası için bir ad girin ve Batch hesabınız için istediğiniz bölgede bir kaynak grubu oluşturun. Kalan ayarları varsayılan değerlerinde bırakın ve ardından **Oluştur**’a tıklayın.

### <a name="create-a-batch-account"></a>Batch hesabı oluşturma

1. [Azure portalı][azure_portal]’nda **Yeni** > **İşlem** > **Batch Hizmeti**’ne tıklayın.

    ![Market’te Batch][marketplace_portal]
3. **Yeni Batch Hesabı** dikey penceresi görüntülenir. Her dikey pencere öğesinin aşağıdaki açıklamalarına bakın.

    ![Batch hesabı oluşturma][account_portal_byos]

    a. **Hesap adı**: Seçtiğiniz Batch hesabı adı, hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki **Konum** bölümüne bakın). Hesap adı yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    b. **Abonelik**: Birden fazla aboneliğiniz varsa, Batch hizmetine kaydettiğiniz aboneliği seçin.

    c. **Havuzu ayırma modu**: **Kullanıcı aboneliği**’ni seçin.

    d. **Anahtar kasası**: Önceki bölümde Batch hesabınız için oluşturduğunuz anahtar kasasını seçin. İsteğe bağlı olarak, yeni bir anahtar kasası oluşturun. Kasayı seçtikten sonra Azure Batch hizmetine anahtar kasası erişimi vermek üzere onay kutusunu işaretleyin.

    c. **Kaynak grubu**: Anahtar kasasını oluşturduğunuz kaynak grubunu seçin.

    d. **Konum**: Batch hesabı için anahtar kasasını oluşturduğunuz Azure bölgesi.

    e. **Depolama hesabı** (isteğe bağlı): Batch hesabınızla ilişkilendireceğiniz genel amaçlı depolama hesabı. Çoğu Batch hesabı için önerilen seçenek budur. Daha fazla bilgi için aşağıdaki [Bağlı Azure Storage hesabı](#linked-azure-storage-account) konusuna bakın.

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.

   Portal, dağıtımın devam ettiğini gösterir. İşlem tamamlandıktan sonra **Bildirimler** bölümünde **Dağıtım başarılı** bildirimi görünür.



## <a name="view-batch-account-properties"></a>Batch hesabı özelliklerini görüntüleme
Hesap oluşturulduktan sonra **Batch hesabı dikey penceresini** açarak ayarlarına ve özelliklerine erişebilirsiniz. Batch hesabı dikey penceresinin sol menüsünü kullanarak tüm hesap ayarlarına ve özelliklerine erişebilirsiniz.

![Azure portalında Batch hesabı dikey penceresi][account_blade]

* **Batch hesabı URL'si**: [Batch API'leri](batch-apis-tools.md#azure-accounts-for-batch-development) ile uygulama geliştirirken, Batch kaynaklarınıza erişebilmeniz için bir hesap URL'si gereklidir. Batch hesabı URL’sinin biçimi aşağıdaki gibidir:

    `https://<account_name>.<region>.batch.azure.com`

![Portalda Batch hesabı URL’si][account_url]

* **Erişim anahtarları** (Batch hizmeti modu): Batch hesabınıza uygulamanızdan yapılan erişimlere kimlik doğrulaması gerçekleştirmek için bir hesap erişim anahtarına ihtiyaç duyarsınız. (Bu ayar, Azure Active Directory kimlik doğrulamasını kullandığınız kullanıcı aboneliği modunda mevcut değildir.)

    Batch hesabınızın erişim anahtarlarını görüntülemek veya yeniden oluşturmak için Batch hesabı dikey penceresinin sol menüsündeki **Arama** kutusuna `keys` girin ve ardından **Anahtarlar**’ı seçin.

    ![Azure portalında Batch hesabı anahtarları][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Bağlı Azure Storage hesabı

Genel amaçlı bir Azure Depolama hesabını isteğe bağlı olarak Batch hesabınıza bağlayabilirsiniz. Batch'in [uygulama paketleri](batch-application-packages.md) özelliği, [Batch Dosya Kuralları .NET](batch-task-output.md) kitaplığının yaptığı gibi Azure Blob depolama kullanır. Bu isteğe bağlı özellikler Batch görevlerinizin çalıştırdığı uygulamaları dağıtmanıza ve oluşturduğu verileri kalıcı hale getirmeniz yardımcı olur.

Yalnızca Batch hesabınız tarafından kullanılacak yeni bir Depolama hesabı oluşturmanız önerilir.

![Genel amaçlı depolama hesabı oluşturma][storage_account]

> [!NOTE]
> Azure Batch şu anda Depolama hesabı türünün yalnızca genel amaçlı kullanımını desteklemektedir. Bu hesap türü [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md) belgesinin 5. adımında [Depolama hesabı oluşturma] (../storage/common/storage-create-storage-account.md#create-a-storage-account) açıklanmıştır.
>
>

> [!WARNING]
> Bağlantılı bir Depolama hesabının erişim anahtarlarını yeniden oluştururken dikkatli olun. Yalnızca bir Storage hesap anahtarını yeniden oluşturun ve bağlı Storage hesabı dikey penceresinde **Anahtarları Eşitle**’ye tıklayın. Anahtarların havuzlarınızdaki işlem düğümlerine yayılması için beş dakika bekleyin, ardından gerekirse diğer anahtarı yeniden oluşturup eşitleyin. İki anahtarı da aynı anda oluşturursanız işlem düğümleriniz iki anahtarı da eşitleyemez ve anahtarlar Storage hesabına erişimi kaybederler.
>
>

![Depolama hesabı anahtarlarını yeniden oluşturma][4]

## <a name="batch-service-quotas-and-limits"></a>Batch hizmet kotaları ve limitleri
Azure aboneliğinizde ve diğer Azure hizmetlerinde olduğu gibi Batch hesapları için belirli [kotalar ve limitler](batch-quota-limit.md) geçerlidir. Bir Batch hesabı için geçerli kotalar hesap **Özellikleri** içindeki portalda görüntülenir.

![Azure portalında Batch hesabı kotaları][quotas]



Ayrıca bu kotaların birçoğu yalnızca Azure portalına gönderilen ücretsiz bir ürün destek isteği ile artırılabilir. Kota artışı istemeye ilişkin ayrıntılar için bkz. [Azure Batch hizmeti için kotalar ve limitler](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Diğer Batch hesabı yönetim seçenekleri
Azure portalını kullanmaya ek olarak Batch hesaplarını aşağıdakilerle oluşturup yönetebilirsiniz:

* [Batch PowerShell cmdlet’leri](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Yönetimi .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar
* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele alır ve büyük ölçekli işlem iş yükü yürütmeye olanak tanıyan hizmetin özelliklerine genel bir bakış sağlar. 
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

