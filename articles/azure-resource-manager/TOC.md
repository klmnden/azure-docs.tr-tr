# Genel Bakış
## [Resource Manager nedir?](resource-group-overview.md)
## [Kaynak sağlayıcıları ve türleri](resource-manager-supported-services.md)
## [Resource Manager ve Klasik dağıtım](resource-manager-deployment-model.md)
## [Abonelik idaresi](resource-manager-subscription-governance.md)
## [Yönetilen Uygulamalar](managed-application-overview.md)

# başlarken
## [Şablonu dışarı aktarma](resource-manager-export-template.md)
## [İlk şablonunuzu oluşturma](resource-manager-create-first-template.md)
## [Resource Manager ile Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Örnekler
## PowerShell
### [Şablon dağıtma](resource-manager-samples-powershell-deploy.md)

## Azure CLI
### [Şablon dağıtma](resource-manager-samples-cli-deploy.md)

# Nasıl yapılır?
## Şablon oluşturma
### [Şablonlar için en iyi uygulamalar](resource-manager-template-best-practices.md)
### [Şablon bölümleri](resource-group-authoring-templates.md)
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
### PowerShell
#### [Şablon dağıtma](resource-group-template-deploy.md)
#### [SAS belirteci ile özel şablon dağıtma](resource-manager-powershell-sas-token.md)
#### [Şablonu dışarı aktarma ve yeniden dağıtma](resource-manager-export-template-powershell.md)
### Azure CLI
#### [Şablon dağıtma](resource-group-template-deploy-cli.md)
#### [SAS belirteci ile özel şablon dağıtma](resource-manager-cli-sas-token.md)
#### [Şablonu dışarı aktarma ve yeniden dağıtma](resource-manager-export-template-cli.md)
### [Portal](resource-group-template-deploy-portal.md)
### [REST API](resource-group-template-deploy-rest.md)
### [Çapraz kaynak grubu dağıtımı](resource-manager-cross-resource-group-deployment.md)
### [Visual Studio Team Services ile sürekli tümleştirme](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Dağıtım sırasında güvenlik değerlerini geçirme](resource-manager-keyvault-parameter.md)

## Yönet
### [PowerShell](powershell-azure-resource-manager.md)
### [Azure CLI](xplat-cli-azure-resource-manager.md)
### [Portal](resource-group-portal.md)
### [REST API](resource-manager-rest-api.md)
### [Kaynakları düzenlemek için etiketleri kullanma](resource-group-using-tags.md)
### [Kaynakları yeni gruba veya aboneliğe taşıma](resource-group-move-resources.md)
### [İdare örnekleri](resource-manager-subscription-examples.md)

## Erişim Denetleme
### Hizmet sorumlusu oluşturma
#### [PowerShell](resource-group-authenticate-service-principal.md)
#### [Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Azure CLI 1.0](resource-group-authenticate-service-principal-cli.md)
#### [Portal](resource-group-create-service-principal-portal.md)
### [Aboneliklere erişmek için kimlik doğrulama API’si](resource-manager-api-authentication.md)
### [Kaynakları kilitleme](resource-group-lock-resources.md)

## Kaynak ilkeleri ayarlama
### [Kaynak ilkeleri nelerdir?](resource-manager-policy.md)
### [İlke atamak için portalı kullanma](resource-manager-policy-portal.md)
### [İlke atamak için betikleri kullanma](resource-manager-policy-create-assign.md)
### Örnekler
#### [Kaynak etiketleri](resource-manager-policy-tags.md)
#### [Depolama](resource-manager-policy-storage.md)
#### [Linux VM](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Windows VM](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)

## Yönetilen uygulamaları kullanma
### [Yönetilen uygulamaları yayımlama](managed-application-publishing.md)
### [Yönetilen uygulamaları tüketme](managed-application-consumption.md)
### [UI tanımları oluşturma](managed-application-createuidefinition-overview.md)

## Denetim
### [Etkinlik günlüklerini görüntüleme](resource-group-audit.md)
### [Dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md)

## Sorun giderme
### [Sık karşılaşılan dağıtım hataları](resource-manager-common-deployment-errors.md)

# Başvuru
## [Şablon biçimi](/azure/templates/)
## [Şablon işlevleri](resource-group-template-functions.md)
### [Dizi ve nesne işlevleri](resource-group-template-functions-array.md)
### [Karşılaştırma işlevleri](resource-group-template-functions-comparison.md)
### [Dağıtım işlevleri](resource-group-template-functions-deployment.md)
### [Sayısal işlevler](resource-group-template-functions-numeric.md)
### [Kaynak işlevleri](resource-group-template-functions-resource.md)
### [Dize işlevleri](resource-group-template-functions-string.md)
## [UI tanımı işlevleri](managed-application-createuidefinition-functions.md)
## [UI tanımı öğeleri](managed-application-createuidefinition-elements.md)
### [Microsoft.Common.DropDown](managed-application-microsoft-common-dropdown.md)
### [Microsoft.Common.FileUpload](managed-application-microsoft-common-fileupload.md)
### [Microsoft.Common.OptionsGroup](managed-application-microsoft-common-optionsgroup.md)
### [Microsoft.Common.PasswordBox](managed-application-microsoft-common-passwordbox.md)
### [Microsoft.Common.Section](managed-application-microsoft-common-section.md)
### [Microsoft.Common.TextBox](managed-application-microsoft-common-textbox.md)
### [Microsoft.Compute.CredentialsCombo](managed-application-microsoft-compute-credentialscombo.md)
### [Microsoft.Compute.SizeSelector](managed-application-microsoft-compute-sizeselector.md)
### [Microsoft.Compute.UserNameTextBox](managed-application-microsoft-compute-usernametextbox.md)
### [Microsoft.Network.PublicIpAddressCombo](managed-application-microsoft-network-publicipaddresscombo.md)
### [Microsoft.Network.VirtualNetworkCombo](managed-application-microsoft-network-virtualnetworkcombo.md)
### [Microsoft.Storage.MultiStorageAccountCombo](managed-application-microsoft-storage-multistorageaccountcombo.md)
### [Microsoft.Storage.StorageAccountSelector](managed-application-microsoft-storage-storageaccountselector.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Azure CLI](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# Kaynaklar
## [İstekleri azaltma](resource-manager-request-limits.md)
## [Zaman uyumsuz işlemleri izleme](resource-manager-async-operations.md)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=azure-resource-manager)
