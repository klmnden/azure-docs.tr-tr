Şimdi bir grafik veritabanı oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. Azure portalında sol taraftaki menüyü seçin **Veri Gezgini (Önizleme)**.

2. Altında **Veri Gezgini (Önizleme)**seçin **yeni bir grafik**. Ardından sayfasında aşağıdaki bilgileri kullanarak doldurun:

    ![Azure portalında Veri Gezgini](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Veritabanı kimliği|sample-database|Yeni veritabanınızın adını *sample-database* olarak belirleyin. Veritabanı adı 1 ila 255 karakter arasında olmalıdır ve içeremez `/ \ # ?` veya sonunda boşluk.
    Graf kimliği|sample-graph|Yeni koleksiyonunuzun adını *sample-graph* olarak belirleyin. Grafik adı karakter gereksinimleri, veritabanı kimliklerine ilişkin karakter gereksinimleri ile aynıdır.
    Depolama kapasitesi| 10 GB|Varsayılan değeri değiştirmeyin. Bu, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|Varsayılan değeri değiştirmeyin. Daha sonra gecikme süresini azaltmak isterseniz, aktarım hızının ölçeğini artırabilirsiniz.

3. Formun doldurulur sonra seçin **Tamam**.
