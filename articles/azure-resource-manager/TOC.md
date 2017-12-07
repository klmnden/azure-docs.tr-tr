# Genel Bakış
## [Resource Manager nedir?](resource-group-overview.md)
## [Kaynak sağlayıcıları ve türleri](resource-manager-supported-services.md)
## [Resource Manager ve Klasik dağıtım](resource-manager-deployment-model.md)
## [Abonelik idaresi](resource-manager-subscription-governance.md)

# başlarken
## [Şablon oluşturma ve dağıtma](resource-manager-create-first-template.md)
## [Şablonlar için VS Code uzantısı](resource-manager-vscode-extension.md)
## [Resource Manager ile Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Nasıl yapılır?
## Şablon oluşturma
### [Şablon bölümleri](resource-group-authoring-templates.md)
### [Şablonlar için en iyi uygulamalar](resource-manager-template-best-practices.md)
### [Diğer şablonlara bağlantı](resource-group-linked-templates.md)
### [Kaynaklar arasında bağımlılık tanımlama](resource-group-define-dependencies.md)
### [Birden çok örnek oluşturma](resource-group-create-multiple.md)
### [Konum ayarlama](resource-manager-template-location.md)
### [Etiket atama](resource-manager-template-tags.md)
### [Alt kaynak adı ve türünü ayarlama](resource-manager-template-child-resource.md)
### [Güncelleştirme kaynağı](resource-manager-update.md)
### [Parametreler için nesneleri kullanma](resource-manager-objects-as-parameters.md)
### [Bağlı şablonlar arasında durum paylaşma](best-practices-resource-manager-state.md)
### [Şablon tasarlamaya yönelik desenler](best-practices-resource-manager-design-templates.md)


## Dağıtma
### Azure PowerShell
#### [Şablon dağıtma](resource-group-template-deploy.md)
#### [SAS belirteci ile özel şablon dağıtma](resource-manager-powershell-sas-token.md)
#### [Şablonu dışarı aktarma ve yeniden dağıtma](resource-manager-export-template-powershell.md)
### Azure CLI
#### [Şablon dağıtma](resource-group-template-deploy-cli.md)
#### [SAS belirteci ile özel şablon dağıtma](resource-manager-cli-sas-token.md)
#### [Şablonu dışarı aktarma ve yeniden dağıtma](resource-manager-export-template-cli.md)
### Azure portalına
#### [Kaynakları dağıtma](resource-group-template-deploy-portal.md)
#### [Şablonu dışarı aktarma](resource-manager-export-template.md)
### [REST API](resource-group-template-deploy-rest.md)
### [Birden çok kaynak grubu veya abonelik](resource-manager-cross-resource-group-deployment.md)
### [Visual Studio Team Services ile sürekli tümleştirme](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Dağıtım sırasında güvenlik değerlerini geçirme](resource-manager-keyvault-parameter.md)

## Yönet
### [Azure PowerShell](powershell-azure-resource-manager.md)
### [Azure CLI](xplat-cli-azure-resource-manager.md)
### [Azure portal](resource-group-portal.md)
### [REST API](resource-manager-rest-api.md)
### [Kaynakları düzenlemek için etiketleri kullanma](resource-group-using-tags.md)
### [Kaynakları yeni gruba veya aboneliğe taşıma](resource-group-move-resources.md)
### [Yönetim gruplarıyla abonelikleri düzenleme](../billing/billing-enterprise-mgmt-group-overview.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [İdare örnekleri](resource-manager-subscription-examples.md)
### [Yönetilen uygulamalar](../managed-applications/overview.md)

## Erişim Denetleme
### Hizmet sorumlusu oluşturma
#### [Azure PowerShell](resource-group-authenticate-service-principal.md)
#### [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Azure portal](resource-group-create-service-principal-portal.md)
### [Aboneliklere erişmek için kimlik doğrulama API’si](resource-manager-api-authentication.md)
### [Kaynakları kilitleme](resource-group-lock-resources.md)

## Denetim
### [Etkinlik günlüklerini görüntüleme](resource-group-audit.md)
### [Dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md)

## Sorun giderme
### [Sık karşılaşılan dağıtım hataları](resource-manager-common-deployment-errors.md)
#### [AccountNameInvalid](resource-manager-storage-account-name-errors.md)
#### [InvalidTemplate](resource-manager-invalid-template-errors.md)
#### [Linux dağıtım sorunları](../virtual-machines/linux/troubleshoot-deploy-vm.md)
#### [NoRegisteredProviderFound](resource-manager-register-provider-errors.md)
#### [NotFound](resource-manager-not-found-errors.md)
#### [ParentResourceNotFound](resource-manager-parent-resource-errors.md)
#### [Linux için Sağlama ve ayırma sorunları](../virtual-machines/linux/troubleshoot-deployment-new-vm.md)
#### [Windows için Sağlama ve ayırma sorunları](../virtual-machines/windows/troubleshoot-deployment-new-vm.md)
#### [RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)
#### [ReservedResourceName](resource-manager-reserved-resource-name.md)
#### [ResourceQuotaExceeded](resource-manager-quota-errors.md)
#### [SkuNotAvailable](resource-manager-sku-not-available-errors.md)
#### [Windows dağıtım sorunları](../virtual-machines/windows/troubleshoot-deploy-vm.md)
### [Dağıtım hataları anlama](resource-manager-troubleshoot-tips.md)

# Başvuru
## [Şablon biçimi](/azure/templates/)
## [Şablon işlevleri](resource-group-template-functions.md)
### [Dizi ve nesne işlevleri](resource-group-template-functions-array.md)
### [Karşılaştırma işlevleri](resource-group-template-functions-comparison.md)
### [Dağıtım işlevleri](resource-group-template-functions-deployment.md)
### [Mantıksal işlevler](resource-group-template-functions-logical.md)
### [Sayısal işlevler](resource-group-template-functions-numeric.md)
### [Kaynak işlevleri](resource-group-template-functions-resource.md)
### [Dize işlevleri](resource-group-template-functions-string.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Azure CLI](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# Kaynaklar
## [Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=monitoring-management)
## [Fiyatlandırma hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=azure-resource-manager)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [İstekleri azaltma](resource-manager-request-limits.md)
## [Zaman uyumsuz işlemleri izleme](resource-manager-async-operations.md)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
