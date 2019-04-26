---
title: Azure'da verileri bir Windows server veya Windows bilgisayarda geri yükleme
description: Azure'da bir Windows server veya Windows bilgisayarda depolanan verileri geri yüklemeyi öğreneceksiniz.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 9/7/2018
ms.author: saurse
ms.openlocfilehash: d58b51f06c21c787e4aa720c803ab6533544d55c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60238474"
---
# <a name="restore-files-to-windows-by-using-the-azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modelini kullanarak Windows için dosyaları geri yükleme

Bu makalede, bir yedekleme kasasından veri geri yükleme açıklanmaktadır. Verileri geri yüklemek için Microsoft Azure kurtarma Hizmetleri (MARS) aracısı Veri Kurtarma Sihirbazı'nı kullanın. Şunları yapabilirsiniz:

* Veri yedekleri, alınan aynı makineye geri yükleyin.
* Verileri alternatif bir makineye geri yükleme.

Yazılabilir bir kurtarma noktası anlık görüntü kurtarma birimi olarak takmak için anında geri yükleme özelliği kullanın. Ardından, seçmeli olarak başvurulabilir, böylece dosyaları geri yükleme kurtarma birimi ve kopyalama dosyaları yerel bir bilgisayara keşfedebilirsiniz.

> [!NOTE]
> [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anında geri yükleme verileri geri yüklemek için kullanmak istiyorsanız gereklidir. Ayrıca, yedekleme verileri destek makalesinde listelenen yerel kasalardaki korunması gerekir. Başvurun [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anında geri yükleme desteği yerel en son listesi için.
>

Azure portalında kurtarma Hizmetleri kasaları ile anında geri yükleme'yi kullanın. Veri yedekleme kasalarında depolanan, Kurtarma Hizmetleri kasalarına dönüştürüldü. Anında geri yükleme kullanmak istiyorsanız, MARS güncelleştirmesini indirin ve anında geri yükleme bahsetmek yordamları izleyin.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Aynı makineye verilerini kurtarmak için anında geri yükleme'ı kullanın

Yanlışlıkla silinen bir dosya ve (yedeğin alındığı) aynı makinede geri yüklemek istiyorsanız aşağıdaki adımları verilerini kurtarmanıza yardımcı olur.

1. **Microsoft Azure Backup** ek bileşenini açın. Ek bileşenini yüklendiği bilmiyorsanız, bilgisayar veya sunucu için arama **Microsoft Azure Backup**.

    Masaüstü uygulaması, arama sonuçlarında görüntülenmesi gerekir.

2. Seçin **veri kurtarma** Sihirbazı'nı başlatın.

    ![Ekran görüntüsü, Azure Backup, vurgulanan verileri Kurtar](./media/backup-azure-restore-windows-server/recover.png)

3. Üzerinde **Başlarken** seçin sayfasında verileri aynı sunucu veya bilgisayara geri yüklemek için **bu sunucu (`<server name>`)** > **sonraki**.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı Başlarken sayfası](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Üzerinde **kurtarma modunu Seç** sayfasında **dosyalara ve klasörlere** > **sonraki**.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı kurtarma modunu Seç sayfası](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
   > [!IMPORTANT]
   > Tek tek dosya ve klasörleri geri yükleme seçeneğini .NET Framework 4.5.2 gerektirir veya üzeri. Görmüyorsanız, **dosyalara ve klasörlere** seçeneği, .NET Framework sürüm 4.5.2 yükseltmelisiniz veya sonraki bir sürümü ve yeniden deneyin.

   > [!TIP]
   > **Dosyalara ve klasörlere** seçeneği kurtarma noktası verilere hızlı erişim sağlar. 80 GB'den fazla Örneğimiz boyutları ile tek tek dosyaların kurtarılması için uygundur ve aktarım sunar veya kopyalama, Kurtarma sırasında en fazla 6 MB/sn hızlandırır. **Birim** seçeneği, belirtilen bir birimdeki tüm yedeklenen verileri kurtarır. Bu seçenek, daha hızlı bir aktarım hızı (en fazla 60 MB/sn), büyük boyutlu veri veya tüm birimleri kurtarmak için ideal olan sağlar.

5. Üzerinde **birim ve tarih seçin** sayfasında, geri yüklemek istediğiniz klasörleri ve dosyaları içeren birimi seçin.

    Takvimde bir kurtarma noktası seçin. Tarihler **kalın** en az bir kurtarma noktasının kullanılabilirliğini gösterir. Birden fazla kurtarma noktası içinde tek bir tarihi kullanılabilir değilse belirli bir kurtarma noktasından seçin **zaman** açılan menüsü.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı birim ve tarih seçin sayfası](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Geri yüklemek için kurtarma noktası seçtikten sonra seçin **bağlama**.

    Azure Backup, yerel kurtarma noktası bağlar ve kurtarma birimi olarak kullanır.

7. Üzerinde **göz atma ve dosyaları kurtarmak** sayfasında **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulmak için.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı göz atın ve kurtarma dosyalar sayfası](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. Windows Gezgini'nde, dosyaları ve klasörleri geri yüklemek istediğiniz kopyalayın ve bunları sunucu veya bilgisayar için yerel olan herhangi bir konuma yapıştırın. Açabilir veya doğrudan kurtarma biriminden dosyaları akışla aktarma ve doğru sürümleri kurtarmakta olduğunu doğrulayın.

    ![Windows Gezgini'nin ekran vurgulanmış kopyalama](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)


9. İşiniz üzerinde bittiğinde **göz atma ve dosyaları kurtarmak** sayfasında **çıkarma**. Ardından **Evet** birimi çıkarmak istediğinizi onaylayın.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı göz atın ve kurtarma dosyalar sayfası](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Seçmezseniz, **çıkarma**, ne zaman takılı süreden itibaren 6 saat için kurtarma birimi takılı kalır. Ancak, en çok 24 saat devam eden bir dosya kopyalama durumunda bağlama zamanı genişletilir. Hiçbir yedekleme işlemleri, toplu bağlıyken çalıştırılır. Ne zaman birimin takılı olduğu süre boyunca çalışmak üzere zamanlanmış herhangi bir yedekleme işlemi, Kurtarma birimi kaldırılan sonra çalışır.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Anında geri yükleme verileri alternatif bir makineye geri yüklemek için kullanın
Tüm sunucunuzu kaybolursa, verileri Azure Backup'tan başka bir makine kurtarmaya devam edebilirsiniz. Aşağıdaki adımlar, iş akışını göstermektedir.


Bu adımlar aşağıdaki terimler:

* *Kaynak makine* – özgün makineden hangi yedeğin ve hangi şu anda kullanılamıyor.
* *Hedef makine* – olduğu veri kurtarılıyor makine.
* *Örnek kasası* – kurtarma Hizmetleri kasası, hedef makine ve kaynak makine kayıtlı. <br/>

> [!NOTE]
> İşletim sisteminin önceki bir sürümünü çalıştıran bir hedef makine yedekleri geri yüklenemez. Örneğin, bir Windows 8 (veya sonrası) bilgisayarda Windows 7 bilgisayarda gerçekleştirilen bir yedekleme geri yüklenebilir. Bir Windows 8 bilgisayarında gerçekleştirilen bir yedekleme, Windows 7 bilgisayara yüklenemez.
>
>

1. Açık **Microsoft Azure Backup** ek bileşenini hedef makinede.

2. Hedef makine ve kaynak makine aynı kurtarma Hizmetleri kasasına kayıtlı olduğundan emin olun.

3. Seçin **veri kurtarma** açmak için **Veri Kurtarma Sihirbazı'nı**.

    ![Ekran görüntüsü, Azure Backup, vurgulanan verileri Kurtar](./media/backup-azure-restore-windows-server/recover.png)

4. Üzerinde **Başlarken** sayfasında **başka bir sunucuya**.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı Başlarken sayfası](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Örnek Kasası'na karşılık gelen kasa kimlik bilgilerini sağlayın ve seçin **sonraki**.

    Kasa kimlik bilgilerini geçersiz (veya süresi dolmuş) ise, örnek kasasından Azure portalında yeni bir kasa kimlik bilgileri dosyası indirin. Geçerli bir kasa kimlik bilgileri verdikten sonra karşılık gelen yedekleme kasasının adını görünür.


6. Üzerinde **yedekleme sunucusu seçin** sayfasında, görüntülenen makineler listesinden kaynak makine seçin ve parolayı girin. Sonra **İleri**’yi seçin.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı yedekleme sunucusu seçin sayfası](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Üzerinde **kurtarma modunu Seç** sayfasında **dosyalara ve klasörlere** > **sonraki**.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı kurtarma modunu Seç sayfası](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Üzerinde **birim ve tarih seçin** sayfasında, geri yüklemek istediğiniz klasörleri ve dosyaları içeren birimi seçin.

    Takvimde bir kurtarma noktası seçin. Tarihler **kalın** en az bir kurtarma noktasının kullanılabilirliğini gösterir. Birden fazla kurtarma noktası içinde tek bir tarihi kullanılabilir değilse belirli bir kurtarma noktasından seçin **zaman** açılan menüsü.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı birim ve tarih seçin sayfası](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Seçin **bağlama** kurtarma noktasını kurtarma birimi olarak hedef makinenizde yerel olarak takmak için.

10. Üzerinde **dosyalara Gözat ve kurtarma** sayfasında **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulmak için.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı göz atın ve kurtarma dosyalar sayfasında](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. Windows Gezgini'nde, dosyaları ve klasörleri kurtarma biriminden kopyalayın ve bunları hedef makine konuma yapıştırın. Açabilir veya doğrudan kurtarma biriminden dosyaları akışla aktarma ve doğru sürümleri kurtarıldığını doğrulayın.

    ![Windows Gezgini'nin ekran vurgulanmış kopyalama](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. İşiniz üzerinde bittiğinde **göz atma ve dosyaları kurtarmak** sayfasında **çıkarma**. Ardından **Evet** birimi çıkarmak istediğinizi onaylayın.

    ![Ekran görüntüsü, kurtarma verileri Sihirbazı göz atın ve kurtarma dosyalar sayfasında](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Seçmezseniz, **çıkarma**, ne zaman takılı süreden itibaren 6 saat için kurtarma birimi takılı kalır. Ancak, en çok 24 saat devam eden bir dosya kopyalama durumunda bağlama zamanı genişletilir. Hiçbir yedekleme işlemleri, toplu bağlıyken çalıştırılır. Ne zaman birimin takılı olduğu süre boyunca çalışmak üzere zamanlanmış herhangi bir yedekleme işlemi, Kurtarma birimi kaldırılan sonra çalışır.
    >

## <a name="next-steps"></a>Sonraki adımlar
Dosya ve klasörlerinizi kurtarma yaptıktan sonra şunları yapabilirsiniz [yedeklemelerinizi yönetme](backup-azure-manage-windows-server.md).
