
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Bağlantı dizesi Azure portaldan alın
Kullanım [Azure portal](https://portal.azure.com/) istemci programınız Azure SQL veritabanı ile etkileşim kurmak için gerekli olan bağlantı dizesini almak için. 

1. Seçin **TÜMÜNE Gözat** > **SQL veritabanları**.

2. Sol üst tarafındaki yakın filtre metin kutusuna veritabanınızın adını girin **SQL veritabanları** dikey.

3. Veritabanınız için satırı seçin.

4. Görsel kolaylık seçin için veritabanınızın dikey göründükten sonra **simge durumuna küçült** gözatma ve veritabanı filtreleme için kullanılan Kanatlar daraltmak için düğmeler. 
   
    ![Veritabanınızı yalıtmak için filtre][10-FilterDatabase]
5. Veritabanınız için dikey seçin **veritabanı bağlantı dizelerini Göster**.

6. ADO.NET bağlantı kitaplığı kullanmak istiyorsanız, etiketli dizesi kopyalama **ADO**. 
   
    ![Veritabanınız için ADO bağlantı dizesini kopyalayın][20-CopyAdoConnectionString]
7. Bir veya başka bir biçiminde, bağlantı dizesi bilgilerini istemci program kodunuza yapıştırın.

Daha fazla bilgi için bkz: [bağlantı dizeleri ve yapılandırma dosyalarını](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
