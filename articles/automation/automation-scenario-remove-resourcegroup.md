<properties
    pageTitle="Kaynak Gruplarını otomatik kaldırma | Microsoft Azure"
    description="Bir Azure Otomasyonu senaryosunun, aboneliğinizdeki tüm Kaynak Gruplarını kaldırmaya yönelik runbook’ları içeren PowerShell İş Akışı sürümü."
    services="automation"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="magoedte"/>


# Azure Otomasyonu senaryosu - kaynak gruplarının kaldırılmasını otomatik hale getirme

Çok sayıda müşteri, bazıları üretim uygulamalarını yönetmeye ayrılırken bazıları geliştirme, test ve hazırlık ortamları için olan birden fazla kaynak grubu oluşturmaktadır. Bu kaynakların dağıtımının otomatik hale getirilmesi bir özelliktir, ancak bir kaynak grubunun tek bir düğme tıklanarak kullanımdan kaldırılması da başka bir özelliktir.  Bunu işlemek için Otomasyon hizmetinin kullanılması mükemmel bir kullanım örneği ve böyle bir ortak yönetim görevini kolaylaştıran bir fırsattır. Bu özellik ayrıca MSDN veya Microsoft İş Ortağı Ağı Bulut Temel Bileşenleri programı gibi bir üye teklifi üzerinden harcama limitine sahip bir Azure aboneliği ile çalışıyorsanız yararlı olur. 

Bu senaryo bir PowerShell runbook'u temel alır ve aboneliğinizden belirttiğiniz bir veya daha fazla kaynak grubunu kaldırmak için tasarlanmıştır.  Runbook devam etmeden önce varsayılan olarak test etmeyi destekler.  Bu şekilde, bu yordamı tamamlamaya hazır olduğunuzdan mutlaka emin olmadan kaynak grubunu yanlışlıkla silmezsiniz.   

## Senaryoyu alma

Bu senaryo [PowerShell Galerisi](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript)’nden indirebileceğiniz veya doğrudan Azure portalındaki [Runbook Galerisi](automation-runbook-gallery.md)’nden içe aktarabileceğiniz bir PowerShell runbook’tan oluşur.<br><br> 

Runbook | Açıklama|
----------|------------|
Remove-ResourceGroup | Bir veya daha fazla Azure kaynak grubunu ve onun kaynaklarını abonelikten kaldırır.  
<br>
Bu runbook için aşağıdaki giriş parametreleri tanımlanmıştır:

Parametre | Açıklama|
----------|------------|
NameFilter (Gerekli) | Silmeyi amaçladığınız kaynak gruplarını sınırlandırmak için bir ad filtresi belirtmenizi sağlar. Virgülle ayrılmış bir liste kullanarak birden çok değer geçirebilirsiniz.<br>Filtre büyük küçük harfe duyarlı değildir ve dizeyi içeren herhangi bir kaynak grubunu eşleştirir.|
PreviewMode (İsteğe bağlı) | Hangi kaynak gruplarının silineceğini görmek ve herhangi bir işlem yapmamak için runbook’u yürütün.<br>Varsayılan değer, runbook’a geçirilen bir veya daha fazla kaynak grubunun yanlışlıkla silinmesini önlemeye yardımcı olmak üzere **true** olarak ayarlanmıştır.  

## Bu senaryoyu yükleme ve yapılandırma

### Ön koşullar

Bu runbook [Azure Farklı Çalıştır hesabı](automation-sec-configure-azure-runas-account.md) kullanarak kimlik doğrulaması yapar.    

### Runbook yükleme ve yayımlama

Runbook’u indirdikten sonra [Runbook’ları içeri aktarma yordamları](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-Azure-Automation) içindeki yordamı kullanarak içeri aktarabilirsiniz.  Runbook’u Otomasyon hesabınıza başarıyla içeri aktarıldıktan sonra yayımlayın.


## Runbook’u kullanma

Aşağıdaki adımlar bu runbook’un yürütülmesinde size yol gösterir ve nasıl çalıştığını anlamanıza yardımcı olur.  Bu örnekte yalnızca runbook test edilecek, kaynak grubu gerçekten silinmeyecektir.  

1. Azure Portal’da Otomasyon hesabınızı açın ve  **Runbook'lar** kutucuğuna tıklayın.
2. **Remove-ResourceGroup** runbook’unu seçin ve **Başlat**’a tıklayın.
3. Runbook’u başlattığınızda **Runbook’u Başlat** dikey penceresi açılır ve parametrelerin aşağıdaki değerlerini yapılandırabilirsiniz.  Aboneliğinizde test etmek istediğiniz ve yanlışlıkla silinirse zararı olmayacak bir veya daha faza kaynak grubunun adını girin.<br> ![Remove-ResouceGroup Parametreleri](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)
    
    >[AZURE.NOTE] Seçili kaynak gruplarının yanlışlıkla silinmesini önlemek için **Previewmode** seçeneğinin **true** olarak ayarlandığından emin olun.  Bu runbook’un bu runbook’u çalıştıran Otomasyon hesabını içeren kaynak grubunu kaldırmayacağını **unutmayın**.  

4. Tüm parametre değerlerini yapılandırdıktan sonra **Tamam**’a tıkladığınızda runbook yürütme kuyruğuna alınır.  

**Remove-ResourceGroup** runbook işinin ayrıntılarını Azure portalında görüntülemek için runbook’un **İşler** kutucuğunu seçin. İş özetinde giriş parametreleri ve çıkış akışına ek olarak işe ilişkin genel bilgiler ve gerçekleşmişse özel durumlar gösterilir.<br> ![Remove-ResourceGroup Runbook İş Durumu](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

**İş Özeti** bölümünde çıkış, uyarı ve hata akışlarından iletiler bulunur. Runbook yürütmeyle ilgili ayrıntılı sonuçları görüntülemek için **Çıkış** kutucuğunu seçin.<br> ![Remove-ResourceGroup Runbook Çıktı Sonuçları](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png) 

## Sonraki adımlar

- Kendi runbook’unuzu oluşturmaya başlamak için bkz. [Azure Otomasyonu’nda runbook oluşturma veya içeri aktarma](automation-creating-importing-runbook.md)
- PowerShell İş Akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell İş Akışı runbook uygulamam](automation-first-runbook-textual.md)


<!--HONumber=Sep16_HO4-->


