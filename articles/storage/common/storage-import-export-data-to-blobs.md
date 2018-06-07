---
title: Azure içeri/dışarı aktarma kullanarak Azure BLOB veri aktarmak | Microsoft Docs
description: Alma oluşturma ve Azure portalında Azure BLOB'ları gelen ve giden veri aktarımı işleri dışarı aktarma hakkında bilgi edinin.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: fe9292459134972b44037a58235cdd817030a956
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661006"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>Azure içeri/dışarı aktarma hizmetini Azure Blob depolama alanına veri aktarmak için kullanın

Bu makalede, Azure Blob depolama alanına güvenli bir şekilde büyük miktarlarda veri almak için Azure içeri/dışarı aktarma hizmeti kullanma hakkında adım adım yönergeler sağlar. Azure BLOB'ları veri almak için hizmet bir Azure veri merkezi verilerinizi içeren şifrelenmiş disk sürücülerini dağıtmayı gerektirir.  

## <a name="prerequisites"></a>Önkoşullar

Azure Blob depolama alanına veri aktarmak için bir alma işi oluşturmadan önce dikkatle gözden geçirin ve bu hizmet için Önkoşullar aşağıdaki listesini tamamlayın. Yapmanız gerekir:

- İçeri/dışarı aktarma hizmeti için kullanılabilir etkin bir Azure aboneliğinizin olması.
- Bir depolama kapsayıcısı ile en az bir Azure depolama hesabına sahip. Listesine bakın [desteklenen depolama hesapları ve depolama türleri için içeri/dışarı aktarma hizmeti](storage-import-export-requirements.md). Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz: [bir depolama hesabı oluşturmak nasıl](storage-create-storage-account.md#create-a-storage-account). Depolama kapsayıcısı hakkında daha fazla bilgi için Git [bir depolama kapsayıcısı oluşturmak](../blobs/storage-quickstart-blobs-portal.md#create-a-container).
- Diskleri yetecek sayıda [desteklenen türleri](storage-import-export-requirements.md#supported-disks). 
- Çalıştıran bir Windows sistemine sahip bir [desteklenen işletim sistemi sürümü](storage-import-export-requirements.md#supported-operating-systems). 
- BitLocker Windows sisteminde etkinleştirin. Bkz: [BitLocker'ı etkinleştirmek nasıl](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
- [Sürüm 1 WAImportExport karşıdan](https://www.microsoft.com/en-us/download/details.aspx?id=42659) Windows sisteminde. Varsayılan klasöre sıkıştırmasını `waimportexportv1`. Örneğin, `C:\WaImportExportV1`.


## <a name="step-1-prepare-the-drives"></a>1. adım: sürücüleri hazırlama

Bu adım, bir günlük dosyası oluşturur. Günlük dosyası sürücü seri numarası, şifreleme anahtarı ve depolama hesabı ayrıntıları gibi temel bilgileri depolar. 

Sürücüleri hazırlamak için aşağıdaki adımları gerçekleştirin.

1.  Disk sürücülerine Windows sistemi SATA bağlayıcıları üzerinden bağlanır.
1.  Tek bir NTFS birimi her sürücüde oluşturun. Birime bir sürücü harfini atayın. Bağlama kullanmayın.
2.  NTFS birimde BitLocker şifrelemesi etkinleştirin. Bir Windows Server sistemi kullanıyorsanız, konusundaki yönergeleri kullanın [Windows Server 2012 R2 BitLocker'ı etkinleştirmek nasıl](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
3.  Verileri şifreli birime kopyalayın. Sürükle ve bırak veya Robocopy ya da böyle bir kopya aracı kullanın.
4.  Yönetici ayrıcalıklarıyla bir PowerShell veya komut satırı penceresi açın. Dizin sıkıştırması açılmış klasör olarak değiştirmek için aşağıdaki komutu çalıştırın:
    
    `cd C:\WaImportExportV1`
5.  Sürücünün BitLocker anahtarını almak için aşağıdaki komutu çalıştırın:
    
    ` manage-bde -protectors -get <DriveLetter>: `
6.  Disk hazırlamak için aşağıdaki komutu çalıştırın. **Veri boyutuna bağlı olarak bu gün olarak birkaç saat sürebilir.** 

    ```
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session#<session number> /sk:<Storage account key> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /skipwrite 
    ```
    Bir günlük dosyası, aracın çalıştığı aynı klasörde oluşturulur. İki diğer dosyalar da oluşturulur - bir *.xml* dosyası (aracını çalıştırdığınız klasöre) ve bir *sürücü manifest.xml* dosyası (klasör veri bulunduğu).
    
    Kullanılan parametreler aşağıdaki tabloda açıklanmıştır:

    |Seçenek  |Açıklama  |
    |---------|---------|
    |/j:     |.Jrn uzantısı ile günlük dosyasının adıdır. Sürücü bir günlük dosyası oluşturulur. Günlük dosyası adı olarak disk seri numarası kullanmanızı öneririz.         |
    |/ID:     |Oturum kimliği Benzersiz oturum sayısı komutu her örneği için kullanın.      |
    |/SK:     |Azure depolama hesabı anahtarı.         |
    |/ t:     |Sürücü harfi edilmeye disk. Örneğin, sürücü `D`.         |
    |/bk:     |Sürücü için BitLocker anahtar. Sayısal parolasını çıktısından ` manage-bde -protectors -get D: `      |
    |/srcdir:     |Edilmeye diskinin sürücü harfi ve ardından `:\`. Örneğin, `D:\`.         |
    |/dstdir:     |Azure depolama alanındaki hedef kapsayıcısının adı.         |
    |/skipwrite:     |Hiçbir kopyalanması için gereken yeni ve varolan verileri diskteki olduğunu belirten hazırlıklı olmak için bir seçenektir.          |
7. Sevk edilmesi gereken her disk için önceki adımı yineleyin. Her komut satırını Çalıştır için sağlanan adıyla bir günlük dosyası oluşturulur.
    
    > [!IMPORTANT]
    > - Günlük dosyası ile birlikte bir `<Journal file name>_DriveInfo_<Drive serial ID>.xml` dosya da aracı bulunduğu aynı klasörde oluşturulur. .Xml dosyası günlük dosyası yerine günlük dosyası çok büyük ise, bir iş oluşturulurken kullanılır. 

## <a name="step-2-create-an-import-job"></a>2. adım: bir içeri aktarma işi oluşturma

Azure portalında bir alma işi oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oturum https://portal.azure.com/.
2. Git **tüm hizmetleri > depolama > içeri/dışarı aktarma işleri**. 
    
    ![İçeri/dışarı aktarma işleri gidin](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Tıklatın **oluşturma içeri/dışarı aktarma işi**.

    ![Create içeri/dışarı aktarma işini tıklatın](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. İçinde **Temelleri**:

    - Seçin **Azure içe**.
    - İçe aktarma işi için açıklayıcı bir ad girin. İşleriniz ilerlemesini izlemek için adını kullanın.
        - Ad yalnızca küçük harfler, sayılar, tire ve alt çizgi içerebilir.
        - Adı bir harf ile başlamalı ve boşluk içeremez.
    - Bir abonelik seçin.
    - Bir kaynak grubu seçin veya girin.  

    ![1. adım - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. İçinde **iş ayrıntıları**:

    - Sürücü hazırlık adımında elde ettiğiniz sürücü günlük dosyalarını karşıya yükleyin. Varsa `waimportexport.exe version1` edildi kullanıldığında, hazırlıklı her sürücü için bir dosya karşıya yükleme. 2 MB günlük dosyası boyutunu aşıyor sonra kullanabileceğiniz `<Journal file name>_DriveInfo_<Drive serial ID>.xml` de ile günlük dosyası oluşturulur. 
    - Veri bulunacağı hedef depolama hesabı seçin. 
    - Teslim konum, seçilen depolama hesabı bölgeye göre otomatik olarak doldurulur.
   
   ![2. adım - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. İçinde **dönüş Sevkiyat bilgisi**:

    - Taşıyıcı açılır listeden seçin.
    - Bu taşıyıcı ile oluşturduğunuz bir geçerli taşıyıcı hesap numarası girin. Microsoft, içe aktarma işi tamamlandıktan sonra geri için sürücüleri sevk etmek için bu hesabı kullanır. Bir hesap numarası yoksa oluşturma bir [FedEx](http://www.fedex.com/us/oadr/) veya [DHL](http://www.dhl.com/) taşıyıcı hesap.
    - Bir tam ve geçerli kişi adı, telefon, e-posta, adres, şehir, ZIP, bölge ve ülke/bölge sağlar.

    ![Adım 3 - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob5.png)
   
5. İçinde **Özet**:

    - Özet olarak sağlanan işi bilgilerini gözden geçirin. İş adı ve Azure geri disklere dağıtmayı Kargo Azure veri merkezi not edin. Bu bilgiler daha sonra sevkiyat etiketi kullanılır.
    - Tıklatın **Tamam** alma işi oluşturamadı.

    ![4. adım - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

## <a name="step-3-ship-the-drives"></a>3. adım: sürücüleri sevk 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]


## <a name="step-4-update-the-job-with-tracking-information"></a>Adım 4: izleme bilgilerini iş güncelleştirin.

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="next-steps"></a>Sonraki adımlar

* [İş ve sürücü durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İçeri/dışarı aktarma gereksinimlerini gözden geçirin](storage-import-export-requirements.md)


