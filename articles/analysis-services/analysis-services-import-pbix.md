---
title: Azure Analysis Services'e Power BI Desktop dosyasını içeri aktarma | Microsoft Docs
description: Azure portalını kullanarak bir Power BI Desktop dosyası (pbix) içeri aktarma açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a2855ca5dbb76d3fcc30c4b1007c20bb48c91c9b
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42061108"
---
# <a name="import-a-power-bi-desktop-file"></a>Power BI Desktop dosyasını içeri aktarın

Azure Analysis Services'e Power BI Desktop dosyası (pbıx) veri modelinde içeri aktarabilirsiniz. Model meta verileri, önbelleğe alınmış verileri ve veri kaynağı bağlantıları içeri aktarılır. Raporlar ve görselleştirmeler içeri aktarılmaz. Power BI Desktop'tan modelleri kullanarak 1400 uyumluluk düzeyinde veri alma işlemi.

**Kısıtlamaları**   

- Olan portalında kullanan web Tasarımcısı özelliğiyle bir pbıx dosyasından içeri aktarma **Önizleme**. İşlevselliği sınırlıdır. Daha gelişmiş model geliştirme ve test için Visual Studio (SSDT) ve SQL Server Management Studio (SSMS) en iyisidir.
- Bir pbıx dosyasından içeri aktarmak için sunucu yöneticisi izinleri olmalıdır.
- Pbıx modelin bağlanıp **Azure SQL veritabanı** ve **Azure SQL veri ambarı** veri kaynakları yalnızca.
- Pbıx modeli Canlı olamaz veya DirectQuery bağlantıları. 
- Pbıx veri modelinizi Analysis Services'de desteklenmeyen meta veri içeriyorsa, içeri aktarma işlemi başarısız.


## <a name="to-import-from-pbix"></a>Pbıx dosyasını içeri aktarmak için

1. Sunucunuzun içinde **genel bakış** > **Web Tasarımcısı**, tıklayın **açık**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. İçinde **Web Tasarımcısı** > **modelleri**, tıklayın **+ Ekle**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. İçinde **yeni modeli**, model adını yazın ve ardından Power BI Desktop dosyasını seçin.

    ![Azure portalında yeni model iletişim kutusu](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. İçinde **alma**bulup dosyanızı seçin.

     ![Azure portalında iletişim bağlanma](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="change-credentials"></a>Kimlik bilgilerini değiştirme

Varsayılan olarak bir veri modeli bir pbıx dosyasından içeri aktardığınızda bir veri kaynağına bağlanmak için kullanılan kimlik bilgilerini ServiceAccount için ayarlanır. Bir model bir pbıx dosyasını içeri aktarıldıktan sonra aşağıdaki yöntemleri kullanarak kimlik bilgilerini değiştirebilirsiniz:

- Temmuz 2018'den (sürüm 17.8.1) kullanın veya sonraki kimlik bilgilerini düzenlemek için SSMS sürümü. Bu, en kolay yoludur.
- TMSL kullanın [Alter komutu](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl) üzerinde [veri kaynakları nesne](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl) bağlantı dizesi özelliğini değiştirmek için. 
- Visual Studio'da model açın, veri kaynağı bağlantısı için kimlik bilgilerini düzenle ve sonra modeli yeniden dağıtın.

SSMS kullanarak kimlik bilgilerini değiştirmek için. 

1. SSMS'de, veritabanını genişletin > **bağlantıları**. 
2. Veritabanı bağlantısına sağ tıklayın ve ardından **kimlik bilgileri Yenile**. 

    ![Kimlik bilgilerini Yenile](./media/analysis-services-import-pbix/aas-import-pbix-creds.png)

3. Kimlik bilgileri iletişim kutusunda kimlik bilgisi türü seçin ve kimlik bilgilerini girin. SQL kimlik doğrulaması için veritabanını seçin. Kuruluş hesabı (OAuth) için Microsoft hesabı seçin.
    ![Kimlik bilgilerini Düzenle](./media/analysis-services-import-pbix/aas-import-pbix-edit-creds.png)

Power BI Desktop'ın Temmuz 2018'den sürüm veri kaynağı izinleri değiştirmek için yeni bir özellik içerir. Üzerinde **giriş** sekmesinde **sorguları Düzenle**  > **veri kaynağı ayarları**. Veri kaynağı bağlantısı seçin ve ardından **düzenleme izinleri**.


## <a name="see-also"></a>Ayrıca bkz.

[Azure portalında bir model oluşturma](analysis-services-create-model-portal.md)   
[Azure Analysis Services'a bağlanın](analysis-services-connect.md)  
