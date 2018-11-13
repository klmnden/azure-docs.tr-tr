---
title: 'Azure portalı: SQL veritabanı oluşturma | Microsoft Docs'
description: Azure portalında SQL Veritabanı mantıksal sunucusu, sunucu düzeyi güvenlik duvarı kuralı ve veritabanı oluşturun ve sorgulayın.
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
ms.date: 11/01/2018
ms.openlocfilehash: 66ee4ac8fe946696d6760891a086a672fa9fc412
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914610"
---
# <a name="quickstart-create-an-azure-sql-database-in-the-azure-portal"></a>Hızlı Başlangıç: Azure portalında Azure SQL veritabanı oluşturma

Bu hızlı başlangıçta, [DTU tabanlı satın alma modelini](sql-database-service-tiers-dtu.md) kullanarak Azure’da SQL veritabanı oluşturma adımları gösterilmektedir. Azure SQL Veritabanı, bulutta yüksek oranda kullanılabilir SQL Server veritabanlarını çalıştırıp ölçeklendirmenize olanak tanıyan bir “Hizmet Olarak Veritabanı” teklifidir. Bu hızlı başlangıçta Azure portalı kullanarak bir SQL veritabanı oluşturma ve ardından veritabanını sorgulama gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

  >[!NOTE]
  >Bu öğretici DTU tabanlı satın alma modelini kullanır ancak [sanal çekirdek tabanlı satın alma modeli ](sql-database-service-tiers-vcore.md) de kullanılabilir.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

Azure SQL veritabanı bir dizi [işlem ve depolama kaynağı](sql-database-service-tiers-dtu.md) ile oluşturulur. Veritabanı bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL Veritabanı mantıksal sunucusu](sql-database-features.md) içinde oluşturulur.

Adventure Works LT örnek verilerini içeren bir SQL veritabanı oluşturmak için bu adımları izleyin.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **Yeni** penceresinden **Veritabanları**’nı seçin ve **Yeni** sayfasında **SQL Veritabanı** altından **Oluştur**’u seçin.

   ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

3. SQL Veritabanı formunu, önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle doldurun:   

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Veritabanı adı** | mySampleDatabase | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). |
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu**  | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Kaynak seçme** | Örnek (AdventureWorksLT) | AdventureWorksLT şemasını ve verilerini yeni veritabanınıza yükler |

   > [!IMPORTANT]
   > Bu hızlı başlangıcın geri kalanında kullanılacağı için bu formdaki örnek veritabanını seçmeniz gerekir.
   >

4. **Sunucu** altında **Gerekli ayarları yapılandır**’a tıklayıp, aşağıdaki görüntüde gösterildiği gibi SQL sunucusu (mantıksal sunucu) formunu aşağıdaki bilgilerle doldurun:   

   | Ayar       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). |
   | **Parola** | Geçerli bir parola | Parolanızda en az 8 karakter bulunmalı ve parolanız şu üç kategoriden karakterler içermelidir: büyük harf karakterler, küçük harf karakterler, sayılar ve alfasayısal olmayan karakterler. |
   | **Abonelik** | Aboneliğiniz | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

   > [!IMPORTANT]
   > Burada belirttiğiniz sunucu yöneticisi kullanıcı adı ve parolası, bu hızlı başlangıcın sonraki bölümlerinde sunucuda ve veritabanlarında oturum açmak için gereklidir. Bu bilgileri daha sonra kullanmak üzere aklınızda tutun veya kaydedin.
   >  

   ![create database-server](./media/sql-database-get-started-portal/create-database-server.png)

5. Formu tamamladığınızda **Seç**’e tıklayın.

6. Hizmet katmanını, DTU'ların sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Her hizmet katmanı için kullanılabilir DTU'lar ve depolama alanı miktarı seçeneklerini araştırın.

   > [!IMPORTANT]
   > Premium katmanında 1 TB’tan fazla depolama alanı, şu an için şu bölgeler dışındaki tüm bölgelerde kullanılabilir: UK Kuzey, Orta Batı ABD, UK Güney 2, Çin Doğu, USDoDCentral, Almanya Orta, USDoDEast, US Gov Güneybatı, US Gov Orta Güney, Almanya Kuzeydoğu, Çin Kuzey, US Gov Doğu. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

7. Bu hızlı başlangıç için **Standart** hizmet katmanını seçip kaydırıcıyı kullanarak **10 DTU (S0)** ve **1** GB depolama alanını seçin.

   ![create database-s1](./media/sql-database-get-started-portal/create-database-s1.png)

8. **Ek Depolama** seçeneğini kullanmak için önizleme koşullarını kabul edin.

   > [!IMPORTANT]
   > Premium katmanında 1 TB’tan fazla depolama alanı, şu an için şu bölgeler dışındaki tüm bölgelerde kullanılabilir: Orta Batı ABD, Çin Doğu, USDoDCentral, USGov Iowa, Almanya Orta, USDoDEast, US Gov Güneybatı, Almanya Kuzeydoğu, Çin Kuzey. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

9. Sunucu katmanını, DTU'ların sayısını ve depolama alanı miktarını seçtikten sonra **Uygula**’ya tıklayın.  

10. SQL Veritabanı formunu tamamladıktan sonra veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama birkaç dakika sürer.

11. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

     ![bildirim](./media/sql-database-get-started-portal/notification.png)

## <a name="query-the-sql-database"></a>SQL veritabanını sorgulama

Azure’da bir örnek veritabanı oluşturduktan sonra, veritabanına bağlanabildiğinizi ve verileri sorgulayabildiğinizi onaylamak üzere Azure portalındaki yerleşik sorgu aracını kullanın.

1. Veritabanınız için SQL Veritabanı sayfasında, sol taraftaki menüde **Sorgu düzenleyicisi (önizleme)**’ne tıklayın, ardından **Oturum aç**’a tıklayın.

   ![oturum açma](./media/sql-database-get-started-portal/query-editor-login.png)

2. SQL sunucu kimlik doğrulamasını seçin, gerekli oturum açma bilgilerini sağlayın ve sonra oturum açmak için **Tamam**’a tıklayın.

3. **ServerAdmin** olarak kimlik doğrulaması yaptıktan sonra sorgu düzenleyici bölmesine aşağıdaki sorguyu yazın.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. **Çalıştır**’a tıklayın ve **Sonuçlar** bölmesindeki sorgu sonuçlarını gözden geçirin.

   ![sorgu düzenleyicisi sonuçları](./media/sql-database-get-started-portal/query-editor-results.png)

5. **Sorgu düzenleyicisi** sayfasını kapatın, kaydedilmemiş düzenlemelerinizi iptal etmek için **Tamam**’a tıklayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Sonraki adımlar](#next-steps)’a geçip farklı yöntemler kullanarak veritabanını sorgulama hakkında bilgi edinmek istiyorsanız bu kaynakları kaydedin. Ancak bu hızlı başlangıçta oluşturduğunuz kaynakları silmek isterseniz, aşağıdaki adımları kullanın.


1. Azure portalında sol taraftaki menüden, **Kaynak grupları**’na tıklayın ve ardından **myResourceGroup**’a tıklayın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- Artık bir veritabanınız olduğuna göre şirket içi araçlarınızdan veritabanına bağlanmak için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturmanız gerekir. Bkz: [Sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-get-started-portal-firewall.md)
- Sunucu düzeyinde güvenlik duvarı kuralı oluşturuyorsanız, şunlar dahil tercih ettiğiniz araç veya dilleri kullanarak [bağlanma ve sorgulama](sql-database-connect-query.md) işlemi yapabilirsiniz:
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Azure CLI kullanarak veritabanları oluşturmak için bkz: [Azure CLI örnekleri](sql-database-cli-samples.md)
- Azure PowerShell kullanarak veritabanları oluşturmak için bkz: [Azure PowerShell örnekleri](sql-database-powershell-samples.md)