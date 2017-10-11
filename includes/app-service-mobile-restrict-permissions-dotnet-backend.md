
Varsayılan olarak, Mobile Apps arka uç API'leri anonim olarak çağrılabilir. Ardından, yalnızca kimliği doğrulanmış kullanıcıların erişimi kısıtlamak gerekir.  

* **Node.js geri bitiş (Azure portalı üzerinden)** :  

    Mobile Apps ayarlarınızı tıklatın **kolay tabloları** ve tablonuzu seçin. Tıklatın **izinleri değiştirme**seçin **kimlik doğrulamalı erişimi yalnızca** tüm izinleri ve ardından **kaydetmek**.
* **.NET geri bitiş (C#)**:  

    Sunucu projesi gidin **denetleyicileri** > **TodoItemController.cs**. Ekleme `[Authorize]` özniteliğini **TodoItemController** sınıfı, şu şekilde. Yalnızca belirli yöntemler için erişimi kısıtlamak için bu yöntemlerden sınıfın yerine tam da bu öznitelik uygulayabilirsiniz. Sunucu projesi yeniden yayımlayın.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Node.js arka ucu (üzerinden Node.js kodu)** :  

    Tablo erişmesi için kimlik doğrulaması için Node.js sunucu komut dosyasına aşağıdaki satırı ekleyin:

        table.access = 'authenticated';

    Daha fazla ayrıntı için bkz: [nasıl yapılır: tablolara erişimi için kimlik doğrulaması gerektiren](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Hızlı Başlangıç kodunu projeyi sitenizden indirmek öğrenmek için bkz: [nasıl yapılır: Git kullanarak Node.js arka uç hızlı başlangıç kod projesi indirme](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
