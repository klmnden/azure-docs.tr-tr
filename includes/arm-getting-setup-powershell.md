## <a name="setting-up-powershell-for-resource-manager-templates"></a>Resource Manager şablonları için PowerShell ayarlayan
Resource Manager ile Azure PowerShell kullanabilmeniz için önce sağ Windows PowerShell ve Azure PowerShell sürümleri gerekir.

### <a name="verify-powershell-versions"></a>PowerShell sürümlerini doğrulama
Windows PowerShell sürüm 3.0 veya 4.0 doğrulayın. Windows PowerShell sürümü bulmak için Windows PowerShell komut isteminde bu komutu yazın.

    $PSVersionTable

Aşağıdaki türde bilgiler alırsınız:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Doğrulayın değerini **PSVersion** 3.0 veya 4.0. Aksi takdirde bkz [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) veya [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Azure hesabınızı ve aboneliğinizi ayarlama
Bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

Bir Azure PowerShell komut istemi açın ve bu komutla Azure'da oturum açın.

    Connect-AzureRmAccount

Birden çok Azure aboneliğiniz varsa, bu komutla, Azure aboneliklerinize listeleyebilirsiniz.

    Get-AzureRmSubscription

Aşağıdaki türde bilgiler alırsınız:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Azure PowerShell komut isteminde şu komutları çalıştırarak geçerli Azure aboneliği ayarlayabilirsiniz. Dahil olmak üzere tırnak işaretleri içindeki her şeyi değiştirin < ve > karakterleri doğru ada sahip.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Azure abonelikleri ve hesapları hakkında daha fazla bilgi için bkz: [nasıl yapılır: aboneliğinize bağlanma](/powershell/azureps-cmdlets-docs#step-3-connect).

