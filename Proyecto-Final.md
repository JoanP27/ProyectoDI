
<style>
body, .markdown-body {
    max-width: 100% !important;
    width: auto !important;
    margin: 0 !important;
    padding: 2em !important;
}

section.portada {
   height:90vh;
   border:1px solid black;
   padding: 1em;
   display: flex;
   justify-content: center;
   align-items: center;
   flex-direction: column;
}

span.titulo{
   font-weight:bold;
   font-size:30pt;
}


span.datos{
   font-size: 14pt;
}


table {
    width: 100% !important;
    border-collapse: collapse;
}

span.linia{
   border: 1px solid black;
   width: 15em;
   margin: 10px 0px;
}

table {
    width: 100% !important;
    border-collapse: collapse;
    table-layout: fixed; 
}

td, th {
    word-wrap: break-word;
    overflow-wrap: break-word;
    vertical-align: top;
}


@media print {
    section.portada, section.page-break {
       break-after: page; 
       page-break-after: always; 
    }
}


</style>

<section class="portada">
<span class="titulo">Proyecto Final</span>
<span class="linia"></span>
<span class="datos">Joan Pomares Herrero - 2 DAM / DAW</span>
</section>

## Resumen del proyecto

Este proyecto se basa en una aplicación de reserva de entradas de un cine, similar al estilo de la web de <a href="https://odeonmulticines.com"> odeon multicines</a>. Tiene 6 vistas diferentes:

### Vista de inicio de sesion y registro

En esta vista el usuario se podra iniciar sesion y registrarse, al hacerlo el panel de navegacion cambiara para mostrar el nombre del usuario logeado. Si el usuario es admin se mostrara la pestaña de gestion de usuarios.

<img src="https://github.com/JoanP27/ProyectoDI/blob/main/LoginIn.png?raw=true">

### Vista de cartelera

La vista de la cartelera mostrara las diferentes peliculas que tiene el cine actualmente, al hacer click en cualquiera de ellas nos llevara a las sesiones que tiene esa pelicula en concreto.

<img src="https://github.com/JoanP27/ProyectoDI/blob/main/Cartelera-logout.png?raw=true">

### Vista de sesiones

La vista de sesiones lista las diferentes sesiones de una o varias peliculas segun el dia en el que cliquemos. En la parte superior se muestra una lista con varios dias de la semana que podemos clicar. Al hacer click en alguna de las horas de una sesion, accederemos a a la vista de seleccion de asientos

<img src="https://github.com/JoanP27/ProyectoDI/blob/main/Sesiones-logout.png?raw=true">

### Vista de seleccion de asientos

En esta vista podremos ver las diferentes filas y asientos de la sala donde se reproducira la pelicula, los asientos pueden estar vacios o ocupados, se puede pulsar en los asientos vacios para poder reservarlos, tras reservar los asientos se podra pulsar en siguiente o en cancelar. Al pulsar siguiente accederemos a la vista de compra de entradas.

<img src="https://github.com/JoanP27/ProyectoDI/blob/main/SeleccionarAsientos-login.png?raw=true">

### Vista de compra de entradas

En esta vista se muestra un resumen de todas las entradas compradas con la posibilidad de quitarlas si se desea. Debajo de la lista de entradas se muestra el subtotal de todas las entradas y los botones de comprar y cancelar, al pulsar comprar se compraran las entradas para el usuario con la sesion iniciada y se imprimira una pdf con el recivo y el codigo de la entrada

<img src="https://github.com/JoanP27/ProyectoDI/blob/main/Comprar-entradas-login.png?raw=true">

<section class="page-break"></section>

## Estructura del proyecto

### Models

<table>
<thead>
<tr>
<th>Nombre</th>
<th>Descripcion</th>
<th>Propiedades</th>
</tr>
</thead>
<tbody>
<td>Sesión</td>
<td>El modelo que representa una sesión de una película</td>
<td>
<ul>
<li> Id<b> (Tipo: int, Clave Primaria)</b> </li>
<li> Película <b>(Tipo: Pelicula)</b> </li>
<li> IdSala <b> (Tipo: int, Required) </b> </li>
<li> Sala<b> (Tipo: Sala, Clave foránea(IdSala))</b> </li>
<li> Fecha <b>(Tipo: DateTime, Required)</b> </li>
<li> Duracion <b> (Tipo: TimeSpan, Required) </b> </li>
</ul>
</td>
</tr>

<tr>
<td>Película</td>
<td>El modelo que representa una pelucula del cine</td>
<td>
<ul>
<li> Id<b> (Tipo: int, Clave Primaria)</b> </li>
<li> Nombre <b>(Tipo: String, Required, StringLength(100))</b> </li>
<li> UrlImagen <b>(Tipo: String, StringLength(200), Required)</b></li>
<li> Sesiones<b> (Tipo: List&lt;Sesion&gt;, InverseProperty: Pelicula)</b> </li>
</ul>
</td>
</tr>

<tr>
<td>Sala</td>
<td>El modelo que representa una sala del cine, representada por el numero</td>
<td>
<ul>
<li> Id<b> (Tipo: int, Clave Primaria, Required)</b> </li>
<li> Numero <b>(Tipo: int, Required)</b> </li>
<li> Filas<b> (Tipo: List&lt;Fila&gt;, InverseProperty: Sala)</b> </li>
<li> Sesiones<b> (Tipo: List&lt;Sesion&gt;, InverseProperty: Sala)</b> </li>
</ul>
</td>
</tr>

<tr>
<td>Fila</td>
<td>El modelo que representa una fila en la sala del cine, representada por el numero de fila</td>
<td>
<ul>
<li> Id<b> (Tipo: int, Clave Primaria)</b> </li>
<li> Numero <b>(Tipo: int, Required)</b> </li>
<li> Asientos<b> (Tipo: List&lt;Asiento&gt;, InverseProperty: Fila)</b></li>
<li>Sala <b>(Tipo: Sala)</b></li>
</ul>
</td>
</tr>

<tr>
<td>Asiento</td>
<td>El modelo que representa un asiento en la sala del cine, representada por el numero de asiento</td>
<td>
<ul>
<li> Id<b> (Tipo: int, Clave Primaria)</b> </li>
<li> Numero <b>(Tipo: int, Required)</b> </li>
<li> Ocupada <b>(Tipo: boolean, NotMapped, Por defecto: false)</b></li>
<li> IdFila <b>(Tipo: int, Required)</b> </li>
<li> Fila <b>(Tipo: Fila)</b> </li>
</ul>
</td>
</tr>

<tr>
<td>Usuario</td>
<td>El modelo que representa al usuario</td>
<td>
<ul>
<li> Id<b> (Tipo: int, Clave Primaria)</b> </li>
<li> Nombre <b>(Tipo: string, String(50), Required)</b> </li>
<li> Email<b> (Tipo: String, Required, StringLength(100))</b></li>
<li> Password <b>(Tipo: String, Required, StringLength(100))</b> </li>
<li>Rol <b>(Tipo: RolUsuario, Required, Por defecto: Cliente)</b> </li>
<li> Entradas <b>(Tipo: List&lt;Entrada&gt;, InverseProperty: Usuario)</b></li>
</ul>
</td>
</tr>

<tr>
<td>RolUsuario</td>
<td>Un enum que indica el tipo de rol para cada usuario</td>
<td>
<ul>
<li>Cliente</li>
<li>Administrador</li>
</ul>
</td>
</tr>

<tr>
<td>Entrada</td>
<td>El modelo que representa a la entrada comprada por un usuario</td>
<td>
<ul>
<li> Id<b> (Tipo: int, Clave Primaria)</b> </li>
<li> Subtotal <b>(Tipo: Decimal, Required)</b> </li>
<li> IdFila <b>(Tipo: int)</b> </li>
<li> Fila <b>(Tipo: Fila)</b> </li>
<li> IdAsiento <b>(Tipo: int, Required)</b> </li>
<li> Asiento <b>(Tipo: Asiento, Clave foranea(idAsiento))</b> </li>
<li> IdSesion<b>(Tipo: int, Required)</b> </li>
<li> Sesion<b> (Tipo: Sesion, Clave foranea(IdSesion))</b></li>
<li> IdUsuario<b>(Tipo: int, Required)</b> </li>
<li> Usuario<b>(Tipo: Usuario, Clave foranea(IdUsuario))</b> </li>
</ul>
</td>
</tr>
</tbody>
</table>

<section class="page-break"></section>

### Views

<table>
<thead>

<tr>
<th>Nombre</th>
<th>Descripcion</th>
</tr>
</thead>
<tbody>
<tr>
<td>
MainWindowView
</td>
<td>
Esta es la ventana donde se mostraran las dierentes vistas que usan user control, tiene un panel de navegacion para poder navegar entre vistas.
</td>
</tr>
<tr>
<td>
LogInView
</td>
<td>
Esta vista gestiona un formulario de inicio de sesion y uno de registrarse.
</td>
</tr>
<tr>
<td>
CarteleraView
</td>
<td>
Esta vista muestra las diferentes peliculas que hay en la cartelera,
Se puede hacer click en las peliculas para acceder a las sesiones de esa pelicula en particular.
</td>
</tr>
<tr>
<td>
SesionesView
</td>
<td>
Esta vista muestra las diferentes sesiones que hay en el cine, ordenadas segun su fecha, las sesiones de la misma pelicula pero con diferente hora aparecen como solo una tarjeta con varias horas para seleccionar. Tambien puede mostrar las sesiones de una sola película.
</td>

</tr>
<tr>
<td>
SeleccionarAsientoView
</td>
<td>
Esta vista muestra todas las filas y asientos de la sala donde se reproducira la pelicula. El usuario podra seleccionar los asientos que esten vacios.
</td>
</tr>
<tr>
<td>
ComprarEntradaView
</td>
<td>
Esta vista muestra un resumen con cada entrada a comprar, se permitira eliminar entradas, cancelar la compra para volver a la pantalla de sesiones y comprar las entradas, generando una factura en formato pdf con los datos de cada entrada comprada que sirve como entrada.
</td>
</tr>
<tr>
<td>
UsuariosCrudView
</td>
<td>
Esta vista muestra una lista de usuarios y permite la eliminacion, registro y modificacion de los usuarios. Solo un usuario <b>administrador</b> podra acceder a esta vista.
</td>
</tr>
</tbody>
</table>

<section class="page-break"></section>

### View Models

<table style="width:100%">
<thead>
<tr>
<th style="width:10em">Nombre</th>
<th>Propiedades</th>
<th>Relay Commands</th>



</tr>
</thead>
<tbody>

<tr>
<td>MainWindowViewModel</td>
<td>
<ul>
<li><b>CurrentView(Object): </b> La view a mostrar </li>
<li><b>LoggedUser(User)</b> El usuario con sesión iniciada</li>
<li><b>isLogged(Boolean): </b> Si hay una sesion iniciada</li>

</ul>
</td>
<td>
<ul>
<li><b>ShowLogInView: </b> Cambia <em>currentView</em> a LogInView</li>
<li><b>ShowCarteleraView: </b> Cambia <em>currentView</em> a CarteleraView</li>
<li><b>ShowSesionesView: </b> Cambia <em>currentView</em> a SesionesView</li>
<li><b>ShowSeleccionarAsientoView: </b> Cambia <em>currentView</em> a SeleccionarAsientoView</li>
<li><b>ShowComprarEntradaView: </b> Cambia <em>currentView</em> a ComprarEntradaView</li>
<li><b>ShowUsuariosCrudView: </b> Cambia <em>currentView</em> a UsuariosCrudView</li>
</ul>
</td>
</tr>

<tr>
<td>LogInViewModel</td>
<td>
<ul>
<li><b>RegisterUser</b>: Usuario a registrar</li>
<li><b>LoginUser</b>: Usuario a iniciar sesion</li>

<li><b>RegisterName: </b> Nombre del usuario a registrar </li>
<li><b>RegisterEmail: </b> Correo del usuario a registrar</li>
<li><b>RegisterPassword: </b> Contraseña del usuario a registrar </li>
<li><b>RegisterPasswordRepeat: </b> Contraseña a repetir del usuario a registrar</li>

<li><b>LoginName: </b> Nombre del usuario a iniciar sesión</li>
<li><b>LoginEmail: </b> Email del usuario a iniciar sesión</li>
<li><b>LoginPassword: </b> Contraseña del usuario a iniciar sesión</li>
</ul>
</td>
<td>
<ul>
<li><b>RegisterCommand:</b> Valida y registra al usuario en la base de datos si los datos son correctos</li>
<li><b>LoginCommand:</b> Valida y inicia sesion del usuario en la base de datos si los datos son correctos</li>
</ul>
</td>
</tr>

<tr>
<td>CarteleraViewModel</td>
<td>
<ul>
<li><b>FilmList(ObservableCollection&lt;Film&gt;):</b> Lista de Peliculas en la cartelera</li>
</ul>
</td>
<td>
<ul>
<li><b>ViewFilmSessionsCommand(ICommand&lt;Film&gt;): </b> Navega a la ventana de sesiones donde se mostraran las sesiones de esa pelicula </li>
</ul>
</td>
</tr>
<tr>

<td>SesionesViewModel</td>
<td>
<ul>
<li><b>VisibleFilmList(ObservableCollection&lt;Session&gt;): </b> Lista de peliculas visible en la view</li>
<li><b>DateList(ObservableCollection&lt;DateTime&gt;): </b> Lista de dias seleccionables</li>
</ul>
</td>
<td>
<ul>
<li><b>ViewSeatsCommand(ICommand&lt;Session&gt;):</b> Navega a la view de seleccion de asientos</li>
</ul>
</td>
</tr>

<tr>
<td>SeleccionarAsientoViewModel</td>
<td>
<ul>
<li><b>VisibleRowList(ObservableCollection&lt;Fila&gt;):</b> Lista de filas con asientos</li>

</ul>
</td>
<td>
<ul>
<li><b>ViewBuyTicketCommand(ICommand&lt;List&lt;Entrada&gt;&gt;):</b> Navega a la view de comprar entradas con las entradas compradas</li>
</ul>
</td>
</tr>

<tr>
<td>ComprarEntradaViewModel</td>
<td>
<ul>
<li><b>VisibleTicketList(ObservableCollection&lt;Entrada&gt;):</b> Lista de entradas visibles</li>

</ul>
</td>
<td>
<ul>
<li><b>BuyTicketsCommand(ICommand):</b> Calcula el precio y genera una factura con las entradas compradas</li>
</ul>
</td>
</tr>

<tr>
<td>UsuariosCrudViewModel</td>
<td>
<ul>
<li><b>VisibleUserList(ObservableCollection&lt;Usuario&gt;)</b></li>
<li><b>SelectedUser(Usuario)</b></li>
<li><b>UserName: </b> Nombre del usuario a registrar </li>
<li><b>UserEmail: </b> Correo del usuario a registrar</li>
<li><b>UserPassword: </b> Contraseña del usuario a registrar </li>
</ul>
</td>
<td>
<ul>
<li><b>RegisterUserCommand(ICommand)</b></li>
<li><b>RemoveUserCommand(ICommand)</b></li>
<li><b>SelectedItemChangedCommand(ICommand)</b></li>
</ul>
</td>
</tr>
</tbody>
</table>

<section class="page-break"></section>

### Services

<table>
<thead>
<tr>
<th>Nombre</th>
<th>Descripción</th>
<th>Metodos</th>
</tr>
</thead>
<tbody>

<tr>
<td>ServiceSesion</td>
<td>Servicio que gestiona el modelo Sesión</td>
<td>
<ul>
<li><b>Listar()</b>: devuelve List&lt;Sesion&gt;, Lista todas las sesiones de la base de datos </li>
<li><b>ListarPorId(int id)</b>: devuelve Sesión, Lista la sesion de la base de datos que coincida con el id pasado por parametro </li>
<li><b>Insertar()</b>: devuelve Sesion, Inserta una sesión en la base de datos </li>
<li><b>Actualizar()</b>: devuelve Boolean, Actualiza una sesión de la base de datos </li>
<li><b>Borrar()</b>: devuelve Boolean, Borra una sesión de la base de datos </li>
</ul>
</td>
</tr>

<tr>
<td>ServicePelicula</td>
<td>Servicio que gestiona el modelo Película</td>
<td>
<ul>
<li><b>Listar()</b>: devuelve List&lt;Pelicula&gt;, Lista todas las películas de la base de datos </li>
<li><b>ListarPorId(int id)</b>: devuelve Película, Lista la película de la base de datos que coincida con el id pasado por parametro </li>
<li><b>Insertar()</b>: devuelve Pelicula, Inserta una película en la base de datos </li>
<li><b>Actualizar()</b>: devuelve Boolean, Actualiza una película de la base de datos </li>
<li><b>Borrar()</b>: devuelve Boolean, Borra una película de la base de datos </li>
</ul>
</td>
</tr>

<tr>
<td>ServiceSala</td>
<td>Servicio que gestiona el modelo Sala</td>
<td>
<ul>
<li><b>Listar()</b>: devuelve List&lt;Sala&gt;, Lista todas las salas de la base de datos </li>
<li><b>ListarPorId(int id)</b>: devuelve Sala, Lista la sala de la base de datos que coincida con el id pasado por parametro </li>
<li><b>Insertar()</b>: devuelve Sala, Inserta una sala en la base de datos </li>
<li><b>Actualizar()</b>: devuelve Boolean, Actualiza una sala de la base de datos </li>
<li><b>Borrar()</b>: devuelve Boolean, Borra una sala de la base de datos </li>
</ul>
</td>
</tr>

<tr>
<td>ServiceFila</td>
<td>Servicio que gestiona el modelo Fila</td>
<td>
<ul>
<li><b>Listar()</b>: devuelve List&lt;Fila&gt;, Lista todas las filas de la base de datos </li>
<li><b>ListarPorId(int id)</b>: devuelve Fila, Lista la fila de la base de datos que coincida con el id pasado por parametro </li>
<li><b>Insertar()</b>: devuelve Fila, Inserta una fila en la base de datos </li>
<li><b>Actualizar()</b>: devuelve Boolean, Actualiza una fila de la base de datos </li>
<li><b>Borrar()</b>: devuelve Boolean, Borra una fila de la base de datos </li>
</ul>
</td>
</tr>

<tr>
<td>ServiceAsiento</td>
<td>Servicio que gestiona el modelo Asiento</td>
<td>
<ul>
<li><b>Listar()</b>: devuelve List&lt;Asiento&gt;, Lista todas los asientos de la base de datos </li>
<li><b>ListarPorId(int id)</b>: devuelve Asiento, Lista el asiento de la base de datos que coincida con el id pasado por parametro </li>
<li><b>Insertar()</b>: devuelve Asiento, Inserta un asiento en la base de datos </li>
<li><b>Actualizar()</b>: devuelve Boolean, Actualiza un asiento de la base de datos </li>
<li><b>Borrar()</b>: devuelve Boolean, Borra un asiento de la base de datos </li>
</ul>
</td>
</tr>

<tr>
<td>ServiceUsuario</td>
<td>Servicio que gestiona el modelo Usuario</td>
<td>
<ul>
<li><b>Listar()</b>: devuelve List&lt;Usuario&gt;, Lista todos los usuarios de la base de datos </li>
<li><b>ListarPorId(int id)</b>: devuelve Usuario, Lista el usuario de la base de datos que coincida con el id pasado por parametro </li>
<li><b>Insertar()</b>: devuelve Usuario, Inserta un usuario en la base de datos </li>
<li><b>Actualizar()</b>: devuelve Boolean, Actualiza un usuario de la base de datos </li>
<li><b>Borrar()</b>: devuelve Boolean, Borra un usuario de la base de datos </li>
</ul>
</td>
</tr>

<tr>
<td>ServiceEntrada</td>
<td>Servicio que gestiona el modelo Entrada</td>
<td>
<ul>
<li><b>Listar()</b>: devuelve List&lt;Entrada&gt;, Lista todas las entradas de la base de datos </li>
<li><b>ListarPorId(int id)</b>: devuelve Entrada, Lista la entrada de la base de datos que coincida con el id pasado por parametro </li>
<li><b>Insertar()</b>: devuelve Entrada, Inserta una entrada en la base de datos </li>
<li><b>Actualizar()</b>: devuelve Boolean, Actualiza una entrada de la base de datos </li>
<li><b>Borrar()</b>: devuelve Boolean, Borra una entrada de la base de datos </li>
</ul>
</td>
</tr>
<tr>
<td>ServiceNavigation</td>
<td>Gestiona la navegacion entre user controls</td>
<td>
<ul>
<li><b>navigateTo(object view):</b> le dice al MainWindowViewModel que user control mostrar</li>
</ul>
</td>
</tr>

</tbody>
</table>

## Enlaces

Proyecto de figma: <a href="https://www.figma.com/design/C5zsXY6dWEGKVrvBfbVvJh/Proyecto-final-Dise%C3%B1o-Interfaces?node-id=0-1&m=dev&t=YRZr95skbGLP00S3-1">[enlace]</a>




