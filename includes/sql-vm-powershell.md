
## <a name="start-your-powershell-session"></a>PowerShell oturumunuzu başlatma
En son ilk gerek [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) yüklü ve çalışır. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Bu konuda kullanım örnekleri [Azure Resource Manager dağıtım modeli](../articles/azure-resource-manager/resource-group-overview.md), bu nedenle örneklerde [Azure Resource Manager cmdlet'lerini](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Çalıştırma [ **Connect-AzureRmAccount** ](http://msdn.microsoft.com/library/mt619267.aspx) cmdlet'ini sunulabilir kimlik bilgilerinizi girmeniz için bir oturum ile ekranında. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

    Connect-AzureRmAccount

Kullanan birden çok aboneliğiniz varsa [ **Set-AzureRmContext** ](http://msdn.microsoft.com/library/mt619263.aspx) cmdlet'i kullanması gereken PowerShell oturumunuzun hangi aboneliği seçin. Geçerli PowerShell oturumunda hangi aboneliğin kullanıldığını görmek için [**Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx) cmdlet’ini kullanın. Tüm aboneliklerinizi görmek için [**Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx) cmdlet’ini çalıştırın.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

