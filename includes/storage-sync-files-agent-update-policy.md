---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/11/2018
ms.author: tamram
ms.openlocfilehash: 4f59f68c1598f737ea7cb3a0e8046fc0779ed9d3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113800"
---
Azure dosya eşitleme aracısının sorunlarını gidermek için ve yeni işlevler eklemek için düzenli olarak güncelleştirilir. Kullanılabilir olarak Azure dosya eşitleme Aracısı güncelleştirmelerini almak için Microsoft Update'i yapılandırma öneririz.

#### <a name="major-vs-minor-agent-versions"></a>Büyük küçük Aracı sürümleri karşılaştırması
* Ana Aracı sürümleri, genellikle yeni özellikler içerir ve duymuşsunuzdur sürüm numarasının ilk parçası olarak sahip. Örneğin: \*2.\*.\*\*
* Küçük Aracı sürümleri "düzeltme" olarak da adlandırılır ve ana sürümler daha sık kullanıma sunulur. Bunlar genellikle hata düzeltmeleri ve geliştirmeleri daha küçük ancak herhangi bir yeni özellik içerir. Örneğin: \* \*.3.\*\*

#### <a name="upgrade-paths"></a>Yükseltme yolları
Dört Onaylandı ve test Azure dosya eşitleme Aracısı güncelleştirmelerini yüklemek için yollar vardır. 
1. **(Tercih edilir) Aracı güncelleştirmeleri otomatik olarak indirip Microsoft Update yapılandırın.**  
    Her zaman sunucu aracı için en son düzeltmeleri erişimi olduğundan emin olmak için her bir Azure dosya eşitleme güncelleştirmesi sürüyor öneririz. Microsoft Update bu işlem sorunsuz, otomatik olarak indirerek ve güncelleştirmeleri için yükleme sağlar.
2. **Aracı güncelleştirmeleri karşıdan yüklenip kurulacak AfsUpdater.exe kullanın.**  
    AfsUpdater.exe aracı yükleme dizininde bulunur. Aracı Güncelleştirmeleri indirmek ve yüklemek için yürütülebilir dosyaya çift tıklayın. 
3. **Mevcut bir Azure dosya eşitleme aracısının, bir Microsoft Update düzeltme eki dosyası veya yürütülebilir bir .msp kullanarak düzeltme eki. En son Azure dosya eşitleme güncelleştirme paketini indirilebileceğini [Microsoft Update Kataloğu](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Yürütülebilir bir .msp çalışan otomatik olarak önceki yükseltme yolu Microsoft Update tarafından kullanılan aynı yönteme ile Azure dosya eşitleme yüklemenizi yükseltir. Microsoft Update düzeltme eki uygulama, bir Azure dosya eşitleme yüklemesini yerinde yükseltme gerçekleştirin.
4. **En yeni Azure dosya eşitleme Aracısı Yükleyicisi'nden indirin [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257).**  
    Var olan bir Azure dosya eşitleme Aracısı yüklemesini yükseltmek için eski sürümü kaldırın ve ardından indirilen Yükleyicisi'nden en son sürümünü yükleyin. Sunucu kaydı, eşitleme grupları ve diğer ayarları Azure dosya eşitleme yükleyici tarafından korunur.

#### <a name="automatic-agent-lifecycle-management"></a>Otomatik aracı yaşam döngüsü yönetimi
Aracı sürümü 6 ile dosya eşitleme takım bir aracı otomatik yükseltme özelliğini kullanıma sundu. İki moddan birini seçebilir ve sunucuda yükseltme denenmesi bir bakım penceresi belirtin. Bu özellik, aracı yaşam döngüsü yönetimi ile ya da sona erme, Aracıdan engelleyen bir guardrail sağlayarak yardımcı olur ya da geçerli ayar kalın bir sorunsuz için izin verme için tasarlanmıştır.
1. **Varsayılan ayar** sona erme Aracıdan önleme girişiminde bulunur. Bir aracı gönderilen sona erme tarihini 21 gün içinde aracı kendi kendine yükseltme dener. Haftada bir kez süre sonundan önce ve seçili bakım penceresinde 21 gün içinde yükseltme girişimi başlayacak. **Bu seçeneği, normal Microsoft Update düzeltme ekleri alma ihtiyacını ortadan kaldırmaz.**
2. İsteğe bağlı olarak, yeni bir aracı sürümü kullanılabilir hemen sonra aracıyı otomatik olarak kendisini yükseltecek olduğunu seçebilirsiniz. Bu ayrıca seçili bakım penceresi sırasında oluşur ve genel kullanıma sunulduğunda hemen sonra yeni özellikler ve geliştirmeleri avantajlarından yararlanarak sunucuya izin. Normal güncelleştirme düzeltme ekleri sunucunuza yanı sıra büyük Aracı sürümleri sağlayacak, sorunsuz, Önerilen ayar budur.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Aracı yaşam döngüsü ve değişiklik Yönetimi garanti eder
Azure dosya eşitleme sürekli olarak yeni özellikler ve geliştirmeler sağlayan bir bulut hizmetidir. Bu, belirli bir Azure dosya eşitleme Aracısı sürümü yalnızca sınırlı bir süre için desteklenmesi anlamına gelir. Dağıtımınızı kolaylaştırmak için aşağıdaki kurallar yeterli süre ve aracı güncelleştirmeler/yükseltmeler değişiklik Yönetimi işleminizin uyum sağlamak için bildirim olması garanti:

- İlk yayın tarihinden itibaren en az altı ay için ana Aracı sürümleri desteklenir.
- En az üç aylık büyük Aracı sürümleri desteğini arasında bir çakışma var. garanti ediyoruz. 
- Uyarılar, süre sonundan önce en az üç ay yakında olacak süresi dolmuş Aracısı'nı kullanarak kayıtlı sunucular için verilir. Kayıtlı sunucu depolama eşitleme hizmeti kayıtlı sunucuları bölümü altında aracının eski bir sürümünü kullanıp kullanmadığını kontrol edebilirsiniz.
- Küçük aracı sürümü ömrünü ilişkili ana sürüme bağlıdır. Örneğin, Aracı sürüm 3.0 yayınlandığında, Aracı sürümleri 2. \* tüm birlikte süresi dolacak şekilde ayarlanır.

> [!Note]
> Bir süre sonu uyarısı ile bir aracı sürümü yüklemek bir uyarı görüntüler ancak başarılı. Yükleyin veya bir süresi dolmuş aracı sürümü ile bağlayın çalışılıyor desteklenmiyor ve engellenir.
