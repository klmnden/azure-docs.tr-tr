---
title: Azure Kubernetes hizmeti ile Azure Active Directory Tümleştirme
description: Azure Active Directory özellikli Azure Kubernetes Service (AKS) kümesi ve Azure CLI'yı oluşturmak için kullanmayı öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 04/16/2019
ms.author: iainfou
ms.openlocfilehash: 0216a8c7d4e52e89098979223e9b792398e25038
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920176"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service-using-the-azure-cli"></a>Azure CLI kullanarak Azure Kubernetes hizmeti ile Azure Active Directory Tümleştirme

Azure Kubernetes Service (AKS), Azure Active Directory (AD) kullanıcı kimlik doğrulaması için kullanmak üzere yapılandırılabilir. Bu yapılandırmada, bir Azure AD kimlik doğrulama belirteci kullanarak bir AKS kümesine oturum açabilirsiniz. Küme operatörleri, bir kullanıcının kimlik veya dizin grubu üyeliğine göre Kubernetes rol tabanlı erişim denetimi (RBAC) olarak da yapılandırabilirsiniz.

Bu makalede gerekli oluşturma işlemi gösterilmektedir Azure AD bileşenleri, ardından AD etkin bir Azure kümesine dağıtma ve AKS kümesinde temel bir RBAC rolü oluşturun. Ayrıca [Azure portalını kullanarak şu adımları tamamlayın][azure-ad-portal].

Bu makalede kullanılan tam örnek betik için bkz: [Azure CLI örnekleri - Azure AD ile tümleştirme AKS][complete-script].

Aşağıdaki sınırlamalar geçerlidir:

- Azure AD, yalnızca yeni, RBAC özellikli bir küme oluşturduğunuzda etkinleştirilebilir. Azure AD var olan bir AKS kümesi üzerinde etkinleştirilemiyor.
- *Konuk* kullanıcıların Azure AD'de gibi farklı bir dizinden bir Federasyon oturum açma gibi kullanırsanız desteklenmez.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI Sürüm 2.0.61 gerekir veya daha sonra yüklü ve yapılandırılmış. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

Tutarlılık ve bu makalede komutları çalıştırmak amacıyla istenen AKS küme adınız için bir değişken oluşturun. Adı aşağıdaki örnekte *myakscluster*:

```azurecli-interactive
aksname="myakscluster"
```

## <a name="azure-ad-authentication-overview"></a>Azure AD kimlik doğrulamasına genel bakış

Azure AD kimlik doğrulaması, AKS kümelerine Openıd Connect ile sağlanır. Openıd Connect, OAuth 2.0 protokolünü üzerinde yerleşik bir kimlik katmanı olan. Openıd Connect hakkında daha fazla bilgi için bkz: [Open ID connect belgeleri][open-id-connect].

Gelen bir Kubernetes kümesi içinde Web kancası belirteci kimlik doğrulaması kimlik doğrulama belirteçleri doğrulamak için kullanılır. Web kancası belirteci kimlik doğrulaması yapılandırılır ve AKS kümesinin bir parçası yönetilir. Web kancası belirteci kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Web kancası authentication belgeleri][kubernetes-webhook].

> [!NOTE]
> AKS kimlik doğrulaması için Azure AD yapılandırırken iki Azure AD uygulaması yapılandırılır. Bu işlem, bir Azure Kiracı Yöneticisi tarafından tamamlanması gerekir.

## <a name="create-azure-ad-server-component"></a>Azure AD sunucu bileşeni oluşturma

AKS ile tümleştirmek için oluşturun ve kimlik istekler için bir uç nokta olarak davranan bir Azure AD uygulamasını kullanın. İhtiyacınız olan ilk Azure AD uygulaması, bir kullanıcının Azure AD grup üyeliği alır.

Sunucu bileşeni kullanarak uygulama oluşturma [az ad app oluşturma] [ az-ad-app-create] komutu, sonra Grup üyeliğini talep kullanarak güncelleştirme [az ad app update] [ az-ad-app-update] komutu. Aşağıdaki örnekte *aksname* tanımlanan değişkeni [başlamadan önce](#before-you-begin) bölüm ve bir değişken oluşturur

```azurecli-interactive
# Create the Azure AD application
serverApplicationId=$(az ad app create \
    --display-name "${aksname}Server" \
    --identifier-uris "https://${aksname}Server" \
    --query appId -o tsv)

# Update the application group memebership claims
az ad app update --id $serverApplicationId --set groupMembershipClaims=All
```

Artık server kullanarak uygulama için hizmet sorumlusu oluşturma [az ad sp oluşturma] [ az-ad-sp-create] komutu. Bu hizmet sorumlusu, Azure platformunun içinde kendi kimliğini doğrulamak için kullanılır. Ardından kullanarak hizmet sorumlusu gizli dizi alma [az ad sp kimlik bilgilerini Sıfırla] [ az-ad-sp-credential-reset] adlı değişkene atayın ve komutu *serverApplicationSecret* birini kullanmak için Aşağıdaki adımlar:

```azurecli-interactive
# Create a service principal for the Azure AD application
az ad sp create --id $serverApplicationId

# Get the service principal secret
serverApplicationSecret=$(az ad sp credential reset \
    --name $serverApplicationId \
    --credential-description "AKSPassword" \
    --query password -o tsv)
```

Azure AD aşağıdaki eylemleri gerçekleştirmek için izinler gerekiyor:

* Dizin verilerini okuma
* Oturum açma ve kullanıcı profilini okuma

Kullanarak bu izinleri [az ad app izni ekleme] [ az-ad-app-permission-add] komutu:

```azurecli-interactive
az ad app permission add \
    --id $serverApplicationId \
    --api 00000003-0000-0000-c000-000000000000 \
    --api-permissions e1fe6dd8-ba31-4d61-89e7-88639da4683d=Scope 06da0dbc-49e2-44d2-8312-53f166ab848a=Scope 7ab1d382-f21e-4acd-a863-ba3e13f7da61=Role
```

Son olarak, önceki adımda server kullanarak uygulama için atanan izinler [az ad app iznini] [ az-ad-app-permission-grant] komutu. Geçerli hesabın, bir kiracı Yöneticisi değilse, bu adımı başarısız olur. Aksi takdirde kullanarak yönetici onayı gerektirebilir bilgi istemek Azure AD uygulaması için izinler eklemek etmeniz [az ad app izni yönetici-onayı][az-ad-app-permission-admin-consent]:

```azurecli-interactive
az ad app permission grant --id $serverApplicationId --api 00000003-0000-0000-c000-000000000000
az ad app permission admin-consent --id  $serverApplicationId
```

## <a name="create-azure-ad-client-component"></a>Azure AD istemci bileşeni oluşturma

Kubernetes CLI ile bir AKS kümesi için bir kullanıcı oturum ikinci Azure AD uygulaması kullanıldığında (`kubectl`). Bu istemci uygulaması, kullanıcıdan kimlik doğrulama isteği alır ve kendi kimlik bilgileri ve izinleri doğrular. İstemci bileşeni kullanmak için Azure AD uygulamanızı oluşturma [az ad app oluşturma] [ az-ad-app-create] komutu:

```azurecli-interactive
clientApplicationId=$(az ad app create \
    --display-name "${aksname}Client" \
    --native-app \
    --reply-urls "https://${aksname}Client" \
    --query appId -o tsv)
```

Kullanan istemci uygulamaları için hizmet sorumlusu oluşturma [az ad sp oluşturma] [ az-ad-sp-create] komutu:

```azurecli-interactive
az ad sp create --id $clientApplicationId
```

Kimlik doğrulaması akışı kullanarak iki uygulama bileşenleri arasında izin vermek için sunucu uygulamasının oAuth2 Kimliğini alın [az ad app show] [ az-ad-app-show] komutu. Bu oAuth2 kimlik, bir sonraki adımda kullanılır.

```azurecli-interactive
oAuthPermissionId=$(az ad app show --id $serverApplicationId --query "oauth2Permissions[0].id" -o tsv)
```

İstemci uygulama izinlerini ekleme ve oAuth2 iletişimini kullanmak için sunucu uygulama bileşenleri akış kullanarak [az ad app izni ekleme] [ az-ad-app-permission-add] komutu. Sonra istemci uygulama sunucusu kullanılarak uygulama ile iletişim için izinleri vermelisiniz [az ad app iznini] [ az-ad-app-permission-grant] komutu:

```azurecli-interactive
az ad app permission add --id $clientApplicationId --api $serverApplicationId --api-permissions $oAuthPermissionId=Scope
az ad app permission grant --id $clientApplicationId --api $serverApplicationId
```

## <a name="deploy-the-cluster"></a>Küme dağıtma

Oluşturulan iki Azure AD uygulamaları ile artık AKS kümesi oluşturun. İlk olarak kullanarak bir kaynak grubu oluşturun. [az grubu oluşturma] [ az-group-create] komutu. Aşağıdaki örnekte kaynak grubu oluşturulmaktadır *EastUS* bölgesi:

Küme için bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location EastUS
```

Azure aboneliğini kullanarak Kiracı kimliği alma [az hesabı show] [ az-account-show] komutu. Ardından, AKS kümesi kullanarak oluşturma [az aks oluşturma] [ az-aks-create] komutu. AKS kümesi oluşturmak için komutu sunucu ve istemci uygulama kimlikleri, sunucu uygulama hizmet sorumlusu gizli anahtarı ve Kiracı Kimliğinizi sağlar:

```azurecli-interactive
tenantId=$(az account show --query tenantId -o tsv)

az aks create \
    --resource-group myResourceGroup \
    --name $aksname \
    --node-count 1 \
    --generate-ssh-keys \
    --aad-server-app-id $serverApplicationId \
    --aad-server-app-secret $serverApplicationSecret \
    --aad-client-app-id $clientApplicationId \
    --aad-tenant-id $tenantId
```

Son olarak, küme kullanarak yönetici kimlik bilgilerini alma [az aks get-credentials] [ az-aks-get-credentials] komutu. Aşağıdaki adımlardan birinde, normal alma *kullanıcı* küme kimlik bilgilerini Azure AD kimlik doğrulaması görmek için akışı çalıştırılırken.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --admin
```

## <a name="create-rbac-binding"></a>RBAC bağlama oluşturma

Bir Azure Active Directory hesabı kullanarak AKS kümesiyle kullanılmadan önce bir rol bağlama veya küme rolünü bağlaması oluşturulması gerekir. *Rolleri* vermek için izinleri tanımlamanıza ve *bağlamaları* bunları istediğiniz kullanıcılar için geçerlidir. Bu atamaları, tüm küme üzerinde veya belirtilen bir ad alanı için uygulanabilir. Daha fazla bilgi için [kullanarak RBAC yetkilendirme][rbac-authorization].

Kullanıcı şu anda kullanarak oturum için kullanıcı asıl adı (UPN) alma [az ad imzalı kullanıcı show] [ az-ad-signed-in-user-show] komutu. Bu kullanıcı hesabı, sonraki adımda Azure AD tümleştirmesi için etkinleştirilir.

```azurecli-interactive
az ad signed-in-user show --query userPrincipalName -o tsv
```

> [!IMPORTANT]
> Aynı Azure AD kiracısında için RBAC bağlama verdiğiniz kullanıcı ise göre izinleri atamak *userPrincipalName*. Kullanıcı, başka bir Azure AD Kiracı, sorgulama ve kullanma *objectID* özelliği bunun yerine.

Adlı bir YAML bildirimi oluşturma `basic-azure-ad-binding.yaml` ve aşağıdaki içeriği yapıştırın. Son satırında değiştirin *userPrincipalName_or_objectId* önceki komuttan UPN veya nesne kimliği çıktısını ile:

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

ClusterRoleBinding kullanarak oluşturduğunuz [kubectl uygulamak] [ kubectl-apply] komut ve dosya YAML bildiriminizi adını belirtin:

```console
kubectl apply -f basic-azure-ad-binding.yaml
```

## <a name="access-cluster-with-azure-ad"></a>Azure AD ile erişim kümesi

Artık geçirelim AKS kümesi için Azure AD kimlik doğrulaması tümleştirmesini test. Ayarlama `kubectl` normal kullanıcı kimlik bilgilerini kullanacak şekilde yapılandırma bağlamı. Bu bağlam, Azure AD üzerinden geri tüm kimlik doğrulama isteklerini geçirir.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --overwrite-existing
```

Artık [kubectl pod'ları alma] [ kubectl-get] komutu tüm ad alanları geneline pod'ları görüntülemek için:

```console
kubectl get pods --all-namespaces
```

Bir web tarayıcısı kullanarak bir Azure AD kimlik bilgilerini kullanarak kimlik doğrulaması isteminde bir oturum alırsınız. Başarıyla kimlik doğrulaması yaptınız sonra `kubectl` komut aşağıdaki örnek çıktıda gösterildiği gibi bu pod'ların AKS kümesinde görüntüler:

```console
$ kubectl get pods --all-namespaces

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BYMK7UXVD to authenticate.

NAMESPACE     NAME                                    READY   STATUS    RESTARTS   AGE
kube-system   coredns-754f947b4-2v75r                 1/1     Running   0          23h
kube-system   coredns-754f947b4-tghwh                 1/1     Running   0          23h
kube-system   coredns-autoscaler-6fcdb7d64-4wkvp      1/1     Running   0          23h
kube-system   heapster-5fb7488d97-t5wzk               2/2     Running   0          23h
kube-system   kube-proxy-2nd5m                        1/1     Running   0          23h
kube-system   kube-svc-redirect-swp9r                 2/2     Running   0          23h
kube-system   kubernetes-dashboard-847bb4ddc6-trt7m   1/1     Running   0          23h
kube-system   metrics-server-7b97f9cd9-btxzz          1/1     Running   0          23h
kube-system   tunnelfront-6ff887cffb-xkfmq            1/1     Running   0          23h
```

Kimlik doğrulama belirteci alınan `kubectl` önbelleğe alınır. Belirtecin süresi dolduğunda veya Kubernetes yapılandırma dosyası yeniden oluşturulana oturum açmak için yalnızca reprompted.

Aşağıdaki örnek çıktıda gösterildiği gibi bir web tarayıcısı kullanarak başarıyla açıldıktan sonra bir yetkilendirme hata iletisini görürseniz, aşağıdaki olası sorunları kontrol edin:

```console
error: You must be logged in to the server (Unauthorized)
```

* Değil de oturumunuz kullanıcı bir *Konuk* (Bu, genellikle durum farklı bir dizin Federasyon oturum açma kullanıyorsanız) Azure AD örneğinde.
* Kullanıcı, 200'den fazla grupların bir üyesi değil.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede gösterilen komutları içeren tam komut dosyası için bkz. [aks'deki Azure AD tümleştirme betik örnekleri deposu][complete-script].

Küme kaynaklarına erişimi denetlemek için Azure AD kullanıcılarını ve gruplarını kullanmak için bkz: [AKS rol tabanlı erişim denetimi ve Azure AD kimlikleri kullanarak küme kaynaklarında erişim denetimi][azure-ad-rbac].

Kubernetes kümelerini güvenliğini sağlama hakkında daha fazla bilgi için bkz. [AKS için erişim ve kimlik seçeneklerini)][rbac-authorization].

Kimlik ve kaynak denetimi ile ilgili en iyi yöntemler için bkz. [en iyi uygulamalar için kimlik doğrulama ve yetkilendirme aks'deki][operator-best-practices-identity].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[complete-script]: https://github.com/Azure-Samples/azure-cli-samples/tree/master/aks/azure-ad-integration/azure-ad-integration.sh

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-ad-app-create]: /cli/azure/ad/app#az-ad-app-create
[az-ad-app-update]: /cli/azure/ad/app#az-ad-app-update
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create
[az-ad-app-permission-add]: /cli/azure/ad/app/permission#az-ad-app-permission-add
[az-ad-app-permission-grant]: /cli/azure/ad/app/permission#az-ad-app-permission-grant
[az-ad-app-permission-admin-consent]: /cli/azure/ad/app/permission#az-ad-app-permission-admin-consent
[az-ad-app-show]: /cli/azure/ad/app#az-ad-app-show
[az-group-create]: /cli/azure/group#az-group-create
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-signed-in-user-show]: /cli/azure/ad/signed-in-user#az-ad-signed-in-user-show
[azure-ad-portal]: azure-ad-integration.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
