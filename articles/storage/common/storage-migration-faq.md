---
title: Azure depolama geçişi ile ilgili SSS | Microsoft Docs
description: Geçirme Azure depolama hakkında sık sorulan sorulara yanıtlar
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.subservice: common
ms.openlocfilehash: cf1cba6f6d26d66fc560c86ea42459fa276cc880
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66114915"
---
# <a name="frequently-asked-questions-about-azure-storage-migration"></a>Azure depolama geçişi hakkında sık sorulan sorular

Bu makalede, Azure depolama geçişi hakkında sık sorulan sorular yanıtlanmaktadır. 

## <a name="faq"></a>SSS

**Dosyaları bir kapsayıcıdan kopyalamak için bir komut dosyası nasıl oluşturulur?**

Kapsayıcıları arasında dosya kopyalamak için AzCopy kullanabilirsiniz. Aşağıdaki örneğe bakın:

    AzCopy /Source:https://xxx.blob.core.windows.net/xxx
    /Dest:https://xxx.blob.core.windows.net/xxx /SourceKey:xxx /DestKey:xxx
    /S

AzCopy kullanan [kopyalama Blob API'sine](https://docs.microsoft.com/rest/api/storageservices/copy-blob) kapsayıcıda her dosya kopyalamak için.  
  
Herhangi bir sanal makine veya AzCopy çalıştırmak için internet erişimi olan yerel makine kullanabilirsiniz. Bu otomatik olarak yapması için bir Azure Batch zamanlama kullanabilirsiniz, ancak daha karmaşıktır.  
  
Otomasyon betiği, içerik işleme depolama yerine Azure Resource Manager dağıtımı için tasarlanmıştır. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md).

**Aynı bölgede aynı depolama hesabında iki dosya paylaşımlarında arasında veri kopyalamak için herhangi bir ücreti uygulanır mı?**

Hayır. Bu işlem için ücret alınmaz.

**Nasıl yapılır? başka bir depolama hesabı için tüm depolama Hesabımı yedekleyin**

Tüm depolama hesabınızı doğrudan yedekleme seçeneği yoktur. Ancak, el ile kapsayıcı bu depolama hesabında başka bir hesaba AzCopy veya Depolama Gezgini'ni kullanarak taşıyabilirsiniz. Aşağıdaki adımlar, AzCopy kapsayıcı taşımak için nasıl kullanılacağını gösterir:  
 

1.  Yükleme [AzCopy](storage-use-azcopy.md) komut satırı aracı. Bu araç, VHD dosyasını depolama hesapları arasında taşımanıza yardımcı olur.

2.  Yükleyicisi'ni kullanarak Windows üzerinde AzCopy yükledikten sonra bir komut istemi penceresi açın ve ardından bilgisayarınızda AzCopy yükleme klasörüne göz atın. AzCopy varsayılan olarak, yüklü **% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy** veya **%ProgramFiles%\Microsoft SDKs\Azure\AzCopy**.

3.  Kapsayıcı taşımak için aşağıdaki komutu çalıştırın. Metni gerçek değerlerle değiştirmeniz gerekir.   
     
            AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
            /Dest:https://destaccount.blob.core.windows.net/mycontainer2
            /SourceKey:key1 /DestKey:key2 /S

    - `/Source`: Kaynak depolama hesabı (kapsayıcı kadar) bir URI sağlayın.  
    - `/Dest`: Hedef depolama hesabı (kapsayıcı kadar) bir URI sağlayın.  
    - `/SourceKey`: Kaynak depolama hesabı için birincil anahtarı sağlayın. Depolama hesabı seçerek, bu anahtarı Azure portalından kopyalayabilirsiniz.  
    - `/DestKey`: Hedef depolama hesabı için birincil anahtarı sağlayın. Depolama hesabı seçerek, bu anahtarı portalından kopyalayabilirsiniz.

Bu komutu çalıştırdıktan sonra kapsayıcı dosyaları hedef depolama hesabına taşınır.

> [!NOTE]
> AzCopy CLI ile birlikte çalışmaz **deseni** bir Azure'dan kopyaladığınızda geçiş başka bir bloba.
>
> Doğrudan kopyalayın ve AzCopy komutunu düzenleyebilir ve emin olmak için karşılıklı denetim **deseni** kaynak eşleşir. Da emin olmanız **/S** joker karakterler geçerli olduğunu. Daha fazla bilgi için [AzCopy parametreleri](storage-use-azcopy.md).

**Nasıl veri depolama kapsayıcısından diğerine taşıyabilirim?**

Şu adımları uygulayın:

1.  Hedef blob kapsayıcısını (klasör) oluşturun.

2.  Kullanım [AzCopy](https://azure.microsoft.com/blog/azcopy-5-1-release/) farklı bir blob kapsayıcısına özgün blob kapsayıcısından içeriklerini kopyalamak için.

**Azure depolama alanındaki başka bir Azure dosya paylaşımından verileri taşımak için bir PowerShell Betiği nasıl oluşturulur?**

AzCopy, verileri Azure depolama alanındaki başka bir Azure dosya paylaşımından taşımak için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).

**Azure Depolama'ya büyük .csv dosyalarını nasıl yükleyebilirim?**

AzCopy, Azure Depolama'ya büyük .csv dosyalarını yüklemek için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).

**Günlükleri D sürücüsünden Azure storage hesabım için her gün taşımak zorunda. Bu nasıl otomatikleştirebilirim?**

Azcopy'yi kullanma ve Görev Zamanlayıcısı'nda bir görev oluşturun. Bir AzCopy toplu komut dosyası kullanarak bir Azure depolama hesabına dosya yükleme. Daha fazla bilgi için [nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri çalıştırma](../../cloud-services/cloud-services-startup-tasks.md).

**Abonelikler arasında depolama Hesabımı nasıl taşırım?**

AzCopy, depolama hesabınızın abonelikler arasında taşıma için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).

**Nasıl taşırım 10 TB veri için başka bir bölgede depolama?**

AzCopy, verileri taşımak için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).

**Nasıl verileri şirket içinden Azure Depolama'ya kopyalayabilirim?**

AzCopy, verileri kopyalamak için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).

**Nasıl verileri şirket içinden Azure dosyaları'na taşıyabilirim?**

AzCopy, verileri taşımak için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).

**Bir sanal makinede bir kapsayıcı klasörüne nasıl eşleştiği?**

Bir Azure Dosya Paylaşımı'nı kullanın.

**Nasıl yapılır? geri Azure dosya depolaması ayarlama**

Yedekleme çözümü yoktur. Ancak, Azure dosyaları zaman uyumsuz kopya da destekler. Bu nedenle, dosyalarını kopyalayabilirsiniz:

- Bir paylaşımını bir depolama hesabı içinde başka bir paylaşımı veya farklı bir depolama hesabı.

- Bir paylaşımını bir depolama hesabındaki bir blob kapsayıcısı veya farklı bir depolama hesabı.

Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md).

**Yönetilen diskler için başka bir depolama hesabına nasıl taşırım?**

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Şu adımları uygulayın:

1.  Yönetilen disk bağlı olduğu sanal makineyi durdurun.

2.  Yönetilen disk VHD aşağıdaki Azure PowerShell betiğini çalıştırarak bir çalışma alanından diğerine kopyalayın:

    ```
    Connect-AzAccount

    Select-AzSubscription -SubscriptionId <ID>

    $sas = Grant-AzDiskAccess -ResourceGroupName <RG name> -DiskName <Disk name> -DurationInSecond 3600 -Access Read

    $destContext = New-AzStorageContext –StorageAccountName contosostorageav1 -StorageAccountKey <your account key>

    Start-AzStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer 'vhds' -DestContext $destContext -DestBlob 'MyDestinationBlobName.vhd'
    ```

3.  VHD kopyaladığınız başka bir bölgede VHD dosyasını kullanarak yönetilen disk oluşturun. Bunu yapmak için aşağıdaki Azure PowerShell betiğini çalıştırın:  

    ```
    $resourceGroupName = 'MDDemo'

    $diskName = 'contoso\_os\_disk1'

    $vhdUri = 'https://contoso.storageaccou.com.vhd

    $storageId = '/subscriptions/<ID>/resourceGroups/<RG name>/providers/Microsoft.Storage/storageAccounts/contosostorageaccount1'

    $location = 'westus'

    $storageType = 'StandardLRS'

    $diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $vhdUri -StorageAccountId $storageId -DiskSizeGB 128

    $osDisk = New-AzDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
    ``` 

Bir yönetilen diskten bir sanal makine dağıtma hakkında daha fazla bilgi için bkz. [CreateVmFromManagedOsDisk.ps1](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/blob/master/CreateVmFromManagedOsDisk.ps1).

**1-2 TB veri Azure portalından nasıl indirebilirim?**

AzCopy, verileri indirmek için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).

**Avrupa bölgesinde bir depolama hesabı için ikincil konuma nasıl değiştirebilirim?**

Bir depolama hesabı oluşturduğunuzda, hesap için birincil bölge seçin. İkincil bölgeye seçimini birincil bölgeye göre hesaplanır ve değiştirilemez. Daha fazla bilgi için [coğrafi olarak yedekli depolama (GRS): Azure depolama için bölgeler arası çoğaltma](storage-redundancy.md).

**Azure depolama hizmeti şifrelemesi (SSE) hakkında daha fazla bilgiyi nereden bulabilirim?**  
  
Aşağıdaki makalelere bakın:

-  [Azure Depolama güvenlik kılavuzu](storage-security-guide.md)

-  [Bekleyen veri için Azure depolama hizmeti şifrelemesi](storage-service-encryption.md)

**Nasıl taşıma veya bir depolama hesabından veri indirme?**

AzCopy, verileri indirmek için kullanın. Daha fazla bilgi için [Windows üzerinde AzCopy ile veri aktarımı](storage-use-azcopy.md) ve [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md).


**Bir depolama hesabındaki verileri nasıl şifreliyor mu?**

Bir depolama hesabında şifreleme etkinleştirdikten sonra mevcut veri şifrelenmez. Mevcut verileri şifrelemek için onu tekrar depolama hesabına yüklemeniz gerekir.

AzCopy, verileri farklı bir depolama hesabı için kopyalayın ve ardından verileri geri taşımak için kullanın. Ayrıca [bekleyen şifreleme](storage-service-encryption.md).

**Bir VHD için bir yerel makine dışındaki portalda yükleme seçeneğini kullanarak nasıl indirebilirim?**

Kullanabileceğiniz [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) VHD indirilemedi.

**Bir depolama hesabı çoğaltma yerel olarak yedekli depolama için coğrafi olarak yedekli depolama alanından değiştirmek için herhangi bir önkoşul var mı?**

Hayır. 

**Azure dosyaları olarak yedekli depolama nasıl erişim sağlanır?**

Okuma erişimli coğrafi olarak yedekli depolama, yedekli depolamaya erişmek için gereklidir. Ancak, Azure dosyaları, yalnızca yerel olarak yedekli depolama ve salt okunur erişim izni vermiyor standart coğrafi olarak yedekli depolama destekler. 

**Bir standart depolama hesabı için bir premium depolama hesabından nasıl taşırım?**

Şu adımları uygulayın:

1.  Bir standart depolama hesabı oluşturun. (Veya aboneliğinizde var olan bir standart depolama hesabı kullanın.)

2.  AzCopy indirin. Aşağıdaki AzCopy komutlardan birini çalıştırın.
      
    Tüm diskleri depolama hesabına kopyalamak için:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /S 

    Yalnızca bir disk kopyalamak için diskin adını sağlayın. **deseni**:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /Pattern:abc.vhd

   
İşlemin tamamlanması birkaç saat sürebilir.

Aktarım başarıyla tamamlandığını emin olmak için hedef depolama hesabı kapsayıcı Azure portalında inceleyin. Diskleri için standart depolama hesabı kopyalandıktan sonra bunları var olan bir diski sanal makineye ekleyebilirsiniz. Daha fazla bilgi için [Azure portalında Windows sanal makine için yönetilen bir veri diski nasıl](../../virtual-machines/windows/attach-managed-disk-portal.md).  
  
**Dosya Paylaşımı için Azure Premium Depolama'ya nasıl dönüştürebilirim?**

Premium depolama, bir Azure dosya paylaşımında kullanılamaz.

**Bir standart depolama hesabından premium depolama hesabına nasıl yükseltebilirim? Bir standart depolama hesabı için nasıl bir premium depolama hesabından düşürme?**

Hedef depolama hesabı oluşturma, veri kaynağı hesaptan hedef hesabına kopyalayın ve ardından kaynak hesabı silmeniz gerekir. Verileri kopyalamak için AzCopy gibi bir araç kullanabilirsiniz.

Sanal makineleriniz varsa, depolama hesabı verileri geçirmeden önce ek adımları atmanız gerekir. Daha fazla bilgi için [(yönetilmeyen diskler) Azure Premium Depolama'ya geçiş](storage-migration-to-premium-storage.md).

**Klasik depolama hesabından bir Azure Resource Manager depolama hesabına nasıl taşırım?**

Kullanabileceğiniz **taşıma AzStorageAccount** cmdlet'i. Bu cmdlet birden fazla adım vardır (doğrulayın, hazırlama, yürütme). Bunu yapmadan önce taşıma doğrulayabilirsiniz.

Sanal makineleriniz varsa, depolama hesabı verileri geçirmeden önce ek adımları atmanız gerekir. Daha fazla bilgi için [Azure PowerShell kullanarak geçirme Iaas kaynaklarını Klasik modelden Azure Resource Manager'a](../..//virtual-machines/windows/migration-classic-resource-manager-ps.md).

**Bir Azure depolama hesabından Linux tabanlı bir bilgisayara indirebilir, veya nasıl bir Linux makine verilerini karşıya yükleme?**

Azure CLI'yı kullanabilirsiniz.

- Tek bir blob indirme:

      azure storage blob download -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -b "<Remote File Name>" -d "<Local path where the file will be downloaded to>"

- Tek bir blobu karşıya yükle: 

      azure storage blob upload -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -f "<Local File Name>"

**Nasıl miyim diğer kişilerin erişim depolama Kaynaklarım verebilir misiniz?**

Diğer kişilerin depolama kaynaklarına erişim vermek için:

-   Bir kaynağa erişim sağlamak için paylaşılan erişim imzası (SAS) belirteci kullanın. 

-   Depolama hesabı için birincil veya ikincil anahtara sahip bir kullanıcı sağlayın. Daha fazla bilgi için [depolama hesabınızı yönetme](storage-account-manage.md#access-keys).

-   Erişim ilkesi, anonim erişime izin verecek şekilde değiştirin. Daha fazla bilgi için [kapsayıcılara ve blob'lara anonim kullanıcılara izin vermek](../blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

**AzCopy yüklendiği?**

-   Microsoft Azure depolama komut satırından AzCopy erişirseniz, yazın **AzCopy**. Komut satırı AzCopy ile birlikte yüklenir.

-   32 bit sürümünü yüklediyseniz bu buradaki: **% ProgramFiles(x86) %\\Microsoft SDKs\\Azure\\AzCopy**.

-   64-bit sürümünü yüklediyseniz bu buradaki: **% ProgramFiles %\\Microsoft SDKs\\Azure\\AzCopy**.

**(Örneğin, bölgesel olarak yedekli depolama, coğrafi olarak yedekli depolama veya okuma erişimli coğrafi olarak yedekli depolama) bir çoğaltılmış bir depolama hesabı için nasıl ikincil bölge'de depolanan verilere erişim sağlanır?**

-   Bölgesel olarak yedekli depolama veya coğrafi olarak yedekli depolama kullanıyorsanız, bu bölgeye bir yük devretme başlatın sürece verileri ikincil bölgeden erişemez. Yük devretme işlemi hakkında daha fazla bilgi için bkz. [olağanüstü durum kurtarma ve depolama hesabı yük devretme (Önizleme) Azure Depolama'daki](storage-disaster-recovery-guidance.md).

-   Okuma erişimli coğrafi olarak yedekli depolama kullanıyorsanız, verileri ikincil bölgeden herhangi bir zamanda erişebilirsiniz. Aşağıdaki yöntemlerden birini kullanın:  
      
    - **AzCopy**: Append **-ikincil** ikincil uç noktasına erişmek için URL'de depolama hesabı adı. Örneğin:  
     
      https://storageaccountname-secondary.blob.core.windows.net/vhds/BlobName.vhd

    - **SAS belirteci**: Bir SAS belirteci uç noktasından verilere erişmek için kullanın. Daha fazla bilgi için [paylaşılan erişim imzaları kullanma](storage-dotnet-shared-access-signature-part-1.md).

**HTTPS özel etki alanı ile depolama Hesabımı nasıl kullanabilirim? Örneğin, ne yaptığım "https:\//mystorageaccountname.blob.core.windows.net/images/image.gif" olarak görünür "https:\//www.contoso.com/images/image.gif"?**

SSL, özel etki alanları ile depolama hesapları şu anda desteklenmiyor.
Ancak HTTPS olmayan özel etki alanlarını kullanabilirsiniz. Daha fazla bilgi için [Blob Depolama uç noktanız için özel etki alanı yapılandırma](../blobs/storage-custom-domain-name.md).

**FTP için bir depolama hesabına erişim verileri nasıl kullanırım?**

Bir depolama hesabı, FTP kullanarak doğrudan erişmenin hiçbir yolu yoktur. Ancak, bir Azure sanal Makine'yi ayarlayın ve ardından sanal makine üzerinde bir FTP sunucusuna yükleyin. Azure dosyaları paylaşımına ya da sanal makine için kullanılabilir olan veri diski depolamak FTP sunucusu olabilir.

Depolama Gezgini veya benzer bir uygulama kullanmaya gerek kalmadan verileri yalnızca indirmek istiyorsanız, bir SAS belirteci kullanmanız mümkün olabilir. Daha fazla bilgi için [paylaşılan erişim imzaları kullanma](storage-dotnet-shared-access-signature-part-1.md).

**Nasıl Bloblar bir depolama hesabından diğerine geçirebilirim?**

 Bunu kullanarak yapabilirsiniz bizim [Blob geçiş öncesinde bir betik](../scripts/storage-common-transfer-between-storage-accounts.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
