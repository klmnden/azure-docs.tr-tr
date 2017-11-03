Dağıtım sırasında parametre değerleri geçirmek için bir parametre dosyası kullanmak, bir JSON dosyası formatı ile aşağıdaki örneğe benzer şekilde oluşturmanız gerekir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

Parametre dosyanın boyutu 64 KB'den büyük olamaz.

Bir parametre (örneğin, parola) için önemli bir değer sağlamanız gerekiyorsa, bu değer bir anahtar Kasası'na ekleyin. Anahtar kasası, önceki örnekte gösterildiği gibi dağıtım sırasında alın. Daha fazla bilgi için bkz: [dağıtımı sırasında güvenli değerlerini geçirin](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 

