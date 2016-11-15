---
title: "SQL Veritabanı eğitmeni: SQL veritabanı oluşturma | Microsoft Belgeleri"
description: "SQL Veritabanı mantıksal sunucusu, sunucu güvenlik duvarı kuralı, SQL veritabanı ve örnek veriler oluşturma hakkında bilgi edinin. Ayrıca, istemci araçlarına bağlanma, kullanıcıları yapılandırma ve bir veritabanı güvenlik duvarı kuralı oluşturma hakkında bilgi alın."
keywords: "sql veritabanı öğreticisi, sql veritabanı oluşturma"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/07/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 914d5f7e6b8e9165fc8ba9021e1030b865ba5fb2


---
# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>SQL Veritabanı öğreticisi: Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturma
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-get-started.md)
> * [C#](sql-database-get-started-csharp.md)
> * [PowerShell](sql-database-get-started-powershell.md)
> 
> 

Bu öğreticide, Azure portalını kullanarak şu işlemleri gerçekleştirmeyi öğreneceksiniz:

* Örnek verilerle bir Azure SQL veritabanı oluşturun.
* Tek bir IP adresi veya bir IP adresi aralığı için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma.

Aynı görevleri [C#](sql-database-get-started-csharp.md) veya [PowerShell](sql-database-get-started-powershell.md) kullanarak gerçekleştirebilirsiniz.

[!INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>İlk Azure SQL veritabanınızı oluşturma
1. O anda bağlı değilseniz [Azure portal](http://portal.azure.com)’a bağlanın.
2. **Yeni**'ye ve **Veri + Depolama**'ya tıklayın; ardından, **SQL Veritabanı**'nı bulun.
   
    ![Yeni SQL veritabanı 1](./media/sql-database-get-started/sql-database-new-database-1.png)
3. SQL Veritabanı dikey penceresini açmak için **SQL Veritabanı**'na tıklayın. Bu dikey penceredeki içerik, abonelik sayınıza ve var olan nesne (örneğin, var olan sunucular) sayınıza göre değişir.
   
    ![Yeni SQL veritabanı 2](./media/sql-database-get-started/sql-database-new-database-2.png)
4. **Veritabanı adı** metin kutusunda, ilk veritabanınız için bir ad (örneğin, "veritabanım") sağlayın. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
   
    ![Yeni SQL veritabanı 3](./media/sql-database-get-started/sql-database-new-database-3.png)
5. Birden çok aboneliğiniz varsa bir abonelik seçin.
6. **Kaynak grubu** altındaki **Yeni oluştur** seçeneğine tıklayıp ilk kaynak grubunuz için bir ad (örneğin, "kaynak grubum") sağlayın. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
   
    ![Yeni SQL veritabanı 4](./media/sql-database-get-started/sql-database-new-database-4.png)
7. **Kaynak seç** altındaki **Örnek**'e tıklayın ve ardından **Örnek seç** altındaki **AdventureWorksLT [V12]** öğesine tıklayın.
   
    ![Yeni SQL veritabanı 5](./media/sql-database-get-started/sql-database-new-database-5.png)
8. **Sunucu** altındaki **Gerekli ayarları yapılandır**'a tıklayın.
   
    ![Yeni SQL veritabanı 6](./media/sql-database-get-started/sql-database-new-database-6.png)
9. Sunucu dikey penceresinde, **Yeni sunucu oluştur**'a tıklayın. Sunucu nesnesinde bir Azure SQL veritabanı oluşturulur. Bu, yeni bir sunucu veya var olan bir sunucu olabilir.
   
    ![Yeni SQL veritabanı 7](./media/sql-database-get-started/sql-database-new-database-7.png)
10. Yeni sunucunuz için sağlamanız gereken bilgileri öğrenmek üzere **Yeni sunucu** dikey penceresini inceleyin.
    
    ![Yeni SQL veritabanı 8](./media/sql-database-get-started/sql-database-new-database-8.png)
11. **Sunucu adı** metin kutusunda, ilk sunucunuz için bir ad (örneğin, "yeni sunucu nesnem") sağlayın. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![Yeni SQL veritabanı 9](./media/sql-database-get-started/sql-database-new-database-9.png)
12. **Sunucu yöneticisi oturum açma** altında, bu sunucu için bir yönetici oturum açma kullanıcı adı (örneğin, "yönetici hesabım") sağlayın. Bu oturum açma işlemi, asıl sunucu oturum açma işlemi olarak bilinir. Yeşil onay işareti, geçerli bir ad sağladığınızı gösterir.
    
    ![Yeni SQL veritabanı 10](./media/sql-database-get-started/sql-database-new-database-10.png)
13. **Parola** ve **Parolayı onayla**'nın altında, asıl sunucu oturum açma hesabı için bir parola (örneğin, "p@ssw0rd1".) sağlayın. Yeşil onay işareti, geçerli bir parola sağladığınızı gösterir.
    
    ![Yeni SQL veritabanı 11](./media/sql-database-get-started/sql-database-new-database-11.png)
14. **Konum**'un altında, konumunuza uygun bir veri merkezi (örneğin, "Avustralya Doğu") seçin.
    
    ![Yeni SQL veritabanı 12](./media/sql-database-get-started/sql-database-new-database-12.png)
15. **V12 sunucusu oluştur (Son güncelleştirme) seçeneğinin altında, yalnızca Azure SQL sunucusunun geçerli sürümünü oluşturma seçeneğinizin olduğuna dikkat edin.
    
    ![Yeni SQL veritabanı 13](./media/sql-database-get-started/sql-database-new-database-13.png)
16. **Azure hizmetlerinin sunucuya erişmesine izin ver** seçeneği için onay kutusunun varsayılan olarak seçili olduğuna ve bu seçimin burada değiştirilemeyeceğine dikkat edin. Bu gelişmiş bir seçenektir. Çoğu senaryo için gerekli olmasa da sunucu güvenlik duvarı ayarlarına giderek ilgili ayarı bu sunucu nesnesi için değiştirebilirsiniz.
    
    ![Yeni SQL veritabanı 14](./media/sql-database-get-started/sql-database-new-database-14.png)
17. Yeni sunucu dikey penceresinde, seçimlerinizi gözden geçirin ve ardından yeni veritabanınız için bu sunucuyu belirlemek üzere **Seç**'e tıklayın.
    
    ![Yeni SQL veritabanı 15](./media/sql-database-get-started/sql-database-new-database-15.png)
18. SQL Veritabanı dikey penceresindeki **Fiyatlandırma katmanı** seçeneğinin altında, ilk veritabanınız için en uygun maliyetli fiyatlandırma katmanını seçmek üzere **S2 Standard (S2 Standart)** seçeneğine ve ardından **Temel**'e tıklayın. Fiyatlandırma katmanını daha sonra istediğiniz zaman değiştirebilirsiniz.
    
    ![Yeni SQL veritabanı 16](./media/sql-database-get-started/sql-database-new-database-16.png)
19. SQL Veritabanı dikey penceresinde, seçimlerinizi gözden geçirin ve ardından ilk sunucunuzu ve veritabanınızı oluşturmak için **Oluştur**'a tıklayın. Sağladığınız değerler doğrulandı ve dağıtım başlıyor.
    
    ![Yeni SQL veritabanı 17](./media/sql-database-get-started/sql-database-new-database-17.png)
20. Dağıtımınızın durumunu denetlemek için Portal araç çubuğunda **Bildirimler** öğesine tıklayın.
    
    ![Yeni SQL veritabanı 18](./media/sql-database-get-started/sql-database-new-database-18.png)

> [!IMPORTANT]
> Dağıtım tamamlandığında, Azure'da yeni Azure SQL sunucunuz ve veritabanınız oluşturulur. SQL Veritabanı güvenlik duvarını Azure dışındaki bağlantılara açmak için bir sunucu güvenlik duvarı kuralı oluşturana kadar, SQL Server araçlarını kullanarak yeni sunucunuza ve veritabanınıza bağlanamazsınız.
> 
> 

[!INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu SQL Veritabanı öğreticisini tamamladığınıza ve bazı örnek verilerle bir veritabanı oluşturduğunuza göre sık kullandığınız araçları kullanarak araştırmaya hazırsınız.

* Transact-SQL ve SQL Server Management Studio'yu (SSMS) daha önce kullandıysanız [SSMS ile bir SQL veritabanına bağlanma ve veritabanını sorgulama](sql-database-connect-query-ssms.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* Excel kullanmayı biliyorsanız [Excel ile Azure’da SQL veritabanına bağlanma](sql-database-connect-excel.md) işlemini nasıl gerçekleştireceğinizi öğrenin.
* Kodlamaya başlamak için hazırsanız [SQL Veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) konu başlığına göz atarak programlama dilinizi belirleyin.
* Şirket içi SQL Server veritabanlarınızı Azure'a taşımak istiyorsanız daha fazla bilgi edinmek için bkz. [Bir veritabanını SQL Veritabanı’na geçirme](sql-database-cloud-migrate.md).
* BCP komut satırı aracını kullanarak yeni bir tabloya bir CSV dosyasından bazı veriler yüklemek istiyorsanız bkz. [BCP kullanarak bir CSV dosyasından SQL Veritabanına veri yükleme](sql-database-load-from-csv-with-bcp.md).
* Azure SQL Veritabanı güvenliğini keşfetmeye başlamak istiyorsanız bkz. [Güvenliğe giriş](sql-database-get-started-security.md)

## <a name="additional-resources"></a>Ek kaynaklar
[SQL Database nedir?](sql-database-technical-overview.md)




<!--HONumber=Nov16_HO2-->


