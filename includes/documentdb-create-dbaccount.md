1. Yeni bir pencerede [Azure portalında](https://portal.azure.com/) oturum açın.
2. Atlama Çubuğunda **Yeni**'ye, **Veritabanları**'na ve ardından **NoSQL (DocumentDB)** öğesine tıklayın.
   
   ![Diğer Hizmetler ve DocumentDB (NoSQL) seçeneklerinin gösterildiği Azure portalı ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  
3. **Yeni hesap** dikey penceresinde, DocumentDB hesabı için istenen yapılandırmayı belirtin.
   
    ![Yeni DocumentDB dikey penceresinin ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)
   
   * **Kimlik** kutusuna, DocumentDB hesabını tanımlayacak bir ad girin.  **Kimlik** doğrulandığında, **Kimlik** kutusunda yeşil bir onay işareti görünür. **Kimlik** değeri, URI içinde konak adı olarak kullanılır. **Kimlik**, yalnızca küçük harf, rakam ve "-" karakteri içerebilir; 3 ila 50 karakter uzunluğunda olmalıdır. *documents.azure.com*'un seçtiğiniz uç nokta adına eklendiğine ve bunun, DocumentDB hesabınızın uç noktası olarak kullanıldığına dikkat edin.
   * **NoSQL API** kutusunda **DocumentDB** öğesini seçin.  
   * **Abonelik** için, DocumentDB hesabınızda kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa bu hesap varsayılan olarak seçilidir.
   * **Kaynak Grubu**'nda, DocumentDB hesabınız için bir kaynak grubu seçin veya oluşturun.  Varsayılan olarak yeni bir kaynak grubu oluşturulur. Daha fazla bilgi edinmek için bkz. [Azure portalını kullanarak Azure kaynaklarınızı yönetme](../articles/azure-portal/resource-group-portal.md).
   * DocumentDB hesabınızın barındırılacağı coğrafi konumu belirtmek için **Konum**'u kullanın. 
4. Yeni DocumentDB hesabı seçenekleri yapılandırıldıktan sonra **Oluştur**'a tıklayın. Dağıtım durumunu denetlemek için, Bildirimler hub'ına bakın.  
   
   ![Hızlı bir şekilde veritabanı oluşturma - DocumentDB hesabının oluşturulduğunu gösteren Bildirimler hub'ı ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  
   
   ![DocumentDB hesabının başarılı bir şekilde oluşturulduğunu ve bir kaynak grubuna dağıtıldığını gösteren Bildirimler hub'ı ekran görüntüsü - Çevrimiçi veritabanı oluşturucu bildirimi](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)
5. DocumentDB hesabı oluşturulduktan sonra, varsayılan ayarlarla birlikte kullanıma hazırdır. Varsayılan ayarları gözden geçirmek için atlama çubuğunda **NoSQL (DocumentDB)** simgesine tıklayın, yeni hesabınızı seçin ve kaynak menüsünde **Varsayılan tutarlılık** öğesine tıklayın.

   ![Azure DocumentDB veritabanı hesabınızı Azure portalında nasıl açacağınızı gösteren ekran görüntüsü](./media/documentdb-create-dbaccount/azure-documentdb-database-open-account-portal.png)  

   DocumentDB hesabının varsayılan tutarlılığı **Oturum** olarak ayarlandı.  Kullanılabilir diğer tutarlılık seçeneklerinden birini belirleyerek varsayılan tutarlılığı ayarlayabilirsiniz. DocumentDB tarafından sunulan tutarlılık düzeyleri hakkında daha fazla bilgi edinmek için bkz. [DocumentDB'deki tutarlılık düzeyleri](../articles/documentdb/documentdb-consistency-levels.md).

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md


<!--HONumber=Jan17_HO1-->


