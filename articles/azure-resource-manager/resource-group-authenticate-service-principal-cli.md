---
title: "Azure CLI ile Azure uygulama kimliği oluşturma | Microsoft Docs"
description: "Bir Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmak ve rol tabanlı erişim denetimi aracılığıyla kaynaklara erişim izni için Azure CLI kullanmayı açıklar. Uygulama ile bir parola veya sertifika kimlik doğrulaması yapmayı gösterir."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 3c5826d58887ff1af4df8e66999d9c1a1643bcc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Kaynaklara erişmek üzere hizmet sorumlusu oluşturmak için Azure CLI kullanma

Bir uygulama ya da kaynaklara erişmek için gereken komut dosyası varsa, uygulamanın kendi kimlik bilgileriyle kimlik doğrulamasını ve uygulama için bir kimlik ayarlayın. Bu kimlik, bir hizmet sorumlusu bilinir. Bu yaklaşım sağlar:

* Kendi izinlerinizi farklı uygulama kimliği için izinleri atayın. Genellikle, bu izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.
* Sertifika kimlik doğrulaması için Katılımsız betik yürütülürken kullanın.

Bu makalede nasıl kullanılacağı gösterilmektedir [Azure CLI 1.0](../cli-install-nodejs.md) bir uygulamanın kendi kimlik bilgilerini ve kimlik altında çalışmasına ayarlamak için. En son sürümünü yüklemek [Azure CLI 1.0](../cli-install-nodejs.md) ortamınızla eşleşen bu makaledeki örneklerde emin olmak için.

## <a name="required-permissions"></a>Gerekli izinler
Bu konuda tamamlamak için Azure Active Directory ve Azure aboneliğinize yeterli izniniz olması gerekir. Özellikle, Azure Active Directory'de bir uygulama oluşturun ve hizmet sorumlusu rol atama mümkün olması gerekir. 

Hesabınızın yeterli izinlere sahip olup olmadığını denetlemenin en kolay yolu portalı kullanmaktır. Bkz. [Portalda gerekli izinleri denetleme](resource-group-create-service-principal-portal.md#required-permissions).

Şimdi, bir bölüm için ya da devam [parola](#create-service-principal-with-password) veya [sertifika](#create-service-principal-with-certificate) kimlik doğrulaması.

## <a name="create-service-principal-with-password"></a>Parola ile hizmet sorumlusu oluşturma
Bu bölümde, bir parola ile AD uygulaması oluşturmak için aşağıdaki adımları gerçekleştirin ve hizmet sorumlusu okuyucu rolüne atayın.

1. Hesabınızda oturum açın.
   
   ```azurecli
   azure login
   ```
2. Bir uygulama kimliği oluşturmak için aşağıdaki komutta gösterildiği gibi uygulama ve bir parola adını sağlayın:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   Yeni hizmet sorumlusunu döndürülür. İzin verme nesne kimliği gereklidir. Hizmet asıl adları ile listelenen GUID açarken gereklidir. Uygulama kimliği ile aynı değere GUID'dir. Örnek uygulamalarda, bu değer olarak adlandırılır `Client ID`. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. Aboneliğiniz için hizmet asıl izinleri verin. Bu örnekte, hizmet sorumlusu Abonelikteki tüm kaynaklar okuma izni verir okuyucu rolüne ekleyin. Diğer roller için bkz: [RBAC: yerleşik roller](../active-directory/role-based-access-built-in-roles.md). ObjectID parametresi için uygulama oluştururken kullanılan nesne kimliği sağlayın. Bu komutu çalıştırmadan önce süre Azure Active Directory yaymak yeni bir hizmet sorumlusu için izin vermelidir. Genellikle, bu komutları el ile çalıştırdığınızda görevleri arasında yeterli bir süre geçti. Bir komut dosyası komut arasında uyku moduna bir adımı eklemeniz gerekir (gibi `sleep 15`). Asıl dizinde yok bildiren bir hata görürseniz, komutu yeniden çalıştırın.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
İşte bu kadar! AD uygulaması ve hizmet sorumlusu ayarlanır. Sonraki bölümde Azure CLI kimlik bilgilerinizle oturum gösterilmiştir. Kimlik bilgisi kodu uygulamanızda kullanmak istiyorsanız, bu konu ile devam etmek gerekmez. Atlayabilirsiniz [örnek uygulamaları](#sample-applications) uygulama kimliği ve parola oturum açma örnekleri için. 

### <a name="provide-credentials-through-azure-cli"></a>Azure CLI aracılığıyla kimlik bilgilerini sağlayın
Şimdi, işlemleri gerçekleştirmek için uygulama olarak oturum açmak gerekir.

1. Bir hizmet sorumlusu oturum olduğunda, Kiracı kimliği dizininin AD uygulamanız için sağlamanız gerekir. Bir kiracı, Azure Active Directory örneğidir. Şu anda kimliği doğrulanmış aboneliğinizin Kiracı Kimliği almak için kullanın:
   
   ```azurecli
   azure account show
   ```
   
   Hangi döndürür:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Başka bir abonelik Kiracı kimliğini almak gerekiyorsa, aşağıdaki komutu kullanın:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Oturum açma için kullanılacak istemci kimliğini almak gereken durumlarda kullanın:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     Oturum açma için kullanılacak değer hizmet asıl adları listelenen GUID'dir.
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. Hizmet sorumlusu oturum açın.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    İçin parola istenir. AD uygulamasının oluştururken belirttiğiniz parolayı belirtin.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

Şimdi, oluşturduğunuz hizmet sorumlusu için hizmet sorumlusu olarak doğrulanır.

Alternatif olarak, oturum açmak için komut satırından REST işlemlerini çağırabilirsiniz. Kimlik doğrulaması yanıtından, diğer işlemleri ile kullanmak için erişim belirtecini alabilirsiniz. REST işlemlerini çağırarak erişim belirtecini alma bir örnek için bkz: [bir erişim belirteci oluşturma](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>Sertifika ile hizmet sorumlusu oluşturma
Bu bölümde, adımları gerçekleştirin:

* Otomatik olarak imzalanan sertifika oluşturma
* AD uygulamasının sertifikası ve hizmet sorumlusu oluşturma
* Hizmet sorumlusu okuyucu rolüne atayın

Bu adımları tamamlamak için ihtiyacınız [OpenSSL](http://www.openssl.org/) yüklü.

1. Kendinden imzalı bir sertifika oluşturun.
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. Önceki adımı iki dosya - privkey.pem ve cert.pem oluşturuldu. Ortak ve özel anahtarlar, tek bir dosya halinde birleştirin.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Açık **examplecert.pem** dosya ve karakter arasında uzun dizi arayın **---başlangıç sertifika---** ve **---son SERTİFİKAYI---**. Sertifika verileri kopyalayın. Hizmet asıl oluştururken bu verileri bir parametre olarak geçirir.

4. Hesabınızda oturum açın.

   ```azurecli
   azure login
   ```
5. Hizmet sorumlusu oluşturmak için aşağıdaki komutta gösterildiği gibi uygulama ve sertifika verileri adını sağlayın:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   Yeni hizmet sorumlusunu döndürülür. İzin verme nesne kimliği gereklidir. Hizmet asıl adları ile listelenen GUID açarken gereklidir. Uygulama kimliği ile aynı değere GUID'dir. Örnek uygulamalarda, bu değer için istemci kimliği olarak adlandırılır 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. Aboneliğiniz için hizmet asıl izinleri verin. Bu örnekte, hizmet sorumlusu Abonelikteki tüm kaynaklar okuma izni verir okuyucu rolüne ekleyin. Diğer roller için bkz: [RBAC: yerleşik roller](../active-directory/role-based-access-built-in-roles.md). ObjectID parametresi için uygulama oluştururken kullanılan nesne kimliği sağlayın. Bu komutu çalıştırmadan önce süre Azure Active Directory yaymak yeni bir hizmet sorumlusu için izin vermelidir. Genellikle, bu komutları el ile çalıştırdığınızda görevleri arasında yeterli bir süre geçti. Bir komut dosyası komut arasında uyku moduna bir adımı eklemeniz gerekir (gibi `sleep 15`). Asıl dizinde yok bildiren bir hata görürseniz, komutu yeniden çalıştırın.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Otomatik Azure CLI komut dosyası aracılığıyla sertifikası sağlayın
Şimdi, işlemleri gerçekleştirmek için uygulama olarak oturum açmak gerekir.

1. Bir hizmet sorumlusu oturum olduğunda, Kiracı kimliği dizininin AD uygulamanız için sağlamanız gerekir. Bir kiracı, Azure Active Directory örneğidir. Şu anda kimliği doğrulanmış aboneliğinizin Kiracı Kimliği almak için kullanın:
   
   ```azurecli
   azure account show
   ```
   
   Hangi döndürür:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Başka bir abonelik Kiracı kimliğini almak gerekiyorsa, aşağıdaki komutu kullanın:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Sertifika parmak izini almak ve gerekli olmayan karakterleri kaldırmak için kullanın:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Hangi parmak izi değeri benzer döndürür:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Oturum açma için kullanılacak istemci kimliğini almak gereken durumlarda kullanın:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   Oturum açma için kullanılacak değer hizmet asıl adları listelenen GUID'dir.
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. Hizmet sorumlusu oturum açın.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

Şimdi, oluşturduğunuz Azure Active Directory uygulaması için hizmet sorumlusu olarak doğrulanır.

## <a name="change-credentials"></a>Kimlik bilgilerini değiştirme

Ya da güvenliğinin aşılması veya bir kimlik bilgisi sona erme nedeniyle, bir AD uygulaması için kimlik bilgilerini değiştirmek için kullanın `azure ad app set`.

Parolayı değiştirmek için kullanın:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

Bir sertifika değerini değiştirmek için kullanın:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a>Hata ayıklama

Bir hizmet sorumlusu oluşturma sırasında şu hatalarla karşılaşabilirsiniz:

* **"Authentication_Unauthorized"** veya **"abonelik bağlamda bulunamadı."** Hesabınızı olmadığı zaman-bu hatayı görmek [gerekli izinleri](#required-permissions) uygulama kaydetmek için Azure Active Directory üzerinde. Yalnızca yönetici kullanıcıların Azure Active Directory'de uygulamaları kaydedebilirsiniz ve hesabınızın bir yönetici değil, bu hata genellikle, bakın Ya da bir yönetici rolü atayın veya kullanıcıların uygulamaları kaydetmek yöneticinize başvurun.

* Hesabınızı **"kapsamı '/ subscriptions / {GUID}' üzerinde 'Microsoft.Authorization/roleAssignments/write' işlemini gerçekleştirme yetkisi yok."**  -Hesabınız için bir kimlik rol atamak için yeterli izinlere sahip olmadığında bu hataya bakın. Kullanıcı erişimi Yöneticisi rolüne eklemek için abonelik yöneticinize başvurun.

## <a name="sample-applications"></a>Örnek uygulamalar
Farklı platformlarda üzerinden uygulama olarak oturum açma hakkında daha fazla bilgi için bkz:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Sonraki adımlar
* Kaynakları yönetmek için Azure'da bir uygulamayı tümleştirme ayrıntılı adımlar için bkz: [Geliştirici Kılavuzu'na yetkilendirme Azure Kaynak Yöneticisi API'si ile](resource-manager-api-authentication.md).
* Sertifikalar ve Azure CLI kullanma hakkında daha fazla bilgi edinmek için bkz: [sertifika tabanlı kimlik doğrulaması Azure hizmet asıl adı ile Linux komut satırından](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
* Verilen veya kullanıcılar için reddedilen kullanılabilir eylemler listesi için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../active-directory/role-based-access-control-resource-provider-operations.md).
