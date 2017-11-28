---
title: "Azure depolama alanı geçişi ile ilgili SSS | Microsoft Docs"
description: "Azure depolama geçirme hakkında genel soruların yanıtları için"
services: storage
documentationcenter: na
author: genlin
manager: timlt
editor: tysonn
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 11/16/2017
ms.author: genli
ms.openlocfilehash: 516a0487afe11ef6915a002375661a23eaf13edc
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="frequently-asked-questions-about-azure-storage-migration"></a>Azure depolama geçişi hakkında sık sorulan sorular

Bu makalede, Azure depolama geçişi hakkında sık sorulan sorular yanıtlanmaktadır. 

## <a name="faq"></a>SSS

**Dosyaları bir kapsayıcıdan kopyalamak için bir komut dosyası nasıl oluşturulur?**

Kapsayıcıları arasında dosya kopyalamak için AzCopy kullanabilirsiniz. Aşağıdaki örneğe bakın:

    AzCopy /Source:https://xxx.blob.core.windows.net/xxx
    /Dest:https://xxx.blob.core.windows.net/xxx /SourceKey:xxx /DestKey:xxx
    /S

AzCopy kullanan [kopyalama Blob API](https://docs.microsoft.com/rest/api/storageservices/copy-blob) kapsayıcıdaki her dosya için kopya yapmak için.  
  
Herhangi bir sanal makine veya AzCopy çalıştırmak için Internet erişimi olan yerel makine kullanabilirsiniz. Bunu otomatik olarak yapmak için Azure Batch zamanlama kullanabilirsiniz, ancak daha karmaşık.  
  
Otomasyon betiğini depolama içerik işleme yerine Azure Resource Manager dağıtımı için tasarlanmıştır. Daha fazla bilgi için bkz: [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](../../azure-resource-manager/resource-group-template-deploy.md).

**Herhangi bir ücret aynı bölgede aynı depolama hesabında iki farklı dosya paylaşımlarında arasında veri kopyalamak için söz konusu var mı?**

Hayır. Bu işlem için ücret ödemeden yoktur.

**Nasıl yedeklerim tüm depolama Hesabımı başka bir depolama hesabı için yedekleme?**

Tüm depolama hesabınızı doğrudan yedeklemek için bir seçenek yoktur. Ancak, el ile kapsayıcı bu depolama hesabı için başka bir hesap AzCopy veya Depolama Gezgini'ni kullanarak taşıyabilirsiniz. Aşağıdaki adımlar, AzCopy kapsayıcı taşımak için nasıl kullanılacağını gösterir:  
 

1.  Yükleme [AzCopy](storage-use-azcopy.md) komut satırı aracı. Bu araç VHD dosyasının depolama hesapları arasında taşımanıza yardımcı olur.

2.  AzCopy Windows Yükleyicisi'ni yükledikten sonra bir komut POromprt penceresi açın ve bilgisayarınızda AzCopy yükleme klasörüne gidin. AzCopy varsayılan olarak, yüklü **% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy** veya **%ProgramFiles%\Microsoft SDKs\Azure\AzCopy**.

3.   Kapsayıcı taşımak için aşağıdaki komutu çalıştırın. Metni gerçek değer ile değiştirmeniz gerekir.   
     
            AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
            /Dest:https://destaccount.blob.core.windows.net/mycontainer2
            /SourceKey:key1 /DestKey:key2 /S

        - / source: (en fazla kapsayıcı) kaynak depolama hesabı URI sağlayın  
        - / dest: (en fazla kapsayıcı) hedef depolama hesabı URI sağlayın  
        - /sourcekey: kaynak depolama sağlama hesabının birincil anahtarını kopyalayabilirsiniz bu anahtarı portaldan depolama hesabı seçerek.  
        - /destkey: hedef depolama sağlama hesabının birincil anahtarını kopyalayabilirsiniz bu anahtarı portaldan depolama hesabı seçerek.

Bu komut yürüttükten sonra kapsayıcı dosyalar için hedef depolama hesabı taşınır.

**Bir Azure kopyaladığınızda AzCopy CLI "Düzeni" anahtarı ile birlikte çalışmaz başka bir blob.**

Doğrudan kopyalayın ve AzCopy cmd düzenleyebilir ve desen kaynak eşleştiğinden emin olmak için Çapraz denetimi. Da emin olmanız **/S** joker karakterler geçerli olduğunu. Daha fazla bilgi için bkz: [AzCopy parametreleri](storage-use-azcopy.md).

**Nasıl veri bir depolama kapsayıcıdan diğerine taşıyabilirim?**

Bunu yapmak için şu adımları uygulayın:

1.  Hedef blob kapsayıcısı (klasör) oluşturun.

2.  Kullanım [Azcopy](https://azure.microsoft.com/en-us/blog/azcopy-5-1-release/) içeriği farklı bir Blob kapsayıcıya özgün blob kapsayıcısından kopyalamak için.

**Verileri bir Azure dosya paylaşımından başka bir Azure depolama birimine taşımak için bir PowerShell komut dosyası nasıl oluşturulur?**

Verileri bir Azure dosya paylaşımından başka bir azure depolama birimine taşımak için AzCopy kullanın. Daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).

**Azure depolama alanına nasıl büyük .csv dosyaları yükleme?**

AzCopy büyük .csv dosyaları Azure depolama alanına yüklemek için kullanın. Daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).

**Günlükleri "sürücüden, D" Azure depolama Hesabımı her gün taşımaya zorunda. Bunu nasıl otomatikleştirmek?**

AzCopy kullanın ve Görev Zamanlayıcısı'nda bir görev oluşturun. Dosyaları, AzCopy toplu komut dosyası kullanarak bir Azure depolama hesabı karşıya yükleyin. Daha fazla bilgi için bkz: [nasıl yapılandırılacağı ve bir bulut hizmeti için başlangıç görevleri çalıştırmak](../../cloud-services/cloud-services-startup-tasks.md).

**Depolama Hesabımı abonelikler arasında nasıl taşıyabilirim?**

Depolama hesabınız abonelikler arasında taşımak için AzCopy kullanın. Daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).

**Nasıl ı taşıyabilirsiniz verileri başka bir bölge adresindeki depolama 10 TB?**

AzCopy verileri taşımak için kullanın. Daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).

**Nasıl ı veri şirket içinden Azure depolama alanına kopyalayabilir miyim?**

Verileri kopyalamak için AzCopy kullanın. Daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).

**Nasıl ı veriler şirket içinden Azure dosya hizmeti taşıyabilir miyim?**

AzCopy veri taşımak için kullanın. Daha fazla bilgi için daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).

**Bir sanal makinede bir kapsayıcı klasörü nasıl eşleme?**

Azure dosya paylaşımı kullanın.

**Nasıl yedeklerim geri Azure dosya depolama alanı?**

Yedekleme çözümü yoktur. Ancak Azure dosyaları zaman uyumsuz kopya destekler. Dosyaları bir paylaşımından başka bir paylaşımına (bir depolama hesabındaki veya farklı bir depolama hesabı), kopyalayabilirsiniz veya bir blob kapsayıcısını (bir depolama hesabındaki veya farklı bir depolama hesabı için).
Daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md).

**Başka bir depolama hesabına nasıl yönetilen diskleri taşıma?**

Bunu yapmak için şu adımları uygulayın:

1.  Yönetilen diski bağlı sanal makineyi durdurun.

2.  Yönetilen disk VHD aşağıdaki Azure PowerShell betiğini çalıştırarak bir alanından diğerine kopyalayın:

    ```
    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId <ID>

    $sas = Grant-AzureRmDiskAccess -ResourceGroupName <RG name> -DiskName <Disk name> -DurationInSecond 3600 -Access Read

    $destContext = New-AzureStorageContext –StorageAccountName contosostorageav1 -StorageAccountKey <your account key>

    Start-AzureStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer 'vhds' -DestContext $destContext -DestBlob 'MyDestinationBlobName.vhd'
    ```

3.  Yönetilen bir disk, VHD kopyaladığınız başka bir bölgede VHD dosyasını kullanarak oluşturun. Bunu yapmak için aşağıdaki Azure PowerShell betiğini çalıştırın:  

    ```
    $resourceGroupName = 'MDDemo'

    $diskName = 'contoso\_os\_disk1'

    $vhdUri = 'https://contoso.storageaccou.com.vhd

    $storageId = '/subscriptions/<ID>/resourceGroups/<RG name>/providers/Microsoft.Storage/storageAccounts/contosostorageaccount1'

    $location = 'westus'

    $storageType = 'StandardLRS'

    $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $vhdUri -StorageAccountId $storageId -DiskSizeGB 128

    $osDisk = New-AzureRmDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
    ``` 

Yönetilen bir diskten bir sanal makine dağıtma hakkında daha fazla bilgi için bkz: [CreateVmFromManagedOsDisk.ps1](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/blob/master/CreateVmFromManagedOsDisk.ps1).

**Nasıl ı indirebilirsiniz hakkında 1-2 TB veri Azure portalından?**

AzCopy veri yüklemek için kullanın. Daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).

**Bir depolama hesabı için Avrupa bölgeye ikincil konum nasıl değiştirebilirim?**

Bir depolama hesabı oluşturduğunuzda, hesap için birincil bölge seçin. İkincil bölge seçimini birincil bölge üzerinde temel alır ve değiştirilemez. Bkz: [Azure Storage çoğaltma](storage-redundancy.md).

**Daha fazla bilgi Azure depolama hizmeti şifreleme (SSE) hakkında nereden alabilirim?**  
  
Aşağıdaki makalelere bakın:

-  [Azure depolama Güvenlik Kılavuzu](storage-security-guide.md)

-   [Rest verileri için Azure Storage hizmeti şifreleme](storage-service-encryption.md)

**Nasıl taşımak veya verileri bir depolama hesabından karşıdan yükleme?**

AzCopy veri yüklemek için kullanın. Daha fazla bilgi için daha fazla bilgi için bkz: [Windows AzCopy ile veri aktarma](storage-use-azcopy.md) ve [Linux'ta AzCopy ile veri aktarma](storage-use-azcopy-linux.md).


**Bir depolama hesabında verileri nasıl şifreliyor mu?**

Depolama hesabındaki şifreleme etkinleştirdikten sonra varolan veriler şifrelenmez. Var olan verileri şifrelemek için yeniden veri depolama hesabı yüklemeniz gerekir.  Bunu yapmak için şu adımları uygulayın:

Farklı bir depolama hesabı için verileri kopyalayın ve bir depolama hesabına geri taşımak için AZcopy kullanın. Aynı zamanda [bekleyen şifreleme](storage-service-encryption.md).

**Bir yerel makineye, diğerinden Portal yükleme seçeneğini kullanarak bir VHD'yi nasıl indirebilirim?**

Kullanabileceğiniz [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) bir VHD yüklemek için.

**Bir depolama hesabı çoğaltma için LRS GRS değiştirmek için Önkoşullar bulunur?**

Hayır. 

**Azure dosyaları yedekli depolama nasıl erişirim?**

Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS), yedekli depolama erişmek için gereklidir. Ancak, Azure dosyaları yalnızca LRS ve salt okunur erişime izin vermiyorsa standart GRS destekler. 

**Standart depolama birimine nasıl Premium depolama biriminden hareket?**

Bunu yapmak için şu adımları uygulayın:

1.  Yeni bir standart depolama hesabı oluşturun (veya aboneliğinizde var olan bir standart depolama hesabı kullanabilirsiniz).

2.  AzCopy indirin. Aşağıdaki AzCopy komutlardan birini çalıştırın.
      
    Tüm diskleri depolama hesabına kopyalamak için:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /S 

    Yalnızca bir disk kopyalamak için desen diskin adını sağlayın.

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /Pattern:abc.vhd

   
İşlemin tamamlanması birkaç saat sürebilir.

Aktarım başarıyla tamamlandığını emin olmak için hedef depolama hesabı kapsayıcısının Azure portalında inceleyin. Diskleri için standart depolama hesabı kopyaladıktan sonra bunları varolan bir diski sanal makineye ekleyebilirsiniz. Daha fazla bilgi için bkz: [Azure portalında Windows sanal makine için bir yönetilen veri diski eklemek nasıl](../../virtual-machines/windows/attach-managed-disk-portal.md).  
  
**Bir dosya paylaşımı için nasıl Premium depolama alanına Dönüştür?**

Premium depolama Azure dosya paylaşımına izin verilmiyor.

**Premium depolama hesabı için nasıl bir standart depolama biriminden yükseltme? Standart depolama hesabı için nasıl bir Premium depolama biriminden düşürmek?**

- Hedef depolama hesabı oluşturma, veri kaynağı hesabından hedef hesabına kopyalamak ve ardından kaynak hesabı silmeniz gerekir.

- AzCopy gibi bir araç veri kopyalama işlemini gerçekleştirmek için kullanabilirsiniz.

- Sanal makineler de varsa, gerçekleştirmeniz gereken bazı ek adımlar vardır, depolama hesabı verileri geçirmeden önce. Daha fazla bilgi için bkz: [Azure Premium Storage (yönetilmeyen diskleri) geçirme](storage-migration-to-premium-storage.md).

**Klasik depolama hesabından bir Azure Resource Manager depolama hesabı nasıl taşıyabilirim?**

1.  Taşıma AzureStorageAccount cmdlet'ini kullanabilirsiniz.

2.  Bu cmdlet birden çok adımı (doğrulama, hazırlama, yürütme) vardır ve gerçekte yapmadan önce taşıma doğrulayabilirsiniz.

3.  Sanal makineler de varsa, depolama hesabı verileri geçirmeden önce gerçekleştirmeniz gereken bazı ek adımlar vardır. Daha fazla bilgi için bkz: [Azure PowerShell kullanarak geçirmek Iaas kaynaklardan Klasik Azure Kaynak Yöneticisi](../..//virtual-machines/windows/migration-classic-resource-manager-ps.md).

**Nasıl veri Linux tabanlı bir bilgisayara bir Azure depolama hesabından karşıdan yükle, veya Linux makine verileri yüklemek mi?**

Azure CLI kullanabilirsiniz.

-   Tek bir blob indirin

        azure storage blob download -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -b "<Remote File Name>" -d "<Local path where the file will be downloaded to>"

-   Tek bir blob karşıya yükle: 

        azure storage blob upload -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -f "<Local File Name>"

**Nasıl ı diğer kişilerin depolama Kaynaklarım erişim verebilirsiniz?**

Diğer kişilerin depolama kaynaklarına erişmesini sağlamak için:

-   Bir kaynağa erişim sağlamak için bir paylaşılan erişim imzası (SAS) belirteci kullanır. 

-   Depolama hesabı için birincil veya ikincil anahtarı olan bir kullanıcı sağlayın. Daha fazla bilgi için bkz: [depolama hesabınızı yönetme](storage-create-storage-account.md#manage-your-storage-account).

-   Erişim İlkesi anonim erişime izin verecek şekilde değiştirin. Daha fazla bilgi için bkz: [kapsayıcılar ve bloblar için anonim kullanıcıların izinleri verin](../blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

**AzCopy yüklendiği?**

-   "Microsoft Azure Storage komut satırından" AzCopy erişim, 'AzCopy.' yazın. Komut satırı AzCopy yüklenir.

-   32-bit sürümünü yüklediyseniz, bunu burada bulunur: **% ProgramFiles(x86) %\\Microsoft SDKs\\Azure\\AzCopy.**

-   64-bit sürümünü yüklediyseniz, bunu burada bulunur: **% ProgramFiles %\\Microsoft SDKs\\Azure\\AzCopy**.

**(Örneğin, ZRS, GRS veya RA-GRS) çoğaltılan depolama hesabı için ikincil bölge'de depolanan verileri nasıl erişirim?**

-   Bölge olarak yedekli depolama (ZRS) veya coğrafi olarak yedekli depolama (GRS) kullanıyorsanız, bir yük devretme oluşmadığı sürece ikincil bölge ' veri erişemiyor. Yük devretme işlemi hakkında daha fazla bilgi için bkz: [depolama yük devretme oluşursa beklenmesi gerekenler](storage-disaster-recovery-guidance.md#what-to-expect-if-a-storage-failover-occurs).

-   Okuma erişimli coğrafi olarak yedekli depolama birimi kullanıyorsanız (**RA-GRS**), herhangi bir zamanda veri ikincil bölge ' erişebilirsiniz. Bunu yapmak için aşağıdaki yöntemlerden birini kullanın:  
      
    AzCopy: Ekle '-ikincil ' ikincil uç noktasına erişmek için URL'de depolama hesabı adı. Örneğin:  
     
    https://storageaccountname-Secondary.BLOB.Core.Windows.NET/vhds/BlobName.vhd

    SAS belirteci: uç noktasından verilere erişmek için bir SAS belirteci kullanın. Daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS)](storage-dotnet-shared-access-signature-part-1.md).

**Depolama Hesabımı ile nasıl bir HTTPS özel etki alanı kullanıyor? Örneğin, nasıl yaptığım "https://mystorageaccountname.blob.core.windows.net/images/image.gif" "https://www.contoso.com/images/image.gif" görüntülenir?**

SSL özel etki alanları ile depolama hesapları şu anda desteklenmiyor.
Ancak, HTTPS olmayan özel etki alanlarını kullanabilirsiniz. Daha fazla bilgi için bkz: [Blob storage uç noktanız için özel etki alanı adı yapılandırma](../blobs/storage-custom-domain-name.md).

**Bir depolama hesabı verilere erişmek için FTP nasıl kullanırım?**

FTP kullanarak doğrudan bir depolama hesabına erişmek için bir yolu yoktur. Ancak, bir Azure sanal makine kurun ve ardından bir FTP sunucusu sanal makineye yükleyin. Bir Azure dosya paylaşımı veya sanal makine için kullanılabilir bir veri diski dosyaları depolamak FTP sunucusu olabilir.
Yalnızca veri Depolama Gezgini veya benzer bir uygulamada kullanmak zorunda kalmadan indirmek istiyorsanız, bir SAS belirteci kullanmak mümkün olabilir. Daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma gereksiniminiz varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.