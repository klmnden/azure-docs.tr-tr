## <a name="deploy-your-function-code"></a>İşlev kodunuzu dağıtma  

Yeni işlev uygulamanızda işlevi kodunuzu oluşturmanın birkaç yolu vardır. Bu konu başlığında, GitHub’daki bir örnek depoya bağlanılır. Daha önce olduğu gibi, aşağıdaki kodda `<app_name>` yer tutucusunu oluşturduğunuz işlev uygulamasının adıyla değiştirin. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Dağıtım kaynağı ayarladıktan sonra Azure CLI bilgileri (null değerler için okunabilirlik kaldırıldı) aşağıdaki örneğe benzer şekilde gösterir:

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "kind": null,
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```
