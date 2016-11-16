## <a name="about-records"></a>Kayıtları hakkında

Her DNS kaydında bir ad ve bir tür vardır. Kayıtlar, içerdikleri verilere göre çeşitli türlerde düzenlenmiştir. En yaygın tür olan "A" kaydı bir adı bir IPv4 adresiyle eşleştirir. Başka bir tür olan "MX" kaydıysa bir adı bir posta sunucusuyla eşleştirir.

Azure DNS; A, AAAA, CNAME, MX, NS, PTR, SOA, SRV ve TXT gibi tüm yaygın DNS kayıt türlerini destekler. Şunlara dikkat edin:

* SOA kaydı kümeleri her bölgeyle birlikte otomatik olarak oluşturulur ve ayrı ayrı oluşturulamaz.
* SPF kayıtlarının TXT kaydı türü kullanılarak oluşturulması gerekir. Daha fazla bilgi için [bu sayfaya](http://tools.ietf.org/html/rfc7208#section-3.1) bakın.

Azure DNS’de, kayıtlar göreli adlar kullanılarak belirtilir. "Tam" etki alanı adında (FQDN) bölge adı varken "göreli" adda bu yoktur. Örneğin, "contoso.com" bölgesindeki "www" göreli kayıt adı www.contoso.com tam kayıt adını verir.

## <a name="about-record-sets"></a>Kayıt kümeleri hakkında

Bazen, verilen ada ve türe sahip birden fazla DNS kaydı oluşturmanız gerekebilir. Örneğin, "www.contoso.com" web sitesinin iki farklı IP adresinde barındırıldığını var sayalım. Web sitesine, her IP adresi için bir tane olmak üzere iki farklı A kaydı gerekir. Bu bir kayıt kümesi örneğidir:

    www.contoso.com.        3600    IN    A    134.170.185.46
    www.contoso.com.        3600    IN    A    134.170.188.221

Azure DNS kayıt kümelerini kullanarak DNS kayıtlarını yönetir. Kayıt kümesi, bir bölgede yer alan aynı ada sahip ve aynı türde olan DNS kayıtlarının koleksiyonudur. Çoğu kayıt kümesinde tek bir kayıt olsa da, bunun gibi örneklerdeki bir kayıt kümesinde bir kayıttan fazlası vardır; ancak bunlar pek yaygın değildir.

SOA ve CNAME kaydı kümelerinin durumu özeldir. DNS standartları bu türlerde aynı ada sahip birden çok kayda izin vermez.

Yaşam süresi veya TTL, yeniden sorgulanmadan önce istemci tarafından ne süreyle önbelleğe alınacağını belirtir. Bu örnekte, TTL 3600 saniye veya 1 saattir. TTL her kayıt için değil, kayıt kümesi için belirtilir; bu nedenle, bu kayıt kümesindeki tüm kayıtlar için aynı değer kullanılır.

#### <a name="wildcard-record-sets"></a>Joker karakter kaydı kümeleri

Azure DNS [joker kayıtlarını](https://en.wikipedia.org/wiki/Wildcard_DNS_record) destekler. Sorgulara yanıt olarak eşleşen bir adla döndürülürler (joker karakter olmayan bir kayıt kümesine ait daha yakın bir eşleşme olmadıkça). Joker karakter kaydı kümeleri NS ve SOA dışında tüm kayıt türleri tarafından desteklenir.

Joker karakter kaydı kümesi oluşturmak için "\*" kayıt kümesi adını kullanın. Bunun yerine, "\*" etiketine sahip bir ad da kullanabilirsiniz; örneğin,"\*.foo".

#### <a name="cname-record-sets"></a>CNAME kayıt kümeleri

CNAME kaydı kümeleri aynı ada sahip diğer kayıt kümeleriyle birlikte olamaz. Örneğin, "www" göreli adına sahip bir CNAME kaydı kümesini ve "www" göreli adına sahip bir A kaydını aynı anda oluşturamazsınız. Bölge tepesinde (ad = ‘@’) her zaman, bölge oluşturulduğunda oluşturulan NS ve SOA kaydı kümeleri bulunduğundan bölge tepesinde CNAME kaydı kümesi oluşturamazsınız. Bu kısıtlamalar DNS standartları kaynaklıdır, Azure DNS sınırlamaları değildir.


<!--HONumber=Nov16_HO2-->


