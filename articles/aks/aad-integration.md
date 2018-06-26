---
title: Azure Active Directory Azure Kubernetes hizmeti ile tümleştirme
description: Azure Active Directory özellikli Azure Kubernetes hizmetin nasıl oluşturulacağını kümeleri.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/17/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7d157d50bbcd25edd9cd6693a71fb04535cbeb79
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937390"
---
# <a name="integrate-azure-active-directory-with-aks---preview"></a>Azure Active Directory Tümleştirme AKS - Önizleme

Azure Kubernetes hizmet (AKS), Azure Active Directory kullanıcı kimlik doğrulaması için kullanmak üzere yapılandırılabilir. Bu yapılandırmada, Azure Active Directory kimlik doğrulama belirteci kullanarak bir Azure Kubernetes hizmeti kümesine oturum açabilir. Ayrıca, küme yöneticileri bir kullanıcı kimliği veya dizin grup üyeliğine dayalı Kubernetes rol tabanlı erişim denetimini yapılandırmak kullanabilirsiniz.

Bu belge AKS ve Azure AD için tüm gerekli Önkoşullar oluşturma, Azure AD etkin küme dağıtma ve basit bir RBAC rolü AKS kümede oluşturma ayrıntıları.

> [!IMPORTANT]
> Azure Kubernetes hizmet (AKS) RBAC ve Azure AD tümleştirme halen **Önizleme**. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.
>

## <a name="authentication-details"></a>Kimlik doğrulama ayrıntıları

Azure AD kimlik doğrulaması Azure Kubernetes kümelerine Openıd Connect ile sağlanır. Openıd Connect üstünde OAuth 2.0 protokolünü yerleşik bir kimlik katmanıdır. Openıd Connect hakkında daha fazla bilgi bulunabilir [Open ID connect belgelerine][open-id-connect].

Gelen Kubernetes küme içinde Web kancası belirteci kimlik doğrulaması kimlik doğrulama belirteçleri doğrulamak için kullanılır. Web kancası belirteci kimlik doğrulaması yapılandırılıp AKS kümesinin bir parçası yönetilebilir. Web kancası belirteci kimlik doğrulaması bulunabilir hakkında daha fazla bilgi [Web kancası authentication belgeleri][kubernetes-webhook].

> [!NOTE]
> AKS kimlik doğrulaması için Azure AD yapılandırırken, iki Azure AD uygulaması yapılandırılır. Bu işlem bir Azure Kiracı Yöneticisi tarafından tamamlanması gerekir.

## <a name="create-server-application"></a>Sunucu uygulaması oluştur

İlk Azure AD uygulaması, kullanıcıların Azure AD grup üyeliğini almak için kullanılır.

1. **Azure Active Directory** > **Uygulama kayıtları** > **Yeni uygulama kaydı**’nı seçin.

  Uygulama seçme bir ad verin **Web uygulaması / API** uygulama türü için hiçbir biçimlendirilmiş URI değeri girin **oturum açma URL'si**. Seçin **oluşturma** bittiğinde.

  ![Azure AD kaydı oluşturun](media/aad-integration/app-registration.png)

2. Seçin **bildirim** ve düzenleme `groupMembershipClaims` değeri `"All"`.

  Güncelleştirme tamamlandıktan sonra kaydedin.

  ![Tüm grup üyeliği güncelleştirme](media/aad-integration/edit-manifest.png)

3. Azure AD uygulaması geri üzerinde seçin **ayarları** > **anahtarları**.

  Anahtar bir açıklama ekleyin, bir sona erme tarihi seçin ve Seç **kaydetmek**. Anahtar değerini not edin. Azure AD dağıtma AKS küme etkinleştirildiğinde, bu değer olarak adlandırılır `Server application secret`.

  ![Uygulama özel anahtarı alma](media/aad-integration/application-key.png)

4. Azure AD uygulamaya dönmek **ayarları** > **gerekli izinleri** > **Ekle**  >   **Bir API seçin** > **Microsoft Graph** > **seçin**.

  Altında **uygulama izinleri** yanına işaretleyin **dizin verilerini okuma**.

  ![Uygulama grafik izinleri ayarlama](media/aad-integration/read-directory.png)

5. Altında **İZİNLERE TEMSİLCİ**, yanına işaretleyin **oturum açın ve kullanıcı profilini okuma** ve **dizin verilerini okuma**. Bir kez yapılır güncelleştirmelerini kaydedin.

  ![Uygulama grafik izinleri ayarlama](media/aad-integration/delegated-permissions.png)

6. Seçin **Bitti** ve **izinler** bu adımı tamamlamak için. Geçerli hesap bir kiracı yönetici değilse, bu adımı başarısız olur

  ![Uygulama grafik izinleri ayarlama](media/aad-integration/grant-permissions.png)

7. Uygulamaya dönün ve not edin **uygulama kimliği**. Bir Azure AD etkin AKS küme dağıtırken, bu değer olarak adlandırılır `Server application ID`.

  ![Uygulama Kimliği alma](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>İstemci uygulaması oluşturma

İkinci Azure AD uygulaması Kubernetes CLI (kubectl.) oturum açarken kullanılır

1. **Azure Active Directory** > **Uygulama kayıtları** > **Yeni uygulama kaydı**’nı seçin.

  Uygulama Seç bir ad verin **yerel** uygulama türü için hiçbir biçimlendirilmiş URI değeri girin **yeniden yönlendirme URI'si**. Seçin **oluşturma** bittiğinde.

  ![AAD kaydı oluşturun](media/aad-integration/app-registration-client.png)

2. Azure AD uygulamadan seçin **ayarları** > **gerekli izinleri** > **Ekle** > **seçin bir API** ve bu belgeyi son adımda oluşturduğunuz sunucu uygulamasının adını arayın.

  ![Uygulama izinlerini yapılandırma](media/aad-integration/select-api.png)

3. ' A tıklayın ve uygulama yanındaki onay işareti koyun **seçin**.

  ![AKS AAD sunucusu uygulama uç noktası seçin](media/aad-integration/select-server-app.png)

4. Seçin **Bitti** ve **izinler** bu adımı tamamlamak için.

  ![İzinleri verme](media/aad-integration/grant-permissions-client.png)

5. Geri AD uygulaması üzerinde not edin **uygulama kimliği**. Bir Azure AD etkin AKS küme dağıtırken, bu değer olarak adlandırılır `Client application ID`.

  ![Uygulama Kimliği alma](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>Kiracı kimliğini alma

Son olarak, Azure Kiracı kimliği alın. Bu değer, AKS küme dağıtırken de kullanılır.

Azure portalından seçin **Azure Active Directory** > **özellikleri** ve not edin **dizin kimliği**. Bir Azure AD etkin AKS küme dağıtırken, bu değer olarak adlandırılır `Tenant ID`.

![Azure Kiracı Kimliğinizi alma](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>Küme dağıtma

Kullanım [az grubu oluşturma] [ az-group-create] AKS küme için bir kaynak grubu oluşturmak için komutu.

```azurecli
az group create --name myAKSCluster --location eastus
```

Küme dağıtmanızı [az aks oluşturma] [ az-aks-create] komutu. Aşağıdaki örnek komutunu değerleri Azure AD uygulamaları oluştururken toplanan değerlerle değiştirin.

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --generate-ssh-keys --enable-rbac \
  --aad-server-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

## <a name="create-rbac-binding"></a>RBAC bağlama oluşturma

Bir Azure Active Directory hesabı AKS kümeyle kullanılabilmesi için önce bir rol bağlama veya küme rolü bağlama oluşturulması gerekir.

İlk olarak, kullanın [az aks get-kimlik] [ az-aks-get-credentials] komutunu `--admin` yönetici erişimi olan küme oturum açmak için bağımsız değişken.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster --admin
```

Ardından, aşağıdaki bildirim ClusterRoleBinding bir Azure AD hesabı oluşturmak için kullanın. Azure AD kiracınıza birinden ile kullanıcı adını güncelleştirin. Bu örnek için kümenin tüm ad alanlarını hesabı tam erişim sağlar.

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

Bir rolü bağlama ayrıca bir Azure AD grubunun tüm üyeleri için oluşturulabilir. Tüm üyeleri, şu bildirimi verir `kubernetes-admin` grup kümesine yönetici erişimi.

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
   name: "kubernetes-admin"
```

RBAC Kubernetes kümeyle güvenliğini sağlama konusunda daha fazla bilgi için bkz: [kullanarak RBAC yetkilendirmeyi][rbac-authorization].

## <a name="access-cluster-with-azure-ad"></a>Azure AD ile erişim kümesi

Ardından, yönetici olmayan kullanan kullanıcı bağlamı çekme [az aks get-kimlik] [ az-aks-get-credentials] komutu.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Herhangi bir kubectl komutunu çalıştırdıktan sonra Azure kimlik doğrulaması istenir. İzleyin ekrandaki yönergeleri.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-42032720-0   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-1   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-2   Ready     agent     1h        v1.9.6
```

Tamamlandıktan sonra kimlik doğrulama belirteci önbelleğe alınır. Yalnızca zaman belirtecinin süresi doldu veya yeniden oluşturulduğunda Kubernetes yapılandırma dosyasında oturum reprompted.

## <a name="next-steps"></a>Sonraki Adımlar

RBAC ile Kubernetes kümeleriyle güvenliğini sağlama hakkında daha fazla bilgi [kullanarak RBAC yetkilendirmeyi] [ rbac-authorization] belgeleri.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az_aks_get_credentials
[az-group-create]: /cli/azure/group#az_group_create
[open-id-connect]: ../active-directory/develop/active-directory-protocols-openid-connect-code.md
