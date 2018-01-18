# geocodeapp
This web application uses Google geocode map api to locate addresses.  It pulls in the address, county, latitude and longitude.   
<!DOCTYPE>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.3/css/bootstrap.min.css" integrity="sha384-Zug+QiDoJOrZ5t4lssLdxGhVrurbmBWopoEl+M6BdEfwnCJZtKxi1KgxUyJq13dy" crossorigin="anonymous">
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    
    <style>
        body{
            margin-top:70px;
            background:#fff;
            color:#333;
        }
    </style>
    <title>My Geocode App</title>
    </head>
    
<body>
    
    <div class="container">
        <div class="row"> 
            <div class="col-md-6 offset-md-3">
        <h2 id="text-center">Enter Location:</h2>
        <form id="location-form">
          <div class="form-group"> 
        <input type="text" id="location-input" class="form-control form-control-lg">
              
            <br>
            <button type="submit" class="btn btn-primary btn-block">Submit</button>
              </div> 
        </form>
    <div class="card-block" id="formatted-address"></div>
    <div class="card-block" id="address-components"></div>
        <div class="card-block" id="geometry"></div>
        </div>
        </div>
        </div>
        
    <script>
        
    //Call Geocode
        //geoCode();
        
    //Get location form
    var locationForm = document.getElementById('location-form');
        
    //Listen for submit
    locationForm.addEventListener('submit',geoCode);
        
    function geoCode(e){
        //Prevent actual submit
        e.preventDefault();
        
            var location = document.getElementById('location-input').value; 
            axios.get('https://maps.googleapis.com/maps/api/geocode/json',{
                  params:{
                      address: location,
                      key: 'AIzaSyBPT5b9W9i3ieq4D7CmZLL25oXGpX4nGUk'
                  }
                  })
             .then(function(response){
                //Log full response
                console.log(response);
                
                //Formatted address
               var formattedAddress = response.data.results[0].formatted_address;
                var formattedAddressOutput = `<ul class="list-group">
<li class="list-group-item">${formattedAddress}</li>
        
        </ul>`;
                
            //Address components
            var addressComponents = response.data.results[0].address_components;
            var addressComponentsOutput = '<ul class="list-group">';
            
            for(var i=0; i<addressComponents.length;i++){
                addressComponentsOutput +=`
<li class="list-group-item">${addressComponents[i].types[0]}: ${addressComponents[i].long_name}</li>`;
            }    
                addressComponentsOutput += '</ul>';
                
            //Geometry
            var lat = response.data.results[0].geometry.location.lat;
                var lng = response.data.results[0].geometry.location.lng;
                var geometryOutput = `<ul class="list-group">
<li class="list-group-item"><strong>Latitude</strong>:${lat}</li>
<li class="list-group-item"><strong>Longitude</strong>:${lng}</li>
        
        </ul>`;    
                
           //Output to app
            document.getElementById('formatted-address').innerHTML = formattedAddressOutput;
            
                
            document.getElementById('address-components').innerHTML = addressComponentsOutput;
                
            document.getElementById('geometry').innerHTML = geometryOutput;    
                
                
                    
                
            })
        .catch(function(error){
                console.log(error);
    });
    }
    
    </script>
    </body>



</html>
