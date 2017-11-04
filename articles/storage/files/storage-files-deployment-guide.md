---
title: "Azure dosyaları dağıtma | Microsoft Docs"
description: "Azure dosyaları başlangıçtan bitişe kadar dağıtmayı öğrenin."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: a594f31c002556f9a5fddaa17fb19273065eed47
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-deploy-azure-files"></a>Azure Dosyaları’nı dağıtma
[Azure dosyaları](storage-files-introduction.md) tam olarak yönetilen dosya paylaşımları, endüstri standardı SMB protokolü erişilebilir bulutta sunar. Bu makalede Azure dosyaları kuruluşunuz içinde hemen hemen dağıtmak nasıl yapacağınızı gösterir.

Okuma tavsiye [bir Azure dosyaları dağıtımını planlama](storage-files-planning.md) bu makaledeki adımları izleyerek önce.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, aşağıdaki adımları önceden tamamlamış varsayılmaktadır:

- İstenen dayanıklılık ve şifreleme seçeneklerinizi istediğiniz bölgede bir Azure depolama hesabı oluşturulur. Bkz: [depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) bir depolama hesabı oluşturma hakkında adım adım yönergeler için.
- Bir Azure dosya paylaşımı depolama hesabınızdaki istenen kota oluşturulur. Bkz: [bir dosya paylaşımı oluşturmak](storage-how-to-create-file-share.md) bir dosya paylaşımı oluşturma hakkında adım adım yönergeler için.

## <a name="transfer-data-into-azure-files"></a>Azure dosyaları içine veri aktarımı
Bu saklı şirket içi gibi varolan dosya paylaşımları, yeni Azure dosya paylaşımına geçirmek isteyebilirsiniz. Bu bölümde Azure dosyasına veri paylaşımı gelen ayrıntılı çeşitli popüler yöntemler aracılığıyla taşıma gösterecektir [Planlama Kılavuzu](storage-files-planning.md#data-transfer-method)

### <a name="azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme)
Azure dosya eşitleme (Önizleme) esneklik, performans ve uyumluluk bir şirket içi dosya sunucusunun vermeden kuruluşunuzun dosya paylaşımları Azure dosyalarında merkezileştirmenizi sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Eşitleme mekanizması uzun vadeli kullanım için istenen değil olsa bile azure dosya eşitleme, verileri bir Azure dosya paylaşımı geçirmek için kullanılabilir. Azure dosya eşitleme Azure dosya paylaşım içine veri aktarmak için nasıl kullanılacağı hakkında daha fazla bilgi bulunabilir [bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md).

### <a name="azure-importexport"></a>Azure İçeri/Dışarı Aktarma
Azure içeri/dışarı aktarma hizmeti, büyük miktarlarda verinin bir Azure dosya paylaşımına sevkiyat sabit disk sürücüleri tarafından bir Azure veri merkezi için güvenli bir şekilde aktarım olanak sağlar. Bkz: [Azure depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanma](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) hizmet daha ayrıntılı bir genel bakış için.

> [!Note]  
> Azure içeri/dışarı aktarma hizmeti bir Azure dosya paylaşımı dosyalarından verme şu anda desteklemiyor.

Aşağıdaki adımlar, Azure dosya paylaşımınızı bir şirket içi konumdan verilerini içeri aktaracak.

1. Posta sabit disklere gereken sayıda Azure'da temin edin. Sabit diskler herhangi bir disk boyutta olabilir ancak ya da bir 2,5" olması gerekir veya 3,5" SSD veya SATA II veya SATA III standart destekleme HDD. 

2. Bağlanın ve her disk sunucu/veri aktarımı yaparken Bilgisayara bağlayın. En iyi performans için şirket içi dışarı aktarma işini yerel olarak verileri içeren sunucu üzerinde çalışan öneririz. Veri hizmet dosya sunucusu bir NAS cihazı olduğunda gibi bazı durumlarda bu mümkün olmayabilir. Bu durumda, her diski bir PC'de bağlama edilebilir.

3. Her bir sürücü başlatılmadığı, çevrimiçi olduğundan ve bir sürücü harfi atandığından emin olun. , Bir sürücüyü çevrimiçi duruma getirin, başlatmak ve bir sürücü harfi atamak için Disk Yönetimi MMC ek bileşenini (diskmgmt.msc) açın.

    - Çevrimiçi (Bu zaten çevrimiçi değilse), bir disk Getir, diskin Disk Yönetimi MMC alt bölmesinde sağ tıklatın ve "Çevrimiçi" seçmek için.
    - Bir disk başlatmak için diski daha düşük (disk çevrimiçi olduktan sonra) bölmesinde ve "Başlat" seçin, sağ tıklayın. "GPT" sorulduğunda seçtiğinizden emin olun.

        ![Disk Yönetimi MMC diski Başlat menüsünün ekran görüntüsü](media/storage-files-deployment-guide/transferdata-importexport-1.PNG)

    - Diske bir sürücü harfi atamak için "" çevrimiçi olması ve disk üzerindeki ayrılmamış sağ tıklatın ve "Yeni Basit Birim"'i tıklatın. Bu, sürücü harfi atamak olanak tanır. Bu daha sonra gerçekleştirilir gibi birimi biçimlendirmek gerekmez unutmayın.

        ![Disk Yönetimi MMC Yeni Basit Birim Sihirbazı'nın ekran görüntüsü](media/storage-files-deployment-guide/transferdata-importexport-2.png)

4. Veri kümesi CSV dosyası oluşturun. Veri kümesi CSV içi veri yoluna ve veri kopyalanacağı istenen Azure dosya paylaşımı arasında bir eşleme dosyasıdır. Örneğin, aşağıdaki veri kümesi CSV dosyası bir şirket içi dosya paylaşımı ("F:\shares\scratch") bir Azure dosya paylaşımına ("MyAzureFileShare") eşler:
    
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\shares\scratch\","MyAzureFileShare/",file,rename,"None",None
    ```

    Birden çok paylaşımları bir depolama hesabı ile belirtilebilir. Bkz: [dataset CSV dosyasını hazırlama](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-the-dataset-csv-file) daha fazla bilgi için.

5. Driveset CSV dosyası oluşturun. Driveset CSV dosyası şirket içi dışarı aktarma aracı kullanılabilir diskleri listeler. Örneğin, aşağıdaki driveset CSV dosya listeleri `X:`, `Y:`, ve `Z:` şirket içi kullanılacak sürücüler iş dışarı aktar:

    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    X,Format,SilentMode,Encrypt,
    Y,Format,SilentMode,Encrypt,
    Z,Format,SilentMode,Encrypt,
    ```
    
    Bkz: [driveset CSV dosyasını hazırlama](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-initialdriveset-or-additionaldriveset-csv-file) daha fazla bilgi için.

6. Kullanım [WAImportExport aracı](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) verilerinizi bir veya daha fazla sabit diskleri kopyalama.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
    ```

    > [!Warning]  
    > Sabit disk sürücülerini veya günlük dosyası verilerini disk hazırlığı tamamladıktan sonra değişiklik yapmayın.

7. [Bir içeri aktarma işi oluşturma](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-an-export-job).
    
### <a name="robocopy"></a>Robocopy
Robocopy Windows ve Windows Server ile birlikte gelen iyi bilinen kopyalama bir araçtır. Robocopy yerel olarak dosya paylaşımı oluşturma ve ardından Robocopy komutunu hedef olarak bağlanmış konumu kullanarak Azure dosyalarına veri aktarmak için kullanılabilir. Robocopy kullanma oldukça basittir:

1. [Azure dosya paylaşımını bağlama](storage-how-to-use-files-windows.md). En iyi performans için Azure dosya paylaşımı verileri içeren sunucu üzerinde yerel olarak takma öneririz. Veri hizmet dosya sunucusu bir NAS cihazı olduğunda gibi bazı durumlarda bu mümkün olmayabilir. Bu durumda, bir Bilgisayardaki Azure dosya paylaşımını bağlama edilebilir. Bu örnekte, `net use` komut satırında dosya paylaşımını bağlamak için kullanılır:

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. Kullanım `robocopy` verileri Azure dosya paylaşımına taşımak için komut satırında:

    ```
    robocopy <path-to-local-share> <path-to-azure-file-share> /E /Z /MT:32
    ```
    
    Robocopy çok sayıda istendiği kopyalama davranışını değiştirmek için seçenekleri vardır. Daha fazla bilgi için görüntülemek [Robocopy](https://technet.microsoft.com/library/cc733145.aspx) el ile sayfa.

### <a name="azcopy"></a>AzCopy
AzCopy ve Azure Blob Depolama yanı sıra Azure dosyaları gelen veri kopyalamak için tasarlanmış bir komut satırı yardımcı programını basit komutları en uygun performans ile kullanmaktır. AzCopy kullanarak kolaydır:

1. Karşıdan [AzCopy Windows en son sürümünü](http://aka.ms/downloadazcopy) veya [Linux](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#download-and-install-azcopy).
2. Kullanım `azcopy` verileri Azure dosya paylaşımına taşımak için komut satırında. Windows söz dizimi aşağıdaki gibidir: 

    ```
    azcopy /Source:<path-to-local-share> /Dest:https://<storage-account>.file.core.windows.net/<file-share>/ /DestKey:<storage-account-key> /S
    ```

    Linux üzerinde komut sözdizimi biraz farklıdır:

    ```
    azcopy --source <path-to-local-share> --destination https://<storage-account>.file.core.windows.net/<file-share>/ --dest-key <storage-account-key> --recursive
    ```

    AzCopy çok sayıda istendiği kopyalama davranışını değiştirmek için seçenekleri vardır. Daha fazla bilgi için görüntülemek [Windows AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) ve [AzCopy Linux'ta](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="automatically-mount-on-needed-pcsservers"></a>Gerekli bilgisayarları/sunucularda otomatik olarak bağlama
Bir şirket içi dosya paylaşımı değiştirmek için önceden paylaşımları üzerinde kullanılacak makinelerde bağlamak yararlıdır. Bu, makine listesini otomatik olarak yapılabilir.

> [!Note]  
> Bir Azure dosya paylaşımına bağlanması depolama hesabı anahtarı parola olarak kullanarak gerektirir, bu nedenle yalnızca güvenilen ortamlarda takma öneririz. 

### <a name="windows"></a>Windows
PowerShell üzerinde birden fazla PC'de bağlama komutu çalıştırın kullanılabilir. Aşağıdaki örnekte, `$computers` el ile doldurulur, ancak otomatik olarak bağlamak için bilgisayar listesini oluşturabilir. Örneğin, bu değişken sonuçları Active Directory'den ile doldurabilirsiniz.

```PowerShell
$computer = "MyComputer1", "MyComputer2", "MyComputer3", "MyComputer4"
$computer | ForEach-Object { Invoke-Command -ComputerName $_ -ScriptBlock { net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name> /PERSISTENT:YES } }
```

### <a name="linux"></a>Linux
SSH ile birlikte bir basit bash komut dosyası, aşağıdaki örnekte aynı sonucu yol açabilir. `$computer` Değişkeni kullanıcı tarafından doldurulmalıdır sol benzer:

```PowerShell
computer = ("MyComputer1" "MyComputer2" "MyComputer3" "MyComputer4")
for item in "${dur[@]}"
do
    ssh $item "sudo bash -c 'echo \"//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino\" >> /etc/fstab'", "sudo mount -a"
done
```

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme dağıtımını planlama](storage-sync-files-planning.md)
- [Windows Azure dosya sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
- [Linux üzerinde Azure dosya sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)