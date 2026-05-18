<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">

  <style>
    .slidecontainer {
      width: 100%;
    }

    .slider {
      -webkit-appearance: none;
      width: 100%;
      height: 10px;
      background: #d3d3d3;
      outline: none;
      opacity: 0.7;
      transition: opacity .2s;
    }

    .slider:hover {
      opacity: 1;
    }

    .slider::-webkit-slider-thumb {
      width: 15px;
      height: 15px;
      background: #222;
      cursor: pointer;
    }
  </style>
</head>

<body>

<!-- ✔️ ВЕРНУЛ НАЗАД -->
<p>
  <a href="http://intbio.github.io/grant_2023_RNFmoluch/year3">← Назад</a>
</p>

<script src="https://unpkg.com/ngl@2.0.0-dev.35/dist/ngl.js"></script>

<script>

  var pdb = "no_ptm_13.pqr";
  var xtc = "no_ptm_13.xtc";
  var trjstep = 4000;

  document.addEventListener("DOMContentLoaded", function() {

    // кнопки выключены до загрузки траектории
    function setControlsEnabled(state) {
      document.getElementById("play").disabled = !state;
      document.getElementById("pause").disabled = !state;
      document.getElementById("myRange").disabled = !state;
    }

    setControlsEnabled(false);

    window.stage = new NGL.Stage("viewport0", {
      backgroundColor: "#FFFFFF"
    });

    window.stage.loadFile(pdb).then(function(nucl) {

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

      NGL.autoLoad(xtc).then(function(frames) {

        nucl.addTrajectory(frames);

        window.traj = nucl.trajList[0].trajectory;

        window.traj.player =
          new NGL.TrajectoryPlayer(window.traj, {
            start: 0,
            timeout: 1,
            mode: "once",
            interpolateType: "spline",
            step: 1,
            interpolateStep: 5
          });

        window.isInternalSliderUpdate = false;

        // включаем кнопки ТОЛЬКО когда всё готово
        setControlsEnabled(true);

        setTimeout(function() {
          window.traj.setFrame(0);
          document.getElementById("frame_counter").innerHTML = 0;
        }, 50);

        window.traj.signals.frameChanged.add(function() {

          var fnum = window.traj.currentFrame;

          window.isInternalSliderUpdate = true;

          document.getElementById("myRange").value = fnum;

          window.isInternalSliderUpdate = false;

          document.getElementById("frame_counter")
            .innerHTML = (fnum * trjstep);

        });

        document.getElementById("myRange")
          .setAttribute("max", window.traj.frames.length - 1);

      });

      nucl.autoView();

    });

    var slider = document.getElementById("myRange");

    slider.oninput = function() {

      if (window.isInternalSliderUpdate) return;

      if (window.traj && window.traj.player) {
        window.traj.player.pause();
      }

      window.traj.setFrame(this.value);
    };

  });

</script>

<br>

<div id="viewport0" style="height:500px; border: thin solid black"></div>

<div class="slidecontainer">

  <button id="play" disabled
    onclick="if(window.traj && window.traj.player){ window.traj.player.play(); }">
    Play
  </button>

  <button id="pause" disabled
    onclick="if(window.traj && window.traj.player){ window.traj.player.pause(); }">
    Pause
  </button>

  <input type="range" min="0" max="100" value="0"
    class="slider" id="myRange" disabled>

  <p>
    Frame: <span id="frame_counter">0</span>
  </p>

</div>

</body>
</html>
