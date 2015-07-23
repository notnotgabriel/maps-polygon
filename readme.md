# Objetivo: Mostrar como usar google maps polygon

Links:
[Post no blog da Princi Web](http://www.princiweb.com.br/blog/programacao/google-apis/google-maps-polygon.html)

Salve galera!

Hoje ensinarei a vocês como desenhar uma área poligonal em seu mapa, e de forma simples, mostrar se o usuário está dentro desta área delimitada.

A API do google maps V3 traz bastante novidades. Dentre elas estão os Shapes, formas que você consegue demarcar em seu mapa.

https://developers.google.com/maps/documentation/javascript/shapes

Para iniciar, vamos criar um mapa simples:

```
  function initialize() {
    var mapOptions = {
    zoom: 18,

    center: new google.maps.LatLng(-22.879125525135013, -47.05825782513126),
      mapTypeId: google.maps.MapTypeId.ROADMAP
    };
    var map = new google.maps.Map(document.getElementById('map'), mapOptions);
  }

  google.maps.event.addDomListener(window, 'load', initialize);
```

Não esqueça de chamar o script do google maps em seu html

```
<script src="https://maps.googleapis.com/maps/api/js?v=3.exp&signed_in=true&libraries=geometry"></script>
```

Note que estamos incluindo na chamada o parâmetro libraries=geometry. Ele vai carregar algumas funções úteis para trabalharmos com os shapes.

Vamos declarar um array com as coordenadas que usaremos para desenhar nosso shape.
Como um polígono deve ter 3 ou mais ângulos, salvei 3 coordenadas que circundam o nosso endereço.

```
var princiCoords = [
  new google.maps.LatLng(-22.878735,-47.058499),
  new google.maps.LatLng(-22.879694,-47.058392),
  new google.maps.LatLng(-22.878686,-47.057598)
];
```

Usaremos o objeto Polygon para demarcar a área, e para declará-lo basta fazer:

```
var princiPolygon = new google.maps.Polygon({
  // Coordenadas do seu objeto
  paths: princiCoords,
  // Cor do da linha
  strokeColor: '#E12D93',
  // Opacidade da linha
  strokeOpacity: 0.8,
  // Espessura da linha
  strokeWeight: 4,
  // Cor de preenchimento do objeto
  fillColor: '#942E7C',
  // Opacidade do preenchimento
  fillOpacity: 0.35
});
```

Dentro da função initialize, devemos inserir o shape em nosso mapa usando:

```
princiPolygon.setMap(map);
```

Prontinho, já temos um objeto Shape desenhado em nosso mapa.

Vamos criar uma função simples, que vai somente carregar novas coordenadas, simulando uma troca de endereço.
Criaremos também um botão para chamar esta função.

```
function carregarNoMapa() {
  var novoLocal = new google.maps.LatLng(-22.878735,-47.058499)
  map.setCenter(novoLocal);
}

var bt = document.getElementById('bt-carregar');

bt.addEventListener('click', function(e) {
  carregarNoMapa();
});

```

Agora falta verificarmos se o novo endereço está dentro do shape certo?
Para "escutar" mudanças no mapa, utilizaremos o evento 'center_changed', que é disparado toda vez que o centro do mapa muda.

```
  google.maps.event.addListener(map, 'center_changed', function(){
    // Pega as coordenadas do centro do mapa e salva no array
    var coordsCentro = [map.getCenter().A, map.getCenter().F];
  });
```

Lembram da biblioteca geometry que carregamos previamente na tag script?
Ela nos serve o método 'containsLocation', que verifica se as coordenadas passadas como parâmetro estão dentro do nosso shape, e retorna true ou false.

```
estaNaArea = google.maps.geometry.poly.containsLocation(new google.maps.LatLng(coordsCentro[0], coordsCentro[1]), princiPolygon);
```

Para mostrar o resultado, vamos fazer um if simples e exibir no html mesmo.

```
if(estaNaArea){
  resultado.innerHTML = 'Você <b>está</b> na área da Princi Web';
}
else{
  resultado.innerHTML = 'Você <b>não está</b> na área da Princi Web';
}
```

Não deixe de baixar o projeto em sua máquina, e deixar suas críticas sugestões ou dúvidas.

Até a próxima. :smile: