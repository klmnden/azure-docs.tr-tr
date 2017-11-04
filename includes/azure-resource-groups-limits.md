| Kaynak | Varsayılan Sınır | Üst Sınır |
| --- | --- | --- |
| Kaynak başına [kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (her kaynak türü) |800 |Kaynak türü değişir |
| Her kaynak grubu dağıtım geçmişinde dağıtımları |800 |800 |
| Her dağıtım kaynakları |800 |800 |
| Yönetim kilit (başına benzersiz kapsamı) |20 |20 |
| Etiket sayısı (her kaynak veya kaynak grubu) |15 |15 |
| Etiket anahtar uzunluğu |512 |512 |
| Etiket değeri uzunluğu |256 |256 |


#### <a name="template-limits"></a>Şablon sınırları

| Değer | Varsayılan Sınır | Üst Sınır |
| --- | --- | --- |
| Parametreler |256 |256 |
| Değişkenler |256 |256 |
| Kaynakları (kopya sayısı dahil) |800 |800 |
| Çıkışları |64 |64 |
| Şablon ifadesi |24.576 karakter |24.576 karakter |
| Dışarı aktarılan şablonları kaynakları |200 |200 | 
| Şablon boyutu |1 MB |1 MB |
| Parametre dosya boyutu |64 KB |64 KB |

İç içe geçmiş bir şablon kullanarak bazı şablonu sınırı aşabilir. Daha fazla bilgi için bkz: [Azure kaynaklarını dağıtırken bağlı şablonları kullanma](../articles/azure-resource-manager/resource-group-linked-templates.md). Parametreler, değişkenleri veya çıkışları sayısını azaltmak için değerlerden bir nesnede birleştirebilirsiniz. Daha fazla bilgi için bkz: [nesneleri parametreleri olarak](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

Her kaynak grubu 800 dağıtımlarının sınıra ulaştıysanız, artık gerekli olmayan geçmişinden dağıtımları silin. Geçmişi ile girişleri silebilirsiniz [az grup dağıtımı Sil](/cli/azure/group/deployment#az_group_deployment_delete) Azure CLI için veya [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) PowerShell'de. Bir giriş dağıtım geçmişinden silinmesi dağıtma kaynakları etkilemez. 