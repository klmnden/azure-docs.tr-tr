<properties
    pageTitle="PowerShell ile yeni SQL Database kurulumu | Microsoft Azure"
    description="PowerShell ile SQL veritabanı oluşturmayı hemen öğrenin. Genel veritabanı kurulum görevleri PowerShell cmdlet'leri ile yönetilebilir."
    keywords="yeni sql veritabanı oluşturma,veritabanı kurulumu"
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
    ms.date="08/19/2016"
    ms.author="sstein"/>


# SQL veritabanı oluşturma ve PowerShell cmdlet'leri ile sık kullanılan veritabanı kurulum görevlerini gerçekleştirme


> [AZURE.SELECTOR]
- [Azure portalına](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



PowerShell cmdlet'lerini kullanarak SQL veritabanı oluşturmayı öğrenin. (Esnek veritabanı oluşturmak için bkz. [PowerShell ile yeni bir esnek veritabanı havuzu oluşturma](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## Veritabanı kurulumu: Kaynak grubu, sunucu ve güvenlik duvarı kuralı oluşturma

Seçili Azure aboneliğiniz için cmdlet çalıştırma erişimi elde ettikten sonra bir sonraki adımda veritabanının oluşturulacağı sunucuyu içeren kaynak grubunu oluşturabilirsiniz. Bir sonraki komutu istediğiniz herhangi bir geçerli konumu kullanacak şekilde düzenleyebilirsiniz. Geçerli konumların bir listesini almak için **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location** komutunu çalıştırın.

Kaynak grubu oluşturmak için şu komutu çalıştırın:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### Sunucu oluşturma

SQL veritabanları Azure SQL Database sunucularında oluşturulur. Sunucu oluşturmak için **New-AzureRmSqlServer** komutunu çalıştırın. Sunucu adınız tüm Azure SQL Veritabanı sunucuları için benzersiz olmalıdır. Sunucu adı zaten alınmışsa hata alırsınız. Bu komutun tamamlanmasının birkaç dakika sürebileceğini unutmayın. Komutu, seçtiğiniz herhangi bir geçerli konumu kullanmak üzere düzenleyebilirsiniz ancak bir önceki adımda oluşturduğunuz kaynak grubunda kullandığınız konumun aynısını kullanmanız gerekir.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Bu komutu çalıştırdığınızda sizden kullanıcı adı ve parola istenir. Azure kimlik bilgilerinizi girmeyin. Bunun yerine, sunucu yöneticisi olarak oluşturmak üzere kullanıcı adını ve parolayı girin. Bu makalenin alt kısmındaki betik, kodda sunucu kimlik bilgilerinin nasıl ayarlanacağını gösterir.

Sunucu başarılı bir şekilde oluşturulduktan sonra sunucu bilgileri görüntülenir.

### Sunucuya erişim izni vermek üzere bir sunucu güvenlik duvarı kuralı yapılandırma

Sunucuya erişmek için bir güvenlik duvarı kuralı oluşturmanız gerekir. Başlangıç IP adresini ve bitiş IP adresini bilgisayarınız için geçerli değerlerle değiştirerek aşağıdaki komutu çalıştırın.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Kuralı başarılı bir şekilde oluşturulduktan sonra güvenlik duvarı kuralı bilgileri görüntülenir.

Diğer Azure hizmetlerinin sunucuya erişimine izin vermek için bir güvenlik duvarı kuralı ekleyin ve hem StartIpAddress hem de EndIpAddress değerini 0.0.0.0 olarak ayarlayın. Bu kural herhangi bir Azure aboneliğinden gelen Azure trafiğinin sunucuya erişmesini sağlar.

Daha fazla bilgi için bkz. [Azure SQL Database Güvenlik Duvarı](sql-database-firewall-configure.md).


## SQL veritabanı oluşturma

Artık yapılandırılmış bir kaynak grubunuz, sunucunuz ve güvenlik duvarı kuralınız olduğunuza göre sunucuya erişebilirsiniz.

Şu komut, S1 performans düzeyine sahip Standart hizmet katmanında (boş) bir SQL veritabanı oluşturur:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Veritabanı başarılı bir şekilde oluşturulduktan sonra veritabanı bilgileri görüntülenir.

## SQL veritabanı PowerShell betiği oluşturma

Aşağıdaki PowerShell betiğiyle bir SQL veritabanı ve onun tüm bağımlı kaynaklarını oluşturabilirsiniz. Tüm `{variables}` değerlerini, aboneliğinize ve kaynaklarınıza özgü değerlerle değiştirin. (Kendi değerlerinizi belirlediğinizde **{}** öğesini kaldırın.)

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## Sonraki adımlar
SQL veritabanı oluşturduktan ve temel veritabanı kurulumu görevlerini gerçekleştirdikten sonra şu işlem için hazır duruma gelirsiniz:

- [SQL Veritabanı'nı PowerShell ile yönetme](sql-database-command-line-tools.md)
- [SQL Server Management Studio ile SQL Database'e bağlanma ve bir örnek T-SQL sorgusu gerçekleştirme](sql-database-connect-query-ssms.md)


## Ek Kaynaklar

- [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)



<!--HONumber=Sep16_HO3-->


