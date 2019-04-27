---
title: Bilgisayar grupları Azure İzleyici'de oturum sorgular | Microsoft Docs
description: Azure İzleyici'de bilgisayar grupları, bilgisayarlar belirli bir dizi kapsam günlük sorguları izin verir.  Bu makalede, bilgisayar gruplarını ve bunların günlük sorguda nasıl kullanılacağını oluşturmak için kullanabileceğiniz farklı yöntemler açıklanır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: bwren
ms.openlocfilehash: c2babb5a86d69881b6a76c6dceae80a24a891f6c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60741003"
---
# <a name="computer-groups-in-azure-monitor-log-queries"></a>Azure İzleyici günlük sorguları bilgisayar grupları
Azure İzleyici'de bilgisayar grupları kapsamına izin [oturum sorguları](../log-query/log-query-overview.md) bilgisayarlar belirli bir dizi.  Her Grup ya da tanımladığınız bir sorgu kullanarak bilgisayarları veya grupları alarak farklı kaynaktaki doldurulur.  Grup içinde bir günlük sorgu eklendiğinde sonuçları gruptaki bilgisayarların eşleşen kayıtları sınırlıdır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="creating-a-computer-group"></a>Bir bilgisayar grubu oluşturuluyor
Azure İzleyici'de aşağıdaki tabloda yöntemlerden birini kullanarak bir bilgisayar grubu oluşturabilirsiniz.  Aşağıdaki bölümlerde her yöntemi hakkında ayrıntılı bilgi sağlanır. 

| Yöntem | Açıklama |
|:--- |:--- |
| Günlük sorgusu |Bilgisayar listesi döndüren bir günlük sorgusu oluşturun. |
| Log Arama API’si |Günlük arama API'si, program aracılığıyla bir günlük sorgusu sonuçlarına dayalı bir bilgisayar grubu oluşturmak için kullanın. |
| Active Directory |Otomatik olarak bir Active Directory etki alanının üyesi olan ve bir grubu, Azure İzleyici'de için her güvenlik grubu oluşturun. herhangi bir aracı bilgisayarların grup üyeliği tarayın. (Yalnızca Windows makinelerde)|
| Configuration Manager | System Center Configuration Manager'dan koleksiyonları içeri aktarma ve Azure İzleyici'de her biri için bir grup oluşturun. |
| Windows Server Update Services |Otomatik olarak WSUS sunucular veya istemciler grupları hedeflemek için tarama ve Azure İzleyici'de her biri için bir grup oluşturun. |

### <a name="log-query"></a>Günlük sorgusu
Bilgisayar grupları günlük sorgudan oluşturulan tanımladığınız bir sorgu tarafından döndürülen tüm bilgisayarların içerir.  Bu sorguyu bilgisayar grubu grubun oluşturulmasından bu yana değişiklikler yansıtılır böylece her kullanılışında çalıştırılır.  

Bir bilgisayar grubu için herhangi bir sorgu kullanabilirsiniz, ancak farklı bir bilgisayar kullanarak döndürmelidir `distinct Computer`.  Bir bilgisayar grubu olarak için kullanabileceğiniz örnek sorgu verilmiştir.

    Heartbeat | where Computer contains "srv" | distinct Computer

Azure portalında günlük araması bir bilgisayar grubu oluşturmak için aşağıdaki yordamı kullanın.

1. Tıklayın **günlükleri** içinde **Azure İzleyici** Azure portalındaki menü.
1. Oluşturun ve gruba istediğiniz bilgisayarları döndüren bir sorgu çalıştırın.
1. Tıklayın **Kaydet** ekranın üstünde.
1. Değişiklik **Kaydet** için **işlevi** seçip **bu sorguyu bilgisayar grubu olarak Kaydet**.
1. Bilgisayar grubu tabloda açıklanan ve'a tıklayın, her bir özellik için değerleri sağlayın **Kaydet**.

Aşağıdaki tabloda, bir bilgisayar grubu tanımlayan özellikleri açıklanmaktadır.

| Özellik | Açıklama |
|:---|:---|
| Ad   | Portalda görüntülemek için sorguyu adı. |
| İşlev diğer adı | Bir sorguda bilgisayar grubu tanımlamak için kullanılan benzersiz bir diğer ad. |
| Kategori       | Portalda sorguları düzenlemek için kategorisi. |


### <a name="active-directory"></a>Active Directory
Azure İzleyici'nın Active Directory grup üyeliklerini İçeri Aktar'ı yapılandırırken, Log Analytics aracısını ile tüm Windows etki alanına katılmış bilgisayarların grup üyeliği analiz eder.  Bir bilgisayar grubu, Azure İzleyici'de her Active Directory güvenlik grubu oluşturulur ve her Windows bilgisayarı üyesi olduğu güvenlik gruplarına karşılık gelen bilgisayar gruplarına eklenir.  Bu üyelik sürekli olarak 4 saatte bir güncelleştirilir.  

> [!NOTE]
> İçeri aktarılan Active Directory grupları, yalnızca Windows makineleri içerir.

Azure Active Directory güvenlik grupları'ndan almak için İzleyici yapılandırma **Gelişmiş ayarlar** Azure portalında Log Analytics çalışma alanınızda.  Seçin **bilgisayar grupları**, **Active Directory**, ardından **alma Active Directory grup üyeliklerini bilgisayarlardan**.  Başka bir yapılandırma işlemi gerekmez.

![Bilgisayar grupları Active Directory'den](media/computer-groups/configure-activedirectory.png)

Grupları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Döndürmek için bu bağlantıları birini tıklayabilirsiniz **ComputerGroup** bu bilgiyi kaydeder.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Azure İzleyici, WSUS grup üyeliklerini İçeri Aktar'ı yapılandırırken, herhangi bir Log Analytics aracısını bilgisayarlarla hedefleme grup üyeliğini analiz eder.  İstemci tarafı kullanıyorsanız hedefleme, Azure İzleyici bağlı ve tüm WSUS parçası olan herhangi bir bilgisayar grupları hedefleme alınan Azure İzleyici grup üyeliği sahiptir. Sunucu tarafı kullanıyorsanız hedefleme, Log Analytics aracısını sırada Azure İzleyici içeri aktarılacak grup üyeliği bilgileri için WSUS sunucusunda yüklenmelidir.  Bu üyelik sürekli olarak 4 saatte bir güncelleştirilir. 

Azure İzleyici, WSUS gruplardan içeri aktarmak için yapılandırdığınız **Gelişmiş ayarlar** Azure portalında Log Analytics çalışma alanınızda.  Seçin **bilgisayar grupları**, **WSUS**, ardından **içeri aktarma WSUS grup üyeliklerini**.  Başka bir yapılandırma işlemi gerekmez.

![WSUS bilgisayar grupları](media/computer-groups/configure-wsus.png)

Grupları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Döndürmek için bu bağlantıları birini tıklayabilirsiniz **ComputerGroup** bu bilgiyi kaydeder.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Azure İzleyici, Configuration Manager koleksiyon üyeliklerini içeri aktarmak için yapılandırdığınızda, her bir koleksiyon için bir bilgisayar grubu oluşturur.  Koleksiyon üyeliği bilgilerini, bilgisayar gruplarını güncel kalmasını sağlamak için 3 saatte alınır. 

Configuration Manager koleksiyonları içeri aktarmadan önce şunları yapmalısınız [Configuration Manager'a bağlanmak için Azure İzleyici](collect-sccm.md).  Daha sonra İçeri Aktar yapılandırabilirsiniz **Gelişmiş ayarlar** Azure portalında Log Analytics çalışma alanınızda.  Seçin **bilgisayar grupları**, **SCCM**, ardından **alma Configuration Manager koleksiyon üyelikleri**.  Başka bir yapılandırma işlemi gerekmez.

![SCCM bilgisayar grupları](media/computer-groups/configure-sccm.png)

Koleksiyonları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Döndürmek için bu bağlantıları birini tıklayabilirsiniz **ComputerGroup** bu bilgiyi kaydeder.

## <a name="managing-computer-groups"></a>Bilgisayar gruplarını yönetme
Günlük sorgusu veya günlük arama API'den oluşturulan bilgisayar grupları görüntüleyebileceğiniz **Gelişmiş ayarlar** Azure portalında Log Analytics çalışma alanınızda.  Seçin **bilgisayar grupları** ardından **grupları kaydedilen**.  

Tıklayın **x** içinde **Kaldır** bilgisayar grubunu silmek için sütun.  Tıklayın **üyelerini görüntüle** üyelerini döndürür grubun günlük araması gerçekleştirmek bir grubu simgesi.  Bir bilgisayar grubu değiştirilemez, ancak bunun yerine gerekir silin ve değiştirilen ayarlarla yeniden oluşturun.

![Kayıtlı bilgisayar grupları](media/computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-query"></a>Bir bilgisayar grubu günlük sorgu kullanma
Genellikle, aşağıdaki sözdizimini kullanarak bir işlev olarak diğer düşünerek sorguda bir günlük sorgusu oluşturulan bir bilgisayar grubu kullanabilirsiniz:

  `Table | where Computer in (ComputerGroup)`

Örneğin, aşağıdaki mycomputergroup adlı bir bilgisayar grubunda yalnızca bilgisayarlar için UpdateSummary kayıtları döndürmek için kullanabilirsiniz.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


İçeri aktarılan bilgisayar grupları ve dahil edilen bilgisayarlarında depolanır **ComputerGroup** tablo.  Örneğin, aşağıdaki sorguyu bilgisayarların listesini etki alanı bilgisayarları grubunda Active Directory'den döndürecektir. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

Aşağıdaki sorguda UpdateSummary kayıtları yalnızca bilgisayarlar için etki alanı bilgisayarları döndürür.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```




## <a name="computer-group-records"></a>Bilgisayar Grup kaydı
Active Directory veya WSUS oluşturulan her bilgisayar grup üyeliğini için Log Analytics çalışma alanında bir kayıt oluşturulur.  Bu kayıtları bir türüne sahip **ComputerGroup** ve aşağıdaki tabloda gösterilen özelliklere sahiptir.  Kayıt günlüğü sorgulara bağlı bilgisayar grupları için oluşturulmaz.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*ComputerGroup* |
| `SourceSystem` |*Analytics'teki* |
| `Computer` |Üye bilgisayar adıdır. |
| `Group` |Grubun adı. |
| `GroupFullName` |Kaynak ve kaynak adı da dahil olmak üzere grubun tam yolu. |
| `GroupSource` |Bu grup kaynak gelen toplanmış. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| `GroupSourceName` |Grup toplandığı kaynağının adı.  Active Directory'de, bu etki alanı adıdır. |
| `ManagementGroupName` |SCOM aracıları için yönetim grubunun adı.  Diğer aracılar için AOI - budur\<çalışma alanı kimliği\> |
| `TimeGenerated` |Tarih ve saat bilgisayar grubu oluşturulduğunda veya güncelleştirildiğinde. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.  

