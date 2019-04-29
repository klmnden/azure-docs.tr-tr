---
title: PowerShell kullanarak Notification Hubs’ı Dağıtma ve Yönetme
description: Oluşturma ve Otomasyon için PowerShell kullanarak Notification hubs'ı yönetme
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 4dbbaeea736dd46478ad9992201ea28bd7bfc2ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61457847"
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Dağıtma ve PowerShell kullanarak notification hubs'ı yönetme

## <a name="overview"></a>Genel Bakış

Bu makale oluşturma ve yönetme Azure PowerShell kullanarak Notification Hubs nasıl kullanılacağını gösterir. Bu makalede aşağıdaki genel otomasyon görevleri gösterilmektedir.

- Bildirim hub'ı oluşturma
- Kimlik Bilgilerini Ayarla

Ayrıca, bildirim hub'ları için yeni bir service bus ad alanı oluşturmak ihtiyacınız varsa bkz [PowerShell ile Service Bus'ı yönetme](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Bildirim hub'ları yönetme, doğrudan Azure PowerShell ile dahil edilen cmdlet'ler tarafından desteklenmiyor. En iyi yaklaşım powershell'den Microsoft.Azure.NotificationHubs.dll derlemeye başvuru sağlamaktır. Derleme ile dağıtılmış [Microsoft Azure Notification Hubs NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure abonelik tabanlı bir platformdur. Bir aboneliği edinme hakkında daha fazla bilgi için bkz. [satın alma seçenekleri], [Üye teklifleri], veya [ücretsiz deneme].
- Azure PowerShell ile bir bilgisayar. Yönergeler için [için Notification Hubs .NET API].
- PowerShell betikleri, NuGet paketlerini ve .NET Framework genel yaklaşım.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Service Bus için .NET derlemesine bir başvuru da dahil olmak üzere

Azure Notification hubs'ı yönetme henüz Azure PowerShell'in PowerShell cmdlet'leriyle dahil değildir. Bildirim hub'ları sağlamak için sağlanan .NET istemci kullanabilirsiniz [Microsoft Azure Notification Hubs NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

İlk olarak, kodunuzu bulabilir emin **Microsoft.Azure.NotificationHubs.dll** Visual Studio projesi bir NuGet paketi olarak yüklenen derleme. Esnek olmak için betik aşağıdaki adımları gerçekleştirir:

1. Başlangıçtan çağrıldı yolu belirler.
2. Adlı bir klasör bulana kadar yolu geri döner `packages`. Visual Studio projeleri için NuGet paketlerini yükleme sırasında bu klasör oluşturulur.
3. Yinelemeli olarak arar `packages` adlı bir derleme için klasörü `Microsoft.Azure.NotificationHubs.dll`.
4. Daha sonra kullanmak üzere türleri kullanılabilir olacak şekilde derlemeye başvurur.

Bu adımları bir PowerShell komut dosyası nasıl uygulandığını aşağıda verilmiştir:

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

## <a name="create-the-namespacemanager-class"></a>Oluşturma `NamespaceManager` sınıfı

Bildirim hub'ları sağlamak için bir örneğini oluşturmak [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) SDK'dan sınıfı.

Kullanabileceğiniz [Get-AzureSBAuthorizationRule] cmdlet'i bir bağlantı dizesi sağlamak için kullanılan bir yetkilendirme kuralı almak için Azure PowerShell ile dahil. Bir başvuru `NamespaceManager` örneği depolanan `$NamespaceManager` değişkeni. `$NamespaceManager` bildirim hub'ı sağlamak için kullanılır.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-a-new-notification-hub"></a>Yeni bir bildirim hub'ı sağlama

Yeni bir bildirim hub'ı sağlamak için kullanın [Bildirim hub'ları için .NET API'si].

Bu betik kısmında dört yerel değişkenleri ayarlayın.

1. `$Namespace`: Bu, bildirim hub'ı oluşturmak istediğiniz ad alanı adına ayarlayın.
2. `$Path`: Bu yol, yeni bildirim hub'ı adına ayarlayın.  Örneğin, "MyHub".
3. `$WnsPackageSid`: Paket SID'si Windows uygulamanız için bunu ayarlayın gelen [Windows Dev Center](https://developer.microsoft.com/en-us/windows).
4. `$WnsSecretkey`: Gizli anahtar için Windows uygulamanız için bunu [Windows Dev Center](https://developer.microsoft.com/en-us/windows).

Bu değişkenler, ad alanınıza bağlanın ve bir Windows uygulaması için WNS kimlik bilgileriyle Windows Bildirim Hizmetleri (WNS) bildirimleri işlemek için yapılandırılan yeni bir Notification Hub'ı oluşturmak için kullanılır. Paket edinme hakkında bilgi için SID ve gizli anahtar bakın [Notification Hubs ile çalışmaya başlama](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) öğretici.

- Kod parçacığını kullanır `NamespaceManager` bildirim hub'ı tarafından tanımlanan, kontrol etmek için nesne `$Path` bulunmaktadır.
- Yoksa, betik oluşturur `NotificationHubDescription` WNS ile kimlik bilgileri ve buna ileten `NamespaceManager` sınıfı `CreateNotificationHub` yöntemi.

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

- [Service Bus’ı PowerShell ile yönetme](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Service Bus kuyrukları, konular ve abonelikler bir PowerShell betiğini kullanarak oluşturma](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Bir Service Bus Namespace ve bir PowerShell betiğini kullanarak bir olay hub'ı oluşturma](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Kullanıma hazır bazı komut dosyaları indirme için de kullanılabilir:

- [Service Bus PowerShell betikleri](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[Satın alma seçenekleri]: https://azure.microsoft.com/pricing/purchase-options/
[Üye teklifleri]: https://azure.microsoft.com/pricing/member-offers/
[Ücretsiz deneme]: https://azure.microsoft.com/pricing/free-trial/
[için Notification Hubs .NET API]: /powershell/azureps-cmdlets-docs
[Bildirim hub'ları için .NET API'si]: https://docs.microsoft.com/dotnet/api/overview/azure/notification-hubs?view=azure-dotnet
[Get-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbnamespace
[New-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace
[Get-AzureSBAuthorizationRule]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule
