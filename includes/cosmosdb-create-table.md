Şimdi Veri Gezgini'ni kullanarak bir grafik kapsayıcısı oluşturabilir ve veritabanınıza veri ekleyebilirsiniz. 

1. Azure portalındaki gezinme menüsünün **Tablolar** bölümünde **Veri Gezgini**'ne tıklayın. 
2. Veri Gezgini dikey penceresinde **Yeni Koleksiyon**'a tıklayın ve sayfaya aşağıda verilen bilgileri girin.

    ![Azure portalında Veri Gezgini](./media/cosmosdb-create-graph/azure-cosmosdb-data-explorer.png)

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|sample-database|Yeni veritabanınızın kimliği. Veritabanı adı 1 ile 255 karakter arasında olmalı, `/ \ # ?` içermemeli ve boşlukla bitmemelidir.
    Koleksiyon kimliği|sample-table|Yeni koleksiyonunuzun kimliği. Koleksiyon adı karakter gereksinimleri veritabanı kimliğiyle aynıdır.
    Depolama Kapasitesi| 10 GB|Varsayılan değeri değiştirmeyin. Bu, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|Varsayılan değeri değiştirmeyin. Daha sonra gecikme süresini azaltmak isterseniz, aktarım hızının ölçeğini artırabilirsiniz.
    Bölüm anahtarı|/databases|Verileri bölümlere eşit şekilde dağıtacak bölüm anahtarıdır. Koleksiyon performansının yüksek olması için doğru bölüm anahtarının seçilmesi önemlidir. Daha fazla bilgi için bkz. [Bölümleme için tasarlama](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Formu doldurduktan sonra **Tamam**'a tıklayın.