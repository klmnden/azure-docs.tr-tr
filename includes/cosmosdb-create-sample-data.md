Şimdi Veri Gezgini'ni kullanarak yeni koleksiyonunuza veri ekleyebilirsiniz.

1. Yeni veritabanı, Veri Gezgini'nin Koleksiyonlar bölmesinde görüntülenir. **Öğeler** veritabanını genişletin, **ToDoList** koleksiyonunu genişletin, **Belgeler**'e ve ardından **Yeni Belgeler**'e tıklayın. 

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/cosmosdb-create-sample-data/azure-cosmosdb-data-explorer-emulator-new-document.png)
  
2. Şimdi aşağıdaki yapıyı kullanarak koleksiyona birkaç yeni belge ekleyin. Burada her belge için benzersiz bir kimlik girin ve diğer özellikleri dilediğiniz şekilde değiştirin. Azure Cosmos DB, verilerinizin bir şemaya uygun olmasını şart koşmadığı için yeni belgelerinizin yapısını istediğiniz şekilde oluşturabilirsiniz.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries."
     }
     ```

     Şimdi verilerinizi almak için Veri Gezgini'ndeki sorguları kullanabilirsiniz. Veri Gezgini koleksiyondaki tüm belgeleri almak için varsayılan olarak SELECT * FROM komutunu kullanır, ancak bunu SELECT * FROM c ORDER BY c.name ASC komutuyla değiştirerek tüm belgelerin ad özelliğine göre alfabetik sırada döndürülmesini sağlayabilirsiniz. 
 
     Veri Gezgini'ni kullanarak ayrıca saklı yordamlar, UDF'ler ve tetikleyiciler oluşturabilir, bu sayede sunucu tarafı iş mantığını gerçekleştirebilirsiniz. Veri Gezgini, API'lerdeki tüm yerleşik programlı veri erişimini açığa çıkarır ancak Azure portalındaki verilerinize kolayca erişmenizi sağlar.