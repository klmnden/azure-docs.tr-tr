Saniye başına yapabileceğiniz sorgu sayısı (QPS), hizmet ve aboneliğinizin türüne göre belirlenir. Uygulamanızın, kotanızın sınırları içinde kalmak için gereken mantığı içerdiğinden emin olmanız gerekir. QPS sınırınızı aşarsanız istek HTTP durum kodu 429 ile başarısız olur. Ayrıca yanıt, başka bir istek göndermeden önce kaç saniye beklemeniz gerektiğini belirten Retry-After üst bilgisini de içerir.  
  
### <a name="denial-of-service-dos-versus-throttling"></a>Hizmet Reddi (DOS) ve Azaltma karşılaştırması

Hizmet, bir DOS saldırısı ile QPS ihlali arasındaki farkı ayırt eder. Hizmet bir hizmet reddi saldırısı olduğundan şüphelenirse, istek başarılıyla sonuçlanır (HTTP durum kodu 200 TAMAM) ancak yanıt gövdesi boş olur.