Şimdi bir grafik veritabanı oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. Azure portalında sol taraftaki menüyü seçin **Veri Gezgini (Önizleme)**.

2. Altında **Veri Gezgini (Önizleme)**seçin **yeni bir grafik**. Ardından sayfasında aşağıdaki bilgileri kullanarak doldurun:

    ![Azure portalında Veri Gezgini](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|sample-database|Yeni veritabanınızın kimliği. Veritabanı adı 1 ila 255 karakter arasında olmalıdır ve içeremez `/ \ # ?` veya sonunda boşluk.
    Grafik kimliği|sample-graph|Yeni grafiğinizin kimliği. Grafik adları veritabanı kimlikleri aynı karakter gereksinimlerine sahip.
    Depolama kapasitesi| 10 GB|Varsayılan değeri değiştirmeyin. Bu, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|Varsayılan değeri değiştirmeyin. Daha sonra gecikme süresini azaltmak isterseniz, aktarım hızının ölçeğini artırabilirsiniz.
    Bölüm anahtarı|/firstName|Verileri her bölüme eşit şekilde dağıtan bir bölüm anahtarı. Doğru bölüm anahtarı seçerek bir kullanıcı grafik oluşturmada önemlidir. Daha fazla bilgi için bkz: [bölümleme için tasarlama](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Formun doldurulur sonra seçin **Tamam**.
