# *Link de tutoriales utiles*

La idea es colocar diferentes links de tutoriales diversos sobre diferentes cosas que me han servido.

## Exportar galeria powerapps a excel
https://www.youtube.com/watch?v=97D--j0IRno&ab_channel=Mart%C3%ADnGesualdo

Nota de ese tutorial: 

1. La opcion direct no la tienen, en ese caso la solucion la encontre aqui mismo en un comentario y es agregar algo al codigo para que el archivo se descargue. quedaria asi: Launch(_url & "?download=1")

2. El orden de las columnas sin importar como este en el codigo al exportar queda de forma alfabetica. La solucion en texto no creo que llegue a comprenderse bien pero ahi voy: antes de la opcion crear tabla csv en el flow se deben agregar 2 pasos mas, el primero es el paso redactar o compose(depende el idioma) y en este deben seleccionar el excelData como entrada, tras agregar ese deben guardar el flow y darle clic al boton en la app para que se ejecute el flujo y puedan ir al historico de ejecutados y copiar el output de ese paso del flujo, este se usa en el segundo paso adicional que hay que agregar al flow que se llama parse json o analisis del archivo json. Aqui en las entradas deben seleccionar las salidas del paso anterior y en esquema deben darle a generar desde la muestra y pegar el output que copiaron anteriormente, tras hacer eso, en el paso de crear csv table se quita el json que teniamos antes y se selecciona el cuerpo(body) del paso anterior y en campos quitan el automatico y seleccionan personalizado y colocan los valores en el orden que desean con sus respectivos valores.
