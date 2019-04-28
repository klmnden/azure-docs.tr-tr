---
title: "Hızlı Başlangıç: Duraklatma ve sürdürme işlem Azure SQL veri ambarı'nda - Azure portalı | Microsoft Docs"
description: Duraklatma işlem Azure portalında Azure SQL veri ambarı'nda maliyetlerden tasarruf etmek için kullanın. Veri ambarı kullanılmaya hazır olduğunda sürdürebilirsiniz.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 04/18/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9c3ed6dd79d6225b38751c910253cfa1f0720d1c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61475623"
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında bir Azure SQL veri ambarı için işlem duraklatma ve sürdürme

Duraklatma işlem Azure portalında Azure SQL veri ambarı'nda maliyetlerden tasarruf etmek için kullanın. [İşlem devam](sql-data-warehouse-manage-compute-overview.md) veri ambarı kullanılmaya hazır olduğunuzda.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

Kullanım [oluşturma ve bağlanma - portal](create-data-warehouse-portal.md) adlı bir veri ambarı oluşturmak için **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Duraklatma işlem

Maliyetlerden tasarruf etmek için duraklatma ve sürdürme işlem kaynaklarını isteğe bağlı. Örneğin, veritabanı sırasında gece ve hafta sonları kullanmanız gerekmez, bu saatlerde duraklatabilir ve gün boyunca devam. Veritabanı durdurulduğunda bilgi işlem kaynakları için ücret ödemezsiniz. Ancak, depolama için ücret ödemeye devam eder. 

SQL veri ambarını duraklatmak için bu adımları izleyin.

1. Azure portalının sol taraftaki sayfasında **SQL veritabanları**’na tıklayın.
2. **SQL veritabanları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Bu, veri ambarını açar. 
3. Üzerinde **mySampleDataWarehouse** sayfasında, fark **durumu** olduğu **çevrimiçi**.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

4. Veri ambarını duraklatmak için tıklatın **duraklatmak** düğmesi. 
5. Devam etmek istiyorsanız onay soru soran görüntülenir. **Evet**'e tıklayın.
6. Birkaç dakika bekleyin ve ardından dikkat edin **durumu** olduğu **duraklatma**.

    ![Duraklatılıyor](media/pause-and-resume-compute-portal/pausing.png)

7. Duraklatma işlemi tamamlandığında durumu ise **duraklatıldı** ve seçenek düğmesini **Başlat**.
8. İşlem kaynakları veri ambarı için çevrimdışı. Hizmet yeniden başlatana kadar işlem için ücret ödemezsiniz.

    ![İşlem çevrimdışı](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>İşlem devam et

SQL veri ambarı sürdürmek için aşağıdaki adımları izleyin.

1. Azure portalının sol taraftaki sayfasında **SQL veritabanları**’na tıklayın.
2. **SQL veritabanları** sayfasından **mySampleDataWarehouse** seçeneğini belirleyin. Bu, veri ambarını açar. 
3. Üzerinde **mySampleDataWarehouse** sayfasında, fark **durumu** olduğu **duraklatıldı**.

    ![İşlem çevrimdışı](media/pause-and-resume-compute-portal/compute-offline.png)

4. Veri ambarı sürdürmek için tıklayın **Başlat**. 
5. Başlamak istiyorsanız onay soru soran görüntülenir. **Evet**'e tıklayın.
6. Bildirim **durumu** olduğu **sürdürülüyor**.

    ![Sürdürülüyor](media/pause-and-resume-compute-portal/resuming.png)

7. Veri ambarını yeniden çevrimiçi olduğunda durumudur **çevrimiçi** ve seçenek düğmesini **duraklatma**.
8. Veri ambarı işlem kaynaklarını çevrimiçi olduğundan ve hizmeti kullanabilirsiniz. İşlem ücretleri devam ettirildi.

    ![Çevrimiçi işlem](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veri ambarı birimleri ve veri Ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır. 

- Verileri depoda tutmak istiyorsanız, duraklatabilirsiniz.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz. 

Kaynakları istediğiniz gibi temizlemek için bu adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com)ve veri ambarınıza tıklayın.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. İşlemi duraklatmak için, **Duraklat** düğmesine tıklayın. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için **Başlat**’a tıklayın.

2. İşlem veya depolama için ücretlendirilmemek üzere veri ambarını kaldırmak için **Sil**’e tıklayın.

3. Oluşturduğunuz SQL sunucusunu kaldırmak için tıklayın **mynewserver-20171113.database.windows.net**ve ardından **Sil**.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

4. Kaynak grubunu kaldırmak için, **myResourceGroup**’a tıklayıp daha sonra **Kaynak grubunu sil**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Artık duraklatıldı ve veri ambarınıza yönelik işlem sürdürülüyor. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
