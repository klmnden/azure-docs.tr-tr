---
title: Bir Azure yedekleme sunucusundan veri kurtarma
description: Herhangi bir sunucudan Azure yedekleme bu kasaya kayıtlı bir kurtarma Hizmetleri kasası yazdıramadığının verileri kurtarın.
services: backup
author: nkolli1
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: adigan
ms.openlocfilehash: 8559532f873e8073e736f881374fec1c080d08c3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604412"
---
# <a name="recover-data-from-azure-backup-server"></a>Azure Backup Sunucusu’ndan veri kurtarma
Azure yedekleme sunucusu, bir kurtarma Hizmetleri kasası yedeklediğiniz verileri kurtarmak için kullanabilirsiniz. İşlem yapmak için bu nedenle Azure yedekleme sunucusu Yönetim Konsolu'na tümleştirilmiştir ve diğer Azure Backup bileşenleri için kurtarma iş akışı benzer.

> [!NOTE]
> Bu makalede [System Center Data Protection Manager 2012 R2 için UR7 veya sonraki sürümleriyle] geçerlidir (https://support.microsoft.com/en-us/kb/3065246), ile birleştirilmiş [en son Azure Backup aracısını](http://aka.ms/azurebackup_agent).
>
>

Bir Azure yedekleme sunucusundan verileri kurtarmak için:

1. Gelen **kurtarma** sekmesini Azure yedekleme sunucusu Yönetim Konsolu, tıklatın **'Dış DPM Ekle'** (en üst ekranın sol).   
    ![Dış DPM Ekle](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Yeni Yükleme **kasa kimlik bilgileri** ile ilişkili kasasından **Azure yedekleme sunucusu** veri kurtarılmakta olan yerlerde, Azure yedekleme sunucusu Azure yedekleme sunucuları listesinden Kurtarma Hizmetleri Kasayla birlikte kaydedilen seçin ve sağlamak **şifreleme parolası** verisini kurtarıldığı sunucuyla ilişkili.

    ![Dış DPM kimlik bilgileri](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Yalnızca Azure yedekleme sunucuları aynı kayıt kasayla ilişkili birbirlerinin verileri kurtarabilir.
   >
   >

    Dış Azure yedekleme sunucusu başarıyla eklendikten sonra dış sunucu ve yerel Azure yedekleme sunucusundan veri göz atabilirsiniz **kurtarma** sekmesi.
3. Dış Azure yedekleme sunucusu tarafından korunan üretim sunucularında kullanılabilir listesini bulun ve uygun veri kaynağını seçin.

    ![Dış DPM sunucusu Gözat](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Seçin **ay ve yıl** gelen **kurtarma noktaları** açılan, gerekli seçin **kurtarma tarih** kurtarma noktasının oluşturulduğu için ve select **kurtarma süresini**.

    Dosya ve klasörlerin listesini taranan ve herhangi bir yere kurtarılan alt bölmesinde görüntülenir.

    ![Dış DPM sunucusunu kurtarma noktaları](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Uygun öğeyi sağ tıklatın ve'ı tıklatın **kurtarmak**.

    ![Dış DPM kurtarma](./media/backup-azure-alternate-dpm-server/recover.png)
6. Gözden geçirme **kurtarmak seçimi**. Veri ve kurtarılan yedekleme kopyasının süresi yanı sıra, yedek kopyayı oluşturulduğu kaynak doğrulayın. Seçimi yanlışsa tıklatın **iptal** uygun Kurtarma noktasını seçmek için geri kurtarma sekmesine gidin. Seçimi doğruysa **sonraki**.

    ![Dış DPM kurtarma özeti](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Seçin **alternatif bir konuma Kurtar**. **Gözat** doğru konuma kurtarma.

    ![Dış DPM kurtarma alternatif konumu](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. İlgili seçeneği **kopya oluştur**, **atla**, veya **üzerine yaz**.

   * **Kopya Oluştur** -bir ad çakışması varsa, dosyanın bir kopyasını oluşturur.
   * **Atla** - bir ad çakışması varsa özgün dosya bırakır dosyasını kurtarmak değil.
   * **Üzerine** - bir ad çakışması varsa dosya kopyasının üzerine yazar.

     Uygun seçeneği belirtin **geri güvenlik**. Verilerin nerede kurtarılacak hedef bilgisayarın güvenlik ayarlarını veya kurtarma noktası oluşturulduğunda ürün için uygun güvenlik ayarlarını uygulayabilirsiniz.

     Tanımlamak olup bir **bildirim** kurtarma başarıyla tamamlandıktan sonra gönderilir.

     ![Dış DPM kurtarma bildirimleri](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. **Özet** ekran kadarki seçilen seçenekleri listeler. Tıkladığınızda **'Kurtar'**, verileri içi uygun konuma kurtarılır.

    ![Dış DPM kurtarma seçenekleri özeti](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > Kurtarma işi izlenebilir **izleme** Azure yedekleme sunucusu sekmesinde.
   >
   >

    ![Kurtarma izleme](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. Tıklayabilirsiniz **dış DPM Temizle** üzerinde **kurtarma** dış DPM sunucusu görünümünü kaldırmak için DPM sunucusunun sekmesi.

    ![Dış DPM Temizle](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Hata iletileri sorunlarını giderme
| Hayır. | Hata İletisi | Sorun giderme adımları |
|:---:|:--- |:--- |
| 1. |Bu sunucu, kasa kimlik bilgileri tarafından belirtilen kasaya kayıtlı değil. |**Neden:** seçilen kasa kimlik bilgilerini Azure yedekleme kurtarma denemesi sunucuyla ilişkili kurtarma Hizmetleri Kasası'na ait olmadığından, bu hata görünür. <br> **Çözüm:** indirme için kasa kimlik bilgilerini kurtarma Hizmetleri kasası Azure yedekleme sunucusu kayıtlı. |
| 2. |Kurtarılabilir veriler kullanılamıyor veya seçili sunucu DPM sunucusu değil. |**Neden:** vardır kayıtlı kurtarma Hizmetleri Kasası'na herhangi bir Azure yedekleme sunucusu veya sunucuları henüz meta veriler karşıya yüklenmedi veya seçili sunucu bir Azure yedekleme sunucusu (diğer adıyla Windows Server veya Windows istemcisi) değil. <br> **Çözüm:** varsa Kurtarma Hizmetleri Kasası'na kayıtlı diğer Azure yedekleme sunucusu, en son Azure Backup aracısının yüklü olduğundan emin olun. <br>Varsa kayıtlı kurtarma Hizmetleri Kasası'na diğer Azure yedekleme sunucusu, kurtarma işlemini başlatmak için yüklemeden sonra bir gün bekleyin. Gecelik iş buluta korunan tüm yedeklemeler için meta veriler karşıya yükler. Veri kurtarma için kullanılabilir. |
| 3. |Başka bir DPM sunucusu bu kasaya kaydedilmiştir. |**Neden:** diğer Azure yedekleme, Kurtarma denemesi kasaya kayıtlı sunucu yok.<br>**Çözüm:** varsa Kurtarma Hizmetleri Kasası'na kayıtlı diğer Azure yedekleme sunucusu, en son Azure Backup aracısının yüklü olduğundan emin olun.<br>Varsa kayıtlı kurtarma Hizmetleri Kasası'na diğer Azure yedekleme sunucusu, kurtarma işlemini başlatmak için yüklemeden sonra bir gün bekleyin. Gecelik iş bulut tüm korumalı yedeklemeler için meta veriler karşıya yükleme. Veri kurtarma için kullanılabilir. |
| 4. |Sağlanan şifreleme parolası şu sunucuyla ilişkili parolayla eşleşmiyor: **<server name>** |**Neden:** kurtarılmakta olan Azure yedekleme sunucunun veri verilerden şifreleme işleminde kullanılan şifreleme parolası sağlanan şifreleme parolası eşleşmiyor. Aracısı, verileri şifrelemek alamıyor. Bu nedenle kurtarma başarısız olur.<br>**Çözüm:** Lütfen Azure yedekleme verileri kurtarıldığı sunucu ile ilişkilendirilen tam aynı şifreleme parolası belirtin. |

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Harici bir DPM sunucusu UR7 ve en son Azure Backup aracısını yükledikten sonra neden ekleyemiyorum?

Buluta (güncelleştirme paketi 7'den önceki, bir güncelleştirme paketi kullanılarak) korunan veri kaynakları ile DPM sunucusu için UR7 ve en son Azure Backup aracısını başlatmaya yükledikten sonra en az bir gün beklemeniz gerekir **dış DPM Ekle sunucu**. Bir günlük süre için DPM koruma gruplarının meta verileri Azure'a yüklemeniz için gereklidir. Koruma grubu meta veri gecelik işi üzerinden ilk kez yüklenir.

### <a name="what-is-the-minimum-version-of-the-microsoft-azure-recovery-services-agent-needed"></a>Gerekli Microsoft Azure kurtarma Hizmetleri Aracısı'nın en düşük sürümü nedir?

Microsoft Azure kurtarma Hizmetleri Aracısı ya da Azure Yedekleme aracısı, bu özelliği etkinleştirmek için gerekli en düşük sürümü 2.0.8719.0 ' dir.  Aracının sürümünü görüntülemek için: Denetim Masası'nı açın **>** tüm Denetim Masası öğeleri **>** programlar ve Özellikler **>** Microsoft Azure kurtarma Hizmetleri Aracısı. Küçüktür 2.0.8719.0 sürümse yükleyip [en son Azure Backup aracısını](https://go.microsoft.com/fwLink/?LinkID=288905).

![Dış DPM Temizle](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Sonraki adımlar:
• [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
