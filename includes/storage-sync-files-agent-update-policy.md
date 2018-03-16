Azure dosya eşitleme Aracısı, yeni işlevler eklemek için düzenli olarak ve sorunları gidermek üzere güncelleştirilir. Kullanılabilir olarak Azure dosya eşitleme aracı için güncelleştirmeleri almak için Microsoft Update'e yapılandırma öneririz.

#### <a name="major-vs-minor-agent-versions"></a>Büyük küçük Aracı sürümleri karşılaştırması
* Ana Aracı sürümleri genellikle yeni özellikler içerir ve bir artan sürüm numarasını ilk parçası olarak sahiptir. Örneğin: *2.\*.\**
* İkincil Aracı sürümleri "düzeltme ekleri" olarak da adlandırılır ve ana sürüm daha sık serbest bırakılır. Bunlar genellikle hata düzeltmeleri ve daha küçük geliştirmeleri ancak hiçbir yeni özellikler içerir. Örneğin:  *\*.3.\**

#### <a name="upgrade-paths"></a>Yükseltme yolları
Üç onaylanmış ve test Azure dosya eşitleme aracı güncelleştirmeleri yüklemek için yol vardır. Bu, birincil ve ikincil sürümleri için yollar iş güncelleştirme.
1. **(Önerilen) Microsoft Update aracı güncelleştirmeleri otomatik olarak yükleyip yapılandırın.**  
    Her zaman en son düzeltmeler için sunucu Aracısı erişiminiz emin olmak için her bir Azure dosya eşitleme güncelleştirme alma öneririz. Microsoft Update bu işlem sorunsuz, otomatik olarak indirip güncelleştirmeleri sizin için yapar.
2. **Var olan bir Azure dosya eşitleme aracıyı bir Microsoft Update düzeltme eki dosyası veya yürütülebilir bir .msp kullanarak düzeltme eki. En son Azure dosya eşitleme güncelleştirme paketini yüklenebilir [Microsoft Update Kataloğu](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync).**  
    Yürütülebilir bir .msp çalıştıran önceki yükseltme yolu Microsoft Update tarafından otomatik olarak kullanılan yöntem ile Azure dosya eşitleme yüklemenizi yükseltir. Microsoft Update düzeltme eki uygulanırken bir Azure dosya eşitleme yüklemenin bir yerinde yükseltme işlemini gerçekleştirecek.
3. **Yeni Azure dosya eşitleme Aracısı Yükleyicisi'nden indirin [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Yükleyici yükleme, bir Microsoft Installer paketi veya yürütülebilir bir .msi içindir.**  
    Var olan Azure dosya eşitleme aracı yüklemesi yükseltmek için eski sürüm kaldırılır ve ardından indirilen Yükleyicisi'nden en son sürümünü yükleyin. Sunucu kaydı, eşitleme grubu ve diğer ayarları Azure dosya eşitleme yükleyicisi tarafından korunur.

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Aracı yaşam döngüsü ve değişiklik Yönetimi garanti altına alır.
Azure dosya eşitleme sürekli olarak giriş yeni özellikleri ve işlevleri sağlayan bir bulut hizmetidir. Bu, belirli bir Azure dosya eşitleme Aracısı sürümü yalnızca sınırlı bir süre için desteklenmeye anlamına gelir. Dağıtımınızı kolaylaştırmak için yeterli zaman ve aracı güncelleştirmeler/yükseltmeler değişiklik Yönetimi işleminizin uyum sağlamak için bildirim, güvence altına almak için aşağıdaki kuralları vardır:

- Ana Aracı sürümleri, en az altı ay boyunca ilk sürüm tarihten itibaren desteklenir.
- En az üç aylık ana Aracı sürümleri desteği arasında bir çakışma var. garanti ediyoruz. 
- Uyarılar, en az üç ay süre sonundan önce en kısa sürede olacak süresi dolan aracı kullanarak kayıtlı sunucuları için verilir. Bir kayıtlı sunucu Aracısı'nın eski bir sürümü depolama eşitleme hizmeti kayıtlı sunucuları bölümü altında kullanıp kullanmadığını kontrol edebilirsiniz.
- Bir ikincil Aracısı sürümü ömrü ilişkili ana sürümüne bağlıdır. Örneğin, Aracı sürüm 3.0 serbest bırakıldığında, Aracı sürümleri 2. \* tüm birlikte süresi dolacak şekilde ayarlanır.

> [!Note]
> Bir süre sonu uyarısı ile bir aracı sürümü yüklemek bir uyarı görüntüler ancak başarılı. Yükleme veya süresi dolmuş aracı sürümü ile bağlanma girişimi desteklenmez ve engellenir.