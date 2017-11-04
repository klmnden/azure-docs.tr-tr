#### <a name="prerequisites"></a>Ön koşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* Bir [Office 365](https://office365.com) hesabı  

Office 365 hesabınıza bir mantıksal uygulama kullanmadan önce Office 365 hesabınıza bağlanmaya mantıksal uygulama yetkilendirin. Kolayca Azure portalındaki mantıksal uygulama içinde bunu yapabilirsiniz.  

Aşağıdaki adımları kullanarak Office 365 hesabınıza bağlanmaya mantıksal uygulamanızı yetkilendirin.

1. Bir mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda seçin **Göster Microsoft yönetilen API'ler** açılır listesinde ve ardından arama kutusuna "office 365" girin. Tetikleyiciler veya Eylemler birini seçin:  
    ![Office 365 bağlantı oluşturma adım](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Daha önce Office 365 için herhangi bir bağlantısı oluşturmadıysanız, Office 365 kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
    ![Office 365 bağlantı oluşturma adım](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Seçin **oturum**, kullanıcı adı ve parolayı girin. Seçin **oturum**:  
    ![Office 365 bağlantı oluşturma adım](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Bu kimlik bilgileri bağlanmak ve Office 365 hesabınıza erişmek için mantıksal uygulamanızı yetkilendirmek için kullanılır. 
4. Bağlantıyı oluşturan dikkat edin. Şimdi, mantıksal uygulamanızı diğer adımlarla devam edin:   
    ![Office 365 bağlantı oluşturma adım](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

