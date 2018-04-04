# Select-Dinamico-Geografico-en-LARAVEL
Funcion para los select dinamicos geograficos [Estado-ciudad] en PHP-Laravel


Utilizando la arquitectura MVC (Modelo Vista Controlador) debemos de tener el modelo creado.
En mi caso, utilizo los modelos "TiPais", "TiEstados", "TiMunicipios" y "TiParroquias" que pertenencen al controlador de geografia "GEO"

Existen dos maneras de hacerlas en laravel, al menos 2 formas encontre de hacerlo. Mostraré la más fácil


»»CONTROLADOR

  public function getMunicipio(Request $request, $idestado) 
  {
  //Llamado de los modelos para select dependientes de estado
      if($request->ajax()) {
          $municipios = TiMunicipios::municipios($idestado);
             return response()->json($municipios);
      } 
  }
  
  public function getParroquia(Request $request, $idmunicipio)
  {
      if($request->ajax()) {
          $parroquias = TiParroquias::parroquias($idmunicipio);
          return response()->json($parroquias);
      }
  }
  
    
 »»Ruta (route/web.php en Laravel 5)
 
  //Le indicamos la url por la que va a pasar los datos así como el nombre de la funcion en el controlador
  Route::get('municipio/{idestado}', 'GeoController@getMunicipio');
  Route::get('parroquia/{idmunicipio}', 'GeoController@getParroquia');
 
 
 »».JS
 
 //Función js con Ajax para el llamado de los select de la vista
  $('#estado').on('change', function() {
        var clvestado = $('#estado').val();

        $.ajax({
          url: "municipio/"+idestado,
          type: "GET",
          dataType: "json",
          error: function(element){
          //console.log(element);
          }, 
          success: function(respuesta){
            $("#municipio").html('<option value="" selected="true"> Seleccione una opción </option>');
            respuesta.forEach(element => {
              $('#municipio').append('<option value='+element.idmunicipio+'> '+element.nombre+' </option>')
            });
          }
        });
    });

  $('#municipio').on('change', function() {
        var idmunicipio = $('#municipio').val();

        $.ajax({
          url: "parroquia/"+idmunicipio,
          type: "GET",
          dataType: "json",
          error: function(){

          }, 
          success: function(respuesta){
            $('#parroquia').html('<option value="" selected="true"> Seleccione una opción </option>');
            respuesta.forEach(element => {
              $("#parroquia").append('<option value='+element.idparroquia+'> '+element.nombre+' </option>')
            });
          }
        });
  });
 
 
 Y esto así, debería mostrar los select dependientes.
 
