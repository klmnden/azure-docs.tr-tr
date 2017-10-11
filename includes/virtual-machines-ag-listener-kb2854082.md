Herhangi bir küme sunucusunda, Windows Server 2008 R2 veya Windows Server 2012 çalıştırıyorsanız, ardından, doğrulamanız gerekir düzeltme [KB2854082](http://support.microsoft.com/kb/2854082) her bir şirket içi sunucular veya kümenin parçası olan Azure VM'ler üzerinde yüklü. Ayrıca herhangi bir sunucu veya kümedeki ancak değil kullanılabilirlik grubundaki VM bu düzeltmenin yüklü olması gerekir.

Uzak Masaüstü oturumunda küme düğümlerinin her biri için indirme [KB2854082](http://support.microsoft.com/kb/2854082) yerel bir dizine. Ardından, düzeltmeyi her küme düğümünde sırayla yükleyin. Küme düğümünde Küme hizmeti şu anda çalışıyorsa, sunucu düzeltme yükleme sonunda yeniden başlatılır.

> [!WARNING]
> Küme hizmetini durdurma veya sunucunun yeniden başlatılması kümenizi ve kullanılabilirlik grubu çekirdek sistem durumu etkiler ve kümenizi çevrimdışı duruma neden olabilir. Yükleme sırasında kümenin yüksek kullanılabilirliği sürdürmek için emin olun:
> 
> * En uygun çekirdek sistem kümedir. 
> * Herhangi bir düğümde düzeltmeyi yüklemeden önce tüm küme düğümleri çevrimiçi değil.
> * Kümedeki herhangi bir düğümde düzeltmeyi yüklemeden önce tam olarak sunucunun yeniden başlatılması gibi bir düğümde tamamlanıncaya kadar çalışabilmesi düzeltme yükleme izin verin.
> 
> 

