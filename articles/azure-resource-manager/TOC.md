# Genel Bakış
## [Resource Manager nedir?](resource-group-overview.md)
## [Desteklenen hizmetler, bölgeler ve API sürümleri](resource-manager-supported-services.md)
## [Resource Manager’ı ve Klasik dağıtımı anlama](resource-manager-deployment-model.md)
## [Öngörücü abonelik idaresi](resource-manager-subscription-governance.md)
## [Kuruluşlar için idare örnekleri](resource-manager-subscription-examples.md)

# Başlarken
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
### Birden çok örnek oluşturmak için döngüyü kopyalama
#### [Temel söz dizimi](resource-group-create-multiple.md)
#### [Sıralı döngü](resource-manager-sequential-loop.md)
#### [Özellik kopyalama](resource-manager-property-copy.md)
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
### [Visual Studio Team Services ile sürekli tümleştirme](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Dağıtım sırasında güvenlik değerlerini geçirme](resource-manager-keyvault-parameter.md)
## Yönet
### [PowerShell](powershell-azure-resource-manager.md)
### [Azure CLI](xplat-cli-azure-resource-manager.md)
### [Portal](resource-group-portal.md)
### [REST API](resource-manager-rest-api.md)
### [Kaynakları düzenlemek için etiketleri kullanma](resource-group-using-tags.md)
### [Kaynakları yeni gruba veya aboneliğe taşıma](resource-group-move-resources.md)
## Erişim Denetleme
### [PowerShell ile hizmet sorumlusu oluşturma](resource-group-authenticate-service-principal.md)
### [Azure CLI 2.0 ile hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Azure CLI 1.0 ile hizmet sorumlusu oluşturma](resource-group-authenticate-service-principal-cli.md)
### [Portal aracılığıyla hizmet sorumlusu oluşturma](resource-group-create-service-principal-portal.md)
### [Aboneliklere erişmek için kimlik doğrulama API’si](resource-manager-api-authentication.md)
### [Kaynakları kilitleme](resource-group-lock-resources.md)
### [Güvenlikle ilgili dikkat edilmesi gerekenler](best-practices-resource-manager-security.md)
## Kaynak ilkeleri ayarlama
### [Kaynak ilkeleri nelerdir?](resource-manager-policy.md)
### [Portal ilke ataması](resource-manager-policy-portal.md)
### [Betik ilke ataması](resource-manager-policy-create-assign.md)
### [Kaynak etiketi ilkeleri](resource-manager-policy-tags.md)
### [Depolama ilkeleri](resource-manager-policy-storage.md)
### [Linux VM ilkeleri](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Windows VM ilkeleri](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
## Denetim ve Sorun Giderme
### [Sık karşılaşılan dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md)
### [Etkinlik günlüklerini görüntüleme](resource-group-audit.md)
### [Dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md)

# Başvuru
## [Şablon işlevleri](resource-group-template-functions.md)
### [Dizi ve nesne işlevleri](resource-group-template-functions-array.md)
### [Karşılaştırma işlevleri](resource-group-template-functions-comparison.md)
### [Dağıtım işlevleri](resource-group-template-functions-deployment.md)
### [Sayısal işlevler](resource-group-template-functions-numeric.md)
### [Kaynak işlevleri](resource-group-template-functions-resource.md)
### [Dize işlevleri](resource-group-template-functions-string.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Azure 2.0 CLI](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [Şablon biçimi](/azure/templates/)
## [REST](/rest/api/resources/)

# Kaynaklar
## [İstekleri azaltma](resource-manager-request-limits.md)
## [Zaman uyumsuz işlemleri izleme](resource-manager-async-operations.md)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Videolar](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)
## [Hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=azure-resource-manager)
