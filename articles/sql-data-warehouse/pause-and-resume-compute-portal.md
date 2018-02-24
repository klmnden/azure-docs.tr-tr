---
title: "Hızlı Başlangıç: Azure SQL Data Warehouse'da - Azure portalında duraklatma ve sürdürme işlem | Microsoft Docs"
description: "Bir Azure SQL veri ambarı'nın maliyet tasarrufu sağlamak için işlem duraklatmak azure portal görevler. Veri ambarı kullanılmaya hazır olduğunuzda sürdürebilirsiniz."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/23/2018
ms.author: barbkess
ms.openlocfilehash: 30dede32b35f995f89e2946af34da10353f55212
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında Azure SQL Data Warehouse için duraklatma ve sürdürme işlem
Bir Azure SQL veri ambarı'nın maliyet tasarrufu sağlamak için Duraklat işlem. [İşlem devam](sql-data-warehouse-manage-compute-overview.md) veri ambarı kullanılmaya hazır olduğunuzda.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Kullanım [oluşturma ve Connect - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Duraklatma işlem
Maliyet tasarrufu sağlamak duraklatma ve sürdürme işlem kaynakları isteğe bağlı. Örneğin, veritabanı gece sırasında ve hafta sonları kullandığınız olmaz, bu saatlerde duraklatma ve gün boyunca sürdürün. Veritabanı duraklatıldığında işlem kaynaklarını tahsil olmaz. Ancak, depolama için sizden ücret devam eder. 

SQL data warehouse duraklatmak için aşağıdaki adımları izleyin.

1. Tıklatın **SQL veritabanları** Azure portalının sol sayfasındaki.
2. Seçin **mySampleDataWarehouse** gelen **SQL veritabanları** sayfası. Bu, veri ambarı açar. 
3. Üzerinde **mySampleDataWarehouse** sayfasında, fark **durum** olan **çevrimiçi**.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

4. Veri ambarı duraklatmak için tıklatın **duraklatma** düğmesi. 
5. Devam etmek istiyorsanız onay soru soran görüntülenir. **Evet**'e tıklayın.
6. Birkaç dakika bekleyin ve ardından fark **durum** olan **duraklatma**.

    ![Duraklatılıyor](media/pause-and-resume-compute-portal/pausing.png)

7. Duraklatma işlemi tamamlandığında durumudur **duraklatıldı** ve seçenek düğmesi **Başlat**.
8. Veri ambarı için işlem kaynaklarını çevrimdışı. Hizmet yeniden başlatana kadar işlem için sizden ücret olmaz.

    ![Çevrimdışı işlem](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Resume işlem
SQL data warehouse sürdürmek için aşağıdaki adımları izleyin.

1. Tıklatın **SQL veritabanları** Azure portalının sol sayfasındaki.
2. Seçin **mySampleDataWarehouse** gelen **SQL veritabanları** sayfası. Bu, veri ambarı açar. 
3. Üzerinde **mySampleDataWarehouse** sayfasında, fark **durum** olan **duraklatıldı**.

    ![Çevrimdışı işlem](media/pause-and-resume-compute-portal/compute-offline.png)

4. Veri ambarı devam etmek için tıklatın **Başlat**. 
5. Başlatmak istiyorsanız onay soru soran görüntülenir. **Evet**'e tıklayın.
6. Bildirim **durum** olan **sürdürülüyor**.

    ![Sürdürülüyor](media/pause-and-resume-compute-portal/resuming.png)

7. Veri ambarı yeniden çevrimiçi olduğunda durumudur **çevrimiçi** ve seçenek düğmesi **duraklatma**.
8. Veri ambarı için işlem kaynakları artık çevrimiçi olduğundan ve hizmetini kullanabilirsiniz. İşlem ücretleri devam ettirildi.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve veri ambarında depolanan veri için ücret. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır. 

- Veri deposunda tutmak istiyorsanız, işlem duraklatın.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz. 

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. Oturum [Azure portal](https://portal.azure.com)ve veri ambarınız'ı tıklatın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için **Başlat**’a tıklayın.

2. İşlem veya depolama için ücretlendirilmemek üzere veri ambarını kaldırmak için **Sil**’e tıklayın.

3. Oluşturduğunuz SQL server kaldırmak için tıklatın **mynewserver 20171113.database.windows.net**ve ardından **silmek**.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

4. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Şimdi duraklatıldı sahip ve işlem için veri ambarınız sürdürüldü. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
