const express = require('express');
const https = require('https');
const fs = require('fs');
const path = require('path');
const app = express();
 
//setting app
app.set('port', process.env.PORT || 3443);

//middlwares 
app.use(express.json());
app.use(express.urlencoded({entended: false}));

//routes
app.get("/", (req, res)=>{
  res.json({massage: "Bienvenidos/as a API Abogados" });
});

// Endpoint - Listar disponibilidades
app.get("/disponibilidades/:inicio/:termino", (req, res) => {
    const fechaInicio = req.params.inicio;
    const fechaTermino = req.params.termino;
    // ir a buscar a la DB los horarios disponibles para fechas inicio y término
    // console.log("Fecha inicio: ", fechaInicio);
    // console.log("Fecha término: ", fechaTermino);
  
  const respuesta_db = [
      {
        "fecha": "2024-10-15",  
        "horarios": [
          {
            "desde": "09:00",
            "hasta": "10:00",
            "disponible": true,
            "color": "green",
            "abogado": "Juan Pérez"
          }, 
          {
            "desde": "10:00",
            "hasta": "11:00",
            "disponible": false,
            "color": "red",
            "abogado": "Juan Pérez"
          },
          {
            "desde": "11:00",
            "hasta": "12:00",
            "disponible": true,
            "color": "green",
            "abogado": "Juan Pérez"
          }
       ]
      },
      {
        "fecha": "2024-10-16",
        "horarios": [
          {
            "desde": "09:00",
            "hasta": "10:00",
            "disponible": true,
            "color": "green",
            "abogado": "Juan Pérez"
          }, 
          {
            "desde": "10:00",
            "hasta": "11:00",
            "disponible": true,
            "color": "green",
            "abogado": "Juan Pérez"
          },
          {
            "desde": "11:00",
            "hasta": "12:00",
            "disponible": true,
            "color": "green",
            "abogado": "Juan Pérez"
          }
        ]
      }, 
      {
        "fecha": "2024-10-17",
        "horarios": [
         {
            "desde": "09:00",
            "hasta": "10:00",
            "disponible": false,
            "color": "red",
            "abogado": "Juan Pérez"
          }, 
          {
            "desde": "10:00",
            "hasta": "11:00",
            "disponible": false,
            "color": "red",
            "abogado": "Juan Pérez"
          },
          {
            "desde": "11:00",
            "hasta": "12:00",
            "disponible": false,
            "color": "red",
            "abogado": "Juan Pérez"
          }
        ]
      }
    ]
  
  const respuesta = {
      "fechaInicio": fechaInicio,
      "fechaTermino": fechaTermino,
      "disponibilidades": respuesta_db,
      "mensaje": "Listar disponibilidades"
    }
  
    res.json(respuesta);
  })
  
// Endpoint - Reservar hora
app.post("/reservar", (req, res) => {
    const datos = req.body;
   
  
  // Ir a guardar a la DB datos de la reserva
  const procesada = false;
  
  let respuesta = {
   "reservado": true,
   "resultado": "ok",
   "mensaje": "Hora reservada"
  }  
  if (!procesada) {
    respuesta = {
      "reservado": false,
      "resultado": "error",
      "mensaje": "Hora se encuentra reservada por otra persona"
    }
  }
  
  res.json(respuesta);
})
 
//levantando el servidor HTTP
//app.listen(app.get('port'),()=>{
//    console.log(`Servidor iniciado! ${app.get('port')}`);
//} );

const sslServer = https.createServer(
    {     
        key: fs.readFileSync(path.join(__dirname,'cert','key.pem')),
        cert: fs.readFileSync(path.join(__dirname,'cert','cert.pem'))
    },
    app 
);         

sslServer.listen(app.get('port'), () => 
    console.log(`Servidor iniciado en el puerto ${app.get('port')}`)
);
