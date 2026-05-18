### Траектория огрубленной фибриллы без ПТМ гистоновых хвостов.
[Назад](http://intbio.github.io/grant_2023_RNFmoluch/year3)

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
      -webkit-appearance: none;
      appearance: none;
      width: 15px;
      height: 15px;
      background: #222222;
      cursor: pointer;
    }

    .slider::-moz-range-thumb {
      width: 25px;
      height: 25px;
      background: #4CAF50;
      cursor: pointer;
    }
  </style>
</head>

<body>

<script src="https://unpkg.com/ngl@2.0.0-dev.35/dist/ngl.js"></script>

<script>

  var pdb = "no_ptm_13.pqr";
  var xtc = "no_ptm_13.xtc";
  var trjstep = 4000;

  document.addEventListener("DOMContentLoaded", function() {

    window.stage = new NGL.Stage("viewport0", {
      backgroundColor: "#FFFFFF"
    });

    window.stage.loadFile(pdb).then(function(nucl) {

      nucl.addRepresentation("spacefill");

      NGL.autoLoad(xtc).then(function(frames) {

        nucl.addTrajectory(frames);

        window.traj = nucl.trajList[0].trajectory;

        window.traj.setFrame(0);
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

        window.traj.signals.frameChanged.add(function() {

          var fnum = window.traj.currentFrame;

          window.isInternalSliderUpdate = true;

          document.getElementById("myRange").value = fnum;

          window.isInternalSliderUpdate = false;

          document.getElementById("frame_counter")
            .innerHTML = (fnum * trjstep);

        });

        document.getElementById("myRange")
          .setAttribute(
            "max",
            window.traj.frames.length - 1
          );

      });

      nucl.autoView();

    });

    var slider = document.getElementById("myRange");

    slider.oninput = function() {

      // ignore internal slider updates
      if (window.isInternalSliderUpdate) {
        return;
      }

      if (
        window.traj &&
        window.traj.player
      ) {
        window.traj.player.pause();
      }

      window.traj.setFrame(this.value);

    };

  });

</script>

<br>

<div
  id="viewport0"
  style="height:500px; border: thin solid black">
</div>

<div class="slidecontainer">

  <button
    type="button"
    id="play"
    onclick="
      if(window.traj && window.traj.player){
        window.traj.player.play();
      }
    ">
    Play
  </button>

  <button
    type="button"
    id="pause"
    onclick="
      if(window.traj && window.traj.player){
        window.traj.player.pause();
      }
    ">
    Pause
  </button>

  <input
    type="range"
    min="0"
    max="100"
    value="0"
    class="slider"
    id="myRange">

  <p>
    Frame:
    <span id="frame_counter">0</span>
  </p>

</div>

</body>
</html>
