---
title: Bir Windows Server veya Windows bilgisayarı azure'da verileri geri yükleme
description: Windows Server veya Windows bilgisayarı için azure'da depolanan verileri geri yüklemeyi öğreneceksiniz.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 7/25/2018
ms.author: saurse
ms.openlocfilehash: a1c9df57ddebbb1cf471f705acfbd6651c151d7b
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247287"
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Resource Manager dağıtım modelini kullanarak bir Windows sunucusu veya Windows istemci makinesine dosyaları geri yükleme

Bu makalede, bir yedekleme kasasından veri geri yükleme açıklanmaktadır. Verileri geri yüklemek için Microsoft Azure kurtarma Hizmetleri (MARS) aracısı Veri Kurtarma Sihirbazı'nı kullanın. Verileri geri yüklediğinizde, filtrelenebilir:

* Veri yedekleri, alınan aynı makineye geri yükleyin.
* Verileri geri yüklemek için başka bir makineyi.

Ocak 2017'de Microsoft, MARS aracısı için bir önizleme güncelleştirme yayımladı. Hata düzeltmeleri ile birlikte, bu güncelleştirme, anında kurtarma birimi olarak yazılabilir bir kurtarma noktası anlık görüntüsünü bağlayın olanak tanıyan geri yükleme sağlar. Ardından, seçmeli olarak başvurulabilir, böylece dosyaları geri yükleme, yerel bir bilgisayara kurtarma birimi ve kopyalama dosyaları keşfedebilirsiniz.

> [!NOTE]
> [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anında geri yükleme verileri geri yüklemek için kullanmak istiyorsanız gereklidir. Ayrıca Yedekleme verileri destek makalesinde listelenen yerel kasalardaki korunması gerekir. Başvurun [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anında geri yükleme desteği yerel en son listesi için. Anında geri yükleme **değil** şu anda tüm bölgelerde kullanılabilir.
>

Azure portalında kurtarma Hizmetleri kasaları ile anında geri yükleme'yi kullanın. Veri yedekleme kasalarında depolanan, Kurtarma Hizmetleri kasalarına dönüştürüldü. Anında geri yükleme kullanmak istiyorsanız, MARS güncelleştirmesini indirin ve anında geri yükleme bahsetmek yordamları izleyin.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Aynı makineye verilerini kurtarmak için anında geri yükleme'ı kullanın

Yanlışlıkla silinen bir dosya ve (yedeğin alındığı) aynı makinede geri yüklemek istiyorsanız, aşağıdaki adımları verilerini kurtarmanıza yardımcı olur.

1. Açık **Microsoft Azure Backup** yaslama içinde. Ek bileşenini yüklendiği bilmiyorsanız, bilgisayar veya sunucu için arama **Microsoft Azure Backup**.

    Masaüstü uygulaması, arama sonuçlarında görüntülenmesi gerekir.

2. Tıklayın **veri kurtarma** Sihirbazı'nı başlatın.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server/recover.png)

3. Üzerinde **Başlarken** verileri aynı sunucuya veya bilgisayara geri yüklemek için bölmesinde seçin **bu sunucu (`<server name>`)** tıklatıp **sonraki**.

    ![Bu sunucu seçeneği aynı makinede verileri geri yüklemek için](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Üzerinde **kurtarma modunu Seç** bölmesinde seçin **dosyalara ve klasörlere** ve ardından **sonraki**.

    ![Dosyalara göz atın](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Üzerinde **birim ve tarih seçin** bölmesinde, dosyaları ve/veya geri yüklemek istediğiniz klasörleri içeren birimi seçin.

    Takvimde bir kurtarma noktası seçin. Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. Tarihler **kalın** en az bir kurtarma noktasının kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa bir tarih seçtiğinizde belirli bir kurtarma noktasından seçin **zaman** açılan menüsü.

    ![Birim ve tarih](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Geri yüklemek için kurtarma noktası seçtikten sonra tıklayın **bağlama**.

    Azure Backup, yerel kurtarma noktası bağlar ve kurtarma birimi olarak kullanır.

7. Üzerinde **göz atma ve dosyaları kurtarmak** bölmesinde tıklayın **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulmak için.

    ![Kurtarma Seçenekleri](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. Windows Gezgini'nde, dosyaları ve/veya klasörleri geri yükleme ve sunucu veya bilgisayar için yerel olan herhangi bir konuma yapıştırın istediğiniz kopyalayın. Açabilir veya doğrudan kurtarma biriminden dosyaları akışla aktarma ve doğru sürümleri kurtarılan doğrulayın.

    ![Dosyaları ve yerel konum takılı birim klasörleri kopyalama ve yapıştırma](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. İşiniz bittiğinde şirket dosyaları ve/veya klasörleri geri **göz atma ve kurtarılan dosyaların** bölmesinde tıklayın **çıkarma**. Ardından **Evet** birimi çıkarmak istediğinizi onaylayın.

    ![Birim çıkarın ve onaylayın](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Çıkarma işlemi tıklamayın, Kurtarma birimi zaman oluşturulmasından andan itibaren 6 saat boyunca takılı kalır. Ancak, bağlama genişletilmiş en fazla 24 saat devam eden bir dosya kopyalama'olması durumunda en fazla süredir. Hiçbir yedekleme işlemleri, toplu bağlıyken çalıştırılır. Birim, bağlı olduğu süre boyunca çalışmak üzere zamanlanmış herhangi bir yedekleme işlemi kurtarma birimi kaldırılan sonra çalışır.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Anında geri yükleme verileri alternatif bir makineye geri yüklemek için kullanın
Tüm sunucunuzu kaybolursa, verileri Azure Backup'tan başka bir makine kurtarmaya devam edebilirsiniz. Aşağıdaki adımlar, iş akışını göstermektedir.


Bu adımlarda kullanılan terminolojiyi içerir:

* *Kaynak makine* – özgün makineden alındığı ve hangi şu anda kullanılamıyor.
* *Hedef makine* – olduğu veri kurtarılıyor makine.
* *Örnek kasası* – kurtarma Hizmetleri kasası için *kaynak makine* ve *hedef makine* kaydedilir. <br/>

> [!NOTE]
> İşletim sisteminin önceki bir sürümü çalıştıran bir hedef makine yedekleri geri yüklenemez. Örneğin, bir Windows bilgisayarı olabilir 7 gerçekleştirilen bir yedekleme bilgisayar bir Windows 8 veya daha sonra geri. Bir Windows 8 bilgisayarında gerçekleştirilen bir yedekleme, Windows 7 bilgisayara yüklenemez.
>
>

1. Açık **Microsoft Azure Backup** şirket içinde yaslama *hedef makine*.

2. Olun *hedef makine* ve *kaynak makine* aynı kurtarma Hizmetleri kasasına kayıtlı.

3. Tıklayın **veri kurtarma** açmak için **Veri Kurtarma Sihirbazı'nı**.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server/recover.png)

4. Üzerinde **Başlarken** bölmesinde **başka bir sunucu**

    ![Başka bir sunucu](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Karşılık gelen kasa kimlik bilgilerini sağlayın *örnek kasası*, tıklatıp **sonraki**.

    Kasa kimlik bilgilerini geçersiz (veya süresi dolmuş) ise, yeni bir kasa kimlik bilgileri dosyasını indirin *örnek kasası* Azure portalında. Geçerli bir kasa kimlik bilgilerini sağladığınızda, karşılık gelen yedekleme kasasının adını görünür.


6. Üzerinde **yedekleme sunucusu seçin** bölmesinde seçin *kaynak makine* görüntülenen makineler listesinden ve parolayı girin. Ardından **İleri**'ye tıklayın.

    ![Makinelerin listesi](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Üzerinde **kurtarma modunu Seç** bölmesinde seçin **dosyalara ve klasörlere** tıklatıp **sonraki**.

    ![Arama](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Üzerinde **birim ve tarih seçin** bölmesinde, dosyaları ve/veya geri yüklemek istediğiniz klasörleri içeren birimi seçin.

    Takvimde bir kurtarma noktası seçin. Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. Tarihler **kalın** en az bir kurtarma noktasının kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa bir tarih seçtiğinizde belirli bir kurtarma noktasından seçin **zaman** açılan menüsü.

    ![Arama öğeleri](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Tıklayın **bağlama** yerel olarak kurtarma noktası üzerinde bir kurtarma birimi bağlamak için *hedef makine*.

10. Üzerinde **göz atma ve dosyaları kurtarmak** bölmesinde tıklayın **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulmak için.

    ![Şifreleme](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. Windows Gezgini'nde, dosyaları ve/veya klasörleri kurtarma biriminden kopyalayıp bunları sizin *hedef makine* konumu. Açabilir veya doğrudan kurtarma biriminden dosyaları akışla aktarma ve doğru sürümleri kurtarılan doğrulayın.

    ![Şifreleme](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. İşiniz bittiğinde şirket dosyaları ve/veya klasörleri geri **göz atma ve kurtarılan dosyaların** bölmesinde tıklayın **çıkarma**. Ardından **Evet** birimi çıkarmak istediğinizi onaylayın.

    ![Şifreleme](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Çıkarma işlemi tıklamayın, Kurtarma birimi zaman oluşturulmasından andan itibaren 6 saat boyunca takılı kalır. Ancak, bağlama genişletilmiş en fazla 24 saat devam eden bir dosya kopyalama'olması durumunda en fazla süredir. Hiçbir yedekleme işlemleri, toplu bağlıyken çalıştırılır. Birim, bağlı olduğu süre boyunca çalışmak üzere zamanlanmış herhangi bir yedekleme işlemi kurtarma birimi kaldırılan sonra çalışır.
    >

## <a name="next-steps"></a>Sonraki adımlar
* Dosya ve klasörlerinizi kurtarma yaptıktan sonra şunları yapabilirsiniz [yedeklemelerinizi yönetme](backup-azure-manage-windows-server.md).
