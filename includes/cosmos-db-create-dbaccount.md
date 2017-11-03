1. Yeni bir tarayıcı penceresinde oturum [Azure portal](https://portal.azure.com/).
2. Tıklatın **yeni** > **veritabanları** > **Azure Cosmos DB**.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. İçinde **yeni hesabı** sayfasında, yeni Azure Cosmos DB hesap ayarlarını girin. 
 
    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Kimlik|*Benzersiz bir ad girin*|Bu Azure Cosmos DB hesabını tanımlayacak benzersiz bir ad girin. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulacağından benzersiz ancak tanımlanabilir bir kimlik kullanın.<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 ila 50 karakterden oluşmalıdır.
    API|SQL (DocumentDB)|API oluşturmak için hesabı türünü belirler. Azure Cosmos DB sağlar dört API uygulamanızın gereksinimlerine uygun: Gremlin (grafiği), MongoDB, SQL (DocumentDB) ve tablosu (anahtar-değer), her gerektiren şu anda ayrı bir hesap. <br><br>Seçin **SQL (DocumentDB)** çünkü bu hızlı başlangıcı, SQL söz dizimi kullanılarak sorgulanabilir bir belge veritabanı oluşturuluyor.<br><br>[DocumentDB API'si hakkında daha fazla bilgi edinin](../articles/cosmos-db/documentdb-introduction.md)|
    Abonelik|*Aboneliğiniz*|Bu Azure Cosmos DB hesap için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|*Yukarıdaki Kimliğinde sağlanan gibi aynı benzersiz bir ad girin*|Hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz. 
    Konum|*Kullanıcılarınız için en yakın bölgeyi seçin*|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Hızlı erişim için veri vermediğiniz kullanıcılarınıza en yakın konumu kullanın.
    Coğrafi yedeklilik etkinleştir| Boş bırakın | Bu, ikinci (eşleştirilmiş) bölgede veritabanınızı çoğaltılmış bir sürümünü oluşturur. Bu alanı boş bırakın.  
    Panoya sabitle | Şunu seçin: | Bu kutuyu seçin, böylece portalı panonuza kolay erişim için yeni veritabanı hesabınızı eklenir.

    Sonra **Oluştur**’a tıklayın.

    ![Azure Cosmos DB için yeni hesap dikey penceresi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

4. Hesap oluşturma birkaç dakika sürer. Oluşturma sırasında portal görüntüler hesap **dağıtımı Azure Cosmos DB** döşeme.

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount/deploying-cosmos-db.png)

    Hesap oluşturulduktan sonra **Tebrikler! Azure Cosmos DB hesabınız oluşturuldu** sayfası görüntülenir. 

