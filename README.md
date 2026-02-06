<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Happy Rose Day My Jalebi 🌹</title>

<style>
html,body{
  margin:0;
  padding:0;
  height:100%;
  overflow:hidden;
  font-family:'Poppins',sans-serif;
  background: radial-gradient(circle at top, #ff7eb3, #1a0012);
}

#scene{
  position:absolute;
  width:100%;
  height:100%;
}

.glow{
  position:absolute;
  width:300px;
  height:300px;
  background:#ff4d6d;
  border-radius:50%;
  filter:blur(170px);
  top:6%;
  left:50%;
  transform:translateX(-50%);
  opacity:0.5;
}

.overlay{
  position:absolute;
  width:100%;
  bottom:7%;
  text-align:center;
  color:#fff;
  padding:0 18px;
  z-index:10;
}

h1{
  font-size:2.5em;
  margin:0;
  text-shadow:0 0 30px #ffb3c6;
}

#wish{
  margin-top:14px;
  font-size:1.1em;
  line-height:1.65;
  min-height:130px;
  color:#ffe6ec;
  text-shadow:0 0 18px rgba(255,150,180,0.6);
}

.signature{
  margin-top:16px;
  font-style:italic;
  color:#ffd1dc;
}
</style>
</head>

<body>

<div id="scene"></div>
<div class="glow"></div>

<div class="overlay">
  <h1>Happy Rose Day 🌹</h1>
  <div id="wish"></div>
  <div class="signature">— Forever yours, Rabbit 🐰</div>
</div>

<script src="https://cdn.jsdelivr.net/npm/three@0.152.2/build/three.min.js"></script>

<script>
/* Typing text (auto) */
const text =
"My Jalebi 😚💗, this rose isn’t just a flower — it’s my heart. " +
"Just like this rose, my love for you grows deeper, softer, and more beautiful with every moment. " +
"You fill my world with sweetness, warmth, and endless smiles. " +
"Today and always, this rose is yours… just like me. 🌹💞";

let t=0;
setTimeout(()=>{
  function type(){
    if(t<text.length){
      document.getElementById("wish").innerHTML += text.charAt(t);
      t++;
      setTimeout(type,45);
    }
  }
  type();
},3000);

/* THREE.JS SETUP */
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(45,innerWidth/innerHeight,0.1,1000);
camera.position.z = 7;

const renderer = new THREE.WebGLRenderer({alpha:true,antialias:true});
renderer.setSize(innerWidth,innerHeight);
document.getElementById("scene").appendChild(renderer.domElement);

/* Lights */
scene.add(new THREE.AmbientLight(0xffc0cb,1.7));
const light = new THREE.PointLight(0xff4d6d,2.5);
light.position.set(5,6,5);
scene.add(light);

/* Rose */
const rose = new THREE.Group();
let petals=[];
for(let i=0;i<160;i++){
  const g = new THREE.SphereGeometry(0.14,16,16);
  const m = new THREE.MeshStandardMaterial({
    color:new THREE.Color(`hsl(${340+Math.random()*20},80%,60%)`),
    roughness:0.3,
    metalness:0.4
  });
  const p = new THREE.Mesh(g,m);
  p.userData={r:i*0.05};
  rose.add(p);
  petals.push(p);
}
scene.add(rose);

/* Hearts */
const hearts = new THREE.Group();
for(let i=0;i<70;i++){
  const g = new THREE.SphereGeometry(0.05,8,8);
  const m = new THREE.MeshBasicMaterial({color:0xffb3c6});
  const h = new THREE.Mesh(g,m);
  h.position.set((Math.random()-0.5)*8,Math.random()*6,(Math.random()-0.5)*8);
  hearts.add(h);
}
scene.add(hearts);

let bloomStart=false;
setTimeout(()=>{ bloomStart=true; },10000);

function animate(){
  requestAnimationFrame(animate);

  rose.rotation.y += 0.0025;

  if(bloomStart){
    petals.forEach((p,i)=>{
      const a=i*0.4;
      p.position.x=Math.cos(a)*p.userData.r;
      p.position.z=Math.sin(a)*p.userData.r;
      p.position.y=(i-80)*0.02;
    });
  }

  hearts.children.forEach(h=>{
    h.position.y+=0.014;
    if(h.position.y>6) h.position.y=-4;
  });

  renderer.render(scene,camera);
}
animate();

window.addEventListener("resize",()=>{
  camera.aspect=innerWidth/innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth,innerHeight);
});
</script>

</body>
</html>
