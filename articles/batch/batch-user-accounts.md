---
title: Kullanıcı hesapları görevleri Azure Batch'de çalıştırma | Microsoft Docs
description: Azure toplu işlemde çalışan görevler için kullanıcı hesaplarını yapılandırma
services: batch
author: dlepow
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
ms.author: danlep
ms.openlocfilehash: 1b9c0514e93fa89f8776d830ef242fc4963a6f7b
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="run-tasks-under-user-accounts-in-batch"></a>Toplu işlemde kullanıcı hesapları görevleri Çalıştır

Azure Batch görevinde her zaman bir kullanıcı hesabı altında çalışır. Varsayılan olarak, standart kullanıcı hesapları, yönetici izinleri olmayan altında görevlerini çalıştırın. Bu varsayılan kullanıcı hesabı ayarları genellikle yeterlidir. Bazı senaryolarda, ancak altında çalıştırmak için bir görev istediğiniz kullanıcı hesabını yapılandırmak yararlı olacaktır. Bu makalede, kullanıcı hesapları ve nasıl bunları senaryonuz için yapılandırabilirsiniz türleri açıklanmaktadır.

## <a name="types-of-user-accounts"></a>Kullanıcı hesabı türleri

Azure toplu işlem, görevleri çalıştırmak için iki tür kullanıcı hesabı sağlar:

- **Otomatik kullanıcı hesapları.** Otomatik kullanıcı hesapları Batch hizmeti tarafından otomatik olarak oluşturulan yerleşik kullanıcı hesaplarıdır. Varsayılan olarak, görevler bir otomatik kullanıcı hesabı altında çalışır. Bir görev hangi otomatik bir kullanıcı hesabı altında çalışması gerektiğini belirtmek bir görev için otomatik kullanıcı belirtimi yapılandırabilirsiniz. Otomatik kullanıcı belirtimi ayrıcalık düzeyinde ve kapsam görevin çalışacağını otomatik kullanıcı hesabının belirtmenize olanak tanır. 

- **Adlı bir kullanıcı hesabı.** Havuz oluşturduğunuzda havuz için bir veya daha fazla adlandırılmış kullanıcı hesapları belirtebilirsiniz. Her kullanıcı hesabı havuzunun her bir düğümde oluşturulur. Hesap adı yanı sıra, kullanıcı hesabının parolasını ayrıcalık düzeyi ve, Linux havuzları, SSH özel anahtar için belirtin. Bir görev eklediğinizde, bu görev, altında çalışması gerektiğini adlı bir kullanıcı hesabı belirtebilirsiniz.

> [!IMPORTANT] 
> Batch hizmeti sürümü 2017 01 01.4.0 sürümünün çağırmak için kodunuzu güncelleştirin gerektiren önemli bir değişiklik tanıtır. Geçirme koddan toplu daha eski bir sürümü olup olmadığını unutmayın **runElevated** özelliği REST API veya toplu istemci kitaplıkları artık desteklenmiyor. Yeni **Userıdentity** ayrıcalık düzeyini belirtmek için bir görev özelliği. Başlıklı bölüme bakın [en son toplu istemci Kitaplığı'na kodunuzu güncelleştirin](#update-your-code-to-the-latest-batch-client-library) istemci kitaplıklarından birini kullanıyorsanız, toplu kodunuzu güncelleştirmek için hızlı yönergeler için.
>
>

> [!NOTE] 
> Bu makalede ele alınan kullanıcı hesapları Uzak Masaüstü Protokolü (RDP) veya güvenli Kabuk (SSH), güvenlik nedenleriyle desteklemez. 
>
> Linux sanal makine yapılandırması SSH yoluyla çalıştıran bir düğüme bağlanmak için bkz: [azure'da bir Linux VM için Uzak Masaüstü kullanım](../virtual-machines/virtual-machines-linux-use-remote-desktop.md). RDP aracılığıyla Windows çalışan düğümlerine bağlanmak için bkz: [bir Windows Server VM Bağlan](../virtual-machines/windows/connect-logon.md).<br /><br />
> RDP aracılığıyla bulut hizmeti yapılandırması çalıştıran bir düğüme bağlanmak için bkz: [bir rolde Azure bulut Hizmetleri için Uzak Masaüstü Bağlantısı etkinleştirmek](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md).
>
>

## <a name="user-account-access-to-files-and-directories"></a>Dosyalar ve dizinler için kullanıcı hesabı erişimi

Bir otomatik kullanıcı hesabı ve adlı bir kullanıcı hesabı görevin çalışma dizini, paylaşılan dizine ve çok örnekli görevler dizin okuma/yazma erişimi. Her iki hesap türü başlangıç ve iş hazırlama dizinlerinin okuma erişimi.

Bir görev, bir başlangıç görevi çalıştırmak için kullanılan aynı hesap altında çalışıyorsa, görev Başlat görev dizininde okuma-yazma erişimi vardır. Benzer şekilde, bir görev iş hazırlama görevi çalıştırmak için kullanılan aynı hesap altında çalışıyorsa, görev iş hazırlama görevi dizini okuma-yazma erişimi vardır. Bir görev başlangıç görevi veya iş hazırlama görevi farklı bir hesap altında çalışıyorsa, görev yalnızca ilgili dizine okuma erişimi vardır.

Dosyalar ve dizinler görevden erişme hakkında daha fazla bilgi için bkz: [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md#files-and-directories).

## <a name="elevated-access-for-tasks"></a>Görevler için yükseltilmiş erişim 

Kullanıcı hesabının ayrıcalık düzeyi, bir görevin yükseltilmiş erişim ile çalışıp çalışmayacağını gösterir. Bir otomatik kullanıcı hesabı ve adlı bir kullanıcı hesabı ile yükseltilmiş erişim çalıştırabilirsiniz. Ayrıcalık düzeyi için iki seçenek şunlardır:

- **NonAdmin:** yükseltilmiş erişimi olmayan standart bir kullanıcı olarak görev çalıştırır. Toplu kullanıcı hesabı için varsayılan ayrıcalık düzeyi her zaman olduğu **NonAdmin**.
- **Yönetici:** görevi yükseltilmiş erişimi bir kullanıcı olarak çalışır ve tam yönetici izinlerine sahip çalışır. 

## <a name="auto-user-accounts"></a>Otomatik kullanıcı hesapları

Varsayılan olarak, görevler yükseltilmiş erişimi olmadan ve görev kapsamlı standart bir kullanıcı olarak bir otomatik kullanıcı hesabı altında toplu işlemde çalıştırın. Otomatik kullanıcı belirtimi görev kapsam için yapılandırıldığında, Batch hizmeti yalnızca bu görev için bir otomatik kullanıcı hesabı oluşturur.

Görev kapsamına havuzu kapsam alternatiftir. Bir görev için otomatik kullanıcı belirtimi havuzu kapsam için yapılandırıldığında, görev havuzdaki herhangi bir görev için kullanılabilir olan bir otomatik kullanıcı hesabı altında çalışır. Havuz kapsamı hakkında daha fazla bilgi için başlıklı bölüme bakın [havuzu kapsamlı otomatik kullanıcı olarak bir görevi çalıştırmayı](#run-a-task-as-the-autouser-with-pool-scope).   

Varsayılan kapsamı, Windows ve Linux düğümlerinde farklıdır:

- Windows düğümlerinde görevler varsayılan olarak görev kapsam altında çalışır.
- Linux düğümleri havuzu kapsam altında her zaman çalışır.

Her biri benzersiz otomatik kullanıcı hesabına karşılık gelen otomatik kullanıcı belirtimi için dört olası yapılandırmaları vardır:

- Yönetici olmayan erişim görev kapsamlı (varsayılan otomatik kullanıcı belirtimi)
- Görev kapsamlı yönetici (yükseltilmiş) erişimi
- Havuz kapsamlı yönetici olmayan erişim
- Havuz kapsamlı yönetici erişimi

> [!IMPORTANT] 
> Görev kapsam altında çalışan görevler gerçek diğer görevler için bir düğümde erişimi. Ancak, kötü niyetli bir kullanıcı hesabı erişimi olan yönetici ayrıcalıklarıyla çalıştırır ve diğer görev dizinleri erişen bir görev göndererek bu kısıtlamaya işe yarayabilir. Kötü niyetli bir kullanıcı, bir düğüme bağlanmak için RDP veya SSH da kullanabilirsiniz. Böyle bir senaryo önlemek için Batch hesabı anahtarları erişimi korumak önemlidir. Hesabınızı tehlikeye şüpheleniyorsanız, anahtarlarınızı yeniden emin olun.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Yükseltilmiş erişimi olan bir otomatik kullanıcı olarak bir görevi çalıştırma

Yükseltilmiş erişimi bir görev çalıştırmanız gerektiğinde yönetici ayrıcalıkları otomatik kullanıcı belirtimi yapılandırabilirsiniz. Örneğin, bir başlangıç görevi yazılım düğümüne yüklemek için yükseltilmiş erişmeniz gerekebilir.

> [!NOTE] 
> Genel olarak, yalnızca gerekli olduğunda yükseltilmiş erişimi kullanmak en iyisidir. En iyi yöntemler, istenen sonucu elde etmek gerekli en düşük ayrıcalık verilmesi önerilir. Örneğin, bir başlangıç görevi geçerli kullanıcıya yönelik yazılımları yerine tüm kullanıcılar için yüklerse görevlere yükseltilmiş erişimi vermekten kaçının mümkün olabilir. Başlangıç görevi de dahil olmak üzere aynı hesabı altında çalıştırmak için gereken tüm görevler için havuz kapsamı ve yönetici olmayan erişim için otomatik kullanıcı belirtimi yapılandırabilirsiniz. 
>
>

Aşağıdaki kod parçacıkları, otomatik kullanıcı belirtimine yapılandırılacağı gösterilmektedir. Örnekler ayrıcalık düzeyini set `Admin` ve kapsamını `Task`. Görev kapsamı varsayılan ayar, ancak örnek açısından buraya dahil edilir.

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

Bir düğüm sağlandığında, iki havuzu genelinde otomatik-kullanıcı hesapları havuzu, yükseltilmiş erişim biriyle ve yükseltilmiş erişimi olmadan bir içindeki her bir düğümde oluşturulur. Belirli bir görev için havuz kapsamına otomatik kullanıcının kapsamı ayarlama, bu iki havuzu genelinde otomatik-kullanıcı hesaplarından birini altında görev çalıştırır. 

Ne zaman otomatik-kullanıcı, aynı havuz genelinde otomatik-kullanıcı hesabı altında çalışacak yönetici erişimi olan çalışan tüm görevler için havuz kapsam belirtin. Benzer şekilde, yönetici izinleri olmadan çalışan görevler de tek havuzu genelinde otomatik-kullanıcı hesabı altında çalışır. 

> [!NOTE] 
> İki havuzu genelinde otomatik-kullanıcı hesapları ayrı hesaplarıdır. Havuzu genelinde yönetici hesabı altında çalışan görevler standart hesap altında ve tersi yönde çalışan görevler ile verileri paylaşamaz. 
>
>

Aynı otomatik kullanıcı hesabı altında çalışan yararınıza görevleri aynı düğüm üzerinde çalışan diğer görevleri ile verileri paylaşamaz olmasıdır.

Çalışan görevlerin iki havuzu genelinde otomatik-kullanıcı hesaplarından birini altında yararlı olduğu bir senaryo gizli görevler arasında paylaşımıdır. Örneğin, diğer görevleri kullanabilirsiniz düğüm üzerine bir gizlilik sağlamak bir başlangıç görevi gerektiğini varsayalım. Windows Data Protection API (DPAPI) kullanabilirsiniz, ancak yönetici ayrıcalıkları gerekiyor. Bunun yerine, gizli kullanıcı düzeyinde koruyabilir. Aynı kullanıcı hesabı altında çalışan görevler yükseltilmiş erişimi olmadan gizli erişebilir.

Başka bir senaryo havuzu kapsamlı bir otomatik kullanıcı hesabı altında görevleri çalıştırmak istediğiniz bir ileti geçirme arabirimi (MPI) dosya paylaşımıdır. Bir MPI dosya paylaşımı, MPI görev düğümler aynı dosya verileri üzerinde çalışması gereken yararlıdır. Baş düğüm aynı otomatik kullanıcı hesabı altında çalışıyorsa, alt düğümleri erişebileceği bir dosya paylaşımı oluşturur. 

Aşağıdaki kod parçacığını otomatik kullanıcının kapsam Batch .NET içinde bir görev için havuz kapsamı için ayarlar. Görev standart havuzu genelinde otomatik-kullanıcı hesabı altında çalışan şekilde ayrıcalık düzeyi atlandı.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>Adlı kullanıcı hesapları

Bir havuz oluşturduğunuzda, adlandırılmış kullanıcı hesapları tanımlayabilirsiniz. Adlı bir kullanıcı hesabı adı ve sağladığınız parola sahiptir. Adlı bir kullanıcı hesabı için ayrıcalık düzeyi belirtebilirsiniz. Linux düğümleri için bir SSH özel anahtarı da sağlayabilir.

Adlı bir kullanıcı hesabı havuzdaki tüm düğümlerde var ve bu düğümler üzerinde çalışan tüm görevler için kullanılabilir. Adlandırılmış kullanıcılar bir havuz için herhangi bir sayıda tanımlayabilir. Bir görevi veya görev koleksiyonu eklediğinizde, görevin havuzunda tanımlanan adlı kullanıcı hesaplarından birini altında çalıştığı belirtebilirsiniz.

Adlı bir kullanıcı hesabı, aynı kullanıcı hesabı altında bir işteki tüm görevler çalıştırabilir, ancak diğer işleri aynı anda çalışan görevleri birbirlerinden ayrı tutmak istediğiniz yararlıdır. Örneğin, her iş için adlandırılmış bir kullanıcı oluşturun ve her iş görevlerinin bu adlı bir kullanıcı hesabı altında çalıştırın. Her bir iş, daha sonra kendi görevleri ile ancak diğer işlerin görevleri bir gizlilik paylaşabilirsiniz.

Adlı bir kullanıcı hesabı, dosya paylaşımları gibi dış kaynaklar üzerinde izinlerini ayarlar bir görevi çalıştırmak için de kullanabilirsiniz. Adlı bir kullanıcı hesabı ile kullanıcı kimliğini denetlemek ve kullanıcı kimliği izinlerini ayarlamak için kullanabilirsiniz.  

Adlı kullanıcı hesapları parola daha az SSH Linux düğümleri arasında sağlar. Çok örnekli görevleri çalıştırmak için gereken Linux düğümleri adlı bir kullanıcı hesabı kullanabilirsiniz. Havuzdaki her düğüme görevleri tüm havuzu üzerinde tanımlı bir kullanıcı hesabı altında çalışabilir. Çok örnekli görevler hakkında daha fazla bilgi için bkz: [çoklu kullanmak\-örneği MPI uygulamaları çalıştırmak için görevleri](batch-mpi.md).

### <a name="create-named-user-accounts"></a>Adlı kullanıcı hesapları oluşturma

Toplu işlemde adlı kullanıcı hesaplarını oluşturmak için kullanıcı hesapları topluluğu havuzuna ekleyin. Aşağıdaki kod parçacıkları .NET, Java ve Python adlandırılmış kullanıcı hesaplarını oluşturmayı gösterir. Bu kod parçacıkları yönetici ve bir havuz hesaplarında adlı yönetici olmayan nasıl oluşturulacağını gösterir. Bulut Hizmeti Yapılandırması'nı kullanarak havuzları örnekleri oluşturma, ancak sanal makine Yapılandırması'nı kullanarak bir Windows veya Linux havuzu oluştururken aynı yaklaşımı kullanın.

#### <a name="batch-net-example-windows"></a>Batch .NET örnek (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                                         
    virtualMachineSize: "small",                                                
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


#### <a name="batch-java-example"></a>Batch Java örneği

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

#### <a name="batch-python-example"></a>Batch Python örneği

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

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Yükseltilmiş erişimle görevi adlı bir kullanıcı hesabı altında çalıştırma

Görevin yükseltilmiş bir kullanıcı olarak bir görevi çalıştırmayı ayarlayın **Userıdentity** ile oluşturulmuş bir adlı bir kullanıcı hesabı özelliğine kendi **ElevationLevel** özelliğini `Admin`.

Bu kod parçacığını görev adlı bir kullanıcı hesabı altında çalışması gerektiğini belirtir. Havuz oluşturduğunuzda bu adlı bir kullanıcı hesabı havuzunda tanımlandı. Bu durumda, adlı bir kullanıcı hesabı yönetici izinlerine sahip oluşturuldu:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a>En son toplu istemci Kitaplığı'na kodunuzu güncelleştirin

Batch hizmeti sürümü 2017 01 01.4.0 önemli bir değişiklik tanıtır değiştirme **runElevated** ile önceki sürümlerde kullanılabilir özellik **Userıdentity** özelliği. Aşağıdaki tablolar, kodunuzu istemci kitaplıkları önceki sürümlerinden güncelleştirmek için kullanabileceğiniz basit bir eşleme sağlar.

### <a name="batch-net"></a>Batch .NET

| Kodunuzu kullanıyorsa...                  | Buna güncelleştirme...                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated` Belirtilmemiş. | Güncelleştirme gerekmiyor                                                                                               |

### <a name="batch-java"></a>Batch Java

| Kodunuzu kullanıyorsa...                      | Buna güncelleştirme...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated` Belirtilmemiş. | Güncelleştirme gerekmiyor                                                                                                                     |

### <a name="batch-python"></a>Batch Python

| Kodunuzu kullanıyorsa...                      | Buna güncelleştirme...                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`, burada <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin)) `                |
| `run_elevated=False`                      | `user_identity=user`, burada <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.nonadmin)) `             |
| `run_elevated` Belirtilmemiş. | Güncelleştirme gerekmiyor                                                                                                                                  |


## <a name="next-steps"></a>Sonraki adımlar

### <a name="batch-forum"></a>Toplu işlem Forumu

[Azure toplu işlem Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch) MSDN'de toplu ele almaktadır ve hizmet hakkında sorular sormak için iyi bir yerdir. HEAD üzerinde üzerinden faydalı sabitlenmiş gönderiler için ve Batch çözümlerinizi derleme sırasında çıktıkları anda sorularınızı gönderin.