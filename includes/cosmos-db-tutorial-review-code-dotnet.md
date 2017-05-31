Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. DocumentDBRepository.cs dosyasını açtığınızda DocumentDB kaynaklarını bu kod satırlarının oluşturduğunu göreceksiniz. 

* DocumentClient başlatılır.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Yeni bir veritabanı oluşturulur.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Yeni bir koleksiyon oluşturulur.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```
