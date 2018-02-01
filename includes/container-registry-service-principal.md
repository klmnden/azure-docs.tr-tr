## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Kapsayıcı kaydınız erişimi olan bir hizmet sorumlusu oluşturmak için aşağıdaki komut dosyasını kullanabilirsiniz. Güncelleştirme `ACR_NAME` değişken kapsayıcı kaydınız adını ve isteğe bağlı olarak `--role` değeri [az ad sp oluşturma-için-rbac] [ az-ad-sp-create-for-rbac] farklı izinleri vermek için komutu. Bilmeniz gereken [Azure CLI](/cli/azure/install-azure-cli) bu komut dosyasını kullanmak için yüklü.

Betiği çalıştırdıktan sonra hizmet sorumlusuna ilişkin not edin **kimliği** ve **parola**. Kimlik bilgilerini olduktan sonra uygulamaları ve Hizmetleri kapsayıcı kaydınız hizmet sorumlusu olarak kimlik doğrulaması için yapılandırabilirsiniz.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Var olan bir hizmet sorumlusunu kullanın

Var olan bir hizmet sorumlusu kayıt defteri erişim vermek için yeni bir rol hizmet sorumlusu atamanız gerekir. Yeni bir hizmet sorumlusu oluşturma ile gibi çekme, anında iletme ve çekme ve sahibi erişim izni verebilir.

Aşağıdaki komut dosyası kullanan [az rol ataması oluşturma] [ az-role-assignment-create] vermek için komut *çekme* bir hizmet asıl izinleri belirttiğiniz `SERVICE_PRINCIPAL_ID` değişkeni. Ayarlama `--role` farklı düzeyde bir erişim vermek istiyorsanız değeri.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

Bunların her ikisi de komut dosyaları GitHub yanı sıra üzerinde sürümleri için PowerShell bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
