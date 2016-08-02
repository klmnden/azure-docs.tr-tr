<properties 
    pageTitle="PowerShell ile yeni SQL Database kurulumu | Microsoft Azure" 
    description="PowerShell ile nasıl yeni bir SQL veritabanı oluşturacağınızı öğrenin. Genel veritabanı kurulum görevleri PowerShell cmdlet'leri ile yönetilebilir." 
    keywords="create new sql database,database setup"
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="05/09/2016"
    ms.author="sstein"/>

# Yeni bir SQL veritabanı oluşturma ve PowerShell cmdlet'leri ile genel veritabanı kurulum görevlerini gerçekleştirme 


> [AZURE.SELECTOR]
- [Azure Portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



PowerShell cmdlet'lerini kullanarak SQL veritabanı oluşturmayı öğrenin. (Esnek veritabanı oluşturmak için bkz. [PowerShell ile yeni bir esnek veritabanı havuzu oluşturma](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## Veritabanı kurulumu: Kaynak grubu, sunucu ve güvenlik duvarı kuralı oluşturma

Artık seçili Azure aboneliğiniz için cmdlet çalıştırma erişiminiz olduğuna göre bir sonraki adımda veritabanının oluşturulacağı sunucuyu içeren kaynak grubunu oluşturabilirsiniz. Bir sonraki komutu istediğiniz herhangi bir geçerli konumu kullanacak şekilde düzenleyebilirsiniz. Geçerli konumların bir listesini almak için **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location** komutunu çalıştırın.

Yeni bir kaynak grubu oluşturmak için şu komutu çalıştırın:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "West US"

Yeni kaynak grubunu başarılı bir şekilde oluşturduktan sonra, ekranda **ProvisioningState : Succeeded** öğesini de içeren bilgileri görürsünüz.


### Sunucu oluşturma 

SQL Database'ler, Azure SQL Database sunucularında oluşturulur. Yeni bir sunucu oluşturmak için **New-AzureRmSqlServer** komutunu çalıştırın. ServerName değerini sunucunuzun adıyla değiştirin. Sunucu adının tüm Azure SQL sunucuları arasında genel olarak benzersiz olması gerekir, bu nedenle sunucu adı daha önce alınmışsa burada bir hata ile karşılaşırsınız. Bu komutun tamamlanmasının birkaç dakika sürebileceğini unutmayın. Komutu, seçtiğiniz herhangi bir geçerli konumu kullanmak üzere düzenleyebilirsiniz ancak bir önceki adımda oluşturduğunuz kaynak grubunda kullandığınız konumun aynısını kullanmanız gerekir.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "West US" -ServerVersion "12.0"

Bu komutu çalıştırdığınızda **Kullanıcı adı** ve **Parola** girmenizi isteyen bir pencere açılır. Bunlar Azure kimlik bilgileriniz değildir, yeni sunucu için oluşturmak istediğiniz yönetici kimlik bilgileriniz olacak olan kullanıcı adını ve parolayı girin.

Sunucu başarılı bir şekilde oluşturulduktan sonra sunucu bilgileri görüntülenir.

### Sunucuya erişim izni vermek üzere bir sunucu güvenlik duvarı kuralı yapılandırma

Sunucuya erişmek için bir güvenlik duvarı kuralı oluşturun. Başlangıç IP adresini ve bitiş IP adresini bilgisayarınız için geçerli değerlerle değiştirerek aşağıdaki komutu çalıştırın.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Kuralı başarılı bir şekilde oluşturulduktan sonra güvenlik duvarı kuralı bilgileri görüntülenir.

Diğer Azure hizmetlerinin sunucuya erişimine izin vermek için bir güvenlik duvarı kuralı ekleyin ve hem StartIpAddress hem de EndIpAddress değerini 0.0.0.0 olarak ayarlayın. Bu işlem sonrasında tüm Azure aboneliklerinden gelen Azure trafiğinin sunucuya erişimine izin verildiğini unutmayın.

Daha fazla bilgi için bkz. [Azure SQL Database Güvenlik Duvarı](sql-database-firewall-configure.md).


## Yeni bir SQL veritabanı oluşturma

Artık yapılandırılmış bir kaynak grubunuz, sunucunuz ve güvenlik duvarı kuralınız olduğunuza göre sunucuya erişebilirsiniz.

Şu komut ile S1 performans düzeyine sahip Standart hizmet katmanında yeni (boş) bir SQL veritabanı oluşturulur:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Veritabanı başarılı bir şekilde oluşturulduktan sonra veritabanı bilgileri görüntülenir.

## Yeni bir SQL veritabanı PowerShell betiği oluşturma

    $SubscriptionId = "4cac86b0-1e56-bbbb-aaaa-000000000000"
    $ResourceGroupName = "resourcegroupname"
    $Location = "Japan West"
    
    $ServerName = "uniqueservername"
    
    $FirewallRuleName = "rule1"
    $FirewallStartIP = "192.168.0.0"
    $FirewallEndIp = "192.168.0.0"
    
    $DatabaseName = "database1"
    $DatabaseEdition = "Standard"
    $DatabasePerfomanceLevel = "S1"
    
    
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId
    
    $ResourceGroup = New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    $Server = New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName -ServerName $ServerName -Location $Location -ServerVersion "12.0"
    
    $FirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $ServerName -FirewallRuleName $FirewallRuleName -StartIpAddress $FirewallStartIP -EndIpAddress $FirewallEndIp
    
    $SqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -Edition $DatabaseEdition -RequestedServiceObjectiveName $DatabasePerfomanceLevel
    
    $SqlDatabase
    


## Sonraki adımlar
Yeni bir SQL veritabanı oluşturduktan ve temel veritabanı kurulumu görevlerini gerçekleştirdikten sonra şu işlem için hazır duruma gelirsiniz:

- [SQL Server Management Studio ile SQL Database'e bağlanma ve bir örnek T-SQL sorgusu gerçekleştirme](sql-database-connect-query-ssms.md)


## Ek Kaynaklar

- [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)



<!--HONumber=Jun16_HO2-->


