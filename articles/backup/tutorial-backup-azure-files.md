---
title: Azure Backup hizmeti ile Azure dosyaları dosya paylaşımlarını yedekleme
description: Bu öğreticide, Azure dosya paylaşımlarını yedekleme açıklanmaktadır.
services: backup
author: dcurwin
ms.author: dacurwin
ms.date: 06/10/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: 474d5454e30c35d3f3ccf4ea994093ef47bd6ceb
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275995"
---
# <a name="back-up-azure-file-shares"></a>Azure dosya paylaşımlarını yedekleme
Bu makalede, Azure portalını kullanarak [Azure dosya paylaşımlarını](../storage/files/storage-files-introduction.md) yedekleme ve geri yükleme işlemlerinin nasıl yapılacağı açıklanmaktadır.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure Dosyalarını yedeklemek için Kurtarma Hizmetleri kasasını yapılandırma
> * Geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırma


## <a name="prerequisites"></a>Önkoşullar
Azure dosya paylaşımını yedekleyebilmeniz için önce [desteklenen Depolama Hesabı türlerinden](tutorial-backup-azure-files.md#limitations-for-azure-file-share-backup-during-preview) birinde yer aldığından emin olmalısınız. Bunu doğruladıktan sonra dosya paylaşımlarınızı koruyabilirsiniz.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Önizleme sırasında Azure dosya paylaşımı yedeklemesi için kısıtlamalar
Azure dosya paylaşımları için yedekleme önizlemede sunulmaktadır. Hem genel amaçlı v1'de Azure dosya paylaşımları ve genel amaçlı v2 depolama hesapları desteklenmektedir. Aşağıdaki yedekleme senaryoları, Azure dosya paylaşımları için desteklenmemektedir:
- Sanal Ağların veya Güvenlik Duvarının etkin olduğu depolama hesaplarında Azure dosya paylaşımlarını koruyamazsınız.
- Azure Backup'ı kullanarak Azure dosyaları korumak için kullanılabilir hiçbir CLI yoktur.
- Günlük zamanlanan maksimum yedekleme sayısı birdir.
- Günlük zamanlanan maksimum istek üzerine yedekleme sayısı dörttür.
- Kurtarma Hizmetleri kasanızdaki yedeklemelerin yanlışlıkla silinmesini önlemek için depolama hesabındaki [kaynak kilitlerini](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) kullanın.
- Azure Backup tarafından oluşturulan anlık görüntülerin silmeyin. Anlık görüntülerin silinmesi, kurtarma noktalarının kaybolması ve/veya geri yükleme işlemlerinin başarısız olmasıyla sonuçlanabilir
- Azure Backup tarafından korunan dosya paylaşımları silmeyin. Geçerli çözüm dosya paylaşımı silindiğinde, Azure Backup tarafından alınan tüm anlık görüntüleri silin ve bu nedenle tüm geri yükleme noktalarını kaybedersiniz.

İle depolama hesaplarında Azure dosya paylaşımları için geri [bölgesel olarak yedekli depolama](../storage/common/storage-redundancy-zrs.md) (ZRS) çoğaltma şu anda yalnızca orta ABD (CUS), Doğu ABD (EUS), Doğu ABD 2 (EUS2), Kuzey Avrupa (NE), Güneydoğu Asya (SEA), Batı Avrupa (WE) ve Batı ABD 2 (WUS2).

## <a name="configuring-backup-for-an-azure-file-share"></a>Azure dosya paylaşımı için yedeklemeyi yapılandırma
Bu öğreticide zaten yerleşik bir Azure dosya paylaşımınız olduğu varsayılır. Azure dosya paylaşımınızı yedeklemek için:

1. Dosya paylaşımınızla aynı bölgede bir Kurtarma Hizmetleri kasası oluşturun. Zaten bir kasanız varsa, kasanızın Genel Bakış sayfasını açın ve **Yedekle**'ye tıklayın.

    ![Yedekleme kasanızın genel bakış sayfasında'yi tıklatın.](./media/backup-file-shares/overview-backup-page.png)

2. İçinde **yedekleme hedefi** menüsünde, gelen **neleri yedeklemek istiyorsunuz?** , Azure dosya paylaşımını seçin.

    ![Yedekleme hedefi olarak Azure Dosya Paylaşımı'nı seçin](./media/backup-file-shares/choose-azure-fileshare-from-backup-goal.png)

3. Azure dosya paylaşımınızı Kurtarma Hizmetleri kasasına yapılandırmak için **Yedekle**'ye tıklayın.

   ![Azure dosya paylaşımını kasayla ilişkilendirmek için Yedekle'ye tıklayın](./media/backup-file-shares/set-backup-goal.png)

    Kasa Azure dosya paylaşımıyla ilişkili sonra yedekleme menüsü açılır ve bir depolama hesabı seçmenizi ister. Menü, zaten bir kurtarma Hizmetleri kasası ile ilişkili olmayan kasanızın bulunduğu bölgede desteklenen tüm depolama hesapları görüntülenir.

   ![Depolama hesabınızı seçin](./media/backup-file-shares/list-of-storage-accounts.png)

4. Depolama hesapları listesinde bir hesap seçin ve **Tamam**'a tıklayın. Azure, depolama hesabında yedeklenebilecek dosya paylaşımlarını arar. Dosya paylaşımlarınızı kısa süre önce eklediyseniz ve listede görmüyorsanız, dosya paylaşımlarının gösterilmesi için biraz zaman tanıyın.

   ![Dosya paylaşımları bulundu](./media/backup-file-shares/discover-file-shares.png)

5. Gelen **dosya paylaşımları** listesinde, bir veya daha fazla yedekleme ve istediğiniz dosya paylaşımlarını seçin **Tamam**.

6. Dosya Paylaşımlarınızı seçtikten sonra, Yedekle menüsü **Yedekleme ilkesi**'ne dönüşür. Bu menüde mevcut yedekleme ilkelerinden birini seçin veya yeni ilke oluşturun ve ardından **Yedeklemeyi Etkinleştir**'e tıklayın.

   ![Bir yedekleme ilkesi seçin veya yeni bir tane oluşturun](./media/backup-file-shares/apply-backup-policy.png)

    Yedekleme ilkesi oluşturulduktan sonra, planlanan zamanda Dosya Paylaşımlarının anlık görüntüsü alınır ve seçilen süre için kurtarma noktası korunur.

## <a name="create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturma
Yedekleme ilkesini yapılandırdıktan sonra kadar sonraki zamanlanmış yedekleme verilerinizin korunmasını sağlamak için bir isteğe bağlı yedekleme oluşturmak isteyebilirsiniz.


### <a name="to-create-an-on-demand-backup"></a>İsteğe bağlı yedekleme oluşturmak için

1. Dosya paylaşımı kurtarma noktalarını içeren Kurtarma Hizmetleri kasasını açın ve **Yedekleme Öğeleri**'ne tıklayın. Yedekleme Öğesi türlerinin listesi gösterilir.

   ![Yedekleme öğeleri listesi](./media/backup-file-shares/list-of-backup-items.png)

2. Listeden **Azure Depolama (Azure Dosyaları)** öğesini seçin. Azure dosya paylaşımlarının listesi görüntülenir.

   ![Azure dosya paylaşımlarının listesi](./media/backup-file-shares/list-of-azure-files-backup-items.png)

3. Azure dosya paylaşımları listesinden istediğiniz dosya paylaşımını seçin. Seçili dosya paylaşımının Yedekleme Öğesi menüsü açılır.

   ![Seçili dosya için yedekleme öğesi menüsü](./media/backup-file-shares/backup-item-menu.png)

4. Yedekleme Öğesi menüsünde **Şimdi Yedekle**'ye tıklayın. Bu isteğe bağlı bir yedekleme işi olduğundan, kurtarma noktasıyla ilişkilendirilmiş bir bekletme ilkesi yoktur. **Şimdi Yedekle** iletişim kutusu açılır. Kurtarma noktasını bekletmek istediğiniz son günü belirtin.

   ![Kurtarma noktası bekletme tarihi seçin](./media/backup-file-shares/backup-now-menu.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalı kullanarak şu işlemleri gerçekleştirdiniz:

> [!div class="checklist"]
> * Azure Dosyalarını yedeklemek için Kurtarma Hizmetleri kasasını yapılandırma
> * Geri yükleme noktası oluşturmak için isteğe bağlı yedekleme işini çalıştırma

Azure dosya paylaşımını yedekten geri yüklemek için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Azure dosya paylaşımlarının yedeğinden geri yükle](./backup-azure-files.md#restore-from-backup-of-azure-file-share)
 
