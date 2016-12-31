---
title: "Azure Batch hesabı oluşturma | Microsoft Belgeleri"
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
ms.date: 09/21/2016
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: dfcf1e1d54a0c04cacffb50eca4afd39c6f6a1b1
ms.openlocfilehash: 8875c5e91a2a1ae848ebf725ca00bd43ec667dce


---
# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Azure portalı kullanarak Azure Batch hesabı oluşturma
> [!div class="op_single_selector"]
> * [Azure portal](batch-account-create-portal.md)
> * [Batch Yönetimi .NET](batch-management-dotnet.md)
> 
> 

[Azure portalında][azure_portal] Azure Batch hesabı oluşturmayı öğrenin ve erişim anahtarları ile hesap URL’leri gibi önemli hesap özelliklerinin nerede bulunacağı hakkında bilgi edinin. Bu konuda ayrıca Batch fiyatlandırması ve [uygulama paketlerini](batch-application-packages.md) kullanabilmeniz ve [iş ile görev çıktısını kalıcı hale getirebilmeniz](batch-task-output.md) için bir Azure Storage hesabının Batch hesabınıza bağlanması ele alınmaktadır.

## <a name="create-a-batch-account"></a>Batch hesabı oluşturma
1. [Azure portalında][azure_portal] oturum açın.
2. **Yeni** > **İşlem** > **Batch Hizmeti**'ne tıklayın.
   
    ![Market’te Batch][marketplace_portal]
3. **Yeni Batch Hesabı** dikey penceresi görüntülenir. Her bir dikey pencere öğesinin açıklaması için aşağıda *a* ile *e* arası öğelere bakın.
   
    ![Batch hesabı oluşturma][account_portal]
   
    a. **Hesap adı**: Batch hesabı için benzersiz bir ad. Bu ad hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki *Konum* ’a bakın). Bu, yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.
   
    b. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.
   
    c. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubu ya da isterseniz yeni bir tane oluşturabilirsiniz.
   
    d. **Konum**: Batch hesabının oluşturulacağı bir Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.
   
    e. **Depolama hesabı** (isteğe bağlı): Yeni Batch hesabınızla ilişkilendireceğiniz (bağlayacağınız) **Genel amaçlı** bir depolama hesabı. Daha fazla bilgi için aşağıdaki [Bağlı Azure Storage hesabı](#linked-azure-storage-account) konusuna bakın.
4. Hesabı oluşturmak için **Oluştur**’a tıklayın.
   
   Portal, hesabı **Dağıtmakta** olduğunu gösterir ve tamamlandıktan sonra *Bildirimler* alanında **Dağıtımlar başarıyla tamamlandı** bildirimi görüntülenir.

## <a name="view-batch-account-properties"></a>Batch hesabı özelliklerini görüntüleme
Hesap oluşturulduktan sonra **Batch hesabı dikey penceresini** açarak ayarlarına ve özelliklerine erişebilirsiniz. Batch hesabı dikey penceresinin sol menüsünü kullanarak tüm hesap ayarlarına ve özelliklerine erişebilirsiniz.

![Azure portalında Batch hesabı dikey penceresi][account_blade]

* **Batch hesabı URL’si**: [Batch geliştirme API’leri](batch-technical-overview.md#batch-development-apis) ile oluşturduğunuz uygulamaların kaynakları yönetmesi ve hesap içinde işleri çalıştırması için bir hesap URL’si gerekir. Batch hesabı URL’sinin biçimi aşağıdaki gibidir:
  
    `https://<account_name>.<region>.batch.azure.com`

![Portalda Batch hesabı URL’si][account_url]

* **Erişim anahtarları**: Uygulamalarınız Batch hesabınızdaki kaynaklarla çalışırken bir erişim anahtarı da gerekir. Batch hesabınızın erişim anahtarlarını görüntülemek veya yeniden oluşturmak için Batch hesabı dikey penceresinin sol menüsündeki **Arama** kutusuna `keys` girin ve ardından **Anahtarlar**’ı seçin.
  
    ![Azure portalında Batch hesabı anahtarları][account_keys]

## <a name="pricing"></a>Fiyatlandırma
Batch hesapları yalnızca, Batch hesabı için ücret ödemediğiniz anlamına gelen "Ücretsiz Katmanı" içinde sunulur. Batch hesaplarınızın kullandığı temel alınan Azure işlem kaynakları için ve iş yükleriniz çalıştığında diğer hizmetler tarafından kullanılan kaynaklar için ücretlendirilirsiniz. Örneğin, havuzlarınızdaki işlem düğümleri ve Azure Storage’a depoladığınız veriler için görevlerinizin girdi veya çıktısı olarak ücret ödersiniz. Benzer şekilde, Batch [uygulama paketleri](batch-application-packages.md) özelliğini kullanırsanız uygulama paketlerinizi depolamak için kullanılan Azure Storage kaynakları için ücretlendirilirsiniz. Daha fazla bilgi için bkz. [Batch fiyatlandırması][batch_pricing].

## <a name="linked-azure-storage-account"></a>Bağlı Azure Storage hesabı
Daha önce bahsedildiği gibi **Genel amaçlı** bir Storage hesabını (isteğe bağlı olarak) Batch hesabınıza bağlayabilirsiniz. Batch [uygulama paketleri](batch-application-packages.md) özelliği, [Batch Dosya Kuralları .NET](batch-task-output.md) kitaplığının yaptığı gibi Genel amaçlı bir Storage hesabında blob depolama kullanır. Bu isteğe bağlı özellikler Batch görevlerinizin çalıştırdığı uygulamaları dağıtmanıza ve oluşturduğu verileri kalıcı hale getirmeniz yardımcı olur.

Batch şu anda [Azure Storage hesapları hakkında](../storage/storage-create-storage-account.md) bölümünde 5. adım olan [Depolama hesabı oluşturma](../storage/storage-create-storage-account.md#create-a-storage-account) içinde açıklandığı gibi *sadece* **Genel amaçlı** depolama hesabı türünü destekler. Batch hesabınıza bir Azure Storage hesabı bağladığınızda *yalnızca* **Genel amaçlı** depolama hesabı bağladığınızdan emin olun.

!["Genel amaçlı" depolama hesabı oluşturma][storage_account]

Yalnızca Batch hesabınız tarafından kullanılacak bir Storage hesabı oluşturmanız önerilir.

> [!WARNING]
> Bağlantılı bir Storage hesabının erişim anahtarlarını yeniden oluşturulurken dikkatli olun. Yalnızca bir Storage hesap anahtarını yeniden oluşturun ve bağlı Storage hesabı dikey penceresinde **Anahtarları Eşitle**’ye tıklayın. Anahtarların havuzlarınızdaki işlem düğümlerine yayılması için beş dakika bekleyin, ardından gerekirse diğer anahtarı yeniden oluşturup eşitleyin. İki anahtarı da aynı anda oluşturursanız işlem düğümleriniz iki anahtarı da eşitleyemez ve anahtarlar Storage hesabına erişimi kaybederler.
> 
> 

  ![Depolama hesabı anahtarlarını yeniden oluşturma][4]

## <a name="batch-service-quotas-and-limits"></a>Batch hizmet kotaları ve limitleri
Azure aboneliğinizde ve diğer Azure hizmetlerinde olduğu gibi Batch hesapları için belirli [kotalar ve limitler](batch-quota-limit.md) geçerlidir. Bir Batch hesabı için geçerli kotalar hesap **Özellikleri** içindeki portalda görüntülenir.

![Azure portalında Batch hesabı kotaları][quotas]

Batch iş yüklerinizi tasarlayıp ölçeğini artırırken kotaları göz önünde bulundurun. Örneğin, havuzunuz belirttiğiniz hedef işlem düğümü sayısına ulaşmıyorsa Batch hesabınız için çekirdek kota sınırına ulaşmış olabilirsiniz.

Ayrıca, Azure aboneliğiniz için tek bir Batch hesabıyla kısıtlanmazsınız. Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi aynı abonelik ve farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

Bu kotaların birçoğu yalnızca Azure portalına gönderilen ücretsiz bir ürün destek isteği ile artırılabilir. Kota artışı istemeye ilişkin ayrıntılar için bkz. [Azure Batch hizmeti için kotalar ve limitler](batch-quota-limit.md).

## <a name="other-batch-account-management-options"></a>Diğer Batch hesabı yönetim seçenekleri
Azure portalını kullanmaya ek olarak Batch hesaplarını aşağıdakilerle oluşturup yönetebilirsiniz:

* [Batch PowerShell cmdlet’leri](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [Batch Yönetimi .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar
* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Azure Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele alır ve büyük ölçekli işlem iş yükü yürütmeye olanak tanıyan hizmetin özelliklerine genel bir bakış sağlar. 
* [Batch .NET istemci kitaplığı](batch-dotnet-get-started.md)nı kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. [Giriş makalesi](batch-dotnet-get-started.md) birden fazla işlem düğümünde bir iş yükü yürütmek üzere Batch hizmetini kullanan çalışan bir uygulama için size rehberlik sağlar ve iş yükü dosyası hazırlama ve alma için Azure Storage kullanmayı içerir.

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



<!--HONumber=Dec16_HO2-->


