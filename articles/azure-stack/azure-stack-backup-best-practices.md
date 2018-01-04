---
title: "Altyapı Backup hizmeti en iyi uygulamalar Azure yığınının | Microsoft Docs"
description: "Azure yığın yıkıcı bir hata varsa veri kaybı azaltmaya yardımcı olmak için veri merkezinizde yönetmek ve dağıttığınızda en iyi yöntemler kümesi izleyebilirsiniz."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2017
ms.author: mabrigg
ms.openlocfilehash: b9438f3bab92f40a5c79ce7b7a572195c182be45
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="infrastructure-backup-service-best-practices"></a>Altyapı Backup hizmeti en iyi uygulamalar

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Yıkıcı bir hata durumunda veri kaybını azaltmaya yardımcı olmak için veri merkezinizdeki Azure yığın yönetmek ve dağıttığınızda en iyi uygulamaları takip edebilirsiniz.

İşlemi akış değişiklik yapıldığında yüklemenizi hala uyumlu olup olmadığını doğrulamak için düzenli aralıklarla en iyi uygulamaları gözden geçirmelidir. Bu en iyi yöntemleri uygulayarak sırasında sorunları karşılaşmamanız gerekir, Yardım için Microsoft Support başvurun.

## <a name="configuration-best-practices"></a>Yapılandırma en iyi uygulamalar

### <a name="deployment"></a>Dağıtım

Her Azure yığın bulut dağıtımdan sonra altyapı yedeklemeyi etkinleştirin. Tüm istemci/sunucu erişimi olan bir yedeklemelerden işleci yönetim API uç zamanlayabilirsiniz AzureStack araçlarını kullanma.

### <a name="networking"></a>Ağ

Yol için Evrensel Adlandırma Kuralı (UNC) dize bir tam etki alanı adı (FQDN) kullanmanız gerekir. IP adresi, ad çözümlemesi mümkün değilse mümkündür. Bir UNC dize paylaşılan dosyaları veya aygıt konumunu belirtir.

### <a name="encryption"></a>Şifreleme

Şifreleme anahtarı, dış depolama birimine dışarı yedekleme verilerini şifrelemek için kullanılır. Anahtar AzureStack araçlarla oluşturulabilir. 

![AzureStack araçları](media\azure-stack-backup\azure-stack-backup-encryption1.png)

Anahtar (örneğin, ortak Azure anahtar kasası gizli) güvenli bir konumda depolanması gerekir. Bu anahtar Azure yığını yeniden dağıtım sırasında kullanılması gerekir. 

![Anahtarı güvenli bir konuma depolanır.](media\azure-stack-backup\azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>İşletimsel en iyi uygulamalar

### <a name="backups"></a>Yedeklemeler

 - Altyapı yedekleme denetleyicisi isteğe bağlı olarak tetiklenmesi gerekiyor. Günde en az iki kez yedekleme önerilir.
 - Bu yüzden kapalı kalma süresi management deneyimleri ya da kullanıcı uygulamaları sistem çalışırken yedekleme işleri yürütün. Yedekleme işleri makul bir yük altında bir çözüm için 20-40 dakika bekler.
 - Yönerge sağlanan OEM kullanarak, el ile yedekleme ağ anahtarları ve donanım yaşam döngüsü ana bilgisayar (HLH) burada altyapı yedek denetleyicisi depoları denetim düzlemine yedekleme verileri aynı yedekleme paylaşımında depolanması gerekir. Anahtar ve HLH yapılandırmaları bölge klasöre depolamayı düşünün. Aynı bölgede birden çok Azure yığın örneği varsa, bir ölçek birimi ait her yapılandırma için bir tanımlayıcı kullanmayı düşünün.

### <a name="folder-names"></a>Klasör adları

 - Altyapı MASBACKUP klasörü otomatik olarak oluşturur. Microsoft tarafından yönetilen bir paylaşım budur. MASBACKUP ile aynı düzeyde paylaşımları oluşturabilirsiniz. Klasör veya Azure yığın oluşturmaz MASBACKUP içinde depolama veri oluşturma önerilmez. 
 -  Kullanıcı FQDN ve klasör adını bölgesindeki farklı Bulutlar gelen yedekleme verilerinin ayırt etmek için. Azure yığın dağıtımı ve uç noktaları tam etki alanı adını (FQDN) bölge parametresi ve dış etki alanı adı parametresi birleşimidir. Daha fazla bilgi için bkz: [Azure yığın datacenter tümleştirmesi - DNS](azure-stack-integrate-dns.md).

Örneğin, yedekleme fileserver01.contoso.com üzerinde barındırılan AzSBackups paylaşımıdır. Bu dosya paylaşımına dış etki alanı adı ve bölge adını kullanan bir alt kullanarak Azure yığın dağıtım başına bir klasör olabilir. 

FQDN: contoso.com  
Bölge: nyc


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

MASBackup Azure yığın yedekleme verilerini depoladığı klasörüdür. Bu klasör, kendi verilerinizi depolamak için kullanmamalısınız. OEM ya tüm yedekleme verilerini depolamak için bu klasörün kullanmamalısınız. 

OEM'ler bölge klasörü altında bileşenleri için yedek verileri depolamak için önerilir. Her ağ anahtarları, donanım yaşam döngüsü ana bilgisayar (HLH) ve benzeri kendi alt klasöründe depolanabilir. Örneğin:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>İzleme

Aşağıdaki uyarıları, sistem tarafından desteklenir:

| Uyarı                                                   | Açıklama                                                                                     | Düzeltme                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Dosya Paylaşımı kapasitesi yetersiz olduğundan yedekleme başarısız oldu | Dosya Paylaşımı kapasitesi dışında yedekleme denetleyicisi yedekleme dosyalarının konumuna veremezsiniz. | Daha fazla depolama kapasitesi ekleyin ve yedekleme yeniden deneyin. Alan boşaltmak için var olan yedekleri (eskisinden ilk başlayarak) silin.                    |
| Yedekleme bağlantı sorunları nedeniyle başarısız oldu.             | Ağ Azure yığın arasında dosya paylaşımı sorunları yaşıyor.                          | Ağ sorunu gidermek ve yedekleme işlemini yeniden deneyin.                                                                                            |
| Yolda bir hata nedeniyle yedekleme başarısız oldu                | Dosya Paylaşımı yolu çözümlenemiyor                                                          | Paylaşım erişilebilir olduğundan emin olmak için farklı bir bilgisayara paylaşımından eşleyin. Artık geçerli değilse yolu güncelleştirmeniz gerekebilir.       |
| Kimlik doğrulama sorunu nedeniyle yedekleme başarısız oldu               | Kimlik bilgileri ile ilgili bir sorun veya kimlik doğrulama etkiler bir ağ sorunu olabilir.    | Paylaşım erişilebilir olduğundan emin olmak için farklı bir bilgisayara paylaşımından eşleyin. Artık geçerliyse kimlik bilgilerini güncelleştirmeniz gerekebilir. |
| Genel bir hata nedeniyle yedekleme başarısız oldu                    | Başarısız istek aralıklı bir sorun nedeniyle olabilir. Yedekleme yeniden deneyin.                    | destek çağrısı                                                                                                                               |

## <a name="next-steps"></a>Sonraki adımlar

 - İçin başvuru materyallerini gözden [Yedekleme hizmet altyapı](azure-stack-backup-reference.md).  
 - Etkinleştirme [altyapı yedekleme hizmeti](azure-stack-backup-enable-backup-console.md).