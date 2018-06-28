## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-blank-sql-database"></a>Boş bir SQL veritabanı oluşturma

Azure SQL veritabanı bir dizi [işlem ve depolama kaynağı](../articles/sql-database/sql-database-service-tiers-dtu.md) ile oluşturulur. Veritabanı bir [Azure kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md) ve bir [Azure SQL Veritabanı mantıksal sunucusu](../articles/sql-database/sql-database-features.md) içinde oluşturulur. 

Boş bir SQL veritabanı oluşturmak için aşağıdaki adımları izleyin. 

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

2. **Yeni** penceresinden **Veritabanları**’nı seçin ve **Yeni** sayfasında **SQL Veritabanı** altından **Oluştur**’u seçin.

   ![create empty-database](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

3. SQL Veritabanı formunu, önceki görüntüde gösterildiği gibi aşağıdaki bilgilerle doldurun:   

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Veritabanı adı** | mySampleDatabase | Geçerli veritabanı adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). | 
   | **Abonelik** | Aboneliğiniz  | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
   | **Kaynak grubu** | myResourceGroup | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Kaynak seçme** | Boş veritabanı | Boş bir veritabanı oluşturulması gerektiğini belirtir. |

4. Yeni veritabanınız için yeni bir sunucu oluşturup yapılandırmak üzere **Sunucu**’ya tıklayın. **Yeni sunucu formu**’nu aşağıdaki bilgilerle doldurun: 

   | Ayar       | Önerilen değer | Açıklama | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Sunucu adı** | Genel olarak benzersiz bir ad | Geçerli sunucu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Sunucu yöneticisi oturum açma bilgileri** | Geçerli bir ad | Geçerli oturum açma adları için bkz. [Veritabanı Tanımlayıcıları](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
   | **Parola** | Geçerli bir parola | Parolanızda en az 8 karakter bulunmalı ve parolanız şu üç kategoriden karakterler içermelidir: büyük harf karakterler, küçük harf karakterler, sayılar ve alfasayısal olmayan karakterler. |
   | **Konum** | Geçerli bir konum | Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/). |

   ![create database-server](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

5. **Seç**'e tıklayın.

6. Hizmet katmanını, DTU'ların sayısını ve depolama alanı miktarını belirtmek için **Fiyatlandırma katmanı**’na tıklayın. Her hizmet katmanı için kullanılabilir DTU'lar ve depolama alanı miktarı seçeneklerini araştırın. 

7. Bu öğreticide, **standart** hizmet katmanını seçip kaydırıcıyı kullanarak **100 DTU (S3)** ve **400** GB depolama alanını seçin.

   ![create database-s1](../articles/sql-database/media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. **Ek Depolama** seçeneğini kullanmak için önizleme koşullarını kabul edin. 

   > [!IMPORTANT]
   > \* Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* Premium katmanında şu anda şu bölgelerde 1 TB’den fazla depolama alanı mevcuttur: Orta Kanada, Doğu Kanada, Fransa Orta, Orta Almanya, Doğu Japonya, Orta Kore, Güney Orta ABD, Güneydoğu Asya, Doğu ABD2, Batı ABD, ABD Devleti Virginia ve Batı Avrupa. Bkz. [P11 P15 Geçerli Sınırlamalar](../articles/sql-database/sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Sunucu katmanını, DTU'ların sayısını ve depolama alanı miktarını seçtikten sonra **Uygula**’ya tıklayın.  

10. Boş veritabanı için bir **harmanlama** seçin (bu öğretici için varsayılan değeri kullanın). Harmanlamalar hakkında daha fazla bilgi için bkz. [Harmanlamalar](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. Veritabanını sağlamak için **Oluştur**’a tıklayın. Sağlama işleminin tamamlanması yaklaşık olarak bir buçuk dakika sürer. 

12. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.
    
     ![bildirim](../articles/sql-database/media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

SQL Veritabanı hizmeti, güvenlik duvarını belirli IP adreslerine açmaya yönelik bir güvenlik duvarı kuralı oluşturulmadıkça, dış uygulama ve araçların sunucuya ya da sunucu üzerindeki herhangi bir veritabanına bağlanmasını engelleyen sunucu düzeyinde bir güvenlik duvarı kuralı oluşturur. SQL Veritabanı güvenlik duvarı üzerinden yalnızca IP adresinize yönelik dış bağlantıları etkinleştirmek üzere istemcinizin IP adresi için bir [SQL Veritabanı sunucu düzeyi güvenlik duvarı kuralı](../articles/sql-database/sql-database-firewall-configure.md) oluşturmak için bu adımları izleyin. 

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL Veritabanı sunucunuza bağlanamazsınız.
>

1. Dağıtım tamamlandıktan sonra, soldaki menüden **SQL veritabanları**'na ve ardından **SQL veritabanları** sayfasında **mySampleDatabase** öğesine tıklayın. Veritabanınıza ilişkin genel bakış sayfası açılır ve tam sunucu adı (örneğin, **mynewserver20170824.database.windows.net**) görüntülenerek daha fazla yapılandırma seçeneği sunulur. 

2. Sonraki hızlı başlangıçlarda sunucunuza ve veritabanlarına bağlanmak için bu tam sunucu adını kopyalayın. 

   ![sunucu adı](../articles/sql-database/media/sql-database-get-started-portal/server-name.png) 

3. Araç çubuğunda **Sunucu güvenlik duvarını ayarla**’ya tıklayın. SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır. 

   ![sunucu güvenlik duvarı kuralı](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Geçerli IP adresinizi yeni bir güvenlik duvarı kuralına eklemek için araç çubuğunda **İstemci IP’si Ekle** öğesine tıklayın. Güvenlik duvarı kuralı, 1433 numaralı bağlantı noktasını tek bir IP adresi veya bir IP adresi aralığı için açabilir.

5. **Kaydet**’e tıklayın. Geçerli IP adresiniz için mantıksal sunucuda 1433 numaralı bağlantı noktası açılarak sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

6. **Tamam**’a tıklayın ve sonra **Güvenlik duvarı ayarları** sayfasını kapatın.

Artık SQL Server Management Studio’yu veya seçtiğiniz başka bir aracı kullanarak, daha önce oluşturduğunuz sunucu yöneticisi hesabıyla bu IP adresinden SQL Veritabanı sunucusuna ve sunucuya ait veritabanlarına bağlanabilirsiniz.


> [!IMPORTANT]
> Varsayılan olarak, SQL Veritabanı güvenlik duvarı üzerinden erişim tüm Azure hizmetleri için etkindir. Tüm Azure hizmetleri için devre dışı bırakmak isterseniz bu sayfadaki **KAPALI** öğesine tıklayın.

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

Azure SQL Veritabanı sunucunuzun tam sunucu adını Azure portaldan alabilirsiniz. SQL Server Management Studio kullanarak sunucunuza bağlanmak için tam sunucu adını kullanırsınız.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın.

   ![bağlantı bilgileri](../articles/sql-database/media/sql-database-get-started-portal/server-name.png)
