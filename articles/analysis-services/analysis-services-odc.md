---
title: Bir Azure Analysis Services sunucusuna bağlanmak için .odc dosyası oluşturma | Microsoft Docs
description: Bağlanmak ve bir Azure Analysis Services sunucusundan veri almak için bir Office veri bağlantısı dosyası oluşturmayı öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 37f068be544f964f3aec63d85702098c8f382ab8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60785719"
---
# <a name="create-an-office-data-connection-file"></a>Bir Office veri bağlantısı dosyası oluştur

Bu makaledeki bilgiler, Excel 2016'dan sürüm numarası 16.0.7369.2117 bir Azure Analysis Services sunucusuna bağlanmak için bir Office veri bağlantısı dosyası nasıl oluşturabileceğinizi açıklar veya daha önce veya Excel 2013. Güncelleştirilmiş [MSOLAP.7 sağlayıcısı](analysis-services-data-providers.md) de gereklidir.


1. Aşağıdaki örnek bağlantı dosyasını kopyalayın ve bir metin düzenleyicisine yapıştırın. 

2. İçinde `odc:ConnectionString`, aşağıdaki özellikleri değiştirin:

    *   İçinde `Data Source=asazure://<region>.asazure.windows.net/<servername>;` değiştirme `<region>` Analysis Services sunucunuzun bölgeye ve `<servername>` sunucunuzun adı.

    *   İçinde `Initial Catalog=<database>;` değiştirme `<database>` veritabanınızın adı.

3. İçinde `<odc:CommandText>Model</odc:CommandText>` değiştirme `Model` modeli veya perspektif adı. 

4. Dosyayı kaydetmek bir `.odc` C:\Users uzantısı\\*kullanıcıadı*\Documents\My veri kaynakları klasörü.

5. Dosyaya sağ tıklayın ve ardından **Excel'de Aç**. Veya Excel'de üzerinde **veri** Şerit, tıklayın **varolan bağlantılar**dosyanızı seçin ve ardından **açık**.



**Örnek bağlantı dosyası**
```
<html xmlns:o="urn:schemas-microsoft-com:office:office"
xmlns="https://www.w3.org/TR/REC-html40">

<head>
<meta http-equiv=Content-Type content="text/x-ms-odc; charset=utf-8">
<meta name=ProgId content=ODC.Cube>
<meta name=SourceType content=OLEDB>
<meta name=Catalog content="Database">
<meta name=Table content=Model>
<title>AzureAnalysisServicesConnection</title>
<xml id=docprops><o:DocumentProperties
  xmlns:o="urn:schemas-microsoft-com:office:office"
  xmlns="https://www.w3.org/TR/REC-html40">
  <o:Name>SampleAzureAnalysisServices</o:Name>
 </o:DocumentProperties>
</xml><xml id=msodc><odc:OfficeDataConnection
  xmlns:odc="urn:schemas-microsoft-com:office:odc"
  xmlns="https://www.w3.org/TR/REC-html40">
  <odc:Connection odc:Type="OLEDB">
   <odc:ConnectionString>Provider=MSOLAP.7;Data Source=asazure://<region>.asazure.windows.net/<servername>;Initial Catalog=<database>;</odc:ConnectionString>
   <odc:CommandType>Cube</odc:CommandType>
   <odc:CommandText>Model</odc:CommandText>
  </odc:Connection>
 </odc:OfficeDataConnection>
</xml>
<style>
<!--
    .ODCDataSource
    {
    behavior: url(dataconn.htc);
    }
-->
</style>
 
</head>

<body onload='init()' scroll=no leftmargin=0 topmargin=0 rightmargin=0 style='border: 0px'>
<table style='border: solid 1px threedface; height: 100%; width: 100%' cellpadding=0 cellspacing=0 width='100%'> 
  <tr> 
    <td id=tdName style='font-family:arial; font-size:medium; padding: 3px; background-color: threedface'> 
      &nbsp; 
    </td> 
     <td id=tdTableDropdown style='padding: 3px; background-color: threedface; vertical-align: top; padding-bottom: 3px'>

      &nbsp; 
    </td> 
  </tr> 
  <tr> 
    <td id=tdDesc colspan='2' style='border-bottom: 1px threedshadow solid; font-family: Arial; font-size: 1pt; padding: 2px; background-color: threedface'>

      &nbsp; 
    </td> 
  </tr> 
  <tr> 
    <td colspan='2' style='height: 100%; padding-bottom: 4px; border-top: 1px threedhighlight solid;'> 
      <div id='pt' style='height: 100%' class='ODCDataSource'></div> 
    </td> 
  </tr> 
</table> 

  
<script language='javascript'> 

function init() { 
  var sName, sDescription; 
  var i, j; 
  
  try { 
    sName = unescape(location.href) 
  
    i = sName.lastIndexOf(".") 
    if (i>=0) { sName = sName.substring(1, i); } 
  
    i = sName.lastIndexOf("/") 
    if (i>=0) { sName = sName.substring(i+1, sName.length); } 

    document.title = sName; 
    document.getElementById("tdName").innerText = sName; 

    sDescription = document.getElementById("docprops").innerHTML; 
  
    i = sDescription.indexOf("escription>") 
    if (i>=0) { j = sDescription.indexOf("escription>", i + 11); } 

    if (i>=0 && j >= 0) { 
      j = sDescription.lastIndexOf("</", j); 

      if (j>=0) { 
          sDescription = sDescription.substring(i+11, j); 
        if (sDescription != "") { 
            document.getElementById("tdDesc").style.fontSize="x-small"; 
          document.getElementById("tdDesc").innerHTML = sDescription; 
          } 
        } 
      } 
    } 
  catch(e) { 

    } 
  } 
</script> 

</body> 
 
</html>

```



