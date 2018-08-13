
## <a name="start-your-powershell-session"></a>PowerShell oturumunuzu başlatma
En son ilk gerek [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) yüklü ve çalışır. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Bu konuda kullanım örnekleri [Azure Resource Manager dağıtım modeli](../articles/azure-resource-manager/resource-group-overview.md), bu nedenle örneklerde [Azure Resource Manager cmdlet'leri](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Çalıştırma [ **Connect-AzureRmAccount** ](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet'i ve kimlik bilgilerinizi girmeniz için bir oturum açma ekranı gösterilir. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

    Connect-AzureRmAccount

Kullanan birden fazla aboneliğiniz varsa [ **Set-AzureRmContext** ](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext) cmdlet'ini kullanması gereken PowerShell oturumunuzun hangi aboneliği seçin. Geçerli PowerShell oturumunda hangi aboneliğin kullanıldığını görmek için [**Get-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext) cmdlet’ini kullanın. Tüm aboneliklerinizi görmek için [**Get-AzureRmSubscription**](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/get-azurermsubscription) cmdlet’ini çalıştırın.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

