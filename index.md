<!DOCTYPE html>

<html>
<head>
    <title>Triplantary</title>
    
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body onload="startUp()">
    
    <div id="headerDiv" style="float:left; height: 10vh; width: 98vw;">
         
        <img src="triplanetarylogo.png" style="float: right; height: 100%;">
    </div>

    <div id="dashboardDiv" style = "width: 20%; float: left;">
        <br>
        <br>
        <span style='font-family: "Arial Black", Gadget, sans-serif'>
            Mission:
            
        </span>
        
        <br>
        <br>
        Boost off from Earth in a Transport with 10 units of fuel.
        Reach Ganymede as quickly as possible without refueling.
        <br>
        <br>
        <span style='font-family: "Arial Black", Gadget, sans-serif'>
            Navigation Log:
        </span>
        <br>
        <div id= "log" style="border: 1px solid; word-wrap: break-word;">B</div>
        <br>
        <br>
            Fuel: <span id= "fuelSpan" style="">10</span>
        <br>
        <div id="navigationDiv" style="" >
            Velocity: <span id="velocitySpan">none</span>
        </div>
        <br>
        <span onmouseover = "showLittleMap();" onmouseout = "hideLittleMap();" >Overview</span>
        <br>
        <br>
        <div id="boostDiv">
            Sitting on Earth, choose direction to boost into orbit:
        </div>
        <div id="navInstDiv">
            Select a destination. Those with green numbers involve a burn, the blue
            square will be the result of coasting.
        </div>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>

        <div id="infoDiv" style="border: 1px solid;">

<span style="display: none;">            Coordinates: <span id="coords"></span><br><br></span>
            
            <div id="Sol" style="display: none;">
                Big ball o'fusion.
            </div>
            <div id="Mercury" style="display: none;">
                One face world.
            </div>
            <div id="Venus" style="display: none;">
                Goddess on a mountain top.
            </div>
            <div id="Terra" style="display: none;">
                Our home, for now.
            </div>
            <div id="Mars" style="display: none;">
                Diameter: 6778km  <br>
                Distance From Sun: 230,000,000km<br>
                Surface Gravity: 3.7m/s<sup>2</sup><br>
                Synodic Period: 687 terran days<br>
                Sol: 24 hours 39 minutes<br>
                
            </div>
            <div id="Jupiter" style="display: none;">
                I think it's looking at us.
            </div>
            <div id="Luna" style="display: none;">
                *The* Moon. Natural satellite
            </div>
            <div id="Callisto" style="display: none;">
                I sing to your spirit.
            </div>
            <div id="Ganymede" style="display: none;">
                Fun name.
            </div>
            <div id="Io" style="display: none;">
                So cold.
            </div>
            <div id="Ceres" style="display: none;">
                Minor planet.
            </div>
            <div id="Clandestine" style="display: none;">
                Nothing here but worthless chunks of rock, floating in space.
            </div>
            
        </div>

    </div>

    <div id="mapDiv" style = "width: 80%; float: left; overflow: scroll; height: 80vh;" onmousemove="mouseoverMap(event)" onclick="mapClick(event)" >
        
                <canvas id="theMap" width="1225" height="1680" style="border:1px solid #000000; "></canvas>
                <canvas id="cleanMap" width="1225" height="1680" style="border:1px solid #000000; display: none; "></canvas>
                <canvas id="pathMap" width="1225" height="1680" style="border:1px solid #000000; display: none; "></canvas>
                
            <canvas id="littleMap" width="613" height="825" style="border:1px solid #000000; display: none; ">
            </canvas>
                

    </div>
    

    <div>
        <img src="arrow1.png" id="arrow1" style="display: none;" >
        <img src="arrow2.png" id="arrow2" style="display: none;" >
        <img src="arrow3.png" id="arrow3" style="display: none;" >
        <img src="arrow4.png" id="arrow4" style="display: none;" >
        <img src="arrow5.png" id="arrow5" style="display: none;" >
        <img src="arrow6.png" id="arrow6" style="display: none;" >
        <img src="harrow1.png" id="harrow1" style="display: none;" >
        <img src="harrow2.png" id="harrow2" style="display: none;" >
        <img src="harrow3.png" id="harrow3" style="display: none;" >
        <img src="harrow4.png" id="harrow4" style="display: none;" >
        <img src="harrow5.png" id="harrow5" style="display: none;" >
        <img src="harrow6.png" id="harrow6" style="display: none;" >
        <img src="asteroid1.png" id="asteroid1" style="display: none;" >
        <img src="asteroid2.png" id="asteroid2" style="display: none;" >
        <img src="asteroid3.png" id="asteroid3" style="display: none;" >
        <img src="minorplanet.png" id="minorplanet" style="display: none;" >
    </div>

<script>
    
    planets = [
    
        { col: 15, row: 44, gravity: 1, diameter: 17, name: "Sol", color: "yellow" },
        { col: 17, row: 47, gravity: 1, diameter: 5, name: "Mercury", color: "red" },
        { col: 9, row: 48, gravity: 1, diameter: 8, name: "Venus", color: "CornflowerBlue" },
        { col: 25, row: 38, gravity: 1, diameter: 8, name: "Terra", color: "blue" },
        { col: 6, row: 24, gravity: 1, diameter: 8, name: "Mars", color: "red" },
        { col: 17, row: 8, gravity: 1, diameter: 12, name: "Jupiter", color: "yellow" },
        { col: 27, row: 37, gravity: 0, diameter: 4, name: "Luna", color: "white" },
        { col: 13, row: 8, gravity: 1, diameter: 4, name: "Callisto", color: "green" },
        { col: 20, row: 6, gravity: 1, diameter: 4, name: "Ganymede", color: "CornflowerBlue" },
        { col: 18, row: 10, gravity: 0, diameter: 4, name: "Io", color: "red" },
        { col: 9, row: 17, gravity: 0, diameter: 0, name: "Ceres", color: "" },
        { col: 26, row: 18, gravity: 0, diameter: 0, name: "Clandestine", color: "" }
    ];
    
    hexWidth = 35;
    hexHeight = 40;
    hexSide = 20
    
    rows = 55;
    columns = 35;
    
    navOptions = [];
    gravHexes = [];
    
    function startUp() {
        
        mapDraw = document.getElementById("theMap").getContext("2d");
        mapDraw.lineWidth = 0.5;
                
        littleMapDraw = document.getElementById("littleMap").getContext("2d");
        cleanMapDraw = document.getElementById("cleanMap").getContext("2d");
        pathMapDraw = document.getElementById("pathMap").getContext("2d");

        for ( row = 0; row < rows; row += 2 ) {
            
            baseY = row * 30;  // why is this? something to do with doing two rows at once...
            
            for ( col = 0; col < columns; col++ ) {
                
                baseX = col * hexWidth;
                
                //console.log( row + ", " + col );
                
                mapDraw.moveTo( baseX + 0, baseY + hexSide/2 );
                mapDraw.lineTo( baseX + hexWidth/2, baseY );
                mapDraw.lineTo( baseX + hexWidth, baseY + hexSide/2 );
                mapDraw.lineTo( baseX + hexWidth, baseY + hexSide/2 + hexSide );
                mapDraw.lineTo( baseX + hexWidth/2, baseY + hexHeight );
                mapDraw.lineTo( baseX + 0, baseY + hexSide/2 + hexSide );
                mapDraw.moveTo( baseX + hexWidth/2, baseY + hexHeight ); // that's for the next row, the short, odd numbered rows (by zero count)
                mapDraw.lineTo( baseX + hexWidth/2, baseY  + 60); // that's for the next row, the short, odd numbered rows (by zero count)
                //mapDraw.stroke();
            }
        }        
        
        mapDraw.stroke();
        
//draw 20-sided

        //twentysider = poly20( 13, 37, 100 );
        //
        ////mapDraw.beginPath();
        ////for ( i=0; i<20; i+=2 ) {
        ////    mapDraw.arc( twentysider[i], twentysider[i+1], 5, 0, 2 * Math.PI);
        ////}
        ////mapDraw.fillStyle = "blue";
        ////mapDraw.fill();
        ////mapDraw.closePath();
        //
        //mapDraw.beginPath();
        //mapDraw.moveTo( twentysider[0], twentysider[1] );
        //
        //for ( i=2; i< twentysider.length; i+=2 ) {
        //    mapDraw.lineTo( twentysider[i], twentysider[i+1] );
        //}
        //mapDraw.lineTo( twentysider[0], twentysider[1] );
        //mapDraw.stroke();


        mapDraw.font = "italic bold 18px Arial";
        
        mapDraw.textAlign="right";
        mapDraw.fillText( planets[0].name, planets[0].col*hexWidth - 5, planets[0].row*30 + 7 );

        mapDraw.textAlign="left";
        mapDraw.fillText( planets[1].name, (planets[1].col + 1.5 ) * hexWidth, planets[1].row*30 + 10 );

        mapDraw.textAlign="left";
        mapDraw.fillText( planets[2].name, (planets[2].col + 1 ) * hexWidth, planets[2].row*30 + 10 );

        mapDraw.textAlign="right";
        mapDraw.fillText( planets[3].name, planets[3].col*hexWidth - 5, planets[3].row*30 + 7 );

        mapDraw.textAlign="left";
        mapDraw.fillText( planets[4].name, (planets[4].col + 1 ) * hexWidth, planets[4].row*30 + 10 );

        mapDraw.textAlign="left";
        mapDraw.fillText( planets[5].name, (planets[5].col + 1 ) * hexWidth, planets[5].row*30 + 8 );

        mapDraw.textAlign="left";
        mapDraw.fillText( planets[6].name, (planets[6].col + 1.5 ) * hexWidth, planets[6].row*30 + 10 );

        mapDraw.textAlign="left";
        mapDraw.fillText( planets[7].name, (planets[7].col + 1 ) * hexWidth, planets[7].row*30 + 8 );

        mapDraw.textAlign="left";
        mapDraw.fillText( planets[8].name, (planets[8].col + 1 ) * hexWidth, planets[8].row*30 + 8 );

        mapDraw.font = "italic bold 14px Arial";        
        
        mapDraw.textAlign="center";
        mapDraw.fillText( planets[9].name, (planets[9].col + 0.5 ) * hexWidth, planets[9].row*30 + 14 );
        
        mapDraw.textAlign="center";
        mapDraw.fillText( planets[10].name, (planets[10].col + 1 ) * hexWidth, planets[10].row*30 + 10 );

        mapDraw.textAlign="center";
        mapDraw.fillText( planets[11].name, (planets[11].col + 0.5 ) * hexWidth, planets[11].row*30 + 10 );

        collisionPolys = [];

        for ( i=0; i<planets.length; i++ ) {
            
            drawPlanet( planets[i] );
            
            collisionPolys.push( poly20( planets[i].col, planets[i].row, planets[i].diameter ) );
            
        }

        // this part draws the crash polygons for checking. 
        //mapDraw.beginPath();
        //mapDraw.strokeStyle="red";
        //for ( i=0; i<collisionPolys.length; i++ ) {
        //
        //    mapDraw.moveTo( collisionPolys[i][0], collisionPolys[i][1] );
        //
        //    for ( j = 2; j<40; j+=2 ) {
        //        mapDraw.lineTo( collisionPolys[i][j], collisionPolys[i][j+1] );
        //    }
        //    mapDraw.lineTo( collisionPolys[i][j], collisionPolys[i][j+1] );
        //    
        //
        //}        
        //mapDraw.stroke();

        
        minorPlanet( planets[10].col, planets[10].row );
        minorPlanet( planets[11].col, planets[11].row );
        
        //// for testing, remove
        //for ( i=0; i<gravHexes.length; i++ ) {
        //    
        //    mapDraw.beginPath();
        //    mapDraw.moveTo( gravHexes[i][1], gravHexes[i][2] );
        //    
        //    for ( j=0; j<4; j++) {
        //        
        //        mapDraw.lineTo( gravHexes[i][j*2+3],gravHexes[i][j*2+4] );
        //        
        //    }
        //    mapDraw.lineTo( gravHexes[i][1], gravHexes[i][2] );
        //    
        //    mapDraw.strokeStyle = "red";
        //    mapDraw.stroke();
        //    mapDraw.closePath();
        //    
        //}
        
        asteroids = [ [ 0, 14 ], [ 1, 14 ], [ 1, 15 ], [ 1, 16 ], [ 0, 16 ], [ 3, 16 ], [ 0, 20 ], [ 1, 20 ], [ 2, 20 ],
                        [ 1, 19 ], [ 1, 21 ], [ 2, 18 ], [ 4, 18 ], [ 5, 18 ], [ 5, 17 ], [ 7, 17 ], [ 10, 17 ], [ 11, 20 ], [ 11, 22 ],
                        [ 12, 21 ], [ 13, 20 ], [ 12, 24 ], [ 13, 23 ], [ 13, 25 ], [ 15, 23], [ 15, 25 ], [ 16, 24 ], [ 15, 22 ], [ 16, 21 ],
                        [ 15, 19 ], [ 16, 22 ], [ 15, 21 ], [ 17, 21 ], [ 20, 19 ], [ 20, 18 ], [ 21, 16 ], [ 22, 17 ], [18, 19 ],
                        [ 19, 21 ], [ 19, 22 ], [ 18, 23 ], [ 19, 24], [ 21, 24 ], [ 22, 24 ], [ 21, 23 ], [ 22, 22 ],
                        [ 24, 22 ], [ 24, 23 ], [ 23, 20 ], [ 22, 19 ], [23, 19 ], [ 26, 22 ], [ 28, 22 ], [ 27, 23 ],
                        [ 26, 20 ], [ 25, 19], [ 26, 19 ], [ 25, 18 ], [ 27, 18 ], [ 25, 17 ], [ 26, 17 ],  //  That's the group around Clandestine
                        [ 28, 18 ], [28, 19 ], [ 29, 21 ], [ 30, 22 ], [ 30, 23 ], [ 31, 22 ], [ 31, 20 ],
                        [ 29, 17 ], [ 31, 18 ], [ 32, 18 ], [ 33, 18 ], [ 32, 21 ], [ 33, 21 ], [ 34, 20 ]
                        ];
        
        for ( i=0; i<asteroids.length; i++ ) {
            
            asteroid( asteroids[i][0], asteroids[i][1] );

        }
        
        
        mapDraw.lineWidth=2;
        
        x = planets[1].col;
        y = planets[1].row;
        detectionRange=[ [ x-1, y-2 ], [ x-2, y-5 ], [ x+3, y-5 ], [ x+5, y ], [ x+2, y ], [ x+1, y+2 ], [ x+3, y+5], [ x-2, y+5 ], [ x-5, y ], [ x-2, y ] ];
        mapDraw.strokeStyle = planets[1].color;
        drawDetection();

        x = planets[2].col;
        y = planets[2].row;
        detectionRange = [ [ x-5, y ], [ x-3, y-5 ], [ x+2, y-5 ], [ x+5, y ], [ x+2, y+5 ], [ x-3, y+5 ] ];
        mapDraw.strokeStyle = planets[2].color;
        drawDetection();

        x = planets[3].col;
        y = planets[3].row;
        detectionRange = [ [ x-5, y ], [ x-3, y-5 ], [ x-1, y-5 ], [ x, y-6 ], [ x+5, y-6], [ x+7, y-1 ], [  x+5, y+4 ], [ x+3, y+4 ], [ x+2, y+5 ], [ x-3, y+5 ] ];
        mapDraw.strokeStyle = planets[3].color;
        drawDetection();

        x = planets[4].col;
        y = planets[4].row;
        detectionRange = [ [ x-5, y ], [ x-3, y-5 ], [ x+2, y-5 ], [ x+5, y ], [ x+2, y+5 ], [ x-3, y+5 ] ];
        mapDraw.strokeStyle = planets[4].color;
        drawDetection();

        x = planets[7].col;
        y = planets[7].row;
        detectionRange = [ [ x-5, y ], [ x-3, y-5 ], [ x+2, y-5 ], [ x+3, y-3 ], [ x+2, y ], [ x+3, y+3 ], [ x+2, y+5 ], [ x-3, y+5 ] ];
        mapDraw.strokeStyle = planets[7].color;
        drawDetection();

        x = planets[8].col;
        y = planets[8].row;
        detectionRange = [ [ x, y-1 ], [ x+2, y-5 ], [ x+5, y], [ x+2, y+5 ], [ x, y+1 ], [ x+2, y+5 ], [ x+5, y], [ x+2, y-5 ] ];
        mapDraw.strokeStyle = planets[8].color;
        drawDetection();

        x = planets[9].col;
        y = planets[9].row;
        detectionRange = [ [ x-1, y ], [ x-5, y ], [ x-3, y+5 ], [ x+2, y+5 ], [ x, y+1], [ x+2, y+5 ],[ x-3, y+5 ],  [ x-5, y ] ];
        mapDraw.strokeStyle = planets[9].color;
        drawDetection();

        //draws map division         
        //detectionRange=[ [ 0, 27 ], [ 33, 27 ] ];
        //mapDraw.strokeStyle = 'black';
        //drawDetection();
        


        document.getElementById("mapDiv").scrollTop=582;
        document.getElementById("mapDiv").scrollLeft=175;
        
        littleMapDraw.drawImage( document.getElementById("theMap"), 0, 0, 613, 825 );
        cleanMapDraw.drawImage( document.getElementById("theMap"), 0, 0 );
        pathMapDraw.drawImage( document.getElementById("theMap"), 0, 0 );
        
        //navControlCtx = document.getElementById("navControls").getContext("2d");
        //
        //for ( i = 0; i < 2; i++) {
        //    
        //    
        //    
        //    
        //}
        
        startScenario();

    }

    function startScenario() {
        
        shipX = planets[3].col;
        shipY = planets[3].row;
        shipFuel = 10;
        shipVelocity="";
        
        document.getElementById("boostDiv").style.display = "block";

        mapDraw.drawImage( document.getElementById("cleanMap"), 0, 0 );
        pathMapDraw.drawImage( document.getElementById("cleanMap"), 0, 0 );
        
        if ( shipVelocity == "") {
            
            drawBoost( planets[3] );
            
        } else {
            
            // if you start in deep space, we would draw the regular nav options here
            
        }
        
    }
    
    function drawBoost( wanderer ) {
        
        navOptions=[];
        
        if ( shipY%2 == 1 ) {
            
            drawBoostCircle( shipX, shipY-1, 5 );
            drawBoostCircle( shipX+1, shipY-1, 6 );
            drawBoostCircle( shipX-1, shipY, 4 );
            drawBoostCircle( shipX+1, shipY, 1 );
            drawBoostCircle( shipX, shipY+1, 3 );
            drawBoostCircle( shipX+1, shipY+1, 2 );
            
        } else {
            
            drawBoostCircle( shipX-1, shipY-1, 5 );
            drawBoostCircle( shipX, shipY-1, 6 );
            drawBoostCircle( shipX-1, shipY, 4 );
            drawBoostCircle( shipX+1, shipY, 1 );
            drawBoostCircle( shipX-1, shipY+1, 3 );
            drawBoostCircle( shipX, shipY+1, 2 );
            
        }
        
    }
    
    function drawBoostCircle( x, y, direction ) {
        
        centerCoords( x, y );
        
//        console.log( centerX, centerY );
        
        mapDraw.beginPath();
        mapDraw.arc( centerX, centerY, 5, 0, 2 * Math.PI);
        mapDraw.fillStyle = "blue";
        mapDraw.fill();
        mapDraw.closePath();

        navOptions.push([ centerX-15, centerY-15, centerX+15, centerY+15, direction, x, y ]);
        
    }
    
    
    function mapClick( theEvent ) {
        
//        moved=0;
        
        for ( i=0; i<navOptions.length; i++ ) {
            
            if ( theEvent.offsetX >= navOptions[i][0] &&
                 theEvent.offsetY >= navOptions[i][1] &&
                 theEvent.offsetX <= navOptions[i][2] &&
                 theEvent.offsetY <= navOptions[i][3] ) {
                
                document.getElementById("log").innerText+=navOptions[i][4];
                
                centerCoords( shipX, shipY );
                originShipX = centerX;
                originShipY = centerY;

                centerCoords( navOptions[i][5], navOptions[i][6] );
                destinationShipX = centerX;
                destinationShipY = centerY;
                
                if ( i == 0 ) {
                    pathMapDraw.strokeStyle = "grey";
                } else {
                    pathMapDraw.strokeStyle = "red";
                }
                pathMapDraw.beginPath();
                pathMapDraw.arc( originShipX, originShipY, 5 ,0, 2 * Math.PI );
                pathMapDraw.stroke();
//                pathMapDraw.closePath();
                 
                pathMapDraw.strokeStyle = "blue";
                pathMapDraw.beginPath();
                pathMapDraw.moveTo( originShipX, originShipY );
                pathMapDraw.lineTo( destinationShipX, destinationShipY );
                pathMapDraw.stroke();
                pathMapDraw.closePath();
                
                
                mapDraw.clearRect(0, 0, theMap.width, theMap.height);

                mapDraw.drawImage( document.getElementById("pathMap"), 0, 0 );
                
                // before we move the ship we check if the line passes through a gravity pentagon
                // if so we add that to the velocity array (clean up follows)
                
                for ( j=0; j<gravHexes.length; j++ ) {
                    
                    if ( !( shipX == gravHexes[j][11] && shipY == gravHexes[j][12] ) ) {
                        
                        entered=0;
                        
                        for ( k=1; k<9; k+=2 ){
                            
                            if ( intersects( gravHexes[j][k],
                                               gravHexes[j][k+1],
                                               gravHexes[j][k+2],
                                               gravHexes[j][k+3],
                                               originShipX, originShipY,
                                               destinationShipX, destinationShipY ) ) {
                                entered++;
                                console.log("...passed through grav hex at " + gravHexes[j][11] + ", " + gravHexes[j][12] );
                            }
                            
                        }
    
                        if ( intersects( gravHexes[j][k],
                                            gravHexes[j][k+1],
                                            gravHexes[j][1],
                                            gravHexes[j][2],
                                            originShipX, originShipY,
                                            destinationShipX, destinationShipY ) ) {
                            entered++;
                            console.log("...passed through grav hex at " + gravHexes[j][11] + ", " + gravHexes[j][12] );
                        }
    
                        if ( entered > 0 ) {
                            
                            //add the direction
//                            velocityArray.push( gravHexes[j][0].toString() );
                            console.log( "pushing " + gravHexes[j][0] +" "+ gravHexes[j][0].toString() );
                            
                            shipVelocity += gravHexes[j][0].toString();
                        }
                    }
                    
                    
                }

               // WORK TO DO - check for collision, if so no report on dashboard, no new nav options
                crashed = false;

                for ( j = 0; j < planets.length; j++ ) {
                    
                    for ( k=0; k<36; k+=2 ) {
                        
                        if ( intersects( collisionPolys[j][k],
                                            collisionPolys[j][k+1],
                                            collisionPolys[j][k+2],
                                            collisionPolys[j][k+3],
                                            originShipX, originShipY,
                                            destinationShipX, destinationShipY) ) {
                            crashed=true;
                            console.log( j + ","+k);
                        }                        
                        
                    }
                    
                    if ( intersects( collisionPolys[j][38],
                                        collisionPolys[j][39],
                                        collisionPolys[j][0],
                                        collisionPolys[j][1],
                                        originShipX, originShipY,
                                        destinationShipX, destinationShipY) ) {
                        crashed=true;
                        console.log("crashed" + j + ","+k);
                    }
                    
                    if ( crashed ) {
                        j=planets.length;
                    }

                    
                }
                
                if ( crashed ) { alert ( "you have crashed"); }
                
                shipX = navOptions[i][5];
                shipY = navOptions[i][6];
                
                if ( navOptions[i][4] != 0 ) {

                    shipVelocity += navOptions[i][4];
                    
                }
                
                // clean up velocity
                velocityArray = shipVelocity.split("");
                
                if ( velocityArray.indexOf("1") > -1 &&  velocityArray.indexOf("4") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("1"), 1 );
                     velocityArray.splice( velocityArray.indexOf("4"), 1 );
                }
                
                if ( velocityArray.indexOf("2") > -1 &&  velocityArray.indexOf("5") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("2"), 1 );
                     velocityArray.splice( velocityArray.indexOf("5"), 1 );
                }
                
                if ( velocityArray.indexOf("3") > -1 &&  velocityArray.indexOf("6") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("3"), 1 );
                     velocityArray.splice( velocityArray.indexOf("6"), 1 );
                }
                
                if ( velocityArray.indexOf("2") > -1 &&  velocityArray.indexOf("6") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("2"), 1 );
                     velocityArray.splice( velocityArray.indexOf("6"), 1 );
                     velocityArray.push( "1" );
                }
                
                if ( velocityArray.indexOf("1") > -1 &&  velocityArray.indexOf("3") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("1"), 1 );
                     velocityArray.splice( velocityArray.indexOf("3"), 1 );
                     velocityArray.push( "2" );
                }
                
                if ( velocityArray.indexOf("2") > -1 &&  velocityArray.indexOf("4") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("2"), 1 );
                     velocityArray.splice( velocityArray.indexOf("4"), 1 );
                     velocityArray.push( "3" );
                }
                
                if ( velocityArray.indexOf("3") > -1 &&  velocityArray.indexOf("5") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("3"), 1 );
                     velocityArray.splice( velocityArray.indexOf("5"), 1 );
                     velocityArray.push( "4" );
                }
                
                if ( velocityArray.indexOf("4") > -1 &&  velocityArray.indexOf("6") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("4"), 1 );
                     velocityArray.splice( velocityArray.indexOf("6"), 1 );
                     velocityArray.push( "5" );
                }
                
                if ( velocityArray.indexOf("5") > -1 &&  velocityArray.indexOf("1") > -1 ) {
                     velocityArray.splice( velocityArray.indexOf("5"), 1 );
                     velocityArray.splice( velocityArray.indexOf("1"), 1 );
                     velocityArray.push( "6" );
                }
                
                shipVelocity="";
                for ( j=0; j < velocityArray.length; j++ ){
                    shipVelocity += velocityArray[j];
                }
                
                document.getElementById("velocitySpan").innerText = shipVelocity;
                
                if ( navOptions[i][4] != 0 ) {
                    shipFuel--;
                    document.getElementById("fuelSpan").innerText = shipFuel;
                }
                
                // WORK TO DO - if in orbit show LAND button
                
                
                
                
                 
                
                if ( crashed ) {
                    
                    
                    
                    
                } else if ( shipFuel == 0 ) {

                    navOptions=[];
                    
                    // WORK TO DO - coast off board, report on dashboard
                    
                } else {
                    
                    navOptions=[];
                    
            
                    generateNavOptions();
                    
                }
                
                i = navOptions.length;
                
            }
            
        }
        
        
    }
    
    
    function generateNavOptions() {
        
        coastX = shipX;
        coastY = shipY;
        
//        console.log( velocityArray );
        
        for ( i=0; i < velocityArray.length; i++ ) {
            
//            console.log( i + ", " + velocityArray[i] + ", " + coastY );
            
            if ( coastY%2 == 1 ) {
                
                switch ( velocityArray[i] ) {
                    case "1":
                        coastX++;
                        break;
                    case "2":
                        coastX++;
                        coastY++;
                        break;
                    case "3":
                        coastY++;
                        break;
                    case "4":
                        coastX--;
                        break;
                    case "5":
                        coastY--;
                        break;
                    case "6":
                        coastX++;
                        coastY--;
                        break;
                }
                
            } else {
                
                switch ( velocityArray[i] ) {
                    case "1":
                        coastX++;
                        break;
                    case "2":
                        coastY++;
                        break;
                    case "3":
                        coastX--;
                        coastY++;
                        break;
                    case "4":
                        coastX--;
                        break;
                    case "5":
                        coastX--;
                        coastY--;
                        break;
                    case "6":
                        coastY--;
                        break;
                }
            }
            
        }
        
        console.log( "ship: " + shipX + ", " + shipY );
        console.log( "coast: " + coastX + ", " + coastY );
        
        // coastX and CoastY are established
        
        centerCoords( coastX, coastY );
        mapDraw.beginPath();
        mapDraw.strokeStyle = "green";
        mapDraw.rect( centerX - 15, centerY - 15, 30, 30 );        
        mapDraw.stroke();
        mapDraw.closePath();        
        
        navOptions.push([ centerX - 15, centerY - 15, centerX + 15, centerY + 15, 0, coastX, coastY ] );
        
        //console.log( coastY );
        
        if ( coastY%2 == 1 ) {
            
            //console.log("odd");
            
            destY = coastY;
            destX = coastX + 1;
            pushDestination( 1 );
            
            destY = coastY + 1;
            destX = coastX + 1;
            pushDestination( 2 );
            
            destY = coastY + 1;
            destX = coastX;
            pushDestination( 3 );
            
            destY = coastY;
            destX = coastX - 1;
            pushDestination( 4 );
            
            destY = coastY - 1;
            destX = coastX;
            pushDestination( 5 );
            
            destY = coastY - 1;
            destX = coastX + 1;
            pushDestination( 6 );
            
        } else {

            //console.log("even");
            
            destY = coastY;
            destX = coastX + 1;
            pushDestination( 1 );
            
            destY = coastY + 1;
            destX = coastX;
            pushDestination( 2 );
            
            destY = coastY + 1;
            destX = coastX - 1;
            pushDestination( 3 );
            
            destY = coastY;
            destX = coastX - 1;
            pushDestination( 4 );
            
            destY = coastY - 1;
            destX = coastX - 1;
            pushDestination( 5 );
            
            destY = coastY - 1;
            destX = coastX;
            pushDestination( 6 );
            
        }        
        
    }
    
    function pushDestination ( direction ) {
        
        centerCoords( destX, destY );
        
        navOptions.push( [ centerX - 15, centerY - 15, centerX + 15, centerY + 15, direction, destX, destY  ] );
        
        mapDraw.beginPath();
        mapDraw.fillStyle = "blue";
        mapDraw.arc( centerX, centerY, 5, 0, 2 * Math.PI );        
        mapDraw.stroke();
        mapDraw.closePath();        
        
        
    }
    
    
    function drawPlanet( wanderer ) {
        
//        console.log( wanderer );
        
        x = wanderer.col * 35 + 17;
        if ( wanderer.row%2 == 1 ) {
            x += 18;
        }
        
        y = wanderer.row * 30 + 20;
        
//        console.log( x+ "," + y );
        
        mapDraw.beginPath();
        mapDraw.arc( x, y, wanderer.diameter, 0, 2 * Math.PI);
        mapDraw.fillStyle=wanderer.color;
        mapDraw.fill();
        
        mapDraw.stroke();
        mapDraw.closePath();
        
        if ( wanderer.gravity ) {     
    
            mapDraw.drawImage( document.getElementById("arrow1"), x-36, y-9, 18, 18 );
            mapDraw.drawImage( document.getElementById("arrow2"), x-24, y-34, 19, 19 );
            mapDraw.drawImage( document.getElementById("arrow3"), x+5, y-34, 19, 19 );
            mapDraw.drawImage( document.getElementById("arrow4"), x+19, y-9, 18, 18 );
            mapDraw.drawImage( document.getElementById("arrow5"), x+5, y+15, 18, 18 );
            mapDraw.drawImage( document.getElementById("arrow6"), x-24, y+15, 18, 18 );
    
        } else {
        
            mapDraw.drawImage( document.getElementById("harrow1"), x-36, y-9, 18, 18 );
            mapDraw.drawImage( document.getElementById("harrow2"), x-24, y-34, 20, 20 );
            mapDraw.drawImage( document.getElementById("harrow3"), x+5, y-34, 21, 21 );
            mapDraw.drawImage( document.getElementById("harrow4"), x+19, y-9, 18, 18 );
            mapDraw.drawImage( document.getElementById("harrow5"), x+5, y+15, 21, 21 );
            mapDraw.drawImage( document.getElementById("harrow6"), x-24, y+15, 20, 20 );
        }
        
        centerCoords( wanderer.col, wanderer.row );
        
        gravHexes.push( [ 4, centerX, centerY,
                                centerX + hexWidth, centerY - hexSide,
                                centerX + (hexWidth * 3 / 2), y - ( hexSide / 2 ),
                                centerX + (hexWidth * 3 / 2), y + ( hexSide / 2 ),
                                centerX + hexWidth, centerY + hexSide,
                                wanderer.col + 1, wanderer.row
                                ] );
        
        gravHexes.push( [ 5, centerX, centerY,
                                centerX + hexWidth, centerY + hexSide,
                                centerX + hexWidth, centerY + hexSide*2,
                                centerX + hexWidth/2, centerY + hexSide*5/2,
                                centerX,  centerY + hexSide*2,
                                wanderer.col + ( wanderer.row%2 ? 1 : 0 ), wanderer.row + 1
                                ] );
        
        gravHexes.push( [ 6, centerX, centerY,
                                centerX,  centerY + hexSide*2,
                                centerX - hexWidth/2, centerY + hexSide*5/2,
                                centerX - hexWidth, centerY + hexSide*2,
                                centerX - hexWidth, centerY + hexSide,
                                wanderer.col + ( wanderer.row%2 ? 0 : -1 ), wanderer.row + 1
                                ] );
        
        gravHexes.push( [ 1, centerX, centerY,
                                centerX - hexWidth, centerY - hexSide,
                                centerX - (hexWidth * 3 / 2), y - ( hexSide / 2 ),
                                centerX - (hexWidth * 3 / 2), y + ( hexSide / 2 ),
                                centerX - hexWidth, centerY + hexSide,
                                wanderer.col - 1, wanderer.row
                                ] );
        
        gravHexes.push( [ 2, centerX, centerY,
                                centerX - hexWidth, centerY - hexSide,
                                centerX - hexWidth, centerY - hexSide*2,
                                centerX - hexWidth/2, centerY - hexSide*5/2,
                                centerX,  centerY - hexSide*2,
                                wanderer.col + ( wanderer.row%2 ? 0 : -1 ), wanderer.row - 1
                                ] );
        
        gravHexes.push( [ 3, centerX, centerY,
                                centerX,  centerY - hexSide*2,
                                centerX + hexWidth/2, centerY - hexSide*5/2,
                                centerX + hexWidth, centerY - hexSide*2,
                                centerX + hexWidth, centerY - hexSide,
                                wanderer.col + ( wanderer.row%2 ? 1 : 0 ), wanderer.row - 1
                                ] );
        
        
    }

    function centerCoords( x, y ) {
        
        centerY = y * 30 + 20;
        
        centerX = x * 35 + 18;
        
        if ( y%2 == 1 ) {
            centerX += 18;
        }
        
    }
    
    function drawDetection() {
        
 //       console.log( detectionRange );
        
        centerCoords( detectionRange[0][0], detectionRange[0][1] );
        
        mapDraw.beginPath();
        mapDraw.moveTo( centerX, centerY );
        
        for ( i=1; i<detectionRange.length; i++ ) {
            
            centerCoords( detectionRange[i][0], detectionRange[i][1] );

            mapDraw.lineTo( centerX, centerY );
            
        }

        centerCoords( detectionRange[0][0], detectionRange[0][1] );

        
        mapDraw.lineTo( centerX, centerY );
        
        mapDraw.stroke();
        mapDraw.closePath();
        
    }
    
    function showLittleMap() {
        
        document.getElementById("theMap").style.display = "none";
        document.getElementById("littleMap").style.display = "block";
        
    }

    function hideLittleMap() {
        
        document.getElementById("theMap").style.display = "block";
        document.getElementById("littleMap").style.display = "none";
        
    }


    function minorPlanet( col, row ) {
        
        x = col * 35 + 17;
        if ( row%2 == 1 ) {
            x += 18;
        }
        
        y = row * 30 + 20;
        
        mapDraw.drawImage( document.getElementById("minorplanet"), x-9, y-9, 18, 18 );
        
    }

    function asteroid1( col, row ) {
        
        centerCoords( col, row );        
        
    
            //mapDraw.fillStyle = "green";
            //mapDraw.arc( centerX, centerY, 16, 0, 2 * Math.PI);
            //mapDraw.fill();
            //mapDraw.closePath();
            
        mapDraw.fillStyle = "black";

        for ( k=0; k<10; k++ ) {
            
            bodyX = Math.random() * 28 - 14;
            bodyY = Math.random() * 28 - 14;
            
            mapDraw.beginPath();
            mapDraw.arc( centerX + bodyX, centerY + bodyY, Math.random()+1, 0, 2 * Math.PI);
            mapDraw.fill();
            mapDraw.closePath();
            
            
        }
        
        

        
    }
        function asteroid( col, row ) {
        
    //    console.log(col+","+row);
        
        x = col * 35 + 17 + 19;
        if ( row%2 == 1 ) {
            x += 18;
        }
        
        y = row * 30 + 12;
                
        switch ( Math.floor(Math.random()*3) ) {
        
            case 0:
                mapDraw.drawImage( document.getElementById("asteroid1"), x-36, y-9, 33, 33 );
                break;
        
        
            case 1:
                mapDraw.drawImage( document.getElementById("asteroid2"), x-36, y-9, 33, 33 );
                break;
        
        
            case 2:
                mapDraw.drawImage( document.getElementById("asteroid3"), x-36, y-9, 33, 33 );
                break;
        }
        
        
        
    }
    
    function mouseoverMap( theEvent ) {
        
        row = Math.floor(theEvent.offsetY/30);
        
        col = theEvent.offsetX;
        
        if ( row%2 == 1 ) {
            col-=18;
        }
        
        col = Math.floor(col/35);

//        console.log( col + ", " + row );

        document.getElementById("coords").innerText = col + ", " + row;
        
        for ( i=0; i<planets.length; i++ ) {
            
            document.getElementById(planets[i].name).style.display = "none";

            if ( row == planets[i].row && col == planets[i].col) {
                
                document.getElementById(planets[i].name).style.display = "block";
                
            }
        }

    }
    
    
    // returns true iff the line from (a,b)->(c,d) intersects with (p,q)->(r,s)
    function intersects(a,b,c,d,p,q,r,s) {
      var det, gamma, lambda;
      det = (c - a) * (s - q) - (r - p) * (d - b);
      if (det === 0) {
        return false;
      } else {
        lambda = ((s - q) * (r - a) + (p - r) * (s - b)) / det;
        gamma = ((b - d) * (r - a) + (c - a) * (s - b)) / det;
        return (0 < lambda && lambda < 1) && (0 < gamma && gamma < 1);
      }
    };
    
    
    function lineIntersect(x1,y1,x2,y2, x3,y3,x4,y4) {
        var x=((x1*y2-y1*x2)*(x3-x4)-(x1-x2)*(x3*y4-y3*x4))/((x1-x2)*(y3-y4)-(y1-y2)*(x3-x4));
        var y=((x1*y2-y1*x2)*(y3-y4)-(y1-y2)*(x3*y4-y3*x4))/((x1-x2)*(y3-y4)-(y1-y2)*(x3-x4));
        if (isNaN(x)||isNaN(y)) {
            return false;
        } else {
            if (x1>=x2) {
                if (!(x2<=x&&x<=x1)) {return false;}
            } else {
                if (!(x1<=x&&x<=x2)) {return false;}
            }
            if (y1>=y2) {
                if (!(y2<=y&&y<=y1)) {return false;}
            } else {
                if (!(y1<=y&&y<=y2)) {return false;}
            }
            if (x3>=x4) {
                if (!(x4<=x&&x<=x3)) {return false;}
            } else {
                if (!(x3<=x&&x<=x4)) {return false;}
            }
            if (y3>=y4) {
                if (!(y4<=y&&y<=y3)) {return false;}
            } else {
                if (!(y3<=y&&y<=y4)) {return false;}
            }
        }
        return true;
    }     

    function poly20( x, y, radius ) {
        
        centerCoords( x, y );
        
        newPoly = [];
        
        for ( m = 0; m < 20; m++ ) {
            
            nextX = centerX + radius * Math.cos( 0.87 + m * 2 * Math.PI / 20 );
            
            nextY = centerY + radius * Math.sin( 0.87 + m * 2 * Math.PI / 20 );
            
            newPoly.push( nextX, nextY );

        }
        
        return newPoly;
    }
    

</script>


</body>
</html>
