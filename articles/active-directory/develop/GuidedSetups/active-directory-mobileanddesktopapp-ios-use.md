---
title: "Azure AD v2 iOS Başlarken - kullanım | Microsoft Docs"
description: "İOS (Swift) uygulamaları, Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 2ac1117a31a101705539a1f75520ce8de43809a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak üzere Microsoft kimlik doğrulama kitaplığı (MSAL) kullanın

Açık `ViewController.swift` ve kod ile değiştirin:

```swift
import UIKit
import MSAL

class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    let kClientID = "Your_Application_Id_Here"
    let kAuthority = "https://login.microsoftonline.com/common/v2.0"

    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    
    var accessToken = String()
    var applicationContext = MSALPublicClientApplication.init()

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

     // This button will invoke the call to the Microsoft Graph API. It uses the
     // built in Swift libraries to create a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check to see if we have a current logged in user. If we don't, then we need to sign someone in.
            // We throw an interactionRequired so that we trigger the interactive signin.
            
            if  try self.applicationContext.users().isEmpty {
                throw NSError.init(domain: "MSALErrorDomain", code: MSALErrorCode.interactionRequired.rawValue, userInfo: nil)
            } else {
            
            // Acquire a token for an existing user silently
            
            try self.applicationContext.acquireTokenSilent(forScopes: self.kScopes, user: applicationContext.users().first) { (result, error) in
    
                    if error == nil {
                        self.accessToken = (result?.accessToken)!
                        self.loggingText.text = "Refreshing token silently)"
                        self.loggingText.text = "Refreshed Access token is \(self.accessToken)"
                        
                        self.signoutButton.isEnabled = true;
                        self.getContentWithToken()
    
                    } else {
                        self.loggingText.text = "Could not acquire token silently: \(error ?? "No error information" as! Error)"
    
                    }
                }
            }
        }  catch let error as NSError {
            
            // interactionRequired means we need to ask the user to sign-in. This usually happens
            // when the user's Refresh Token is expired or if the user has changed their password
            // among other possible reasons.
            
            if error.code == MSALErrorCode.interactionRequired.rawValue {
                
                self.applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in
                        if error == nil {
                            self.accessToken = (result?.accessToken)!
                            self.loggingText.text = "Access token is \(self.accessToken)"
                            self.signoutButton.isEnabled = true;
                            self.getContentWithToken()
                            
                        } else  {
                            self.loggingText.text = "Could not acquire token: \(error ?? "No error information" as! Error)"
                        }
                }
                
            }
            
        } catch {
            
            // This is the catch all error.
            
            self.loggingText.text = "Unable to acquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable to create Application Context. Error: \(error)"
        }
    }


    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        
        if self.accessToken.isEmpty {
            signoutButton.isEnabled = false; 
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi
#### <a name="getting-a-user-token-interactively"></a>Kullanıcı etkileşimli olarak belirteci alma
Çağırma `acquireToken` yöntemi, oturum açmak için kullanıcıdan bir tarayıcı penceresi sonuçlanıyor. Uygulamalar genellikle gerektirir etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmak bir kullanıcı ya da (örneğin kullanıcının parolasının süresi) belirteci başarısız edinmeye sessiz bir işlem.

#### <a name="getting-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
`acquireTokenSilent` Yöntemi belirteci satın almalar ve herhangi bir kullanıcı etkileşimi olmadan yenileme işler. Sonra `acquireToken` ilk kez yürütüldüğünde `acquireTokenSilent` veya belirteçleri yenileme isteği için çağrıları sessizce gerçekleştirilmediğinden sonraki çağrılar için-korumalı kaynaklara erişmek için kullanılan belirteçleri elde etmek için yaygın olarak kullanılan yöntem.

Sonuç olarak, `acquireTokenSilent` başarısız olur – örneğin kullanıcı oturumunuz veya başka bir aygıtta parolalarını değişti. MSAL algıladığında, etkileşimli bir eylem gerektirmeyen tarafından sorunu çözmek için harekete bir `MSALErrorCode.interactionRequired` özel durum. Uygulamanız bu özel durumun iki yolla işleyebilir:

1.  Karşı çağırmaya `acquireToken` hemen hangi sonuçları oturum açmak için kullanıcıdan içinde. Bu deseni genelde çevrimiçi uygulamalarda kullanılır söz konusu olduğunda çevrimdışı içerik uygulamada kullanıcı için kullanılabilir. Bu desen destekli bu kurulum tarafından oluşturulan örnek uygulama kullanır: uygulama yürütme eylemi ilk zamanında görebilirsiniz. Hiçbir kullanıcı, uygulamayı her zamankinden kullanıldığından `applicationContext.users().first` bir null değer içerir ve bir ` MSALErrorCode.interactionRequired ` özel durum. Ardından örnek kodda çağırarak özel durumu işler `acquireToken` oturum açmak için kullanıcıdan içinde sonuçlanır.

2.  Uygulamalar bir etkileşimli oturum açma kullanıcı oturum açmak için doğru zamanı seçebilir ya da uygulama deneyebilirsiniz gerekli olan kullanıcı için görsel bir gösterge de yapabilirsiniz `acquireTokenSilent` daha sonra. Bu genellikle kullanılan kullanıcı kesintiye olmadan diğer uygulamanın işlevselliğini kullanabilir - Örneğin, çevrimdışı içeriği uygulamada kullanılabilir olduğunda. Bu durumda, kullanıcı ne zaman korumalı kaynağa erişmek için ya da eski bilgileri yenilemek için oturum açmak istedikleri veya uygulamanızın denemeye karar verebilirsiniz karar verebilir `acquireTokenSilent` ağ zaman geri geçici olarak kullanılamıyor kaldıktan sonra.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle kullanarak Microsoft Graph API çağrısı

Aşağıda yeni bir yöntem ekleyin `ViewController.swift`. Bu yöntem yapmak için kullanılan bir `GET` Microsoft grafik API'sini kullanarak karşı istek bir *HTTP Authorization Üstbilgisi*:

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify the Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
    request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
    let urlSession = URLSession(configuration: sessionConfig, delegate: self, delegateQueue: OperationQueue.main)
    
    urlSession.dataTask(with: request) { data, response, error in
        let result = try? JSONSerialization.jsonObject(with: data!, options: [])
                    if result != nil {
                
                self.loggingText.text = result.debugDescription
            }
        }.resume()
}
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma

Bu örnek uygulamasında `getContentWithToken()` yöntemi bir HTTP yapmak için kullanılan `GET` isteği için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana dönün. Bu yöntem alınan belirteç ekler *HTTP Authorization Üstbilgisi*. Bu örnek için Microsoft Graph API kaynaktır *bana* endpoint – kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Oturum kapatma ayarlama

Aşağıdaki yöntemi ekleyin `ViewController.swift` kullanıcı imzalamak için:

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from the cache for this application for the provided user
        // first parameter:   The user to remove from the cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Oturum kapatma hakkında daha fazla bilgi

`signoutButton` Yöntemi kaldırır kullanıcı MSAL kullanıcı önbellekten – bu etkili bir şekilde etkileşimli olarak yapılırsa bir belirteç almak için gelecekteki bir isteği yalnızca başarılı şekilde geçerli kullanıcının unuttunuz MSAL söyler.

Bu örnek uygulamasında tek bir kullanıcı desteklese de MSAL nerede birden fazla hesap aynı zamanda oturum açmadan – bir e-posta uygulamasının bir kullanıcı birden fazla hesap sahip olduğu örneğidir senaryolarını destekler.
<!--end-collapse-->

## <a name="register-the-callback"></a>Kaydı geri çağırma

Kullanıcının kimliğini doğrular sonra tarayıcı kullanıcı yeniden uygulamaya yönlendirir. Bu geri çağırma kaydetmek için aşağıdaki adımları izleyin:

1.  Açık `AppDelegate.swift` ve MSAL içeri aktarın:

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

