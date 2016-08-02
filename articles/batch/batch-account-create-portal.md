<properties
    pageTitle="Azure Batch hesabı oluşturma | Microsoft Azure"
    description="Büyük ölçekli paralel iş yükleri bulutta çalıştırmak için Azure portalda bir Azure Batch hesabı oluşturmayı öğrenin"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="marsma"/>

# Azure portalda bir Azure Batch hesabı oluşturma ve yönetme

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Batch Yönetimi .NET](batch-management-dotnet.md)

[Azure portal][azure_portal] büyük ölçekli paralel iş yükünü işlemek için kullanabileceğiniz bir Azure Batch hesabını yönetmek için ihtiyacınız olan araçları sağlar. Ancak biz bu makalede, portal kullanarak Batch hesabı oluşturmayı adım adım anlatacak ve bir Batch hesabının önemli ayarlarını ve özelliklerini vurgulayacağız. Örneğin, Batch ile geliştirdiğiniz uygulamalar ve hizmetler Batch hizmetiyle iletişim için, her ikisi de Azure portalda bulunan, hesabınızın URL’sine ve erişim anahtarına ihtiyaç duyar.

>[AZURE.NOTE] Azure portal şu anda hesap oluşturma, Batch hesabı ayarlarını ve özelliklerini yönetme ve havuzlar ve işler oluşturma ve izleme dahil, Batch hizmetindeki özelliklerin bir alt grubunu desteklemektedir. Batch hizmetinin tüm özellikleri Batch API’leri aracılığıyla geliştiricilerin kullanımına sunulmuştur.

## Batch hesabı oluşturma

1. [Azure portal][azure_portal]’da oturum açın.

2. **Yeni** > **Sanal Makineler** > **Batch Hizmeti**’ne tıklayın.

    ![Market’te Batch][marketplace_portal]

3. **Yeni Batch Hesabı** dikey penceresi görüntülenir. Her bir dikey pencere öğesinin açıklaması için aşağıda *a* ile *e* arası öğelere bakın.

    ![Batch hesabı oluşturma][account_portal]

    a. **Hesap adı**: Batch hesabı için benzersiz bir ad. Bu ad hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır (aşağıdaki *Konum*’a bakın). Bu, yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    b. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.

    c. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubu ya da isterseniz yeni bir tane oluşturabilirsiniz.

    d. **Konum**: Batch hesabının oluşturulacağı bir Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.

    e. **Depolama hesabı** (isteğe bağlı): Yeni Batch hesabınızla ilişkilendireceğiniz (bağlayacağınız) **Genel amaçlı** bir depolama hesabı. Batch hizmetinin [uygulama paketleri](batch-application-packages.md) özelliği bağlı depolama hesabını uygulama paketlerini depolamak ve almak için kullanır. Bu özellik hakkında daha fazla bilgi için bkz. [Azure Batch uygulama paketleriyle uygulama dağıtımı](batch-application-packages.md) .

     > [AZURE.IMPORTANT] Bağlı bir Depolama hesabında anahtarları yeniden oluşturma özel durumların dikkate alınmasını gerektirir. Daha fazla ayrıntı için bkz. [Batch hesaplarıyla ilgili dikkat edilecek noktalar](#considerations-for-batch-accounts)

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.

  Portal, hesabı **Dağıtmakta** olduğunu gösterir ve tamamlandıktan sonra, *Bildirimler* alanında bir **Dağıtımlar başarıyla tamamlandı** bildirimi görüntülenir.

## Batch hesabı özelliklerini görüntüleme

Batch hesabı dikey penceresi hesapla ilgili çeşitli özellikleri görüntülemenin yanı sıra erişim anahtarları, kotalar, kullanıcılar ve depolama hesabı ilişkisi gibi ek ayarlara erişim sağlar.

* **Batch hesabı URL'si**: Bu URL [Batch REST][api_rest] API ya da [Batch .NET][api_net] istemci kitaplığı gibi API’leri kullanırken Batch hesabınıza erişim imkanı sağlar ve aşağıdaki biçimlere uyar:

    `https://<account_name>.<region>.batch.azure.com`

* **Erişim anahtarları**: Batch hesabınızın erişim anahtarlarını görüntülemek ve yönetmek için, **Anahtarları yönet** dikey penceresini açmak üzere anahtar simgesine tıklayın veya **Tüm ayarları** > **Anahtarlar**’a tıklayın. [Batch REST][api_rest] ya da [Batch .NET][api_net] istemci kitaplığı gibi Batch hizmeti API’leriyle iletişim kurarken bir erişim anahtarı gerekir.

    ![Batch hesabı anahtarları][account_keys]

* **Tüm ayarlar**: Batch hesabı için tüm ayarları yönetmek veya özelliklerini görüntülemek için, **Ayarlar** dikey penceresini açmak üzere **Tüm ayarlar**’a tıklayın. Bu dikey pencere erişim hesabına ilişkin hesap kotalarını görüntüleme, Batch hesabına bağlamak için bir Azure Storage hesabı seçme ve kullanıcıları yönetme dahil, tüm ayarlara ve özelliklere ve erişim imkanı sağlar.

    ![Batch hesabı ayarları ve özellikleri dikey pencereleri][5]

## Batch hesaplarıyla ilgili dikkat edilecek noktalar

* [Batch PowerShell cmdlet’leri](batch-powershell-cmdlets-get-started.md) ve the [Batch Yönetimi .NET](batch-management-dotnet.md) kitaplığı ile de Batch hesapları oluşturabilir ve yönetebilirsiniz.

* Batch hesabının kendisi için ücretlendirilmezsiniz. Batch hesaplarınızın kullandığı Azure işlem kaynakları için ve iş yükleriniz çalıştığında diğer hizmetler tarafından kullanılan kaynaklar için ücretlendirilirsiniz Örneğin, havuzlarınızdaki işlem düğümlerini için ücretlendirilirsiniz ve [uygulama paketleri](batch-application-packages.md) özelliğini kullanırsanız, uygulama paketi sürümlerinizi depolamak için kullanılan Azure Storage kaynakları için ücretlendirilirsiniz. Daha fazla bilgi için bkz. [Batch fiyatlandırması][batch_pricing].

* Tek bir Batch hesabında birden fazla Batch iş yükü çalıştırabilir ya da iş yüklerinizi farklı Azure bölgelerindeki Batch hesapları arasında dağıtabilirsiniz.

* Birden fazla büyük ölçekli Batch iş yükü çalıştırıyorsanız, Azure aboneliğinize ve her bir Batch hesabınıza uygulanan [Batch hizmeti kotaları ve sınırlarına](batch-quota-limit.md) dikkat edin. Bir Batch hesabındaki geçerli kotalar hesap özelliklerindeki portalda görüntülenir.

* Batch hesabınızla bir depolama hesabı ilişkilendirirseniz (bağlarsanız), depolama hesabı erişim anahtarlarını yeniden oluştururken dikkatli olun. Yalnızca tek bir depolama hesabı anahtar anahtarını yeniden oluşturmalısınız, bağlı depolama hesabı dikey penceresinde **Anahtarları Eşitle**’ye tıklayın, anahtarın havuzlarınızdaki işlem düğümlerine yayılması için 5 dakika bekleyin ve ardından gerekirse diğer anahtarı yeniden oluşturun ve eşitleyin. İki anahtarı da aynı anda oluşturursanız, işlem düğümleriniz iki anahtarı da eşitleyemez ve anahtarlarının depolama hesabına erişimi kaybederler.

  ![Depolama hesabı anahtarlarını yeniden oluşturma][4]

> [AZURE.IMPORTANT] Batch şu anda, [Azure Storage hesapları hakkında](../storage/storage-create-storage-account.md) bölümünde 5. adım olan [Depolama hesabı oluşturma](../storage/storage-create-storage-account.md#create-a-storage-account)da açıklandığı gibi, *sadece* **Genel amaçlı** depolama hesabı türünü destekler. Batch hesabınıza bir Azure Storage hesabı bağladığınızda, *yalnızca* **Genel amaçlı** depolama hesabı bağlayın.

## Sonraki adımlar

* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Azure Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele alır ve büyük ölçekli işlem iş yükü yürütmeye olanak tanıyan hizmetin özelliklerine genel bir bakış sağlar. 

* [Batch .NET istemci kitaplığı](batch-dotnet-get-started.md)nı kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. [Giriş makalesi](batch-dotnet-get-started.md) birden fazla işlem düğümünde bir iş yükü yürütmek üzere Batch hizmetini kullanan çalışan bir uygulama için size rehberlik sağlar ve iş yükü dosyası hazırlama ve alma için Azure Storage kullanmayı içerir.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Depolama hesabı anahtarlarını yeniden oluşturma"
[5]: ./media/batch-account-create-portal/batch_acct_05.png "Batch hesabı ayarları ve özellikleri dikey pencereleri"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG



<!---HONumber=Jun16_HO2-->


