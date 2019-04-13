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
ms.author: ninarn
ms.reviewer: carlrab
manager: craigg
ms.date: 04/11/2019
ms.openlocfilehash: a5fbc58feea8779ba8a7a61dfc89158e20bd2c92
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59544305"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalını kullanarak Azure SQL veritabanı tek veritabanı oluşturma

Oluşturma bir [tek veritabanı](sql-database-single-database.md) Azure SQL veritabanında bir veritabanı oluşturmak için hızlı ve kolay bir dağıtım seçeneğidir. Bu hızlı başlangıçta oluşturma ve ardından Azure portalını kullanarak tek bir veritabanını sorgulama gösterilmektedir.

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Bu hızlı Başlangıçta tüm adımları için oturum açın [Azure portalında](https://portal.azure.com/).

## <a name="create-a-single-database"></a>Tek veritabanı oluşturma

Bir dizi işlem, bellek, GÇ ve depolama kaynakları iki birini kullanarak tek bir veritabanı olan [satın alma modeli](sql-database-purchase-models.md). Tek bir veritabanı oluşturduğunuzda, aynı zamanda tanımlamış bir [SQL veritabanı sunucusu](sql-database-servers.md) yönetip içine yerleştirdiğiniz [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) belirli bir bölgede.

AdventureWorksLT örnek verilerini içeren tek bir veritabanı oluşturmak için:

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
2. Seçin **veritabanları** seçip **SQL veritabanı** açmak için **SQL veritabanı oluşturma** sayfası. 

   ![Tek veritabanı oluşturma](./media/sql-database-get-started-portal/create-database-1.png)

1. Üzerinde **Temelleri** sekmesinde **Project Details** bölümüne yazın veya aşağıdaki değerleri seçin:

   - **Abonelik**: Açılan menü ve görünmüyorsa doğru aboneliği seçin.
   - **Kaynak grubu**: Seçin **Yeni Oluştur**, türü `myResourceGroup`seçip **Tamam**.

   ![Yeni SQL veritabanı - temel sekmesi](media/sql-database-get-started-portal/new-sql-database-basics.png)


1. İçinde **veritabanı ayrıntıları** bölümüne yazın veya aşağıdaki değerleri seçin: 

   - **Veritabanı adı**: `mySampleDatabase` yazın.
   - **Sunucu**: Seçin **Yeni Oluştur** ve aşağıdaki değerleri girin ve ardından **seçin**. 
       - **Sunucu adı**: Tür `mysqlserver`; bazı sayılar için benzersizlik yanı sıra. 
       - **Sunucu Yöneticisi oturum açma**: `azureuser` yazın.
       - **Parola**: Parola gereksinimlerini karşılayan bir karmaşık bir parola yazın. 
       - **Konum**: Açılan listeden, aşağıdaki gibi bir konum seçin `West US 2`. 

       ![Yeni sunucu](media/sql-database-get-started-portal/new-server.png)

        > [!IMPORTANT]
        > Sunucu Yöneticisi oturum açma ve parola, sunucu ve veritabanları için bu ve diğer hızlı başlangıçlar oturum açabilmek kaydetmeyi unutmayın. Oturum açma veya parolayı unutursanız, oturum açma adı veya parola sıfırlamasına **SQL server** sayfası. Açmak için **SQL server** sayfasında, veritabanı sunucu adını seçin **genel bakış** veritabanı oluşturulduktan sonra sayfa.

      ![SQL veritabanı ayrıntıları](media/sql-database-get-started-portal/sql-db-basic-db-details.png)

   - **SQL esnek havuzu kullanmak istediğiniz**: Seçin **Hayır** seçeneği. 
   - **İşlem ve depolama**: Seçin **yapılandırma veritabanı** ve bu hızlı başlangıçta **standart** Hizmet katmanını ve seçip kaydırıcıyı kullanarak **10 Dtu (S0)** ve **1** Depolama GB. **Uygula**’yı seçin. 

    ![Katmanını yapılandırma](media/sql-database-get-started-portal/create-database-s1.png) 


      > [!NOTE]
      > Bu hızlı başlangıçta kullanılmaktadır [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md), ancak [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) de kullanılabilir.
      > [!IMPORTANT]
      > 1 TB'den fazla depolama Premium katmanında şu anda tüm bölgelerde kullanılabilir: Çin Doğu, Kuzey Çin, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, US DoD bölgeler ve ABD kamu orta. Bu bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır.  Daha fazla bilgi için [P11 P15 geçerli sınırlamalar](sql-database-single-database-scale.md#dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

    



1. Seçin **ek ayarlar** sekmesi. 
1. İçinde **veri kaynağı** bölümündeki **mevcut verilerden yararlanabilirsiniz**seçin `Sample`. 

   ![Ek SQL veritabanı ayarları](media/sql-database-get-started-portal/create-sql-database-additional-settings.png)

   > [!IMPORTANT]
   > Seçtiğinizden emin olun **örnek (AdventureWorksLT)** bu ve bu verileri kullanan diğer Azure SQL veritabanı hızlı başlangıçları kolayca izleyebilmeniz veri.

1. Geri kalan değerler varsayılan ve select bırakın **gözden geçir + Oluştur** formun alt kısmındaki. 
1. Son ayarları gözden geçirin ve seçin **Oluştur**. 

8. Üzerinde **SQL veritabanı** form, select **Oluştur** dağıtma ve kaynak grubu, sunucu ve veritabanı sağlama.


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
