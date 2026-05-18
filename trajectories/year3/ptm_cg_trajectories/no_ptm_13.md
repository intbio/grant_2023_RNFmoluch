<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">
</head>

<body>

<h3>Траектория огрубленной фибриллы без ПТМ гистоновых хвостов.</h3>
<p>
  <a href="http://intbio.github.io/grant_2023_RNFmoluch/year3">← Назад</a>
</p>

<script src="https://unpkg.com/ngl@2.0.0-dev.35/dist/ngl.js"></script>

<div id="viewport0" style="height:500px; border: thin solid black"></div>

<br>

<button id="play" onclick="playTraj()">Play</button>
<button id="pause" onclick="pauseTraj()">Pause</button>

<input type="range" min="0" max="100" value="0" id="myRange">
<p>Frame: <span id="frame_counter">0</span></p>

<script>

var pdb = "no_ptm_13.pqr";
var xtc = "no_ptm_13.xtc";
var trjstep = 4000;

function playTraj() {
  if (window.traj && window.traj.player) {
    window.traj.player.play();
  } else {
    setTimeout(playTraj, 200);
  }
}

function pauseTraj() {
  if (window.traj && window.traj.player) {
    window.traj.player.pause();
  }
}

document.addEventListener("DOMContentLoaded", function () {

  window.stage = new NGL.Stage("viewport0", {
    backgroundColor: "#FFFFFF"
  });

  window.stage.loadFile(pdb).then(function (nucl) {

    nucl.addRepresentation("spacefill");

    nucl.addRepresentation("spacefill", {
      sele: ":P",
      colorScheme: "partialCharge",
      radiusType: "explicit"
    });

    nucl.addRepresentation("spacefill", {
      sele: ":D",
      colorScheme: "partialCharge",
      radiusType: "explicit"
    });

    NGL.autoLoad(xtc).then(function (frames) {

      nucl.addTrajectory(frames);

      window.traj = nucl.trajList[0].trajectory;

      window.traj.player = new NGL.TrajectoryPlayer(window.traj, {
        start: 0,
        timeout: 1,
        mode: "once",
        interpolateType: "spline",
        step: 1,
        interpolateStep: 5
      });

      window.traj.setFrame(0);

      window.traj.signals.frameChanged.add(function () {
        var fnum = window.traj.currentFrame;
        document.getElementById("myRange").value = fnum;
        document.getElementById("frame_counter").innerHTML = fnum * trjstep;
      });

      document.getElementById("myRange")
        .setAttribute("max", window.traj.frames.length - 1);

    });

    nucl.autoView();

  });

  document.getElementById("myRange").oninput = function () {

    if (!window.traj) return;

    if (window.traj.player) {
      window.traj.player.pause();
    }

    window.traj.setFrame(this.value);
  };

});

</script>

</body>
</html>
