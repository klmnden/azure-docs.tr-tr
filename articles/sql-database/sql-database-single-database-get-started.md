---
title: 'Azure portalı: Tek veritabanı - Azure SQL veritabanı oluşturma | Microsoft Docs'
description: Oluşturun ve Azure portalını kullanarak Azure SQL veritabanı'nda tek bir veritabanını sorgulama.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 02/25/2019
ms.openlocfilehash: 5aeb84e5086fb0cf5c30e175ad419ee70bed55ad
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58075194"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure SQL veritabanı tek veritabanı oluşturma

Oluşturma bir [tek veritabanı](sql-database-single-database.md) Azure SQL veritabanında bir veritabanı oluşturmak için hızlı ve kolay bir dağıtım seçeneğidir. Bu hızlı başlangıçta oluşturma ve ardından Azure portalını kullanarak tek bir veritabanını sorgulama gösterilmektedir.

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Bu hızlı Başlangıçta tüm adımları için oturum açın [Azure portalında](https://portal.azure.com/).

## <a name="create-a-single-database"></a>Tek veritabanı oluşturma

Bir dizi işlem, bellek, GÇ ve depolama kaynakları iki birini kullanarak tek bir veritabanı olan [satın alma modeli](sql-database-purchase-models.md). Tek bir veritabanı oluşturduğunuzda, aynı zamanda tanımlamış bir [SQL veritabanı sunucusu](sql-database-servers.md) yönetip içine yerleştirdiğiniz [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) belirli bir bölgede.

AdventureWorksLT örnek verilerini içeren tek bir veritabanı oluşturmak için:

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
2. Seçin **veritabanları** seçip **SQL veritabanı**.
3. İçinde **SQL veritabanı oluşturma** form yazın veya aşağıdaki değerleri seçin:

   - **Veritabanı adı**: Girin *mySampleDatabase*.
   - **Abonelik**: Açılan menü ve görünmüyorsa doğru aboneliği seçin.
   - **Kaynak grubu**: Seçin **Yeni Oluştur**, türü *myResourceGroup*seçip **Tamam**.
   - **Kaynak Seç**: Açılır listesine tıklayıp **örnek (AdventureWorksLT)**.

     > [!IMPORTANT]
     > Seçtiğinizden emin olun **örnek (AdventureWorksLT)** bu ve bu verileri kullanan diğer Azure SQL veritabanı hızlı başlangıçları kolayca izleyebilmeniz veri.
  
   ![Tek veritabanı oluşturma](./media/sql-database-get-started-portal/create-database-1.png)

4. Altında **sunucu**seçin **Yeni Oluştur**.
5. İçinde **yeni sunucu** form yazın veya aşağıdaki değerleri seçin:

   - **Sunucu adı**: Girin *mysqlserver*.
   - **Sunucu Yöneticisi oturum açma**: Tür *azureuser*.
   - **Parola**: Girin *Azure1234567*.
   - **Parolayı onaylayın**: Parolayı yeniden yazın.
   - **Konum**: Açılan menü ve herhangi bir geçerli konumu seçin.  

   > [!IMPORTANT]
   > Sunucu Yöneticisi oturum açma ve parola, sunucu ve veritabanları için bu ve diğer hızlı başlangıçlar oturum açabilmek kaydetmeyi unutmayın. Oturum açma veya parolayı unutursanız, oturum açma adı veya parola sıfırlamasına **SQL server** sayfası. Açmak için **SQL server** sayfasında, veritabanı sunucu adını seçin **genel bakış** veritabanı oluşturulduktan sonra sayfa.

    ![Sunucu oluşturma](./media/sql-database-get-started-portal/create-database-server.png)

6. **Seç**’i seçin.
7. Üzerinde **SQL veritabanı** form, select **fiyatlandırma katmanı**. Dtu ve her hizmet katmanı için kullanılabilir depolama miktarını keşfedin.

   > [!NOTE]
   > Bu hızlı başlangıçta kullanılmaktadır [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md), ancak [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) de kullanılabilir.
   > [!IMPORTANT]
   > 1 TB'den fazla depolama Premium katmanında şu anda tüm bölgelerde kullanılabilir: Çin Doğu, Kuzey Çin, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, US DoD bölgeler ve ABD kamu orta. Bu bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır.  Daha fazla bilgi için [P11 P15 geçerli sınırlamalar](sql-database-single-database-scale.md#dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

8. Bu hızlı başlangıçta seçin **standart** Hizmet katmanını ve seçip kaydırıcıyı kullanarak **10 Dtu (S0)** ve **1** GB depolama alanı.
9. **Uygula**’yı seçin.  

   ![Fiyatlandırma seçin](./media/sql-database-get-started-portal/create-database-s1.png)

10. Üzerinde **SQL veritabanı** form, select **Oluştur** dağıtma ve kaynak grubu, sunucu ve veritabanı sağlama.

    Dağıtım birkaç dakika sürer. Seçebileceğiniz **bildirimleri** dağıtım ilerlemesini izlemek için araç çubuğunda.

    ![Bildirim](./media/sql-database-get-started-portal/notification.png)

## <a name="query-the-database"></a>Veritabanını sorgulama

Yerleşik sorgu aracını Azure portalında bir veritabanı oluşturduğunuza göre veritabanına bağlanmak ve verileri sorgulamak için kullanın.

1. Üzerinde **SQL veritabanı** seçin, veritabanı için sayfa **sorgu Düzenleyicisi (Önizleme)** soldaki menüde.

   ![Sorgu Düzenleyicisi oturum açın](./media/sql-database-get-started-portal/query-editor-login.png)

2. Oturum açma bilgilerinizi girin ve seçin **Tamam**.
3. Aşağıdaki sorguyu girin **sorgu Düzenleyicisi** bölmesi.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. Seçin **çalıştırma**ve ardından sorgu sonuçlarını gözden **sonuçları** bölmesi.

   ![Sorgu Düzenleyicisi sonuçları](./media/sql-database-get-started-portal/query-editor-results.png)

5. Kapat **sorgu Düzenleyicisi** sayfasında ve seçin **Tamam** kaydedilmemiş düzenlemelerinizi iptal etmek isteyip istemediğiniz sorulduğunda.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynak grubu, veritabanı sunucusu ve tek veritabanı için gitmek isterseniz tutmak [sonraki adımlar](#next-steps). Sonraki adımlar bağlanın ve farklı yöntemler kullanarak veritabanını sorgulama işlemini göstermektedir.

Bu kaynakları kullanarak tamamladığınızda, aşağıda gösterildiği gibi silebilirsiniz:

1. Azure portalında sol menüden seçim yapın **kaynak grupları**ve ardından **myResourceGroup**.
2. Kaynak grubu sayfanızda seçin **kaynak grubunu Sil**.
3. Girin *myResourceGroup* alan ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

- Şirket içi veya uzak Araçlar tek bir veritabanına bağlanmak için sunucu düzeyinde güvenlik duvarı kuralı oluşturun. Daha fazla bilgi için [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-server-level-firewall-rule.md).
- Bir sunucu düzeyinde güvenlik duvarı kuralı oluşturduktan sonra [bağlanma ve sorgulama](sql-database-connect-query.md) birkaç farklı araçları ve dilleri kullanarak veritabanınızı.
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Azure CLI kullanarak tek bir veritabanı oluşturmak için bkz [Azure CLI örnekleri](sql-database-cli-samples.md).
- Azure PowerShell kullanarak tek bir veritabanı oluşturmak için bkz [Azure PowerShell örnekleri](sql-database-powershell-samples.md).
