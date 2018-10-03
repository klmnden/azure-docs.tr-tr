---
title: Bilgisayar grupları Azure Log analytics'te günlük aramaları | Microsoft Docs
description: Log analytics'te bilgisayar grupları, bilgisayarlar belirli bir dizi kapsam günlük aramaları izin verir.  Bu makalede, bilgisayar grupları ve nasıl bir günlük aramasında kullanılacak oluşturmak için kullanabileceğiniz farklı yöntemler açıklanır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 7e4889148a752b552f8bd65702ea5dda450ded31
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48044306"
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Bilgisayar grupları Log analytics'te günlük aramaları

Log analytics'te bilgisayar grupları kapsamına izin [günlük aramaları](log-analytics-log-search-new.md) bilgisayarlar belirli bir dizi.  Her Grup ya da tanımladığınız bir sorgu kullanarak bilgisayarları veya grupları alarak farklı kaynaktaki doldurulur.  Grubun bir günlük aramasına dahil olduğu zaman sonuçları gruptaki bilgisayarlara eşleşen kayıtları sınırlıdır.

## <a name="creating-a-computer-group"></a>Bir bilgisayar grubu oluşturuluyor
Aşağıdaki tabloda yöntemlerden birini kullanarak Log analytics'te bilgisayar grubu oluşturabilirsiniz.  Aşağıdaki bölümlerde her yöntemi hakkında ayrıntılı bilgi sağlanır. 

| Yöntem | Açıklama |
|:--- |:--- |
| Günlük araması |Bilgisayar listesi döndüren bir günlük araması oluşturun. |
| Log Arama API’si |Günlük arama API'si, program aracılığıyla bir günlük araması sonuçlarına göre bir bilgisayar grubu oluşturmak için kullanın. |
| Active Directory |Otomatik olarak bir Active Directory etki alanının üyesi olan ve bir grubu Log Analytics'te için her güvenlik grubu oluşturun. herhangi bir aracı bilgisayarların grup üyeliği tarayın. |
| Configuration Manager | System Center Configuration Manager'dan koleksiyonları içeri aktarmak ve Log Analytics'te her biri için bir grup oluşturun. |
| Windows Server Update Services |Otomatik olarak WSUS sunucular veya istemciler grupları hedeflemek için tarama ve Log Analytics'te her biri için bir grup oluşturun. |

### <a name="log-search"></a>Günlük araması
Bilgisayar grupları günlük aramasından oluşturulabilir, tanımladığınız bir sorgu tarafından döndürülen tüm bilgisayarların içerir.  Bu sorguyu bilgisayar grubu grubun oluşturulmasından bu yana değişiklikler yansıtılır böylece her kullanılışında çalıştırılır.  

Bir bilgisayar grubu için herhangi bir sorgu kullanabilirsiniz, ancak farklı bir bilgisayar kullanarak döndürmelidir `distinct Computer`.  Bir bilgisayar grubu olarak için kullanabileceğiniz örnek arama aşağıdadır.

    Heartbeat | where Computer contains "srv" | distinct Computer

Aşağıdaki tabloda, bir bilgisayar grubu tanımlayan özellikleri açıklanmaktadır.

| Özellik | Açıklama |
|:---|:---|
| Görünen Ad   | Portalda görüntülemek için arama adı. |
| Kategori       | Aramalar portalında düzenlemek için kategorisi. |
| Sorgu          | Bilgisayar grubu sorgusu. |
| İşlev diğer adı | Bir sorguda bilgisayar grubu tanımlamak için kullanılan benzersiz bir diğer ad. |

Azure portalında günlük araması bir bilgisayar grubu oluşturmak için aşağıdaki yordamı kullanın.

2. Açık **günlük araması** ve ardından **kayıtlı aramalar** ekranın üstünde.
3. Tıklayın **Ekle** ve bilgisayar grubu için her bir özellik için değerler sağlayın.
4. Seçin **bu sorguyu bilgisayar grubu olarak Kaydet** tıklatıp **Tamam**.



### <a name="active-directory"></a>Active Directory
Log Analytics, Active Directory grup üyeliklerini içeri aktarmak için yapılandırdığınızda, OMS Aracısı ile herhangi bir etki alanına katılmış bilgisayarların grup üyeliği analiz eder.  Bir bilgisayar grubu, Log Analytics'te her Active Directory güvenlik grubu oluşturulur ve her bilgisayarın üyesi olduğu güvenlik gruplarına karşılık gelen bilgisayar gruplarına eklenir.  Bu üyelik sürekli olarak 4 saatte bir güncelleştirilir.  

Log Analytics, Log Analytics'ten Active Directory güvenlik gruplarını almak için yapılandırdığınız **Gelişmiş ayarlar** Azure portalında.  Seçin **bilgisayar grupları**, **Active Directory**, ardından **alma Active Directory grup üyeliklerini bilgisayarlardan**.  Başka bir yapılandırma işlemi gerekmez.

![Bilgisayar grupları Active Directory'den](media/log-analytics-computer-groups/configure-activedirectory.png)

Grupları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Döndürmek için bu bağlantıları birini tıklayabilirsiniz **ComputerGroup** bu bilgiyi kaydeder.

### <a name="windows-server-update-service"></a>Windows Server Update Service
WSUS grup üyeliklerini içeri aktarmak için Log Analytics yapılandırdığınızda, OMS Aracısı ile ilgili tüm bilgisayarlar hedefleme grup üyeliğini analiz eder.  İstemci tarafı kullanıyorsanız hedefleme, Log Analytics'e bağlı ve tüm WSUS parçası olan herhangi bir bilgisayar grupları hedefleme Log Analytics'e içe grup üyeliği sahiptir. Sunucu tarafı kullanıyorsanız hedefleme, OMS Aracısı Log Analytics'e içeri aktarılacak grubu üyelik bilgilerini WSUS sunucusu yüklenmelidir.  Bu üyelik sürekli olarak 4 saatte bir güncelleştirilir. 

Log Analytics, Log Analytics'ten içe aktarma WSUS grupları için yapılandırdığınız **Gelişmiş ayarlar** Azure portalında.  Seçin **bilgisayar grupları**, **WSUS**, ardından **içeri aktarma WSUS grup üyeliklerini**.  Başka bir yapılandırma işlemi gerekmez.

![WSUS bilgisayar grupları](media/log-analytics-computer-groups/configure-wsus.png)

Grupları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Döndürmek için bu bağlantıları birini tıklayabilirsiniz **ComputerGroup** bu bilgiyi kaydeder.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Configuration Manager koleksiyon üyeliklerini içeri aktarmak için Log Analytics yapılandırdığınızda, her bir koleksiyon için bir bilgisayar grubu oluşturur.  Koleksiyon üyeliği bilgilerini, bilgisayar gruplarını güncel kalmasını sağlamak için 3 saatte alınır. 

Configuration Manager koleksiyonları içeri aktarmadan önce şunları yapmalısınız [Configuration Manager'ı Log Analytics'e bağlama](log-analytics-sccm.md).  Log Analytics alma daha sonra yapılandırabilirsiniz **Gelişmiş ayarlar** Azure portalında.  Seçin **bilgisayar grupları**, **SCCM**, ardından **alma Configuration Manager koleksiyon üyelikleri**.  Başka bir yapılandırma işlemi gerekmez.

![SCCM bilgisayar grupları](media/log-analytics-computer-groups/configure-sccm.png)

Koleksiyonları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Döndürmek için bu bağlantıları birini tıklayabilirsiniz **ComputerGroup** bu bilgiyi kaydeder.

## <a name="managing-computer-groups"></a>Bilgisayar gruplarını yönetme
Günlük araması veya Log Analytics günlük araması API'den oluşturulan bilgisayar grupları görüntüleyebileceğiniz **Gelişmiş ayarlar** Azure portalında.  Seçin **bilgisayar grupları** ardından **grupları kaydedilen**.  

Tıklayın **x** içinde **Kaldır** bilgisayar grubunu silmek için sütun.  Tıklayın **üyelerini görüntüle** üyelerini döndürür grubun günlük araması gerçekleştirmek bir grubu simgesi.  Bir bilgisayar grubu değiştirilemez, ancak bunun yerine gerekir silin ve değiştirilen ayarlarla yeniden oluşturun.

![Kayıtlı bilgisayar grupları](media/log-analytics-computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-search"></a>Bir bilgisayar grubu içinde bir günlük araması kullanma
Genellikle, aşağıdaki sözdizimini kullanarak bir işlev olarak diğer düşünerek bir günlük araması sorgu oluşturulan bir bilgisayar grubu kullanabilirsiniz:

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
Active Directory veya WSUS oluşturulan her bilgisayar grup üyeliğini için Log Analytics çalışma alanında bir kayıt oluşturulur.  Bu kayıtları bir türüne sahip **ComputerGroup** ve aşağıdaki tabloda gösterilen özelliklere sahiptir.  Günlük aramaları bağlı bilgisayar grupları için kayıt oluşturulmaz.

| Özellik | Açıklama |
|:--- |:--- |
| Tür |*ComputerGroup* |
| SourceSystem |*Analytics'teki* |
| Bilgisayar |Üye bilgisayar adıdır. |
| Grup |Grubun adı. |
| GroupFullName |Kaynak ve kaynak adı da dahil olmak üzere grubun tam yolu. |
| GroupSource |Bu grup kaynak gelen toplanmış. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |Grup toplandığı kaynağının adı.  Active Directory'de, bu etki alanı adıdır. |
| ManagementGroupName |SCOM aracıları için yönetim grubunun adı.  Diğer aracılar için AOI - budur\<çalışma alanı kimliği\> |
| TimeGenerated |Tarih ve saat bilgisayar grubu oluşturulduğunda veya güncelleştirildiğinde. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [günlük aramaları](log-analytics-log-searches.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.  

