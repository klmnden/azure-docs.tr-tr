1. Yeni bir pencerede [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sol taraftaki menüde **Yeni**'ye, **Veritabanları**'na ve ardından **Azure Cosmos DB**'ye tıklayın.
   
   ![Diğer Hizmetler ve Azure Cosmos DB seçeneklerinin vurgulandığı Azure portalı ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. **Yeni hesap** dikey penceresinde, Azure Cosmos DB hesabı için istenen yapılandırmayı belirtin. 

    Azure Cosmos DB'de dört programlama modelinden birini seçebilirsiniz: Gremlin (grafik), MongoDB, SQL (DocumentDB) ve Tablo (anahtar-değer). 
    
    Bu hızlı başlangıçta Tablo API'si ile programlama yapacağımız için formu doldururken **Tablo (anahtar-değer)** seçeneğini belirleyin. Ancak bir sosyal medya uygulaması için grafik verileriniz, bir katalog uygulamasından belge verileriniz veya bir MongoDB uygulamasından aktarılmış verileriniz varsa Azure Cosmos DB'nin tüm görev açısından kritik uygulamalarınız için yüksek oranda kullanılabilir ve genel olarak dağıtılmış bir veritabanı hizmeti platformu sunacağını unutmayın.

    Yeni hesap dikey penceresini ekran görüntüsündeki bilgileri kullanarak doldurun. Hesabınızın kurulumunu yaparken benzersiz değerler belirleyeceğiniz için değerler ekran görüntüsündekilerle bire bir aynı olmayacaktır. 
 
    ![Yeni Azure Cosmos DB dikey penceresinin ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Kimlik|*Benzersiz değer*|Azure Cosmos DB hesabını tanımlamak için kullanılacak benzersiz ad. Girdiğiniz kimliğe *documents.azure.com* eklenerek URI'niz oluşturulur, bu nedenle benzersiz ancak uygun bir kimlik kullanmanız gerekir. Kimlik yalnızca küçük harf, rakam ve "-" karakteri içerebilir; 3 ila 50 karakter uzunluğunda olmalıdır.
    AP|Tablo (anahtar-değer)|Bu makalenin ilerleyen bölümlerinde [Tablo API'si](../articles/cosmos-db/table-introduction.md) ile programlama yapacağız.|
    Abonelik|*Aboneliğiniz*|Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliği. 
    Kaynak Grubu|*Kimlikle aynı değer*|Hesabınız için yeni kaynak grubu adı. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz. 
    Konum|*Kullanıcılarınıza en yakın bölge*|Azure Cosmos DB hesabınızın barındırılacağı coğrafi konum. Verilere en hızlı erişim için kullanıcılarınıza en yakın konumu seçin.   

4. Hesabı oluşturmak için **Oluştur**’a tıklayın.
5. Araç çubuğunda **Bildirimler**’e tıklayarak dağıtım işlemini izleyin.

    ![Dağıtım başlatıldı bildirimi](./media/documentdb-create-dbaccount/azure-documentdb-nosql-notification.png)

6.  Dağıtım tamamlandığında Tüm Kaynaklar kutucuğundan yeni hesabı açın. 

    ![Tüm Kaynaklar kutucuğunda DocumentDB hesabı](./media/documentdb-create-dbaccount/azure-documentdb-all-resources.png)