---
title: Azure AD B2C'de kaynak sahibi parolası kimlik bilgileri akışını yapılandırma | Microsoft Docs
description: Azure AD B2C'de kaynak sahibi parolası kimlik bilgileri akışını yapılandırmayı öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/24/2018
ms.author: davidmu
ms.openlocfilehash: 4a052da4a6a16fde0583b3a1dbecc2a6de5cc0a9
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-the-resource-owner-password-credentials-flow-ropc-in-azure-ad-b2c"></a>Azure AD B2C'de kaynak sahibi parolası kimlik bilgileri akışını (ROPC) yapılandırma

Kaynak sahibi parolası kimlik bilgilerini (ROPC) bir OAUTH standart kimlik doğrulama akışı akışıdır burada uygulama bağlı olan taraf, kullanıcı kimliği ve parolası kimliği belirteci, erişim belirteci ve bir yenileme belirteci gibi alışverişleri geçerli kimlik bilgileri olarak da biliyor. 

> [!NOTE]
> Bu özelliğin önizlemede değil.

Azure AD B2C'de, bu seçenekleri desteklenir:

- **Native Client** – kimlik doğrulaması sırasında kullanıcı etkileşimi olur Android gibi yerel işletim sistemi çalıştıran veya tarayıcısında Javascript gibi çalışan bir mobil uygulama olabilir bir kullanıcı tarafı cihazda çalışan kodu kullanarak.
- **Ortak istemci akışı** – yalnızca bir uygulama tarafından toplanan kullanıcı kimlik gönderilir API çağrısında. Kimlik bilgileri uygulamanın gönderilmez.
- **Yeni Talep ekleme** -kimliği belirteç içerikleri, yeni talepleri eklemek için değiştirilebilir. 

Bu akışlar desteklenmez:

- **Sunucudan sunucuya** kimlik koruması sistemi (IDPS) arayan (yerel istemci) etkileşim bir parçası olarak toplanan güvenilir bir IP adresi gerekiyor.  Bir sunucu tarafı API çağrısı başarısız kimlik doğrulama Dinamik Eşik aşılırsa IDPS saldırgan yinelenen bir IP adresi tanımlayabilir ve yalnızca sunucunun IP adresi kullanılır.
- **Gizli istemci akışı** - uygulama istemci kimliği doğrulandı, ancak uygulama gizli anahtarı doğrulanmamış.

##  <a name="create-a-resource-owner-policy"></a>Kaynak sahibi ilkesi oluşturma

1. Azure portalında Azure AD B2C kiracınızın genel yönetici olarak oturum açın.
2. Azure AD B2C kiracınıza geçiş yapmak için portalın sağ üst köşesinden B2C dizinini seçin.
3. Altında **ilkeleri**seçin **kaynak sahibi ilkeleri**.
4. İlke için bir ad sağlayın *ROPC_Auth*ve ardından **uygulama talepleri**.
5. Uygulamanız için gibi gereksinim uygulama talepleri seçmek *görünen adı*, *e-posta adresi*, ve *kimlik sağlayıcısı*.
6. Tıklatın **Tamam**ve ardından **oluşturma**.

Ardından, bu örnek gibi bir uç nokta görürsünüz:

`https://login.microsoftonline.com/yourtenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1A_ROPC_Auth`


## <a name="register-an-application"></a>Bir uygulamayı kaydetme

1. B2C ayarlarında **Uygulamalar**’a tıklayın ve ardından **+ Ekle**’ye tıklayın.
2. Uygulama için bir ad sağlayın *ROPC_Auth_app*.
3. Tıklatın **Hayır** için **Web uygulaması/Web API** tıklatıp **Evet** için **Native client**.
4. Bunlar ve tıklatın gibi diğer tüm değerler bırakın **oluşturma**.
5. Yeni uygulama seçin ve uygulama kimliğini not edin

## <a name="test-the-policy"></a>İlkeyi test etme

Bir API çağrısı oluşturmak için sık kullanılan API geliştirme uygulamanızın kullanın ve ilkeniz hata ayıklamak için yanıt gözden geçirin. Böyle bir çağrı POST isteğinin gövdesi olarak aşağıdaki tablodaki bilgilerle oluşturun:
- Değiştir *yourtenant.onmicrosoft.com* B2C kiracınızın adı
- Değiştir *B2C_1A_ROPC_Auth* ROPC ilkeniz tam adı
- Değiştir *bef2222d56-552f-4a5b-b90a-1988a7d634c3* kaydınızı uygulama kimliği ile.

`https://te.cpim.windows.net/yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token`

| Anahtar | Değer |
| --- | ----- |
| kullanıcı adı | leadiocl@outlook.com |
| password | Passxword1 |
| grant_type | password |
| scope | openıd bef2222d56-552f-4a5b-b90a-1988a7d634c3 offline_access |
| client_id | bef2222d56-552f-4a5b-b90a-1988a7d634c3 |
| response_type | belirteç id_token |

*Client_id* uygulama kimliği olarak daha önce not ettiğiniz değerdir *Offline_access* bir yenileme belirteci almak istiyorsanız, isteğe bağlıdır. 

Fiili POST isteği şöyle görünür:

```
POST /yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token HTTP/1.1
Host: te.cpim.windows.net
Content-Type: application/x-www-form-urlencoded

username=leadiocl%40trashmail.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```


Çevrimdışı erişim ile başarılı bir yanıt aşağıdaki örneğe benzer:

```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJpc3MiOiJodHRwczovL3RlLmNwaW0ud2luZG93cy5uZXQvZjA2YzJmZTgtNzA5Zi00MDMwLTg1ZGMtMzhhNGJmZDllODJkL3YyLjAvIiwiZXhwIjoxNTEzMTMwMDc4LCJuYmYiOjE1MTMxMjY0NzgsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsIm9pZCI6IjNjM2I5NjljLThjNDktNGUxMS1hNGVmLWZkYjJmMzkyZjA0OSIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF6cCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsInZlciI6IjEuMCIsImlhdCI6MTUxMzEyNjQ3OH0.MSEThYZxCS4SevBw3-3ecnVLUkucFkehH-gH-P7SFcJ-MhsBeQEpMF1Rzu_R9kUqV3qEWKAPYCNdZ3_P4Dd3a63iG6m9TnO1Vt5SKTETuhVx3Xl5LYeA1i3Slt9Y7LIicn59hGKRZ8ddrQzkqj69j723ooy01amrXvF6zNOudh0acseszt7fbzzofyagKPerxaeTH0NgyOinLwXu0eNj_6RtF9gBfgwVidRy9OzXUJnqm1GdrS61XUqiIUtv4H04jYxDem7ek6E4jsH809uSXT0iD5_4C5bDHrpO1N6pXSasmVR9GM1XgfXA_IRLFU4Nd26CzGl1NjbhLnvli2qY4A", 
    "token_type": "Bearer", 
    "expires_in": "3600", 
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6IjEuMCJ9.aJ_2UW14dh4saWTQ0jLJ7ByQs5JzIeW_AU9Q_RVFgrrnYiPhikEc68ilvWWo8B20KTRB_s7oy_Eoh5LACsqU6Oz0Mjnh0-DxgrMblUOTAQ9dbfAT5WoLZiCBJIz4YT5OUA_RAGjhBUkqGwdWEumDExQnXIjRSeaUBmWCQHPPguV1_5wSj8aW2zIzYIMbofvpjwIATlbIZwJ7ufnLypRuq_MDbZhJkegDw10KI4MHJlJ40Ip8mCOe0XeJIDpfefiJ6WQpUq4zl06NO7j8kvDoVq9WALJIao7LYk_x9UIT-3d0W0eDBHGSRcNgtMYpymaN9ltx6djcEesXNn4CFnWG3g.y6KKeA9EcsW9zW-g.TrTSgn4WBt18gezegxihBla9SLSTC3YfDROQsL9K4yX4400FKlTlf-2l9CnpGTEdWXVi7sIMHCl8S4oUiXd-rvY2mn_NfDrbbVJfgKp1j7Nnq9FFyeJEFcP_FtUXgsNTG9iwfzWox04B1d845qNRWiS9N8BhAAAIdz5N0ChHuOxsVOC0Y_Ly3DNe-JQyXcq964M6-jp3cgi4UqMxT837L6pLY5Ih_iPsSfyHzstsFeqyUIktnzt1MpTlyW-_GDyFK1S-SyV8PPQ7phgFouw2jho1iboHX70RlDGYyVmP1CfQzKE_zWxj3rgaCZvYMWN8fUenoiatzhvWkUM7dhqKGjofPeL8rOMkhl6afLLjObzhUg3PZFcMR6guLjQdEwQFufWxGjfpvaHycZSKeWu6-7dF8Hy_nyMLLdBpUkdrXPob_5gRiaH72KvncSIFvJLqhY3NgXO05Fy87PORjggXwYkhWh4FgQZBIYD6h0CSk2nfFjR9uD9EKiBBWSBZj814S_Jdw6HESFtn91thpvU3hi3qNOi1m41gg1vt5Kh35A5AyDY1J7a9i_lN4B7e_pknXlVX6Z-Z2BYZvwAU7KLKsy5a99p9FX0lg6QweDzhukXrB4wgfKvVRTo.mjk92wMk-zUSrzuuuXPVeg", 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJleHAiOjE1MTMxMzAwNzgsIm5iZiI6MTUxMzEyNjQ3OCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly90ZS5jcGltLndpbmRvd3MubmV0L2YwNmMyZmU4LTcwOWYtNDAzMC04NWRjLTM4YTRiZmQ5ZTgyZC92Mi4wLyIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsImFjciI6ImIyY18xYV9yZXNvdXJjZW93bmVydjIiLCJpYXQiOjE1MTMxMjY0NzgsImF1dGhfdGltZSI6MTUxMzEyNjQ3OCwib2lkIjoiM2MzYjk2OWMtOGM0OS00ZTExLWE0ZWYtZmRiMmYzOTJmMDQ5IiwiYXRfaGFzaCI6Ikd6QUNCTVJtcklwYm9OdkFtNHhMWEEifQ.iAJg13cgySsdH3cmoEPGZB_g-4O8KWvGr6W5VzRXtkrlLoKB1pl4hL6f_0xOrxnQwj2sUgW-wjsCVzMc_dkHSwd9QFZ4EYJEJbi1LMGk2lW-PgjsbwHPDU1mz-SR1PeqqJgvOqrzXo0YHXr-e07M4v4Tko-i_OYcrdJzj4Bkv7ZZilsSj62lNig4HkxTIWi5Ec2gD79bPKzgCtIww1KRnwmrlnCOrMFYNj-0T3lTDcXAQog63MOacf7OuRVUC5k_IdseigeMSscrYrNrH28s3r0JoqDhNUTewuw1jx0X6gdqQWZKOLJ7OF_EJMP-BkRTixBGK5eW2YeUUEVQxsFlUg" 
} 
```

## <a name="redeem-a-refresh-token"></a>Bir yenileme belirteci kullanın

Aşağıdaki tabloda bulunan bilgileri istek gövdesi olarak şuna benzer bir POST çağrısına oluşturun:

`https://te.cpim.windows.net/yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token`

| Anahtar | Değer |
| --- | ----- |
| grant_type | refresh_token |
| response_type | id_token |
| client_id | bef2222d56-552f-4a5b-b90a-1988a7d634c3 |
| kaynak | bef2222d56-552f-4a5b-b90a-1988a7d634c3 |
| refresh_token | eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3... |

*Client_id* ve *kaynak* uygulama kimliği olarak daha önce not ettiğiniz değerleri *Refresh_token* daha önce bahsedilen kimlik doğrulaması çağrısında alınan bir belirteç.

## <a name="implement-with-your-preferred-native-sdk-or-use-app-auth"></a>Tercih edilen yerel SDK'sı ile nasıl uygulanacağını veya uygulama kimlik doğrulama kullanın


Azure AD B2C uygulamasında OAuth 2.0 standartları veya ortak istemci ROPC karşıladığından ve çoğu istemci SDK'ları ile uyumlu olması gerekir.  Bu akış yaygın, AppAuth iOS ve Android için AppAuth üretimde Test ettiğimiz.  Bkz: https://appauth.io/ en son bilgiler için.

Adresindeki github'dan Azure AD B2C ile kullanmak için yapılandırılan çalışma örnekleri indirebilirsiniz https://aka.ms/aadb2cappauthropc Android için ve https://aka.ms/aadb2ciosappauthropc.




