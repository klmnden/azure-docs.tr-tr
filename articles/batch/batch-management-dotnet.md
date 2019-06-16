---
title: .NET - Azure Batch hesabı kaynaklarına istemci kitaplığı ile yönetme | Microsoft Docs
description: Oluşturma, silme ve Azure Batch hesabı kaynaklarına toplu işlem yönetimi .NET kitaplığı ile değiştirin.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 16279b23-60ff-4b16-b308-5de000e4c028
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/24/2017
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 301a3f9a500c41cf13dfa071d3526d2128b5e131
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60775143"
---
# <a name="manage-batch-accounts-and-quotas-with-the-batch-management-client-library-for-net"></a>.NET için Batch Yönetimi istemci kitaplığı ile batch hesaplarını ve kotalarını yönetme

> [!div class="op_single_selector"]
> * [Azure portal](batch-account-create-portal.md)
> * [Batch Yönetimi .NET](batch-management-dotnet.md)
> 
> 

Bakım yükü Azure Batch uygulamalarınızı kullanarak düşürün [toplu işlem yönetimi .NET] [ api_mgmt_net] kitaplığını kullanarak Batch hesabı oluşturma, silme, anahtar yönetimi ve kota bulma otomatikleştirin.

* **Oluşturma ve Batch hesapları silme** herhangi bir bölge içinde. İçinde her ayrı bir Batch hesabı faturalandırma için atanan istemcileriniz için bir hizmet, örneğin, bir bağımsız yazılım satıcısı (ISV) sağlarsanız, müşteri Portalı'na hesabı oluşturma ve silme özelliklerini ekleyebilirsiniz.
* **Alma ve hesap anahtarlarını yeniden** program aracılığıyla herhangi bir toplu iş hesaplarınız için. Düzenli geçiş veya hesap anahtarlarını sonu zorunlu güvenlik ilkeleriyle uyumlu yardımcı olabilir. Çeşitli Azure bölgelerinde birkaç Batch hesapları varsa, bu geçiş işlemi, otomasyon, çözümünüzün verimliliğini artırır.
* **Hesabı kotaları denetleyin** ve deneme yanılma kararın hangi Batch hesaplarını, hangi karşılaşmasını belirleme dışına taşıyın. İşler başlamadan önce hesabı kotaları denetleyerek, havuzları oluşturma ya da işlem düğümleri ekleme, nerede proaktif olarak ayarlayabilir veya bu işlem kaynakları oluşturulur. Bu hesaplar, ek kaynaklar ayrılmadan önce kota artırır hangi hesapların gerektirdiği belirleyebilirsiniz.
* **Diğer Azure Hizmetleri özelliklerini birleştirme** toplu işlem yönetimi .NET kullanarak tam özellikli bir yönetim deneyimi-- [Azure Active Directory][aad_about]ve [Azure Resource Manager] [ resman_overview] birlikte aynı uygulamada. Bu özellikler ve bunların API'leri kullanarak bir uyumlu kimlik doğrulaması deneyimi, kaynak grupları ve bir uçtan uca yönetim çözümü için yukarıda açıklanan özellikleri oluşturma ve silme olanağı sağlayabilir.

> [!NOTE]
> Bu makalede Batch hesapları, anahtarları ve kotalar programlı yönetim odaklanmakla birlikte, bu etkinliklerin çoğunu gerçekleştirebilirsiniz [Azure portalında][azure_portal]. Daha fazla bilgi için [Azure portalını kullanarak bir Azure Batch hesabı oluşturma](batch-account-create-portal.md) ve [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md).
> 
> 

## <a name="create-and-delete-batch-accounts"></a>Oluşturma ve Batch hesapları silme
Belirtildiği gibi birincil Batch Yönetimi API'si özellikleri oluşturmak ve bir Azure bölgesindeki Batch hesapları silmek için biridir. Bunu yapmak için [BatchManagementClient.Account.CreateAsync] [ net_create] ve [DeleteAsync][net_delete], ya da zaman uyumlu karşılıkları.

Aşağıdaki kod parçacığı bir hesabı oluşturur, yeni oluşturulan hesaba Batch hizmetinden alır ve onu siler. Bu kod parçacığı ve bu makalede, diğerleri `batchManagementClient` , tam olarak başlatılmış bir örneğidir [BatchManagementClient][net_mgmt_client].

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
> Batch yönetimi .NET kitaplığı ve onun BatchManagementClient sınıfını kullanan uygulamaların gerektiren **Hizmet Yöneticisi** veya **Abonelikteki** Batch sahip abonelik erişimi Yönetilecek hesabı. Daha fazla bilgi için Azure Active Directory bölümüne bakın ve [hesap yönetimi] [ acct_mgmt_sample] kod örneği.
> 
> 

## <a name="retrieve-and-regenerate-account-keys"></a>Alma ve hesap anahtarlarını yeniden oluştur
Kullanarak birincil ve ikincil hesabı anahtarları, aboneliğiniz kapsamındaki herhangi bir Batch hesabı elde [ListKeysAsync][net_list_keys]. Bu anahtarları kullanarak yeniden oluşturabilirsiniz [RegenerateKeyAsync][net_regenerate_keys].

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
> Yönetim uygulamalarınız için kolaylaştırılmış bağlantı iş akışı oluşturabilirsiniz. Batch hesabı ile yönetmek istediğiniz bir hesap anahtarı edinip [ListKeysAsync][net_list_keys]. Ardından, Batch .NET Kitaplığı'nızın başlatılırken bu anahtarı kullanmasını [BatchSharedKeyCredentials] [ net_sharedkeycred] başlatılırken kullanılan sınıfı [BatchClient] [ net_batch_client].
> 
> 

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure aboneliği ve Batch hesabı kotaları denetleyin
Azure abonelikleri ve tüm Batch gibi tek tek Azure hizmetlerinin belirli varlıkları sayısını sınırlayan varsayılan kotaları var. İçin varsayılan kotaları Azure abonelikleri için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md). Batch hizmetinin varsayılan kotaları için bkz: [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md). Batch yönetimi .NET kitaplığını kullanarak uygulamalarınızda bu kotalar kontrol edebilirsiniz. Bu hesapları ekleyin veya işlem havuzları gibi kaynakları ve işlem düğümleri için önce ayırma kararlar olanak sağlar.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Azure aboneliğiniz için Batch hesabı kotaları denetleyin
Bir bölgede bir Batch hesabı oluşturmadan önce Azure aboneliğinizi bu bölgede bir hesap eklemeniz mümkün olup olmadığını kontrol edebilirsiniz.

Aşağıdaki kod parçacığında, önce kullandığımız [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] abonelik içinde tüm Batch hesaplarının bir koleksiyonu almak için. Biz bu koleksiyon elde sonra hedef bölgede kaç hesaplarıdır belirleyin. Sonra da kullandığımız [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] Batch hesabı kotası elde edilir ve (varsa) kaç hesapları bu bölgede oluşturulabilir belirleyin.

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

Yukarıdaki kod parçacığında `creds` örneğidir [TokenCloudCredentials][azure_tokencreds]. Bu nesne oluşturma örneği için bkz [hesap yönetimi] [ acct_mgmt_sample] github'daki kod örneği.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Bir Batch hesabı için işlem kaynak kotaları denetleyin
Batch çözümünüz bilgi işlem kaynaklarının artırmadan önce hesabın kotaları aşan olmaz ayırmak istediğiniz kaynakları emin olmak için kontrol edebilirsiniz. Aşağıdaki kod parçacığında adlı Batch hesabı için kota bilgilerini yazdırmaya `mybatchaccount`. Kendi uygulamanızda hesabın oluşturulması için ek kaynaklar işlenip işlenmeyeceğini belirlemek için bu bilgileri kullanabilirsiniz.

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
> Azure abonelik ve Hizmetleri için varsayılan kotaları olsa da, bu limitlerin çoğu, bir istek göndererek yükseltilebilir [Azure portalında][azure_portal]. Örneğin, [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md) yönergeler için Batch hesabı kotaları artırma.
> 
> 

## <a name="use-azure-ad-with-batch-management-net"></a>Batch yönetimi .NET ile Azure AD kullanma

Batch yönetimi .NET kitaplığı, bir Azure kaynak sağlayıcısı istemci ve ile birlikte kullanılan [Azure Resource Manager] [ resman_overview] hesabı kaynaklarına programlı olarak yönetmek için. Azure AD ve Batch yönetimi .NET kitaplığı dahil olmak üzere tüm Azure kaynak sağlayıcısı istemci aracılığıyla yapılan isteklerin kimliğini doğrulamak için gerekli [Azure Resource Manager][resman_overview]. Azure AD ile Batch yönetimi .NET kitaplığını kullanma hakkında daha fazla bilgi için bkz: [kullanımı Azure Batch çözümlerinin kimlik doğrulaması için Active Directory](batch-aad-auth.md). 

## <a name="sample-project-on-github"></a>GitHub üzerinde örnek proje

Batch yönetimi .NET nasıl çalıştığını görmek için kullanıma [hesap yönetimi] [ acct_mgmt_sample] GitHub üzerinde örnek proje. Hesap Yönetimi örnek uygulaması, aşağıdaki işlemleri göstermektedir:

1. Azure AD'de bir güvenlik belirteci kullanarak elde [ADAL][aad_adal]. Kullanıcı zaten oturum açmamış, bunlar Azure kullanıcılardan kimlik bilgileri istenir.
2. Azure AD'den alınan güvenlik belirteci ile bir [SubscriptionClient] [ resman_subclient] hesapla ilişkili aboneliklerin listesi için Azure sorgulanamıyor. Birden fazla aboneliğiniz varsa, kullanıcı listeden bir abonelik seçebilirsiniz.
3. Seçili abonelikle ilişkili kimlik bilgilerini alın.
4. Oluşturma bir [ResourceManagementClient] [ resman_client] kimlik bilgilerini kullanarak nesne.
5. Kullanım bir [ResourceManagementClient] [ resman_client] bir kaynak grubu oluşturmak için nesne.
6. Kullanım bir [BatchManagementClient] [ net_mgmt_client] birkaç Batch hesap işlemleri gerçekleştirmek için nesne:
   * Yeni kaynak grubunda bir Batch hesabı oluşturun.
   * Yeni oluşturulan hesaba Batch hizmetinden alın.
   * Yeni hesap için hesap anahtarları yazdırın.
   * Hesabı için yeni bir birincil anahtarı yeniden oluştur.
   * Hesap için kota bilgilerini yazdırın.
   * Aboneliği için kota bilgisi yazdırın.
   * Abonelik içindeki tüm hesapları yazdırın.
   * Yeni oluşturulan hesaba silin.
7. Kaynak grubunu silin.

Yeni oluşturulan Batch hesabı ve kaynak grubu silmeden önce bunları görüntüleyebilirsiniz [Azure portalında][azure_portal]:

Örnek uygulamayı çalıştırabilmeniz için önce Azure portalında Azure AD kiracınız ile kaydeder ve Azure Resource Manager API için izinler. İçinde sağlanan adımları izleyerek [Active Directory ile kimlik doğrulaması Batch yönetimi çözümleri](batch-aad-auth-management.md).


[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Azure Active Directory nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]:../active-directory/develop/authentication-scenarios.md "Azure AD için kimlik doğrulama senaryoları"
[aad_integrate]:../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md "Uygulamaları Azure Active Directory ile tümleştirme"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: https://portal.azure.com
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
