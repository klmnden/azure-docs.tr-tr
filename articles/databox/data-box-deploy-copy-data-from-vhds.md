---
title: Microsoft Azure Data Box ile yönetilen disklere kopyalayın ve Vhd'lerden verileri içeri aktarma | Microsoft Docs
description: Veri Vhd'lerden şirket içi VM iş yüklerini Azure Data Box için kopyalama hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 02/27/2019
ms.author: alkohli
ms.openlocfilehash: ec2013a793f766221a66912d6de9d8da8b8106dd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59282568"
---
# <a name="tutorial-use-data-box-to-import-data-as-managed-disks-in-azure"></a>Öğretici: Yönetilen diskler azure'da kullanım Data Box'ı olarak verileri içeri aktarmak için

Bu öğreticide, şirket içi VHD'ler azure'da yönetilen disklere geçirmek için Azure Data Box kullanmayı açıklar. Şirket içi Vm'leri Vhd'lerden Data Box, sayfa blobları kopyalanır ve Azure yönetilen diskler olarak yüklenir. Bu yönetilen diskler can sonra Azure Vm'lerine bağlı olmalıdır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları gözden geçirin
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladınız [Öğreticisi: Azure Data Box ' ayarlamak](data-box-deploy-set-up.md).
2. Data Box'ınızı aldınız ve sipariş durumu Portalı'nda **teslim edildi**.
3. Yüksek hızlı bir ağa bağlı olursunuz. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı kullanılabilir değilse, 1 GbE veri bağlantı kullanır, ancak kopyalama hızı etkilenir.
4. Gözden geçirdiğimize göre:

    - Desteklenen [yönetilen disk boyutları Azure nesne boyutu sınırları](data-box-limits.md#azure-object-size-limits).
    - [Azure tanıtımını yönetilen diskler](/azure/virtual-machines/windows/managed-disks-overview). 

## <a name="connect-to-data-box"></a>Data Box'a bağlanma

Data Box, belirtilen kaynak gruplarında bağlı olarak, her bir ilişkili kaynak grubu için bir paylaşım oluşturur. Örneğin, varsa `mydbmdrg1` ve `mydbmdrg2` olduğunda oluşturulan sipariş verme, aşağıdaki paylaşımları oluşturulur:

- `mydbmdrg1_MDisk`
- `mydbmdrg2_MDisk`

İçinde her paylaşımı, depolama hesabınızdaki kapsayıcılar, karşılık gelen aşağıdaki üç klasör oluşturulur.

- Premium SSD
- Standart HDD
- Standart SSD

Aşağıdaki tabloda Data Box'ınızı paylaşımları UNC yollarını gösterir.
 
|        Bağlantı protokolü           |             Paylaşımının UNC yolu                                               |
|-------------------|--------------------------------------------------------------------------------|
| SMB |`\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Premium SSD>\file1.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Standard HDD>\file2.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Standard SSD>\file3.vhd` |  
| NFS |`//<DeviceIPAddress>/<ResourceGroup1_MDisk>/<Premium SSD>/file1.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<Standard HDD>/file2.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<Standard SSD>/file3.vhd` |

Olup, SMB veya NFS Data Box paylaşımlarına bağlanmak için kullandığınıza bağlı olarak, bağlanma adımları farklıdır.

> [!NOTE]
> REST bağlanmak için bu özellik desteklenmiyor.

### <a name="connect-to-data-box-via-smb"></a>SMB üzerinden Data Box'a bağlanın

Bir Windows Server ana bilgisayar kullanıyorsanız, Kutusu'na veri bağlamak için aşağıdaki adımları izleyin.

1. İlk adım kimlik doğrulamasından geçmek ve oturum başlatmaktır. **Bağlan ve kopyala**'ya gidin. Tıklayın **kimlik bilgilerini alma** kaynak grubunuz ile ilişkili paylaşımlar için erişim kimlik bilgileri alınamıyor. Erişim kimlik bilgilerini de alabilirsiniz **cihaz ayrıntıları** Azure portalında.

    > [!NOTE]
    > Yönetilen disklere yönelik tüm paylaşımlar için kimlik bilgilerini aynıdır.

    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-deploy-copy-data-from-vhds/get-share-credentials1.png)

2. Erişim paylaşımı ve kopyalama verileri iletişim kutusu, kopyalama **kullanıcıadı** ve **parola** paylaşım için. **Tamam** düğmesine tıklayın.
    
    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-deploy-copy-data-from-vhds/get-share-credentials2.png)

3. Kaynağınız ile ilişkili paylaşımlara erişmek için (*mydbmdrg1* aşağıdaki örnekte), ana bilgisayardan bir komut penceresi açın. Komut istemine şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Bu örnekte, UNC paylaşımı yolları aşağıdaki gibidir:

    - `\\169.254.250.200\mydbmdrg1_MDisk`
    - `\\169.254.250.200\mydbmdrg2_MDisk`
    
4. İstendiğinde paylaşımın parolasını girin. Aşağıdaki örnekte yukarıdaki komutla paylaşıma bağlanma adımları gösterilmektedir.

    ```
    C:\>net use \\169.254.250.200\mydbmdrgl_MDisk /u:mdisk
    Enter the password for ‘mdisk’ to connect to '169.254.250.200':
    The command completed successfully.
    C: \>
    ```

4. Windows + R tuşlarına basın. **Çalıştır** penceresinde `\\<device IP address>\<ShareName>` değerini belirtin. Tıklayın **Tamam** dosya Gezgini'ni açın.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer1.png)

    Artık aşağıdaki klasörleri her paylaşımı içinde precreated görmeniz gerekir.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer2.png)


### <a name="connect-to-data-box-via-nfs"></a>Data Box'NFS aracılığıyla bağlanma

Linux ana bilgisayarı kullanıyorsanız aşağıdaki adımları gerçekleştirerek Data Box'ı NFS istemcilerine izin verecek şekilde yapılandırın.

1. Paylaşıma erişmesine izin verilen istemcilerin IP adreslerini sağlayın. Yerel web arabiriminde **Bağlan ve kopyala** sayfasına gidin. **NFS ayarları** bölümünde **NFS istemci erişimi**'ne tıklayın.

    ![NFS istemci erişimini yapılandırma 1](media/data-box-deploy-copy-data-from-vhds/nfs-client-access1.png)

2. NFS istemcisinin IP adresini girin ve **Ekle**'ye tıklayın. Bu adımı tekrarlayarak birden fazla NFS istemcisi için erişim sağlayabilirsiniz. **Tamam** düğmesine tıklayın.

    ![NFS istemci erişimini yapılandırma 2](media/data-box-deploy-copy-data-from-vhds/nfs-client-access2.png)

2. Linux ana bilgisayarında NFS istemcisinin [desteklenen sürümünün](data-box-system-requirements.md) yüklü olduğundan emin olun. Linux dağıtımınıza uygun sürümü kullanın.

3. NFS istemcisi yüklendikten sonra, Data Box cihazınızdaki NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    Aşağıdaki örnekte, Data Box paylaşımına NFS yoluyla nasıl bağlanılacağı gösterilir. Data Box cihaz IP'si `169.254.250.200` şeklindedir, `mydbmdrg1_MDisk` ubuntuVM'ye bağlanmıştır ve bağlama noktası `/home/databoxubuntuhost/databox` ile belirtilmiştir.

    `sudo mount -t nfs 169.254.250.200:/mydbmdrg1_MDisk /home/databoxubuntuhost/databox`


## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

Veri sunucusuna bağlandıktan sonra sonraki adıma veri kopyalamaktır. VHD dosyası, sayfa blobu olarak hazırlama depolama hesabına kopyalanır. Sayfa blob'u ardından bir yönetilen diskin dönüştürülür ve bir kaynak grubuna taşındı.

Veri kopyalamaya başlamadan önce aşağıdaki konuları gözden geçirin:

- Her zaman VHD'lerin precreated klasörlerden birine kopyalayın. VHD'lerin dışında bu klasörleri veya oluşturduğunuz bir klasöre kopyalamak, VHD'ler sayfa blobları Azure depolama hesabına yüklenir ve yönetilen diskler değil.
- Yönetilen disk oluşturmak için yalnızca sabit VHD'lerin karşıya yüklenebilir. VHDX dosyası veya dinamik ve fark kayıt VHD'leri desteklenmez.
- Yalnızca bir kaynak grubunda belirli bir ada sahip bir yönetilen disk precreated klasörler arasında olabilir. Başka bir gelir precreated klasörlere karşıya VHD'ler benzersiz adlara sahip olmalıdır. Belirtilen ada, bir kaynak grubunda zaten mevcut bir yönetilen disk eşleşmiyor emin olun.
- Yönetilen disk sınırları gözden [Azure nesne boyutu sınırları](data-box-limits.md#azure-object-size-limits).

Olup, SMB veya NFS bağlanan bağlı olarak, şunları kullanabilirsiniz:

- [SMB üzerinden veri kopyalama](data-box-deploy-copy-data.md#copy-data-to-data-box)
- [NFS aracılığıyla veri kopyalama](data-box-deploy-copy-data-via-nfs.md#copy-data-to-data-box)

Son kopyalama işlerinin tamamlanmasını bekleyin. Sonraki adıma geçmeden önce hatasız kopyası işleri tamamlandığından emin emin olun.

![Herhangi bir hata ** Bağlan ve Kopyala ** sayfası](media/data-box-deploy-copy-data-from-vhds/verify-no-errors-connect-and-copy.png)

Kopyalama işlemi sırasında bir hata varsa, günlükleri indirmek **Bağlan ve Kopyala** sayfası.

- Hizalı değil 512 bayt olan dosya kopyaladıysanız, dosya hazırlama depolama hesabınıza sayfa blobu olarak karşıya değil. Günlüklerindeki bir hata görürsünüz. Dosyayı kaldırın ve 512 bayt hizalı bir dosyaya kopyalayın.

- Uzun bir ada sahip bir VHDX (Bu dosyaları desteklenmez) kopyaladıysanız, günlüklerindeki bir hata görürsünüz.

    ![Hata günlüklerinden ** Bağlan ve Kopyala ** sayfası](media/data-box-deploy-copy-data-from-vhds/errors-connect-and-copy.png)

    Sonraki adıma devam etmeden önce hataları giderin.

Veri bütünlüğünü sağlamak için sağlama toplamı veri kopyalama sırasında satır içinde hesaplanır. Kopyalama tamamlandıktan sonra cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.
    
![Panoda boş ve kullanılan alanı doğrulama](media/data-box-deploy-copy-data-from-vhds/verify-used-space-dashboard.png)

Kopyalama işi tamamlandığında gidebilirsiniz **göndermeye hazırlama**.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşulları gözden geçirin
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama


Data Box'ınızı Microsoft'a göndermeye hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)

