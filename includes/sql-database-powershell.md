
## <a name="start-your-powershell-session"></a>PowerShell oturumunuzu başlatma
Öncelikle, en son [Azure PowerShell](https://msdn.microsoft.com/library/mt619274\(v=azure.300\).aspx) sürümünün yüklü ve çalışıyor olması gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../articles/powershell-install-configure.md).

> [!NOTE]
> SQL Database’in pek çok yeni özelliği yalnızca [Azure Resource Manager dağıtım modeli](../articles/azure-resource-manager/resource-group-overview.md) kullanıldığında desteklenir; bu nedenle örneklerde Resource Manager’la ilgili [Azure SQL Database PowerShell cmdlet’leri](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) kullanılır. Hizmet yönetimi (klasik) dağıtım modeli [Azure SQL Veritabanı Hizmet Yönetimi cmdlet'leri](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) geriye dönük uyumluluk için desteklenir; yine de Resource Manager cmdlet'lerini kullanmanız önerilir.
> 
> 

[**Add-AzureRmAccount**](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) cmdlet’ini çalıştırın; kimlik bilgilerinizi gireceğiniz bir giriş yapma ekranıyla tanınacaksınız. Azure portala giriş yapmak için kullandığınız kimlik bilgilerinin aynısını kullanın.

    Add-AzureRmAccount

Birden fazla aboneliğiniz varsa, PowerShell oturumunuzun hangi aboneliği kullanacağını seçmek için [**Set-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) cmdlet’ini kullanın. Geçerli PowerShell oturumunda hangi aboneliğin kullanıldığını görmek için [**Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx) cmdlet’ini kullanın. Tüm aboneliklerinizi görmek için [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx) cmdlet’ini çalıştırın.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'


<!--HONumber=Nov16_HO2-->


