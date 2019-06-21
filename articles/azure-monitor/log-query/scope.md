---
title: Sorgu kapsamındaki Azure İzleyici Log Analytics'te günlük | Microsoft Docs
description: Azure İzleyici Log analytics'te günlük sorgusu için kapsam ve zaman aralığını açıklar.
services: log-analytics
author: bwren
manager: carmonm
ms.service: log-analytics
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: bwren
ms.openlocfilehash: a948b80f6524339f0908a2fb19c4a83d70b3b140
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67297173"
---
# <a name="log-query-scope-and-time-range-in-azure-monitor-log-analytics"></a>Azure İzleyici Log analytics'te günlük sorgu kapsam ve zaman aralığı
Çalıştırdığınızda bir [günlük sorgusu](log-query-overview.md) içinde [Azure portalında Log Analytics](get-started-portal.md), sorgu tarafından değerlendirilen veri kümesi kapsamı ve seçtiğiniz zaman aralığı bağlıdır. Bu makalede, kapsam ve zaman aralığını ve her gereksinimlerinize bağlı olarak nasıl ayarlanacağı açıklanır. Ayrıca, farklı türlerde kapsamları davranışını açıklar.


## <a name="query-scope"></a>Sorgu kapsamı
Sorgu tarafından değerlendirilen kayıtları sorgu kapsamını tanımlar. Bu genellikle tüm kayıtları tek Log Analytics çalışma alanı veya Application Insights uygulamasını içerir. Log Analytics belirli bir izlenen Azure kaynağı için bir kapsam ayarlamanıza olanak sağlar. Bu kaynak için birden çok çalışma alanı Yazar olsa bile bu kendi veri yalnızca odaklanmak bir kaynak sahibi sağlar.

Kapsam her zaman üst kısmında görüntülenen Log Analytics pencerenin sol. Simge kapsamı bir Log Analytics çalışma alanı veya Application Insights uygulamasını olup olmadığını gösterir. Herhangi bir simge başka bir Azure kaynak gösterir.

![`Scope`](media/scope/scope.png)

Kapsam, Log Analytics başlatmak için kullandığınız ve bazı durumlarda tıklayarak kapsamı değiştirebilirsiniz yöntemi tarafından belirlenir. Aşağıdaki tabloda kullanılan kapsam farklı türlerini listeler ve her biri için farklı ayrıntıları.

| Sorgu kapsamı | Kayıt kapsamı | Seçme | Kapsam değiştirme |
|:---|:---|:---|:---|
| Log Analytics çalışma alanı | Log Analytics çalışma alanındaki tüm kayıtlar. | Seçin **günlükleri** gelen **Azure İzleyici** menüsü veya **Log Analytics çalışma alanları** menüsü.  | Kapsam, herhangi bir kaynak türü için değiştirebilirsiniz. |
| Application Insights uygulaması | Application Insights uygulamanın tüm kayıtlar. | Seçin **Analytics** gelen **genel bakış** Application Insights'ın sayfası. | Yalnızca kapsam başka bir Application Insights uygulamaya değiştirebilirsiniz. |
| Kaynak grubu | Kaynak grubundaki tüm kaynaklar tarafından oluşturulan kayıtları. Birden fazla Log Analytics çalışma alanına veri içerebilir. | Seçin **günlükleri** kaynak grubu menüsünde. | Kapsamı değiştirilemez.|
| Abonelik | Abonelikteki tüm kaynaklar tarafından oluşturulan kayıtları. Birden fazla Log Analytics çalışma alanına veri içerebilir. | Seçin **günlükleri** abonelik menüsünde.   | Kapsamı değiştirilemez. |
| Diğer Azure kaynakları | Kaynak tarafından oluşturulan kayıtları. Birden fazla Log Analytics çalışma alanına veri içerebilir.  | Seçin **günlükleri** kaynak menüsünde.<br>OR<br>Seçin **günlükleri** gelen **Azure İzleyici** menü ve yeni bir kapsam seçin. | Yalnızca kapsam için aynı kaynak türünü değiştirebilirsiniz. |

### <a name="limitations-when-scoped-to-a-resource"></a>Bir kaynağa kapsamlı yüklenirken uygulanan sınırlamalar

Portalda tüm seçenekleri ve tüm sorgu komutları sorgu kapsamındaki bir Log Analytics çalışma alanı ya da bir Application Insights uygulama olduğunda kullanılabilir. Tek bir çalışma alanı veya uygulama ile ilişkili olduğundan kaynak ancak kapsamlı, aşağıdaki kullanılamıyor portalda seçenekleri:

- Kaydet
- Sorgu Gezgini
- Yeni uyarı kuralı

Aşağıdaki komutlar, sorgu kapsamındaki tüm çalışma alanlarıyla ilgili kaynağın veya kaynak kümesi için verileri zaten içerecektir sonra bir kaynak için kapsamlı, sorguda kullanamazsınız:

- [Uygulama](app-expression.md)
- [Çalışma alanı](workspace-expression.md)
 


## <a name="time-range"></a>Zaman aralığı
Zaman aralığı için kaydın oluşturulduğu zamana göre sorgu değerlendirilen kayıt kümesini belirtir. Bu, standart bir çalışma alanı ya da uygulama aşağıdaki tabloda belirtilen tüm kayıtların özelliği tarafından tanımlanır.

| Location | Özellik |
|:---|:---|
| Log Analytics çalışma alanı          | TimeGenerated |
| Application Insights uygulaması | timestamp     |

Log Analytics pencerenin üst kısmındaki zaman toplayıcıdan seçerek zaman aralığı ayarlayın.  Önceden tanımlı bir süre seçin ya da seçin **özel** belirli bir zaman aralığı belirtmek için.

![Saat Seçici](media/scope/time-picker.png)

Yukarıdaki tabloda gösterildiği gibi standart saati özelliğini kullanan sorgudaki filtre ayarlarsanız, Saat Seçici değişikliklerini **sorguda Ayarla**, ve saat seçici devre dışı bırakılır. Bu durumda, sonraki herhangi bir işleme yalnızca filtrelenen kayıtlar ile çalışması gereken şekilde sorgu üst kısmındaki filtreyi yerleştirmek için en etkili yoldur.

![Filtrelenmiş sorgu](media/scope/query-filtered.png)

Kullanırsanız [çalışma](workspace-expression.md) veya [uygulama](app-expression.md) başka bir çalışma alanı veya uygulama verileri almak için komut, Saat Seçici farklı şekilde davranabilir. Bir Log Analytics çalışma alanı kapsamdır ve kullandığınız **uygulama**, veya kapsamı bir Application Insights uygulamasıdır ve kullanırsanız **çalışma**, Log Analytics özelliği içinde kullanılan anlamayabilir sonra Filtre süresi filtre belirlemeniz gerekir.

Aşağıdaki örnekte, kapsamı bir Log Analytics çalışma alanına ayarlanır.  Sorgu kullanan **çalışma** başka bir Log Analytics çalışma alanı verilerini almak için. Saat Seçici değişikliklerini **sorguda Ayarla** beklenen kullanan bir filtre gördüğünden **TimeGenerated** özelliği.

![Çalışma alanı ile sorgulama](media/scope/query-workspace.png)

Sorgu kullanıyorsa **uygulama** bir Application Insights uygulamasından ancak veri almak için Log Analytics tanımadığı **zaman damgası** filtre ve Saat Seçici özelliğinde değişmeden kalır. Bu durumda, iki filtre uygulanır. 7 gün içinde belirtmesine rağmen örnek sorguda yalnızca son 24 saat içinde oluşturulan kayıtları dahil **burada** yan tümcesi.

![Uygulama ile sorgulama](media/scope/query-app.png)

## <a name="next-steps"></a>Sonraki adımlar

- İzlenecek yol bir [Azure portalında Log Analytics kullanma Öğreticisi](get-started-portal.md).
- İzlenecek yol bir [sorgu yazmakla ilgili öğretici](get-started-queries.md).