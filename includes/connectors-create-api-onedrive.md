#### <a name="prerequisites"></a>Ön koşullar
* Bir Azure hesabı; oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free)
* A [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) hesabı 

Bir mantıksal uygulama OneDrive hesabınıza kullanmadan önce OneDrive hesabınıza bağlanmak için mantıksal uygulama yetkilendirin.  Kolayca Azure portalındaki mantıksal uygulama içinde bunu yapabilirsiniz. 

Aşağıdaki adımları kullanarak OneDrive hesabınıza bağlanmak için mantıksal uygulamanızı yetkilendirin.

1. Bir mantıksal uygulama oluşturun. Logic Apps Tasarımcısı'nda seçin **Göster Microsoft yönetilen API'ler** açılır listesinde ve ardından arama kutusuna "onedrive" girin. Tetikleyiciler veya Eylemler birini seçin:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Daha önce onedrive herhangi bir bağlantısı oluşturmadıysanız, OneDrive kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Seçin **oturum**, kullanıcı adı ve parolayı girin. Seçin **oturum**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Bu kimlik bilgileri bağlanmak ve OneDrive hesabınızdaki verilere erişmek için mantıksal uygulamanızı yetkilendirmek için kullanılır. 
4. Seçin **Evet** OneDrive hesabınızı kullanmak için mantıksal uygulama yetkilendirmek için:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Bağlantıyı oluşturan dikkat edin. Şimdi, mantıksal uygulamanızı diğer adımlarla devam edin:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

