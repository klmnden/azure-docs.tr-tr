---
title: Azure Kubernetes hizmeti ile Azure Active Directory Tümleştirme
description: Azure Active Directory özellikli Azure Kubernetes Service kümeleri oluşturma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/17/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 7ae3818795cddf5dfbb93ca6cc8dfff9d1c44c03
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341254"
---
# <a name="integrate-azure-active-directory-with-aks---preview"></a>Azure Active Directory Tümleştirme ile AKS - Önizleme

Azure Kubernetes Service (AKS), Azure Active Directory kullanıcı kimlik doğrulaması için kullanmak üzere yapılandırılabilir. Bu yapılandırmada, Azure Active Directory kimlik doğrulama belirtecinizi kullanarak bir Azure Kubernetes hizmeti kümesine oturum açabilirsiniz. Ayrıca, küme yöneticileri bir kullanıcı kimliği veya dizin grubu üyeliğine göre Kubernetes rol tabanlı erişim denetimini yapılandırmak kullanabilirsiniz.

Bu belge, AKS ve Azure AD için tüm gerekli Önkoşullar oluşturma, Azure AD etkin küme dağıtma ve AKS kümesinde basit bir RBAC rolü oluşturma ayrıntıları.

> [!IMPORTANT]
> Azure Kubernetes Service (AKS) RBAC ve Azure AD tümleştirmesi, şu anda **Önizleme**. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="authentication-details"></a>Kimlik doğrulama ayrıntıları

Azure AD kimlik doğrulaması, Openıd Connect ile Azure Kubernetes kümeleri için sağlanır. Openıd Connect, OAuth 2.0 protokolünü üzerinde yerleşik bir kimlik katmanı olan. Openıd Connect hakkında daha fazla bilgi bulunabilir [Open ID connect belgeleri][open-id-connect].

Gelen bir Kubernetes kümesi içinde Web kancası belirteci kimlik doğrulaması kimlik doğrulama belirteçleri doğrulamak için kullanılır. Web kancası belirteci kimlik doğrulaması yapılandırılır ve AKS kümesinin bir parçası yönetilir. Web kancası belirteci kimlik doğrulaması bulunabilir hakkında daha fazla bilgi [Web kancası authentication belgeleri][kubernetes-webhook].

> [!NOTE]
> AKS kimlik doğrulaması için Azure AD yapılandırırken iki Azure AD uygulaması yapılandırılır. Bu işlem, bir Azure Kiracı Yöneticisi tarafından tamamlanması gerekir.

## <a name="create-server-application"></a>Sunucu uygulaması oluşturma

İlk Azure AD uygulaması, kullanıcıları Azure AD grup üyeliğini almak için kullanılır.

1. **Azure Active Directory** > **Uygulama kayıtları** > **Yeni uygulama kaydı**’nı seçin.

  Uygulama seçme gibi bir ad vermek **Web uygulaması / API** uygulama türü için biçimlendirilmiş URI değeri girin **oturum açma URL'si**. Seçin **Oluştur** işiniz bittiğinde.

  ![Azure AD kaydı oluşturma](media/aad-integration/app-registration.png)

2. Seçin **bildirim** ve düzenleme `groupMembershipClaims` değerini `"All"`.

  Güncelleştirme tamamlandıktan sonra kaydedin.

  ![Tüm grup üyeliği güncelleştir](media/aad-integration/edit-manifest.png)

3. Azure AD uygulaması geri üzerinde seçin **ayarları** > **anahtarları**.

  Anahtar bir açıklama ekleyin, bir sona erme tarihi seçip **Kaydet**. Anahtar değerini not alın. AKS kümesini Azure AD'yi dağıtma etkin olduğunda, bu değer olarak adlandırılır `Server application secret`.

  ![Uygulama özel anahtarı alma](media/aad-integration/application-key.png)

4. Azure AD uygulamaya dönmek **ayarları** > **gerekli izinler** > **Ekle**  >   **Bir API seçin** > **Microsoft Graph** > **seçin**.

  ![Graph API seçin](media/aad-integration/graph-api.png)

5. Altında **uygulama izinleri** yanında bir onay işareti koyun **dizin verilerini okuma**.

  ![Uygulama graph izinleri ayarlayın](media/aad-integration/read-directory.png)

6. Altında **TEMSİLCİLİ izinler**, bir onay yanına **oturum açın ve kullanıcı profilini okuma** ve **dizin verilerini okuma**. Bir kez yapılan güncelleştirmeler kaydedin.

  ![Uygulama graph izinleri ayarlayın](media/aad-integration/delegated-permissions.png)

7. Seçin **Bitti**, seçin *Microsoft Graph* API'leri listesinden seçip **izinler**. Geçerli hesabın, bir kiracı Yöneticisi değilse, bu adımı başarısız olur

  ![Uygulama graph izinleri ayarlayın](media/aad-integration/grant-permissions.png)

8. Uygulamaya dönmek ve Not **uygulama kimliği**. Azure AD etkin AKS kümesi dağıtırken, bu değer olarak adlandırılır `Server application ID`.

  ![Uygulama Kimliği alma](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>İstemci uygulaması oluşturma

İkinci Azure AD uygulaması, Kubernetes CLI (kubectl) ile oturum açarken kullanılır.

1. **Azure Active Directory** > **Uygulama kayıtları** > **Yeni uygulama kaydı**’nı seçin.

  Uygulama seçme gibi bir ad vermek **yerel** uygulama türü için biçimlendirilmiş URI değeri girin **yeniden yönlendirme URI'si**. Seçin **Oluştur** işiniz bittiğinde.

  ![AAD kaydı oluşturma](media/aad-integration/app-registration-client.png)

2. Azure AD uygulamasından seçin **ayarları** > **gerekli izinler** > **Ekle** > **seçin bir API** ve bu belgenin son adımda oluşturduğunuz sunucu uygulamasının adını arayın.

  ![Uygulama izinlerini yapılandırma](media/aad-integration/select-api.png)

3. ' A tıklayın ve uygulamanın yanında bir onay işareti koyun **seçin**.

  ![AKS AAD sunucu uygulaması uç noktası seçin](media/aad-integration/select-server-app.png)

4. Seçin **Bitti** ve **izinler** bu adımı tamamlamak için.

  ![İzinleri verme](media/aad-integration/grant-permissions-client.png)

5. Geri AD uygulaması üzerinde Not **uygulama kimliği**. Azure AD etkin AKS kümesi dağıtırken, bu değer olarak adlandırılır `Client application ID`.

  ![Uygulama Kimliğini Al](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Kiracı kimliğini alma

Son olarak, Azure kiracınızın Kimliğini alın. Bu değer, AKS kümesi dağıtırken de kullanılır.

Azure portalından seçin **Azure Active Directory** > **özellikleri** ve Not **dizin kimliği**. Azure AD etkin AKS kümesi dağıtırken, bu değer olarak adlandırılır `Tenant ID`.

![Azure Kiracı Kimliğinizi alma](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Küme dağıtma

Kullanım [az grubu oluşturma] [ az-group-create] AKS kümesi için bir kaynak grubu oluşturmak için komutu.

```azurecli
az group create --name myAKSCluster --location eastus
```

Küme dağıtmanızı [az aks oluşturma] [ az-aks-create] komutu. Değerleri aşağıdaki örnek komutta Azure AD uygulamaları oluştururken toplanan değerlerle değiştirin.

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --generate-ssh-keys --enable-rbac \
  --aad-server-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

## <a name="create-rbac-binding"></a>RBAC bağlama oluşturma

Bir Azure Active Directory hesabı kullanarak AKS kümesiyle kullanılmadan önce bir rol bağlama veya küme rolünü bağlaması oluşturulması gerekir.

İlk olarak, [az aks get-credentials] [ az-aks-get-credentials] komutunu `--admin` yönetici erişimi ile küme oturum açmak için bağımsız değişken.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster --admin
```

Ardından, bir Azure AD hesabı için bir ClusterRoleBinding oluşturmak için aşağıdaki bildirimi kullanın. Azure AD kiracınızdan bir kullanıcı adını güncelleştirin. Bu örnekte, kümenin tüm ad alanları için hesap tam erişim sağlar.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: "user@contoso.com"
```

Bir rol bağlama için bir Azure AD grubunun tüm üyelerini de oluşturulabilir. Grubun nesne kimliğini kullanarak Azure AD grupları belirtilir

 ```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

RBAC ile bir Kubernetes kümesi güvenliğini sağlama konusunda daha fazla bilgi için bkz. [kullanarak RBAC yetkilendirme][rbac-authorization].

## <a name="access-cluster-with-azure-ad"></a>Azure AD ile erişim kümesi

Ardından, yönetici olmayan kullanan kullanıcı için bağlam çekme [az aks get-credentials] [ az-aks-get-credentials] komutu.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Herhangi bir kubectl komutunu çalıştırdıktan sonra Azure ile kimlik doğrulaması istenir. İzleyin ekrandaki yönergeleri.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-42032720-0   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-1   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-2   Ready     agent     1h        v1.9.6
```

İşlem tamamlandıktan sonra kimlik doğrulama belirteci önbelleğe alınır. Yalnızca ne zaman belirtecinin süresi doldu veya yeniden oluşturulduğunda Kubernetes yapılandırma dosyasında oturum reprompted.

Başarıyla oturum açtıktan sonra bir yetkilendirme hata iletisini görüyorsanız, bir konuk (farklı bir dizin Federasyon oturum açma kullanıyorsanız, genellikle böyledir) Azure ad'deki olduğu gibi kullanıcı, oturum açan olduğunu denetleyin.
```console
error: You must be logged in to the server (Unauthorized)
```


## <a name="next-steps"></a>Sonraki Adımlar

Kubernetes kümelerini ile RBAC ile güvenli hale getirme hakkında daha fazla bilgi [kullanarak RBAC yetkilendirme] [ rbac-authorization] belgeleri.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az_aks_get_credentials
[az-group-create]: /cli/azure/group#az_group_create
[open-id-connect]: ../active-directory/develop/active-directory-protocols-openid-connect-code.md
