
## <a name="register-your-application"></a>Uygulamanızı kaydetme
Uygulamanızı iki yoldan biriyle kaydedebilirsiniz.

### <a name="option-1-express-mode"></a>Seçenek 1: Hızlı mod
Aşağıdakileri yaparak, uygulamanızın hızlı bir şekilde kaydedebilirsiniz:
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).

2. Seçin **bir uygulama ekleyin**.

3. İçinde **uygulama adı** kutusuna, uygulamanız için bir ad girin.

4. Emin **destekli Kurulum** onay kutusu seçili ve ardından **oluşturma**.

5. Uygulama Kimliği alma yönergeleri izleyin ve kodunuza yapıştırın.

### <a name="option-2-advanced-mode"></a>Seçenek 2: Gelişmiş mod
Uygulamanızı kaydetme ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:
1. Uygulamanızı kaydolmadıysanız Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app).

2. Seçin **bir uygulama ekleyin**.

3. İçinde **uygulama adı** kutusuna, uygulamanız için bir ad girin. 

4. Emin **destekli Kurulum** onay kutusunun temizlenmiş ve ardından **oluşturma**.

5. Seçin **eklemek Platform**seçin **yerel uygulama**ve ardından **kaydetmek**.

6. İçinde **uygulama kimliği** kutusunda, GUID kopyalayın.

7. Visual Studio, açık gidin *App.xaml.cs* dosya ve sonra değiştirmek `your_client_id_here` yalnızca kayıtlı ve kopyaladığınız uygulama kimliği.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
