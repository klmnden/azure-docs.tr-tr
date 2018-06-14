---
title: Azure Storage ile çalışmak için Araçlar | Microsoft Docs
description: Görünüm/Azure Storage verilerinizle etkileşim olanak sağlayan araçlar listesi.
services: storage
documentationcenter: ''
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: e4748642-98c4-437e-b0ed-4f9641c2e894
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: dineshmurthy
ms.openlocfilehash: 5c2add48b128a3e5a632c048f0feb4413fcb26cc
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
ms.locfileid: "23933392"
---
# <a name="azure-storage-client-tools"></a>Azure Storage İstemci Araçları
Azure Storage kullanıcılarının sık bir Azure Storage istemci araç kullanarak kullanıcıların verileriyle görünüm/etkileşim kurmak kullanabilmek ister. Aşağıdaki tablolarda, bunu yapmak izin araçları sayısını listeler. Ya da numaralandırır ve/veya veri soyutlama erişim olanağı sağlar, biz "X" her blok yerleştirin. Tablo ayrıca araçları olup olmadığını boş olmadığını gösterir. "Deneme" ücretsiz deneme yoktur, ancak tam ürün boş değil gösterir. "Y/N" farklı bir sürümünü satın almak için uygun olsa da bir sürümünün ücretsiz olarak kullanılabilir olduğunu gösterir.

Bir anlık görüntü kullanılabilir Azure Storage istemci Araçları'nın yalnızca sağladık. Bu araçları gelişmesi ve işlevselliğinde büyümesine devam edebilir. Düzeltmeler veya güncelleştirmeler varsa, lütfen bize bildirin bir yorum bırakın. Aynı burada - gelmelidir araçlarının biliyorsanız, biz eklemediğiniz mutluluk olacaktır geçerlidir.

**Microsoft Azure Storage istemci araçları**

<table>
  <tr>
    <th rowspan="2">Azure Storage istemci aracı</th>
    <th rowspan="2">Blok blobu</th>
    <th rowspan="2">Sayfa blobu</th>
    <th rowspan="2">BLOB ekleme</th>
    <th rowspan="2">Tablolar</th>
    <th rowspan="2">Kuyruklar</th>
    <th rowspan="2">Dosyalar</th>
    <th rowspan="2">Ücretsiz</th>
    <th colspan="4">Platform</th>
  </tr>
  <tr>
    <td>Web</td>
    <td>Windows</td>
    <td>OSX</td>
    <td>Linux</td>
  </tr>
  <tr>
    <td><a href="https://azure.microsoft.com/features/azure-portal/">Microsoft Azure portalı</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>E</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="http://storageexplorer.com/">Microsoft Azure Depolama Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
  </tr>
  <tr>
    <td><a href="https://www.visualstudio.com/features/azure-tools-vs.aspx">Microsoft Visual Studio Sunucu Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
</table>

**Üçüncü taraf Azure Storage istemci araçları**

Biz işlev veya aşağıdaki üçüncü taraf araçları tarafından talep kalite doğrulamadınız demektir ve bunların listeleme Microsoft tarafından bir onay anlamına gelmez.

<table>
  <tr>
    <th rowspan="2">Azure Storage istemci aracı</th>
    <th rowspan="2">Blok blobu</th>
    <th rowspan="2">Sayfa blobu</th>
    <th rowspan="2">BLOB ekleme</th>
    <th rowspan="2">Tablolar</th>
    <th rowspan="2">Kuyruklar</th>
    <th rowspan="2">Dosyalar</th>
    <th rowspan="2">Ücretsiz</th>
    <th colspan="4">Platform</th>
  </tr>
  <tr>
    <td>Web</td>
    <td>Windows</td>
    <td>OSX</td>
    <td>Linux</td>
  </tr>
  <tr>
    <td><a href="http://www.cerebrata.com/products/azure-management-studio/introduction">Cerabrata: Azure Management Studio</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>Deneme</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://www.red-gate.com/products/azure-development/azure-explorer/index">Redgate: Azure Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="https://github.com/sebagomez/azurestorageexplorer">Azure Web Depolama Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="http://www.cloudberrylab.com/explorer/microsoft-azure.aspx">CloudBerry Gezgini</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
    <td>X</td>
    <td>Y/N</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="http://www.gapotchenko.com/cloudcombine">Bulut birleştirin</a></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>X</td>
    <td>X</td>
    <td></td>
    <td>Deneme</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="http://clumsyleaf.com">ClumsyLeaf: AzureXplorer, CloudXplorer, TableXplorer</a></td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>X</td>
    <td>E</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><a href="http://www.gladinet.com/Azure-Storage/index.htm">Gladinet bulut</a></td>
    <td>X</td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td>Deneme</td>
    <td></td>
    <td>X</td>
    <td></td>
    <td></td>
  </tr>
</table>
