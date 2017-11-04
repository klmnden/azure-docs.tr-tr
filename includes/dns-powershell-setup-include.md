## <a name="set-up-azure-powershell-for-azure-dns"></a>Azure DNS için Azure PowerShell ayarlayın

### <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Azure Resource Manager PowerShell cmdlet'lerinin en yeni sürümünü yüklemeniz gerekir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Daha fazla bilgi için bkz: [PowerShell kullanarak Resource Manager ile](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a>Aboneliği seçme
 
Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzureRmSubscription
```

Hangi Azure aboneliğinizin kullanılacağını seçin.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu konum, kaynak grubundaki kaynaklar için varsayılan konum olarak kullanılır. Ancak tüm DNS kaynakları bölgesel değil de global olduğundan, kaynak grubu konumu seçiminin Azure DNS üzerinde hiçbir etkisi yoktur.

Var olan bir kaynak grubunu kullanıyorsanız bu adımı atlayabilirsiniz.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Azure DNS hizmeti, Microsoft.Network kaynak sağlayıcısı tarafından yönetilir. Azure DNS'yi kullanabilmeniz için, Azure aboneliğinizin bu kaynak sağlayıcısını kullanmak üzere kayıtlı olması gerekir. Bu, her bir abonelik için tek seferlik bir işlemdir.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```