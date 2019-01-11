---
title: Portalı kullanarak bir Azure SQL veritabanı oluşturma | Microsoft Docs
description: Azure portalında bir Azure SQL veritabanı mantıksal sunucusu ve veritabanı oluşturun ve sorgulayın.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 1/9/2019
ms.openlocfilehash: b11eb08a960e81ab938a9b15a1153c44706231c5
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198296"
---
# <a name="quickstart-create-an-azure-sql-database-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında Azure SQL veritabanı oluşturma

Azure SQL veritabanı, bir *hizmet olarak veritabanı* olanak tanıyan bulutta yüksek oranda kullanılabilir SQL Server veritabanlarını çalıştırıp ölçeklendirmenize. Bu hızlı başlangıçta oluşturmaya ve ardından Azure portalını kullanarak Azure SQL veritabanını sorgulamaya başlayın gösterilmektedir. 

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Bu hızlı Başlangıçta tüm adımları için oturum açın [Azure portalında](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

Bir Azure SQL veritabanı bir dizi tanımlanmış olan [işlem ve depolama kaynaklarını](sql-database-service-tiers-dtu.md). Veritabanında oluşturduğunuz bir [Azure SQL veritabanı mantıksal sunucusu](sql-database-features.md) içinde bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md).

AdventureWorksLT örnek verilerini içeren bir SQL veritabanı oluşturmak için:

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
   
1. Seçin **veritabanları** seçip **SQL veritabanı**.
   
1. İçinde **SQL veritabanı oluşturma** form yazın veya aşağıdaki değerleri seçin: 
   
   - **Veritabanı adı**: Girin *mySampleDatabase*.
   - **Abonelik**: Açılan menü ve görünmüyorsa doğru aboneliği seçin.
   - **Kaynak grubu**: Seçin **Yeni Oluştur**, türü *myResourceGroup*seçip **Tamam**. 
   - **Kaynak Seç**: Açılır listesine tıklayıp **örnek (AdventureWorksLT)**.
    
    >[!IMPORTANT]
    >Seçtiğinizden emin olun **örnek (AdventureWorksLT)** bu ve bu verileri kullanan diğer Azure SQL veritabanı hızlı başlangıçları izleyebilmeniz veri.
  
   ![Azure SQL veritabanı oluşturma](./media/sql-database-get-started-portal/create-database-1.png)
   
1. Altında **sunucu**seçin **Yeni Oluştur**. 
   
1. İçinde **yeni sunucu** form yazın veya aşağıdaki değerleri seçin: 
   
   - **Sunucu adı**: Girin *mysqlserver*.
   - **Sunucu Yöneticisi oturum açma**: Tür *azureuser*. 
   - **Parola**: Girin *Azure1234567*. 
   - **Parolayı onaylayın**: Parolayı yeniden yazın.
   - **Konum**: Açılan menü ve herhangi bir geçerli konumu seçin.  
   
   >[!IMPORTANT]
   >Sunucu Yöneticisi oturum açma ve parola, sunucu ve veritabanları için bu ve diğer hızlı başlangıçlar oturum açabilmek kaydetmeyi unutmayın. Oturum açma veya parolayı unutursanız, oturum açma adı veya parola sıfırlamasına **SQL server** sayfası. Açmak için **SQL server** sayfasında, veritabanı sunucu adını seçin **genel bakış** veritabanı oluşturulduktan sonra sayfa.
   
    ![Sunucu oluşturma](./media/sql-database-get-started-portal/create-database-server.png)

1. **Seç**’i seçin.
   
1. Üzerinde **SQL veritabanı** form, select **fiyatlandırma katmanı**. Dtu ve her hizmet katmanı için kullanılabilir depolama miktarını keşfedin.
   
   >[!NOTE]
   >Bu hızlı başlangıçta kullanılmaktadır [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md), ancak [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) de kullanılabilir.
   
   >[!NOTE]
   >1 TB'den fazla depolama Premium katmanında şu anda tüm bölgelerde kullanılabilir: UK Kuzey, Batı Orta ABD, Birleşik Krallık South2, Çin Doğu, USDoDCentral, Almanya Orta, USDoDEast, ABD Devleti Southwest, ABD Devleti Güney Orta, Almanya Kuzeydoğu, Çin Kuzey ve ABD Devleti Doğu. Bu bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Daha fazla bilgi için [P11 P15 geçerli sınırlamalar](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   
1. Bu hızlı başlangıçta seçin **standart** Hizmet katmanını ve seçip kaydırıcıyı kullanarak **10 Dtu (S0)** ve **1** GB depolama alanı.
   
1. **Uygula**’yı seçin.  
   
   ![Fiyatlandırma seçin](./media/sql-database-get-started-portal/create-database-s1.png)
   
1. Üzerinde **SQL veritabanı** form, select **Oluştur** dağıtma ve kaynak grubu, sunucu ve veritabanı sağlama. 
   
   Dağıtım birkaç dakika sürer. Seçebileceğiniz **bildirimleri** dağıtım ilerlemesini izlemek için araç çubuğunda.

   ![Bildirim](./media/sql-database-get-started-portal/notification.png)

## <a name="query-the-sql-database"></a>SQL veritabanını sorgulama

Yerleşik sorgu aracını Azure portalında bir Azure SQL veritabanı oluşturduğunuza göre veritabanına bağlanmak ve verileri sorgulamak için kullanın.

1. Üzerinde **SQL veritabanı** seçin, veritabanı için sayfa **sorgu Düzenleyicisi (Önizleme)** soldaki menüde. 
   
   ![Sorgu Düzenleyicisi oturum açın](./media/sql-database-get-started-portal/query-editor-login.png)
   
1. Oturum açma bilgilerinizi girin ve seçin **Tamam**.
   
1. Aşağıdaki sorguyu girin **sorgu Düzenleyicisi** bölmesi.
   
   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```
   
1. Seçin **çalıştırma**ve ardından sorgu sonuçlarını gözden **sonuçları** bölmesi.

   ![Sorgu Düzenleyicisi sonuçları](./media/sql-database-get-started-portal/query-editor-results.png)
   
1. Kapat **sorgu Düzenleyicisi** sayfasında ve seçin **Tamam** kaydedilmemiş düzenlemelerinizi iptal etmek isteyip istemediğiniz sorulduğunda.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynak grubu, SQL server ve SQL veritabanı için gitmek isterseniz tutmak [sonraki adımlar](#next-steps). Sonraki adımlar bağlanın ve farklı yöntemler kullanarak veritabanını sorgulama işlemini göstermektedir. 

Bu kaynakları kullanarak tamamladığınızda, aşağıda gösterildiği gibi silebilirsiniz:

1. Azure portalında sol menüden seçim yapın **kaynak grupları**ve ardından **myResourceGroup**.
1. Kaynak grubu sayfanızda seçin **kaynak grubunu Sil**. 
1. Girin *myResourceGroup* alan ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

- Şirket içi veya uzak Araçlar, Azure SQL veritabanınıza bağlanmak için sunucu düzeyinde güvenlik duvarı kuralı oluşturun. Daha fazla bilgi için [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-get-started-portal-firewall.md).
- Bir sunucu düzeyinde güvenlik duvarı kuralı oluşturduktan sonra [bağlanma ve sorgulama](sql-database-connect-query.md) birkaç farklı araçları ve dilleri kullanarak veritabanınızı. 
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Azure CLI kullanarak Azure SQL veritabanı oluşturmak için bkz [Azure CLI örnekleri](sql-database-cli-samples.md).
- Azure PowerShell kullanarak Azure SQL veritabanı oluşturmak için bkz [Azure PowerShell örnekleri](sql-database-powershell-samples.md).
