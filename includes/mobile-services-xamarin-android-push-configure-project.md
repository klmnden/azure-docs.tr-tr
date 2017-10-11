
1. Çözüm görünümünde (veya **Çözüm Gezgini** Visual Studio'da), sağ tıklatın **bileşenleri** klasörünü tıklatın **daha almak bileşenleri...** , arama **Google Cloud Messaging istemcisi** bileşeni ve bunu projeye ekleyin.
2. ToDoActivity.cs proje dosyasını açın ve aşağıdakileri ekleyin sınıfı deyimiyle:
   
        using Gcm.Client;
3. İçinde **ToDoActivity** sınıfında, aşağıdaki yeni kodu ekleyin: 
   
        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();
   
        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }
   
    Bu, mobil istemci örneği itme işleyici hizmeti işleminin erişmenize olanak tanır.
4. Aşağıdaki kodu ekleyin **OnCreate** yöntemi, sonra **MobileServiceClient** oluşturulur:
   
       // Set the current instance of TodoActivity.
       instance = this;
   
       // Make sure the GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register the app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

**ToDoActivity** anında iletme bildirimleri eklemek için şimdi hazırlanır.

