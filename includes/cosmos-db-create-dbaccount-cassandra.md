1. Yeni bir tarayıcı penceresinde oturum [Azure portal](https://portal.azure.com/).
2. Tıklatın **yeni** > **veritabanları** > **Azure Cosmos DB**.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. İçinde **yeni hesabı** sayfasında, yeni Azure Cosmos DB hesap ayarlarını girin. 
 
    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Kimlik|*Benzersiz bir ad girin*|Bu Azure Cosmos DB hesabını tanımlayacak benzersiz bir ad girin. Çünkü *documents.azure.com* sağlayan kişi noktanız oluşturmak için bir benzersiz ancak tanımlanabilen kimliğini kullanan Kimliğine eklenir<br><br>Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 ila 50 karakterden oluşmalıdır.
    API|Cassandra|API oluşturmak için hesabı türünü belirler. Azure Cosmos DB sağlar beş API uygulamanızın gereksinimlerine uygun: SQL (belge veritabanı), Gremlin (grafik veritabanı), MongoDB (belge veritabanı), Azure Table ve Cassandra, her gerektiren şu anda ayrı bir hesap. <br><br>Seçin **Cassandra** çünkü bu hızlı başlangıcı, CQL sözdizimi kullanılarak sorgulanabilir bir genişliğinde bir sütun veritabanı oluşturuluyor.<br><br>Cassandra (wide-sütun) listenizde görüntülenmez sonra yapmanız [katılmaya uygulamak](https://aka.ms/cosmosdb-cassandra-signup) Cassandra API Önizleme programı.<br><br> [Cassandra API'si hakkında daha fazla bilgi edinin](../articles/cosmos-db/cassandra-introduction.md)|
    Abonelik|*Aboneliğiniz*|Bu Azure Cosmos DB hesap için kullanmak istediğiniz Azure aboneliğini seçin. 
    Kaynak Grubu|*Yukarıdaki Kimliğinde sağlanan gibi aynı benzersiz bir ad girin*|Hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz. 
    Konum|*Kullanıcılarınız için en yakın bölgeyi seçin*|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Hızlı erişim için veri vermediğiniz kullanıcılarınıza en yakın konumu kullanın.
    Panoya sabitle | Şunu seçin: | Bu kutuyu seçin, böylece portalı panonuza kolay erişim için yeni veritabanı hesabınızı eklenir.

    Sonra **Oluştur**’a tıklayın.

    ![Azure Cosmos DB için yeni hesap dikey penceresi](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-2.png)

4. Hesap oluşturma birkaç dakika sürer. Portal panosunda hesabı oluşturma sırasında görüntüler **dağıtımı Azure Cosmos DB** döşeme.

    ![Azure portal bildirimleri döşeme](./media/cosmos-db-create-dbaccount-cassandra/deploying-cosmos-db.png)

    Hesap oluşturulduktan sonra **Tebrikler! Azure Cosmos DB hesabınız oluşturuldu** sayfası görüntülenir. 

