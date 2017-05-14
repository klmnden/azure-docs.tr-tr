Bu yapılandırmaya başlamadan önce Azure hesabınızda oturum açmalısınız. Bu cmdlet, Azure hesabınıza ilişkin oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir. Daha fazla bilgi için [Windows PowerShell’i Resource Manager ile kullanma](../articles/powershell-azure-resource-manager.md) konusuna bakın.

PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Login-AzureRmAccount
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

```powershell
Get-AzureRmSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```