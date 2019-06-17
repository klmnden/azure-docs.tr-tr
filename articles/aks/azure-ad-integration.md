---
title: Azure Kubernetes hizmeti ile Azure Active Directory Tümleştirme
description: Azure Active Directory özellikli Azure Kubernetes Service (AKS) kümeleri oluşturma
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/26/2019
ms.author: iainfou
ms.openlocfilehash: db166c82e39e9184528fde67ff868229cf9b1d57
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061115"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Azure Kubernetes hizmeti ile Azure Active Directory Tümleştirme

Azure Kubernetes Service (AKS), Azure Active Directory (Azure AD) kullanmak için kullanıcı kimlik doğrulaması için yapılandırılabilir. Bu yapılandırmada, bir AKS kümesi için Azure AD kimlik doğrulama belirtecinizi kullanarak oturum açabilir.

Küme yöneticileri, bir kullanıcının kimlik veya dizin grubu üyeliğine göre Kubernetes rol tabanlı erişim denetimi (RBAC) yapılandırabilirsiniz.

Bu makalede açıklanır nasıl yapılır:

- AKS ve Azure AD önkoşulları dağıtın.
- Azure AD etkin kümesine dağıtın.
- Temel bir RBAC rolü, Azure portalını kullanarak AKS kümesinde oluşturun.

Ayrıca, kullanarak aşağıdaki adımları tamamlayabilirsiniz [Azure CLI][azure-ad-cli].

> [!NOTE]
> Azure AD, yalnızca yeni bir RBAC etkin küme oluşturduğunuzda etkinleştirilebilir. Azure AD var olan bir AKS kümesi üzerinde etkinleştirilemiyor.

## <a name="authentication-details"></a>Kimlik doğrulama ayrıntıları

Azure AD kimlik doğrulaması, AKS kümeye Openıd Connect olması sağlanır. Openıd Connect, OAuth 2.0 protokolünü üzerinde yerleşik bir kimlik katmanı olan.

Openıd Connect hakkında daha fazla bilgi için bkz: [Openıd Connect ile Azure AD kullanarak web uygulamalarına erişim yetkisi verme][open-id-connect].

İçinde bir Kubernetes kümesi, Web kancası belirteci kimlik doğrulaması için kimlik doğrulama belirteçleri kullanılır. Web kancası belirteci kimlik doğrulaması yapılandırılır ve AKS kümesinin bir parçası yönetilir.

Web kancası belirteci kimlik doğrulaması hakkında daha fazla bilgi için bkz: [Web kancası belirteci kimlik doğrulaması] [ kubernetes-webhook] Kubernetes belgeleri bölümünde.

AKS kümesini Azure AD kimlik doğrulamasını sağlamak için iki Azure AD uygulama oluşturulur. İlk kullanıcı kimlik doğrulaması sağlayan bir sunucu bileşeni uygulamasıdır. İkinci kimlik doğrulaması için CLI tarafından istendiğinde kullanılan bir istemci bileşeni uygulamasıdır. Bu istemci uygulaması sunucu uygulaması istemci tarafından sağlanan kimlik bilgilerinin gerçek kimlik doğrulaması için kullanır.

> [!NOTE]
> AKS kimlik doğrulaması için Azure AD yapılandırdığınızda, iki Azure AD uygulamaları yapılandırılır. Bir Azure Kiracı Yöneticisi tarafından adımları her uygulama için izinler tamamlanması gerekir.

## <a name="create-the-server-application"></a>Sunucu uygulaması oluşturma

İlk Azure AD uygulaması, bir kullanıcının Azure AD grup üyeliğini almak için uygulanır. Azure portalında bu uygulamayı oluşturmak için:

1. Seçin **Azure Active Directory** > **uygulama kayıtları** > **yeni kayıt**.

    a. Uygulama gibi bir ad verin *AKSAzureADServer*.

    b. İçin **desteklenen hesap türleri**seçin **hesapları yalnızca kuruluş bu dizinde**.
    
    c. Seçin **Web** için yeniden yönlendirme URI'sini yazın ve tüm URI biçimli değeri, gibi enter *https://aksazureadserver* .

    d. Seçin **kaydetme** işiniz bittiğinde.

2. Seçin **bildirim**ve ardından düzenleme **groupMembershipClaims:** olarak değer **tüm**. Güncelleştirmeleriyle tamamladığınızda seçin **Kaydet**.

    ![Tüm grup üyeliği güncelleştir](media/aad-integration/edit-manifest.png)

3. Azure AD uygulamasının sol bölmesinde seçin **sertifikaları ve parolaları**.

    a. Seçin **+ yeni gizli**.

    b. Gibi anahtar bir açıklama ekleyin *AKS Azure AD sunucusu*. Sona erme süresini seçin ve ardından **Ekle**.

    c. Yalnızca şu anda görüntülenen anahtar değerini not edin. Azure AD etkin AKS kümesi dağıtırken, bu değer sunucu uygulama gizli anahtarı olarak adlandırılır.

4. Azure AD uygulamasının sol bölmesinde seçin **API izinleri**ve ardından **+ izin Ekle**.

    a. Altında **Microsoft APIs**seçin **Microsoft Graph**.

    b. Seçin **temsilci izinleri**ve ardından yanındaki onay kutusunu seçin **dizin > Directory.Read.All (dizin verilerini okuma)** .

    c. Varsayılan temsilci izni varsa **kullanıcı > User.Read (oturum açın ve kullanıcı profilini okuma)** mevcut değil, yanındaki onay kutusunu işaretleyin.

    d. Seçin **uygulama izinleri**ve ardından yanındaki onay kutusunu seçin **dizin > Directory.Read.All (dizin verilerini okuma)** .

    ![Graph izinleri ayarlama](media/aad-integration/graph-permissions.png)

    e. Seçin **izinleri eklemek** güncelleştirmeleri kaydetmek için.

    f. Altında **onay verme**seçin **yönetici onayı vermek**. Bu düğme geçerli hesabın bir kiracı Yöneticisi değilse kullanılabilir değil

    İzinler başarıyla verildi, aşağıdaki bildirim portalda görüntülenir:

   ![Bildirim başarıyla izin verildi](media/aad-integration/permissions-granted.png)

5. Azure AD uygulamasının sol bölmesinde seçin **bir API'yi kullanıma sunmak**ve ardından **+ "kapsam" Ekle**.
    
    a. Girin bir **kapsam adı**e **yönetici onayı görünen adı**ve ardından bir **yönetici onayı açıklaması** gibi *AKSAzureADServer*.

    b. Emin **durumu** ayarlanır **etkin**.

    ![Sunucu uygulamasının diğer hizmetleri ile kullanmak için bir API olarak kullanıma sunma](media/aad-integration/expose-api.png)

    c. Seçin **kapsamı Ekle**.

6. Uygulamaya dönmek **genel bakış** sayfası ve Not **uygulama (istemci) kimliği**. Azure AD etkin AKS kümesi dağıtırken, sunucu uygulama kimliği. Bu değer çağrılır

    ![Uygulama Kimliği alma](media/aad-integration/application-id.png)

## <a name="create-the-client-application"></a>İstemci uygulaması oluşturma

Kubernetes CLI (kubectl) ile oturum açtığınızda, ikinci Azure AD uygulaması kullanılır.

1. Seçin **Azure Active Directory** > **uygulama kayıtları** > **yeni kayıt**.

    a. Uygulama gibi bir ad verin *AKSAzureADClient*.

    b. İçin **desteklenen hesap türleri**seçin **hesapları yalnızca kuruluş bu dizinde**.

    c. Seçin **Web** için yeniden yönlendirme URI'sini yazın ve ardından URI biçimli herhangi bir değer gibi girin *https://aksazureadclient* .

    d. Seçin **kaydetme** işiniz bittiğinde.

2. Azure AD uygulamasının sol bölmesinde seçin **API izinleri**ve ardından **+ izin Ekle**.

    a. Seçin **Apı'lerim**ve ardından, Azure AD sunucu uygulaması gibi önceki adımda oluşturulan *AKSAzureADServer*.

    b. Seçin **temsilci izinleri**ve ardından, Azure AD sunucu uygulaması yanındaki onay kutusunu seçin.

    ![Uygulama izinlerini yapılandırma](media/aad-integration/select-api.png)

    c. Seçin **izinleri eklemek**.

    d. Altında **onay verme**seçin **yönetici onayı vermek**. Bu düğme, geçerli hesap bir kiracı Yöneticisi değilse kullanılamaz İzinler verildiğinde, aşağıdaki bildirim portalda görüntülenir:

    ![Bildirim başarıyla izin verildi](media/aad-integration/permissions-granted.png)

3. Azure AD uygulamasının sol bölmesinde seçin **kimlik doğrulaması**.

    - Altında **varsayılan istemci türü**seçin **Evet** için **istemci genel bir istemci kabul**.

5. Azure AD uygulamasının sol bölmede, uygulama kimliğini not edin. Azure AD etkin AKS kümesi dağıtırken, istemci uygulama kimliği. Bu değer çağrılır

   ![Uygulama Kimliğini Al](media/aad-integration/application-id-client.png)

## <a name="get-the-tenant-id"></a>Kiracı Kimliğinizi alma

Ardından, Azure kiracınızın Kimliğini alın. Bu değer AKS kümesi oluşturduğunuzda kullanılır.

Azure portalından seçin **Azure Active Directory** > **özellikleri** ve Not **dizin kimliği**. Azure AD etkin AKS kümesi oluşturduğunuzda, bu değer Kiracı kimliğini çağrılır

![Azure Kiracı Kimliğinizi alma](media/aad-integration/tenant-id.png)

## <a name="deploy-the-aks-cluster"></a>AKS kümesi dağıtma

Kullanım [az grubu oluşturma] [ az-group-create] AKS kümesi için bir kaynak grubu oluşturmak için komutu.

```azurecli
az group create --name myResourceGroup --location eastus
```

Kullanım [az aks oluşturma] [ az-aks-create] AKS kümesi dağıtmak için komutu. Ardından, aşağıdaki örnek komutta değerleri değiştirin. Sunucu uygulama kimliği, uygulama gizli anahtarı, istemci uygulama kimliği ve Kiracı kimliği için Azure AD uygulamaları oluştururken toplanan değerler kullanın

```azurecli
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --generate-ssh-keys \
  --aad-server-app-id b1536b67-29ab-4b63-b60f-9444d0c15df1 \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 8aaf8bd5-1bdd-4822-99ad-02bfaa63eea7 \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

Bir AKS kümesi oluşturmak için birkaç dakika sürer.

## <a name="create-an-rbac-binding"></a>Bir RBAC bağlama oluşturma

Bir Azure Active Directory hesabı ile bir AKS kümesi kullanmadan önce rolü bağlama veya küme rolünü bağlama oluşturmanız gerekir. Rol izinleri tanımlayın ve bağlamaları bunları istediğiniz kullanıcılar için geçerlidir. Bu atamaları, tüm küme üzerinde veya belirtilen bir ad alanı için uygulanabilir. Daha fazla bilgi için [kullanarak RBAC yetkilendirme][rbac-authorization].

İlk olarak, [az aks get-credentials] [ az-aks-get-credentials] komutunu `--admin` yönetici erişimi ile küme oturum açmak için bağımsız değişken.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Ardından, ClusterRoleBinding AKS kümesi erişim vermek istediğiniz bir Azure AD hesabı oluşturun. Aşağıdaki örnek kümedeki tüm ad alanlarını hesabı tam erişim sağlar:

- Kullanıcı için RBAC bağlama aynı Azure AD kiracısında vermek, kullanıcı asıl adına (UPN) dayalı izinleri atayın. Adım ClusterRoleBinding YAML bildirimi oluşturmak için geçin.

- Kullanıcı, başka bir Azure AD Kiracı, sorgulama ve kullanma **objectID** özelliği bunun yerine. Gerekirse, gerekli bir kullanıcı hesabının objectID almak [az ad kullanıcı show] [ az-ad-user-show] komutu. Gerekli hesabının kullanıcı asıl adı (UPN) sağlayın:

    ```azurecli-interactive
    az ad user show --upn-or-object-id user@contoso.com --query objectId -o tsv
    ```

Gibi bir dosya oluşturun *rbac aad user.yaml*ve aşağıdaki içeriği yapıştırın. Son satırında değiştirin **userPrincipalName_or_objectId** UPN veya nesne kimliğine sahip Seçimi kullanıcı aynı Azure AD kiracısına veya olmasına göre değişir.

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
  name: userPrincipalName_or_objectId
```

Bağlama kullanarak uygulama [kubectl uygulamak] [ kubectl-apply] komutu aşağıdaki örnekte gösterildiği gibi:

```console
kubectl apply -f rbac-aad-user.yaml
```

Bir rol bağlama için bir Azure AD grubunun tüm üyelerini de oluşturulabilir. Azure AD grupları grubu nesne kimliği'ni kullanarak aşağıdaki örnekte gösterildiği gibi belirtilir.

Gibi bir dosya oluşturun *rbac aad group.yaml*ve aşağıdaki içeriği yapıştırın. Grubun nesne Kimliğini bir Azure AD kiracınız ile güncelleştirin:

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

Bağlama kullanarak uygulama [kubectl uygulamak] [ kubectl-apply] komutu aşağıdaki örnekte gösterildiği gibi:

```console
kubectl apply -f rbac-aad-group.yaml
```

RBAC ile bir Kubernetes kümesi güvenliğini sağlama konusunda daha fazla bilgi için bkz. [kullanarak RBAC yetkilendirme][rbac-authorization].

## <a name="access-the-cluster-with-azure-ad"></a>Azure AD ile küme erişim

Yönetici olmayan kullanıcı bağlamı kullanarak çekme [az aks get-credentials] [ az-aks-get-credentials] komutu.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Çalıştırdıktan sonra `kubectl` istenir Azure kullanarak kimlik doğrulaması için komutu. İzleyin ekran aşağıdaki örnekte gösterildiği gibi işlemi tamamlamak için yönergeleri:

```console
$ kubectl get nodes

To sign in, use a web browser to open https://microsoft.com/devicelogin. Next, enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-1   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-2   Ready     agent     1h        v1.13.5
```

İşlem tamamlandığında, kimlik doğrulama belirteci önbelleğe alınır. Yalnızca belirteç süre sonu veya Kubernetes yapılandırma dosyası yeniden oluşturulana tekrar oturum açmanız istenir.

Başarıyla oturum açtıktan sonra bir yetkilendirme hata iletisini görürseniz, aşağıdaki ölçütleri denetleyin:

```console
error: You must be logged in to the server (Unauthorized)
```


- Uygun nesne Kimliğini veya UPN'sini, kullanıcı hesabının aynı Azure AD kiracısında olup olmadığını bağlı olarak, tanımladığınız.
- Kullanıcı, 200'den fazla grupların bir üyesi değildir.
- Sunucu kullanılarak yapılandırılan değerle için uygulama kaydında tanımlanan gizli `--aad-server-app-secret`.

## <a name="next-steps"></a>Sonraki adımlar

Küme kaynaklarına erişimi denetlemek için Azure AD kullanıcılarını ve gruplarını kullanmak için bkz: [AKS rol tabanlı erişim denetimi ve Azure AD kimlikleri kullanarak küme kaynaklarında erişim denetimi][azure-ad-rbac].

Güvenli Kubernetes kümelerini kullanma hakkında daha fazla bilgi için bkz. [AKS için erişim ve kimlik seçeneklerini][rbac-authorization].

Kimlik ve kaynak denetimi hakkında daha fazla bilgi için bkz: [en iyi uygulamalar için kimlik doğrulama ve yetkilendirme aks'deki][operator-best-practices-identity].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
