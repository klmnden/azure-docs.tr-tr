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


<!--HONumber=Feb17_HO2-->


