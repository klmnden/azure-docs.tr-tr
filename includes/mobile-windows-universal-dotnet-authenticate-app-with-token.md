---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: d71d52257b6e8cfa243207c9bfdb5c7de7d3dd37
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891116"
---
1. MainPage.xaml.cs proje dosyasında, aşağıdaki ekleyin **kullanarak** ifadeleri:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Değiştirin **AuthenticateAsync** yöntemini aşağıdaki kod ile:
   
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
                // expired, as shown in this post: https://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Sign in with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider, "{url_scheme_of_your_app}");
   
                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now signed in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must sign in. Sign-In Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    Bu sürümünde **AuthenticateAsync**, depolanan kimlik bilgilerini kullanmak uygulamayı çalışır **PasswordVault** hizmete erişmek için. Depolanan bir kimlik bilgisi olduğunda bir normal oturum açma da gerçekleştirilir.
   
   > [!NOTE]
   > Önbelleğe alınan belirteç süresi dolmuş olabilir ve uygulama kullanımda olduğunda belirteci süre sonu kimlik doğrulamasından sonra da meydana gelebilir. Belirtecin süresi olmadığını belirlemek öğrenmek için bkz. [denetlemek için süresi dolan kimlik doğrulama belirteci](https://aka.ms/jww5vp). Süresi dolan belirteçleri ile ilgili yetkilendirme hataları işleme için bir çözüm için gönderiye bakın [önbelleğe alma ve Azure Mobile Services belirteçlerin süresinin işleme yönetilen SDK](https://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 
   > 
   > 
3. Uygulamayı iki kez yeniden başlatın.
   
    İlk başlatma oturum açma sağlayıcısı ile yeniden gerekli olduğuna dikkat edin. Ancak, önbelleğe alınan kimlik bilgileri ikinci bir yeniden başlatma sırasında kullanılır ve oturum açma atlanır. 

