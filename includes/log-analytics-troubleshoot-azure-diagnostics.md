### <a name="troubleshoot-azure-diagnostics"></a>Azure Tanılama Sorunlarını Giderme

Aşağıdaki hata iletisini alırsanız, Microsoft.insights kaynak sağlayıcısı kayıtlı değildir:

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Kaynak sağlayıcısını kaydetmek için Azure portalında aşağıdaki adımları uygulayın:

1.  Sol gezinti bölmesinde *Abonelikler*’e tıklayın
2.  Hata iletisinde belirtilen aboneliği seçin
3.  *Kaynak Sağlayıcıları*’ne tıklayın
4.  *Microsoft.insights* sağlayıcısını bulun
5.  *Kaydet* bağlantısına tıklayın

![Microsoft.insights kaynak sağlayıcısını kaydetme](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

*Microsoft.insights* kaynak sağlayıcısı kaydedildikten sonra tanılama yapılandırmasını yeniden deneyin.


PowerShell'de aşağıdaki hata iletisini alırsanız, PowerShell sürümünüz güncelleştirmeniz gerekir:

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

PowerShell sürümünüz güncelleştirme Kasım 2016 (v2.3.0) ya da daha sonra ' ndaki yönergeleri kullanarak serbest [Azure PowerShell cmdlet'leri kullanmaya başlama](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) makalesi.
