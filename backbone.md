# BACKBONE

## Dependencias:

  - Underscore
  - Jquery

## Ver versión de Node JS
$ node -v

## Backbone components:
 - Models: Store Data.
 - Views: Mostrar el Data.
 - Controllers: No hay en recientes versiones:
    - Collections.
    - Events.
    - Routers: Navigation.
    - Sync.

# Instalación de Backbone

## Index.html : Se meten las dependencias

  <!-- Order Matters ! -->

  <!-- Dependencias  -->
  <script src="js/libs/jquery.js"></script>
  <script src="js/libs/underscore.js"></script>
  <script src="js/libs/backbone.js"></script>

  <!-- Models  -->
  <script src="js/models/singleFlowerModel.js"></script>

  <!-- Views  -->
  <script src="js/views/singleFlowerView.js"></script>
  <script src="js/views/allFlowersView.js"></script>

  <!-- Collections  -->
  <script src="js/collections/allFlowers.js"></script>

  <!-- Routes  -->
  <script src="js/routes/router.js"></script>

  <!-- App que necesita todo lo anterior ! -->
  <script src="js/flowerApp.js"></script>

## Mas ficheritos:

 - /css : Allí van los estilos
 - /images: Las imagenes de la page
 - index.html: Es la page en si
 - /js/libs: Allí están las librerías backbone, jquery y underscore
 - /js/models: El modelo de dato
 - /js/routes:
 - /js/views:
 - /js/flowerApp: Es lo que realmente se ejecuta en el index.

## Modelling

 - Namespace an app: para que no se repita el nombre 

  *singleFlowerModel.js*

    var app = app || {};
    app.singleFlower = Backbone.Model.extend (
      {
        defaults: {...},
        initialize: function() {...}
      }
    );

  *js/flowerApp.js*

     var redRoses = new app.singleFlower({...});
     var bluRoses = new app.singleFlower({...});
     var greRoses = new app.singleFlower({...});

     console.log(redRoses.toJSON());
     console.log(bluRoses.toJSON());
     console.log(greRoses.toJSON());

 - Ahora al ir al index.html nos devuelve el JSON

#### Watching for changes

 - initialize()
 - get()
 - set()
 - on()

 *this.on('change')* para saber enseguida que cambió algo en el modelo.
 *this.on('change:price')* para saber enseguida que cambió algo concreto en el modelo.

### Creación de Objetos de Objetos

var flowerGroup = new app.FlowersCollection(
  [redRoses, rainBow ]

###### Añadir Objetos

flowerGroup.add(greenFlow);

###### Quitar Objetos

flowerGroup.remove(redRoses);

### EJEMPLO

#### flowerApp.js (MUY IMPORTANTE USAR el valor Link ya que no tenemos nada por defecto)
var fantalizingTulips = new app.singleFlower({
 name: "Tantalizing Tulips",
 price: 122,
 color: "red",
 link: "valor"
});

var fleurDeLis = new app.singleFlower({
 name: "Fleur-de-lis",
 price: 123,
 color: "green",
 link: "valor"
});

var euroFlower = new app.FlowersCollection(
  [fleurDeLis, fantalizingTulips ]
);

fleurDeLis.set({originCountry:"Holland"});

console.log(euroFlower.toJSON());


#### singleFlowerModel


// Namespace our app
var app = app || {};
app.singleFlower = Backbone.Model.extend ({
  defaults: {
    color: "pink",
    img: "images/placeholder.jpg"
  },

  initialize: function() {
    console.log("A model instance named " + this.get("name")·
               +  " has been created and its costs "·
               + this.get("price")
               );

    this.on('change', function(){
      console.log("Something in our model has changed");
    });

    this.on('change:name', function(){
      console.log("The name for the " + this.get("name") + " model just changed to" + this.get("name") );
    });


  }

});


#### allFlowers.js


// Namespace our flower App
 var app = app || {};

app.FlowersCollection = Backbone.Collection.extend({

  model: app.singleFlower

});


## Views

- Uso de Underscore, para Templates.

- render() para hacer el HTML

#### singleFlowerView.js

// Namespace our flowerApp
var app = app || {};

// The view for a single model view, which is one flower
app.singleFlowerView = Backbone.View.extend({

});

#### index.html

 <script id="flowerElement" type="text/template">
    <a href="#<%= link %>">
         <img src="<%= img %>" 
              alt="<%= name %>" 
            class="image" />
    </a>
    <ul>
      <li class="flowerInfo">
              Name:<%= name %>
      </li>
      <li class="flowerInfo">
              Price:<%= price %>
      </li>
      <li class="flowerInfo">
              Color:<%= color %>
      </li>
    </ul>
  </script>

#### Valores por defecto

*IMPORANTE: USAR VALORES POR DEFECTO*

Ej)

 singleFlowerModel.js
   defaults: {
      link: "404"
   }

Si no se usa puede dar este error:
 Uncaught ReferenceError: link is not defined


