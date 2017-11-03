#### <a name="prerequisites"></a>Ön koşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* A [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) hesabı 

Bir mantıksal uygulama Dynamics hesabınızı kullanmadan önce CRM Online hesabınıza bağlanmak için mantıksal uygulama yetkilendirin. Kolayca Azure portalındaki mantıksal uygulama içinde bunu yapabilirsiniz. 

Aşağıdaki adımları kullanarak CRM Online hesabınıza bağlanmak için mantıksal uygulamanızı yetkilendirin.

1. Bir mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda seçin **Göster Microsoft yönetilen API'ler** açılır listesinde ve ardından arama kutusuna "dynamics" girin. Tetikleyiciler veya Eylemler birini seçin:  
   ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Dynamics bağlantılarına daha önce oluşturmadıysanız, Dynamics kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
   ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Seçin **oturum**, kullanıcı adı ve parolayı girin. Seçin **oturum**. 
   
    Bu kimlik bilgileri bağlanın ve Dynamics hesabınızdaki verilere erişmek için mantıksal uygulamanızı yetkilendirmek için kullanılır. 
4. Bağlantıyı oluşturan dikkat edin. Şimdi, mantıksal uygulamanızı diğer adımlarla devam edin:  
   ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

