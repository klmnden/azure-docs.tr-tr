Saniye başına yapabileceğiniz sorgu sayısı (QPS), hizmete ve aboneliğinizin türüne göre belirlenir. Uygulamanızın, kotanızın sınırları içinde kalmak için gereken mantığı içerdiğinden emin olun. QPS sınırına ulaşılırsa veya bu sınır aşılırsa istek başarısız olur ve HTTP 429 durum kodu döndürülür. Yanıt, başka bir istek göndermeden önce beklemeniz gereken süreyi gösteren `Retry-After` üst bilgisini içerir.

## <a name="denial-of-service-versus-throttling"></a>Hizmet reddi ile azaltma karşılaştırması

Hizmette, hizmet reddi (DoS) saldırısıyla QPS ihlali arasında bir ayrım yapılır. Hizmet bir DoS saldırısı olduğundan şüphelenirse istek başarılıyla sonuçlanır (HTTP durum kodu 200 TAMAM). Ancak, yanıt gövdesi boş olur.