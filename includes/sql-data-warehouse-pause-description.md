
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
Maliyet tasarrufu sağlamak duraklatma ve sürdürme işlem kaynakları isteğe bağlı. Örneğin, veritabanı gece sırasında ve hafta sonları kullandığınız olmaz, bu saatlerde duraklatma ve gün boyunca sürdürün. Veritabanı duraklatıldığında Dwu için sizden ücret olmaz.

Ne zaman bir veritabanı duraklatılamadı:

* İşlem ve bellek kaynakları veri merkezindeki kullanılabilir kaynak havuzu döndürülürsünüz
* DWU maliyetleri duraklatma süresi için sıfırdır.
* Veri depolama etkilenmez ve verileriniz olduğu gibi kalır. 
* SQL veri ambarı çalışan ya da sıraya alınan tüm işlemleri iptal eder.

