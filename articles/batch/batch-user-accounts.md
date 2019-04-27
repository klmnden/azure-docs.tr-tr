---
title: Kullanıcı hesaplarını - Azure Batch görevleri çalıştırma | Microsoft Docs
description: Azure Batch hizmetinde görev çalıştırmaya yönelik kullanıcı hesapları yapılandırma
services: batch
author: laurenhughes
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 000495ab84990f15885c254b472be7863c75da58
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549861"
---
# <a name="run-tasks-under-user-accounts-in-batch"></a>Kullanıcı hesaplarını görevleri Batch'de çalıştırma

Azure Batch görevinde, her zaman bir kullanıcı hesabı altında çalışır. Varsayılan olarak, görevler, yönetici izinleri olmayan standart kullanıcı hesapları altında çalışır. Bu varsayılan kullanıcı hesabının ayarlarını genellikle yeterli. Belirli senaryolar için ancak kullanıcı hesabı altında çalıştırılacak bir görev istediğiniz yapılandırabilmesi kullanışlıdır. Bu makalede, kullanıcı hesaplarını ve nasıl senaryonuz için yapılandırabilirsiniz türleri açıklanmaktadır.

## <a name="types-of-user-accounts"></a>Kullanıcı hesabı türleri

Azure Batch, görevleri çalıştırmak için iki tür kullanıcı hesapları sağlar:

- **Otomatik kullanıcı hesapları.** Otomatik kullanıcı hesapları Batch hizmeti tarafından otomatik olarak oluşturulan yerleşik kullanıcı hesaplarıdır. Varsayılan olarak, görevler, bir otomatik kullanıcı hesabı altında çalışır. Bir görev hangi otomatik kullanıcı hesabı altında çalışması gerektiğini belirtmek bir görev için otomatik kullanıcı belirtimi yapılandırabilirsiniz. Otomatik kullanıcı belirtimi görevi çalıştıracak otomatik kullanıcı hesabına kapsamını ve ayrıcalık düzeyini belirtmenize olanak verir. 

- **Adlandırılmış kullanıcı hesabı.** Havuz oluşturduğunuzda, bir havuz için bir veya daha fazla adlandırılmış kullanıcı hesabı belirtebilirsiniz. Her kullanıcı hesabı, havuzdaki her düğümde oluşturulur. Ek olarak hesap adı kullanıcı hesabı parolası, ayrıcalık düzeyi ve Linux havuzlarında SSH özel anahtarı belirtin. Bir görev eklediğinizde, bu görev, altında çalışması gerektiğini adlı bir kullanıcı hesabı belirtebilirsiniz.

> [!IMPORTANT] 
> Batch hizmeti sürüm 2017-01-01.4.0 bu sürümü çağırmak için kodunuzu güncelleştirmenizi gerektiren bir değişiklik sunuyor. Batch daha eski bir sürümünden geçirme kodu varsa, dikkat **runElevated** özelliği, REST API veya Batch istemci kitaplıkları artık desteklenmiyor. Yeni **Userıdentity** özelliği ayrıcalık düzeyini belirtmek için bir görev. Başlıklı bölüme bakın [son Batch istemci kitaplığını için kodunuzu güncelleştirin](#update-your-code-to-the-latest-batch-client-library) istemci kitaplıklarından birini kullanıyorsanız, Batch kodunuzu güncelleme için hızlı yönergeler için.
>
>

> [!NOTE] 
> Bu makalede ele alınan kullanıcı hesaplarına Uzak Masaüstü Protokolü (RDP) veya güvenli Kabuk (SSH), güvenlikle ilgili nedenlerle desteklemez. 
>
> Linux sanal makine yapılandırması SSH üzerinden çalışan bir düğüme bağlanmak için bkz: [azure'da bir Linux sanal makinesi için Uzak Masaüstü kullanım](../virtual-machines/virtual-machines-linux-use-remote-desktop.md). Windows RDP aracılığıyla çalışan düğümlerine bağlanmak için bkz: [Windows Server VM'sine bağlanma](../virtual-machines/windows/connect-logon.md).<br /><br />
> Bulut hizmeti yapılandırmasını çalıştıran RDP aracılığıyla bir düğüme bağlanmak için bkz: [Azure Cloud Services'ta bir rol için Uzak Masaüstü Bağlantısı etkinleştirme](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).
>
>

## <a name="user-account-access-to-files-and-directories"></a>Dosyalar ve dizinler için kullanıcı hesabı erişimi

Otomatik kullanıcı hesabı hem de bir adlandırılmış kullanıcı hesabı ve görevin çalışma dizini, paylaşılan dizine ve çok örnekli görevler dizin okuma/yazma erişimi. Her iki hesap türü başlangıç ve iş hazırlama dizinlerinin okuma erişimi.

Bir görev, bir başlangıç görevi çalıştırmak için kullanılan hesabın altında çalışıyorsa, görevin başlangıç görev dizininde okuma-yazma erişimi vardır. Benzer şekilde, bir görev bir iş hazırlama görevi çalıştırmak için kullanılan hesabın altında çalışıyorsa, görev iş hazırlama görevi dizini okuma-yazma erişimi vardır. Bir görev, iş hazırlama görevi ve başlangıç görevinin farklı bir hesap altında çalışıyorsa, görev ilgili dizin yalnızca okuma erişimi vardır.

Dosyalar ve dizinler bir görevden erişme hakkında daha fazla bilgi için bkz: [büyük ölçekli paralel işlem çözümleri geliştirme batch'le](batch-api-basics.md#files-and-directories).

## <a name="elevated-access-for-tasks"></a>Görevler için yükseltilmiş erişim 

Kullanıcı hesabının ayrıcalık düzeyi, bir görev yükseltilmiş erişim ile çalışıp çalışmayacağını gösterir. Otomatik kullanıcı hesabı hem de bir adlandırılmış kullanıcı hesabı ile yükseltilmiş erişim çalıştırabilirsiniz. Ayrıcalık düzeyi için iki seçeneği şunlardır:

- **NonAdmin:** Yükseltilmiş erişimi olmayan standart bir kullanıcı olarak görev çalışır. Toplu kullanıcı hesabı için varsayılan ayrıcalık düzeyi her zaman olduğu **NonAdmin**.
- **Yönetici:** Görev, bir kullanıcı olarak yükseltilmiş erişim ile çalışır ve tam yönetici izinleri ile çalışır. 

## <a name="auto-user-accounts"></a>Otomatik kullanıcı hesapları

Varsayılan olarak, görevler yükseltilmiş erişim olmadan ve görev kapsamlı bir standart kullanıcı olarak otomatik kullanıcı hesabı altında Batch hizmetinde çalışır. Batch hizmeti, otomatik kullanıcı belirtimi için görev kapsam yapılandırıldığında, yalnızca bu görev için bir otomatik kullanıcı hesabı oluşturur.

Görev kapsama havuzu kapsam alternatiftir. Bir görev için otomatik kullanıcı belirtimi için havuz kapsam yapılandırıldığında, görev, havuzdaki herhangi bir görev için kullanılabilir olan bir otomatik kullanıcı hesabı altında çalışır. Havuz kapsamı hakkında daha fazla bilgi için otomatik kullanıcı havuzu kapsamlı olarak bir görevi çalıştırma başlıklı bölüme bakın.   

Varsayılan kapsam, Windows ve Linux düğümlerinde farklıdır:

- Windows düğümlerine, görevler, görev kapsamında varsayılan olarak çalışır.
- Linux düğümleri havuzu kapsam altında her zaman çalışır.

Her biri benzersiz otomatik kullanıcı hesabına karşılık gelen otomatik kullanıcı belirtimi için dört olası yapılandırmalar şunlardır:

- (Varsayılan otomatik kullanıcı belirtimi) görev kapsamlı yönetici olmayan erişim
- Görev kapsama sahip yönetici (yükseltilmiş) erişimi
- Havuz kapsama sahip yönetici olmayan erişim
- Havuz kapsama sahip yönetici erişimi

> [!IMPORTANT] 
> Görev kapsam altında çalışan görevler pratikte diğer görevler için bir düğümde erişiminiz yok. Ancak, kötü niyetli bir kullanıcı hesabı erişimi olan yönetici ayrıcalıklarıyla çalıştırır ve diğer görev dizinleri erişen bir görev göndererek bu kısıtlamayı çözmek işe yarayabilir. Kötü niyetli bir kullanıcı, bir düğüme bağlanmak için RDP veya SSH de kullanabilirsiniz. Bu tür bir senaryonun önlemek için Batch hesabı anahtarları erişimi korumak önemlidir. Hesabınızı tehlikeye girmiş olabilecek şüpheleniyorsanız anahtarlarınızı yeniden emin olun.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Yükseltilmiş erişimi olan bir otomatik kullanıcı olarak bir görevi çalıştırma

Yükseltilmiş erişim ile bir görev çalıştırmanız gerektiğinde yönetici ayrıcalıkları için otomatik kullanıcı belirtimi yapılandırabilirsiniz. Örneğin, bir başlangıç görevi, yazılım düğümünde yüklemek için yükseltilmiş erişim gerekebilir.

> [!NOTE] 
> Genel olarak, yükseltilmiş erişim yalnızca gerekli olduğunda kullanmak en iyisidir. En iyi uygulamalar, arzu edilen sonucu elde etmek gerekli en düşük ayrıcalık verilmesi önerilir. Örneğin, bir başlangıç görevi geçerli kullanıcıya yönelik yazılımları yerine tüm kullanıcılar için yüklerse görevlere yükseltilmiş erişimi vermekten kaçının mümkün olabilir. Başlangıç görevi de dahil olmak üzere aynı hesabı altında çalıştırılması gereken tüm görevleri için havuz kapsamı ve yönetici olmayan erişim için otomatik kullanıcı belirtimi yapılandırabilirsiniz. 
>
>

Aşağıdaki kod parçacıkları otomatik kullanıcı belirtimi yapılandırma işlemini göstermektedir. Örnekler ayrıcalık düzeyini ayarlar `Admin` ve kapsamını `Task`. Görev kapsamı, varsayılan ayardır, ancak üzere buraya dahil edilir.

#### <a name="batch-net"></a>Batch .NET

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a>Batch Java

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a>Batch Python

```python
user = batchmodels.UserIdentity(
    auto_user=batchmodels.AutoUserSpecification(
        elevation_level=batchmodels.ElevationLevel.admin,
        scope=batchmodels.AutoUserScope.task))
task = batchmodels.TaskAddParameter(
    id='task_1',
    command_line='cmd /c "echo hello world"',
    user_identity=user)
batch_client.task.add(job_id=jobid, task=task)
```

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a>Otomatik kullanıcı havuzu kapsamlı olarak bir görevi çalıştırma

Bir düğüm sağlandığında her düğümünde yükseltilmiş erişimi olmayan bir havuz ve yükseltilmiş erişim ile bir iki havuzu genelinde otomatik kullanıcı hesapları oluşturulur. Belirli bir görev için havuz kapsam için otomatik kullanıcı kapsamı ayarlama, bu iki havuzu genelinde otomatik kullanıcı hesaplarından birine altında görev çalışır. 

Otomatik-kullanıcı, aynı havuz genelinde otomatik kullanıcı hesabı altında Çalıştır yönetici erişimi ile çalışan tüm görevler için havuz kapsam belirttiğinizde. Benzer şekilde, yönetici izinleri olmadan çalışan görevler de tek bir havuzu genelinde otomatik-kullanıcı hesabı altında çalıştırın. 

> [!NOTE] 
> İki havuzu genelinde otomatik kullanıcı hesapları, ayrı hesaplarıdır. Havuzu genelinde yönetici hesabı altında çalışan görevler veri standart hesabı altında ve tersi çalışan görevler ile paylaşım yapamazsınız. 
>
>

Aynı otomatik kullanıcı hesabı altında çalışan avantajı, görevleri aynı düğümde çalışmasını diğer görevlerle veri paylaşamaz olmasıdır.

Gizli dizileri görevler arasında paylaşımı iki havuzu genelinde otomatik kullanıcı hesaplarından birini görevleri çalıştıran kullanışlı olduğu bir senaryodur. Örneğin, diğer görevlerin kullanabileceğiniz düğümü üzerine bir gizli dizi sağlamak bir başlangıç görevi gerektiğini varsayalım. Windows veri koruma API'si (DPAPI) kullanabilirsiniz, ancak yönetici ayrıcalıkları gerektirir. Bunun yerine, gizli dizi kullanıcı düzeyinde koruyabilir. Aynı kullanıcı hesabı altında çalışan görevler gizli yükseltilmiş erişim olmadan erişebilir.

Başka bir senaryo görevler otomatik kullanıcı hesabı altında havuzu kapsamıyla çalıştırmak istediğiniz, ileti geçirme arabirimi (MPI) dosya paylaşımıdır. MPI bir dosya paylaşımı, aynı dosya çalışmanızı MPI görev düğümlerin gerektiğinde faydalıdır. Baş düğüm aynı otomatik kullanıcı hesabı altında çalışıyorsa, alt düğümleri erişebileceği bir dosya paylaşımı oluşturur. 

Aşağıdaki kod parçacığı, Batch. NET'te bir görev için havuz kapsam için otomatik kullanıcı kapsamı ayarlar. Ayrıcalık düzeyi atlanırsa, bu nedenle görev standart havuz genelinde otomatik kullanıcı hesabı altında çalışıyor.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>Adlandırılmış kullanıcı hesapları

Bir havuz oluşturduğunuzda, adlandırılmış kullanıcı hesaplarını tanımlayabilirsiniz. Adlı bir kullanıcı hesabı adı ve sağladığınız parola sahiptir. Adlı bir kullanıcı hesabı için ayrıcalık düzeyini belirtebilirsiniz. Linux düğümleri için SSH özel anahtarı da sağlayabilir.

Adlı bir kullanıcı hesabı, havuzdaki tüm düğümlerde var ve bu düğümler üzerinde çalışan tüm görevler tarafından kullanılabilir. Adlandırılmış kullanıcılar bir havuz için herhangi bir sayıda tanımlayabilir. Bir görevi veya görev koleksiyonu eklediğinizde, görevi havuzda tanımlı adlandırılmış kullanıcı hesaplarından biri altında çalıştığını belirtebilirsiniz.

Adlı bir kullanıcı hesabı, aynı kullanıcı hesabı altında bir işteki tüm görevler çalıştırın, ancak bunları aynı anda çalışan diğer iş görevlerinden yalıtmak istediğinizde yararlıdır. Örneğin, her proje için adlandırılmış bir kullanıcı oluşturun ve her işin görevleri bu adlı bir kullanıcı hesabı altında çalıştırın. Her bir iş, ardından kendi görevlere, ancak diğer işlerin görevleri bir gizli dizi paylaşabilirsiniz.

Adlı bir kullanıcı hesabı, dosya paylaşımları gibi dış kaynaklar üzerindeki izinleri ayarlar bir görevi çalıştırmak için de kullanabilirsiniz. Bir adlandırılmış kullanıcı hesabıyla kullanıcı kimlik denetimi ve kullanıcı kimliği izinleri ayarlamak için kullanabilirsiniz.  

Adlandırılmış kullanıcı hesaplarının parola olmadan SSH Linux düğümleri arasında etkinleştirin. Çok örnekli görevleri çalıştırmak için gereken Linux düğümleri bir adlandırılmış kullanıcı hesabı kullanabilirsiniz. Havuzdaki her düğüme tam havuzunu üzerinde tanımlı bir kullanıcı hesabı altında görevler çalıştırabilirsiniz. Çok örnekli görevler hakkında daha fazla bilgi için bkz. [çoklu kullanın\-örneği MPI uygulamalarını çalıştırmak için görevleri](batch-mpi.md).

### <a name="create-named-user-accounts"></a>Adlandırılmış kullanıcı hesapları oluşturun

Batch hizmetinde adlandırılmış kullanıcı hesaplarını oluşturmak için kullanıcı hesaplarını koleksiyonunu havuzuna ekleyin. Aşağıdaki kod parçacıkları, .NET, Java ve Python adlandırılmış kullanıcı hesapları oluşturma işlemini gösterir. Bu kod parçacıkları, hem yönetim hem de havuz hesaplarında adlı yönetici olmayan oluşturma işlemi gösterilmektedir. Örnekler kullanarak bulut hizmeti yapılandırması havuzları oluşturur, ancak sanal makine Yapılandırması'nı kullanarak Windows veya Linux havuzu oluştururken, aynı yaklaşımı kullanın.

#### <a name="batch-net-example-windows"></a>Batch .NET örnek (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,
    virtualMachineSize: "standard_d1_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));   

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount("adminUser", "xyz123", ElevationLevel.Admin),
    new UserAccount("nonAdminUser", "123xyz", ElevationLevel.NonAdmin),
};

// Commit the pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a>Batch .NET örnek (Linux)

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the virtual machine configuration to use to create the pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create the unbound pool.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                             
    virtualMachineSize: "Standard_A1",                                      
    virtualMachineConfiguration: virtualMachineConfiguration);                  

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount(
        name: "adminUser",
        password: "xyz123",
        elevationLevel: ElevationLevel.Admin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 12345,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
    new UserAccount(
        name: "nonAdminUser",
        password: "123xyz",
        elevationLevel: ElevationLevel.NonAdmin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 45678,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
};

// Commit the pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a>Batch Java örnek

```java
List<UserAccount> userList = new ArrayList<>();
userList.add(new UserAccount().withName(adminUserAccountName).withPassword(adminPassword).withElevationLevel(ElevationLevel.ADMIN));
userList.add(new UserAccount().withName(nonAdminUserAccountName).withPassword(nonAdminPassword).withElevationLevel(ElevationLevel.NONADMIN));
PoolAddParameter addParameter = new PoolAddParameter()
        .withId(poolId)
        .withTargetDedicatedNodes(POOL_VM_COUNT)
        .withVmSize(POOL_VM_SIZE)
        .withCloudServiceConfiguration(configuration)
        .withUserAccounts(userList);
batchClient.poolOperations().createPool(addParameter);
```

#### <a name="batch-python-example"></a>Batch Python örnek

```python
users = [
    batchmodels.UserAccount(
        name='pool-admin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.admin)
    batchmodels.UserAccount(
        name='pool-nonadmin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.nonadmin)
]
pool = batchmodels.PoolAddParameter(
    id=pool_id,
    user_accounts=users,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        node_agent_sku_id=sku_to_use),
    vm_size=vm_size,
    target_dedicated=vm_count)
batch_client.pool.add(pool)
```

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Yükseltilmiş erişim ile bir görevin adlı bir kullanıcı hesabı altında Çalıştır

Yükseltilmiş bir kullanıcı olarak bir görevi çalıştırmak için görev ayarlamak **Userıdentity** özelliği ile oluşturulan adlandırılmış kullanıcı hesabına onun **ElevationLevel** özelliğini `Admin`.

Bu kod parçacığı, görev bir adlandırılmış kullanıcı hesabı altında çalışması gerektiğini belirtir. Havuz oluşturulduğunda bu adlı bir kullanıcı hesabı havuzda tanımlandı. Bu durumda, adlandırılmış kullanıcı hesabının yönetici izinlerine sahip oluşturulmuştur:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a>En son Batch istemci kitaplığını için kodunuzu güncelleştirin

Bir değişiklik, Batch hizmeti sürüm 2017-01-01.4.0 tanıtır değiştirerek **runElevated** ile önceki sürümlerde kullanılabilir özellik **Userıdentity** özelliği. Aşağıdaki tablolarda, kodunuzu önceki sürümlerinden istemci kitaplıkları güncelleştirmek için kullanabileceğiniz basit bir eşleme sağlar.

### <a name="batch-net"></a>Batch .NET

| Kodunuzu kullanıyorsa...                  | Kendisine güncelleştir...                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated` Belirtilmemiş. | Güncelleştirme gerekli                                                                                               |

### <a name="batch-java"></a>Batch Java

| Kodunuzu kullanıyorsa...                      | Kendisine güncelleştir...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated` Belirtilmemiş. | Güncelleştirme gerekli                                                                                                                     |

### <a name="batch-python"></a>Batch Python

| Kodunuzu kullanıyorsa...                      | Kendisine güncelleştir...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`, burada <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin))`                |
| `run_elevated=False`                      | `user_identity=user`, burada <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin))`             |
| `run_elevated` Belirtilmemiş. | Güncelleştirme gerekli                                                                                                                                  |


## <a name="next-steps"></a>Sonraki adımlar

* Batch ayrıntılı genel bakış için bkz: [büyük ölçekli paralel işlem çözümleri geliştirme batch'le](batch-api-basics.md).
