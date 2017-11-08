# Ruby on Rails Blog

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
    rails generate migration create_modelPluralName attribute_1:type attribute_2:type2

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
    var.attribute_1 = "valor1"
    var.attribute_2 = "valor2"
    var.save

*2.*

    var = Model.new(attribute_1: "valor1", attribute_2: "valor2")
    var.save

*3.*

    Model.create(attribute_1: "valor1", attribute_2: "valor2")

## Editar, Borrar y Validaciones

### Busca en modelo un registro por ID
    var = Model.find(id)

### Edita los atributos del registro
    var.attribute_1 = "Nuevo valor"
    var.attribute_2 = "Nuevo valor"
    var.save

### Borrar el registro
    var.destroy

### Agregar validación a un modelo
    class ModelName < ActiveRecord::Base
      validates :attribute_1, presence: true, length: { minimum: 3, maximum: 50 }
      validates :attribute_2, presence: true, length: { minimum: 3, maximum: 50 }
    end

### Acceder a errores por Validaciones
- Ver representción del objeto

      var.errors

- Validar si hay errores. Boolean

      var.errors.any?

- Obtener todos los mensajes de error

      var.errors.full_messages

### Obtener todos los registros de un modelo
    Model.all

### Crear un archivo de migración con todo el template CRUD
    rails generate scaffold ModelName attribute:type attribute_2:type2

## Crear, Editar con Validaciones y errores

### Agregar todas las rutas del CRUD
*routes.rb*

    resources :modelPluralName

### Crear acciones en el controlador
*modelsController*

    def index | new | create | edit | update | show | destroy
      #Index
      @variables = Model.all
      #New
      @variable = Model.new
      #Create
      @variable = Model.new(model_params)
      @variable.save
      redirect_to model_path(@variable)
      #Edit
      @variable = Model.find(params[:id])
      #Update
      @variable = Model.find(params[:id])
      @variable.update(model_params)
      redirect_to model_path(@variable)
      #Show
      @variable = Model.find(params[:id])
    end

    private
      def model_params
        params.require(:model).permit(:attribute1, :atribute2)
      end

### Crear vista para formulario
*vista/new.html.erb | vista/edit.html.erb*

    #Si hay errores los itera
    <% if @variable.errors.any? %>
    <h3>The following errors prevented the article from getting created</h3>
      <ul>
      <% @variable.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
      </ul>
    <% end %>
    #Formulario con inputs mapeados del modelo
    <%= form_for @variable do |f| %>
      <p>
      <%= f.label :title %><br/>
      <%= f.text_field :title %>
      </p>

      <p>
      <%= f.label :description %><br/>
      <%= f.text_area :description %>
      </p>

      <p>
      <%= f.submit %>
      </p>
    <% end %>
