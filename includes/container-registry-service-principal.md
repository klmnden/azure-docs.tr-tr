## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Kapsayıcı kaydınız erişimi olan bir hizmet sorumlusu oluşturmak için aşağıdaki komut dosyasını kullanabilirsiniz. Güncelleştirme `ACR_NAME` değişken kapsayıcı kaydınız adını ve isteğe bağlı olarak `--role` değeri [az ad sp oluşturma-için-rbac] [ az-ad-sp-create-for-rbac] farklı izinleri vermek için komutu. Bilmeniz gereken [Azure CLI](/cli/azure/install-azure-cli) bu komut dosyasını kullanmak için yüklü.

Betiği çalıştırdıktan sonra hizmet sorumlusuna ilişkin not edin **kimliği** ve **parola**. Kimlik bilgilerini olduktan sonra uygulamaları ve Hizmetleri kapsayıcı kaydınız hizmet sorumlusu olarak kimlik doğrulaması için yapılandırabilirsiniz.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_NAME can be any unique name within your
# subscription (you can use the default below).
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate some values required for subsequent command args
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create the service principal with rights scoped to the registry.
# Default permissions are for both docker push and pull access. Modify the
# '--role' argument value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role reader --query password --output tsv)
SP_APP_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $SP_APP_ID"
echo "Service principal password: $SP_PASSWD"
```

## <a name="use-an-existing-service-principal"></a>Var olan bir hizmet sorumlusunu kullanın

Var olan bir hizmet sorumlusu kayıt defteri erişim vermek için yeni bir rol hizmet sorumlusu atamanız gerekir. Yeni bir hizmet sorumlusu oluşturma ile gibi çekme, anında iletme ve çekme ve sahibi erişim izni verebilir.

Aşağıdaki komut dosyası kullanan [az rol ataması oluşturma] [ az-role-assignment-create] vermek için komut *çekme* bir hizmet asıl izinleri belirttiğiniz `SERVICE_PRINCIPAL_ID` değişkeni. Ayarlama `--role` farklı düzeyde bir erişim vermek istiyorsanız değeri.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_ID is the service principal's 'appId' or
# one of its 'servicePrincipalNames' values.
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_ID=<service-principal-ID>

# Populate value required for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Assign the desired role to the service principal. Modify the '--role' argument
# value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
az role assignment create --assignee $SERVICE_PRINCIPAL_ID --scope $ACR_REGISTRY_ID --role reader
```

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create