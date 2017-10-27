# Ruby on Rails

### Crear app en Rails e instalar  dependencias "Ruby GEMs"
    rails new appName
    bundle install

### Arrancar servidor
    rails server

### Acceder a modo terminal
    rails console

## Convenciones para estructura de MVC Rails
- Nombre de Modelo -> Singular, primer letra mayuscula
- Nombre de tabla -> Plural, minusculas del nombre de Modelos
- Nombre del archivo de Modelo -> En minusculas pero en singular, articulo.rb
- Nombre de controlador -> Plural del nombre de Modelo, articulos_controller.rb

## Models y migraciones

### Crear migración
    rails generate migration create_modelPluralName atribute:type atribute2:type2

### Modificar una tabla desde una migración
    rails generate migration add_camponuevo_to_nombretabla
    
- En el archivo de migración generado se agrega el/los campos definiendo la tabla, el nombre del campo y el tipo de dato:

        def change
            add_column :table, :column1, :type
            add_column :table, :column2, :type
            add_column :table, :created_at, :datetime
            add_column :table, :updated_at, :datatime
        end

### Ejecutar los archivos de migración y hacer rollback
    rake db:migrate
    rake db:rollback

### Crear registros en BD. Existen 3 formas
*1.*

    var = Model.new
    var.atribute1 = "valor1"
    var.atribute2 = "valor2"
    var.save

*2.*

    var = Model.new(atribute1: "valor1", atribute2: "valor2")
    var.save

*3.*

    Model.create(atribute1: "valor1", atribute2: "valor2")

## Editar, Borrar y Validaciones

### Buscar un registro por ID de tabla
    var = Model.find(id)

### Crear un archivo de migración con todo el template CRUD
    rails generate scaffold ModelName atribute:type atribute2:type2

