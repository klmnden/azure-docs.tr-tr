---
title: Bir Azure Backup Sunucusu'ndan veri kurtarma
description: Azure Backup bu kasaya kayıtlı tüm sunuculardan bir kurtarma Hizmetleri kasasına koruma uyguladım verileri kurtarın.
services: backup
author: kasinh
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: kasinh
ms.openlocfilehash: d1fb3434f0d3954a07980963866bcd7cce004379
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60650939"
---
# <a name="recover-data-from-azure-backup-server"></a>Azure Backup Sunucusu’ndan veri kurtarma
Azure Backup sunucusu, bir kurtarma Hizmetleri kasasına yedeklediğiniz verileri kurtarmak için kullanabilirsiniz. İşlem yapmak için bu nedenle Azure Backup sunucusu yönetim konsolunda oturum tümleşiktir ve diğer Azure Backup bileşenlerini kurtarma iş akışını benzer.

> [!NOTE]
> Bu makale için geçerli [System Center Data Protection Manager 2012 R2 UR7 veya sonraki sürümlerle](https://support.microsoft.com/en-us/kb/3065246), ile birleştirilmiş [en son Azure Backup aracısını](https://aka.ms/azurebackup_agent).
>
>

Bir Azure Backup Sunucusu'ndan veri kurtarma için:

1. Gelen **kurtarma** sekmesi Azure Backup sunucusu yönetim konsolunda, **'Dış DPM Ekle'** (sırasında ekranın sol üst köşesindeki).   
    ![Dış DPM Ekle](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Yeni indirme **kasa kimlik bilgileri** ilişkili kasadan **Azure Backup sunucusu** veri kurtarılmakta olan yerlerde, Azure yedekleme sunucuları listesi, Azure Backup Sunucusu'ndan kayıtlı seçin Kurtarma Hizmetleri kasası ve sağlayan **şifreleme parolası** verisini kurtarılıyor sunucuyla ilişkili.

    ![Dış DPM kimlik bilgileri](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Yalnızca Azure yedekleme aynı kayıt kasayla ilişkili sunucuları, diğer işlemelerin verileri kurtarabilir.
   >
   >

    Dış Azure Backup sunucusu başarıyla eklendikten sonra dış sunucunun ve yerel Azure Backup Sunucusu'ndan veri göz atabilirsiniz **kurtarma** sekmesi.
3. Dış Azure Backup sunucusu tarafından korunan üretim sunucularında kullanılabilir listesine göz atın ve uygun veri kaynağını seçin.

    ![Dış DPM sunucusuna göz atma](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Seçin **ay ve yıl** gelen **kurtarma noktaları** gerekli seçin, açılan menü **kurtarma tarih** zaman kurtarma noktasının oluşturulduğu ve seçin**Kurtarma zamanı**.

    Dosya ve klasörlerin listesini göz atıp herhangi bir yere kurtarılan alt bölmesinde görünür.

    ![Dış DPM sunucusuna kurtarma noktaları](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Uygun öğeyi sağ tıklayın ve tıklayın **kurtarmak**.

    ![Dış DPM kurtarma](./media/backup-azure-alternate-dpm-server/recover.png)
6. Gözden geçirme **kurtarma seçimi**. Veri ve kurtarılan yedek kopyasının süresi yanı sıra, yedekleme kopyası oluşturulduğu kaynak doğrulayın. Seçimi yanlış ise, tıklayın **iptal** uygun Kurtarma noktasını seçmek için geri kurtarma sekmesine gidin. Seçimi doğru ise, tıklayın **sonraki**.

    ![Dış DPM kurtarma özeti](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Seçin **alternatif bir konuma Kurtar**. **Gözat** doğru konuma kurtarma.

    ![Dış DPM kurtarma alternatif konumu](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. İlgili seçeneğini **kopya oluştur**, **atla**, veya **üzerine yaz**.

   * **Kopya Oluştur** -bir ad çakışması varsa, dosyanın bir kopyasını oluşturur.
   * **Skip** - bir ad çakışması varsa bırakır özgün dosya dosya kurtarılmıyor.
   * **Üzerine** - bir ad çakışması varsa dosyanın kopyasının üzerine yazar.

     Uygun bir seçeneği belirleyin **geri güvenlik**. Verilerin nerede kurtarılmakta olan hedef bilgisayarın güvenlik ayarlarını veya kurtarma noktası oluşturulduğu anda ürün için uygun güvenlik ayarlarını uygulayabilirsiniz.

     Tanımlamak olup olmadığını bir **bildirim** kurtarma başarıyla tamamlandıktan sonra gönderilir.

     ![Dış DPM kurtarma bildirimleri](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. **Özeti** ekran şimdiye seçilen seçenekleri listeler. ' A tıkladığınızda **'Kurtar'**, verilerin uygun şirket içi konuma kurtarılır.

    ![Dış DPM kurtarma seçenekleri özeti](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > Kurtarma işi izlenebilir **izleme** sekmesi Azure Backup sunucusu.
   >
   >

    ![Kurtarma izleme](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. Tıklayabilirsiniz **dış DPM'yi Temizle** üzerinde **kurtarma** DPM sunucusunun dış DPM sunucusunun görünümünü kaldırmak için sekmesinde.

    ![Dış DPM'i Temizle](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Hata iletileri sorunları giderme
| Hayır. | Hata İletisi | Sorun giderme adımları |
|:---:|:--- |:--- |
| 1. |Bu sunucu, kasa kimlik bilgileri tarafından belirtilen kasaya kayıtlı değil. |**Neden:** Seçili kasa kimlik bilgilerini kurtarma girişiminde Azure Backup sunucusu ile ilişkili kurtarma Hizmetleri kasasına ait değil, bu hata görüntülenir. <br> **Çözüm:** Kasa kimlik bilgilerini Azure Backup sunucusu için kayıtlı bir kurtarma Hizmetleri kasasından indirin. |
| 2. |Kurtarılabilir veriler kullanılamıyor veya seçili sunucu DPM sunucusu değil. |**Neden:** Başka bir Azure Backup sunucusu, Kurtarma Hizmetleri Kasası'na kayıtlı olan sunucuları henüz meta verilerini karşıya yüklenmedi veya seçili sunucu bir Azure Backup sunucusu (Windows Server veya Windows istemcisi olarak da bilinir) değil. <br> **Çözüm:** Varsa diğer Azure Backup sunucusu, Kurtarma Hizmetleri kasasına kayıtlı, en son Azure Backup aracısının yüklü olduğundan emin olun. <br>Varsa diğer Azure Backup sunucusu, Kurtarma Hizmetleri kasasına kayıtlı, kurtarma işlemini başlatmak için yükleme sonrasında bir gün bekleyin. Gecelik iş bulut korumalı tüm yedeklemeler için meta verileri yükler. Veri kurtarma için kullanılabilir. |
| 3. |Başka bir DPM sunucusu, bu kasaya kaydedilmiştir. |**Neden:** Hiçbir diğer Azure yedekleme, Kurtarma deneniyor kasaya kayıtlı sunucu yok.<br>**Çözüm:** Varsa diğer Azure Backup sunucusu, Kurtarma Hizmetleri kasasına kayıtlı, en son Azure Backup aracısının yüklü olduğundan emin olun.<br>Varsa diğer Azure Backup sunucusu, Kurtarma Hizmetleri kasasına kayıtlı, kurtarma işlemini başlatmak için yükleme sonrasında bir gün bekleyin. Gecelik iş bulut korumalı tüm yedeklemeler için meta verileri yükler. Veri kurtarma için kullanılabilir. |
| 4. |Sağlanan şifreleme parolası aşağıdaki sunucuyla ilişkili parolayla eşleşmiyor:  **\<sunucu adı >** |**Neden:** Sağlanan şifreleme parolası kurtarılmakta olan Azure Backup Sunucusu'nun veri veriler şifreleme işleminde kullanılan şifreleme parolası eşleşmiyor. Verilerin şifresini çözmek aracı silemiyor. Bu nedenle kurtarma başarısız olur.<br>**Çözüm:** Lütfen Azure yedekleme verilerini kurtarılmakta olan sunucu ile ilişkilendirilen aynı tam şifreleme parolası sağlayın. |

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Harici bir DPM sunucusu UR7 ve en son Azure Backup aracısını yükledikten sonra neden ekleyemiyorum?

UR7 ve en son Azure Backup aracısını başlatmak için yükledikten sonra en az bir gün beklemeniz gerekir (güncelleştirme paketi 7'den önceki, bir güncelleştirme paketini kullanarak) bulut için korunan veri kaynakları ile DPM sunucuları için **dış DPM Ekle sunucu**. Bir günlük dönem için DPM koruma gruplarının meta verileri Azure'a karşıya yüklemek için gereklidir. Koruma grubu meta verileri ilk kez gecelik işi aracılığıyla yüklenir.

### <a name="what-is-the-minimum-version-of-the-microsoft-azure-recovery-services-agent-needed"></a>Gereken Microsoft Azure kurtarma Hizmetleri Aracısı'nın en düşük sürümü nedir?

Microsoft Azure kurtarma Hizmetleri aracısı veya Azure Backup Aracısı, bu özelliği etkinleştirmek için gerekli en düşük sürümü 2.0.8719.0 ' dir.  Aracının sürümünü görüntülemek için: denetim masasını açın **>** tüm Denetim Masası öğeleri **>** programlar ve Özellikler **>** Microsoft Azure kurtarma Hizmetleri Aracısı. Sürüm 2.0.8719.0'den az ise, indirme ve yükleme [en son Azure Backup aracısını](https://go.microsoft.com/fwLink/?LinkID=288905).

![Dış DPM'i Temizle](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Sonraki adımlar:
• [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
