Şimdi bir veritabanı ve koleksiyon oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. **Veri Gezgini** > **Yeni Koleksiyon**’a tıklayın. 
    
    **Koleksiyon Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, Koleksiyon Ekle dikey penceresi](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. **Koleksiyon Ekle** sayfasında, yeni koleksiyon için ayarları girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|Görevler|Yeni veritabanınızın adı olarak *Görevler* girin. Veritabanı adı 1 ila 255 karakterden oluşmalı, boşlukla bitmemeli ve şu karakterleri içermemelidir: /, \\, # ve ?.
    Koleksiyon kimliği|Öğeler|Yeni koleksiyonunuzun adı olarak *Öğeler* girin. Koleksiyon kimliği karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.
    Depolama kapasitesi| Sabit (10 GB)|Değeri **Sabit (10 GB)** olarak değiştirin. Bu değer, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak değiştirin. Aktarım hızını 400 RU/s olarak ayarlamak için depolama hızı **Sabit (10 GB)** olarak ayarlanmalıdır. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. 
    
    **Tamam**’a tıklayın.

    Veri Gezgini, yeni veritabanını ve koleksiyonu görüntüler.

    ![Yeni veritabanını ve koleksiyonu gösteren Azure portalı Veri Gezgini](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)