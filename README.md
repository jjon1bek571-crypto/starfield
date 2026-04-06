# starfield
<!DOCTYPE html>
<html>
<head>
    <title>Light Speed Starfield</title>
    <style>
        body { margin:0; overflow:hidden; background:black; }
        canvas { display:block; }
    </style>
</head>
<body>

<canvas id="c"></canvas>

<script>
const canvas = document.getElementById("c");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let stars = [];

function randomColor() {
    const colors = [
        "#ff4d4d","#00ffff","#ffff00",
        "#ff00ff","#00ff99","#66ccff",
        "#ff9900","#cc66ff"
    ];
    return colors[Math.floor(Math.random()*colors.length)];
}

// Yulduzlar (chiziqli)
for(let i=0;i<700;i++){
    stars.push({
        x: Math.random()*canvas.width - canvas.width/2,
        y: Math.random()*canvas.height - canvas.height/2,
        z: Math.random()*canvas.width,
        pz: Math.random()*canvas.width,
        color: randomColor()
    });
}

let speed = 5;

function draw(){
    ctx.fillStyle = "rgba(0,0,0,0.3)";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    stars.forEach(s=>{
        s.z -= speed;

        if(s.z <= 1){
            s.x = Math.random()*canvas.width - canvas.width/2;
            s.y = Math.random()*canvas.height - canvas.height/2;
            s.z = canvas.width;
            s.pz = s.z;
            s.color = randomColor();
        }

        let k = 128 / s.z;
        let x = s.x * k + canvas.width/2;
        let y = s.y * k + canvas.height/2;

        let pk = 128 / s.pz;
        let px = s.x * pk + canvas.width/2;
        let py = s.y * pk + canvas.height/2;

        s.pz = s.z;

        ctx.beginPath();
        ctx.strokeStyle = s.color;
        ctx.lineWidth = 2;
        ctx.shadowBlur = 10;
        ctx.shadowColor = s.color;

        ctx.moveTo(px, py);
        ctx.lineTo(x, y);
        ctx.stroke();
    });

    requestAnimationFrame(draw);
}

draw();

// Sichqoncha bilan tezlik
document.addEventListener("mousemove", e=>{
    speed = (e.clientX / window.innerWidth) * 20;
});
</script>

</body>
</html>
