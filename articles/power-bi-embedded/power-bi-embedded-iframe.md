<properties
   pageTitle="Microsoft Power BI Embedded Önizleme - IFrame ile bir Power BI raporu ekleme"
   description="Microsoft Power BI Embedded Önizleme - Bir raporu uygulamanızla tümleştirmek, Power BI Embedded uygulama belirteci kimliğini doğrulamak, raporlar almak için temel kod"
   services="power-bi-embedded"
   documentationCenter=""
   authors="dvana"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="05/16/2016"
   ms.author="derrickv"/>

# IFrame ile bir Power BI raporu ekleme
Bu makale, bir raporu uygulamanızla tümleştirmek veya eklemek için **Power BI Embedded** REST API’si, uygulama belirteçleri, IFrame ve bazı JavaScript’leri kullanmak üzere temel kodu gösterir.

[Microsoft Power BI Embedded Önizleme kullanmaya başlama](power-bi-embedded-get-started.md) bölümünde, rapor içeriğiniz için bir ya da daha çok **Çalışma Alanı** tutmak üzere **Çalışma Alanı Koleksiyonu**’nu yapılandırmayı öğrenirsiniz  Ardından, [Microsoft Power BI Embedded örneği kullanmaya başlama](power-bi-embedded-get-started-sample.md) bölümünde bir raporu **Çalışma Alanı**’na aktarırsınız.

Bu makale size uygulamanıza bir rapor ekleme adımlarını gösterir. Bu makaleyi izlemek için, GitHub’da [IFrame ile bir rapor tümleştirme](https://github.com/Azure-Samples/power-bi-embedded-iframe) örneğini indirmelisiniz. Bu örnek, bir raporu tümleştirmeniz için gereken temel C# ve JavaScript kodunu göstermeyi amaçlayan basit bir ASP.NET web formu uygulamasıdır. Model-View-Controller (MVC) tasarım düzenini kullanarak bir rapor tümleştiren daha gelişmiş bir örnek için, GitHub’da [Pano web uygulaması](http://go.microsoft.com/fwlink/?LinkId=761493) örneğine bakın.

Şimdi uygulamanıza bir **Power BI Embedded** raporu nasıl tümleştireceğinizi açıklayarak başlayalım.

Bir raporu tümleştirme adımları şöyledir:

- 1. Adım: [Bir çalışma alanında rapor alma](#GetReport). Bu adımda, [Raporlar Alma](https://msdn.microsoft.com/library/mt711510.aspx) REST işlemini çağırmak için erişim belirteci almak üzere bir uygulama belirteci akışı kullanırsınız. **Raporları Alma** listesinden raporu aldıktan sonra, raporu **IFrame** öğesiyle bir uygulamaya eklersiniz.
- 2. Adım: [Uygulamaya bir rapor ekleme](#EmbedReport). Bu adımda, web uygulamasına bir rapor tümleştirmek, ya da eklemek üzere bir rapor için ekleme belirteci, bazı JavaScript’ler ve IFrame kullanacaksınız.

Rapor tümleştirmeyi görmek amacıyla örneği çalıştırmak istiyorsanız, GitHub’da [IFrame ile bir rapor tümleştirme](https://github.com/Azure-Samples/power-bi-embedded-iframe) örneğini indirin ve üç Web.Config ayarını yapılandırın:

- **AccessKey**: **AccessKey**, raporlar almak ve rapor eklemek için kullanılan bir JSON Web Belirteci oluşturmak için kullanılır. **AccessKey** alma hakkında bilgi için bkz. [Microsoft Power BI Embedded Önizleme kullanmaya başlama](power-bi-embedded-get-started.md).
- **WorkspaceName**: **WorkspaceName** alma hakkında bilgi için bkz. [Microsoft Power BI Embedded Önizleme kullanmaya başlama](power-bi-embedded-get-started.md).
- **WorkspaceId**: **WorkspaceId** alma hakkında bilgi için bkz. [Microsoft Power BI Embedded Önizleme kullanmaya başlama](power-bi-embedded-get-started.md).

Sonraki bölümlerde bir raporu tümleştirmek için gereken kod gösterilir.

<a name="GetReport"/>
## Bir çalışma alanında rapor alma

Bir uygulamaya rapor tümleştirmek için bir rapor **Kimliği** ve **embedUrl**’si gerekir. Bir rapor **Kimliği** ve **embedUrl**’si almak için, [Raporları Al](https://msdn.microsoft.com/library/mt711510.aspx) REST işlemini çağırın ve JSON listesinden raporu seçin. [Uygulamaya bir rapor ekleme](#EmbedReport)de, raporu uygulamanıza eklemek için rapor **Kimliği** ve **embedUrl**’si kullanırsınız.

### Raporlar JSON yanıtını alma
```
{
  "@odata.context":"https://api.powerbi.com/beta/collections/{WorkspaceName}/workspaces/{WorkspaceId}/$metadata#reports","value":[
    {
      "id":"804d3664-…-e71882055dba","name":"Import report sample","webUrl":"https://embedded.powerbi.com/reports/804d3664-...-e71882055dba","embedUrl":"https://embedded.powerbi.com/appTokenReportEmbed?reportId=804d3664-...-e71882055dba"
    },{
      "id":"1d7cff58-…-380610e263a9","name":"Sample Report 2","webUrl":"https://embedded.powerbi.com/reports/1d7cff58-...-380610e263a9","embedUrl":"https://embedded.powerbi.com/appTokenReportEmbed?reportId=1d7cff58-...-380610e263a9"
    }
  ]
}

```

[Raporları Al](https://msdn.microsoft.com/library/mt711510.aspx) REST işlemini çağırmak için, bir uygulama belirteci kullanın. Uygulama belirteci akışı hakkında daha fazla bilgi için, bkz. [Power BI Embedded’de uygulama belirteci akışı hakkında](power-bi-embedded-app-token-flow.md). Aşağıdaki kod, raporlar JSON listesini almayı açıklar. Bir rapor eklemek için bkz. [Uygulamaya bir rapor ekleme](#EmbedReport).

```
protected void getReportsButton_Click(object sender, EventArgs e)
{
    //Get an app token to generate a JSON Web Token (JWT). An app token flow is a claims-based design pattern.
    //To learn how you can code an app token flow to generate a JWT, see the PowerBIToken class.
    var appToken = PowerBIToken.CreateDevToken(workspaceName, workspaceId);

    //After you get a PowerBIToken which has Claims including your WorkspaceName and WorkspaceID,
    //you generate JSON Web Token (JWT) . The Generate() method uses classes from System.IdentityModel.Tokens: SigningCredentials,
    //JwtSecurityToken, and JwtSecurityTokenHandler.
    string jwt = appToken.Generate(accessKey);

    //Set app token textbox to JWT string to show that the JWT was generated
    appTokenTextbox.Text = jwt;

    //Construct reports uri resource string
    var uri = String.Format("https://api.powerbi.com/beta/collections/{0}/workspaces/{1}/reports", workspaceName, workspaceId);

    //Configure reports request
    System.Net.WebRequest request = System.Net.WebRequest.Create(uri) as System.Net.HttpWebRequest;
    request.Method = "GET";
    request.ContentLength = 0;

    //Set the WebRequest header to AppToken, and jwt
    //Note the use of AppToken instead of Bearer
    request.Headers.Add("Authorization", String.Format("AppToken {0}", jwt));

    //Get reports response from request.GetResponse()
    using (var response = request.GetResponse() as System.Net.HttpWebResponse)
    {
        //Get reader from response stream
        using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
        {
            //Deserialize JSON string into PBIReports
            PBIReports PBIReports = JsonConvert.DeserializeObject<PBIReports>(reader.ReadToEnd());

            //Clear Textbox
            tb_reportsResult.Text = string.Empty;

            //Get each report
            foreach (PBIReport rpt in PBIReports.value)
            {
                tb_reportsResult.Text += String.Format("{0}\t{1}\t{2}\n", rpt.id, rpt.name, rpt.embedUrl);
            }
        }
    }
}
```

<a name="EmbedReport"/>
## Uygulamaya bir rapor ekleme

Uygulamanıza bir rapor ekleyebilmeniz için önce bir rapora yönelik ekleme belirteci gerekir. Bu belirteç, **Power BI Embedded** REST işlemlerini çağırmak için kullanılan bir uygulama belirtecine benzer, ancak REST kaynağı yerine bir rapor kaynağı için oluşturulur. Aşağıda rapor için bir uygulama belirteci alma kodu verilmiştir. Rapor uygulama belirtecini kullanmak için bkz. [Uygulamanıza rapor ekleme](#EmbedReportJS).

<a name="EmbedReportToken"/>
### Rapor için bir uygulama belirteci alma

```
protected void getReportAppTokenButton_Click(object sender, EventArgs e)
{
    //Get an embed token for a report.
    var token = PowerBIToken.CreateReportEmbedToken(workspaceName, workspaceId, getReportAppTokenTextBox.Text);

    //After you get a PowerBIToken which has Claims including your WorkspaceName and WorkspaceID,
    //you generate JSON Web Token (JWT) . The Generate() method uses classes from System.IdentityModel.Tokens: SigningCredentials,
    //JwtSecurityToken, and JwtSecurityTokenHandler.
    string jwt = token.Generate(accessKey);

    //Set textbox to JWT string. The JavaScript embed code uses the embed jwt for a report.
    reportAppTokenTextbox.Text = jwt;
}
```

<a name="EmbedReportJS"/>
### Uygulamanıza rapor ekleme

Uygulamanıza bir **Power BI** raporu eklemek için IFrame ve bazı JavaScript kodları kullanın. Aşağıda bir rapor eklemek için örnek IFrame ve JavaScript kodu verilmiştir. Rapor eklemeye ilişkin tüm örnek kodları görmek için, GitHub’da [IFrame ile bir rapor tümleştirme](https://github.com/Azure-Samples/power-bi-embedded-iframe) örneğine bakın.

![Iframe](media\power-bi-embedded-integrate-report\Iframe.png)


```
window.onload = function () {
    // Client side click to embed a selected report.
    var el = document.getElementById("bEmbedReportAction");
    if (el.addEventListener) {
        el.addEventListener("click", updateEmbedReport, false);
    } else {
        el.attachEvent('onclick', updateEmbedReport);
    }

};

// Update embed report
function updateEmbedReport() {
    // Check if the embed url was selected
    var embedUrl = document.getElementById('tb_EmbedURL').value;

    // To load a report do the following:
    // 1: Set the url
    // 2: Add a onload handler to submit the auth token
    var iframe = document.getElementById('iFrameEmbedReport');
    iframe.src = embedUrl;
    iframe.onload = postActionLoadReport;
}

// Post the auth token to the iFrame.
function postActionLoadReport() {

    // Get the app token.
    accessToken = document.getElementById('MainContent_reportAppTokenTextbox').value;

    // Construct the push message structure
    var m = { action: "loadReport", accessToken: accessToken };
    message = JSON.stringify(m);

    // Push the message.
    iframe = document.getElementById('iFrameEmbedReport');
    iframe.contentWindow.postMessage(message, "*");
}
```
Uygulamanıza bir rapor ekledikten sonra, raporu filtreleyebilirsiniz. Sonraki bölümde, bir URL söz dizimini kullanarak bir raporun nasıl filtreleneceği gösterilir.

## Bir raporu filtreleme

Bir URL söz dizimini kullanarak eklenmiş bir raporu filtreleyebilirsiniz. Bunu yapmak için, filtreyi belirterek iFrame src url’nize bir sorgu dizesi parametresi ekleyin. **Değere göre filtreleyebilir** ve **Filtre Bölmesini gizleyebilirsiniz**.


**Değere göre filtreleme**

Değere göre filtre uygulamak için, aşağıdaki şekilde, **eq** işleciyle bir **$filter** sorgu söz dizimi kullanın.

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-0694-...-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

Örneğin, Depolama Zinciri 'Lindseys' değerine göre filtre uygulayabilirsiniz. Url’nin filtre parçası şuna benzer:

```
$filter=Store/Chain%20eq%20'Lindseys'
```

> [AZURE.NOTE] {tableName/fieldName} boşluk ya da özel karakter içeremez. {fieldValue} tek bir kategorik değer kabul eder.

**Filtre Bölmesini Gizleme**

**Filtre Bölmesi**’ni gizlemek için, rapor sorgu dizesine, aşağıdaki şekilde, **filterPaneEnabled** ekleyin.

```
&filterPaneEnabled=false
```

## Sonuç

Bu makalede, uygulamanız eklemek için bir **Power BI**raporu tümleştirmeyi öğrendiniz. Bir uygulamaya rapor tümleştirmeye hızlı şekilde başlamak için GitHub'da bu örnekleri indirin:

- [IFrame ile bir rapor tümleştirme örneği](https://github.com/Azure-Samples/power-bi-embedded-iframe)
- [Örnek pano web uygulaması](http://go.microsoft.com/fwlink/?LinkId=761493)

## Ayrıca Bkz.
- [Microsoft Power BI Embedded Önizleme kullanmaya başlama ](power-bi-embedded-get-started.md)
- [Örnek kullanmaya başlama](power-bi-embedded-get-started-sample.md)
- [System.IdentityModel.Tokens.SigningCredentials](https://msdn.microsoft.com/library/system.identitymodel.tokens.signingcredentials.aspx)
- [System.IdentityModel.Tokens.JwtSecurityToken](https://msdn.microsoft.com/library/system.identitymodel.tokens.jwtsecuritytoken.aspx)
- [System.IdentityModel.Tokens.JwtSecurityTokenHandler](https://msdn.microsoft.com/library/system.identitymodel.tokens.signingcredentials.aspx)
- [Raporları Alma](https://msdn.microsoft.com/library/mt711510.aspx)



<!----HONumber=Jun16_HO2-->


