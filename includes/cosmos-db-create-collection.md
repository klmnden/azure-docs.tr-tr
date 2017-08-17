Şimdi bir veritabanı ve koleksiyon oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. Azure portalının solundaki gezinti menüsünde **Veri Gezgini (Önizleme)** seçeneğine tıklayın. 

2. **Veri Gezgini (Önizleme)** dikey penceresinde **Yeni Koleksiyon**'a tıklayın ve aşağıdaki bilgileri girin:

    ![Azure portalındaki Veri Gezgini dikey penceresi](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|Görevler|Yeni veritabanınızın adı. Veritabanı adı 1 ila 255 karakterden oluşmalı, boşlukla bitmemeli ve şu karakterleri içermemelidir: /, \\, # ve ?.
    Koleksiyon kimliği|Öğeler|Yeni koleksiyonunuzun adı. Koleksiyon adı karakter gereksinimleri, veritabanı kimliklerine ilişkin karakter gereksinimleri ile aynıdır.
    Depolama kapasitesi| Sabit (10 GB)|Varsayılan değeri kullanın. Bu değer, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|Varsayılan değeri kullanın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.
    RU/dk|Kapalı|Varsayılan değeri değiştirmeyin.
    Bölüm anahtarı|/kategori|Verileri her bölüme eşit şekilde dağıtan bir bölüm anahtarı. Koleksiyon performansının yüksek olması için doğru bölüm anahtarının seçilmesi önemlidir. Daha fazla bilgi için bkz. [Bölümleme için tasarlama](../articles/cosmos-db/partition-data.md#designing-for-partitioning).    
3. Formu tamamladığınızda **Tamam**'a tıklayın.

Veri Gezgini, yeni veritabanını ve koleksiyonu gösterir. 