---
title: Azure Cosmos DB anahtarlara erişmek ve depolamak için Key Vault'u kullanın | Microsoft Docs
description: Depolama ve Azure Cosmos DB bağlantı dizesi, anahtarları, URI's erişmek için Azure Key Vault'u kullanın.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: rafats
ms.openlocfilehash: b090c1593b49bec4f51fea8d498860e8af8b2f4b
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053093"
---
# <a name="use-key-vault-to-store-and-access-azure-cosmos-db-keys"></a>Azure Cosmos DB anahtarlara erişmek ve depolamak için Key Vault'u kullanın

Azure Cosmos DB için uygulamalarınızı kullanırken, uygulamanın yapılandırma dosyası içinde uç noktası URI'si ve anahtar kullanarak veritabanı, koleksiyonlar, belgeler erişebilirsiniz.  Ancak, kullanılabilir tüm kullanıcılar için düz metin biçiminde olduğundan, anahtarları ve URL'yi doğrudan uygulama koduna koymak güvenli değildir. URI'si ve anahtarlar kullanılabilir, ancak güvenli bir mekanizma aracılığıyla olduğundan emin olmanız gerekir. Bu, Azure Key Vault, güvenli bir şekilde depolamak ve uygulama parolalarını yönetme burada yardımcı olur.

Aşağıdaki adımlar, depolama ve Azure Cosmos DB erişim anahtarlarını Key Vault'tan okumak için gereklidir:

* Anahtar kasası oluşturma  
* Azure Cosmos DB erişim tuşları anahtar Kasası'na ekleyin.  
* Azure web uygulaması oluşturma  
* Key Vault okuma izni verin ve uygulamayı kaydetme  


## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

1. Oturum [Azure portalında](https://portal.azure.com/).  
2. Seçin **kaynak Oluştur > Güvenlik > Key Vault**.  
3. **Anahtar kasası oluşturma** bölümünde aşağıdaki bilgileri sağlayın:  
   * **Ad:** anahtar kasanız için benzersiz bir ad belirtin.  
   * **Abonelik:** kullanacağınız aboneliği seçin.  
   * **Kaynak Grubu** altında **Yeni oluştur**’u seçin ve bir kaynak grubu adı girin.  
   * Konum aşağı açılır menüden bir konum seçin.  
   * Diğer seçenekleri varsayılan değerlerinde bırakın.  
4. Yukarıdaki bilgileri girdikten sonra **Oluştur**’u seçin.  

## <a name="add-azure-cosmos-db-access-keys-to-the-key-vault"></a>Azure Cosmos DB erişim tuşları, anahtar Kasası'na ekleyin.
1. Önceki adımda oluşturduğunuz, açık Key vault'a gidin **gizli dizileri** sekmesi.  
2. Seçin **+ Oluştur/içeri aktar**, 

   * Seçin **el ile** için **karşıya yükleme seçenekleri**.
   * Sağlayan bir **adı** için gizli anahtarı
   * Cosmos DB hesabınızın bağlantı dizesi sağlamanız **değer** alan. Ve ardından **Oluştur**.

   ![Gizli anahtar oluşturma](./media/access-secrets-from-keyvault/create-a-secret.png)

4. Gizli dizi oluşturulduktan sonra dosyayı açın ve kopyalayın ** şu biçimde bir gizli tanımlayıcı. Sonraki bölümde, bu tanımlayıcıyı kullanacaksınız. 

   `https://<Key_Vault_Name>.vault.azure.net/secrets/<Secret _Name>/<ID>`

## <a name="create-an-azure-web-application"></a>Azure web uygulaması oluşturma

1. Bir Azure web uygulaması oluşturduğunuzda veya uygulamadan indirebileceğiniz [GitHub deposu](https://github.com/Azure/azure-cosmosdb-dotnet/tree/master/Demo/keyvaultdemo). Bu basit bir MVC uygulamasıdır.  

2. İndirilen uygulama ve açılması **HomeController.cs** dosya. Aşağıdaki satırı gizli kimliği güncelleştirin:

   `var secret = await keyVaultClient.GetSecretAsync("<Your Key Vault’s secret identifier>")`

3. **Kaydet** dosya **derleme** çözümü.  
4. Ardından, uygulamayı azure'a dağıtın. Projeye sağ tıklayın ve seçin **yayımlama**. (Örneğin adı WebAppKeyVault1 uygulama) yeni bir app service profili oluşturun ve seçin **Yayımla**.   

5. Uygulama dağıtıldıktan sonra. Azure portalından, dağıtılan web uygulamasına gidin ve açmak **yönetilen hizmet kimliği** bu uygulamanın.  

   ![Yönetilen hizmet kimliği](./media/access-secrets-from-keyvault/turn-on-managed-service-identity.png)

Uygulamayı şimdi çalıştırırsanız, anahtar Kasası'nda bu uygulama için hiçbir izin vermediği gibi aşağıdaki hata iletisini görürsünüz.

![Uygulama erişimi olmadan dağıtılabilir.](./media/access-secrets-from-keyvault/app-deployed-without-access.png)

## <a name="register-the-application--grant-permissions-to-read-the-key-vault"></a>Key Vault okuma izni verin ve uygulamayı kaydetme

Bu bölümde, uygulamayı Azure Active Directory'ye kaydetmeniz ve Key Vault okumak uygulamanın izinlerini verin. 

1. Azure portalında, açık gidin **Key Vault** önceki bölümde oluşturduğunuz.  

2. Açık **erişim ilkeleri**seçin **+ yeni Ekle** izinleri seçin, dağıtılan web uygulaması bulun ve seçin **Tamam**.  

   ![Erişim ilkesi ekleme](./media/access-secrets-from-keyvault/add-access-policy.png)

Şimdi uygulamayı çalıştırırsanız, Key Vault'tan gizli dizi okuyabilirsiniz.

![Uygulama gizli anahtarı ile dağıtılan](./media/access-secrets-from-keyvault/app-deployed-with-access.png)
 
Benzer şekilde, bu anahtar kasasına erişmek için bir kullanıcı ekleyebilirsiniz. Anahtar Kasası'na seçerek eklemeniz gerekir **erişim ilkeleri** ve uygulamayı Visual Studio'dan çalıştırmak gereken tüm izinleri verin. Bu uygulama masaüstünüzden çalışırken, kimliğinizi alır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB için bkz. bir güvenlik duvarını [güvenlik duvarı desteği](firewall-support.md) makalesi.
* Sanal ağ hizmet uç noktasını yapılandırmak için bkz: [sanal ağ hizmet uç noktası kullanarak erişim güvenliğini sağlama](vnet-service-endpoint.md) makalesi.
