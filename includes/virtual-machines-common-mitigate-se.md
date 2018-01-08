
**Son Güncelleştirme'ı belge**: 6 Ocak 18:30:00 PST.

Son açıklanması bir [CPU güvenlik açıklarının yeni sınıf](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) kurgusal yürütme yan kanal saldırıları olarak bilinen daha fazla netlik aramayı müşterilerden soruları sonuçlandı.  

Azure çalışan ve müşteri iş yüklerinin birbirinden ayırır altyapı korunur.  Başka bir deyişle, Azure üzerinde çalışan diğer müşteriler güvenlik açıklarından kullanarak uygulamanızı saldırı olamaz.

## <a name="keeping-your-operating-systems-up-to-date"></a>İşletim sistemlerinin güncel tutma

Bir işletim sistemi güncelleştirme uygulamalarınızı Azure üzerinde çalışan diğer müşterilerden Azure üzerinde çalışan yalıtmak için gerekli olmasa da, bu her zaman işletim sistemi sürümü güncel tutmak üzere en iyi uygulamadır. 

Aşağıdaki teklifleri işletim sisteminizi güncelleştirmeniz için bizim önerilen eylemler şunlardır: 

<table>
<tr>
<th>Teklifi</th> <th>Önerilen Eylem </th>
</tr>
<tr>
<td>Azure Cloud Services </td>  <td>Otomatik güncelleştirmeyi etkinleştirme veya yeni konuk işletim sistemi çalıştırdığınızdan emin olun.</td>
</tr>
<tr>
<td>Azure Linux sanal makineleri</td> <td>Güncelleştirmeler kullanılabilir olduğunda işletim sistemi sağlayıcınızdan yükleyin. </td>
</tr>
<tr>
<td>Azure Windows sanal makineler </td> <td><ul><li>İşletim sistemi güncelleştirmelerini yüklemeden önce desteklenen bir virüsten koruma uygulaması çalıştığını doğrulayın. Uyumluluk bilgileri için virüsten koruma yazılımı satıcınıza başvurun. </li> <li> Yükleme [Ocak güvenlik dökümü](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </li></ul></td>
</tr>
<tr>
<td>Diğer Azure PaaS Hizmetleri</td> <td>Bu hizmetleri kullanan müşteriler için gereken eylemi yok. Azure, işletim sistemi sürümlerini otomatik olarak güncel tutar. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Güvenilmeyen kod çalıştırıyorsanız, ek yönergeler 

Güvenilmeyen kod çalıştırmadığınız sürece ek müşteri Eylem gerekmiyor. Güvenmediğiniz kodu izin verirseniz (örneğin, ikili veya uygulamanızdaki sonra bulutta yürütülen kod parçacığını karşıya müşterilerinize birini izin), sonra da aşağıdaki ek adımları yapılması gerekir.  


### <a name="windows"></a>Windows 
Windows kullanıyorsanız ve güvenilmeyen kod barındırma kurgusal yürütme yan kanal güvenlik açıklarına karşı ek koruma sağlayan çekirdek sanal adres (KVA) gölgeleme adlı Windows özelliği etkinleştirmeniz gerekir. Bu özellik varsayılan olarak kapalıdır ve etkinleştirilirse performansı etkileyebilir. İzleyin [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) sunucu üzerindeki etkinleştirme korumaları için yönergeler. Azure Cloud Services çalıştırıyorsanız, WA çalıştırdığınızı doğrulayın-KONUK-OS-5.15_201801-01 veya WA-GUEST-OS-4.50_201801-01 (kullanılabilir 10 Ocak başlayarak) ve etkin kayıt defteri anahtarı bir başlangıç görevi.


### <a name="linux"></a>Linux
Linux kullanıyorsanız ve güvenilmeyen kod barındırma kullanıcı alanına ait olanlar çekirdekten tarafından kullanılan belleği tabloları ayıran çekirdek sayfa tablosu yalıtım (KPTI) uygulayan daha yeni bir sürüme ayrıca Linux güncelleştirmeniz gerekir. Bu Azaltıcı Etkenler Linux işletim sistemi güncelleştirme gerektirir ve kullanılabilir olduğunda dağıtım sağlayıcınızdan elde edilebilir. İşletim sistemi sağlayıcınız korumaları etkin veya varsayılan olarak devre dışı olup olmadığını söyleyebilir.








## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [CPU güvenlik açığı güvenliğini sağlama Azure müşterilerden](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).