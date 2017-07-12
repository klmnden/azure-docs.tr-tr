1. Yeni bir pencerede [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sol bölmede **Yeni**, **Veritabanları** ve ardından **Azure Cosmos DB** seçeneğine tıklayın.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. **Yeni hesap** dikey penceresinde Azure Cosmos DB hesabınız için istediğiniz yapılandırmayı belirtin. 

    Azure Cosmos DB'de dört programlama modelinden birini seçebilirsiniz: Gremlin (grafik), MongoDB, SQL (DocumentDB) ve Tablo (anahtar-değer). 
    
    Bu hızlı başlangıç makalesinde DocumentDB API'si ile programlama yapacağımız için formu doldururken **SQL (DocumentDB)** seçeneğini belirleyin. Ancak bir sosyal medya uygulaması için grafik verileriniz, anahtar/değer (tablo) verileriniz veya bir MongoDB uygulamasından aktarılmış verileriniz varsa Azure Cosmos DB'nin görev açısından kritik tüm uygulamalarınız için yüksek oranda kullanılabilir ve genel olarak dağıtılmış bir veritabanı hizmeti platformu sunacağını unutmayın.

    **Yeni hesap** dikey penceresindeki alanları, aşağıdaki ekran görüntüsündeki bilgilerden yararlanarak doldurun. Hesabınızı ayarlarken, ekran görüntüsündeki değerlerle eşleşmeyen benzersiz değerleri seçin. 
 
    ![Yeni Azure Cosmos DB dikey penceresi](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Kimlik|*Benzersiz değer*|Azure Cosmos DB hesabınızı tanımlayan benzersiz bir ad. Girdiğiniz kimliğe *documents.azure.com* dizesi eklenerek URI'niz oluşturulur, bu nedenle benzersiz ancak tanımlanabilir bir kimlik kullanın. Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 ila 50 karakterden oluşmalıdır.
    API|SQL (DocumentDB)|Bu makalenin ilerleyen bölümlerinde [DocumentDB API'si](../articles/documentdb/documentdb-introduction.md) ile programlama yapacağız.|
    Abonelik|*Aboneliğiniz*|Azure Cosmos DB hesabınız için kullanmak istediğiniz Azure aboneliği. 
    Kaynak Grubu|*Kimlikle aynı değer*|Hesabınız için yeni kaynak grubu adı. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz. 
    Konum|*Kullanıcılarınıza en yakın bölge*|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konum. Verilere en hızlı erişim için kullanıcılarınıza en yakın konumu seçin.
4. Hesabı oluşturmak için **Oluştur**’a tıklayın.
5. Üst araç çubuğunda **Bildirimler**'e tıklayarak dağıtım işlemini izleyin.

    ![Azure portalındaki Bildirimler bölmesi](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  Dağıtım tamamlandığında **Tüm Kaynaklar** kutucuğundan yeni hesabı açın. 

    ![Tüm Kaynaklar kutucuğundaki DocumentDB hesabı](./media/cosmos-db-create-dbaccount/all-resources.png)
 
