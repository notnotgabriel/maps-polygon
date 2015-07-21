# Objetivo: Mostrar como usar google maps polygon

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

Vamos declarar um array com as coordenadas que usaremos para desenhar nosso shape.
Como um polígono deve ter 3 ou mais ângulos, salvei 3 coordenadas que circundam o nosso endereço.

```
var princiCoords = [
  new google.maps.LatLng(-22.878735,-47.058499),
  new google.maps.LatLng(-22.879694,-47.058392),
  new google.maps.LatLng(-22.878686,-47.057598)
];
```

Para demarcar a nossa área, usaremos o objeto Polygon, e para declará-lo basta fazer:

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

Dentro da função initialize, devemos inserir o objeto em nosso mapa usando:

```
princiPolygon.setMap(map);
```

Prontinho, já temos um objeto Shape desenhado em nosso mapa.

