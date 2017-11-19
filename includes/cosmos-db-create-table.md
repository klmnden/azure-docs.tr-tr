Şimdi, veritabanı ve tablo oluşturmak için Azure portalında Veri Gezgini aracını kullanabilirsiniz. 

1. Tıklatın **Veri Gezgini** > **yeni tablo**. 
    
    **Tablo Ekle** alanı sağ ucundaki görüntülenir, hemen görmek için kaydırmanız gerekebilir.

    ![Azure portalında Veri Gezgini](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

2. İçinde **Tablo Ekle** sayfasında, yeni bir tablo ayarlarını girin.

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Tablo kimliği|sample-table|Yeni tablonuzun kimliği. Tablo adı karakter gereksinimleri, veritabanı kimliklerine ilişkin karakter gereksinimleri ile aynıdır. Veritabanı adı 1 ile 255 karakter arasında olmalı, `/ \ # ?` içermemeli ve boşlukla bitmemelidir.
    Depolama kapasitesi| Sabit (10 GB)|Bir değerle değiştirmek **sabit (10 GB)**. Bu değer, veritabanının depolama kapasitesidir.
    Aktarım hızı|400 RU|İşleme 400 istek birimleri (RU/s) saniyede değiştirin. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.

    **Tamam** düğmesine tıklayın.

    Veri Gezgini Yeni veritabanı ve tablo görüntüler.

    ![Azure portalında yeni bir veritabanı ve koleksiyonu gösteren Veri Gezgini](./media/cosmos-db-create-table/azure-cosmos-db-new-table.png)