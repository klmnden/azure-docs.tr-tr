---
title: ".NET - Azure batch hesabı kaynaklarını istemci kitaplığı ile yönetme | Microsoft Docs"
description: "Oluşturma, silme ve Azure Batch hesabı Batch yönetimi .NET kitaplığı kaynaklarla değiştirme."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eafde9258222a2ab09ade2e366f9cc595a303dec
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a>.NET için Batch hesaplarını ve kotalarını Batch Yönetimi istemci kitaplığı ile yönetme

> [!div class="op_single_selector"]
> * [Azure portalı](batch-account-create-portal.md)
> * [Batch Yönetimi .NET](batch-management-dotnet.md)
> 
> 

Bakım yükü Azure Batch uygulamalarınızda kullanarak düşürün [Batch yönetimi .NET] [ api_mgmt_net] Batch hesabı oluşturma, silme, anahtar yönetimi ve kota bulma otomatikleştirmek için kitaplık.

* **Oluşturma ve toplu işlem hesaplarını silme** hiçbir bölge içinde. İçinde her ayrı bir toplu işlem hesabı faturalama amacıyla atanır, istemciler için bir hizmet, örneğin, bir bağımsız yazılım satıcısı (ISV) sağlarsanız, hesap oluşturma ve silme özellikleri, müşteri portalı ekleyebilirsiniz.
* **Almak ve hesabı anahtarlarını yeniden** program aracılığıyla toplu hesaplarınızdan herhangi birinin için. Düzenli geçiş veya hesabı anahtarları süre sonu zorlayan güvenlik ilkeleriyle uyumlu yardımcı olabilir. Çeşitli Azure bölgelerinde birçok Batch hesapları varsa, bu geçiş işlemi Otomasyon çözümünüzün verimliliğini artırır.
* **Hesap kotalarını denetleyin** ve hangi sınırları hangi Batch hesaplarınızın belirleme dışında deneme yanılma konusunda güvenilir bilgiler gerçekleştirin. İşler başlamadan önce hesap kotalarını denetleyerek, havuzları oluşturma veya işlem düğümleri eklemek, where proaktif olarak ayarlayabilir veya bu işlem kaynakları oluşturulur. Bu hesapların ek kaynakları ayırma önce kota artırır hangi hesapların gerektirdiği belirleyebilirsiniz.
* **Diğer Azure hizmetleriyle özelliklerini birleştirme** Batch yönetimi .NET kullanarak tam özellikli yönetim deneyimi-- [Azure Active Directory][aad_about]ve [Azure Resource Manager] [ resman_overview] aynı uygulamada birlikte. Bu özellikler ve bunların API'lerini kullanarak bir uyumlu kimlik doğrulaması deneyimi, kaynak grupları ve uçtan uca yönetim çözümünü için yukarıda açıklanan özellikleri oluşturma ve silme olanağı sağlayabilir.

> [!NOTE]
> Bu makalede toplu hesaplar, anahtarları ve kotalar programlı yönetim odaklanmakla birlikte, çoğu bu etkinlikleri kullanarak gerçekleştirebileceğiniz [Azure portal][azure_portal]. Daha fazla bilgi için bkz: [Azure portalını kullanarak bir Azure Batch hesabı oluşturma](batch-account-create-portal.md) ve [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md).
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Oluşturma ve toplu işlem hesaplarını silme
Belirtildiği gibi Batch yönetim API'si birincil özelliklerini oluşturma ve bir Azure bölgesi hesaplarında toplu silme biridir. Bunu yapmak için kullanın [BatchManagementClient.Account.CreateAsync] [ net_create] ve [DeleteAsync][net_delete], ya da zaman uyumlu dekiler.

Aşağıdaki kod parçacığını bir hesabı oluşturur, yeni oluşturulan hesaba toplu hizmetinden alır ve ardından siler. Bu kod parçacığında ve bu makalede, diğerleri `batchManagementClient` tamamen başlatılmış bir örneğinin [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [!NOTE]
> Batch yönetimi .NET kitaplığı ve onun BatchManagementClient sınıfını kullanan uygulamaları gerektiren **Hizmet Yöneticisi** veya **Abonelikteki** yönetilecek toplu işlem hesabı sahibi aboneliğe erişim. Daha fazla bilgi için bkz: [Azure Active Directory](#azure-active-directory) bölüm ve [AccountManagement] [ acct_mgmt_sample] kod örneği.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Almak ve hesabı anahtarlarını yeniden oluştur
Kullanarak birincil ve ikincil hesabı anahtarları, abonelik içindeki herhangi bir toplu işlem hesabı elde [ListKeysAsync][net_list_keys]. Bu anahtarlar kullanarak yeniden [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [!TIP]
> Yönetim uygulamalarınız için kolaylaştırılmış bağlantı iş akışı oluşturabilirsiniz. İlk olarak, hesap anahtarı ile yönetmek istediğiniz toplu işlem hesabı için elde [ListKeysAsync][net_list_keys]. Ardından Batch .NET kitaplığın başlatılırken bu anahtarı kullanın [BatchSharedKeyCredentials] [ net_sharedkeycred] başlatılırken kullanılan sınıf [BatchClient][net_batch_client].
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure aboneliği ve toplu işlem hesabı kotası denetleyin
Azure abonelikleri ve tüm toplu gibi ayrı ayrı Azure hizmetlerini belirli varlıkları sayısı sınırı varsayılan kotalar sahiptir. Azure abonelikleri için varsayılan kotaları bkz [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md). Batch hizmeti için varsayılan kotaları bkz [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md). Batch yönetimi .NET kitaplığını kullanarak, uygulamalarınızı bu kotalar kontrol edebilirsiniz. Bu hesapları ekleyin veya işlem kaynaklarını havuzları gibi ve işlem düğümleri önce ayırma kararlar almanıza imkan sağlar.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Azure aboneliği toplu işlem hesabı kotası için denetleyin
Bir bölgede bir toplu işlem hesabı oluşturmadan önce Azure aboneliğinizin bu bölgede hesap eklemek mümkün olup olmadığını kontrol edebilirsiniz.

Aşağıdaki kod parçacığında, ilk kullanırız [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] olan bir abonelik tüm Batch hesaplarının koleksiyonu alınamıyor. Bu koleksiyon elde sonra hedef bölgede kaç hesaplardır belirler. Kullanırız sonra [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] toplu işlem hesabı kotası elde edilir ve bu bölgede kaç hesapları (varsa) oluşturulabilir belirleyin.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

Yukarıdaki kod parçacığında, `creds` örneği [TokenCloudCredentials][azure_tokencreds]. Bu nesne oluşturma örneği görmek için bkz: [AccountManagement] [ acct_mgmt_sample] kodu örneği github'daki.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Bir Batch hesabında işlem kaynak kotaları denetleyin
Batch çözümündeki işlem kaynakları artırmadan önce ayırmak istediğiniz kaynakları emin olmak için hesabın kotaları aşan olmaz kontrol edebilirsiniz. Aşağıdaki kod parçacığında, biz adlı toplu işlem hesabı için kota bilgilerini yazdırma `mybatchaccount`. Kendi uygulamanızda gibi bilgileri hesabı oluşturulacak ek kaynaklar işlenip işlenmeyeceğini belirlemek için kullanabilirsiniz.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [!IMPORTANT]
> Azure abonelikleri ve Hizmetleri için varsayılan kotaları olsa da, bu sınırları çoğunu bir istek göndererek yükseltilebilir [Azure portal][azure_portal]. Örneğin, [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md) toplu işlem hesabı kotası artırma hakkında yönergeler için.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Azure AD Batch yönetimi .NET ile kullanma

Batch yönetimi .NET kitaplığı bir Azure kaynak sağlayıcısı istemci ve ile birlikte kullanılan [Azure Resource Manager] [ resman_overview] hesabı kaynaklarına programlı olarak yönetmek için. Azure AD ve toplu işlem yönetimi .NET kitaplığı dahil olmak üzere tüm Azure kaynak sağlayıcısı istemci aracılığıyla yapılan istekleri kimlik doğrulaması için gerekli olduğunu [Azure Resource Manager][resman_overview]. Azure AD Batch yönetimi .NET kitaplığı ile kullanma hakkında daha fazla bilgi için bkz: [kullanım Azure Batch çözümlerinizi kimlik doğrulaması için Active Directory](batch-aad-auth.md). 

## <a name="sample-project-on-github"></a>Github'da örnek proje

Batch yönetimi .NET eylemde görmek için kullanıma [AccountManagment] [ acct_mgmt_sample] örnek proje github'da. AccountManagment örnek uygulama aşağıdaki işlemleri gösterir:

1. Kullanarak Azure AD'den bir güvenlik belirtecini almak [ADAL][aad_adal]. Kullanıcı zaten oturum açmamış varsa, bunlar Azure kullanıcılardan kimlik bilgileri istenir.
2. Güvenlik belirteci Azure AD'den alınan oluşturmak bir [SubscriptionClient] [ resman_subclient] hesabıyla ilişkilendirilmiş abonelik listesi için Azure sorgulanamıyor. Birden fazla aboneliğiniz varsa kullanıcı listesinden bir abonelik seçebilirsiniz.
3. Seçilen abonelikle ilişkili kimlik bilgilerini alın.
4. Oluşturma bir [ResourceManagementClient] [ resman_client] kimlik bilgilerini kullanarak nesne.
5. Kullanım bir [ResourceManagementClient] [ resman_client] bir kaynak grubu oluşturulacak nesne.
6. Kullanım bir [BatchManagementClient] [ net_mgmt_client] birkaç toplu hesap işlemleri gerçekleştirmek için nesne:
   * Bir toplu işlem hesabı yeni kaynak grubu oluşturun.
   * Yeni oluşturulan hesaba toplu hizmetinden Al.
   * Yeni hesabı için hesap anahtarları yazdırın.
   * Hesap için yeni bir birincil anahtar yeniden oluşturun.
   * Hesap için kota bilgilerini yazdırın.
   * Aboneliği için kota bilgilerini yazdırın.
   * Abonelik içindeki tüm hesapları yazdırın.
   * Yeni oluşturulan hesabı silin.
7. Kaynak grubunu silin.

Yeni oluşturulan toplu iş hesabı ve kaynak grubu silmeden önce bunları görüntüleyebilirsiniz [Azure portal][azure_portal]:

Örnek Uygulama başarıyla çalışması için ilk Azure portalında Azure AD kiracınıza ile kaydeder ve Azure Resource Manager API için izinleri verin. İçinde sağlanan adımları izleyin [Active Directory ile kimlik doğrulaması toplu yönetim çözümleri](batch-aad-auth-management.md).


[aad_about]: ../active-directory/active-directory-whatis.md "Azure Active Directory nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD için kimlik doğrulama senaryoları"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Uygulamaları Azure Active Directory ile tümleştirme"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
