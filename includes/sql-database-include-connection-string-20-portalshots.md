
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Bağlantı dizesi Azure portaldan alın
Kullanım [Azure portal](https://portal.azure.com/) istemci programınız Azure SQL veritabanı ile etkileşim kurmak için gerekli bağlantı dizesini almak için: 

1. Tıklatın **Gözat** > **SQL veritabanları**.
2. Sol üst köşesindeki yakın filtre metni kutusuna veritabanınızın adını girin **SQL veritabanları** dikey.
3. Veritabanınız için satıra tıklayın.
4. Veritabanınız için dikey göründükten sonra visual kolaylık sağlamak için gözatma ve veritabanı filtreleme için kullanılan Kanatlar daraltmak için standart simge durumuna küçült denetimleri tıklatabilirsiniz. 
   
    ![Veritabanınızı yalıtmak için filtre][10-FilterDatabase]
5. Veritabanınız için dikey penceresinde **veritabanı bağlantı dizelerini Göster**.
6. ADO.NET bağlantı kitaplığı kullanmak istiyorsanız, etiketli dizesi kopyalama **ADO**. 
   
    ![Veritabanınız için ADO bağlantı dizesini kopyalayın][20-CopyAdoConnectionString]
7. Bir veya başka bir biçiminde, bağlantı dizesi bilgilerini istemci program kodunuza yapıştırın.

Daha fazla bilgi için bkz.<br/>[Bağlantı dizeleri ve yapılandırma dosyalarını](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
