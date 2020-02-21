# Cristhian-Segura


<html>
	<head>
		<title>My first three.js app</title>
		<style>
			body { margin: 0; }
			canvas { display: block; }
		</style>
	</head>
	<body>
		<script src="js/three.js"></script>
		<script>
			
			var  camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

			var scene = new THREE.Scene();

			var renderer = new THREE.WebGLRenderer();
			renderer.setSize( window.innerWidth, window.innerHeight );
			document.body.appendChild( renderer.domElement );

		
var light = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(light);

    var light2 = new THREE.PointLight(0xffffff, 0.5);
    scene.add(light2);


var degree = Math.PI/180;


for (var I = 0; I<=6; I++) {
var geometry = new THREE.CylinderGeometry(1.8, 2.2, 22);  
var material = new THREE.MeshLambertMaterial( { color: 0xF3FFE2} );
var cil = new THREE.Mesh( geometry, material );
cil.position.z = -100;
cil.position.x = 30-I*10;
cil.position.y = 5;
scene.add( cil );
}


for (var I = 0; I<=1; I++) {
var geometry = new THREE.CylinderGeometry(1.8, 2.2, 22);  
var material = new THREE.MeshLambertMaterial( { color: 0xF3FFE2} );
var cil = new THREE.Mesh( geometry, material );
cil.position.z = -108.5-I*8;
cil.position.x = 30;
cil.position.y = 5;
scene.add( cil );
}



for (var I = 0; I<=6; I++) {
var geometry = new THREE.CylinderGeometry(1.8, 2.2, 22);  
var material = new THREE.MeshLambertMaterial( { color: 0xF3FFE2} );
var cil = new THREE.Mesh( geometry, material );
cil.position.z = -125;
cil.position.x = 30-I*10;
cil.position.y = 5;
scene.add( cil );
}


for (var I = 0; I<=1; I++) {
var geometry = new THREE.CylinderGeometry(1.8, 2.2, 22);  
var material = new THREE.MeshLambertMaterial( { color: 0xF3FFE2} );
var cil = new THREE.Mesh( geometry, material );
cil.position.z = -108.5-I*8;
cil.position.x = -30;
cil.position.y = 5;
scene.add( cil );
}




var length = 30, width = 8;

var shape = new THREE.Shape();
shape.moveTo( length/2, width );
shape.lineTo( length, 0 );
shape.lineTo( 0, 0 );

var extrudeSettings = {
	steps: 2,
	depth: 70,
	bevelEnabled: false,
	bevelThickness: -2,
	bevelSize: 1,
	bevelOffset: 0,
	bevelSegments: 1
};

var geometry = new THREE.ExtrudeGeometry( shape, extrudeSettings );
var material = new THREE.MeshLambertMaterial( { color: 0xF3FFE2 } );
var mesh = new THREE.Mesh( geometry, material ) ;
mesh.position.z=-97;
mesh.position.y =18;
mesh.position.x =-35;
mesh.rotation.y = 90*degree;
scene.add( mesh );


var geometry = new THREE.BoxBufferGeometry( 70,3,30 );
var material = new THREE.MeshLambertMaterial( { color: 0xF3FFE2 } );
var cube = new THREE.Mesh( geometry, material );
cube.position.z=-112;
cube.position.y =17;
cube.position.x =0;
scene.add( cube );


var length = 24, width = 8;

var shape = new THREE.Shape();
shape.moveTo( -60, width );

shape.lineTo( length-8, 8 );
shape.lineTo( length-6, 8 )
shape.lineTo( length-6, 6 );
shape.lineTo( length-4, 6 );
shape.lineTo( length-4, 4 );
shape.lineTo( length-2, 4 );
shape.lineTo( length-2, 2 );
shape.lineTo( length, 2 );
shape.lineTo( length, 0 );
shape.lineTo( -60, 0 );

var extrudeSettings = {
	steps: 1,
	depth: 40,
	bevelEnabled: false,
	bevelThickness: -2,
	bevelSize: 1,
	bevelOffset: 0,
	bevelSegments: 1
};

var geometry = new THREE.ExtrudeGeometry( shape, extrudeSettings );
var material = new THREE.MeshLambertMaterial( { color: 0xF3FFE2 } );
var mesh = new THREE.Mesh( geometry, material ) ;
mesh.position.z=-132;
mesh.position.y =-13;
mesh.position.x =23;
mesh.rotation.y = 0*degree;
scene.add( mesh );




// ---------------- PRIMERA CÁMARA

			camera.position.z = -40;
			camera.position.x = 60;
			camera.rotation.y = 30*degree;
			camera.position.z = -40;

// ----------------SEGUNDA CÁMARA
/*
			camera.position.z = -110;
			camera.position.x = 110;
			camera.position.y = 20;
			camera.rotation.y = 90*degree;
*/
// ----------------TERCERA CÁMARA
/*
			camera.position.x = -50;
			camera.position.y = 0;
			camera.rotation.y = 320*degree;
*/
// ----------------CUARTA CÁMARA

//			camera.position.y = 20;
			
				renderer.render( scene, camera );
			

			
		</script>
	</body>
</html>
