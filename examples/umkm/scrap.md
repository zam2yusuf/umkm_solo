```js
var toditas = '';
var num_sucursales = 0;

function dameSucursales(zona) {
    posicionCaja=zona;
    var ajax = new Ajax.Request("GetAssests.do", {
        parameters: "level=activos&ubicacion=Toledo&lat=39.867&lng=-4.00649999999996&posicionCaja=" + posicionCaja+"&cache=false",
        method: "post",
         onSuccess: function(response){
             var temp = JSON.parse(response.responseText);
             temp = temp.length;
             num_sucursales += temp;
             console.log('Recibidas '+temp);
             toditas += response.responseText.slice(1,response.responseText.length-1)+",";
         }
    });
}

function recursivator(zona) {
  dameSucursales(zona["posicionCaja"]);
  if (zona.subActivos) {
    zona.subActivos.forEach(recursivator);
  }
}

resultadosZonas.forEach(recursivator);
```

```js

function crearTextArea(texto) {
  var toditas_textarea = document.createElement('textarea');

  Element.extend(toditas_textarea);
  toditas_textarea.setStyle({width:"100%",height:"300px"});
  
  toditas_textarea.update(texto);

  document.body.appendChild(toditas_textarea);
  return toditas_textarea;
}

toditas = "[\n"+toditas.slice(0,toditas.length-1)+"\n]";
var toditas_textarea = crearTextArea(toditas);
console.log('Número total de sucursales procesadas: '+num_sucursales);
```

```js

function crearTextArea(texto) {
  var toditas_textarea = document.createElement('textarea');

  Element.extend(toditas_textarea);
  toditas_textarea.setStyle({width:"100%",height:"300px"});
  
  toditas_textarea.update(texto);

  document.body.appendChild(toditas_textarea);
  return toditas_textarea;
}

function aCSV() {
	var csv = '#cp;localidad;provincia;tipo_entidad;numero_entidad;direccion_via;telefono_fax;latitud;longitud\n';
   var json_toditas = JSON.parse(toditas);
	json_toditas.forEach(function(s) {
		csv += s.cp+";"+s.localidad+";"+s.provincia+";"+s.tipo_entidad+";"+s.numero_entidad+";"+
			s.direccion_via+";"+s.telefono_fax+";"+s.latitud+";"+s.longitud+"\n";
	});
	var toditas_textarea = crearTextArea(csv);
}

aCSV();
```

```sh
grep ";oficina;" bankia.raw.csv | awk -F ";" '{print $6";"$1";"$2";"$3";"$7";"$8";"$9}' | sort | uniq > bankias.csv
```
