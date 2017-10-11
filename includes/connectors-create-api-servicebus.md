### <a name="prerequisites"></a>Ön koşullar
Bilmeniz gereken bir [Service Bus](https://azure.microsoft.com/services/service-bus/) hesabı.  

Bir mantıksal uygulama Azure Service Bus hesabınızı kullanmadan önce service bus hesabınıza bağlanmak için mantığı uygulamasını yetkilendirmeniz gerekir. Neyse ki, mantıksal uygulamanızı Azure portalındaki içinde bu kolayca yapabilirsiniz.  

Hizmet veri yolu hesabınıza bağlanmak için mantıksal uygulamanızı yetkilendirmek için adımlar şunlardır:  

1. Mantıksal Uygulama Tasarımcısı'nda hizmet veri yolu, bir bağlantı oluşturmak için seçin **Göster Microsoft yönetilen API'ler** aşağı açılan listesinde. Enter **hizmet veri yolu** arama kutusuna. Tetikleyici veya kullanmak istediğiniz eylemi seçin.  
    ![Hizmet veri yolu bağlantı görüntü 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Önce Service Bus bağlantılarına oluşturmadıysanız, hizmet veri yolu kimlik bilgilerinizi sağlamanız istenir. Bu kimlik bilgileri, mantıksal uygulamanızı bağlanmak ve Service Bus hesabınızın veri erişimi için yetkilendirmek için kullanılır. Hizmet veri yolu Bağlayıcısı, hizmet veri yolu ad alanı için bağlantı dizesi olması gerekir. Ayrıca gerektirir **Yönet** izinleri. Bağlantı dizesi için ad alanı olması veya belirli bir varlık içerip içermediğini öğrenmek için en iyi yolu `EntityPath` parametresi. Bulursa, bir mantıksal uygulama için doğru bağlantı dizesi değil.  
    ![Hizmet veri yolu bağlantı dizesi](./media/connectors-create-api-servicebus/connectionstring.png)
3. Ad alanı için bağlantı dizesi aldıktan sonra Logic Apps, API bağlantısı için kullanabilirsiniz.  
    ![Hizmet veri yolu bağlantı görüntü 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Bağlantı oluşturuldu ve artık bir mantıksal uygulamanızı adımlarda yüklemeye devam etmek ücretsiz dikkat edin.  
    ![Hizmet veri yolu bağlantı görüntü 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

