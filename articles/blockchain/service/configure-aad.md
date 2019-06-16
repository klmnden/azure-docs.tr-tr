---
title: Azure Active Directory erişimini yapılandırma
description: Azure Blockchain hizmeti ile Azure Active Directory erişimi yapılandırma
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: seal
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: 616e342f1d52179c40c225c5dafc9de13ce85e06
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028223"
---
# <a name="how-to-configure-azure-active-directory-access"></a>Azure Active Directory erişimini yapılandırma

Bu makalede, erişim ve Azure Active Directory (Azure AD) kullanıcı, Grup veya uygulama kimlikleri kullanarak Azure blok zinciri hizmet düğümlere bağlanma hakkında bilgi edinin.

Azure AD bulut tabanlı kimlik yönetimi sağlar ve tüm kurumsal ve erişim uygulamalar azure'da arasında tek bir kimlik kullanmanıza olanak tanır. Azure Blockchain hizmeti, Azure AD ile tümleştirilmiş ve Kimlik Federasyonu ve çoklu oturum açma ve çok faktörlü kimlik doğrulaması gibi avantajlar sunar.

## <a name="prerequisites"></a>Önkoşullar

* [Azure portalını kullanarak bir blok zinciri üye oluştur](create-member.md)

## <a name="grant-access"></a>Erişim verme

Üye ve düğüm düzeyindeki hem erişim verebilirsiniz. Üye düzeyinde erişim hakkı verme sırayla üye altındaki tüm düğümleri erişmesine izin vermiş olursunuz.

### <a name="grant-member-level-access"></a>Üye düzeyi erişimi verme

Üye düzeyinde erişim izni vermek için.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Gidin **erişim denetimi (IAM) > Ekle > Rol ataması Ekle**.
1. Seçin **blok zinciri üye düğümü erişimi (Önizleme)** rolüne erişim vermek istediğiniz Azure AD kimliği nesne ekleyin. Azure AD kimliği nesne aşağıdakilerden biri olabilir:

    | Azure AD nesnesi | Örnek |
    |-----------------|---------|
    | Azure AD kullanıcı   | `frank@contoso.onmicrosoft.com` |
    | Azure AD grubu  | `sales@contoso.onmicrosoft.com` |
    | Uygulama Kimliği  | `13925ab1-4161-4534-8d18-812f5ca1ab1e` |

    ![Rol ataması Ekle](./media/configure-aad/add-role-assignment.png)

1. **Kaydet**’i seçin.

### <a name="grant-node-level-access"></a>Düğüm düzeyinde erişim izni ver

1. Düğümü güvenlik giderek düğüm düzeyinde erişim verebilir veya erişimi vermek istediğiniz düğüm adına tıklayın.
1. Blok zinciri üye düğümü erişimi (Önizleme) rolünü seçin ve erişim vermek istediğiniz Azure AD kimliği nesne ekleyin. 

## <a name="connect-using-azure-blockchain-connector"></a>Azure Blockchain bağlayıcısını kullanarak bağlan

İndirin veya kopyalayın [Azure blok zinciri Bağlayıcısı github'dan](https://github.com/Microsoft/azure-blockchain-connector/).

```bash
git clone https://github.com/Microsoft/azure-blockchain-connector.git
```

Hızlı Başlangıç konusundaki izleme **Benioku** kaynak kodunu bir bağlayıcı oluşturmak için.

### <a name="connect-using-an-azure-ad-user-account"></a>Bir Azure AD kullanıcı hesabını kullanarak bağlanın

1. Bir Azure AD kullanıcı hesabını kullanarak kimlik doğrulaması için aşağıdaki komutu çalıştırın. Değiştirin \<myAADDirectory\> ile bir Azure AD etki alanı. Örneğin, `yourdomain.onmicrosoft.com`.

    ```
    connector.exe -remote <myMemberName>.blockchain.azure.com:3200 -method aadauthcode -tenant-id <myAADDirectory> 
    ```

1. Azure AD kimlik bilgilerini ister.
1. Kullanıcı adı ve parolayla oturum açın.
1. Yerel bir ara sunucunuz kimlik başarıyla doğrulandıktan sonra blok zinciri düğümüne bağlanmış olursunuz. Şimdi Geth istemcinizi yerel uç nokta ile de ekleyebilirsiniz.

    ```bash
    geth attach http://127.0.0.1:3100
    ```

### <a name="connect-using-an-application-id"></a>Bir uygulama kimliği kullanarak bağlan

Birçok uygulama, bir Azure AD kullanıcı hesabı yerine bir uygulama Kimliğini kullanarak Azure AD ile kimlik doğrulaması.

Bir uygulama Kimliğini kullanarak, bir düğüme bağlanmak için değiştirin **aadauthcode** ile **aadclient**.

```
connector.exe -remote <myBlockchainEndpoint>  -method aadclient -client-id <myClientID> -client-secret "<myClientSecret>" -tenant-id <myAADDirectory>
```

| Parametre | Açıklama |
|-----------|-------------|
| Kiracı kimliği | Azure AD etki alanı, örneğin, `yourdomain.onmicrosoft.com`
| istemci kimliği | Azure AD'de kayıtlı uygulama istemci kimliği
| İstemci gizli anahtarı | Azure AD'de kayıtlı uygulama istemci gizli bilgisi

Bir uygulamayı Azure AD'ye kaydetme konusunda daha fazla bilgi için bkz. [nasıl yapılır: Bir Azure AD uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma](../../active-directory/develop/howto-create-service-principal-portal.md)

### <a name="connect-a-mobile-device-or-text-browser"></a>Mobil cihaz veya metin tarayıcı bağlanma

Azure AD, bir mobil cihaz veya metin tabanlı tarayıcı Azure AD kimlik doğrulaması açılır ekranın mümkün olduğu için bir kerelik geçiş kodu oluşturur. Geçiş kodunu kopyalayın ve başka bir ortamda Azure AD kimlik doğrulaması ile devam edin.

Geçiş kodu oluşturmak için değiştirin **aadauthcode** ile **aaddevice**. Değiştirin \<myAADDirectory\> ile bir Azure AD etki alanı. Örneğin, `yourdomain.onmicrosoft.com`.

```
connector.exe -remote <myBlockchainEndpoint>  -method aaddevice -tenant-id <myAADDirectory>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Blockchain Service'te veri güvenliği hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Azure Blockchain hizmet güvenliği](data-security.md)