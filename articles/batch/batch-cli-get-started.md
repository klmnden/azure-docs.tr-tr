---
title: "Batch için Azure CLI kullanmaya başlama | Microsoft Docs"
description: "Azure Batch hizmet kaynaklarını yönetmek üzere Azure CLI’daki Batch komutlarına hızlı bir giriş yapın"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 01/23/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 698c481e2eff5e0a3b893a0377d9f4cd2f052eb4
ms.lasthandoff: 03/21/2017


---
# <a name="manage-batch-resources-with-azure-cli"></a>Batch kaynaklarını Azure CLI ile yönetme

Platformlar arası Azure Komut Satırı Arabirimi (Azure CLI); Linux, Mac ve Windows komut kabuklarında Batch hesaplarınızı ve havuzlar, işler ve görevler gibi kaynakları Kabukları görevler gibi kaynakları yönetmenizi sağlar. Azure Batch CLI ile Batch API'leri, Azure portalı ve Batch PowerShell cmdlet’leri ile gerçekleştirdiğiniz görevlerin çoğunu gerçekleştirebilir ve betik oluşturabilirsiniz.

Bu makale Azure CLI sürüm 0.10.5’i temel alır.

## <a name="prerequisites"></a>Önkoşullar
* [Azure CLI'yı yükleme](../cli-install-nodejs.md)
* [Azure CLI’yı Azure aboneliğinize bağlama](../xplat-cli-connect.md)
* **Resource Manager moduna** geçin: `azure config mode arm`

> [!TIP]
> Hizmet güncelleştirmeleri ve geliştirmelerinden yararlanmak için Azure CLI yüklemenizi sık sık güncelleştirmeniz önerilir.
> 
> 

## <a name="command-help"></a>Komut yardımı
Komuttan sonra tek seçenek olarak `-h` ekleyerek Azure CLI’daki her komut için yardım metni görüntüleyebilirsiniz. Örneğin:

* `azure` komutuyla ilgili yardım almak için şunu girin: `azure -h`
* CLI’daki tüm Batch komutlarının listesini almak için şunu kullanın: `azure batch -h`
* Batch hesabı oluşturmayla ilgili yardım almak için şunu girin: `azure batch account create -h`

Emin olmadığınızda herhangi bir Azure CLI komutuyla ilgili yardım almak için `-h` komut satırı seçeneğini kullanın.

## <a name="create-a-batch-account"></a>Batch hesabı oluşturma
Kullanım:

    azure batch account create [options] <name>

Örnek:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Belirtilen parametrelerle yeni bir Batch hesabı oluşturur. En az bir konum, kaynak grubu ve hesap adı belirtmeniz gerekir. Henüz bir kaynak grubunuz yoksa `azure group create` çalıştırarak bir tane oluşturun ve `--location` seçeneği için Azure bölgelerinden birini (örneğin "Batı ABD") belirtin. Örneğin:

    azure group create --name "resgroup001" --location "West US"

> [!NOTE]
> Batch hesabının adı, hesabın oluşturulduğu Azure bölgesinde benzersiz olmalıdır. Bu ad yalnızca küçük alfasayısal karakterler içerebilir ve 3-24 karakter uzunluğunda olmalıdır. Batch hesabı adlarında `-` veya `_` gibi özel karakterler kullanamazsınız.
> 
> 

### <a name="linked-storage-account-autostorage"></a>Bağlantılı depolama hesabı (otomatik depolama)
**Genel amaçlı** bir Depolama hesabını (isteğe bağlı olarak) oluşturduğunuz Batch hesabına bağlayabilirsiniz. Batch [uygulama paketleri](batch-application-packages.md) özelliği, [Batch Dosya Kuralları .NET](batch-task-output.md) kitaplığının yaptığı gibi Genel amaçlı bir Storage hesabında blob depolama kullanır. Bu isteğe bağlı özellikler Batch görevlerinizin çalıştırdığı uygulamaları dağıtmanıza ve oluşturduğu verileri kalıcı hale getirmeniz yardımcı olur.

Var olan bir Azure Depolama hesabını yeni oluşturduğunuz bir Batch hesabına bağlamak için `--autostorage-account-id` seçeneğini belirtin. Bu seçenek, depolama hesabının tam kaynak kimliğini gerektirir.

İlk olarak, depolama hesabınızın ayrıntılarını görüntüleyin:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Ardından `--autostorage-account-id` seçeneği için **Url** değerini kullanın. Url değeri "/subscriptions/" ile başlar ve abonelik kimliğiniz ile Depolama hesabının kaynak yolunu içerir:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Batch hesabını silme
Kullanım:

    azure batch account delete [options] <name>

Örnek:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Belirtilen Batch hesabını siler. Sorulduğunda hesabı kaldırmak istediğinizi onaylayın (hesap kaldırma işleminin tamamlanması biraz zaman alabilir).

## <a name="manage-account-access-keys"></a>Hesap erişim anahtarlarını yönetme
Batch hesabınızda [kaynak oluşturmak ve değiştirmek](#create-and-modify-batch-resources) için bir erişim anahtarı gereklidir.

### <a name="list-access-keys"></a>Erişim anahtarlarını listele
Kullanım:

    azure batch account keys list [options] <name>

Örnek:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Belirtilen Batch hesabı için hesap anahtarlarını listeler.

### <a name="generate-a-new-access-key"></a>Yeni erişim anahtarı oluşturma
Kullanım:

    azure batch account keys renew [options] --<primary|secondary> <name>

Örnek:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

İlgili Batch hesabı için belirtilen hesap anahtarını yeniden oluşturur.

## <a name="create-and-modify-batch-resources"></a>Batch kaynaklarını oluşturma ve değiştirme
Havuzlar, işlem düğümleri, işler ve görevler gibi Batch kaynaklarını oluşturmak, okumak, güncelleştirmek ve silmek (CRUD) için Azure CLI kullanabilirsiniz. Bu CRUD işlemleri Batch hesabı adı, erişim anahtarı ve uç nokta bilgilerini gerektirir. Bunları `-a`, `-k` ve `-u` seçenekleriyle birlikte belirtebilir veya CLI’nın otomatik olarak kullandığı [ortam değişkenlerini](#credential-environment-variables) ayarlayabilirsiniz (doldurulmuşsa).

### <a name="credential-environment-variables"></a>Kimlik bilgisi ortam değişkenleri
Yürüttüğünüz her komut için komut satırında `-a`, `-k` ve `-u` seçeneklerini belirtmek yerine `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY` ve `AZURE_BATCH_ENDPOINT` ortam değişkenlerini ayarlayabilirsiniz. Batch CLI `-a`, `-k` ve `-u` seçeneklerini atlayabilmeniz için bu değişkenleri kullanır (ayarlanmışsa). Bu makalenin geri kalanında bu ortam değişkenlerinin kullanıldığı varsayılmaktadır.

> [!TIP]
> Anahtarlarınızı `azure batch account keys list` ile listeleyin ve `azure batch account show` ile hesabın uç noktasını görüntüleyin.
> 
> 

### <a name="json-files"></a>JSON dosyaları
Havuzlar ve işler gib Batch kaynakları oluşturduğunuzda parametrelerini komut satırı seçenekleri olarak geçirmek yerine yeni kaynağın yapılandırmasını içeren bir JSON dosyası belirtebilirsiniz. Örneğin:

`azure batch pool create my_batch_pool.json`

Yalnızca komut satırı seçeneklerini kullanarak çok sayıda kaynak oluşturma işlemi gerçekleştirilebilse de bazı özellikler, kaynak ayrıntılarını içeren JSON biçimli bir dosya gerektirir. Örneğin, bir başlatma görevi için kaynak belirtmek istiyorsanız bir JSON dosyası kullanmanız gerekir.

Kaynak oluşturmak için gereken JSON dosyasını bulmak üzere MSDN üzerindeki [Batch REST API başvurusu][rest_api] belgelerine bakın. Her "*Kaynak türü* ekleme" konusu kaynağı oluşturmaya yönelik örnek JSON dosyasını içerir ve bu dosyayı JSON dosyalarınıza yönelik şablon olarak kullanabilirsiniz. Örneğin, havuz oluşturmaya yönelik JSON dosyası [Hesaba havuz ekleme][rest_add_pool] konusunda bulunabilir.

> [!NOTE]
> Bir kaynak oluştururken JSON dosyası belirtirseniz komut dosyasında ilgili kaynak için belirttiğiniz diğer tüm parametreler yok sayılır.
> 
> 

## <a name="create-a-pool"></a>Havuz oluşturma
Kullanım:

    azure batch pool create [options] [json-file]

Örnek (Sanal Makine Yapılandırması):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Örnek (Cloud Services Yapılandırması):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Batch hizmetinde işlem düğümlerinin bir havuzunu oluşturur.

[Batch özelliğine genel bakış](batch-api-basics.md#pool) bölümünde anlatıldığı gibi, havuzunuzdaki düğümler için bir işletim sistemi seçtiğiniz iki seçeneğiniz vardır: **Sanal Makine Yapılandırması** ve **Cloud Services Yapılandırması**. Sanal Makine Yapılandırma havuzları oluşturmak için `--image-*` seçeneklerini, Cloud Services Yapılandırma havuzları oluşturmak için `--os-family` seçeneğini kullanın. Hem `--os-family` hem de `--image-*` seçeneğini belirtemezsiniz.

Bir [başlangıç görevi](batch-api-basics.md#start-task) için havuz [uygulama paketleri](batch-application-packages.md) ve komut satırı belirtebilirsiniz. Ancak, başlangıç görevine yönelik kaynak dosyaları belirtmek için bunun yerine bir [JSON dosyası](#json-files) kullanmanız gerekir.

Bir havuzu şununla silin:

    azure batch pool delete [pool-id]

> [!TIP]
> `--image-*` seçeneklerine uygun değerler için [sanal makine görüntüleri listesini](batch-linux-nodes.md#list-of-virtual-machine-images) kontrol edin.
> 
> 

## <a name="create-a-job"></a>Bir iş oluşturma
Kullanım:

    azure batch job create [options] [json-file]

Örnek:

    azure batch job create --id "job001" --pool-id "pool001"

Bir işi Batch hesabına ekler ve görevlerinin yürütüleceği havuzu belirtir.

Bir işi şununla silin:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Havuzlar, işler, görevler ve diğer kaynakları listeleme
Her Batch kaynak türü, Batch hesabınızı sorgulayan ve bu türdeki kaynakları listeleyen bir `list` komutunu destekler. Örneğin, hesabınızdaki havuzları ve bir işteki görevleri listeleyebilirsiniz:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Kaynakları verimli bir şekilde listeleme
Daha hızlı sorgulama için `list` işlemlerine yönelik **select**, **filter** ve **expand** yan tümcesi seçeneklerini belirtebilirsiniz. Batch hizmeti tarafından döndürülen veri miktarını sınırlamak için bu seçenekleri kullanın. Tüm filtreleme sunucu tarafında oluştuğu için yalnızca ilgilendiğiniz veriler kabloya geçer. Liste işlemleri gerçekleştirirken bant genişliğinden (ve böylece zamandan) tasarruf etmek için bu yan tümceleri kullanın.

Örneğin, bu yan tümce yalnızca kimlikleri "renderTask" ile başlayan havuzları döndürür:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Batch CLI, Batch hizmeti tarafından desteklenen üç yan tümcenin tamamını destekler:

* `--select-clause [select-clause]`  Her varlık için bir özellik alt kümesi döndürür
* `--filter-clause [filter-clause]`  Yalnızca belirtilen OData ifadesiyle eşleşen varlıklar döndürür
* `--expand-clause [expand-clause]`  Tek bir temel alınan REST çağrısındaki varlık bilgilerini elde edin. Expand yan tümcesi şu anda yalnızca `stats` özelliğini desteklemektedir.

Üç yan tümceye ilişkin ayrıntılar ve bunlarla liste sorguları gerçekleştirme hakkında bilgi için bkz. [Azure Batch hizmetini verimli bir şekilde sorgulama](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Uygulama paketi yönetimi
Uygulama paketleri havuzlarınızdaki işlem düğümlerine uygulama dağıtmanın basit bir yolunu sağlar. Azure CLI ile uygulama paketlerini karşıya yükleyebilir, paket sürümlerini yönetebilir ve paketleri silebilirsiniz.

Yeni bir uygulama oluşturmak ve bir paket sürümü eklemek için:

Bir uygulama **oluşturun**:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

Bir uygulama paketi **ekleyin**:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

Paketin **etkinleştirin**:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Uygulamanın **varsayılan sürümünü** ayarlayın:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Uygulama paketi dağıtma
Yeni bir havuz oluşturduğunuzda dağıtım için bir veya daha fazla uygulama paketi belirtebilirsiniz. Havuz oluşturma saatinde bir paket belirttiğinizde düğüm havuza katıldıkça her bir düğüme dağıtılır. Paketler ayrıca bir düğüm yeniden başlatıldığında veya yeniden görüntüsü oluşturulduğunda dağıtılır.

Bir uygulama paketini havuza katıldıklarında havuzun düğümlerine dağıtmak üzere havuz oluştururken `--app-package-ref` seçeneğini belirtin. `--app-package-ref` seçeneği, işlem düğümlerine dağıtılacak uygulama kimliklerinin noktalı virgülle ayrılmış bir listesini kabul eder.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Şu anda, komut satırı seçeneklerini kullanarak bir havuz oluştururken işlem düğümlerine *hangi* uygulama paketi sürümünün dağıtılacağını belirtemezsiniz (örneğin, "1.10-beta3"). Bu nedenle, havuzu oluşturmadan önce ilk olarak `azure batch application set [options] --default-version <version-id>` ile uygulamanın varsayılan sürümünü belirtmeniz gerekir (önceki bölüme bakın). Ancak, havuz oluştururken komut satırı seçenekleri yerine bir [JSON dosyası](#json-files) kullanırsanız havuzun paket sürümünü belirtebilirsiniz.

[Azure Batch uygulama paketleri ile uygulama dağıtımı](batch-application-packages.md) içinde uygulama paketlerine ilişkin daha fazla bilgi bulabilirsiniz.

> [!IMPORTANT]
> Uygulama paketlerini kullanmak için Batch hesabınıza [bir Azure Depolama hesabı bağlamanız](#linked-storage-account-autostorage) gerekir.
> 
> 

### <a name="update-a-pools-application-packages"></a>Bir havuzun uygulama paketlerini güncelleştirme
Var olan bir havuza atanan uygulamaları güncelleştirmek için `azure batch pool set` komutunu `--app-package-ref` seçeneği ile verin:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Yeni uygulama paketini var olan havuzda zaten olan işlem düğümlerine dağıtmak için bu düğümleri yeniden başlatmanız ya da görüntülerini yeniden oluşturmanız gerekir:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

> [!TIP]
> `azure batch node list` ile bir havuzdaki düğümlerin listesini, düğüm kimlikleriyle birlikte edinebilirsiniz.
> 
> 

Uygulamayı dağıtımdan önce varsayılan bir sürümle yapılandırmış olmanız gerekir (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
Bu bölümde Azure CLI ile ilgili sorunları giderirken kullanılacak kaynakları sağlanması amaçlanmıştır. Tüm sorunları çözmesi kesin değildir, ancak nedenleri elemenize yardımcı olabilir ve sizi yardım kaynaklarına yönlendirebilir.

* Herhangi bir CLI komutu için **yardım metni** almak üzere `-h` kullanın
* **Ayrıntılı** komut çıktısını görüntülemek için `-v` ve `-vv` kullanın; `-vv` "fazladan" ayrıntılıdır ve gerçek REST istekleri ile yanıtlarını gösterir. Bu anahtarlar tam hata çıktısını görüntülemek için kullanışlıdır.
* `--json` seçeneği ile **komut çıktısını JSON olarak** görüntüleyebilirsiniz. Örneğin, `azure batch pool show "pool001" --json` seçeneği pool001'in özelliklerini JSON biçiminde gösterir. Bundan sonra bu çıktıyı kopyalayıp bir `--json-file` içinde kullanmak üzere değiştirebilirsiniz (bu makalenin başındaki [JSON dosyaları](#json-files) kısmına bakın).
* [MSDN üzerindeki Batch forumu][batch_forum] çok yararlı bir yardım kaynağıdır ve Batch ekip üyeleri tarafından yakından izlenmektedir. Sorun yaşamanız veya belirli bir işlemle ilgili yardım almak istemeniz durumunda sorularınızı gönderdiğinizden emin olun.
* Şu anda her Batch kaynak işlemi Azure CLI tarafından desteklenmemektedir. Örneğin, şu anda bir havuz için uygulama paketi *sürümü* belirtemezken, yalnızca paket kimliği belirtebilirsiniz. Böyle durumlarda komut satırı seçeneklerini kullanmak yerine komutunuz için bir `--json-file` belirtmeniz gerekebilir. Gelecekteki geliştirmeleri seçmek en son CLI sürümü ile güncel kaldığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* Bu özelliği kullanarak Batch işlem düğümleri üzerinde yürüttüğünü uygulamaları yönetme ve dağıtma hakkında bilgi almak için bkz. [Azure Batch uygulama paketleriyle uygulama dağıtımı](batch-application-packages.md).
* Batch’e sorgular için döndürülen bilgi türleri ve öğe sayısını azaltma hakkında bilgi için bkz. [Batch hizmetini etkin bir şekilde sorgulama](batch-efficient-list-queries.md).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx

