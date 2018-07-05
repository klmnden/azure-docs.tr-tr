---
title: Azure Analysis Services sunucularına bağlanma | Microsoft Docs
description: Bağlanmak ve verileri azure'da bir Analysis Services sunucusundan alma hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 19f5fb7b779f538f46b1813f30795e01a75f065c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443265"
---
# <a name="connecting-to-servers"></a>Sunuculara bağlanma

Bu makalede, veri modelleme ve SQL Server Management Studio (SSMS) veya SQL Server veri Araçları (SSDT) gibi yönetim uygulamalarını kullanarak sunucuya bağlanma açıklanır. Veya, ile istemci raporlama uygulamalarında Microsoft Excel, Power BI Desktop veya özel uygulamalar gibi. Azure Analysis Services bağlantıları HTTPS kullanır.

## <a name="client-libraries"></a>İstemci kitaplıkları
[En son istemci kitaplıkları alma](analysis-services-data-providers.md)

Tüm bağlantılar, türünden bağımsız olarak bir sunucuya bağlanın ve bir Analysis Services sunucusuyla arabirim güncelleştirilmiş AMO ADOMD.NET ve OLEDB istemci kitaplıkları gerekir. SSMS, SSDT, Excel 2016 ve Power BI için en son istemci kitaplıkları yüklü veya aylık sürümleriyle güncelleştirilir. Ancak, bazı durumlarda, bir uygulamanın en son olmayabilir mümkündür. Örneğin, ne zaman ilkeleri gecikme güncelleştirir veya Office 365 güncelleştirmelerini ertelenmiş kanalı yararlanabilirsiniz.

## <a name="server-name"></a>Sunucu adı

Azure'da bir Analysis Services sunucusu oluşturduğunuzda, benzersiz bir ad ve sunucu oluşturulacak olduğu bölge belirtin. Sunucu adı bağlantı belirtirken, sunucu adlandırma şeması aşağıdaki gibidir:

```
<protocol>://<region>/<servername>
```
 Dize olduğu Protokolü **asazure**, bölgedir sunucunun oluşturulduğu URI (örneğin, westus.asazure.windows.net) ve servername bölge içinde benzersiz sunucunuzun adıdır.

### <a name="get-the-server-name"></a>Sunucu adını alma
İçinde **Azure portalında** > sunucu > **genel bakış** > **sunucu adı**, tüm sunucu adını kopyalayın. Kuruluşunuzdaki diğer kullanıcılar bu sunucuyu çok bağlanıyorsanız, bu sunucu adı ile bunları paylaşabilirsiniz. Bir sunucu adı belirtilirken, tüm yol kullanılmalıdır.

![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Bağlantı dizesi

Tablolu nesne modelini kullanarak Azure Analysis Services'e bağlanırken, aşağıdaki bağlantı dize biçimleri kullanın:

###### <a name="integrated-azure-active-directory-authentication"></a>Tümleşik Azure Active Directory kimlik doğrulaması
Varsa, tümleşik kimlik doğrulaması Azure Active Directory kimlik bilgisi önbelleği kullanır. Aksi durumda, Azure oturum açma penceresinde gösterilir.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Kullanıcı adı ve parola ile Azure Active Directory kimlik doğrulaması

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows kimlik doğrulaması (tümleşik güvenlik)
Geçerli işlem çalıştıran Windows hesabı kullanın.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>.Odc dosyası kullanarak bağlan
Excel'in önceki sürümleriyle, kullanıcılar Office veri bağlantısı (.odc) dosyası kullanarak bir Azure Analysis Services sunucusuna bağlanabilir. Daha fazla bilgi için bkz. [Office veri bağlantısı (.odc) dosyası oluşturma](analysis-services-odc.md).


## <a name="next-steps"></a>Sonraki adımlar
[Excel ile bağlanma](analysis-services-connect-excel.md)    
[Power BI ile bağlanma](analysis-services-connect-pbi.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)   

