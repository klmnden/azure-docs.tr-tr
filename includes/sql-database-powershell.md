
## PowerShell oturumunuzu başlatma

Öncelikle, yüklü ve çalışan [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx)’iniz (1.0 veya üstü) olmalıdır. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../articles/powershell-install-configure.md).


>[AZURE.NOTE] SQL Database’in pek çok yeni özelliği yalnızca [Azure Resource Manager dağıtım modeli](../articles/resource-group-overview.md) kullanıldığında desteklenir; bu nedenle örneklerde Resource Manager’la ilgili [Azure SQL Database PowerShell cmdlet’leri](https://msdn.microsoft.com/library/azure/mt574084.aspx) kullanılır. Varolan klasik dağıtım modeli [Azure SQL Database (klasik) cmdlet'leri](https://msdn.microsoft.com/library/azure/dn546723.aspx) geriye dönük uyumluluk için desteklenir; biz yine de Resource Manager cmdlet'lerini kullanmanızı öneririz. 


[**Add-AzureRmAccount**](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet’ini çalıştırın; kimlik bilgilerinizi gireceğiniz bir giriş yapma ekranıyla tanınacaksınız. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

    Add-AzureRmAccount

Birden fazla aboneliğiniz varsa, PowerShell oturumunuzun hangi aboneliği kullanacağını seçmek için [**Set-AzureRmContext**](https://msdn.microsoft.com/library/mt619263.aspx) cmdlet’ini kullanın. Geçerli PowerShell oturumunda hangi aboneliğin kullanıldığını görmek için [**Get-AzureRmContext**](https://msdn.microsoft.com/library/mt619265.aspx) cmdlet’ini kullanın. Tüm aboneliklerinizi görmek için [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/mt619284.aspx) cmdlet’ini çalıştırın.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'




<!--HONumber=Jun16_HO2-->


