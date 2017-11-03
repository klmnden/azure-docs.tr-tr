---
title: "Uyarılar ve Azure işlevleri öngörülü ağ izleme yapmak için paket yakalama kullanın | Microsoft Docs"
description: "Bu makalede Azure Ağ İzleyicisi ile bir uyarı tetiklenen paket yakalama oluşturma"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 1b3da4d6e4593f3c71995ef9331fcea2d5b6ec19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Paket yakalama öngörülü ağ izleme için uyarıları ve Azure işlevleri kullanın

Ağ İzleyicisi paket yakalama trafiği sanal makineler ve izlemek için yakalama oturumları oluşturur. Yakalama dosyası, izlemek istediğiniz trafiği izlemek için tanımlanmış bir filtre olabilir. Bu veriler, ardından bir depolama blob veya yerel olarak Konuk makinedeki depolanır.

Bu özellik, Azure işlevleri gibi diğer Otomasyon senaryolardan uzaktan başlatılabilir. Paket yakalama üzerinde tanımlı ağ anormallikleri dayalı öngörülü yakalamaları çalıştırmak için yeteneği verir. Diğer kullanımlar ağ yetkisiz erişim, hata ayıklama istemci-sunucu iletişimleri ve daha fazlasını hakkında bilgi alma ağ istatistikleri toplama içerir.

Azure'da dağıtılan kaynakları 7/24 çalıştırın. Sizin ve ekibinizin 7/24 tüm kaynakların durum etkin olarak izleyemez. Örneğin, 2'de bir sorun oluşursa ne olur?

Ağ İzleyicisi'ni kullanarak, uyarı ve işlevlerden Azure ekosistemi içinde veri ve ağınızdaki sorunları çözmek için araçları ile önceden yanıt verebilir.

![Senaryo][scenario]

## <a name="prerequisites"></a>Ön koşullar

* En son sürümünü [Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Ağ İzleyicisi var olan bir örneği. Zaten yoksa, [Ağ İzleyicisi örneği oluşturun](network-watcher-create.md).
* Ağ İzleyicisi ile aynı bölgede var olan bir sanal makine [Windows uzantısı](../virtual-machines/windows/extensions-nwa.md) veya [Linux sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Senaryo

Bu örnekte, VM normalden daha çok TCP kesimini gönderme ve uyarı almak istediğiniz. TCP kesimini burada bir örnek olarak kullanılır, ancak hiçbir uyarı koşulu kullanabilirsiniz.

Uyarı aldığınızda iletişimi neden artırmıştır anlamak için paket düzeyinde verileri almak istediğiniz. Daha sonra sanal makineyi normal iletişimi döndürmek için adımları alabilir.

Bu senaryo, Ağ İzleyicisi'ni ve geçerli bir sanal makine ile bir kaynak grubu mevcut bir örneği olduğunu varsayar.

Liste aşağıda gerçekleşir iş akışına genel bir bakış verilmiştir:

1. Bir uyarı, VM tetiklenir.
1. Uyarı Azure işlevinizi aracılığıyla bir Web kancası çağırır.
1. Azure işlevinizi uyarı işler ve bir Ağ İzleyicisi paket yakalama oturumu başlatır.
1. Paket yakalama VM üzerinde çalışır ve trafik toplar.
1. Paket yakalama dosyasını gözden geçirin ve tanılama için bir depolama hesabı için yüklenir.

Bu işlemi otomatikleştirmek için oluşturun ve bir uyarı olayı meydana geldiğinde tetiklemek için bizim VM bağlanın. Biz de Ağ İzleyicisi çağırmak için bir işlev oluşturun.

Bu senaryo şunları yapar:

* Paket yakalama başlatır Azure bir işlev oluşturur.
* Bir sanal makinede bir uyarı kuralı oluşturur ve Azure işlevi çağırmak için uyarı kuralı yapılandırır.

## <a name="create-an-azure-function"></a>Bir Azure işlevi oluşturma

İlk adımı, uyarıyı işlemek ve paket yakalama oluşturmak için bir Azure işlevi oluşturmaktır.

1. İçinde [Azure portal](https://portal.azure.com)seçin **yeni** > **işlem** > **işlev uygulaması**.

    ![Bir işlev uygulaması oluşturma][1-1]

2. Üzerinde **işlev uygulaması** dikey penceresinde, aşağıdaki değerleri girin ve ardından **Tamam** uygulama oluşturmak için:

    |**Ayar** | **Değer** | **Ayrıntılar** |
    |---|---|---|
    |**Uygulama adı**|PacketCaptureExample|İşlev uygulaması adı.|
    |**Abonelik**|[Aboneliğinizi] Abonelik için işlev uygulaması oluşturmak için.||
    |**Kaynak Grubu**|PacketCaptureRG|İşlev uygulaması içeren kaynak grubu.|
    |**Barındırma planı**|Tüketim planı| Türü, işlev uygulaması kullanır planlayın. Seçenekler şunlardır tüketimi veya Azure uygulama hizmeti planı. |
    |**Konum**|Orta ABD| Bölge, işlev uygulaması oluşturmak kullanın.|
    |**Depolama hesabı**|{otomatik olarak oluşturulur}| Azure işlevleri için genel amaçlı depolama alanı ihtiyaçlarınızı depolama hesabı.|

3. Üzerinde **PacketCaptureExample işlev uygulamalarının** dikey penceresinde, select **işlevleri** > **özel işlevi**  >  **+**.

4. Seçin **HttpTrigger Powershell**ve ardından kalan bilgileri girin. Son olarak, işlev oluşturmak için seçin **oluşturma**.

    |**Ayar** | **Değer** | **Ayrıntılar** |
    |---|---|---|
    |**Senaryo**|Deneysel|Tür senaryosu|
    |**İşlevinizi adlandırın**|AlertPacketCapturePowerShell|İşlevin adı|
    |**Yetkilendirme düzeyi**|İşlevi|Yetki düzeyini işlevi|

![İşlevleri örneği][functions1]

> [!NOTE]
> PowerShell şablon Deneysel ve tam desteğine sahip değil.

Özelleştirmeleri bu örnek için gereklidir ve aşağıdaki adımlarda açıklanmaktadır.

### <a name="add-modules"></a>Modüller ekleme

Ağ İzleyicisi PowerShell cmdlet'lerini kullanmak için işlev uygulaması son PowerShell modülünü yükleyin.

1. Yerel makinenizde yüklü en son Azure PowerShell modülleri ile aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    Bu örnek, Azure PowerShell modüllerinizi yerel yolunu sağlar. Bu klasörler, bir sonraki adımda kullanılır. Bu senaryoda kullanılan modülleri şunlardır:

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

    ![PowerShell klasörleri][functions5]

1. Seçin **işlev uygulaması ayarları** > **App Service Düzenleyici'ye gidin**.

    ![İşlev uygulama ayarları][functions2]

1. Sağ **AlertPacketCapturePowershell** klasörünü ve ardından adlı bir klasör oluşturun **azuremodules**. 

4. Gereksinim duyduğunuz her modül için bir alt klasör oluşturun.

    ![Klasör ve alt klasörleri][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Sağ **AzureRM.Network** alt ve ardından **dosya yükleme**. 

6. Azure modüllerinizi gidin. Yerel **AzureRM.Network** klasörü, klasördeki tüm dosyaları seçin. Ardından **Tamam**. 

7. İçin bu adımları yineleyin **AzureRM.Profile** ve **AzureRM.Resources**.

    ![Dosyaları karşıya yükleme][functions6]

1. Tamamlandı sonra her klasör, yerel makinenize PowerShell modülü dosyalarından olmalıdır.

    ![PowerShell dosyaları][functions7]

### <a name="authentication"></a>Kimlik Doğrulaması

PowerShell cmdlet'lerini kullanmak için kimlik doğrulaması gerekir. İşlev uygulamasında kimlik doğrulamasını yapılandırın. Kimlik doğrulamasını yapılandırmak için ortam değişkenleri yapılandırmanız ve şifreli bir anahtar dosyası işlevi uygulamaya karşıya gerekir.

> [!NOTE]
> Bu senaryo, nasıl Azure işlevleri ile kimlik doğrulaması uygulamak yalnızca bir örnek sağlar. Bunu yapmak için başka yolları vardır.

#### <a name="encrypted-credentials"></a>Şifrelenmiş kimlik bilgileri

Aşağıdaki PowerShell betiğini adlı bir anahtar dosyası oluşturur **PassEncryptKey.key**. Sağlanan parola şifreli sürümünü de sağlar. Bu parola kimlik doğrulaması için kullanılan Azure Active Directory uygulaması için tanımlanan aynı paroladır.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

Uygulama hizmeti Düzenleyicisi'nde işlevi uygulamanın adlı bir klasör oluşturun **anahtarları** altında **AlertPacketCapturePowerShell**. Ardından karşıya **PassEncryptKey.key** önceki PowerShell örneğinde oluşturulan dosyası.

![İşlevler anahtarı][functions8]

### <a name="retrieve-values-for-environment-variables"></a>Ortam değişkenleri için değerleri alma

Son kimlik doğrulaması için değerlerine erişmek için gerekli olan ortam değişkenleri ayarlamak için gereksinimdir. Aşağıdaki liste, oluşturulan ortam değişkenleri gösterir:

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

İstemci kimliği Azure Active Directory'de bir uygulamanın uygulama Kimliğini gösterir.

1. Kullanmak için bir uygulama zaten yoksa, bir uygulama oluşturmak için aşağıdaki örneği çalıştırın.

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > Uygulama oluştururken kullandığınız parolayı anahtar dosyası kaydedilirken daha önce oluşturduğunuz aynı parola olmalıdır.

1. Azure portalında seçin **abonelikleri**. Abonelik kullanın ve ardından seçmek için Seç **erişim denetimi (IAM)**.

    ![İşlevler IAM][functions9]

1. Kullanın ve ardından için hesabı seçin **özellikleri**. Uygulama Kimliği kopyalayın.

    ![İşlevleri uygulama kimliği][functions10]

#### <a name="azuretenant"></a>AzureTenant

Aşağıdaki PowerShell örnek çalıştırarak Kiracı kimliği alın:

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

AzureCredPassword ortam değişkeni aşağıdaki PowerShell örneğini çalıştıran alma değer değeridir. Bu örnek bir önceki gösterilen aynıdır **şifrelenmiş kimlik bilgileri** bölümü. Gerekli olan çıktısını değerdir `$Encryptedpassword` değişkeni.  PowerShell Betiği kullanılarak şifrelenmiş hizmet asıl parolası budur.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-the-environment-variables"></a>Ortam değişkenleri depolama

1. İşlev uygulamasına gidin. Ardından **işlev uygulaması ayarları** > **uygulaması ayarlarını yapılandır**.

    ![Uygulama ayarlarını yapılandırma][functions11]

1. Ortam değişkenlerini ve değerleri için uygulama ayarları ekleyin ve ardından **kaydetmek**.

    ![Uygulama ayarları][functions12]

### <a name="add-powershell-to-the-function"></a>PowerShell işlevine ekleyin

Bu Ağ İzleyicisi ' çağrılarını Azure işlevi içinde olmak üzere sunulmuştur. Bu işlev uygulaması gereksinimlerine bağlı olarak değişebilir. Ancak, genel akış kod aşağıdaki gibidir:

1. İşlem giriş parametreleri.
2. Sınırlarını doğrulayın ve ad çakışmalarını çözmek için sorgu varolan paket yakalar.
3. Paket yakalama uygun parametrelerle oluşturun.
4. Yoklama paket düzenli aralıklarla tamamlanana kadar yakalayın.
5. Paket yakalama oturumu tamamlandıktan kullanıcıya bildir.

Aşağıdaki örnek, işlevde kullanılabilmesi için PowerShell koddur. İçin değiştirilmesi gereken değerler **Subscriptionıd**, **resourceGroupName**, ve **storageAccountName**.

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID to save captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a>İşlev URL'sini alın 
1. İşlevinizi oluşturduktan sonra işlev ile ilişkili URL çağırmak için uyarıyı yapılandırın. Bu değer almak için işlevi uygulamanızdan işlevi URL'sini kopyalayın.

    ![İşlev URL bulma][functions13]

2. İşlev uygulamanız için işlevi URL'sini kopyalayın.

    ![İşlev URL kopyalanması][2]

Web kancası POST isteği yükte özel özellikler gerektiriyorsa, başvurmak [bir Web kancası Azure ölçüm uyarıyı yapılandırmak](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="configure-an-alert-on-a-vm"></a>Bir VM üzerinde bir uyarı yapılandırmak

Uyarıları, belirli bir ölçüyü kendisine atanmış bir eşik kestiği kişilere bildirmek için yapılandırılabilir. Bu örnekte, gönderilen TCP kesimlerinde uyarısıdır, ancak uyarı için birçok diğer ölçümleri tetiklenebilir. Bu örnekte, bir uyarı işlevi çağırmak için bir Web kancası çağırmak için yapılandırılır.

### <a name="create-the-alert-rule"></a>Uyarı kuralı oluştur

Varolan bir sanal makineye gidin ve ardından bir uyarı kuralı ekleyin. Uyarıları yapılandırma hakkında daha ayrıntılı belgeler bulunabilir [oluşturma uyarıları Azure İzleyicisi'nde için Azure services - Azure portalında](../monitoring-and-diagnostics/insights-alerts-portal.md). Aşağıdaki değerleri girin **uyarı kuralı** dikey ve ardından **Tamam**.

  |**Ayar** | **Değer** | **Ayrıntılar** |
  |---|---|---|
  |**Ad**|TCP_Segments_Sent_Exceeded|Uyarı kuralı adı.|
  |**Açıklama**|TCP kesimini aşıldı eşik gönderilen|Uyarı kuralı açıklaması.||
  |**Ölçüm**|Gönderilen TCP kesimleri| Uyarı tetiklemek için kullanılacak ölçüm. |
  |**Koşul**|Şu değerden fazla:| Ölçüm hesaplanırken kullanılacak koşulu.|
  |**Eşik**|100| Uyarıyı tetikleyen ölçüm değeri. Bu değer, ortamınız için geçerli bir değere ayarlanmalıdır.|
  |**Süresi**|Son beş dakika boyunca| Ölçüm eşiğine arayın olduğu süreyi belirler.|
  |**Web kancası**|[işlev uygulaması URL'SİNDEN Web kancası]| Önceki adımlarda oluşturulan işlevi uygulamasından Web kancası URL'si.|

> [!NOTE]
> TCP kesimleri ölçümü varsayılan olarak etkin değildir. Ek ölçümler ziyaret ederek etkinleştirme hakkında daha fazla bilgi edinin [izleme ve tanılama](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-the-results"></a>Sonuçları gözden geçirin

Uyarı Tetikleyicileri ölçütlerine sonra bir paket yakalama oluşturulur. Ağ İzleyicisi gidin ve ardından **paket yakalama**. Bu sayfada, paket yakalama indirmek için paket yakalama dosyası bağlantısı seçebilirsiniz.

![Görünüm paket yakalama][functions14]

Yakalama dosyası yerel olarak depolanıyorsa, sanal makineye oturum açarak alabilirsiniz.

Azure depolama hesaplarından dosyalarını yükleme hakkında yönergeler için bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanabileceğiniz başka bir araçtır [Depolama Gezgini](http://storageexplorer.com/).

Yakalama yüklendikten sonra okuyabilir herhangi bir aracı kullanarak görüntüleyebilirsiniz bir **.cap** dosya. Aşağıdaki iki bu araçların bağlantıları verilmiştir:

- [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Sonraki adımlar

Paket yakalama ziyaret ederek görüntülemeyi öğrenin [paket yakalama çözümlemesini Wireshark ile](network-watcher-deep-packet-inspection.md).


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
