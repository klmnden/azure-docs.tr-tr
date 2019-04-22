---
title: Uyarılar ve Azure işlevleri ile öngörülü ağ izleme yapmak için paket yakalamayı kullanma | Microsoft Docs
description: Bu makalede bir uyarı tetiklendi paket yakalama ile Azure Ağ İzleyicisi oluşturma
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: c7bfd36bb4e36b10487edbbaa40421f067c9ed3e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59048767"
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Uyarılar ve Azure işlevleri ile öngörülü ağ izleme için paket yakalamayı kullanma

Ağ İzleyicisi paket yakalama, sanal makineler ve giden trafiği izlemek için yakalama oturumu oluşturur. Yakalama dosyası, izlemek istediğiniz trafiği izlemek için tanımlanan bir filtre olabilir. Bu veriler daha sonra bir depolama blobu veya yerel olarak Konuk makineye depolanır.

Bu özellik, Azure işlevleri gibi diğer Otomasyon senaryolardan uzaktan başlatılabilir. Paket yakalaması tanımlı ağ anomali göre proaktif yakalamaları çalıştırma özelliği sağlar. Diğer kullanımlar istatistikleri ağ izinsiz girişi, istemci-sunucu iletişimi hata ayıklama ve daha fazlasını hakkında bilgi alma, toplama içerir.

Azure'da dağıtılan kaynakları 7/24 çalıştırın. Siz ve ekibiniz, 7/24 tüm kaynakların durum etkin olarak izleyemez. Örneğin, 02: 00 bir sorun oluşması durumunda ne olur?

Ağ İzleyicisi'ni kullanarak, uyarı ve işlevlerden içinde Azure ekosistemi tarafından ağınızdaki sorunları çözmek için araçları ve verileri ile proaktif olarak yanıt verebilir.

![Senaryo][scenario]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

* En son sürümünü [Azure PowerShell](/powershell/azure/install-Az-ps).
* Ağ İzleyicisi'nin var olan bir örneği. Zaten yoksa, [Ağ İzleyicisi bir örneğini oluşturmak](network-watcher-create.md).
* Ağ İzleyicisi ile aynı bölgede mevcut bir sanal makine [Windows uzantısı](../virtual-machines/windows/extensions-nwa.md) veya [Linux sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Senaryo

Bu örnekte, sanal makinenizin normalden daha çok TCP kesimini gönderiyor ve uyarı almak istediğiniz. TCP kesimini burada bir örnek olarak kullanılır, ancak herhangi bir uyarı koşulu kullanabilirsiniz.

Uyarı aldığınızda, iletişimi neden arttı anlamak için paket düzeyinde veri almak istersiniz. Ardından sanal makineyi normal iletişimi döndürmek üzere adım atabilirsiniz.

Bu senaryo, Ağ İzleyicisi ile geçerli bir sanal makine ile bir kaynak grubu mevcut bir örneğini sahibi olduğunuzu varsayar.

Aşağıdaki listede yer alan bir iş akışı genel bir bakış verilmiştir:

1. Sanal makinenizde bir uyarı tetiklenir.
1. Uyarı aracılığıyla bir Web kancası, Azure işlevi çağırır.
1. Azure işlevinizi uyarı işler ve bir Ağ İzleyicisi paket yakalama oturumu başlatır.
1. Paket yakalaması VM üzerinde çalışan ve trafiği toplar.
1. Paket yakalama dosyasını gözden geçirin ve İleri tanılama için bir depolama hesabına yüklenir.

Bu işlemi otomatikleştirmek için oluşturun ve bir uyarı olayı ortaya çıktığında tetiklemek için VM'de bağlanın. Ağ İzleyicisi çağırmak için fonksiyon de oluştururuz.

Bu senaryo, şunları yapar:

* Paket yakalaması başlatan bir Azure işlevi oluşturur.
* Bir sanal makinede bir uyarı kuralı oluşturur ve Azure işlevini çağırmak için uyarı kuralı yapılandırır.

## <a name="create-an-azure-function"></a>Azure işlevi oluşturma

İlk adım, uyarı işleme ve bir paket yakalama oluşturmak için bir Azure işlevi oluşturmaktır.

1. İçinde [Azure portalında](https://portal.azure.com)seçin **kaynak Oluştur** > **işlem** > **işlev uygulaması**.

    ![Bir işlev uygulaması oluşturma][1-1]

2. Üzerinde **işlev uygulaması** dikey penceresinde aşağıdaki değerleri girin ve ardından **Tamam** uygulamayı oluşturmak için:

    |**Ayar** | **Değer** | **Ayrıntılar** |
    |---|---|---|
    |**Uygulama adı**|PacketCaptureExample|İşlev uygulamasının adı.|
    |**Abonelik**|[Aboneliğiniz] İşlev uygulaması oluşturmak istediğiniz aboneliği.||
    |**Kaynak Grubu**|PacketCaptureRG|İşlev uygulaması içerecek olan kaynak grubu.|
    |**Barındırma Planı**|Tüketim Planı| Türü, işlev uygulamasını kullanan planlayın. Seçenekler: Tüketim veya Azure App Service planı. |
    |**Konum**|Orta ABD| İşlev uygulaması oluşturmak üzere OU'nun bölge.|
    |**Depolama Hesabı**|{otomatik olarak oluşturulan}| Azure işlevleri için genel amaçlı depolama gerekli depolama hesabı.|

3. Üzerinde **PacketCaptureExample işlev uygulamaları** dikey penceresinde **işlevleri** > **özel işlev**  >  **+**.

4. Seçin **HttpTrigger-Powershell**ve ardından kalan bilgileri girin. Son olarak, işlev oluşturmak için Seç **Oluştur**.

    |**Ayar** | **Değer** | **Ayrıntılar** |
    |---|---|---|
    |**Senaryo**|Deneysel|Senaryo türü|
    |**İşlevinizi adlandırın**|AlertPacketCapturePowerShell|İşlevin adı|
    |**Yetkilendirme düzeyi**|İşlev|Yetkilendirme düzeyi işlevi|

![Örnek işlevleri][functions1]

> [!NOTE]
> PowerShell şablon Deneysel ve tam desteğine sahip değil.

Özelleştirmeleri, bu örnek için gerekli olan ve aşağıdaki adımlarda açıklanmıştır.

### <a name="add-modules"></a>Modül ekle

Ağ İzleyicisi PowerShell cmdlet'lerini kullanmak için işlev uygulaması için en son PowerShell modülünü yükleyin.

1. Yerel makinenizde yüklü en son Azure PowerShell modülleri, aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    (Get-Module Az.Network).Path
    ```

    Bu örnek, Azure PowerShell modüllerini yerel yolunu sağlar. Bu klasör, bir sonraki adımda kullanılır. Bu senaryoda kullanılan modülleri şunlardır:

   * Az.Network

   * Az.Accounts

   * Az.Resources

     ![PowerShell klasörleri][functions5]

1. Seçin **işlev uygulaması ayarları** > **App Service Düzenleyicisi'ne gitmek**.

    ![İşlev uygulaması ayarları][functions2]

1. Sağ **AlertPacketCapturePowershell** klasörünü ve ardından adlı bir klasör oluşturun **azuremodules**. 

4. İhtiyacınız olan her bir modül için bir alt klasör oluşturun.

    ![Klasör ve alt klasörleri][functions3]

    * Az.Network

    * Az.Accounts

    * Az.Resources

1. Sağ **Az.Network** alt klasöre tıklayın ve ardından **dosyaları karşıya yükle**. 

6. Azure modüllerinizi gidin. Yerel **Az.Network** klasörü, klasördeki tüm dosyaları seçin. Sonra **Tamam**’ı seçin. 

7. Bu adımı yineleyin **Az.Accounts** ve **Az.Resources**.

    ![Dosyaları karşıya yükleme][functions6]

1. Sizin tamamladıktan sonra her klasörü yerel makinenizden PowerShell Modülü dosyaları olmalıdır.

    ![PowerShell dosyaları][functions7]

### <a name="authentication"></a>Authentication

PowerShell cmdlet'lerini kullanmak için kimlik doğrulaması gerekir. İşlev uygulamasında kimlik doğrulamasını yapılandırın. Kimlik doğrulamasını yapılandırmak için ortam değişkenlerini yapılandırma ve şifreli bir anahtar dosyası işlev uygulamasına yükleyin.

> [!NOTE]
> Bu senaryo, Azure işlevleri ile kimlik doğrulaması uygulamak yalnızca bir örneğini sağlar. Bunu yapmanın farklı yöntemleri vardır.

#### <a name="encrypted-credentials"></a>Şifrelenmiş kimlik bilgileri

Aşağıdaki PowerShell betiğini adlı bir anahtar dosyası oluşturur **PassEncryptKey.key**. Sağlanan parola şifrelenmiş bir sürümünü de sağlar. Bu parola kimlik doğrulaması için kullanılan Azure Active Directory uygulama için tanımlanmış aynı paroladır.

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

İşlev uygulamasının App Service Düzenleyicisi adlı bir klasör oluşturun **anahtarları** altında **AlertPacketCapturePowerShell**. Ardından karşıya **PassEncryptKey.key** önceki PowerShell örnekte oluşturulan dosya.

![İşlevleri anahtarı][functions8]

### <a name="retrieve-values-for-environment-variables"></a>Ortam değişkenleri için değerleri alma

Son gereksinim kimlik doğrulaması için değerlere erişmek gerekli olan ortam değişkenlerini ayarlamaktır. Aşağıdaki liste, oluşturulan ortam değişkenlerini gösterir:

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

Azure Active Directory'de bir uygulamanın uygulama kimliği istemci kimliğidir.

1. Bir uygulamayı kullanmak için zaten sahip değilseniz, bir uygulama oluşturmak için aşağıdaki örneği çalıştırın.

    ```powershell
    $app = New-AzADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > Uygulamayı oluştururken kullandığınız parolayı daha önce oluşturduğunuz anahtar dosyası kaydedilirken aynı parola olmalıdır.

1. Azure portalında **abonelikleri**. Aboneliğini kullanın ve ardından **erişim denetimi (IAM)**.

    ![IAM işlevleri][functions9]

1. Hesabı kullanın ve ardından seçin **özellikleri**. Uygulama kimliğini kopyalama

    ![İşlevler uygulama kimliği][functions10]

#### <a name="azuretenant"></a>AzureTenant

Aşağıdaki PowerShell örneğini çalıştırarak Kiracı Kimliğini alın:

```powershell
(Get-AzSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

AzureCredPassword ortam değişkeninin değerini, aşağıdaki PowerShell örneğini çalıştırılarak elde değerdir. Bu örnek bir önceki gösterilenle aynıdır **şifrelenmiş kimlik bilgileri** bölümü. Gerekli olan çıktısı değerdir `$Encryptedpassword` değişkeni.  PowerShell Betiği kullanılarak şifrelenmiş hizmet sorumlusu parolası budur.

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

### <a name="store-the-environment-variables"></a>Ortam değişkenlerini Store

1. İşlev uygulamasına gidin. Ardından **işlev uygulaması ayarları** > **uygulama ayarlarını yapılandırma**.

    ![Uygulama ayarlarını yapılandırma][functions11]

1. Ortam değişkenlerini ve değerleri uygulama ayarları ekleyin ve ardından **Kaydet**.

    ![Uygulama ayarları][functions12]

### <a name="add-powershell-to-the-function"></a>PowerShell işleve ekleyin

Bu, artık Azure işlevi içinde Ağ İzleyicisi ' çağrılar yapmak zamanı geldi. Bu işlev uygulamasını gereksinimlerine bağlı olarak değişebilir. Ancak, kod genel akışı şu şekildedir:

1. İşlem giriş parametreleri.
2. Sınırları doğrulamak ve ad çakışmalarını çözmek için sorgu mevcut paket yakalar.
3. Bir paket yakalama, uygun parametrelerle oluşturun.
4. Yoklama paket yakalama düzenli aralıklarla tamamlanana kadar.
5. Paket yakalama oturumu tam olduğunu kullanıcıya bildirin.

Aşağıdaki örnek işlev kullanılabilecek PowerShell kodudur. Değerler için değiştirilmesi gereken **Subscriptionıd**, **resourceGroupName**, ve **storageAccountName**.

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\Az.Accounts\Az.Accounts.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\Az.Network\Az.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\Az.Resources\Az.Resources.psd1" -Global

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
            Connect-AzAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $nw = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a>İşlev URL'sini Al 
1. İşlevinizi oluşturduktan sonra işlev ile ilişkili URL çağırmak için uyarıyı yapılandırın. Bu değeri almak için işlev uygulamanızı işlev URL'sini kopyalayın.

    ![İşlev URL'sini bulma][functions13]

2. İşlev uygulamanız için işlev URL'sini kopyalayın.

    ![İşlev URL'sini kopyalama][2]

Web kancası POST isteğinin yükünde özel özellikler gerektiriyorsa, başvurmak [Azure bir ölçüm uyarısında Web kancası yapılandırma](../azure-monitor/platform/alerts-webhooks.md).

## <a name="configure-an-alert-on-a-vm"></a>Bir VM'de bir uyarı yapılandırma

Uyarılar, belirli bir ölçüyü kendisine atanmış bir eşiği geçtiğinde kişilere bildirimde şekilde yapılandırılabilir. Bu örnekte, uyarı, gönderilen TCP kesimini belirtir, ancak diğer için çok sayıda ölçüm uyarı tetiklenebilir. Bu örnekte, bir uyarı işlevi çağrısı için bir Web kancası çağırma yapılandırılır.

### <a name="create-the-alert-rule"></a>Uyarı kuralı oluşturma

Varolan bir sanal makineye gidin ve ardından bir uyarı kuralı ekleyin. Uyarıları yapılandırma hakkında daha ayrıntılı belgeler bulunabilir [oluşturma uyarılar Azure İzleyici'de Azure Hizmetleri için - Azure portalı](../monitoring-and-diagnostics/insights-alerts-portal.md). Aşağıdaki değerleri girin **uyarı kuralı** dikey penceresine tıklayın ve ardından **Tamam**.

  |**Ayar** | **Değer** | **Ayrıntılar** |
  |---|---|---|
  |**Ad**|TCP_Segments_Sent_Exceeded|Uyarı kuralı adı.|
  |**Açıklama**|TCP kesimini eşiğini gönderilen|Uyarı kuralı açıklaması.|
  |**Ölçüm**|TCP kesimini gönderilen| Uyarı tetiklemek için kullanılacak ölçü. |
  |**Koşul**|Büyüktür| Ölçüm değerlendirilirken kullanılacak koşul.|
  |**Eşik**|100| Uyarıyı tetikleyen ölçüm değeri. Bu değer, ortamınız için geçerli bir değere ayarlanmalıdır.|
  |**Dönem**|Son beş dakika boyunca| Ölçüm eşiğine aramak süreniz belirler.|
  |**Web kancası**|[işlev uygulamasından Web kancası URL'si]| Web kancası URL'si önceki adımlarda oluşturulan işlev uygulamasından.|

> [!NOTE]
> Varsayılan olarak TCP segmentleri ölçüm etkin değil. Ek ölçümler ederek etkinleştirme hakkında daha fazla bilgi [izleme ve tanılamayı etkinleştirme](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-the-results"></a>Sonuçları gözden geçirin

Uyarı tetiklenmeden ölçütlerini sonra bir paket yakalama oluşturulur. Ağ İzleyicisi gidin ve ardından **paket yakalaması**. Bu sayfada, paket yakalama dosyası bağlantısı'paket yakalaması indirmek için seçebilirsiniz.

![Paket yakalaması görüntüle][functions14]

Yakalama dosyasını yerel olarak depolanıyorsa, sanal makineye oturum açarak alabilir.

Azure depolama hesaplarından dosyaları indirme hakkındaki yönergeler için bkz: [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanabileceğiniz başka bir araçtır [Depolama Gezgini](https://storageexplorer.com/).

Yakalama İndirildikten sonra okuyabilirsiniz herhangi bir aracı kullanarak görüntüleyebilirsiniz bir **.cap** dosya. Bu araçların iki bağlantılar aşağıda verilmiştir:

- [Microsoft ileti Çözümleyicisi](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Sonraki adımlar

Paket yakalamaları almanızı sağlamanın ederek görüntülemeyi öğrenin [paket yakalama Wireshark analiziyle](network-watcher-deep-packet-inspection.md).


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
