## <a name="connect-to-outlookcom"></a>Outlook.com'da Bağlan
### <a name="prerequisites"></a>Ön koşullar
* Outlook.com hesabı

Bir mantıksal uygulama Outlook.com hesabınızı kullanmadan önce Outlook.com hesabınıza bağlanmak için mantığı uygulamasını yetkilendirmeniz gerekir. Neyse ki, Azure Portal'da mantıksal uygulama içinde bu kolayca yapabilirsiniz. 

Mantıksal uygulamanızı Outlook.com hesabınıza bağlanmak için yetki vermek için adımlar şunlardır:

1. Tüm mantıksal uygulamalar Tasarımcı açar, mantıksal uygulama oluşturma ve listesini görüntüler tetikler sonra mantıksal uygulamanızı başlatmak üzere kullanabilmeniz için bir tetikleyici tarafından başlatılması gerekir:
   
   ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Arama kutusuna "outlook" girin. Listenin adı "Outlook" ile tüm Tetikleyicileri listelemek için filtre dikkat edin:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Seçin **Office 365 Outlook - yeni e-posta**.   
   Outlook önce herhangi bir bağlantısı oluşturmadıysanız, Outlook.com kimlik bilgilerinizi sağlamanız istenir. Bu kimlik bilgileri bağlanmak için mantıksal uygulamanızı yetkilendirmek için kullanılır ve Outlook.com hesabınızın veri erişim:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Outlook için kimlik bilgilerinizi sağlayın ve oturum açın:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
   Bu kadar. Outlook bağlantı şimdi oluşturduğunuzu düşünün. Bu bağlantı, oluşturduğunuz herhangi bir mantıksal uygulama kullanmak için kullanılabilir.

