---
title: Azure Analysis Services sunucularına bağlanma | Microsoft Docs
description: Bağlanmak ve Azure Analysis Services sunucusundan veri alma hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 53a8a1eea5ffa50fcdaf4a60c9bbd03d30d8e311
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="connecting-to-servers"></a>Sunuculara bağlanma

Bu makalede, veri modellemesi ve SQL Server Management Studio (SSMS) veya SQL Server veri Araçları (SSDT) gibi yönetim uygulamaları kullanarak bir sunucuya bağlanma açıklanır. Veya Microsoft Excel, Power BI Desktop veya özel uygulamalar gibi uygulamalar istemcisiyle raporlama. Azure Analysis Services bağlantı HTTPS kullanın.

## <a name="client-libraries"></a>İstemci kitaplıkları
[Son istemci kitaplıkları Al](analysis-services-data-providers.md)

Tüm bağlantılar türü, bağımsız olarak bir sunucuya bağlanmak ve Analysis Services sunucusu ile arabirim için güncelleştirilmiş AMO, ADOMD.NET ve OLEDB istemci kitaplıkları gerektirir. SSMS, SSDT, Excel 2016 ve Power BI için en son istemci kitaplıkları yüklü veya aylık sürümleriyle güncelleştirilir. Ancak, bazı durumlarda, bir uygulamanın en son olmayabilir mümkündür. Örneğin, ne zaman ilkeleri gecikme güncelleştirir veya Office 365 güncelleştirmelerini ertelenmiş kanalı markalarıdır.

## <a name="server-name"></a>Sunucu adı

Azure'da bir Analysis Services sunucusu oluşturduğunuzda, benzersiz bir ad ve sunucunun oluşturulması olduğu bölge belirtin. Bir bağlantı sunucu adı belirtme sunucu adlandırma şeması olur:

```
<protocol>://<region>/<servername>
```
 Dize olduğu Protokolü **asazure**, bölgedir sunucu oluşturulduğu URI (örneğin, westus.asazure.windows.net) ve servername bölge içinde benzersiz sunucunuzun adıdır.

### <a name="get-the-server-name"></a>Sunucu adını Al
İçinde **Azure portal** > server > **genel bakış** > **sunucu adı**, tüm sunucu adını kopyalayın. Kuruluşunuzda bulunan diğer kullanıcıların bu sunucuya çok bağlanıyorsanız, bu sunucu adı onlarla paylaşabilirsiniz. Bir sunucu adı belirtirken, tüm yol kullanılmalıdır.

![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Bağlantı dizesi

Tablo nesne modelini kullanarak Azure Analysis Services için bağlanırken aşağıdaki bağlantı dize biçimleri kullanın:

###### <a name="integrated-azure-active-directory-authentication"></a>Tümleşik Azure Active Directory kimlik doğrulaması
Tümleşik kimlik doğrulaması Azure Active Directory kimlik bilgisi önbelleği varsa seçer. Aksi durumda, Azure oturum açma penceresi gösterilir.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Kullanıcı adı ve parola ile Azure Active Directory kimlik doğrulaması

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows kimlik doğrulaması (tümleşik güvenliği)
Geçerli işlem çalıştıran Windows hesabını kullanın.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>Bir .odc dosyası kullanarak bağlan
Excel'in eski sürümleriyle, kullanıcıların bir Office veri bağlantısı (.odc) dosyası kullanarak bir Azure Analysis Services sunucusuna bağlanabilir. Daha fazla bilgi için bkz: [bir Office veri bağlantısı (.odc) dosyası oluşturun](analysis-services-odc.md).


## <a name="next-steps"></a>Sonraki adımlar
[Excel ile bağlanma](analysis-services-connect-excel.md)    
[Power BI ile bağlanma](analysis-services-connect-pbi.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)   

