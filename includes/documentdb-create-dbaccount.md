1.  [Azure Portal](https://portal.azure.com/) oturum açın.
2.  Atlama çubuğunda **Yeni**'ye, **Veri + Depolama**'ya ve sonra **DocumentDB (NoSQL)** öğesine tıklayın.

    ![Diğer Hizmetler ve DocumentDB (NoSQL) seçeneğini vurgulayan Azure portalı ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  

3. **Yeni hesap** dikey penceresinde DocumentDB hesabı için istenen yapılandırmayı belirtin.

    ![Yeni DocumentDB dikey penceresi ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)


    - **Kimlik** kutusuna, DocumentDB hesabını tanımlayacak adı girin.  **Kimlik** doğrulandığında, **Kimlik** kutusunda yeşil bir onay işareti belirir. **Kimlik** değeri URI içindeki konak adına dönüşür. **Kimlik**’te yalnızca küçük harfler, rakamlar ve '-' karakteri bulunabilir; 3 - 50 arası karakter uzunluğunda olmalıdır. *documents.azure.com* öğesinin seçtiğiniz uç nokta adına eklendiğini unutmayın; bunun sonucunu DocumentDB hesabınızın uç noktasına dönüşecektir.

    - **Abonelik** için DocumentDB hesabına yönelik kullanmak istediğiniz Azure aboneliğini girin. Hesabınızda yalnızca bir abonelik varsa bu hesap varsayılan olarak seçilidir.

    - **Kaynak Grubu**’nda DocumentDB hesabınız için bir kaynak grubu seçin veya oluşturun.  Varsayılan olarak yeni bir kaynak grubu oluşturulur. Daha fazla bilgi edinmek için bkz. [Azure kaynaklarınızı yönetmek için Azure portalını kullanma](../articles/azure-portal/resource-group-portal.md).

    - DocumentDB hesabınızın barındırılacağı coğrafi konumu belirtmek için **Konum**’u kullanın. 
    
    - Daha sonra oluşturacağınız kaynaklarınıza ve hesabınıza kolay erişim sağlamak için **Panoya sabitle** seçeneğini işaretleyin.  

4.  Yeni DocumentDB hesabı seçenekleri yapılandırıldıktan sonra **Oluştur**’a tıklayın. Dağıtım durumunu kontrol etmek için ilerlemeyi Başlangıç panosunda izleyebilirsiniz.  
    ![Çevrimiçi veritabanı oluşturucusu; Başlangıç panosunda Oluşturuluyor kutucuğunun ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-3.png)  

    Bildirimler hub'ından da ilerleme durumunu izleyebilirsiniz.  

    ![Veritabanlarını hızlı oluşturma - DocumentDB hesabının oluşturulduğunu gösteren Bildirimler hub'ının ekran görüntüsü](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  

    ![DocumentDB hesabının sorunsuz oluşturulduğunu ve bir kaynak grubuna dağıtıldığını gösteren Bildirimler hub’ının ekran görüntüsü- Çevrimiçi veritabanı oluşturucu bildirimi](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)

5.  DocumentDB hesabı oluşturulduktan sonra varsayılan ayarlarla birlikte kullanıma hazırdır. DocumentDB hesabının varsayılan tutarlılığının **Oturum** olarak ayarlandığını unutmayın.  Kaynak menüsünde **Varsayılan Tutarlılık** seçeneğine tıklayarak varsayılan tutarlılık ayarını belirleyebilirsiniz. DocumentDB tarafından sunulan tutarlılık düzeyleri hakkında daha fazla bilgi edinmek için bkz. [DocumentDB'deki tutarlılık düzeyleri](../articles/azure-portal/resource-group-portal.md).

    ![Kaynak Grubu dikey penceresi ekran görüntüsü - uygulama geliştirmesini başlatma](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  

    ![Tutarlılık düzeyi dikey penceresi ekran görüntüsü - Oturum Tutarlılığı](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[Nasıl yapılır: DocumentDB hesabı oluşturma]: #Howto
[Sonraki adımlar]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md



<!--HONumber=sep16_HO1-->


