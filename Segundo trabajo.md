<!DOCTYPE html>
<html>
    <head>
        <title>Personaje - Ejemplo</title>
        <style>
            html, body { margin: 0; padding: 0; overflow: hidden; }
            #info {
                position: absolute;
                padding: 10px;
                width: 100%;
                text-align: center;
                color: #FFFFFF;
            }
        </style>
    </head>
    <body>
        <div id="info">ARANIA<br/>
            Usar flecha arriba y abajo para trasladar.<br/>
            Usar flechas laterales para rotar.<br/>
        </div>
    <script src="js/three.min.js"></script>
    <script>
		var degree = Math.PI/180;
		var scene, aspect, camera, renderer;
		var geometry1, geometry2;
		var hips;
		var rightFoot;
		var leftFoot;
		var startTime = Date.now();
		
		//var axesHelper1 = new THREE.AxesHelper( 1 );
				
		var upArrow = false;
		var downArrow = false;
		var leftArrow = false;
		var rightArrow = false;
		var scaleUp = false;
		var scaleDown = false;
		var xAxis = true;
		var yAxis = false;
		var zAxis = false;
		
		var thetaSum=0;
		var positivo=false;
		
		init();
		animate();


    var light = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(light);

    var light2 = new THREE.PointLight(0xffffff, 0.5);
    scene.add(light2);
	
		function init(){
			scene = new THREE.Scene();
			aspect = window.innerWidth / window.innerHeight;
			camera = new THREE.PerspectiveCamera( 45, aspect, 0.1, 1000);
			renderer = new THREE.WebGLRenderer();
			renderer.setSize( window.innerWidth, window.innerHeight );
			document.body.appendChild( renderer.domElement );
				  			
			//EVENTOS DE TECLADO
			
			var onKeyDown = function ( event ) {
				switch ( event.keyCode ) {
					case 38: // TRASLADAR ADELANTE
						upArrow = true;
						break;
					case 40: // TRASLADAR ATRÁS
						downArrow = true;
						break;
					case 37: // ROTAR CW
						leftArrow = true;
						break;
					case 39: // ROTAR CCW
						rightArrow = true;
						break;
				}
			};	  
		  			
			var onKeyUp = function ( event ) {
				switch ( event.keyCode ) {
					case 38: // TRASLADAR
						upArrow = false;
						break;
					case 40: // TRASLADAR
						downArrow = false;
						break;
					case 37: // ROTAR CW
						leftArrow = false;
						break;
					case 39: // ROTAR CCW
						rightArrow = false;
						break;

				}
			};	  	
			
			document.addEventListener( 'keydown', onKeyDown, false );
			document.addEventListener( 'keyup', onKeyUp, false );
			
			//ELEMENTOS DE ESCENA
							  
			var size = 10;
			var arrowSize = 1;
			var divisions = size;
			var origin = new THREE.Vector3( 0, 0, 0 );
			var x = new THREE.Vector3( 1, 0, 0 );
			var y = new THREE.Vector3( 0, 1, 0 );
		  	var z = new THREE.Vector3( 0, 0, 1 );
			var color1 = new THREE.Color( 0xFFFFFF );
		  	var color2 = new THREE.Color( 0x333333 );
		  	var colorR = new THREE.Color( 0xAA0000 );
		  	var colorG = new THREE.Color( 0x00AA00 );
		  	var colorB = new THREE.Color( 0x0000AA );
			var colorRd = new THREE.Color( 0xAA6666 );
		  	var colorGd = new THREE.Color( 0x66AA66 );
		  	var colorBd = new THREE.Color( 0x6666AA );
		  
		  	//CREAR LAS GRILLAS PARA EL ESCENARIO
		  	var axesHelper1 = new THREE.AxesHelper( size/10 );
			var axesHelper2 = new THREE.AxesHelper( size/10 );
			var axesHelper3 = new THREE.AxesHelper( size/10 );
		  	var gridHelperXY = new THREE.GridHelper( size, divisions, color1, color1);
		  	var gridHelperXZ = new THREE.GridHelper( size, divisions, color2, color2 );
		  	var gridHelperYZ = new THREE.GridHelper( size, divisions, color2, color2 );
            
            //ROTARLAS PARA QUE QUEDEN EN EL ESPACIO ADECUADO
            gridHelperXY.rotateOnWorldAxis ( x, THREE.Math.degToRad(90) );
            gridHelperXZ.rotateOnWorldAxis ( y, THREE.Math.degToRad(90) );
            gridHelperYZ.rotateOnWorldAxis ( z, THREE.Math.degToRad(90) );
            
            //CREAR LAS FLECHAS QUE INDICAN LOS EJES DE COORDENADAS 3D
            var arrowX = new THREE.ArrowHelper( x, origin, arrowSize, colorR );
            var arrowY = new THREE.ArrowHelper( y, origin, arrowSize, colorG );
            var arrowZ = new THREE.ArrowHelper( z, origin, arrowSize, colorB );
			
			//CREAR LAS GEOMETRÍAS


			var geometry = new THREE.BoxBufferGeometry( 0.2, 0.2, 0.2 );



			var points = [];
    			for ( var i = 0; i < 10; i ++ ) {
      			  points.push( new THREE.Vector2( Math.sin(i * 0.2) + 0.2, (i) * .5) );
   			 }
			geometry1 = new THREE.LatheGeometry( points );
			var material = new THREE.MeshLambertMaterial( { color: 0xFFFFFF } );
			var lathe = new THREE.Mesh( geometry1, material );
			lathe.rotation.x = 270*degree;
			lathe.position.z = 3;

			
			
			//CREAR LOS MATERIALES
			var material = new THREE.MeshLambertMaterial( { color: color1, vertexColors: THREE.FaceColors } );
			
			//CREAR LOS OBJETOS

			hips = new THREE.Mesh( geometry, material );
			hips.add(lathe);
			
			// CABEZA, OJOS Y COLA

			var geometry = new THREE.SphereGeometry( 1, 32, 32 );
			var material = new THREE.MeshLambertMaterial( {color: 0xFFFFFF } );
			sphere = new THREE.Mesh( geometry, material );
			sphere.position.z = 3;
			hips.add(sphere);



			var geometry = new THREE.SphereGeometry( 0.2, 32, 32 );
			var material = new THREE.MeshLambertMaterial( {color: 0x000000 } );
			sphere1 = new THREE.Mesh( geometry, material );
			sphere1.position.z = 4;
			hips.add(sphere1);


			var geometry = new THREE.SphereGeometry( 0.1, 32, 32 );
			var material = new THREE.MeshLambertMaterial( {color: 0x000000 } );
			sphere3 = new THREE.Mesh( geometry, material );
			sphere3.position.z = 4;
			sphere3.position.y = .5;	
			sphere3.position.x = -.3;
			hips.add(sphere3);


			var geometry = new THREE.SphereGeometry( 0.1, 32, 32 );
			var material = new THREE.MeshLambertMaterial( {color: 0x000000 } );
			sphere4 = new THREE.Mesh( geometry, material );
			sphere4.position.z = 4;
			sphere4.position.y = .5;
			sphere4.position.x = .3;
			hips.add(sphere4);


			var geometry = new THREE.SphereGeometry( 1.5, 32, 32 );
			var material = new THREE.MeshLambertMaterial( {color: 0xFFFFFF } );
			sphere2 = new THREE.Mesh( geometry, material );
			sphere2.position.z = -2;
			hips.add(sphere2);


			//PATAS


			geometry2 = new THREE.CylinderGeometry( .2, .2, 1, 32 );
			for ( var i = 0; i < geometry2.faces.length; i++) { 
				if( geometry2.faces[i].normal.y != 0) { 
					geometry2.faces[i].color = colorGd; 
				} 
			}
			
			leftLeg = new THREE.Mesh( geometry2, material ); //CILINDRO
			leftLeg2 = new THREE.Mesh( geometry2, material ); //CILINDRO
			leftLeg3 = new THREE.Mesh( geometry2, material ); //CILINDRO
			leftLeg4 = new THREE.Mesh( geometry2, material ); //CILINDRO
			leftLeg5 = new THREE.Mesh( geometry2, material ); //CILINDRO
			leftLeg6 = new THREE.Mesh( geometry2, material ); //CILINDRO
			leftLeg7 = new THREE.Mesh( geometry2, material ); //CILINDRO
			leftLeg8 = new THREE.Mesh( geometry2, material ); //CILINDRO

			rightLeg = new THREE.Mesh( geometry2, material ); //CILINDRO
			rightLeg2 = new THREE.Mesh( geometry2, material ); //CILINDRO
			rightLeg3 = new THREE.Mesh( geometry2, material ); //CILINDRO
			rightLeg4 = new THREE.Mesh( geometry2, material ); //CILINDRO
			rightLeg5 = new THREE.Mesh( geometry2, material ); //CILINDRO
			rightLeg6 = new THREE.Mesh( geometry2, material ); //CILINDRO
			rightLeg7 = new THREE.Mesh( geometry2, material ); //CILINDRO
			rightLeg8 = new THREE.Mesh( geometry2, material ); //CILINDRO

			
			axesHelper1 = new THREE.AxesHelper( 1 );
			axesHelper2 = new THREE.AxesHelper( 1 );
			axesHelper3 = new THREE.AxesHelper( 1 );
			axesHelper4 = new THREE.AxesHelper( 1 );
			axesHelper5 = new THREE.AxesHelper( 1 );
			axesHelper6 = new THREE.AxesHelper( 1 );
			axesHelper7 = new THREE.AxesHelper( 1 );
			axesHelper8 = new THREE.AxesHelper( 1 );
			
			
			leftLeg.applyMatrix( new THREE.Matrix4().makeTranslation(1.1,.2,2) );
			axesHelper1.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.75,0) );
			leftLeg2.applyMatrix( new THREE.Matrix4().makeTranslation(.2,-.2,0) );

			leftLeg3.applyMatrix( new THREE.Matrix4().makeTranslation(1.2,.2,1) );
			axesHelper2.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.75,0) );
			leftLeg4.applyMatrix( new THREE.Matrix4().makeTranslation(.2,-.2,0) );

			leftLeg5.applyMatrix( new THREE.Matrix4().makeTranslation(1.3,.2,0) );
			axesHelper3.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.75,0) );
			leftLeg6.applyMatrix( new THREE.Matrix4().makeTranslation(.3,-.1,0) );

			leftLeg7.applyMatrix( new THREE.Matrix4().makeTranslation(1.4,.2,-1) );
			axesHelper4.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.75,0) );
			leftLeg8.applyMatrix( new THREE.Matrix4().makeTranslation(.2,-.2,0) );



			rightLeg.applyMatrix( new THREE.Matrix4().makeTranslation(-.5,1,2) );
			axesHelper5.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.8,0) );
			rightLeg2.applyMatrix( new THREE.Matrix4().makeTranslation(.2,-.1,0) );

			rightLeg3.applyMatrix( new THREE.Matrix4().makeTranslation(-.8,1,1) );
			axesHelper6.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.75,0) );
			rightLeg4.applyMatrix( new THREE.Matrix4().makeTranslation(.2,-.1,0) );

			rightLeg5.applyMatrix( new THREE.Matrix4().makeTranslation(-1,1,0) );
			axesHelper7.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.75,0) );
			rightLeg6.applyMatrix( new THREE.Matrix4().makeTranslation(.2,-.1,0) );

			rightLeg7.applyMatrix( new THREE.Matrix4().makeTranslation(-1.2,1,-1) );
			axesHelper8.applyMatrix( new THREE.Matrix4().makeTranslation(0,-.75,0) );
			rightLeg8.applyMatrix( new THREE.Matrix4().makeTranslation(.2,-.1,0) );

			
			hips.add(leftLeg);
			leftLeg.add(axesHelper1);			
			axesHelper1.add(leftLeg2);

			hips.add(leftLeg3);
			leftLeg3.add(axesHelper2);			
			axesHelper2.add(leftLeg4);

			hips.add(leftLeg5);
			leftLeg5.add(axesHelper3);			
			axesHelper3.add(leftLeg6);

			hips.add(leftLeg7);
			leftLeg7.add(axesHelper4);			
			axesHelper4.add(leftLeg8);



			hips.add(rightLeg);
			rightLeg.add(axesHelper5);			
			axesHelper5.add(rightLeg2);

			hips.add(rightLeg3);
			rightLeg3.add(axesHelper6);			
			axesHelper6.add(rightLeg4);

			hips.add(rightLeg5);
			rightLeg5.add(axesHelper7);			
			axesHelper7.add(rightLeg6);

			hips.add(rightLeg7);
			rightLeg7.add(axesHelper8);			
			axesHelper8.add(rightLeg8);

			


			leftLeg.rotation.y = 180*degree;
			leftLeg.rotation.z = -90*degree;
			leftLeg2.rotation.z = 20*degree;


			leftLeg3.rotation.y = 180*degree;
			leftLeg3.rotation.z = -90*degree;
			leftLeg4.rotation.z = 20*degree;

			leftLeg5.rotation.y = 180*degree;
			leftLeg5.rotation.z = -90*degree;
			leftLeg6.rotation.z = 20*degree;

			leftLeg7.rotation.y = 180*degree;
			leftLeg7.rotation.z = -90*degree;
			leftLeg8.rotation.z = 20*degree;


			rightLeg.rotation.z = 230*degree;
			rightLeg2.rotation.z = 20*degree;

			rightLeg3.rotation.z = 230*degree;
			rightLeg4.rotation.z = 20*degree;

			rightLeg5.rotation.z = 230*degree;
			rightLeg6.rotation.z = 20*degree;

			rightLeg7.rotation.z = 230*degree;
			rightLeg8.rotation.z = 20*degree;

		
		  	//AGREGAR A LA ESCENA
            		scene.add( gridHelperXZ );
		  	scene.add( arrowX );	
		  	scene.add( arrowY );	
		  	scene.add( arrowZ );	
			scene.add( hips );

		
			
			
			//MOVER LA CAMARA
			camera.position.x = 10;
			camera.position.y = 3;	 
		  	camera.position.z = 10;			
		  	camera.lookAt( origin );
			
			renderer.render( scene, camera );
			}
    
    function animate() {
        render();
        requestAnimationFrame( animate );
    }
    
    function render(){
        var dtime = Date.now()-startTime;
		var tx=0, ty=0, tz=0;	//Variables para traslacion
		var sc = 1;				//Variable para escala
		var theta=0;			//Variable para ángulo de rotacion de piernas
		var sigma=0;			//Variable para ángulo de rotación de caderas
		
		if(thetaSum>=40*Math.PI/180)
			positivo = false;
		if(thetaSum<= 0*Math.PI/180)
			positivo = true;
		
		if(upArrow) {
			tx=0; ty=0; tz=.1;
			if(positivo)
				theta = .05;
			else
				theta = -.05;
		}
		if(downArrow) {
			tx=0; ty=0; tz=-.1;
			if(positivo)
				theta = .05;
			else
				theta = -.05;
		}
		thetaSum+=theta;
		
		if(rightArrow)
			sigma = -.1;
		if(leftArrow)
			sigma = .1;
		
		//MATRIZ DE TRASLACIÓN
		var t = new THREE.Matrix4();
		t.set( 	1, 0, 0, tx,
				0, 1, 0, ty, 
				0, 0, 1, tz,
				0, 0, 0, 1	);
		
		hips.matrix.multiply(t); 	//APLICAR LA TRASLACIÓN A NIVEL LOCAL
		
		//ROTACIONES
		var ct1 = Math.cos(theta);
		var ct2 = Math.cos(-theta);
		var cs = Math.cos(sigma);
		var st1 = Math.sin(theta);
		var st2 = Math.sin(-theta);
		var ss = Math.sin(sigma);
		var r = new THREE.Matrix4();
		var r1 = new THREE.Matrix4();
		var r2 = new THREE.Matrix4();

		//MATRIZ DE ROTACIÓN EN EJE Y
		r.set( 	   cs,  0, ss, 0,
					0,  1,  0, 0, 
				  -ss,  0, cs, 0,
					0,  0,  0, 1 );	
		//MATRICES DE ROTACIÓN EN EJE LOCAL DE PIERNAS	
		r1.set( 	ct1,  -st1,  0, 0,
					st1, ct1, 0, 0, 
					0, 0, 1, 0,
					0,  0,  0, 1 );								
		
		//ROTACION EN UN EJE PARALELO
		var tempMatrix = new THREE.Matrix4();
		tempMatrix.copyPosition( hips.matrix );
		hips.applyMatrix( new THREE.Matrix4().getInverse(tempMatrix) );
		hips.applyMatrix(r);
		hips.applyMatrix(tempMatrix );

		leftLeg.applyMatrix(r1);
		leftLeg3.applyMatrix(r1);
		leftLeg5.applyMatrix(r1);
		leftLeg7.applyMatrix(r1);

		rightLeg.applyMatrix(r1);
		rightLeg3.applyMatrix(r1);
		rightLeg5.applyMatrix(r1);
		rightLeg7.applyMatrix(r1);
		
		//ROTACION EN UN EJE PARALELO
		/*var tempMatrix2 = new THREE.Matrix4();
		tempMatrix2.copyPosition( axesHelper1.matrix );
		leftLeg2.applyMatrix( new THREE.Matrix4().getInverse(tempMatrix2) );*/

		leftLeg2.applyMatrix(r1);
		leftLeg4.applyMatrix(r1);
		leftLeg6.applyMatrix(r1);
		leftLeg8.applyMatrix(r1);

		rightLeg2.applyMatrix(r1);
		rightLeg4.applyMatrix(r1);
		rightLeg6.applyMatrix(r1);
		rightLeg8.applyMatrix(r1);
			
        camera.lookAt( 0, 0, 0 );
        renderer.render( scene, camera );
    }
    </script>
  </body>
</html>
