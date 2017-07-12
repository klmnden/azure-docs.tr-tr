1. Yeni bir pencerede [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sol bölmede **Yeni**, **Veritabanları** ve ardından **Azure Cosmos DB** seçeneğine tıklayın.
   
   ![Azure portalındaki Veritabanları bölmesi](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. **Yeni hesap** dikey penceresinde, Azure Cosmos DB hesabı için istenen yapılandırmayı belirtin. 

    Azure Cosmos DB'de dört programlama modelinden birini seçebilirsiniz: Gremlin (grafik), MongoDB, SQL (DocumentDB) ve Tablo (anahtar-değer).  
       
    Bu hızlı başlangıç makalesinde Grafik API'si ile programlama yapacağımız için formu doldururken **Gremlin (grafik)** seçeneğini belirleyin. Ancak bir katalog uygulamasından belge verileriniz, anahtar/değer (tablo) verileriniz veya bir MongoDB uygulamasından aktarılmış verileriniz varsa Azure Cosmos DB'nin görev açısından kritik tüm uygulamalarınız için yüksek oranda kullanılabilir ve genel olarak dağıtılmış bir veritabanı hizmeti platformu sunacağını unutmayın.

    **Yeni hesap** dikey penceresindeki alanları yalnızca aşağıdaki ekran görüntüsündeki bilgilerden yararlanarak doldurun. Kendi değerleriniz ekran görüntüsündeki değerler ile eşleşmeyeceği için hesabınızı ayarlarken benzersiz değerler seçtiğinizden emin olun. 
 
    ![Azure Cosmos DB dikey penceresi](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-2.png)

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Kimlik|*Benzersiz değer*|Azure Cosmos DB hesabını tanımlamak için seçtiğiniz benzersiz bir ad. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulacağından benzersiz ancak tanımlanabilir bir kimlik kullanın. Kimlik yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir ve 3 ila 50 karakterden oluşmalıdır.
    API|Gremlin (grafik)|Bu makalenin ilerleyen bölümlerinde [Grafik API'si](../articles/cosmos-db/graph-introduction.md) ile programlama yapacağız.|
    Abonelik|*Aboneliğiniz*|Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliği. 
    Kaynak Grubu|*Kimlikle aynı değer*|Hesabınız için yeni kaynak grubu adı. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz. 
    Konum|*Kullanıcılarınıza en yakın bölge*|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konum. Verilere en hızlı erişim için kullanıcılarınıza en yakın konumu seçin.

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.
5. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

    ![Dağıtım başlatıldı bildirimi](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  Dağıtım tamamlandığında **Tüm Kaynaklar** kutucuğundan yeni hesabı açın. 

    ![Tüm Kaynaklar kutucuğunda DocumentDB hesabı](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-all-resources.png)