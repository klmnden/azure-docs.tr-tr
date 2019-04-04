---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: b7883de410a1fd281a154a792dd45132c08f0c03
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891131"
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API'si için bir belirteç almak için Microsoft kimlik doğrulama kitaplığı (MSAL) kullanma

Açık `ViewController.swift` ve kod ile değiştirin:

```swift
import UIKit
import MSAL

// A View Controller that will respond to the events of the Storyboard.
class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {

    // Replace Your_Application_Id_Here with the client ID you received in the portal. The below is for running the demo only.
    let kClientID = "Your_Application_Id_Here"

    // These settings you don't need to edit unless you wish to attempt deeper scenarios with the app.
    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    let kAuthority = "https://login.microsoftonline.com/common"

    var accessToken = String()
    var applicationContext : MSALPublicClientApplication?

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

    /**
        Setup public client application in viewDidLoad
    */

    override func viewDidLoad() {

        super.viewDidLoad()

        do {

            /**
             Initialize a MSALPublicClientApplication with a given clientID and authority
             - clientId:            The clientID of your application, you should get this from the app portal.
             - authority:           A URL indicating a directory that MSAL can use to obtain tokens. In Azure AD
                                    it is of the form https://<instance/<tenant>, where <instance> is the
                                    directory host (e.g. https://login.microsoftonline.com) and <tenant> is a
                                    identifier within the directory itself (e.g. a domain associated to the
                                    tenant, such as contoso.onmicrosoft.com, or the GUID representing the
                                    TenantID property of the directory)
             - error                The error that occurred creating the application object, if any, if you're
                                    not interested in the specific error pass in nil.
             */

            guard let authorityURL = URL(string: kAuthority) else {
                self.loggingText.text = "Unable to create authority URL"
                return
            }

            let authority = try MSALAuthority(url: authorityURL)
            self.applicationContext = try MSALPublicClientApplication(clientId: kClientID, authority: authority)

        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }

    override func viewWillAppear(_ animated: Bool) {

        super.viewWillAppear(animated)
        signoutButton.isEnabled = !self.accessToken.isEmpty
    }

    /**
     This button will invoke the authorization flow.
    */

    @IBAction func callGraphButton(_ sender: UIButton) {

        if self.currentAccount() == nil {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            self.acquireTokenInteractively()
        } else {
            self.acquireTokenSilently()
        }
    }

    func acquireTokenInteractively() {

        guard let applicationContext = self.applicationContext else { return }

        applicationContext.acquireToken(forScopes: kScopes) { (result, error) in

            if let error = error {

                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
    }

    func acquireTokenSilently() {

        guard let applicationContext = self.applicationContext else { return }

        /**
         Acquire a token for an existing account silently
         - forScopes:           Permissions you want included in the access token received
                                in the result in the completionBlock. Not all scopes are
                                guaranteed to be included in the access token returned.
         - account:             An account object that we retrieved from the application object before that the
                                authentication flow will be locked down to.
         - completionBlock:     The completion block that will be called when the authentication
                                flow completes, or encounters an error.
         */

        applicationContext.acquireTokenSilent(forScopes: kScopes, account: self.currentAccount()) { (result, error) in

            if let error = error {

                let nsError = error as NSError

                // interactionRequired means we need to ask the user to sign-in. This usually happens
                // when the user's Refresh Token is expired or if the user has changed their password
                // among other possible reasons.
                if (nsError.domain == MSALErrorDomain
                    && nsError.code == MSALErrorCode.interactionRequired.rawValue) {

                    DispatchQueue.main.async {
                        self.acquireTokenInteractively()
                    }

                } else {
                    self.updateLogging(text: "Could not acquire token silently: \(error)")
                }

                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
    }

    func currentAccount() -> MSALAccount? {

        guard let applicationContext = self.applicationContext else { return nil }

        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        do {

            let cachedAccounts = try applicationContext.allAccounts()

            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }

        } catch let error as NSError {

            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }

        return nil
    }
```

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

#### <a name="getting-a-user-token-interactively"></a>Kullanıcı belirtecini etkileşimli olarak alma

Çağırma `acquireToken` kullanıcıdan oturum açmak için bir tarayıcı penceresi yöntemi sonuçlanıyor. Uygulamalar genellikle etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmak bir kullanıcı gerektirir ya da bir belirteç başarısız (örneğin kullanıcı parolasının süresi doldu) almak için sessiz bir işlem.

#### <a name="getting-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

`acquireTokenSilent` Belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenileme yöntemi işler. Sonra `acquireToken` ilk kez yürütülür `acquireTokenSilent` veya belirteçleri yenileme isteği için çağrıları sessizce yapıldıkça yapılan sonraki çağrılar için-korunan kaynaklara erişim için kullanılan belirteçleri elde etmek için yaygın kullanılan yöntemdir.

Sonuç olarak, `acquireTokenSilent` – örneğin kullanıcı oturumunuz veya başka bir cihazda parolasını değiştirdiğinden başarısız olur. MSAL etkileşimli bir eylem gerektirerek sorun çözülebilir, harekete algıladığında bir `MSALErrorCode.interactionRequired` özel durum. Uygulamanız, bu özel durumun iki şekilde işleyebilir:

1. Karşı çağrı yapmak `acquireToken` hemen sonuçlanır kullanıcının oturum açmasını isteyen içinde. Bu düzen, genellikle çevrimiçi uygulamalarda kullanılır olduğunda çevrimdışı içerik uygulamada kullanıcı için kullanılabilir. Bu Kılavuzlu kurulum tarafından oluşturulan örnek uygulama bu deseni kullanır: uygulamayı yürütme eylemi ilk zamanında görebilirsiniz. Hiçbir kullanıcı, uygulamayı her zamankinden kullanıldığından `applicationContext.allAccounts().first` null bir değer içerir ve bir `MSALErrorCode.interactionRequired` özel durumu oluşturulur. Ardından kod çağırarak özel durumu işleyen `acquireToken` kullanıcının oturum açmasını isteyen içinde elde edilen.

2. Uygulamaları bir etkileşimli oturum açma kullanıcı oturum açmak için doğru zamanda seçebilir ya da uygulama yeniden deneyebilir gerekli olan, kullanıcı için bir görsel gösterimi de yapabilir `acquireTokenSilent` daha sonra. Bu genellikle kullanılan kullanıcı uygulamanın diğer işlevleri kesintiye olmadan kullanabilir - Örneğin, çevrimdışı içeriği uygulamada kullanılabilir olduğunda. Bu durumda, kullanıcı, korumalı kaynağa erişmeye veya güncel olmayan bilgileri yenilemek için oturum açmak istedikleri veya uygulamanızı yeniden denemeye karar verebilirsiniz karar verebilir `acquireTokenSilent` ağ zaman geri geçici olarak kullanılamaz durumda olmasından sonra.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yalnızca edindiğiniz belirteci kullanarak Microsoft Graph API çağırma

Aşağıda yeni yöntemi ekleyin `ViewController.swift`. Bu yöntem yapmak için kullanılan bir `GET` karşı kullanarak Microsoft Graph API isteği bir *HTTP yetkilendirme üst bilgisi*:

```swift
 func getContentWithToken() {

        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
    
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in

            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }

            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {

                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            self.updateLogging(text: "Result from Graph: \(result))")

        }.resume()
    }
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma

Bu örnek uygulamasında `getContentWithToken()` yöntemi bir HTTP yapmak için kullanılan `GET` istemek için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana döndürmesi. Bu yöntem alınan belirteç ekler *HTTP yetkilendirme üst bilgisi*. Bu örnek için Microsoft Graph API kaynaktır *bana* endpoint – kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Oturum kapatma ayarlama

Aşağıdaki yöntemi ekleyin `ViewController.swift` kullanıcının oturumunu kapatmaz için:

```swift 
 @IBAction func signoutButton(_ sender: UIButton) {

        guard let applicationContext = self.applicationContext else { return }

        guard let account = self.currentAccount() else { return }

        do {

            /**
             Removes all tokens from the cache for this application for the provided account
             - account:    The account to remove from the cache
             */

            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signoutButton.isEnabled = false

        } catch let error as NSError {

            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }

}
```

<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Oturum kapatma hakkında daha fazla bilgi

`signoutButton` Yöntemi kaldırır kullanıcı MSAL kullanıcı önbellekten – bu etkili bir şekilde bir belirteç almak için gelecekteki bir istek, yalnızca etkileşimli olarak denenirse başarılı olur böylece geçerli kullanıcının unutmak çok MSAL bildirir.

MSAL, tek bir kullanıcı bu örnek uygulamasında desteklemesine rağmen burada birden çok hesap aynı zamanda oturum – bir kullanıcı birden çok hesaba sahip olduğu bir e-posta uygulaması bir örnek olduğundan senaryolarını destekler.
<!--end-collapse-->

## <a name="register-the-callback"></a>Geri çağırma kaydı

Kullanıcının kimliğini doğrular, sonra tarayıcı kullanıcı yeniden uygulamaya yönlendirir. Bu geri çağırmayı kaydetmek için aşağıdaki adımları izleyin:

1. Açık `AppDelegate.swift` ve MSAL içeri aktarın:

```swift
import MSAL
```

<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Aşağıdaki yöntemi ekleyin, <code>AppDelegate</code> geri çağırmaları işlemek üzere sınıfı:
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if the URL matches the redirect URI for a pending AppAuth
// authorization request and if so, will look for the code in the response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```
