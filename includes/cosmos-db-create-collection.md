Şimdi bir veritabanı ve koleksiyon oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. Tıklatın **Veri Gezgini** > **yeni koleksiyon**. 
    
    **Topluluk Ekle** alanı sağ ucundaki görüntülenir, hemen görmek için kaydırmanız gerekebilir.

    ![Azure portal Veri Gezgini, topluluk Ekle dikey penceresi](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. İçinde **koleksiyonu ekleyin** sayfasında, yeni bir koleksiyon ayarlarını girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|Görevler|Girin *görevleri* yeni bir veritabanı adı olarak. Veritabanı adı 1 ila 255 karakterden oluşmalı, boşlukla bitmemeli ve şu karakterleri içermemelidir: /, \\, # ve ?.
    Koleksiyon kimliği|Öğeler|Girin *öğeleri* yeni koleksiyonunuz için bir ad olarak. Koleksiyon kimlikleri veritabanı adları aynı karakter gereksinimlerine sahip.
    Depolama kapasitesi| Sabit (10 GB)|Bir değerle değiştirmek **sabit (10 GB)**. Bu değer, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|İşleme 400 istek birimleri (RU/s) saniyede değiştirin. Depolama kapasitesi ayarlanmalıdır **sabit (10 GB)** işleme 400 RU/s ayarlamak için. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz. 
    Bölüm anahtarı|/kategori|Girin */Category* bölüm anahtarı olarak. Bölüm anahtarı veri veritabanında her bölüm için eşit olarak dağıtır. Bölümleme hakkında daha fazla bilgi için bkz: [bölümleme için tasarlama](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

    **Tamam** düğmesine tıklayın.

    Veri Gezgini yeni bir veritabanı ve koleksiyonu görüntüler.

    ![Azure portalında yeni bir veritabanı ve koleksiyonu gösteren Veri Gezgini](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)