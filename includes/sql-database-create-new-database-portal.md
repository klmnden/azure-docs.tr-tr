
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Yeni bir Azure SQL Database oluşturma
Yeni Azure SQL Database’i yeni veya var olan Azure SQL Database mantıksal sunucusunda oluşturmak için Azure Portal'da aşağıdaki adımları kullanın.

1. O anda bağlı değilseniz [Azure portal](http://portal.azure.com)’a bağlanın.
2. **Yeni**’ye tıklayın, **SQL Veritabanı** yazın ve **SQL Veritabanı (yeni veritabanı)** seçeneğini belirleyin.
   
     ![Yeni veritabanı](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)
3. **SQL Veritabanı (yeni veritabanı)** öğesine tıklayın.
   
     ![Yeni veritabanı](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)
4. SQL Database hizmetinde yeni veritabanı oluşturmak için **Oluştur**’a tıklayın.
   
     ![Yeni veritabanı](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)
5. Şu sunucu özellikleri için değerleri girin:
   
   * Veritabanı adı
   * Abonelik: Yalnızca birden fazla aboneliğiniz varsa geçerlidir.
   * Kaynak grubu: Yeni başlıyorsanız mantıksal sunucunun kaynak grubunu kullanın.
   * Kaynak seç: Boş veritabanı, örnek verileri veya bir Azure veritabanı yedeği seçebilirsiniz. Şirket içi SQL Server veritabanını geçirmek veya BCP komut satırı aracını kullanarak veri yüklemek için bu makalenin sonundaki bağlantılara bakın.
   * Sunucu: Yeni veya var olan bir mantıksal sunucu.
   * Sunucu yöneticisi oturum açma
   * Parola
   * Fiyatlandırma katmanı: Yeni başlıyorsanız varsayılan S0 değerini kullanın.
   * Harmanlama: Yalnızca boş bir veritabanı seçilmişse geçerlidir.
     
        ![Yeni veritabanı](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)
6. **Oluştur**’a tıklayın. Bildirim alanında dağıtımın başlatıldığını görebilirsiniz.
   
    ![Yeni veritabanı](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)
7. Sonraki adıma geçmeden önce dağıtımın tamamlanmasını bekleyin.
   
     ![Yeni veritabanı](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)



<!--HONumber=Jan17_HO3-->


