
1. MainPage.xaml.cs proje dosyasında aşağıdaki ekleyin **kullanarak** deyimleri:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Değiştir **AuthenticateAsync** aşağıdaki kod ile yöntemi:
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);
   
                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    Bu sürümünde **AuthenticateAsync**, uygulama içinde depolanan kimlik bilgilerini kullanmayı dener **PasswordVault** hizmete erişmek için. Hiçbir depolanmış kimlik bilgisi olduğunda bir normal oturum açma da gerçekleştirilir.
   
   > [!NOTE]
   > Önbelleğe alınan bir belirteç süresi dolmuş olabilir ve uygulama kullanımda olduğunda belirteci süre sonu kimlik doğrulamasından sonra da oluşabilir. Belirtecin süresi varsa belirlemek öğrenmek için bkz: [denetlemek için süresi dolmuş kimlik doğrulama belirteçleri](http://aka.ms/jww5vp). Post süresi dolan belirteçleri ile ilgili yetkilendirme hataları işleme için bir çözüm için bkz: [önbelleğe alma ve Azure Mobile Services belirteçlerin süresinin işleme yönetilen SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 
   > 
   > 
3. Uygulamayı iki kez yeniden başlatın.
   
    İlk başlatma oturum açma sağlayıcısı ile yeniden gerekli olduğuna dikkat edin. Ancak, önbelleğe alınmış kimlik bilgilerini ikinci bir yeniden başlatma sırasında kullanılır ve oturum açma atlanır. 

