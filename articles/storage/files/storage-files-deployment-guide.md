---
title: Azure dosyaları'nı dağıtma | Microsoft Docs
description: Azure dosyaları baştan sona dağıtmayı öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 05/22/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 6afe54d269d273c6a93e6431e9f1c1af7b18cc0e
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63766417"
---
# <a name="how-to-deploy-azure-files"></a>Azure Dosyaları’nı dağıtma
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları endüstri standardı SMB protokolünü erişilebilen bulutta sunar. Bu makale pratikte, kuruluşunuzda Azure dosyaları dağıtmak nasıl gösterir.

Okuma öneririz [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md) bu makaledeki adımları izleyerek önce.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, aşağıdaki adımları zaten tamamladığınız varsayılır:

- İstenen dayanıklılık ve şifreleme seçeneklerinizi istediğiniz bölgede bir Azure depolama hesabı oluşturulur. Bkz: [depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) depolama hesabı oluşturma konusunda adım adım yönergeler için.
- İstenen kotayı depolama hesabınızdaki bir Azure dosya paylaşımı oluşturulur. Bkz: [dosya paylaşımı oluşturma](storage-how-to-create-file-share.md) bir dosya paylaşımı oluşturma konusunda adım adım yönergeler için.

## <a name="transfer-data-into-azure-files"></a>Azure dosyaları ile veri aktarımı
Bu şirket içinde depolanan gibi mevcut dosya paylaşımları, yeni Azure dosya paylaşımına geçirmek isteyebilirsiniz. Bu bölüm veri öğesinden ayrıntılı çeşitli popüler yöntemler aracılığıyla bir Azure dosya paylaşımına taşımak yapmayı gösterir [Planlama Kılavuzu](storage-files-planning.md#data-transfer-method)

### <a name="azure-file-sync"></a>Azure Dosya Eşitleme
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu Windows sunucularınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Eşitleme mekanizması için uzun süreli kullanım istenen değilse bile azure dosya eşitleme, verileri bir Azure dosya paylaşımına geçirmek için kullanılabilir. Azure dosya paylaşımı veri aktarmak için Azure dosya eşitleme kullanma hakkında daha fazla bilgi bulunabilir [bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).

### <a name="azure-importexport"></a>Azure İçeri/Dışarı Aktarma
Azure içeri/dışarı aktarma hizmeti, bir Azure veri merkezine sabit disk sürücüleri sevkiyat tarafından bir Azure dosya paylaşımına güvenli bir şekilde büyük miktarlarda veri aktarmanıza olanak sağlar. Bkz: [Azure depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmetini kullanmak](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) hizmete ilişkin daha ayrıntılı bir genel bakış.

> [!Note]  
> Azure içeri/dışarı aktarma hizmeti, bir Azure Dosya paylaşımındaki dosyaları dışarı aktarma şu anda desteklemiyor.

Aşağıdaki adımlar, Azure dosya paylaşımınızı bir şirket içi konumdan verilerini içeri aktaracak.

1. Gereken sayıda posta sabit diskleri Azure'a edinin. Sabit diskler için disk boyutunu olabilir, ancak bu ya da bir 2,5" olmalıdır veya 3,5" SSD veya HDD, SATA II ve III SATA standart destek. 

2. Bağlanmak ve veri aktarımı yapan sunucu/bilgisayar her diski bağlayın. En iyi performans için şirket içi dışarı aktarma işi verileri içeren sunucu üzerinde yerel olarak çalıştırmanızı öneririz. Dosya sunucusu verileri hizmet veren bir NAS cihazı olduğunda bazı durumlarda bu mümkün olmayabilir. Bu durumda, her disk bir Bilgisayara bağlamak için mükemmel bir şekilde kullanılabilir.

3. Her sürücü çevrimiçi başlatılır ve bir sürücü harfi atanır emin olun. , Çevrimiçi bir sürücüye taşıyın, başlatın ve bir sürücü harfi atamak için Disk Yönetimi MMC ek bileşenini (diskmgmt.msc) açın.

    - Çevrimiçi (zaten çevrimiçi değilse), bir diski duruma, diskin Disk Yönetimi MMC alt bölmesinde sağ tıklayıp "Çevrimiçi" için.
    - Bir diski başlatmak için diski alt bölmesinde (disk çevrimiçi olduktan sonra) ve "Başlat" ı seçin, sağ tıklayın. Sorulduğunda "GPT" seçtiğinizden emin olun.

        ![Disk Yönetimi MMC diski Başlat menüsünde görüntüsü](media/storage-files-deployment-guide/transferdata-importexport-1.PNG)

    - Diske bir sürücü harfi atamak için "" çevrimiçi ve başlatılmış bir diskin üzerindeki ayrılmamış sağ tıklayın ve "Yeni Basit Birim"'i tıklatın. Bu, sürücü harfi atamak olanak tanır. Daha sonra bu gerçekleştirilir gibi birimi biçimlendirmek gerekmez unutmayın.

        ![Disk Yönetimi MMC yeni basit birim sihirbazında görüntüsü](media/storage-files-deployment-guide/transferdata-importexport-2.png)

4. Veri kümesi CSV dosyası oluşturabilirsiniz. İstediğiniz Azure dosya paylaşımını verilerin kopyalanacağı ve şirket içi veri yoluna arasındaki eşlemeyi veri kümesi CSV dosyasıdır. Örneğin, aşağıdaki veri kümesi CSV dosyası, bir Azure dosya paylaşımı ("MyAzureFileShare") bir şirket içi dosya paylaşımı ("F:\shares\scratch") eşlemeleri:
    
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\shares\scratch\","MyAzureFileShare/",file,rename,"None",None
    ```

    Birden çok paylaşımları bir depolama hesabı ile belirtilebilir. Bkz: [veri kümesi CSV dosyası hazırlama](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-the-dataset-csv-file) daha fazla bilgi için.

5. Driveset CSV dosyası oluşturabilirsiniz. Driveset CSV dosyası şirket içi dışarı aktarma aracı için kullanılabilen diskler listelenmiştir. Örneğin, aşağıdaki driveset CSV dosyası listeleri `X:`, `Y:`, ve `Z:` sürücüleri şirket içinde kullanılmak üzere dışarı aktarma işi:

    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    X,Format,SilentMode,Encrypt,
    Y,Format,SilentMode,Encrypt,
    Z,Format,SilentMode,Encrypt,
    ```
    
    Bkz: [driveset CSV dosyasını hazırlama](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-initialdriveset-or-additionaldriveset-csv-file) daha fazla bilgi için.

6. Kullanım [WAImportExport aracı](https://www.microsoft.com/download/details.aspx?id=55280) bir veya daha fazla sabit sürücüler için verilerinizi kopyalamak için.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
    ```

    > [!Warning]  
    > Sabit disk sürücülerini veya günlük dosyası verilerini disk hazırlığı tamamlandıktan sonra değişiklik yapmayın.

7. [İçeri aktarma işi oluşturma](../common/storage-import-export-data-to-files.md#step-2-create-an-import-job).
    
### <a name="robocopy"></a>Robocopy
Robocopy Windows ve Windows Server ile birlikte gelen bilinen kopya bir araçtır. Robocopy, yerel dosya paylaşımını bağlama ve ardından hedef Robocopy komutunu olarak bağlı konumu kullanarak Azure dosyaları veri aktarmak için kullanılabilir. Robocopy kullanılarak oldukça basit bir işlemdir:

1. [Azure dosya paylaşımınızı bağlamak](storage-how-to-use-files-windows.md). En iyi performans için verileri içeren sunucu üzerinde yerel olarak Azure dosya paylaşımını bağlama öneririz. Dosya sunucusu verileri hizmet veren bir NAS cihazı olduğunda bazı durumlarda bu mümkün olmayabilir. Bu durumda, Azure dosya paylaşımını bir PC'de edilebilir. Bu örnekte, `net use` komut satırında, dosya paylaşımını bağlamak için kullanılır:

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. Kullanım `robocopy` verileri Azure dosya paylaşımına taşımak için komut satırında:

    ```
    robocopy <path-to-local-share> <path-to-azure-file-share> /E /Z /MT:32
    ```
    
    Robocopy çok sayıda istediğiniz gibi kopyalama davranışı değiştirmek için seçenekler vardır. Daha fazla bilgi için görüntüleme [Robocopy](https://technet.microsoft.com/library/cc733145.aspx) el kitabı sayfası.

### <a name="azcopy"></a>AzCopy
AzCopy, en iyi performansı sunan basit komutlar kullanılarak Azure dosyaları yanı sıra, Azure Blob Depolama, gelen ve giden veri kopyalamak için tasarlanan bir komut satırı yardımcı programıdır. AzCopy kullanarak kolaydır:

1. İndirme [en son sürümünü Windows üzerinde AzCopy](https://aka.ms/downloadazcopy) veya [Linux](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#download-and-install-azcopy).
2. Kullanım `azcopy` verileri Azure dosya paylaşımına taşımak için komut satırına. Windows üzerinde sözdizimi aşağıdaki gibidir: 

    ```
    azcopy /Source:<path-to-local-share> /Dest:https://<storage-account>.file.core.windows.net/<file-share>/ /DestKey:<storage-account-key> /S
    ```

    Linux üzerinde ilgili komut söz dizimi biraz farklıdır:

    ```
    azcopy --source <path-to-local-share> --destination https://<storage-account>.file.core.windows.net/<file-share>/ --dest-key <storage-account-key> --recursive
    ```

    AzCopy, çok sayıda istediğiniz gibi kopyalama davranışı değiştirmek için seçenekler vardır. Daha fazla bilgi için görüntüleme [Windows üzerinde AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) ve [Linux üzerinde AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="automatically-mount-on-needed-pcsservers"></a>Gereken bilgisayarları/sunucularına otomatik olarak bağlama
Bir şirket içi dosya paylaşımı değiştirmek için önceden paylaşımları üzerinde kullanılacak makinelerde bağlamak yararlıdır. Bu, makinelerin bir listesini otomatik olarak yapılabilir.

> [!Note]  
> Azure dosya paylaşımını bağlama, parola olarak depolama hesabı anahtarını kullanarak gerektirir, bu nedenle yalnızca güvenilen ortamlarda bağlama öneririz. 

### <a name="windows"></a>Windows
Bağlama komutu birden çok bilgisayarlarında çalışan PowerShell kullanılabilir. Aşağıdaki örnekte, `$computers` el ile doldurulur, ancak otomatik olarak bağlamak için bilgisayar listesini oluşturabilirsiniz. Örneğin, bu değişken Active Directory'den sonuçlarla doldurabilirsiniz.

```powershell
$computer = "MyComputer1", "MyComputer2", "MyComputer3", "MyComputer4"
$computer | ForEach-Object { Invoke-Command -ComputerName $_ -ScriptBlock { net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name> /PERSISTENT:YES } }
```

### <a name="linux"></a>Linux
SSH ile birleştirilmiş bir basit bash betiği, aşağıdaki örnekte aynı sonucu sağlayabilir. `$computer` Değişken kullanıcı tarafından doldurulmalıdır sol benzer:

```
computer = ("MyComputer1" "MyComputer2" "MyComputer3" "MyComputer4")
for item in "${computer[@]}"
do
    ssh $item "sudo bash -c 'echo \"//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino\" >> /etc/fstab'", "sudo mount -a"
done
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Windows üzerinde Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
- [Linux üzerinde Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
