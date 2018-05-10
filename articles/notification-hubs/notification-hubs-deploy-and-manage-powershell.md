---
title: PowerShell kullanarak Notification Hubs’ı Dağıtma ve Yönetme
description: Oluşturma ve Otomasyon için PowerShell kullanarak bildirim hub'ları yönetme
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: d2350d8021925278d6362c8227d408476a569319
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>PowerShell kullanarak Notification Hubs’ı Dağıtma ve Yönetme
## <a name="overview"></a>Genel Bakış
Bu makalede oluşturma ve yönetme Azure bildirim PowerShell kullanarak hub'ları nasıl kullanılacağı gösterilmektedir. Bu konuda aşağıdaki ortak Otomasyon görevler gösterilmektedir.

* Bildirim hub'ı Oluştur
* Kimlik Bilgilerini Ayarla

Ayrıca, bildirim hub'larınız için yeni bir hizmet veri yolu ad alanı oluşturmanız gerekiyorsa, bkz: [PowerShell ile yönetme Service Bus](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Bildirim hub'ları yönetme, doğrudan Azure PowerShell ile dahil edilen cmdlet'ler tarafından desteklenmiyor. En iyi powershell'den Microsoft.Azure.NotificationHubs.dll derleme başvurusu yaklaşımdır. Derleme ile dağıtılmış [Microsoft Azure bildirim hub'ları NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure abonelik tabanlı bir platformdur. Bir aboneliği edinme hakkında daha fazla bilgi için bkz: [satın alma seçenekleri], [üye teklifleri], veya [ücretsiz deneme].
* Azure PowerShell ile bir bilgisayar. Yönergeler için bkz: [yükleyin ve Azure PowerShell yapılandırma].
* PowerShell komut dosyaları, NuGet paketlerini ve .NET Framework genel anlama.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Hizmet veri yolu için .NET derlemesine başvuru dahil
Azure bildirim hub'ları yönetme henüz Azure PowerShell'de PowerShell cmdlet'leri ile dahil değildir. Bildirim hub'ları sağlamak için sağlanan .NET istemci kullanabilirsiniz [Microsoft Azure bildirim hub'ları NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Öncelikle, kodunuzu bulabilir emin olun **Microsoft.Azure.NotificationHubs.dll** Visual Studio projesi bir NuGet paketi olarak yüklü derleme. Esnek olması için komut dosyası, aşağıdaki adımları gerçekleştirir:

1. Hangi çağrıldı yolu belirler.
2. Adlı bir klasör bulana kadar yol geçtiğini `packages`. Visual Studio projeleri için NuGet paketlerini yüklediğinizde bu klasör oluşturulur.
3. Özyinelemeli olarak arar `packages` adlı bir derleme için klasör **Microsoft.Azure.NotificationHubs.dll**.
4. Böylece daha sonra kullanmak üzere türleri kullanılabilir derlemeye başvurur.

İşte bu adımları bir PowerShell Betiği nasıl uygulanır:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>NamespaceManager sınıfı oluşturma
Bildirim hub'ları sağlamak için bir örneğini oluşturmak [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) SDK sınıfından. 

Kullanabileceğiniz [Get-AzureSBAuthorizationRule] bir bağlantı dizesi sağlamak için kullanılan bir yetkilendirme kuralı almak için Azure PowerShell ile dahil cmdlet'i. Bir başvuru `NamespaceManager` örneği depolanır `$NamespaceManager` değişkeni. `$NamespaceManager` bildirim hub'ı sağlamak için kullanılır.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Yeni bir Notification Hub sağlama
Yeni bir notification hub sağlamak için kullanmanız [bildirim hub'ları için .NET API].

Bu komut dosyası bölümünde dört yerel değişkenleri ayarlayın. 

1. `$Namespace` : Bu ad alanı için bir bildirim hub'ı oluşturmak istediğiniz kümesinin adı.
2. `$Path` : Bu yol yeni bildirim hub'ı adına ayarlayın.  Örneğin, "MyHub".    
3. `$WnsPackageSid` : Bu paket SID'si Windows uygulamanız için koymak gelen [Windows Geliştirme Merkezi](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Bu gizli anahtarı için Windows uygulamanızdan Ayarla [Windows Geliştirme Merkezi](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Bu değişkenler ad alanınıza bağlanın ve bir Windows uygulaması için WNS kimlik bilgilerine sahip Windows Bildirim Hizmetleri (WNS) bildirimleri işlemek üzere yapılandırılmış yeni bir Notification Hub oluşturmak için kullanılır. Paket alma hakkında bilgi için SID ve gizli anahtar bakın, [Notification Hubs ile çalışmaya başlama](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Öğreticisi. 

* Kod parçacığı kullanan `NamespaceManager` bildirim hub'ı tarafından tanımlanan olmadığını görmek için nesne `$Path` bulunmaktadır.
* Yoksa, komut dosyası oluşturur `NotificationHubDescription` WNS ile kimlik bilgileri ve olarak geçirin `NamespaceManager` sınıfı `CreateNotificationHub` yöntemi.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Ek Kaynaklar
* [Service Bus PowerShell ile yönetme](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [Service Bus kuyrukları, konuları ve abonelikleri bir PowerShell Betiği kullanılarak oluşturma](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Hizmet veri yolu Namespace ve bir PowerShell komut dosyası kullanarak bir Event Hub oluşturma](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Bazı hazır kodlar da indirme için kullanılabilir:

* [Hizmet veri yolu PowerShell betikleri](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[satın alma seçenekleri]: http://azure.microsoft.com/pricing/purchase-options/
[üye teklifleri]: http://azure.microsoft.com/pricing/member-offers/
[ücretsiz deneme]: http://azure.microsoft.com/pricing/free-trial/
[yükleyin ve Azure PowerShell yapılandırma]: /powershell/azureps-cmdlets-docs
[bildirim hub'ları için .NET API]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx

